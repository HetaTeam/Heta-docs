.. _components_multi_hop:

多跳思考
==============

本章节详细介绍了 HRAG 系统中的多跳思考组件。

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