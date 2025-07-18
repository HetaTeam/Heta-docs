.. _examples_backend:

后端示例
============

本章节提供了启动 HRAG 对应后端功能的示例。具体代码在 ``src/backend`` 中。

multi_hop_services.py
---------------------

多跳问答服务，支持通过多轮检索和推理回答复杂问题。

**功能描述**:
- 提供基于RAG的文档检索功能
- 支持多跳问答流程，可组合多个工具进行信息检索
- 包含网页搜索功能，可获取网页内容并解析

**API端点**:

.. list-table:: 
   :widths: 20 30 50
   :header-rows: 1

   * - 端点
     - 方法
     - 描述
   * - ``/web_search``
     - POST
     - 使用Serper API进行网页搜索并返回内容
   * - ``/milvus_retrieval``
     - POST
     - 从Milvus向量数据库检索相关文档
   * - ``/multi_hop_qa``
     - POST
     - 执行多跳问答流程，组合多个工具获取最终答案

**请求示例**:

.. code-block:: json

    {
        "query": "多跳问题示例",
        "max_rounds": 3,
        "selected_tools": ["rag_retrieve", "web_search"],
        "retrieval_setting": {
            "milvus_collection": "challenge_data",
            "top_k": 3,
            "score_threshold": 0.5
        }
    }

data_search_services.py
-----------------------

数据搜索服务，聚合多种数据库的搜索能力。

**功能描述**:
- 提供统一接口访问Elasticsearch、Milvus和Neo4j
- 支持关键词搜索、向量搜索和图谱搜索
- 需要API Key认证 (Bearer 123456)

**API端点**:

.. list-table:: 
   :widths: 20 30 50
   :header-rows: 1

   * - 端点
     - 方法
     - 描述
   * - ``/milvus/services``
     - POST
     - Milvus向量数据库搜索
   * - ``/neo4j/services``
     - POST
     - Neo4j图谱数据库搜索

**请求示例**:

.. code-block:: json

    {
        "query": "搜索查询",
        "retrieval_setting": {
            "milvus_collection": "collection_name",
            "top_k": 5,
            "score_threshold": 0.6
        }
    }

deepwriter_services.py
----------------------

深度写作服务，根据查询生成结构化报告。

**功能描述**:
- 基于检索到的文档生成详细报告
- 支持自定义LLM和嵌入模型
- 需要API Key认证 (Bearer 123456)

**API端点**:

.. list-table:: 
   :widths: 20 30 50
   :header-rows: 1

   * - 端点
     - 方法
     - 描述
   * - ``/deepwriter/retrieval``
     - POST
     - 根据查询生成详细报告

**请求示例**:

.. code-block:: json

    {
        "knowledge_id": "知识库ID",
        "query": "报告主题"
    }

**启动方式**:

所有服务均可使用以下命令启动:

.. code-block:: bash

    python src/backend/<服务文件名>.py

服务默认监听所有网络接口(0.0.0.0)，端口号通过各自配置获取。