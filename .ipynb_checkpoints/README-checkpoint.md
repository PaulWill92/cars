# Predicting Car Prices using Machine Learning

![Challenger](./figures/challenger.jpg)
Image taken from [Fineartamerica](https://fineartamerica.com/featured/5-dodge-challenger-srt-hellcat-draw-carstoon-concept.html).

Within this repo, I created a regression model that predicts car prices.
I found that by doing so, I would be able to aid people like me (a year ago) who needed to sell/buy a car and had no idea what a fair price is.

Use this [app](In progress) to predict price

## Contents

1. [Presentation Slides](https://drive.google.com/file/d/1mDAKD81HligeDWsKDlGVd0qk2NiRACtr/view?usp=sharing)
2. [Data Gathering](https://github.com/PaulWill92/cars/blob/master/Jupyter-Notebooks/01-Automated_Data_Gathering.ipynb)
3. [Data Cleaning](https://github.com/PaulWill92/cars/blob/master/Jupyter-Notebooks/web-scrapers/02-Manual_Data_Cleaning.ipynb)
4. [Data Exploration](https://github.com/PaulWill92/cars/blob/master/Jupyter-Notebooks/02-Data_Exploration.ipynb)
5. [Data Modeling](https://github.com/PaulWill92/cars/blob/master/Jupyter-Notebooks/03-Data_Modeling.ipynb)

My Socials:

- Email: paulaleksis@gmail.com
- Linkedin: [Paul-Aleksis](https://www.linkedin.com/in/paul-aleksis-406776199/)
- Medium: [My Medium](https://medium.com/swlh/predicting-car-prices-using-machine-learning-60a98a56f971)

### Executive Summary

When I was moving to England from America, I had to sell my car. At that time I did a lot of googling and research to try to figure out what the fair market price was. Through my research, I was able to narrow down a fair price. I noticed a lot of fluctuation between car prices even among cars that were the same make and model as my car. What if at that time, I could have created a machine learning algorithm that could have done all that research for me. The problem this repo is going to solve is as follows: **Can I use machine learning to predict car prices based off certain features?**

In order to help solve this problem, I recognized the parameters of my problem:

1. The outcome I am trying to figure out is a single outcome (univariate)
2. I will need many features of a car to figure out its price (multivariable)
3. The price can be a minimum or maximum value of a specific currency (continuous variable)
4. The features and price of the data I will extract are labeled (supervised)

Knowing the above parameters, I know that In order to solve my problem, I am able to use a regressor.

### Methodology

To get started, I created a web crawling script found in my [Data Gathering](https://github.com/PaulWill92/predict-car-prices/blob/master/Jupyter-Notebooks/01-Data_Gathering.ipynb) notebook, to extract features of certain up to date cars from [Auto Village](https://www.autovillage.co.uk/used-car/filter/bodystyle/saloon). I was able to store these values in a list, form a data frame out of the list values, and save that data frame as a [csv file](https://github.com/PaulWill92/predict-car-prices/blob/master/Cleaned-Data/cleaned_cars.csv).

Each car cell that was scraped looked like this:

![Web car](./figures/website_car.png)

The raw scraped dataframe looked like this:

![Data Raw](./figures/scraped_df.png)


After extracting my data, I began cleaning the data set to provide the correct format for modeling in this [Data Cleaning](https://github.com/PaulWill92/cars/blob/master/Jupyter-Notebooks/web-scrapers/02-Manual_Data_Cleaning.ipynb) notebook. I utilized many string manipulation techniques to split my features into seperate columns. By the end of the cleaning, I was left with 1 target variable and 8 predictors.

After the feature engineering and transformations, dataframe looked like this:

![cleaned](./figures/cleaned_df.png)

After data cleaning, I explored my data set looking for outliers and checking the linear correlations between my target variable and predictors. I was able to feature engineer new features. The process is shown in my [Data Exploration](https://github.com/PaulWill92/cars/blob/master/Jupyter-Notebooks/02-Data_Exploration.ipynb) notebook.

### Corelation of new created features
![Heatmap](./figures/heatmap_new_feat.png)

### Correlation of all features
![Heatmap](./figures/heatmap.png)


After exploration, I proceeded to model you can find my modeling in my [Data Modeling](https://github.com/PaulWill92/cars/blob/master/Jupyter-Notebooks/03-Data_Modeling.ipynb) notebook. Since my data set contained multiple categorical values, I had to encode them to be able to use within my baseline model. Once encoded, I split up my data set into 4 sets; train, validation, and test. From these sets, I ran my baseline which scored negatively with all of my features included. 

![Baseline1](./figures/baseline1_score.png)

After this model, I removed my model variable as it had way too much variance and was throwing off my models predictability and got a higher score on my linear regression model.

![Baseline2](./figures/baseline2_score.png)

I tried out other models such as Decision tree, K-nearest neighbours, and Random Forrest regressors. These models scored:

![Model Scores](./figures/model_scores.png)

Based off the above results, I decided to pursue 2 model learners: KNN, and random forrest.
Through the use of GridSearchCV hyper parameter optimization, I was able to decrease some of my random forrest models error. As of now, my highest scoring model is my Random Forrest regressor with a .88 R-squared. Here is a visual of 30 of it's predicted outputs.

![Forrest Predictions](./figures/forrest_predicted_output.png)

Even though this model scored high, certain cars tended to make it either predict way too high or way too low. That is because it made decisions based on the features to reach its price conclusion.

My optimized KNN regressor scored .7 R2 after gridsearch hyper parameter optimization which was better than the original:

![KNN scores](./figures/winner_results.png)


### Winning model selection

Here is a visualization of the KNN regressors predictions on 10 random cars that it has never seen from the test data set:

![KNN test predictions](./figures/winner_predictions.png)


The final model I selected was the KNN Regressor model. This model doesn't make decisions based on features like the random forrest. Instead, it decides based off groups(neighbors) which are defined by the means of the combination of features price.

KNN Regressors predictions on average cars as a whole was very good. When I ran multiple randomized predictions, I noticed that it did not get thrown off by certain sports car luxury model outliers as much as the other models. My data set is mostly comprised of mercedes benz cars that vary heavily in price based on model. Since model was not a feauture used in training, my other regressors struggled with this and often over priced certain cars with similar physical features to luxury model cars. For example, an A class mercedes can cost within the 10-15k range, whereas a C class can be 20k+. This variance makes my other models predict way too high on non luxury model cars and way to low on certain luxury model cars. K neighbors looks at the mean of the nearest cars and says it must belong to a certain group based on it's features as a whole causing it to fall closer within range of these highly varied cars.


To Improve my model, I would extract more features from other auto sale websites and add it to my model. Since my data set was webscraped, the cars acquired were completely random. I would also increase my sample size of cars providing a wide range of brands and models. This would likely help with some of the instability.

## Sources:

[AutoVillage](https://www.autovillage.co.uk/used-car) <br>
[Scikit learn](https://scikit-learn.org/stable/user_guide.html) <br>
[Nimble fins](https://www.nimblefins.co.uk/average-annual-mileage-cars-england-down-%E2%80%93-are-we-really-driving-less)