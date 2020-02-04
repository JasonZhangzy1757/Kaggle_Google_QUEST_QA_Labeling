# Kaggle_Google_QUEST_QA_Labeling
This is a Kaggle Competition started on Dec. 21 2019. URL: https://www.kaggle.com/c/google-quest-challenge/overview

In this competition, we need to use a new dataset to build predictive algorithms for different subjective aspects of question-answering. The question-answer pairs were gathered from nearly 70 different websites, in a "common-sense" fashion. 

There are a list of 30 target labels, which are the same as the column names in the `sample_submission.csv` file. Target labels with the prefix `question_` relate to the `question_title` and/or `question_body` features in the data. Target labels with the prefix `answer_` relate to the answer feature.

Each row contains a single question and a single answer to that question, along with additional features. The training data contains rows with some duplicated questions (but with different answers). The test data does not contain any duplicated questions.

This is not a binary prediction challenge. Target labels are aggregated from multiple raters, and can have continuous values in the range [0,1]. Therefore, predictions must also be in that range. Also, we could see from EDA, values are not literally continuous. It is divided into about nine values, like 1/3, 1/2, 2/3 etc.

Submissions are evaluated on the mean column-wise [Spearman's correlation coefficient](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient). The Spearman's rank correlation is computed for each target column, and the mean of these values is calculated for the submission score.

## Stage 1: Find a Baseline

 - [`200102_Data&EDA_Jason.ipynb`](https://github.com/JasonZhangzy1757/Kaggle_Google_QUEST_QA_Labeling/blob/master/200102_Data%20%26%20EDA_Jason.ipynb) is general EDA over the datasets, so we could have a general understanding of the datasets. 
 
 - Then without doing much feature engineering, a fine-tuned Bert Model [`200102_BertModel_Jason.ipynb`](https://github.com/JasonZhangzy1757/Kaggle_Google_QUEST_QA_Labeling/blob/master/200101_BertModel_Jason.ipynb) is applied to reach a baseline score of 0.385.
 
 - From the cross validation result from [`191228_Baseline_with_validation_Cara.ipynb`](https://github.com/JasonZhangzy1757/Kaggle_Google_QUEST_QA_Labeling/blob/master/191228_Baseline_with_validation_Cara.ipynb) we could find that the general performance is good while three columns among them: `question_not_really_a_question`, `question_type_consequence` and `question_type_spelling` have extremely bad performance due to dramatic imbalance in the training set. 

## Stage 2: Deal with imbalanced columns

 - We have tried Stratified KFold to make sure the distribution of validation samples is the same with the entire dataset to eliminate the imbalance in the training set. The code is [here](https://github.com/JasonZhangzy1757/Kaggle_Google_QUEST_QA_Labeling/blob/master/190103_StratifiedKFold_Emmy.ipynb). Yet the performance never improved.
 
 - Also, we tried to do feature engineering, because for the column `question_type_spelling`, every non-zero value is in the `CULTURE` category (in particular, the `english` or `ell` (English Language Learners) stackexchange URLs). As such, it worked in post-processing to just hardcode '0.0s' in any non-`CULTURE` category and 1.0 for the rest. The code is [here](https://github.com/JasonZhangzy1757/Kaggle_Google_QUEST_QA_Labeling/blob/master/200128_bert-tf2_treat_question_type_spelling_Cara.ipynb) and the performance improved to 0.389.

# Uploading Format
If anyone has tried anything and think it'll be helpful to share, please keep the following format： __"Time_Title_Name"__
- e.g. `191228_EDA_Jason`, `191227_BERT_Trial1_Jason`


### Github Tutorial
If you are not really familiar with how to connect github with your local git or having problems with creating pull requests or branches, there is a simple github tutorial, it might be helpful. 

And if on the contrary, you are very familiar with github and think there should be more in the tutorail, feel free to add more.
