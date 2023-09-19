
以nvidia cuda镜像为基础镜像创建pytorch的开发镜像，并设置工作路径和创建普通用户，并设置用户uid与宿主机用户一致，方便文件操作。              
```sh
FROM nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04

ARG user=zhangym

# Remove any third-party apt sources to avoid issues with expiring keys.
RUN rm -f /etc/apt/sources.list.d/*.list && \
sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list && \
sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
sed -i 's/http:/https:/g' /etc/apt/sources.list

# Install some basic utilities
RUN apt-get update && apt-get install -y --no-install-recommends \
sudo \
vim \
python3-dev \
python3-pip \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* \
&& ln -s /usr/bin/python3 /usr/bin/python && ln -s /usr/bin/pip3 /usr/bin/pip

# Create a non-root user and switch to it
RUN adduser --disabled-password --gecos '' --shell /bin/bash ${user} && \
adduser ${user} sudo && \
echo "${user} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-user && \
usermod -u 1000 ${user} && usermod -G 1000 ${user}

# Create a working directory
RUN mkdir /workspace && chown -R ${user}:${user} /workspace

USER ${user}
RUN python -m pip install --upgrade pip && \
pip config set global.index-url http://mirrors.aliyun.com/pypi/simple && \
pip config set install.trusted-host mirrors.aliyun.com && \
pip install torch==1.9.0+cu102 torchvision==0.10.0+cu102 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html && \
rm -rf /var/lib/apt/lists/* && \
rm -r /home/${user}/.cache/pip && \
sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/g' /home/${user}/.bashrc

WORKDIR /workspace
```     

在`Dockerfile`文件所在目录编译建立该镜像               
```sh
docker build -t jeremycheong/pytorch:1.9.0-cuda10.2-cudnn7-devel .
```
