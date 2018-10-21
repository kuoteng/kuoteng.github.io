Title: 在 Cuda 10.0 的環境下安裝 Pytorch 及 Tensorflow
Date: 2018-10-21
Category: Linux
Tags: ubuntu, machine learning
Slug: Install-pytorch-tensorflow-cuda10
Author: kuoteng

# Tensorflow
可以直接照著[官網的教學](https://www.tensorflow.org/install/source)

- 因為現在 tnesorflow 並不支援 python 3.7，需自己建立3.6的環境
    - 原本我想使用 anaconda 來安裝，但現在 anaconda 上預裝的 python 版本已經變為 3.7，便作罷
- 在虛擬環境下安裝需要的套件
```sh
$ pip install -U --user pip six numpy wheel mock
$ pip install -U --user keras_applications==1.0.5 --no-deps
$ pip install -U --user keras_preprocessing==1.0.3 --no-deps
```
- clone 下 tensorflow 的原始碼
```sh
$ git clone https://github.com/tensorflow/tensorflow.git
$ cd tensorflow
# $ git checkout r1.12 #選擇你自己要的版本，或是直接用 master
```

- 在 tensorflow 資料夾下 configure
    - 記得要要指定 `cuda support`, `cuda version`, `cuDNN version` 的相關數值
```sh
$ ./configure
```

- bazel build
    - 關於安裝 bazel 請參考上一篇
```sh
$ bazel build --config=opt --config=cuda --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=0" //tensorflow/tools/pip_package:build_pip_package
```

- 使用上一步驟產生的檔案來 build 出安裝用的 wheel 檔案
```sh
$ ./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

- 安裝 tensorflow
    - 我這邊是安裝 r1.12 的版本，可能路徑有些不一樣
```sh
$ pip install /tmp/tensorflow_pkg/tensorflow-1.12.0rc0-cp36-cp36m-linux_x86_64.whl
```

# Pytorch

- 在虛擬環境中安裝需要的元件
```sh
$ pip install pyyaml cmake
```
- 執行以下指令
    - 是不是超神的呢...我是別人跟我講才知道
```sh
$ git clone https://github.com/pytorch/pytorch.git
$ cd pytorch
$ git submodule update --init
$ python setup.py install
```
