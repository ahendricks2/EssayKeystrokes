# Predicting Writing Quality with Writing Processes Project Read Me

January 2024

Andrew Hendricks


## Overview

This project is based on the Kaggle competition [Linking Writing Processes to Writing Quality](https://www.kaggle.com/competitions/linking-writing-processes-to-writing-quality).  
  
The goal of the project is to use the data about each student's writing process to predict the score that they receive on their essay while minimizing rmse.  The challenge of the project is that the logs do not include any text from the essay. The predictions have to be made only from writing process behaviors.

## Repository Navigation

[Notebook 1](https://github.com/ahendricks2/EssayKeystrokes/blob/main/Preprocessing_Notebook.ipynb): Contains data aggregation, feature engineering, and EDA

[Notebook 2](https://github.com/ahendricks2/EssayKeystrokes/blob/main/Modeling_Notebook.ipynb): Contains modeling, a second round of feature selection and engineering, and results, conclusions and next steps

[Non-Technical Presentation](https://docs.google.com/presentation/d/1y_g_qGL9L35e6C5tul215TAF6iLZ1nu2zQMeINit31M/edit?usp=sharing)


## Business Problem and Data Understanding

This project is hosted by Vanderbilt University and the Learning Agency Lab. The goal of the project as stated on the competition home page is to "use process features... to predict overall writing quality... This may help direct learners’ attention to their text production process and boost their autonomy, metacognitive awareness, and self-regulation in writing."  By deemphasizing the final writing product, this analysis can help focus writers on elements of writing that they may not usually consider. In addition to students, the analysis could also benefit educators by making them more aware of the processes that do and do not correlate with success.

To summarize the Kaggle competition page, essay writers were hired from Amazon Mechanical Turk and had to use computers with keyboards. Writers had 30 minutes to write their responses to one of four randomly assigned prompts that were based on retired SATs. Before writing, the writers were given instructions about how to write their essays, including features such as a position, evidence, a minimum of 3 paragraphs, and a minimum of 200 words. Participants were warned when they had been active for 2 minutes or moved to a new window.

The train data set for the competition includes scores for about 2.7k essays as well as a set of logs with thousands of records for each essay with over 8 million records total. The test data set is hosted on Kaggle and is not pupblicly available. The log features include the up and down time of each action, a label and a record for what occurs during each action, the cursor position, and the word count. The majority of essays in the training set had scores between 3 and 4.5, and the distribution of the scores was generally normal.


![Score_Spread](https://github.com/ahendricks2/EssayKeystrokes/assets/141271148/f6c8d807-ea9e-4886-b9b6-3448ccb16c89)


## Data Analysis

While conducting exploratory data analysis, I found that features related to the volume of text created or actions performed tended to correlate with essay score.

![Word_Count](https://github.com/ahendricks2/EssayKeystrokes/assets/141271148/29776065-1347-4c09-ba65-585f4e87db97)

![Event_Count](https://github.com/ahendricks2/EssayKeystrokes/assets/141271148/c3677190-d13f-496e-926c-f998869bf7af)

An analysis of pauses revealed a correlation between the amount of text produced per pause and the final score.

![Characters_Per_Pause](https://github.com/ahendricks2/EssayKeystrokes/assets/141271148/c8b3180f-a75a-4b36-9488-4152beef1924)

By contrast, there was not a substantial correlation between the percentage of time used for revision and the final score.

![Revision](https://github.com/ahendricks2/EssayKeystrokes/assets/141271148/94320188-be71-476b-aacd-9015614700ad)


## Modeling and Evaluation

Over the course of the modeling process, I used a random forest regressor, a gradient boosting regressor, an adaptive boosting regressor and XGB with hyperparameter tuning based on grid search.

After hyperparameter tuning, the xgb model was my strongest model, performing slightly better than the gradient boosting regressor.  Overall, the random forest model, gradient boosting model and xgb model performed comparably, and the ADA boost model had the worst performane. 

At the end of the modeling process, I put my three strongest models into a stacking model to improve generalization on unseen data.

<img width="158" alt="Final_Results" src="https://github.com/ahendricks2/EssayKeystrokes/assets/141271148/00987a36-a9d7-4e91-bbf9-e4c42458cc1d">


## Results and Conclusions

My final model had cross-validation scores of .63 rmse and .61 r2. The test data scorer on Kaggle does not include r2, but my final rmse private score was .607.

For the students, educators, and researchers who would be the main stakeholders for this project, it is interesting that the model was able to explain over 60% of the variance in essay scores without any access to the actual text of the essay.  While most analyses of essay quality may focus on features like the quality of the argument or the use of evidence, it is useful to know that factors related purely to writing process have predictive importance for the score of the essay as well.

![Feature_Importance](https://github.com/ahendricks2/EssayKeystrokes/assets/141271148/bf33ba5d-f66a-46c3-8981-325fe6ca15d1)

After the second round of feature selection and feature engineering, word_count and character count remained the two features with the highest predictive importance. The number of input events was also in the top 5 of feature importance. For essay writers, educators, and researchers, this suggests that more is usually more.  Writers who produce more words and characters tend to earn higher scores on their essays.

Factors that pointed towards the complexity of words and sentences-- including features related to the use of commas and hyphens as well as characters per word and sentence length-- also had some of the stronger importances. This suggests that essays that used a writing style that included longer and more complex words and sentences tended to earn higher scores as well.

The absence of certain features in the top 15 was also of interest. Features that pointed towards revision-- such as deletions, replacements, or moving text-- did not have the predictive importance of features related to text production. This may have related to the conditions surrounding the task: writers had only 30 minutes to work, so it may have been more beneficial to use that limited time to produce more text than it was to revise or edit. Still, in this context, the 'good writing is rewriting' maxim did not seem to apply.

To synthesize, the feature importances suggested that writing more words and more characters correlated with earning higher scores. Producing longer words and sentences with commas and hyphens did, too. Actions related to revision had weaker correlations to the final score.
