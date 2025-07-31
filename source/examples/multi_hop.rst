.. _examples_multi_hop:

多跳推理示例
============

本章节提供了一个多跳推理的示例。

下面提供了一个使用 Milvus 查询的实例。需要提供查询问题与对应 Milvus 数据库名称两个参数。

.. code-block:: python

    from src.multi_hop_agent.multi_hop import MultiHopAgent
    
    # 初始化多跳代理
    agent = MultiHopAgent()
    
    # 执行多跳推理
    answer = agent.answer(
        query = "According to the text, what is the key obstacle to enforcing SCM rules on fishing subsidies?",
        collection_name = "world_trade_report"
    )


核心功能
--------

1. **多步推理**：通过多次检索和思考，逐步逼近问题的答案。
2. **信息提取**：从检索到的文档中提取关键信息。
3. **批判性推理**：对收集到的信息进行综合分析和验证。
4. **动态调整**：根据中间结果动态调整检索策略。

核心模块
--------

MultiHopAgent
~~~~~~~~~~~~~

- 入口类，负责初始化并启动多跳推理流程。

- 主要方法：

  - ``answer(query, top_n, score_threshold, max_rounds, collection_name)``: 执行多跳推理，返回答案。

HAgent
~~~~~~
- 继承自 ``FnCallAgent`` ，实现ReAct框架的核心逻辑。

- 关键功能：

  - 调用检索工具（RAGRetrieve）获取文档。

  - 提取关键信息并存储到内存中。

  - 通过批判性推理验证信息并生成最终答案。

RAGRetrieve
~~~~~~~~~~~

- 检索工具，基于Milvus向量数据库实现语义搜索。

- 功能：

  - 根据查询语句检索相关文档。

  - 返回格式化的检索结果。

使用方法
--------

1. 初始化MultiHopAgent：

   .. code-block:: python

      from src.multi_hop_agent.multi_hop import MultiHopAgent
      agent = MultiHopAgent()

2. 执行多跳推理：

   .. code-block:: python

      answer = agent.answer(
          query="According to the text, what is the key obstacle to enforcing SCM rules on fishing subsidies?",
          collection_name="world_trade_report"
      )

配置参数
--------

- ``query``: 用户输入的问题。

- ``top_n``: 每次检索返回的文档数量（默认14）。

- ``score_threshold``: 检索分数阈值（默认0.0）。

- ``max_rounds``: 最大推理轮次（默认3）。

- ``collection_name``: Milvus集合名称。

示例输出
--------

.. code-block:: json

   [
       {
           "thoughts": "Initial reasoning about the query...",
           "memory": "Extracted information from documents...",
           "answer": "Final answer after multi-hop reasoning..."
       }
   ]