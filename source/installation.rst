.. _installation:

安装指南
========

本章节详细介绍了 HRAG 系统的安装步骤和环境配置。

环境准备
--------

系统要求
^^^^^^^^^

* **Python**: 3.10 或更高版本
* **内存**: 至少 8GB RAM
* **存储**: 至少 10GB 可用空间
* **GPU**: 推荐 NVIDIA GPU (用于加速处理)

安装步骤
^^^^^^^^^

1. **创建虚拟环境**

   .. code-block:: bash

      conda create -n h-rag python=3.10
      conda activate h-rag

2. **安装依赖**

   .. code-block:: bash

      pip install -r requirements.txt

3. **验证安装**

   .. code-block:: bash

      python -c "import torch; print('PyTorch version:', torch.__version__)"
      python -c "import transformers; print('Transformers version:', transformers.__version__)"

.. note::
   如果您在安装过程中遇到问题，请查看 :ref:`troubleshooting` 章节。




.. _MinerU_installation:
MinerU 模型下载
--------------------------------

模型文件可以从 Hugging Face 下载。

从 Hugging Face 下载模型
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

使用python脚本 从Hugging Face下载模型文件

    .. code-block:: bash
        
        pip install huggingface_hub
        python download_models.py

python脚本会自动下载模型文件并配置好配置文件中的模型目录

将配置文件 `magic-pdf.json` 放在用户目录中。

:download:`download_models.py </_static/download_models.py>`

:download:`magic-pdf.json </_static/magic-pdf.json>`

.. tip::
    
    windows的用户目录为 "C:\Users\用户名", linux用户目录为 "/home/用户名", macOS用户目录为 "/Users/用户名"

详情可查看 `MinerU 1.3.11 版本使用安装指南 <https://github.com/opendatalab/MinerU/tree/release-1.3.11#quick-start>`_ 