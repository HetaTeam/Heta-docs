.. _troubleshooting:

故障排除
========

### 常见问题

1. **内存不足错误**

   .. code-block:: bash

      # 减少批处理大小
      export BATCH_SIZE=16
      
      # 启用梯度累积
      export GRADIENT_ACCUMULATION_STEPS=2

2. **GPU 内存不足**

   .. code-block:: python

      # 使用 CPU 模式
      import torch
      torch.cuda.is_available = lambda: False

3. **依赖包冲突**

   .. code-block:: bash

      # 重新创建环境
      conda deactivate
      conda env remove -n h-rag
      conda create -n h-rag python=3.10
      conda activate h-rag
      pip install -r requirements.txt

获取帮助
^^^^^^^^^^^^^^^^

如果您遇到其他问题：

* 查看 :ref:`troubleshooting` 章节
* 阅读 :ref:`reference/faq` 常见问题
* 在 `GitHub Issues <https://github.com/your-github-username/hrag/issues>`_ 中提交问题

下一步
------

现在您已经完成了基础设置，可以：

* 阅读 :ref:`basic_usage` 了解详细使用方法
* 查看 :ref:`examples` 中的示例代码
* 探索 :ref:`api` 参考文档
* 学习如何 :ref:`development/extending` 扩展系统功能

.. raw:: html

    <div class="hrag-component">
        <h3>🎯 下一步建议</h3>
        <ul>
            <li><strong>初学者</strong>: 阅读 <a href="basic_usage.html">基础使用指南</a></li>
            <li><strong>开发者</strong>: 查看 <a href="api/index.html">API 文档</a></li>
            <li><strong>高级用户</strong>: 学习 <a href="advanced_usage.html">高级功能</a></li>
        </ul>
    </div> 

