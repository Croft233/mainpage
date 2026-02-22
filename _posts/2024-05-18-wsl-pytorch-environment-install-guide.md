---
title: "WSL下搭建pytorch深度学习环境"
description: >-
  本文介绍了基于wsl的arch linux的深度学习环境的搭建，深度学习框架使用PyTorch。
  WSL始终不是真正的linux环境，并且arch的滚动更新，使得环境的搭建十分折腾，想轻松的方法还是用Ubuntu吧。
  
# author: Retr0
date: 2024-05-18 23:00:00 +0900
categories: [Deep Learning, pytorch, wsl]
tags: [Deep Learning]
pin: true
math: true
render_with_liquid: false
media_subpath: '/posts/20240518'
---

# 1. 安装Minicoda

由于arch是滚动更新，在安装完成并跑通DL后一段时间没更新，进行一次 `sudo pacman -Syu` 就是灾难性的更新。
造成之前搭好的环境，在进行一次滚动更新后一整套工具链被污染从而变得无法使用。

所以咱们还是推荐使用Miniconda，避免以上的问题。

## 1.1 安装过程
- **下载**

  `wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`

  - 注意
  
    在 Arch（尤其是 WSL）上，通常不推荐用 pacman 安装 conda，
    而是用 Anaconda / Miniconda 官方安装器。并且官方明确不支持 Linux 发行版二次打包。

- **安装**

  `bash Miniconda3-latest-Linux-x86_64.sh`

  license：一路 yes

  安装路径：直接回车用默认（~/miniconda3）

  是否初始化 shell：yes

- **生效**
  `source ~/.bashrc`

## 1.2 创建环境

- 创建独立环境
  
  `conda create -n dl python=3.11`

  - 只使用官方渠道!!!

  ```
  conda config --env --set channel_priority strict
  conda config --env --remove-key channels
  conda config --env --add channels pytorch
  conda config --env --add channels nvidia
  ```

- 激活
  
  `conda activate dl`

  你会看到命令行前缀变成：

  `(dl) user@host:~`


# 2. 安装Cuda+Pytorch

## 2.1 驱动

驱动装在Windows端，WSL直接“透传”用

wsl跑dl架构：

```
Windows 11
├─ NVIDIA Windows Driver   ← 真正的 GPU 驱动
├─ WSL2 Kernel (微软 LTS)
│
└─ Arch Linux (WSL)
   ├─ CUDA Toolkit         ← 工具链
   ├─ cuDNN
   └─ PyTorch / TF
```

装完后，在PowerShell中验证：

  `nvidia-smi`

  返回的是下面的，就说明是 从 Windows 驱动映射进来的，不是 Arch 自己的驱动。

  ```
  NVIDIA-SMI 535.xx
  Driver Version: 535.xx
  CUDA Version: 12.x
  ```

## 2.2 安装工具

- 还是用conda来安装cuda：

  `conda install -y pytorch torchvision torchaudio pytorch-cuda=12.1`

## 2.3 验证环境

- 验证
  
  ```
  unset LD_LIBRARY_PATH
  python -c "import torch; print(torch.__version__, torch.version.cuda, torch.cuda.is_available())"
  ```

  这里出现了问题：

  ```
  ImportError: /home/croft/miniconda3/envs/dl/lib/python3.11/site-packages/torch/lib/libtorch_cpu.so: undefined symbol: iJIT_NotifyEvent
  ```

  查完资料后，这是个PyTorch（conda 包）和 Intel MKL 新版本之间的兼容性问题。
  当环境里装到了 MKL 2024.1+ 时，导入 PyTorch 可能会在 `libtorch_cpu.so` 里报 `undefined symbol: iJIT_NotifyEvent`。解决办法通常是 把 MKL 降级到 2024.0.x

  原帖：↓

  [MKL已知坑](https://github.com/pytorch/pytorch/issues/123097)

  接下来就是修复问题：

  - 把 MKL 降到 2024.0.1

    ```
    conda activate dl
    conda install -y "mkl<2024.1" "intel-openmp<2024.1" "mkl-service"
    ```

  - 继续验证

    `python -c "import torch; print(torch.__version__, torch.version.cuda, torch.cuda.is_available())"`

    命令行内返回版本号和`true`说明问题已解决。

  - 跑一个最小 CUDA 张量计算
    
    确认真在 GPU 上算：
   
    ```
    python - << 'EOF'
    import torch, time
    x = torch.randn(4096, 4096, device="cuda")
    y = torch.randn(4096, 4096, device="cuda")

    torch.cuda.synchronize()
    t0 = time.time()
    z = x @ y
    torch.cuda.synchronize()
    print("matmul done, seconds:", time.time() - t0)
    print("z mean:", z.mean().item())
    EOF
    ```

    出现结果和显卡型号说明工具链配置成功了，可以愉快的炼丹了。

