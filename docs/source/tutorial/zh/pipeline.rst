=======
流水线
=======

流水线（Pipeline）提供了一个开箱即用地用于试题的不同任务的方法，并且支持自定义添加组件。以下是目前支持的任务：

.. list-table:: 已支持的任务
   :widths: 30 30 30
   :header-rows: 1

   * - 任务
     - 描述
     - 流水线标识符
   * - 预处理
     - 无下游任务，仅为试题进行自定义的成分分解、语法分解
     - "pre-process"
   * - 难度预测
     - 为给定试题预测难度
     - "property-prediction"
   * - 知识点预测
     - 为给定试题预测知识点，知识点为层级结构
     - "knowledge-prediction"

我们将在本章节介绍EduNLP中流水线的使用方式：

* 使用 *pipeline()* 直接构建一个指定下游推理任务的流水线
* 使用 *pipeline()* 创建不包含下游任务的预处理流水线
* 使用 *pipeline()* 构建组合预处理和指定推理任务的流水线


指定推理任务的流水线
-----------------------
通过指定任务，使用 *pipeline()* 可以构建任意下游任务流水线类，并且自动加载默认模型。之后将试题按照相应格式传入，即可进行推理。

::

   from EduNLP.Pipeline import pipeline

   processor = pipeline(task="property-prediction")

   item = "如图所示，则三角形ABC的面积是_。"
   processor(item)

也可以为推理任务指定自定义本地或 ModelHub 模型来进行推理。

* 指定本地模型：

::

   from EduNLP.Pipeline import pipeline
   from EduNLP.ModelZoo.rnn import ElmoLMForPropertyPrediction
   from EduNLP.Pretrain import ElmoTokenizer

   pretrained_pp_dir = f"examples/test_model/elmo/elmo_pp"
   tokenizer = ElmoTokenizer.from_pretrained(pretrained_pp_dir)
   model = ElmoLMForPropertyPrediction.from_pretrained(pretrained_pp_dir)
   model.eval()
   processor = pipeline(task="property-prediction", model=model, tokenizer=tokenizer)

   item = "如图所示，则三角形ABC的面积是_。"
   processor(item)

* 指定ModelHub模型：

::

   # Coming soon...

预处理流水线
-----------------------
我们准备了一系列预处理组件，包括各种针对成分分解和语法解析的组件类，可以直接通过在 *pipeline()* 中传入组件标识符构建。

你也可以在构建好流水线后手动插入预置组件或自定义组件。仅在插入预置组件时，可以为组件指定参数。

例如：

::

   from EduNLP.Pipeline import pipeline

   item = "如图所示，则三角形ABC的面积是_。"
   processor = pipeline(preprocess=['is_sif', 'to_sif', 'is_sif', 'seg_describe'])
   processor.add_pipe(name='seg', symbol='fm', before='seg_describe')
   print(processor.component_names)

   processor(item)


混合流水线
-----------------------
在 *pipeline()* 中同时传入预处理组件参数和任务参数即可构建混合流水线：

例如：

::

   from EduNLP.Pipeline import pipeline

   processor = pipeline(task="property-prediction", preprocess=['is_sif', 'to_sif', 'is_sif', 'seg_describe'])
   processor(item)