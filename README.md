# Auditing Automated Decision System
### Summary
The Kaggle Competition, Jigsaw Unintended Bias in Toxicity Classification, is a competition to detect toxic comments while minimizing unintended bias. Toxicity is defined as anything rude, disrespectful, or otherwise likely to make someone leave a discussion. In their last year's competition to classify toxic comments, they found that the models incorrectly label names with frequently attacked identity with toxicity. Therefore, Jigsaw Conversation AI team opened this competition to build an Automated Decision System(ADS) that minimizes this type of unintended bias while recognizing toxicity. The ADS that we chose to audit uses LSTM and has the highest votes.

### Dataset
The dataset used in the ADS originates from Civil Comments platform, and Jigsaw extended annotations from human raters for this competition. When obtaining the toxicity label, each comment was shown to up to 10 annotators, and annotators were asked to rate the toxicity to very toxic, toxic, hard to say, and not toxic. To collect identity label, annotators were asked to rate all the attributes that were mentioned in the comment. 

There are 45 columns in the training data, of which 9 are "identity columns" and 6 are "auxiliary columns". In total, we used 16 features. Among them, only 1 (text) is the real input, 9 features are for bias determination, and 5 are for the output features. 

**Feature Datatypes**

<img src=/Images/features.png width=50% height=50%>

**Correlation Map between Identity Columns and Auxiliary Columns**

<img src=/Images/heatmap.png width=70% height=70%>

### Evaluation
The validity of the ADS was evaluated using a newly developed metric that balances overall performance with unintended bias by combining the overall metric with several sub-metrics. The overall metric used was the area under the receiver operating characteristic curve (AUC), and the ADS achieved an AUC of 0.9679 when tested on the expanded test set that was released after the competition. To assess the model's performance on different demographic subsets, various bias AUCs were calculated on each subset, each representing an identity. Subgroup AUC was based on examples that mention a specific identity, Background Positive Subgroup Negative (BPSN) AUC was based on non-toxic examples that mention the identity and toxic examples that do not, and Background Negative Subgroup Positive (BNSP) AUC was based on toxic examples that mention the identity with non-toxic examples that do not. The bias metrics were combined using power mean, and the final metric was calculated by taking the average of the overall metric and the bias metrics. Having a high final metric (0.9350) means that the ADS achieves high accuracy while minimizing the unintended bias.

**Bias AUCs**

<img src=/Images/bias_metrics.png width=70% height=70%>

### Interpretability
The ADS's performance on the out-of-distribution (OOD) samples was investigated and some False Negative and False Positive samples were highlighted.

**False Negative Sample**

<img src=/Images/fn.png>

While word capitalization could indicate toxicity, this is hidden by the classifier because it uses all lowercase tokenization. 

**False Positive Sample**

<img src=/Images/fp.png>

Sample was classified incorrectly as positive because some biased identity words (American, Ismalist)
