.. _configuration:

配置指南
========

本章节介绍了 HRAG 系统的各种配置选项。


配置内容
^^^^^^^^^

H-RAG的相关配置均在 `src/config` 中设置，包含了：

* 数据库配置
* Embedding 模型配置
* LLM 模型配置
* knowledge_graph 相关参数配置

数据库配置、Embedding 模型配置、LLM 模型配置可直接在 `src/config/config.ini` 中更改
knowledge_graph 相关参数配置可直接在 `src/config/knowledge_graph/create_kg_conf.yaml` 中更改


数据库配置
^^^^^^^^^^^^

配置文件 `src/config/config.ini` 内容如下::

   [Elasticsearch]
   host = 127.0.0.1
   front_end_port = 5601
   read_write_port = 9200
   username = elastic
   password = elastic

   [Milvus]
    host = 127.0.0.1
    front_end_port = 8000
    read_write_port = 19530
    username = 
    password = 
    min_content_len = 200


    [Neo4j]
    host = 127.0.0.1
    front_end_port = 7474   
    read_write_port = 7687
    username = neo4j
    password = neo4j2025


    [MySQL]
    host = 127.0.0.1
    front_end_port = 3306
    read_write_port = 3306
    username = root
    password = 123456


