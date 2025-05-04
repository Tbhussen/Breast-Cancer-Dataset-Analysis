# Breast-Cancer-Dataset-Analysis
In this repository, I applied several Data Exploration and Machine Learning skills.

This project made use of the Breast Cancer Dataset found at: https://www.kaggle.com/datasets/yasserh/breast-cancer-dataset/data.

This dataset contains 569 instances of tumor samples categorised into either malignant (M) or benign (B). This dataset is a widely used dataset for breast cancer diagnosis, featuring labeled data points that help in distinguishing malignant from benign cases. This dataset is commonly used for classification tasks in machine learning and medical research.

To start with, we loaded the data and applied some EDA analysis to understand the relations among the different variables. 

- First, we applied `str` function from `dplyr` package to get a sense of what the data contains and the types of variables present.
- Then, we used various visualization to get a feeling of the relations between the variables.
  - In the first set of visualizations using a boxplot, we watched the difference in the distribution of records or variables like: **radius_mean**, **area_mean**, **texture_mean**, **perimeter_mean**, **smoothness_mean**, and **compactness_mean**; between benign and malignant state of the tumor.
  - Another plot that we utilized is the line plot, which we used to get the correlation between each pair of variables. Pairs with scattered lines meant absence of real correlation among them, while lines withh positive slope indicated positive correlation. It is worth mentioning that no negative line slopes were seen.
  - Finally we used a scatter plot to observe the correlation among pairs of variables while applying a coloring based on the `diagnosis`. Here, some variable pairs clearly showed no correlation and unclear distinguishing of the clusters, benign and malignant.

The following step was the construction of confusion matrix. The confusion matrix created by using the `corrplot` package showed the diffrent correlation present across all the variables.

**Model Building**

As the main target of this projcet was to build an algorithm able to predict the breast cancer tumor state, we developed a random forest model which has many advantages that suit our goal. 

Our random forest model achieved a 96.46 % accuracy which can be further inhanced using dimention reduction techniques. Also, this project can be developed to include or exclude some features and observe the accuracy of model improve. 

Lastly, we made a plot of the features' importance based on the random forest model deployed.
