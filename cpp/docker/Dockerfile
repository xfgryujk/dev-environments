# 用法：
# docker build . -t <镜像名> --network host --build-arg HTTP_PROXY=http://<WSL网卡IP>:<代理端口> \
#     --build-arg HTTPS_PROXY=http://<WSL网卡IP>:<代理端口>

FROM debian:12

#
# 系统设置
#

# 时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# apt源
WORKDIR /etc/apt/sources.list.d
RUN cp debian.sources debian.sources.bak \
    && sed -i 's|deb.debian.org/debian$|mirrors.aliyun.com/debian|' debian.sources
RUN apt-get update

WORKDIR /root

# 语言
RUN apt-get install -y locales \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8

#
# 软件包
#

# GCC 12
RUN apt-get install -y build-essential

# LLVM (Clang) 16
RUN apt-get install -y wget lsb-release software-properties-common
# 不知道为什么脚本里的add-apt-repository会失败，所以在外面手动运行
RUN wget https://apt.llvm.org/llvm.sh \
    && chmod +x llvm.sh \
    && add-apt-repository 'deb http://apt.llvm.org/bookworm/  llvm-toolchain-bookworm-16 main' \
    && ./llvm.sh 16 all \
    && rm -f llvm.sh

# CMake 3.27.4
RUN wget https://github.com/Kitware/CMake/releases/download/v3.27.4/cmake-3.27.4-linux-x86_64.sh \
    && chmod +x cmake-3.27.4-linux-x86_64.sh \
    && ./cmake-3.27.4-linux-x86_64.sh --prefix=/usr/local --skip-license \
    && rm -f cmake-3.27.4-linux-x86_64.sh

# 常用命令
RUN apt-get install -y vim less man htop curl net-tools iputils-ping gdb gdbserver git

# SSH服务器，需要的时候手动运行/usr/sbin/sshd。记得加公钥和端口映射
RUN apt-get install -y openssh-server \
    && mkdir -p /run/sshd \
    && touch .ssh/authorized_key \
    && chmod 600 .ssh/authorized_key
EXPOSE 22

#
# 用户设置
#

# shell启动初始化脚本
COPY .bashrc ./

# VSCode远程用户设置
COPY vscode-remote-settings.json ./.vscode-server/data/Machine/settings.json
