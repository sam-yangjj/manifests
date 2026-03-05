# manifests 工程
##  概述
本仓库包含用于管理多个 Git 仓库的 repo manifest 配置文件，支持 Android 或其他大型项目的代码管理。
##  目录结构
```bash
├── platform                            # 平台相关manifests配置
│   └── tina-v821-v1.3.xml              # 全志平台：v821-v1.3 SDK
└── README.md                           # 本说明文件
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
##  拉取代码
拉取全志 v821 v1.3版本 平台代码
```bash
repo init -u git@github.com:sam-yangjj/manifests.git -b main -m platform/tina-v821-v1.3.xml
```
##  拉取某个 tag 平台代码
```bash
repo init -u git@github.com:sam-yangjj/manifests.git -b tag_1.1 -m platform/tina-v821-v1.3.xml
```
##  同步代码
```bash
repo sync -j4 --no-clone-bundle
```
--no-clone-bundle: 禁用克隆包，跳过优化包，直接使用常规Git协议，避免因bundle校验失败导致的同步中断，首次同步速度可能稍慢，但更稳定。
