# Install `llama-cpp` with GPU support

## Table of Contents

- [Install `llama-cpp` with GPU support](#install-llama-cpp-with-gpu-support)
  - [Table of Contents](#table-of-contents)
  - [About this Repository](#about-this-repository)
  - [Environment](#environment)
    - [Specs](#specs)
    - [Dependencies](#dependencies)
  - [Installation Process](#installation-process)
    - [CPU Specific Instructions](#cpu-specific-instructions)
    - [GPU Specific Instructions](#gpu-specific-instructions)
  - [References](#references)

---

## About this Repository

I've been struggling over the last few weeks to get `llama-cpp` up and running. It wasn't quite as easy as downloading the Python package and installing it with a few flags. After long hours of trying to figure out why I wouldn't get the all-important `BLAS = 1` to run GPU inferences, I've figured it out, and this repository will serve as a refresher on how to reproduce the installation process that worked.

Keep in mind that this may not be the best way to install `llama-cpp` (or even the right way for all that matter), but this was the way that worked for me. Here's a detailed description of the environment I used: OS, dependencies, and build process for both CPU and GPU inferences.

I strongly advise running inferences locally on a CPU unless you've got a beefy GPU. In my opinion, the sacrifice of lower response times for higher accuracy in favour of GPU inferences is not worth it. I'd much prefer the model give me a quicker response especially when I'm trying to integrate this tool into my own LLM-backed projects.

---

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

### CPU Specific Instructions

Use `CMake` to build the project in a `build/` folder then run a test inference.

```bash
mkdir build
cd build
cmake ..
cmake --build . --config Release
```

### GPU Specific Instructions

Here's where things get a little dicey. To install and run `llama-cpp` with `cuBLAS` support, the regular installation from the official GitHub repository's `README` is bugged. Here's a hotfix that should let you build the project and install it okay.

In the `Makefile`, change the line,
`NVCCFLAGS += -arch=native`
To read,
`NVCCFLAGS += -arch=all-major`

There might be some warnings about depreciation but it compiled for me. After that, you can use `make` to build the project.

```bash
make LLAMA_CUBLAS=1
```

**Note: You don't need to make a separate `build/` directory to build the project.**

---

## References

- https://github.com/ggerganov/llama.cpp
- https://www.reddit.com/r/LocalLLaMA/comments/17ruptr/cant_get_cuda_to_work_with_llamacpppython_in_wsl/
- https://pureinfotech.com/shutdown-linux-distro-wsl/
- https://github.com/ggerganov/llama.cpp/discussions/2142
- https://kubito.dev/posts/llama-cpp-linux-nvidia/

---