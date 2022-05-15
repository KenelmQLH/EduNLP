Pretraining
==============

In the field of NLP, Pretrained Language Models has become a very important basic technology.
In this chapter, we will introduce the pre training tools in EduNLP:

* How to train with a corpus to get a pretrained model
* How to load the pretrained model
* Public pretrained models

Import modules
---------------

::

   from EduNLP.I2V import get_pretrained_i2v
   from EduNLP.Vector import get_pretrained_t2v

Train a model
------------------

The module interface definition is in `EduNLP.Pretrain`, including tokenization, data processing, model definition, model training.

Basic Steps
##################

1.Determine the type of model and select the appropriate tokenizer (GensimWordTokenizer、 GensimSegTokenizer) to finish tokenization.

2.Call `train_vector` function to get the required pretrained model。

Examples：

::

   >>> tokenizer = GensimWordTokenizer(symbol="gmas", general=True)
   >>> token_item = tokenizer("有公式$\\FormFigureID{wrong1?}$，如图$\\FigureID{088f15ea-xxx}$,\
   ... 若$x,y$满足约束条件公式$\\FormFigureBase64{wrong2?}$,$\\SIFSep$，则$z=x+7 y$的最大值为$\\SIFBlank$")
   >>> print(token_item.tokens[:10])
   ['公式', '[FORMULA]', '如图', '[FIGURE]', 'x', ',', 'y', '约束条件', '公式', '[FORMULA]']
   
   # 10 dimension with fasstext method
   train_vector(sif_items, "../../../data/w2v/gensim_luna_stem_tf_", 10, method="d2v")


Load models
----------------

Transfer the obtained model to the I2V module to load the model.
 
Examples：

::

   >>> model_path = "../test_model/d2v/test_gensim_luna_stem_tf_d2v_256.bin"
   >>> i2v = D2V("text","d2v",filepath=model_path, pretrained_t2v = False)


Examples of Model Training
------------------------------------

Get the dataset
####################

.. toctree::
   :maxdepth: 1
   :titlesonly:

   prepare_dataset  <../../build/blitz/pretrain/prepare_dataset.ipynb>

Examples of d2v in gensim model
##################################

.. toctree::
   :maxdepth: 1
   :titlesonly:

   d2v_bow_tfidf  <../../build/blitz/pretrain/gensim/d2v_bow_tfidf.ipynb>
   d2v_general  <../../build/blitz/pretrain/gensim/d2v_general.ipynb>
   d2v_stem_tf  <../../build/blitz/pretrain/gensim/d2v_stem_tf.ipynb>

Examples of w2v in gensim model
##################################

.. toctree::
   :maxdepth: 1
   :titlesonly:

   w2v_stem_text  <../../build/blitz/pretrain/gensim/w2v_stem_text.ipynb>
   w2v_stem_tf  <../../build/blitz/pretrain/gensim/w2v_stem_tf.ipynb>

Examples of seg_token
#############################

.. toctree::
   :maxdepth: 1
   :titlesonly:

   d2v.ipynb  <../../build/blitz/pretrain/seg_token/d2v.ipynb>
   d2v_d1  <../../build/blitz/pretrain/seg_token/d2v_d1.ipynb>
   d2v_d2  <../../build/blitz/pretrain/seg_token/d2v_d2.ipynb>

Examples of advanced models
#############################

.. nbgallery::
    :caption: This is a thumbnail gallery:
    :name: pretrain_gallery_en1
    :glob:

    ELMo pretrain  <../../build/blitz/pretrain/elmo.ipynb>

    BERT pretrain <../../build/blitz/pretrain/bert.ipynb>


.. nbgallery::
    :caption: This is a thumbnail gallery:
    :name: pretrain_gallery_en2
    :glob:

    DisenQNet pretrain  <../../build/blitz/pretrain/disenq.ipynb>

    QuesNet pretrain <../../build/blitz/pretrain/quesnet.ipynb>