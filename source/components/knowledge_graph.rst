.. _components_knowledge_graph:

知识图谱组件
============

本章节详细介绍了 HRAG 系统中的知识图谱组件。

知识图谱构建
^^^^^^^^^^^^^^^^

知识图谱构建提供 HiRAG 与 TRAG 两种方法。两种方法均由：构建实体关系三元组、生成实体关系对应描述、构建知识图谱三部分组成，其中共用同一个构建实体关系三元组方法。


构建实体关系三元组：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    from src.data_processor.knowledge_graph.triple_extractor import triple_extractor
    
    # 根据MinerU生成的文件得到三元组
    input_path = "src/resources/pdf"
    triple_path = "src/resources/temp/knowledge_graph/triple"
    corpus_path = "src/resources/temp/knowledge_graph/corpus"  #语料库路径
    triple_extractor(input_path, triple_path, corpus_dir = corpus_path)



生成实体关系对应描述：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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