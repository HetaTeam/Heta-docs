快速开始指南
============

本指南将帮助您快速上手 HRAG 系统，从安装到运行第一个示例。

.. raw:: html

    <div class="quick-start">
        <h3>⚡ 5分钟快速体验</h3>
        <p>如果您想立即体验 HRAG 的功能，可以按照以下步骤操作：</p>
    </div>

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

      # 安装 uv
      pip install --upgrade pip
      pip install uv

      # 使用 uv 创建 h-rag 环境并激活环境
      uv venv h-rag --python=3.10
      source h-rag/bin/activate  # On Unix/macOS
      h-rag\Scripts\activate     # On Windows

2. **安装依赖**

   .. code-block:: bash

      uv pip install -e ".[dev]"

3. **验证安装**

   .. code-block:: bash

      python -c "import torch; print('PyTorch version:', torch.__version__)"
      python -c "import transformers; print('Transformers version:', transformers.__version__)"


.. note::
   如果您在安装过程中遇到问题，请查看 :ref:`troubleshooting` 章节。

基础使用
--------

1. 文档解析
^^^^^^^^^^^^

HRAG 支持两种文档解析方法：MinerU 和 Docling。

使用 MinerU 解析文档
~~~~~~~~~~~~~~~~~~~~
.. tip::
    初次使用 MinerU 请下载对应模型文件，操作指南请查看:安装指南的 :ref:`MinerU_installation` 部分。

单文档解析

.. code-block:: python

    from src.data_parser.mineru_parser import MinerUParser
    
    # 初始化解析器
    parser = MinerUParser()
    
    # 解析 PDF 文档
    pdf_file_name = "src/resources/pdf/XXX.pdf"
    output_dir = "src/resources/pdf/XXXoutput" # 解析后文件路径
    parser.process_pdf(pdf_file_name, output_dir)

批量文档解析

.. code-block:: python

    from src.data_parser.mineru_pdf_parser import get_pdf_mineru_info
    
    # 解析 PDF 文档
    input_path = "src/resources/pdf" # PDF 文件路径，（同解析后文件路径）
    get_pdf_mineru_info(input_path) 


使用 Docling 解析文档
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

单文档解析

.. code-block:: python
    
    from src.data_parser.docling_pdf_parser import DoclingPDFParser

    # PDF 文件路径
    pdf_file_name = "src/resources/pdf/XXX.pdf"
    # 解析后文件路径
    output_dir = "output"

    # 初始化解析器
    parser = DoclingPDFParser(pdf_file_name, output_dir)
    # 解析 PDF 文档
    parser.process_pdf()

批量文档解析

.. code-block:: python

    from src.data_parser.docling_pdf_parser import get_pdf_docling_info
    
    # 解析 PDF 文档（默认为 RAG-Challenge 数据）
    input_path = "src/resources/data/pdf_reports" 
    get_pdf_docling_info(input_path) 



2. 数据转换
^^^^^^^^^^^^

将解析后的文档转换为向量数据库格式，保存为 PKL：

针对 MinerU 解析的结果的处理

.. code-block:: python

    from src.data_processor.converters.pdf_to_chunk_converter import PDFToChunkConverter
    
    # 配置转换器
    converter = PDFToChunkConverter()

    # 执行转换
    converter.mineru_convert(
        input_path="src/resources/pdf",
        output_path="src/pkl_files/mineru.pkl",
        image_embedding=False # 是否对图片进行向量化
    )

针对 Docling 解析的结果

.. code-block:: python

    from src.data_processor.converters.pdf_to_chunk_converter import PDFToChunkConverter
    
    # 配置转换器
    converter = PDFToChunkConverter()

    # 执行转换
    converter.docling_convert(
        input_path="src/resources/data/pdf_reports", # 默认为 RAG-Challenge 数据
        output_path="src/pkl_files/docling.pkl"
    )


3. 知识图谱构建
^^^^^^^^^^^^^^^^

知识图谱构建提供 HiRAG 与 TRAG 两种方法。两种方法均由：构建实体关系三元组、生成实体关系对应描述、构建知识图谱三部分组成，其中共用同一个构建实体关系三元组方法。


构建实体关系三元组：

.. code-block:: python

    from src.data_processor.knowledge_graph.triple_extractor import triple_extractor
    
    # 根据MinerU生成的文件得到三元组
    input_path = "src/resources/pdf"
    triple_path = "src/resources/temp/knowledge_graph/triple"
    corpus_path = "src/resources/temp/knowledge_graph/corpus"  #语料库路径
    triple_extractor(input_path, triple_path, corpus_dir = corpus_path)


生成实体关系对应描述：

.. code-block:: python

    # HiRAG 
    from src.data_processor.knowledge_graph.entity_relation_extractor import entity_relation_extractor

    #根据已有语料库与三元组，提取实体与关系
    output_path = "src/resources/temp/knowledge_graph"
    corpus_path = "src/resources/temp/knowledge_graph/corpus"  #语料库路径
    triple_path = "src/resources/temp/knowledge_graph/triple"
    entity_relation_extractor(corpus_path, output_path,  method="hirag", triple_path = triple_path)


    # TRAG 
    from src.data_processor.knowledge_graph.entity_relation_extractor import entity_relation_extractor

    #根据已有语料库与三元组，提取实体与关系
    output_path = "src/resources/temp/knowledge_graph"
    corpus_path = "src/resources/temp/knowledge_graph/corpus"  #语料库路径
    triple_path = "src/resources/temp/knowledge_graph/triple"
    entity_relation_extractor(corpus_path, output_path,  method="trag", triple_path = triple_path)


构建知识图谱：

.. code-block:: python
    
    # HiRAG
    from src.data_processor.knowledge_graph.graph_builder import hirag_graph_builder

    # 实体关系三元组等数据构建hirag，并存入working_dir
    data_path = "src/resources/temp/knowledge_graph/hirag_data"
    working_dir = "src/resources/temp/knowledge_graph/hirag"  
    hirag_graph_builder(data_path, working_dir)

    # TRAG
    from src.data_processor.knowledge_graph.graph_builder import trag_graph_builder

    # 实体关系三元组等数据构建hirag，并存入working_dir
    data_path = "src/resources/temp/knowledge_graph/trag_data"
    working_dir = "src/resources/temp/knowledge_graph/trag"  
    trag_graph_builder(data_path, working_dir)


4. 启动服务
^^^^^^^^^^^^^^^^

启动后端服务进行问答：

.. code-block:: python

    from src.backend.data_search_services import DataSearchService
    
    # 启动服务
    service = DataSearchService()
    service.start()

.. raw:: html

    <div class="api-endpoint">
        <h4>API 端点示例</h4>
        <p><span class="method">POST</span> <span class="url">/api/v1/search</span></p>
        <p>用于文档检索和问答的 API 端点</p>
    </div>

高级功能
--------

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

故障排除
--------

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