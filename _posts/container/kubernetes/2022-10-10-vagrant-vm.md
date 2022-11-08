---
title: "K8S-Vagrant 가상 머신 생성 접속 삭제"
categories: kubernetes
tags: kubernetes
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

쿠버네티스 테스트 환경 구축을 위해 가상 머신을 생성하고 접속하는 방법과 가상 머신을 삭제하는 방법을 알아보자. VirtualBox와 Vagrant를 통해 가상 머신을 생성/삭제할 수 있고, SuperPuTTY를 통해 가상 머신에 접속할 수 있다.

<br>

## 내용

- 테스트 환경
- VirtualBox, Vagrant 설치
- PuTTY, SuperPuTTY 설치
- Vagrant를 이용한 가상 머신 생성
- SuperPuTTY를 이용한 가상 머신 접속
- Vagrant를 이용한 가상 머신 삭제

<br>

## 테스트 환경

- OS: Windows 10
- RAM: 16GB

<br>

## VirtualBox, Vagrant 설치

아래 링크를 통해 VirtualBox와 Vagrant를 다운로드 받은 후 순서대로 설치한다.

- [VirtualBox 6.1.12](https://download.virtualbox.org/virtualbox/6.1.12/VirtualBox-6.1.12-139181-Win.exe)
- [Vagrant 2.2.9](https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.msi)

<br>

## PuTTY, SuperPuTTY 설치

아래 링크를 통해 PuTTY와 SuperPuTTY를 다운로드 받은 후 순서대로 설치한다.

- [PuTTY](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.77-installer.msi) - putty-64bit-0.77-installer.msi
- [SuperPuTTY](https://github.com/jimradford/superputty/releases/download/1.4.0.9/SuperPuttySetup-1.4.0.9.msi) - ****Stable SuperPuTTY 1.4.0.9 Release****

<br>

## Vagrant를 이용한 가상 머신 생성

베이그런트를 이용해 가상 머신을 프로비저닝하기 위해 필요한 코드 파일을 관리할 임의의 경로를 하나 만든다. (ex. D:/workspace/) 이 경로에서 명령 프롬프트를 실행하여 가상 머신을 생성한다.

### 가상 머신 생성

`vagrant init` 을 실행하여 `Vagrantfile` 코드 파일을 만든다. 

```bash
vagrant init
```

`Vagrantfile` 을 코드 편집기로 열고, 아래 내용으로 수정한다. `m-k8s` 라는 이름으로 마스터 노드를 정의하고, `w1-k8s`, `w2-k8s` 그리고 `w3-k8s` 라는 이름으로 워커 노드를 정의한다. 

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config| 
  config.vm.define "m-k8s" do |cfg|
    cfg.vm.box = "sysnet4admin/CentOS-k8s"
    cfg.vm.provider "virtualbox" do |vb|
      vb.name = "m-k8s(github_SysNet4Admin)"
      vb.cpus = 2
      vb.memory = 2048
      vb.customize ["modifyvm", :id, "--groups", "/k8s-SM(github_SysNet4Admin)"]
    end
    cfg.vm.host_name = "m-k8s"
    cfg.vm.network "private_network", ip: "192.168.1.10"
    cfg.vm.network "forwarded_port", guest: 22, host: 60010, auto_correct: true, id: "ssh"
    cfg.vm.synced_folder "../data", "/vagrant", disabled: true   
    cfg.vm.provision "shell", path: "install_pkg.sh"
    cfg.vm.provision "file", source: "ping_2_nds.sh", destination: "ping_2_nds.sh"
    cfg.vm.provision "shell", path: "config.sh"
  end
  
  #=============#
  # Added Nodes #
  #=============#

  (1..3).each do |i|
    config.vm.define "w#{i}-k8s" do |cfg|
      cfg.vm.box = "sysnet4admin/CentOS-k8s"
      cfg.vm.provider "virtualbox" do |vb|
        vb.name = "w#{i}-k8s(github_SysNet4Admin)"
        vb.cpus = 1
        vb.memory = 1024
        vb.customize ["modifyvm", :id, "--groups", "/k8s-SM(github_SysNet4Admin)"]
      end
      cfg.vm.host_name = "w#{i}-k8s"
      cfg.vm.network "private_network", ip: "192.168.1.10#{i}"
      cfg.vm.network "forwarded_port", guest: 22, host: "6010#{i}",auto_correct: true, id: "ssh"
      cfg.vm.synced_folder "../data", "/vagrant", disabled: true
      cfg.vm.provision "shell", path: "install_pkg.sh"
    end
  end
end
```

`Vagrantfile` 이 위치한 경로에 `config.sh` 파일을 만들고, 아래 내용으로 수정한다. 마스터 노드에 복사한 `ping_2_nds.sh` 파일에 대해 권한을 부여하는 명령이 담긴 스크립트 파일이다.

```ruby
#!/usr/bin/env bash
# modify permission  
chmod 744 ./ping_2_nds.sh
```

`Vagrantfile` 이 위치한 경로에 `install_pkg.sh` 파일을 만들고, 아래 내용으로 수정한다. 외부 패키지를 설치하는 명령이 담긴 스크립트 파일이다.

```ruby
#!/usr/bin/env bash
# install packages 
yum install epel-release -y
yum install vim-enhanced -y
```

`Vagrantfile` 이 위치한 경로에 `ping_2_nds.sh` 파일을 만들고, 아래 내용으로 수정한다. 워커 노드들의 네트워크 상태를 확인하는 명령이 담긴 스크립트 파일이다.

```ruby
# ping 3 times per nodes
ping 192.168.1.101 -c 3
ping 192.168.1.102 -c 3
ping 192.168.1.103 -c 3
```

`vagrant up` 명령으로 가상 머신 프로비저닝을 진행한다.

```bash
vagrant up
```

`vagrant ssh m-k8s` 명령으로 마스터 노드에 ssh로 접속한다.

```bash
vagrant ssh m-k8s
```

`./ping_2_nds.sh` 파일을 실행해 워커 노드들의 네트워크 상태를 확인한다.

```bash
./ping_2_nds.sh
```

<br>

## SuperPuTTY를 이용한 가상 머신 접속

SuperPuTTY를 실행한 후  `putty.exe location` 을 PuTTY 가 설치된 경로로 설정한다.  SuperPuTTY 창의 오른쪽 `Sessions` 에서 PuTTY Sessions 폴더 하위에 k8s 라는 폴더를 생성한다. 이 폴더를 우클릭 후 New를 누른다. 아래와 같이 가상 머신의 정보를 입력한다. 

- 마스터 노드 가상 머신 정보
    - m-k8s
    - Host IP: 127.0.0.1
    - TCP Port: 60010
    - Extra Putty Arguments: -pw vagrant
- 워커 노드1 가상 머신 정보
    - w1-k8s
    - Host IP: 127.0.0.101
    - TCP Port: 60101
    - Extra Putty Arguments: -pw vagrant
- 워커 노드2 가상 머신 정보
    - w2-k8s
    - Host IP: 127.0.0.102
    - TCP Port: 60102
    - Extra Putty Arguments: -pw vagrant
- 워커 노드3 가상 머신 정보
    - w3-k8s
    - Host IP: 127.0.0.103
    - TCP Port: 60103
    - Extra Putty Arguments: -pw vagrant

모든 가상 머신의 접속 정보를 위와 같이 설정한 후 k8s 폴더를 우클릭하여 Connect All 을 누르면 모든 노드에 접속할 수 있다. 상단의 Commands에 명령을 입력하면 모든 노드에 동시에 명령을 실행할 수 있다.

<br>

## Vagrant를 이용한 가상 머신 삭제

접속 중인 마스터 노드에서 `exit` 명령을 통해 Host로 빠져나온다.

```bash
exit
```

`vagrant destroy -f` 명령으로 가상 머신을 삭제한다.

```bash
vagrant destroy -f
```