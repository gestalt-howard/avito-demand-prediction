# Avito Demand Prediction Kaggle Challege

***Project Completion Date: June 27, 2018***

![avito logo](https://github.com/gestalt-howard/avito-demand-prediction/blob/master/images/logo-avito.png)

## Project Overview:
The 2018 Avito Demand Predictio Kaggle Challenge was hosted by Avito, the Russian version of "Craigslist". In this challenge, Avito wanted Kagglers to predict how successful any particular ad listing would be based on criteria such as ad description, time of posting, image quality, ad title, and more. By doing this, Avito hopes to provide a better service to their customers by forecasting the demand of a posted product. The competition homepage can be found here: https://www.kaggle.com/c/avito-demand-prediction.

Ultimately, I ended up ranking **563rd place out of 1917**. A description of the files in this repo along with the approaches I tried in this competition is below.

## File Descriptions:

* **sample_submission.csv**: An sample submission file that details the template of a submission

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

If you'd like to retrace my steps, I recommend running through the notebooks in the following order:

1. **parameters_json_gen.ipynb**: 

## Project Approach:
The project kick-off was an exhaustive feature analysis and preprocessing script contained in ***Stage_0_Data_Exploration.ipynb***. In this notebook, I explored statistical characteristics of all the given features
