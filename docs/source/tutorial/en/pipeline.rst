=======
Pipeline
=======

Pipeline provides a method out-of-the-box applying for many different tasks, which supports extending custom components.
Check the table below for all supported tasks:

.. list-table:: Supported Tasks
   :widths: 30 30 30
   :header-rows: 1

   * - Task
     - Description
     - Pipeline identifier
   * - Pre-processing
     - apply for no downstream task, only execute component segmentation and syntax parsing
     - "pre-process"
   * - Property prediction
     - prediction the property for a given question
     - "property-prediction"
   * - Knowledge prediction
     - prediction the hierarchical knowledge points for a given question
     - "knowledge-prediction"

This tutorial will teach you to use EduNLP Pipeline in these ways:

* Use a *pipeline()* to create a pipeline for specific task inference
* Use a *pipeline()* to create a pre-processing pipeline for no downstream inference task
* Use a *pipeline()* to create a more flexible pipeline for both together


Pipeline for specific task inference
-----------------------
With a general *pipeline()*, you can create any downstream task pipeline and automatically load a default model.
Pass your formatted question items and return the inference results.

::

   from EduNLP.Pipeline import pipeline

   processor = pipeline(task="property-prediction")

   item = "如图所示，则三角形ABC的面积是_。"
   processor(item)

You can also use a specific model, whether from local or ModelHub

* Specify a local model：

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

* Specify a ModelHub model：

::

   # Coming soon...

Pre-processing Pipeline
-----------------------
We prepared various pre-processing components, including for component segmentation and syntax parsing.
You can construct it simply by passing component identifiers to *pipeline()*.

You can also manually insert pre-built or custom components. Specified parameters are only allowed for inserted pre-built components.

For example：

::

   from EduNLP.Pipeline import pipeline

   item = "如图所示，则三角形ABC的面积是_。"
   processor = pipeline(preprocess=['is_sif', 'to_sif', 'is_sif', 'seg_describe'])
   processor.add_pipe(name='seg', symbol='fm', before='seg_describe')
   print(processor.component_names)

   processor(item)


Mixed pipeline
-----------------------
Construct a mixed pipeline simply by passing pre-processing and task-related parameters together to *pipeline()*.

For example：

::

   from EduNLP.Pipeline import pipeline

   processor = pipeline(task="property-prediction", preprocess=['is_sif', 'to_sif', 'is_sif', 'seg_describe'])
   processor(item)