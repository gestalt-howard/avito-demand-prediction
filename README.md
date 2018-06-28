# Avito Demand Prediction Kaggle Challege

***Project Completion Date: June 27, 2018***

![avito logo](https://github.com/gestalt-howard/avito-demand-prediction/blob/master/images/logo-avito.png)

## Project Overview:
The 2018 Avito Demand Prediction Kaggle Challenge was hosted by Avito, the Russian version of "Craigslist". In this challenge, Avito wanted Kagglers to predict how successful any particular ad listing would be based on criteria such as ad description, time of posting, image quality, ad title, and more. By doing this, Avito hopes to provide a better service to their customers by forecasting the demand of a posted product. The competition homepage can be found here: https://www.kaggle.com/c/avito-demand-prediction.

My final ranking was 563rd place out of 1917, **in the top 30%**. The following sections contain a detailed description of the files in this repo along with the approaches I tried throughout the course of this competition.

## File Descriptions:

* **sample_submission.csv**: A sample submission file that demonstrates the proper template of this Kaggle competition's submission

### Data Folder:
Please note that due to Github's file size restrictions, I couldn't upload any of Avito's datasets. To replicate my results, please go to the Avito challenge's website (provided above), download the **train.csv.zip** and **test.csv.zip** files, unzip both files, and place them into the ***data*** directory.
* **models0**: Contains prediction files outputted from Stage 0 models
* **models1**: Contains prediction files outputted from Stage 1 models
* **models2**: Contains prediction files outputted from Stage 2 models

### Scripts Folder:
The scripts folder contains notebooks that detail my data exploration and model generation / training processes through 3 stages.
* *Stage 0*: Initial data exploration and first model using Light GBM
* *Stage 1*: Data pipeline experimentation using multiple configurations of Light GBM
* *Stage 2*: Inclusion of additional features and final project stage

If you'd like to retrace my steps, I recommend running through the notebooks in the following sequence:

1. **parameters_json_gen.ipynb**: Generates a JSON file containing all the folder and file paths necessary for the data exploration and machine learning models to run
2. **Stage_0_Data_Exploration**: Running through this notebook will provide foundational insights into the datasets and also generate the Stage 0 and Stage 1 preprocessed training and test datasets
3. **All Other Scripts**: After the previous two notebooks have been run, you can run any other of the other notebooks in any desired order *(each notebook contains a high-level introduction of the model's function)*

## Approach and Methods:
### Stage 0:
My first step in this project was performing an exhaustive feature analysis and preprocessing (contained in the ***Stage_0_Data_Exploration.ipynb*** script). In this notebook, I explored statistical characteristics of all features given in the **train.csv** and **test.csv** datasets provided by Avito. My focus at this stage was to keep my dataset as lightweight as possible to enable rapid model prototyping. By performing detailed data analysis first, I was able to streamline my feature-selection process to create a relatively small preprocessed dataset.

One of the most fascinating results of this preliminary data analysis is shown in the figure below:

![deal probability artifact](https://github.com/gestalt-howard/avito-demand-prediction/blob/master/images/deal_prob_artifact.png)

After de-noising the deal probability feature *(aka the target variable)* by thresholding the counts of feature occurrences, it can be seen that there appear to be two general clusters of deal probabilities (one to the left of 0.6 and one to the right). In Stage 1 of this project, I explore the relevancy of this behavior.

Following the initial data visualization, I ran a Light GBM regression model to procure my first leaderboard scores. Conveniently, the Light GBM API also included a functionality for determining the most important features from the training dataset. These "most-important" features are visualized below:

![stage 0 important features](https://github.com/gestalt-howard/avito-demand-prediction/blob/master/images/stage0_important.png)

### Stage 1:
After the Stage 0 Model revealed that **image_top_1** was the most important feature, I decided to attempt to optimize my model's pipeline in an effort to exploit this behavior. Stage 1 yielded a total of 3 different models:
* **Model 1v0**: Separated the training and test datasets into **four (4)** distinct segments based on **image_top_1** values determined in the Stage 1 data exploration notebook and subsequently trained a separate Light GBM regressor on each set
* **Model 1v1**: Implemented a different pipeline from 1v0
  * Model 1v1 first split the training and test datasets using a binary classifier that exploited the **deal probability** behavior discovered during Stage 0 data exploration (0 or 1 for above or below the 0.6 threshold)
  * Light GBM regressors were trained on the two splits
* **Model 1v2**: Attempted to address a major shortcoming of Model 1v1
  * Since there are significantly more samples in the *below threshold* set, the binary classifier in 1v1 is heavily biased towards the *below threshold* class
  * Model 1v2 implemented upsampling of the sparser *above threshold* set to balance out the binary classifier's training set

In the end, the 3 different models produced in Stage 1 yielded improvements over Stage 0. However, these improvements weren't substantial enough to justify adopting a more complex pipeline and also suggested that more work could've been done on the feature preprocessing front.

### Stage 2:
The final stage in this project looked into the effects of a different preprocessing strategy. There were a total of 2 models created in this stage:
* **Model 2v0**: Replaced one-hot encoding of categorical features with a single-value encoding and reverted to a simple model pipeline of a single Light GBM regressor model
* **Model 2v1**: Implemented TFIDF transformation of all text information and also included text metadata

Despite including a preprocessing scheme that yielded significantly more features (around 300k) than my other models, Model 2v1 yielded my final winning results. This result speaks to the robustness of Light GBM in extracting meaningful insights even when dealing with a nontrivial quantity of features.

### Final Thoughts:
Considering that I began this project on June 14 and only made my first submission on June 21, I'm happy to have made it into the top 30% in my first serious Kaggle challenge. Furthermore, I'm exiting this challenge with a much-deeper understanding of gradient boosting and its highly robust cousin: Light GBM. I look forward to utilizing my updated skillset in the many projects to come.
