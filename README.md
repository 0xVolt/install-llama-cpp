# install-llama-cpp
A repository with information on how to get llama-cpp setup with GPU support should you choose.

## Commands:

1. conda create -n llama-cpp python=3.10.9
2. conda activate llama-cpp
3. git clone https://github.com/ggerganov/llama.cpp
4. cd llama.cpp
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