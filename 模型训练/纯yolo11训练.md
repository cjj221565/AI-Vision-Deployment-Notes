# YOLO11 Windows CPU 环境部署与本地推理实战笔记

## 操作步骤
1. 安装 Anaconda （清华镜像）
```bash
https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/
```

> 选择 Anaconda3-2025.12-2-Windows-x86_64.exe 版本下载 安装时有选项可以全部勾选 下载路径不可以有中文

2. 配置国内源  在Anaconda Prompt中运行
```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/

conda config --set show_channel_urls yes
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

> 因为Anaconda和pip默认连接国外服务器 下载慢且容易中断 以上命令是换个国内高速通道

3. 创建虚拟环境并安装 CPU 版 PyTorch
```bash
conda create -n yolo python=3.10 -y
# -n后面跟的是环境名字 -y代表自动确认 目的是不让电脑系统不让各种库搞乱
conda activate yolo
# 激活环境 黑框前面的(base)会变成(yolo) 安装的东西在环境中生效
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
# 最关键一步 安装CPU版PyTorch由于电脑没有独显 -index-url后面指定必须下载CPU版本
pip install ultralytics
# 安装YOLO核心库 ultralytics 是YOLO官方库 自动把opencv、nmpy 这些依赖装好
```


>因为电脑没有独显，所以绝对不能安装文档里写的 cu121（GPU版），必须换成 cpu 版

4. 本地推理验证
```bash
yolo predict model=D:/AI-Work/models/yolo11n.pt source=D:/AI-Work/images project=D:/AI-Work/output device=cpu
# predict是推理命令 model指向模型文件 source指向本地图片 device=cpu 强制使用CPU
```

## 遇到问题及排错

1. GitHub连接超时 （无法下载测试图片）
本地下载模型图片 并复制到 Anaconda 的工作目录 C:\Users\cjj22\ 下
2. 推理结果误判 （精度问题）
典型的数据集偏差问题