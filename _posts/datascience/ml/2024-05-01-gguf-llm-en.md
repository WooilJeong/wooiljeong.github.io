---
title: Running LLM locally with GGUF files
categories: ml
tags: llm
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

Recently, high-performance, lightweight language models such as Meta's Llama3 and MS's Phi-3 have been open-sourced on HuggingFace. These models can be considered as an alternative to using large language models (LLMs) like ChatGPT or Claude as APIs. To quickly and easily run open source models in your local environment, it's worth noting that a file format called GGUF is often used. Let's take a quick look at GGUF and how you can use it to run Llama3 models in your local environment.

<br>

## GGUF(Georgi Gerganov Unified Format)

GGUF is a program that runs large models using GGML and a file format that stores the model. For reference, [GGML](https://ggml.ai/) is a library for ML that allows you to run large models quickly, even on a modest computer. GGUF is designed as a binary format that makes it quick and easy to load and save models. A binary format is a computer-readable format that represents information as a combination of zeros and ones. Developers typically create models using programming tools like PyTorch and save them in GGUF format so that they can be written in GGML. GGUF improves on previously used formats like GGML, GGMF, and GGJT to include all the necessary information and is designed to be extensible so that new information can be added and still fit into existing models.


<br>

## GGUF File Structure

![png](/assets/img/post_img/2024-04-28-gguf-llm/0.png)

Referring to the GGUFv3 diagram (above) by [@mishig25] (https://github.com/mishig25), you can get a rough idea of the GGUF file structure. You can see that model-related metadata (name, structure, dimensions, types, offsets, etc.) are written as Key Value pairs, and tensor information (structure, name, context length, file type, token information, etc.) as values.

<br>

## Download Llama3 GGUF file from HuggingFace

You can download the Meta-Llama-3-8B-Instruct-GGUF model file from the HuggingFace QuantFactory by following the steps below.

- Access the HuggingFace repository

https://huggingface.co/QuantFactory/Meta-Llama-3-8B-Instruct-GGUF

![png](/assets/img/post_img/2024-04-28-gguf-llm/1-en.png)

- Files tab - click the icon to download the Q8_0.gguf file

![png](/assets/img/post_img/2024-04-28-gguf-llm/2-en.png)

<br>

## Install Ollama

Ollama is an open source that makes it easy to run LLMs. Some of the models officially supported by Ollama (llama3, phi3, etc.) can be installed/run with a simple command. Of course, you can install/run the GGUF file downloaded above directly.

- Download and install Ollama by following this link

https://ollama.com/

![png](/assets/img/post_img/2024-04-28-gguf-llm/3.png)

<br>

## Create Modelfile

If you have a custom LLM model downloaded from Huggingface, etc. that is not an official model supported by Ollama, you need to add the model manually by creating a Modelfile file as shown below.

- Create a Modelfile file in the same path as the GGUF file

```yaml
FROM Meta-Llama-3-8B-Instruct.Q8_0.gguf

TEMPLATE """{{{{- if .System }}}}
<s>{{{{ .System }}}}</s>
{{{{- end }}}}
<s>Human:
{{{{ .Prompt }}}}</s>
<s>Assistant:
"""

SYSTEM """A chat between a curious user and an artificial intelligence assistant. The assistant gives helpful, detailed, and polite answers to the user's questions."""

PARAMETER temperature 0
PARAMETER num_predict 3000
PARAMETER num_ctx 4096
PARAMETER stop <s>
PARAMETER stop </s>
```

<br>

## Run Llama3

With the two files, GGUF and Modelfile, ready, you can now create and run the model from CMD.

- Add model command

```bash
ollama create Meta-Llama-3-8B-Instruct -f Modelfile
```

- Run the model

```bash
ollama run Meta-Llama-3-8B-Instruct
```