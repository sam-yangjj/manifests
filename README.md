# manifests 工程
##  概述
本仓库包含用于管理多个 Git 仓库的 repo manifest 配置文件，支持 Android 或其他大型项目的代码管理。

##  目录结构

```bash
├── docker
│   ├── Dockerfile                                  # 构建镜像的 dockerfile
│   └── ubuntu-base-22.04-base-amd64.tar.gz         # 官方 ubuntu docker 镜像
├── platform                                        # 平台相关 manifests 配置
│   ├── tina-v821-v1.3.xml                          # 全志平台：v821-v1.3 SDK
│   └── tina-v821-v1.3_o.xml                        # 全志平台：v821-v1.3 SDK（简单方式）
└── README.md                                       # 本说明文件
```

##  准备工作

下载 repo 命令

```bash
#  下载 Repo 程序
mkdir -p ~/bin

curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

#    设置执行权限
chmod a+x ~/bin/repo

#  将 Repo 添加到系统PATH
echo "export PATH=~/bin:\$PATH" >> ~/.bashrc
source ~/.bashrc
```

###  国内源下载

```bash
#  下载 Repo 程序
mkdir -p ~/bin

# 使用清华镜像下载 (推荐)
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo -o ~/bin/repo

#    设置执行权限
chmod a+x ~/bin/repo

#  将 Repo 添加到系统PATH
echo "export PATH=~/bin:\$PATH" >> ~/.bashrc
echo "export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'" >> ~/.bashrc
source ~/.bashrc
```

##  拉取代码

拉取全志 v821 v1.3版本 平台代码

```bash
repo init -u git@github.com:sam-yangjj/manifests.git -b main -m platform/tina-v821-v1.3.xml
```

通过 https 的方式拉取
```bash
repo init -u https://github.com/sam-yangjj/manifests.git -b main -m platform/tina-v821-v1.3_o.xml
```

##  拉取某个 tag 平台代码

以 tag_1.1 举例（实际按照实际有的 tag 拉取）

```bash
repo init -u git@github.com:sam-yangjj/manifests.git -b tag_1.1 -m platform/tina-v821-v1.3.xml
```

##  同步代码

```bash
repo sync -j4 --no-clone-bundle
```

--no-clone-bundle: 禁用克隆包，跳过优化包，直接使用常规Git协议，避免因bundle校验失败导致的同步中断，首次同步速度可能稍慢，但更稳定。

##  编译
###  通过本机编译
根据 SDK 手册的 README.md 安装相应依赖，直接编译即可

###  通过 docker 编译
根据本仓库的 ubuntu22.04 docker 镜像和 Dockerfile 构建镜像并创建容器

##  source build/envsetup.sh
设置环境变量

##  lunch
lunch 选择该选项
```bash
2.  v821-avaota_f1-tina                | Application    (应用场景) : Avaota F1/Avaota F1-B Development Board
 Boot Type      (启动方式) : NORMAL    | Hardware Name  (硬件丝印) : Avaota F1/Avaota F1-B Development Board

```

接着根据芯片丝印选择具体型号
```bash
You're selecting board: v821-avaota_f1, choose your chip:
    1 V821L2-XXX
    2 V821M2-WXX
    3 V821L2-WXX
    4 V521D2-WXX
    5 V821L2-BXX
    6 V821M2-WBX
    7 V821L2-WBX
    8 V521D2-WBX
    9 V821D2-WBX
Which chip would you like? 7

```
##  make
进行编译
##  pack
进行打包  

即可生成镜像：out/v821l2-wbx_linux_avaota_f1_uart0_nor.img


###  编译失败
####  情况一：编译链缺失

编译失败主要是项目中的大文件（>100MB的文件没有拉取下来）  
有以下五个文件

```bash
find . -type f -size +100M -exec ls -l {} \;
-rw-r--r-- 1 sam sam 187045012  2月  8 18:56 ./prebuilt/kernelbuilt/riscv32/nds32le-linux-glibc-v5d.txz
-rw-r--r-- 1 sam sam 230047328  2月  8 18:57 ./prebuilt/rootfsbuilt/riscv/nds32le-linux-musl-v5d.tar.xz
-rw-r--r-- 1 sam sam 171387236  2月  8 18:56 ./rtos/lichee/rtos/tools/Xuantie-900-gcc-elf-newlib-x86_64-V2.10.1.tar.gz
-rw-r--r-- 1 sam sam 108280640  2月  8 18:56 ./brandy/brandy-2.0/tools/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi.tar.xz
-rw-r--r-- 1 sam sam 309457356  2月  8 18:56 ./brandy/brandy-2.0/tools/toolchain/Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.8.1.tar.xz

find . -type f -size +100M -exec ls -lh {} \;
-rw-r--r-- 1 sam sam 179M  2月  8 18:56 ./prebuilt/kernelbuilt/riscv32/nds32le-linux-glibc-v5d.txz
-rw-r--r-- 1 sam sam 220M  2月  8 18:57 ./prebuilt/rootfsbuilt/riscv/nds32le-linux-musl-v5d.tar.xz
-rw-r--r-- 1 sam sam 164M  2月  8 18:56 ./rtos/lichee/rtos/tools/Xuantie-900-gcc-elf-newlib-x86_64-V2.10.1.tar.gz
-rw-r--r-- 1 sam sam 104M  2月  8 18:56 ./brandy/brandy-2.0/tools/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi.tar.xz
-rw-r--r-- 1 sam sam 296M  2月  8 18:56 ./brandy/brandy-2.0/tools/toolchain/Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.8.1.tar.xz

find . -type f -size +100M -exec md5sum {} \;
eb8edd2e5eca1a8838bc6a63cffd7590  ./prebuilt/kernelbuilt/riscv32/nds32le-linux-glibc-v5d.txz
4b898eac1569ef4bc8ed4571ba520f42  ./prebuilt/rootfsbuilt/riscv/nds32le-linux-musl-v5d.tar.xz
54ce0bab6090c1449234697951027c14  ./rtos/lichee/rtos/tools/Xuantie-900-gcc-elf-newlib-x86_64-V2.10.1.tar.gz
063977200779374cf433424a440c2444  ./brandy/brandy-2.0/tools/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi.tar.xz
86ad8ac0b0a2c80a5166977202e081b4  ./brandy/brandy-2.0/tools/toolchain/Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.8.1.tar.xz

```

分别进入 prebuilt、rtos、brandy 仓库，通过以下命令拉取文件

```bash
git lfs pull

```

####  情况二：编译线程问题
make -j4 多线程编译有问题，全志 SDK 的锅，问题不大  
改为 make -j1 单线程编译即可，后续小修改用多线程即可
