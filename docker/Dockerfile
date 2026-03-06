# 使用本地 tar 包作为基础镜像
FROM scratch
ADD ubuntu-base-22.04-base-amd64.tar.gz /

# 设置环境变量，避免安装时出现交互界面（如时区选择）
ENV DEBIAN_FRONTEND=noninteractive

# 替换为清华大学的 apt 源，然后执行更新和安装 [citation:2]
# 使用 && 连接命令，并在最后清理缓存，可以有效减小镜像层的大小
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.tuna.tsinghua.edu.cn@g' /etc/apt/sources.list && \
    sed -i 's@//security.ubuntu.com@//mirrors.tuna.tsinghua.edu.cn@g' /etc/apt/sources.list && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        build-essential \
        python3 \
        python3-dev \
        subversion \
        git \
        libncurses5-dev \
        zlib1g-dev \
        gawk \
        flex \
        bison \
        quilt \
        libssl-dev \
        xsltproc \
        libxml-parser-perl \
        mercurial \
        bzr \
        ecj \
        xxd \
        bc \
        fakeroot \
        rsync \
        wget \
        file \
        busybox \
        cpio \
        cvs \
        unzip \
        lsof \
        tree \
        python-is-python3 \
        kconfig-frontends \
        android-tools-mkbootimg \
        python2 \
        libpython3-dev \
        gcc-multilib && \
    # 处理 i386 架构的软件源并安装相关库
    dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        libc6:i386 \
        libstdc++6:i386 \
        lib32z1 && \
    # 清理apt缓存以减小镜像体积 [citation:2]
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 恢复环境变量（可选）
ENV DEBIAN_FRONTEND=

# 设置默认工作目录，你可以根据需要修改
WORKDIR /workspace

# 指定容器启动时的默认命令
CMD ["/bin/bash"]
