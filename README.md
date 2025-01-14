# Pre-built LibTorch for ARM32/64 Systems

The pre-built package is [https://github.com/wangkuiyi/pytorch-rpi](https://github.com/wangkuiyi/pytorch-rpi) and [https://github.com/ljk53/pytorch-rpi](https://github.com/ljk53/pytorch-rpi).

## NVIDIA Jetson Nano

<ins>**Alternative 1**</ins>

The steps to build it include:

1. Log in to an NVIDIA Jetson Nano.
1. Operating System: Ubuntu 18.04 for a recent Python.
1. Install Clang with `sudo apt install -y clang`.
1. Set environment variables:
   ```bash
   export USE_CUDA=1
   export USE_CUDNN=1
   export MAX_JOBS=4
   export USE_NNPACK=0
   export USE_QNNPACK=0
   export USE_PYTORCH_QNNPACK=0
   export BUILD_CAFFE2_OPS=0
   export USE_MKLDNN=0
   export USE_OPENCV=0
   ```
1. Run `./build_libtorch.sh` to build and pack the zip file.

<ins>**Alternative 2**</ins>

The steps to get libtorch.so with **GNU C++ compiler**:

1. Visit Nvidia forum [here](https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-8-0-now-available/72048). Download PyTorch what do you want, for example PyTorch v1.7.0 ([Python 3.6 - **torch-1.7.0-cp36-cp36m-linux_aarch64.whl**](https://nvidia.box.com/shared/static/cs3xn3td6sfgtene6jdvsxlr366m2dhq.whl))
1. Follow command below:
   ```bash
   $ sudo apt install python3-pip 
   $ sudo pip3 install torch-1.7.0-cp36-cp36m-linux_aarch64.whl
   $ pip3 show torch    # to check path of the installed torch 
   ```
1. After installation, check libtorch.so in path:
   ```bash
   $ cd /usr/local/lib/python3.6/dist-packages/torch/lib/      # that's consistent with output of command above
   $ ls
   ```
1. Sett CMakeLists.txt like below
   ```cmake
   set(CMAKE_PREFIX_PATH "/usr/local/lib/python3.6/dist-packages/torch")
   find_package(Torch REQUIRED)
   if (Torch_FOUND)
       message(STATUS "Torch library found!")
       message(STATUS "    include path: ${TORCH_INCLUDE_DIRS}" \n)
   else ()
       message(FATAL_ERROR "Could not locate Torch" \n)
   endif()
   ```

<ins>**Alternative 3**</ins>

The steps to get libtorch.so with **Clang C++ compiler**:

1. Visit Qengineering repo [here](https://github.com/Qengineering/PyTorch-Jetson-Nano). Download PyTorch what do you want, for example PyTorch v1.7.0 (Python 3.6 - [**torch-1.7.0a0-cp36-cp36m-linux_aarch64.whl**](https://drive.google.com/file/d/1aWuKu8eqkZwVzFFvguVuwkj0zdCir9qX/view?usp=sharing) and [**torchvision-0.8.1a0+45f960c-cp36-cp36m-linux_aarch64.whl**](https://drive.google.com/file/d/1WhplBjODLjNmYWEvQliCdkt3CqQTsClm/view?usp=sharing))
1. Follow command below:
   ```bash
   $ sudo apt install python3-pip 
   $ sudo pip3 install torch-1.7.0a0-cp36-cp36m-linux_aarch64.whl
   $ sudo pip3 install torchvision-0.8.1a0+45f960c-cp36-cp36m-linux_aarch64.whl
   $ pip3 show torch    # to check path of the installed torch 
   ```
1. After installation, check libtorch.so in path:
   ```bash
   $ cd /usr/local/lib/python3.6/dist-packages/torch/lib/      # that's consistent with output of command above
   $ ls
   $ export CC=/usr/bin/clang
   $ export CXX=/usr/bin/clang++
   ```
1. Comment script in TorchConfig.cmake in file *torch/share/cmake/Torch/*
   ```bash
   if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
      #set(TORCH_CXX_FLAGS "-D_GLIBCXX_USE_CXX11_ABI=0")
   endif()
   ```

3. Sett CMakeLists.txt like below
   ```cmake
   set(CMAKE_CXX_FLAGS "-D_GLIBCXX_USE_CXX11_ABI=1")
   set(CMAKE_PREFIX_PATH "/usr/local/lib/python3.6/dist-packages/torch")
   find_package(Torch REQUIRED)
   if (Torch_FOUND)
       message(STATUS "Torch library found!")
       message(STATUS "    include path: ${TORCH_INCLUDE_DIRS}" \n)
   else ()
       message(FATAL_ERROR "Could not locate Torch" \n)
   endif()
   ```


## NVIDIA Drive PX2

The steps to build it include:

1. Log in to an NVIDIA Drive PX2 computer.
1. Upgrade the system from Ubuntu 16.04 to Ubuntu 18.04 for a recent Python.
1. Install Clang with `sudo apt install -y clang`.
1. Set environment variables:
   ```bash
   export USE_CUDA=0
   export USE_QNNPACK=0
   export USE_PYTORCH_QNNPACK=0
   ```
1. Run `./build_libtorch.sh` to build and pack the zip file.

## Raspberry Pi (32-bit)

1. Install the latest Raspberry Pi OS (32-bit).
1. Run `sudo apt update && sudo apt upgrade`.
1. Run `git clone git@github.com:ljk53/pytorch-rpi && cd pytorch-rpi`.
1. Run `LIBTORCH_VARIANT=armv7l-cxx11-abi-shared-without-deps ./build_libtorch.sh`.

## Raspberry Pi (64-bit)

1. Install the latest 64-bit Ubuntu 20.04 for Raspberry Pi.
1. Run `sudo apt update && sudo apt upgrade`.
1. Run `git clone git@github.com:ljk53/pytorch-rpi && cd pytorch-rpi`.
1. Run `MAX_JOBS=2 LIBTORCH_VARIANT=aarch64-cxx11-abi-shared-without-deps ./build_libtorch.sh`.
   Note: set the max number of concurrent build jobs to 2 to avoid running out of the RAM.

## References
+ Pre-built LibTorch for ARM32/64 Systems (torch and libtorch) **Tested Ubuntu**: https://github.com/ljk53/pytorch-rpi
+ pytorch-arm-builds (torch and torchvision) **Tested Fedora**: https://github.com/nmilosev/pytorch-arm-builds
+ Q-engineering: https://github.com/Qengineering
   + PyTorch-Raspberry-Pi-64-OS: https://github.com/Qengineering/PyTorch-Raspberry-Pi-64-OS
   + PyTorch-Jetson-Nano: https://github.com/Qengineering/PyTorch-Jetson-Nano
+ **Nvidia** PyTorch for Jetson - version 1.11 now available: https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-11-now-available/72048
