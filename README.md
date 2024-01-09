# Install `llama-cpp` with GPU support

## About this Repository

I've been struggling over the last few weeks to get `llama-cpp` up and running. It wasn't quite as easy as downloading the Python package and installing it with a few flags. After long hours of trying to figure out why I wouldn't get the all-important `BLAS = 1` to run GPU inferences, I've figured it out, and this repository will serve as a refresher on how to reproduce the installation process that worked.

Keep in mind that this may not be the best way to install `llama-cpp` (or even the right way for all that matter), but this was the way that worked for me. Here's a detailed description of the environment I used: OS, dependencies, and build process for both CPU and GPU inferences.

I strongly advise running inferences locally on a CPU unless you've got a beefy GPU. In my opinion, the sacrifice of lower response times for higher accuracy in favour of GPU inferences is not worth it. I'd much prefer the model give me a quicker response especially when I'm trying to integrate this tool into my own LLM-backed projects.

## Environment

### Specs

OS: Ubuntu 22.04.3 LTS on Windows 10 x86_64
Kernel: 5.10.16.3-microsoft-standard-WSL2
Shell: zsh 5.8.1
CPU: Intel i7-10750H (12) @ 2.591GHz
GPU: Nvidia GTX 1650Ti Mobile

I set up `llama-cpp` on Ubuntu running on WSL2. All these commands should work for any Ubuntu based distribution of Linux.

### Dependencies

1. GCC and G++ Compilers for C and C++
2. CMake and Make
3. Python 3.10.9 (Check setup in a `conda` environment)
4. Miniconda / Anaconda
5. Git
6. LLMs downloaded locally in `.gguf` format. You may use `llama-cpp`'s helper scripts to convert `.bin` models to `.gguf`.

## Installation Process

Regardless of you having a GPU, it's best practice to start with a dedicated `conda` environment for installation of this package. Start with:

```bash
conda create -n llama-cpp python=3.10.9
conda activate llama-cpp
```

Then, clone the `llama-cpp` repository from Github with:

```bash
git clone https://github.com/ggerganov/llama.cpp
```

Now, change directory into your cloned folder and install the requirements using `pip`:

```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
```

### CPU Installation

1. conda create -n llama-cpp python=3.10.9
2. conda activate llama-cpp
5. python3 -m pip install -r requirements.txt

### CPU Only
1. mkdir build
2. cd build
3. cmake ..
4. cmake --build . --config Release

### GPU

In the Makefile, change the line:
NVCCFLAGS += -arch=native

to read:
NVCCFLAGS += -arch=all-major

There might be some warnings about depreciation but it compiled for me.

1. make LLAMA_CUBLAS=1

## References