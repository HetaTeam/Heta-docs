.. _basic_usage:

基础使用
========

1. 文档解析
^^^^^^^^^^^^

HRAG 支持两种文档解析方法：MinerU 和 Docling。

使用 MinerU 解析文档
~~~~~~~~~~~~~~~~~~~~
.. tip::
    初次使用 MinerU 请下载对应模型文件，操作指南请查看:安装指南的 :ref:`MinerU_installation` 部分。

批量文档解析

.. code-block:: bash

    python tests/data_parser/test_mineru_pdf_parser.py --input_path src/resources/pdf

更详细的使用指南见 :ref:`components_data_parser` 

使用 Docling 解析文档
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

批量文档解析

.. code-block:: bash

    python tests/data_parser/test_docling_pdf_parser.py --root_path src/resources/test_data


更详细的使用指南见 :ref:`components_data_parser` 


2. 数据转换
^^^^^^^^^^^^

将解析后的文档转换为向量数据库格式，保存为 PKL：

针对 MinerU 解析的结果的处理

.. code-block:: bash

    python tests/data_processor/test_data_mineru_converter.py

针对 Docling 解析的结果

.. code-block:: bash

    python tests/data_processor/test_data_docling_converter.py \
        --root_path src/resources/data \
        --output_path src/pkl_files/challenge_docling.pkl

Docling 解析数据插入向量数据库

.. code-block:: bash

    python tests/data_processor/test_insert_to_vector_dbs.py \
        --root_path src/resources/data \
        --pkl_path src/pkl_files/challenge_docling.pkl \ 
        --vector_db milvus 


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
    from src.data_processor.knowledge_graph.graph_builder import graph_builder

    # 实体关系三元组等数据构建hirag，并存入working_dir
    data_path = "src/resources/temp/knowledge_graph/hirag_data"
    working_dir = "src/resources/temp/knowledge_graph/hirag"  
    graph_builder(data_path, working_dir,method="hirag")

    # TRAG
    from src.data_processor.knowledge_graph.graph_builder import graph_builder

    # 实体关系三元组等数据构建hirag，并存入working_dir
    data_path = "src/resources/temp/knowledge_graph/trag_data"
    working_dir = "src/resources/temp/knowledge_graph/trag"  
    graph_builder(data_path, working_dir,method="hirag")


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

