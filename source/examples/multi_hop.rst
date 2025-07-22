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