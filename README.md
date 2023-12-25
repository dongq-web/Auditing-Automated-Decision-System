# Evaluating Toxicity Classification
### Summary
Comment classification is an Natural Language Processing(NLP) task aims at identifying postentially harmful comments on online platform.

<img src=/Images/classification.png width=50% height=50%>

The Kaggle Competition, Jigsaw Unintended Bias in Toxicity Classification, is a competition to detect toxic comments while minimizing unintended bias. 
- Toxicity = anything rude, disrespectful, or otherwise likely to make someone leave a discussion
- bias = incorrectly classify a comment as toxic because it mentions certain identity, such as gender, race or religion

The task of this project is to evaluate a **LSTM** based model to calculate its fairness metrics and analyze its interpretability.

### Dataset
The dataset used in the ADS originates from Civil Comments platform, and Jigsaw extended annotations from human raters for this competition. 

**Sample Comments and Toxicity Score**

<img src=/Images/sample_data.png width=70% height=70%>

Comments with toxicity scores > 0.5 are classified as toxic

There are 45 columns in the training data, of which 9 are "identity columns" and 6 are "auxiliary columns". In total, we used 16 features. Among them, only 1 (text) is the real input, 9 features are for bias determination, and 5 are for the output features. 

**Feature Datatypes**

<img src=/Images/features.png width=50% height=50%>

**Correlation Map between Identity Columns and Auxiliary Columns**

<img src=/Images/heatmap.png width=70% height=70%>

Most of the identity columns are positive correlated with the the output columns. For example, the correlation between white(identity) and target(toxicity) is 0.19, which means that if a comment mentions white, it is more likely for it to be classified as toxic.

### Evaluation
- Subgroup AUC = AUC of the dataset that mentions specific identity subgroup
- Background Positive Subgroup Negative (BPSN) AUC = AUC baased on non-toxic examples that mention the identity and toxic examples that do not
- Background Negative Subgroup Positive (BNSP) AUC = AUC based on toxic examples that mention the identity with non-toxic examples that do not

<img src=/Images/bias_metrics.png width=70% height=70%>

### Interpretability
The model's performance on the out-of-distribution (OOD) samples was investigated.

**Correct Sample**
<img src=/Images/correct.png>

**False Negative Sample**

<img src=/Images/fn.png>

While word capitalization could indicate toxicity, this is hidden by the classifier because it uses all lowercase tokenization. 

**False Positive Sample**

<img src=/Images/fp.png>

Sample was classified incorrectly as positive because some biased identity words (American, Ismalist)
