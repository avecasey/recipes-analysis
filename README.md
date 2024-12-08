# Whisking Through Data for Scrumptious Calorie Predictions
Authors: Souma Mitra (souma@umich.edu) and Abby VeCasey (avecasey@umich.edu)
## Introduction

The "Recipes and Ratings" dataset contains thousands of recipes from food.com, each row representing a review of a particular recipe. Each recipe can have multiple reviews, and therefore multiple rows in the dataset. There are 219393 rows and 16 columns in our cleaned dataset. We will use this dataset to answer the question of how accurately we can predict the number of calories in a recipe given other recipe details, and which factors best predict the number of calories, and to answer this we will use columns “name”, which is the recipe name, “id”, which is a numerical id given to identity each recipe, “minutes”, which is the number of minutes it takes to complete each recipe, “contributor_id”, which is the unique id of the person who submitted the recipe, “n_steps”, which is the number of steps it takes to complete the recipe, “n_ingredients”, which is the number of ingredients in the recipe, “average_rating”, which is the average rating given to the recipe, and “calories”, “total fat”, “sugar”, “sodium”, “protein”, “saturated fat”, and “carbohydrates”, which are the amounts of each of these in the recipe. By understanding this dataframe and answering this question, we can provide recipe writers with a better understanding of how many calories are in each of their recipes without having to calculate it.

---

## Data Cleaning and Exploratory Data Analysis

To begin cleaning the data, we merged the recipes and ratings datasets together, as the dataset suggested. We then changed all ratings of 0 to be np.nan, which makes sense because ratings usually only go from 1-5, so recipes with ratings of 0 indicate that there is no rating. We then added an “average_rating” column, which is the average rating per recipe. Then, we decided to split the “nutrition” column into separate columns for each nutrition statistic, for example “calories” and “total fat”. Doing this allowed us to use these new variables for our analysis, and to find new relationships between variables that we could not have previously investigated. Then, we had to convert each of these new columns from type “object” to type “float”, because these are numerical values, and we needed them to be represented this way to proceed with our analysis of their relationships with other variables. Doing this allowed us to be able to visualize these variables with charts like scatter plots and histograms. Finally, we dropped columns unnecessary for our analysis, including “recipe_id” because it was a duplicate of “id”, “date submitted”, “'steps”, “description”, “ingredients”, “tags”, and “review”. Below is a visual of the first 5 rows of our cleaned dataset.


| name                                 |     id |   minutes |   contributor_id |   n_steps |   n_ingredients |          user_id |   rating |   average_rating |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates |
|--------------------------------------|--------|-----------|------------------|-----------|-----------------|------------------|----------|------------------|------------|-------------|---------|----------|-----------|-----------------|-----------------|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 |        10 |               9 | 386585           |        4 |                4 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 |        12 |              11 | 424680           |        5 |                5 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |
| 412 broccoli casserole               | 306168 |        40 |            50969 |         6 |               9 |  29782           |        5 |                5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |
| 412 broccoli casserole               | 306168 |        40 |            50969 |         6 |               9 |      1.19628e+06 |        5 |                5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |
| 412 broccoli casserole               | 306168 |        40 |            50969 |         6 |               9 | 768828           |        5 |                5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |

Next, we began exploratory data analysis, creating a histogram of the distribution of calories for recipes with fewer than 1000 calories. From the graph below, we can see that the distribution of calories is skewed to the right, and most recipes in this range have between 150 and 300 calories.

<iframe
  src="assets/calories1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We then explored the relationship between calories and number of ingredients in the recipe, deciding to split recipes between those with less than 9 ingredients and those with 9 or more. From the boxplot below, we can see that the median number of calories for recipes with fewer than 9 ingredients is below the median number of calories for recipes in the other group, which implies that recipes with more ingredients tend to have more calories.

<iframe
  src="assets/calories_ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/calories_time1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This group by table shows us the average nutrition information, number of recipes, and average recipe information per recipe contributor, of the top 50 contributors. This information is important to understand the nature of recipies that the top 50 recipe authors post. 

|   total_recipes |   average_minutes |   average_steps |   average_n_ingredients |   average_rating |   average_calories |   average_total_fat |   average_sugar |   average_sodium |   average_protein |   average_saturated_fat |   average_carbohydrates |
|-----------------|-------------------|-----------------|-------------------------|------------------|--------------------|---------------------|-----------------|------------------|-------------------|-------------------------|-------------------------|
|            3060 |           59.2291 |        10.9562  |                 9.65065 |          4.7874  |            417.687 |             33.4444 |         74.702  |         28.9265  |           23.6765 |                 34.1755 |                14.802   |
|             701 |           95.4779 |        11.5977  |                 9.98146 |          4.65391 |            482.653 |             33.5264 |         63.3053 |         30.1954  |           40.4308 |                 46.1683 |                16.174   |
|             751 |           56.5499 |         9.27963 |                 8.78828 |          4.45075 |            420.367 |             31.3422 |         46.719  |         32.6724  |           43.9294 |                 38.7763 |                11.9148  |
|             868 |          126.018  |        11.2442  |                10.8203  |          4.7285  |            236.902 |             14.826  |         52.0991 |         13.7638  |           17.5956 |                 16.6233 |                 8.74078 |
|            1795 |           43.429  |         8.26797 |                 7.84345 |          4.78293 |            319.64  |             23.3939 |         39.7788 |         31.8396  |           31.1671 |                 27.9426 |                 9.11866 |
|            2436 |           97.3001 |         9.35591 |                 9.57348 |          4.78781 |            437.114 |             38.8612 |         63.5739 |         28.9606  |           33.9901 |                 42.2496 |                11.2278  |
|             802 |           35.793  |         5.92519 |                 8.94638 |          4.75231 |            357.314 |             25.101  |         54.3017 |         20.8254  |           29.9601 |                 27.7469 |                11.0249  |
|             699 |           73.4964 |         8.80114 |                 8.90701 |          4.77697 |            312.614 |             21.3419 |         54.7239 |         23.8913  |           21.2432 |                 25.0057 |                11.2947  |
|            1091 |           66.7525 |         7.50596 |                10.4088  |          4.84694 |            508.263 |             37.9982 |         98.5866 |         28.4876  |           30.7754 |                 57.2374 |                17.8863  |
|            1614 |           72.3234 |        10.6945  |                 9.0316  |          4.70194 |            376.489 |             29.9777 |         42.7187 |         25.4455  |           35.2534 |                 43.2007 |                10.2169  |
|            2310 |           60.4697 |         9.65758 |                 8.34545 |          4.82547 |            434.246 |             32.0208 |         93.1442 |         21.6277  |           25.8442 |                 40.4463 |                15.4874  |
|            1134 |           50.8986 |         6.85626 |                 7.34656 |          4.76256 |            277.613 |             23.94   |         29.4374 |         30.3598  |           17.7637 |                 18.7725 |                 8.13668 |
|             705 |           42.078  |         7.50355 |                 8.8766  |          4.82602 |            380.924 |             25.9589 |         79.2766 |         16.7745  |           22.2681 |                 34.6241 |                15.3901  |
|            1143 |           50.5459 |         9.78828 |                 9.31584 |          4.78699 |            344.828 |             29.0236 |         39.8215 |         43.6089  |           30.7095 |                 31.2441 |                 8.97725 |
|             995 |           43.6151 |         8.36683 |                 8.77588 |          4.74174 |            592.648 |             44.0814 |         58.2985 |         32.3206  |           55.4151 |                 54.0844 |                17.8553  |
|            2754 |          112.537  |        12.5425  |                10.8951  |          4.84972 |            559.235 |             48.9299 |         86.459  |         30.6612  |           41.8217 |                 64.8998 |                15.0298  |
|            1111 |         2078.74   |        13.0945  |                 8.75788 |          4.81395 |            518.957 |             36.8911 |         51.0513 |         27.9793  |           37.2529 |                 45.541  |                17.6562  |
|             994 |           52.5835 |         9.93662 |                10.1217  |          4.83196 |            441.355 |             34.994  |         60.8219 |         26.4799  |           37.7223 |                 38.2887 |                13.3863  |
|             725 |           50.8193 |         9.79586 |                 9.52    |          4.76371 |            267.761 |             17.7531 |         26.3697 |         20.9628  |           39.2566 |                 19.1628 |                 6.22207 |
|             788 |           75.302  |         9.28553 |                 8.87944 |          4.50738 |            380.978 |             26.0279 |         58.5431 |         29.8668  |           32.1701 |                 25.8553 |                13.1713  |
|            1193 |           44.7385 |         6.58592 |                 8.69573 |          4.78718 |            381.246 |             26.2506 |         46.1928 |         19.6471  |           39.0444 |                 31.3546 |                12.0075  |
|            1035 |           71.1208 |        10.4126  |                 8.29565 |          4.77345 |            431.053 |             32.5556 |         50.57   |         28.7382  |           40.1411 |                 42.3275 |                12.2464  |
|             741 |           74.2011 |        11.4062  |                 9.88529 |          4.76122 |            554.227 |             42.0553 |        127.615  |         27.9433  |           37.6329 |                 61.0189 |                19.0958  |
|             758 |           63.1636 |        12.2032  |                 9.6372  |          4.61769 |            483.157 |             29.3087 |         81.9472 |         39.0989  |           42.8391 |                 38.1253 |                18.6095  |
|             891 |          100.516  |         7.16611 |                 7.51515 |          4.60748 |            383.474 |             29.2963 |         39.4714 |         31.7127  |           39.1762 |                 38.5679 |                10.523   |
|            2368 |           41.0249 |         8.86318 |                 8.64105 |          4.76776 |            359.764 |             26.5933 |         54.7382 |         18.5988  |           28.5722 |                 33.0325 |                10.9848  |
|            1553 |           79.0547 |        11.5325  |                 9.566   |          4.76684 |            562.079 |             44.3786 |         77.2968 |         38.0953  |           41.49   |                 52      |                17.5274  |
|             849 |           82.6031 |         9.42167 |                 9.9788  |          4.68005 |            470.633 |             39.8657 |         61.9847 |         39.4782  |           40.3498 |                 52.4523 |                12.6207  |
|            2503 |           33.6336 |         6.0004  |                 7.82621 |          4.80303 |            263.465 |             15.8326 |         74.1454 |         10.2089  |           15.1898 |                 20.3947 |                11.4914  |
|            1016 |           57.3465 |        11.9409  |                 9.20374 |          4.75181 |            440.324 |             30.2913 |        113.75   |         36.315   |           31.5974 |                 43.6722 |                16.4429  |
|            1680 |           37.5167 |        10.3125  |                 8.60417 |          4.85226 |            388.325 |             33.2821 |         50.0351 |         27.272   |           30.0405 |                 45.4149 |                 9.99107 |
|            1588 |           52.0082 |         7.5699  |                 7.33312 |          4.70259 |            355.508 |             29.7469 |         47.1159 |         17.8778  |           34.8268 |                 33.665  |                 8.58501 |
|             969 |           53.7162 |        12.2425  |                 9.95666 |          4.8307  |            488.985 |             38.0175 |         43.6264 |         37.1238  |           46.1868 |                 44.3849 |                13.4974  |
|             863 |           60.5261 |        10.6477  |                 9.73812 |          4.73609 |            382.113 |             30.4936 |         44.5017 |         20.4137  |           31.219  |                 34.9143 |                11.102   |
|            1060 |           36.9934 |         8.65    |                 8.03774 |          4.73159 |            394.951 |             28.9689 |         60.4943 |         23.084   |           30.5547 |                 36.2594 |                12.5651  |
|             872 |           95.9771 |         8.07913 |                 6.93693 |          4.8392  |            320.869 |             25.82   |         53.5665 |         13.0069  |           20.9622 |                 28.4851 |                 7.88188 |
|            1572 |           32.9275 |         7.97519 |                 7.83842 |          4.77626 |            327.606 |             21.9364 |         63.4663 |         18.6202  |           20.9835 |                 24.5344 |                12.9148  |
|             905 |           62.1293 |         9.64862 |                 8.3768  |          4.76712 |            368.702 |             28.2575 |         54.0398 |         27.2608  |           32.3536 |                 35.2287 |                10.8729  |
|             863 |           33.3453 |         8.6686  |                 8.63963 |          4.75957 |            433.118 |             27.9571 |         54.9733 |         14.5203  |           24.3708 |                 34.6107 |                18.3882  |
|             743 |           78.0754 |         9.1319  |                 8.16555 |          4.79184 |            350.871 |             24.5976 |         67.179  |          9.66622 |           21.6393 |                 21.852  |                13.5222  |
|             904 |           70.6925 |         8.68584 |                 8.02655 |          4.74286 |            458.836 |             30.4524 |         50.8827 |         23.7113  |           33.5199 |                 39.5221 |                17.0918  |
|            1711 |          135.907  |         6.80538 |                 8.9287  |          4.7199  |            452.228 |             34.6803 |         61.6283 |         36.0158  |           44.8328 |                 43.5786 |                12.7551  |
|            1236 |           55.962  |         8.04126 |                 8.48301 |          4.76789 |            275.127 |             18.2856 |         55.5129 |         13.5049  |           19.2087 |                 23.6545 |                10.4256  |
|            1019 |           55.4622 |        10.841   |                 9.08342 |          4.71728 |            283.828 |             20.2777 |         31.0913 |         18.8744  |           27.577  |                 19.7733 |                 8.34249 |
|             762 |           32.5853 |         7.79265 |                 7.75853 |          4.7038  |            256.524 |             17.4528 |         40.6732 |         13.9081  |           22.6745 |                 21.2743 |                 8.51706 |
|             759 |           48.8498 |         9.64032 |                 9.82082 |          4.67647 |            339.621 |             27.5415 |         25.361  |         20.3953  |           36.0487 |                 32.5652 |                 8.01318 |
|             876 |           55.0719 |         8.9726  |                 9.65183 |          4.7623  |            289.536 |             19.7203 |         41.5925 |         19.7637  |           29.6929 |                 26.1313 |                 8.71689 |
|             805 |           55.0422 |         8.38509 |                 9.07205 |          4.78834 |            332.084 |             27.2099 |         29.636  |         45.8373  |           27.4646 |                 32.1615 |                 8.82484 |
|            1867 |           84.7381 |        11.045   |                 9.74879 |          4.81743 |            463.962 |             35.5865 |         76.7017 |         42.8956  |           35.4253 |                 44.4124 |                14.4649  |
|             738 |           32.393  |        10.4187  |                 8.59485 |          4.77496 |            445.8   |             39.3008 |         59.1369 |         20.1125  |           32.5583 |                 53.8482 |                12.1084  |

---

## Framing a Prediction Problem
We will be predicting the number of calories a recipe has based on the nutrition information. This is a regression problem. This is useful because it can allow for recipe contributors to estimate the calories in a recipe based on the nutrition information, and see which factors contribute most significantly to the number of calories. To evaluate the performance of this model, we will be using MSE (Mean Squared Error). 
---

## Baseline Model
For this model, we will be conducting a linear regression of the number of calories on the total fat, sugar, sodium, protein, saturated fat, and carbohydrates, all of which are quantitative variables. We also chose to standardize each of the features, to allow us to compare the effects and understand which was the most important in predicting calories. The MSE of this model was 1471.969.
---

## Final Model
To improve upon our model, we decided to add the features described above, and use a Lasso regression to help with variable selection. We decided it was important to use the variable 300_mins_or_less, because we saw a difference in distribution of calories based on this distinction. Similarly, we added the variable less_than_9_ingredients because we saw a difference in calorie distribution between recipes with less than 9 ingredients and recipes with 9 or more ingredients. We tuned our hyperparameters with cross-validation. The MSE of this model was 1469.696.
