.. _examples_knowledge_graph:

知识图谱示例
============

本章节提供了 HiRAG 与 TRAG 两种方法的知识图谱构建和使用的示例。


两种方法均由：构建实体关系三元组、生成实体关系对应描述、构建知识图谱三部分组成，其中共用同一个构建实体关系三元组方法。


构建实体关系三元组：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

将 MinerU 处理后的结果加载为语料，根据语料构建实体关系三元组：

.. code-block:: python

    from src.data_processor.knowledge_graph.triple_extractor import triple_extractor
    
    # 根据MinerU生成的文件得到三元组
    input_path = "src/resources/pdf"
    corpus_path = "src/resources/temp/knowledge_graph/corpus"  #语料库路径
    triple_path = "src/resources/temp/knowledge_graph/triple"
    triple_extractor(input_path, triple_path, corpus_dir = corpus_path)


**参数说明：**

- input_path ： 输入文件的路径，包含需要处理的原始文档与 MinerU 解析后的结果。

- corpus_path ： 语料库路径，MinerU 解析后的结果会处理成语料以便生成三元组。

- triple_path ： 构建三元组保存路径。




生成实体关系对应描述：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

根据已有语料库与三元组，生成实体关系的对应描述。其中 TRAG 方法还会首先生成对应的六元组，在进行实体关系描述的生成。

.. code-block:: python

    # HiRAG 
    from src.data_processor.knowledge_graph.entity_relation_extractor import entity_relation_extractor

    #根据已有语料库与三元组，提取实体与关系的描述
    corpus_path = "src/resources/temp/knowledge_graph/corpus"  #语料库路径

    output_path = "src/resources/temp/knowledge_graph/hirag_data"
    triple_path = "src/resources/temp/knowledge_graph/triple"
    entity_relation_extractor(corpus_path, output_path,  method="hirag", triple_path = triple_path)


    # TRAG 
    from src.data_processor.knowledge_graph.entity_relation_extractor import entity_relation_extractor

    #根据已有语料库与三元组，提取实体与关系的描述
    corpus_path = "src/resources/temp/knowledge_graph/corpus"  #语料库路径

    output_path = "src/resources/temp/knowledge_graph/trag_data"
    triple_path = "src/resources/temp/knowledge_graph/triple"
    entity_relation_extractor(corpus_path, output_path,  method="trag", triple_path = triple_path)


**参数说明：**

- output_path ： 输出文件的路径，生成的实体关系描述保存路径。

- corpus_path ： 语料库路径，MinerU 解析后的结果会处理成语料以便生成三元组。

- triple_path ： 构建三元组保存路径。TRAG 方法生成的六元组也保存在这里。


构建知识图谱：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

根据生成的实体关系描述，构建知识图谱。

.. code-block:: python
    
    # HiRAG
    from src.data_processor.knowledge_graph.graph_builder import graph_builder

    # 实体关系三元组等数据构建hirag，并存入working_dir
    data_path = "src/resources/temp/knowledge_graph/hirag_data"
    working_dir = "src/resources/temp/knowledge_graph/hirag"  
    graph_builder(data_path, working_dir,method="hirag")

    # TRAG
    from src.data_processor.knowledge_graph.graph_builder import graph_builder

   # 实体关系三元组等数据构建trag，并存入working_dir
    data_path = "src/resources/temp/knowledge_graph/trag_data"
    working_dir = "src/resources/temp/knowledge_graph/trag"  
    graph_builder(data_path, working_dir,method="trag")


**参数说明：**

- data_path ： 构建知识图谱所需的数据，即生成实体关系描述的结果。

- working_dir ：构建好的知识图谱保存路径。