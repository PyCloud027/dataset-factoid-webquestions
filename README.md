WebQuestions QA Benchmarking Dataset
====================================

WebQuestions (http://nlp.stanford.edu/software/sempre/ - Berant et al.,
2013, CC-BY) is a popular dataset for benchmarking QA engines,
especially ones that work on structured knowledge bases.

This is an effort to make the dataset more organized and easier to use.
We assign a unique ID to each question, and distribute extra annotations
for each question, e.g. pertaining the questions and Freebase.  We also
provide several topic-based splits.

This is a development version of the dataset.  If you use it, please
cite the Git repository and date + shortid of the last commit.  There
are no stability guarantees yet both regarding the data format and the
actual set of questions.  This dataset is distributed under the terms
of the CC-BY 4.0 licence.

  * ``main/`` has the dataset splits as distributed.
  * ``d-freebase/`` has extra Freebase-specific data.
  * ``d-nlp/`` has extra data from NLP analysis of the question texts.  (TODO)
  * ``t-movies/`` has question sub-splits pertaining to the movies topic.  (TODO)

Splits
------

The original WebQuestions dataset has *train* (3778 q) and *test*
(2032 q) splits.  We keep all the questions within their respective
splits, but separate *train* to a few further splits for the benefit
of application of machine learning methods:

  * *devtest* (189 q):  A set of questions to use for development but
    not to train models on.  You may want to use these to decide which
    features to add, then check if the features generalize even for
    questions the model was not trained on.

  * *val* (755 q): A validation set - questions you do not use for
    training the model but for testing its performance when tuning
    the model (set of features, hyperparameters...).  By using this
    set instead of the *test* set, you are making sure that you are
    not overfitting for the test set indirectly.  Ideally, you would
    completely withhold the *test* set, reporting only the final
    performance of your model on it, and use the *val* set for
    day-to-day development.

  * *trainmodel* (2834 q): A set of questions to use for model training.

Using these further splits is optional, just make sure to report whether
you used the sub-splits or the complete *train* split for training (e.g.
you are testing just a trivial model that requires no tuning, or simply
want to be very strict about comparability with past research).  It is
also okay to use *devtest* and *val* combination as a validation set.

To generate a .json file with the full *train* split, run
``scripts/mktrain.py``.

Data Model
----------

The questions have identifiers in the form of "wqr%06d" (train) or
"wqs%06d" (test) respectively, where %06d is six-digit number assigned
based on the original dataset order.  The master JSON files consist
of an array with single object per question, where each object has
string attribute "qId", string attribute "qText" and an attribute
"answers" which is an array of strings.

Various extra data are present in the ``d-*/`` directories (see the
respective READMEs).  To build a single file per split with full data
for each question, run ``scripts/fulldata.py``.

To build a YodaQA-compatible dataset in the TSV format, run
``scripts/json2tsv.pl``.
