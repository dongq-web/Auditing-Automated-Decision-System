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
To better evaluate the model's performance on reducing bias, the comments are divided into two groups: identity group and background group. Identity group is the comments that mention certain identities, such as gender, race and religion, whereas backgound group is the comments that don't mention any identity. Among the two groups of comments, there are positive examples that are toxic and negative examples that are non-toxic.

<img src=/Images/group.png width=70% height=70%>

- Subgroup AUC = AUC on the examples that mention specific identity subgroup. A low value in this metric means the model does a poor job of distinguishing between toxic and non-toxic comments that mention the identity.

<img src=/Images/subgroup.png width=70% height=70%>
  
- Background Positive Subgroup Negative (BPSN) AUC = AUC based on negative examples from the identity group and positive examples from the background group. A low value in this metric means that the model confuses non-toxic examples that mention the identity with toxic examples that do not.

<img src=/Images/bpsn.png width=70% height=70%>

- Background Negative Subgroup Positive (BNSP) AUC = AUC based on positive examples from the identity gorup and negative examples from the backgroup group. A low value here means that the model confuses toxic examples that mention the identity with non-toxic examples that do not.
  
<img src=/Images/bnsp.png width=70% height=70%>

<img src=/Images/bias_metrics.png width=70% height=70%>

Among the 9 identity subgroups, the black subgroup had the lowest Subgroup AUC, while the Christian subgroup had the highest Subgroup AUC. However, all subgroups achieved high BPSN AUC and BNSP AUC scores, indicating that the model was able to accurately distinguish between toxic and non-toxic examples that mention the identity from those that do not.

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
