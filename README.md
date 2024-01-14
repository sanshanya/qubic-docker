# qubic-docker

> <u>*本教程在ubuntu上实现*</u>
>
> <u>*其余系统可自行安装Docker和Nvidia容器工具*</u>
>
> <u>*并跳至步骤三*</u>



**步骤一：安装显卡驱动(CUDA可选安装)**

如果宿主机不使用docker 则需要安装CUDA 12版本

但本教程使用Docker，故只安装驱动

以下是国内安装Nvidia相关工具的最佳实践

**首先确保你是root用户，或者接下来的命令都加上sudo**

下载nvidia密钥

```shell
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb 
```

安装nvidia密钥

```shell
sudo dpkg -i /tmp/cuda-keyring_1.0-1_all.deb 
```

更新软件源

```shell
sudo apt-get update
```

安装驱动

```shell
apt install  cuda-drivers-fabricmanager_545
```

如果你想要CUDA一同安装用如下命令

```shell
apt install cuda-12-3
```

**步骤二：安装Docker以及Nvidia容器工具**

安装Docker教程不再赘述

以下是社区提供的安装方法

```shell
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror Aliyun
```

Nvidia容器工具安装

```shell
#如果你已经安装nvidia密钥，执行以下命令。
sudo apt-get install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
```

重启Docker

```
sudo systemctl restart docker
```

**步骤三：构建Docker镜像及启动镜像**

下载本仓库的cpu.dockerfile和gpu.dockerfile

构建镜像指令为

```
# gpu是镜像名，./gpu.dockerfile是你的dockerfile文件位置，cpu.dockerfile构建Cpu挖矿镜像，gpu.dockerfile构建GPU挖矿镜像
docker build -t gpu  --build-arg version=1.8.3 -f ./gpu.dockerfile .
```

启动镜像指令为

```
# -e后面接的参数，name参数是你的矿工名字，token是你的token，num是使用到的线程数， --gpus all是使用全部gpu（你也可以指定使用某个GPU），-d gpu是指使用名为gpu的镜像
docker run -e name=work2 -e token="XXX" -e num=y --gpus all  -d gpu
```

