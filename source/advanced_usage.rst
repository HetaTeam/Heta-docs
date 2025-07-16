.. _advanced_usage:

高级使用
========

混合检索
^^^^^^^^^^^^^^^^

HRAG 支持多种检索策略的组合：

.. code-block:: bash

    # 单条测试
    python tests/hybrid_retrieval/test_hybrid_weighted_retrieval.py \
        --single \
        --company_name "Downer EDI Limited" \
        --query "Did Downer EDI Limited announce a share buyback plan in the annual report? If there is no mention, return False." \
        --alpha 0.5 \
        --top_k 14 \
        --root_path src/resources/data

    # 批量评测
    python tests/hybrid_retrieval/test_hybrid_weighted_retrieval.py --alpha 0.5 --top_k 14


重排序
^^^^^^^^^^^^^^^^

使用重排序技术提高检索精度：

下载相关模型以及详细的使用方法，请查看 :ref:`rerank` 章节。

.. code-block:: bash

    
    # 从 huggingface 中下载模型进行重排序
    # 使用 bge-reranker-large 模型进行重排序
    python tests/rerank/test_rerank_huggingface.py \
        --root_path src/resources/data \
        --parent_document_retrieval \
        --top_n_retrieval 14 \
        --vector_db milvus
        --rerank_model bge-reranker-large
    
    # 使用 VLLM 部署的模型进行重排序
    python tests/rerank/test_rerank_VLLM.py \
        --root_path src/resources/data \
        --parent_document_retrieval \
        --top_n_retrieval 14 \
        --vector_db milvus


多跳推理
^^^^^^^^^^^^^^^^

支持多跳推理的智能问答：

.. code-block:: python

    from src.multi_hop_agent.src.multi_hop import MultiHopAgent
    
    # 初始化多跳代理
    agent = MultiHopAgent()
    
    # 执行多跳推理
    answer = agent.answer("需要多步推理的复杂问题")

性能优化
--------

配置优化
^^^^^^^^^^^^^^^^

.. code-block:: python

    # 优化配置示例
    config = {
        "chunk_size": 512,
        "chunk_overlap": 50,
        "batch_size": 32,
        "max_workers": 4,
        "cache_dir": "./cache"
    }

内存优化
^^^^^^^^^^^^^^^^

.. code-block:: python

    # 内存优化设置
    import torch
    
    # 启用梯度检查点
    torch.utils.checkpoint.checkpoint_sequential = True
    
    # 设置内存分配策略
    torch.cuda.set_per_process_memory_fraction(0.8)

.. raw:: html

    <div class="performance-metrics">
        <h4>📈 性能基准</h4>
        <table>
            <tr><th>操作</th><th>数据量</th><th>处理时间</th></tr>
            <tr><td>PDF 解析</td><td>25页</td><td>20秒</td></tr>
            <tr><td>三元组提取</td><td>100条语料</td><td>50分钟</td></tr>
            <tr><td>知识图谱构建</td><td>100个实体</td><td>3分钟</td></tr>
        </table>
    </div>

