Title: Install Nvidia driver, CUDA and cuDNN with Ubuntu 18.04
Date: 2018-10-19
Category: Linux
Tags: Machine-Learning, Ubuntu, Linux
Slug: Install-Nvidia-driver-CUDA-and-cuDNN
Author: kuoteng

這學期在學校剛好有機會拿到使用顯示卡的機會，在這邊紀錄一下Ubuntu 18.04的環境建置

# Nvidia driver
- 先解除安裝原本的driver
```sh
$ sudo apt-get purge nvidia*
# $ sudo apt-get autoremove 看需不需要
```
- 更新版本與安裝
```sh
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
$ sudo apt install nvidia-driver-410
```
- 重新啟動之後檢查
```sh
$ lsmod | grep nvidia
# or
$ nvidia-smi
```
# Cuda
- 先去[官網](https://developer.nvidia.com/cuda-downloads)下載最新的 deb
```sh
$ sudo apt-get install gcc
$ sudo dpkg -i cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
$ sudo apt-key add /var/cuda-repo-10–0-local-10.0.130–410.48/7fa2af80.pub
$ sudo apt-get update
$ sudo apt install cuda
```
- 檢查cuda版本
```sh
$ cat /proc/driver/nvidia/version
$ nvcc -V
```
- 在 ~/.bashrc 中加入以下這段 (看你的shell是用什麼)
```sh
# CUDA
export CUDA_HOME=/usr/local/cuda-10.0
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64

PATH=${CUDA_HOME}/bin:${PATH}
export PATH
```
- 使用範例檢查
```
$ cuda-install-samples-10.0.sh <dir>
$ cd <dir>/NVIDIA_CUDA-10.0_Samples
$ make
$ ./bin/x86_64/linux/release/deviceQuery

```
# cuDNN
- 去[官網](https://developer.nvidia.com/cudnn)下載 deb 檔，依序下載並安裝
    - Runtime Library
    - Developer Library
    - Code Samples and User Guide
```sh
sudo dpkg -i libcudnn7_7.3.0.29–1+cuda10.0_amd64.deb
sudo dpkg -i libcudnn7-dev_7.3.0.29–1+cuda10.0_amd64.deb
sudo dpkg -i libcudnn7-doc_7.3.0.29–1+cuda10.0_amd64.deb
```
- 檢查是否安裝成功
```sh
cp -r /usr/src/cudnn_samples_v7/ $HOME
cd $HOME/cudnn_samples_v7/mnistCUDNN
make clean && make
./mnistCUDNN
```

# ref

https://medium.com/@vitali.usau/install-cuda-10-0-cudnn-7-3-and-build-tensorflow-gpu-from-source-on-ubuntu-18-04-3daf720b83fe
