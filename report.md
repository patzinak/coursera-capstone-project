# Predicting Single-Day Ski Resort Lift Ticket Prices

## 1. Introduction

### 1.1. Background

Downhill skiing and snowboarding are fun recreational activities that have been associated with health benefits
[[1]]. Recently, the ski lift ticket prices have been steadily rising [[2]], while the industry might have been
facing difficult times dealing with its own challenges [[3]].

As a window price for a single-day ski lift ticket for one person can exceed $200 at the top resorts,
it is often the most important factor to reckon with when a skier or snowboarder wants to budget a one day trip to
a mountain. Even when it comes to a multi-day vacation to a remote resort, the lift ticket expenses can be
still easily comparable to the total cost of lodging and transportation cost of getting to and from the resort.
Given the fact that for the most people such prices could not be viewed as nuance expenses,
it is naturally to expect that the ski lift price must hold to the value and quality of the experience
that the resort promise to deliver.

A one-day, adult ski lift ticket is not the only option that a resort provides these days.
There are half-day tickets, tickets for kids, beginner packages, lift and rentals packages, etc. Most importantly,
there is typically an option to buy a season pass. The pass options also vary significantly in their prices
and terms. Indeed, there seem to be some evidence that resorts are pushing for their season pass sales while
making the single-day lift tickets prohibitively expensive [[2]]. Even if this is the case, and the single-day lift
tickets are priced above their market values, it is still reasonable to assume that the single-day ticket price reflects
the quality of the resort. This could be, for example, because a holder of ski pass is expected to spend several days
at the company resorts and, thus, spend more on other snow activities (cross-country skiing, snow-tubing, snowshoeing,
sled riding, etc.), as well as on dinning, nightlife, and lodging. If so, the resorts that have better off-mountain
infrastructure can provide more appealing season pass options, and this, in its turn, can justify the further increase
of a single-day ticket price. Whatever is the marketing strategy of a particular resort, a single-day lift ticket
price is an important metric to compare the resorts, and it is the figure that many customers pay attention to.

When it comes to the ticket price, it is natural to assume that the resort mountain infrastructure,
such as the number of ski lifts, and the quality of mountain terrain,
such as the vertical drop and the number of trails, should be priced-in into the lift tickets. However, it is less obvious
whether the off-mountain infrastructure, such a number of restaurants, bars, and hotels, affect the ticket prices.

### 1.2. Business problem

This project aims to predict a ski resort single-day ski ticket price based on the available
information about the resort. The main question that will be addressed here is whether the resort
off-mountain infrastructure, i.e. the number of restaurants, bars, and hotels, could improve the prediction
outcomes and could be, therefore, considered to be an important factor of the perceived ski resort value.

### 1.3. Interest

Ski resorts might be interested in knowing the expected market value for the lift ticket
at their resorts, to achieve any competitive advantage. This is especially relevant as the ski prices
changes at least on an annual basis. The ability to reliably evaluate market prices is important for dynamic pricing.
If any influence of the off-mountain infrastructure on the lift ticket prices is found, the resorts may consider
investing heavily into new venue development to improve the perceived resort value
and justify further increase of the ticket prices.

Skiers and snowboarders might be interested in knowing
how justified are the ticket prices at a particular resort in order, for example, to choose a resort
that have great mountain slopes terrain but does not necessary provide a lot of dinning options.

## 2. Data acquisition and cleaning

### 2.1 Data sources
The following datasets are used in building and evaluating the prediction models.

* [www.skiresort.info][4] contains an extensive list of the U.S. ski resorts with the basic information
for each resort such as the vertical drop,
the base and peak elevations above the sea level,
the total length of trail runs and their relative difficulty levels,
number of ski lifts,
as well as the lift ticket price. This list was web-scraped and used as a starting point dataset.

* [www.onthesnow.com][5] contains another table on single-day prices at North America ski resorts
and was web-scraped to improve the dataset price relevance.

* [wikipedia.org][6] has a ski resort comparison table. This table was also web-scraped to improve the dataset quality.

* [Google Search][7] was used to automatically determine the longitude and latitude of any particular ski resort.

* [Foursquare Places API][8] was used to retrieve
the number of nearby shops, cafes, restaurants, bars, and hotels within a 5 km radius from a given resort location.
The number of nearby venues was used as a proxy measure of the resort off-mountain infrastructure attractivness.

### 2.2 Data cleaning

The [www.skiresort.info][4] data formed the initial ski resort table for this project as it is the largest dataset out of the listed above. All duplicate entries in the table were removed. There were some missing price values. The price information and the statistics on the number of ski lifts was improved by retriving the values from the other datasets. Special care was taken to match the names of the resorts in different datasets as there is some variance. This was achieved by dropping less-informative words such as *Ski*, *Resort*, *Mountain*, etc. to obtain the shortest form of the resort name. The shorten names were checked for their uniqueness.
In the process of combining the price information, the highest number was kept because this should be a reasonable assumption in determining the most recent price. 33 resorts that still had missing price values were dropped from the dataset as this subset constituted less than 10% of the entire dataset and almost all of those resorts are very small and have only one or two ski lifts. 

After acquiring the location information for all resorts, the number of nearby venues within a 5 km radius from the resort locations were added to the table using [Foursquare Places API][8]. The list of venues consisted of shops, cafes, restaurants, bars, and hotels. Shops and cafes were included for the test purposes as they are not typically associated with the ski resort activities. 

### 2.3 Feature selection

After data cleaning, there were 383 entries with price information and 13 additional numerical features in the dataset. Upon examining the meaning of each feature, it was clear that there was some redundancy in the features. For example, *Vertical Drop* is the difference between *Peak Elevation* and *Base Elevation*. Similarly, *Total Trails* is the sum of *Green Trails*, *Blue Trails*, and *Black Trails*. *Restaurants*, *Cafes*, and *Shops* have correlations above 0.90, indicating another potential redundancy.

![Correlation Matrix][correlations]

## 3. Methodology

### 3.1 Models

Four models that are different in the subsets of features were tested.

* Model *All Stats*. This model included the initial numerical features without any venue information. The performance of this model provided the baseline against which the other models can be compared to.

* Model *Selected Stats*. *Peak Elevation* and *Total Trails* were removed from this model to illustrate the fact that removing of the redundant features does not change the model performance.

* Model *Selected Stats & All Venues*. Here, all venue features were added in addition to the features used in model *Selected Stats*.

* Model *Selected Stats & Venues*. Here, only information on the number of restaurants and hotels was added to the *Selected Stats* model. This is a relatively simple model, with all obvious redundancies removed. The hope was that the model performance would not suffer from the redundant and highly-correlated feature removal.

In this project, linear regression models using these feature sets were build and characterized.

### 3.2 Exploratory data analysis

The success of the linear regression models depends on the amount of correlation between the predicting values and the features.

The scatter plot below explores the correlation between the ski lift ticket price and *Peak Elevation*, *Base Elevation*, and *Vertical Drop*.

![Elevations and Vertical Drop vs Price Scatter Plot][scatter_elevations]

While the correlation between the resort vertical drop and the ski lift ticket price is expected (the rides typically prefer resorts with longer trails), there seem to be some correlations between the price and the resort elevation above the sea level. It might be still meaningful to keep one of these absolute evaluation features as a hypothetical proxy for the amount of average snowfall or the length of the ski season.

The scatter plot below explores the correlation between the ski lift ticket price and the number of ski lifts.

![Number of Ski Lifts vs Price Scatter Plot][scatter_ski_lifts]

Clearly and not surprisingly, there is a significant amount of correlation, and this is one of the most important features for any predictive model.

The scatter plot below explores the correlation between the ski lift ticket price and the accumulated trail lengths at different difficulty levels.

![Trail Lengths vs Price Scatter Plot][scatter_trails]

Clearly, there is notable correlation between the accumulated trail lengths, and it is meaningful to keep these features.

The scatter plot below explores the correlation between the ski lift ticket price and the number of nearby venues of different types.

![Number of Nearby Venues vs Price Scatter Plot][scatter_venues]

The correlations between the number of venues and the lift ticket price are less obvious. However, upon a close inspection, one can notice some correlations between the price and number of hotels and restaurants.

### 3.3 Model evaluation

The performance of all models was evaluated by computing Residual Mean Squared (RMS) and R^2 scores using *k*-fold splitting of the initial dataset into training and testing subsets, and running over 100 splitting realizations to get the accurate estimations. 

![RMS Score Plot][rms_score]
<img src="https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/rms_table.png" alt="RMS Score Table" width="625">

It is clear that RMS score is reduced for the models that include venue features.

![R^2 Score Plot][r2_score]
<img src="https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/r2_table.png" alt="R^2 Score Table" width="625">

It is clear that R^2 improves when the venue information is included from about 0.71 to 0.73. While the improvement might not be that impressive it is obvious that this improvement is still larger than  any difference in the R^2 score between two models that do not include any venue information, and between the other two models that are different in selected venue features.

Thus, inspection of the RMS and R^2 scores confirms that removing redundant or highly-correlated features do not significantly reduce the model performance, while adding some venue information can improve it. The performance scores favors model *Selected Stats & Venues* as it provides the best performance with a relatively limited set of features.

The distribution plots of the actual and predicted prices give further insights into the performance of the best model.

![Performance on Training Dataset][train]
![Performance on Test Dataset][test]

We can see that there are price regions, especially on the cheaper side, that can benefit from further model improvements.

## 4. Results

From the model performance evaluations, it is clear that including the nearby venue data can improve the model performance.

For further insights into the best performing model, we can look into the model linear regression coefficients 

![Model Coefficients][coefficients]

The meaning behind these coefficients is the added price value associated with each unit of the specific feature. We can see that not all features contribute equally, even when they share the same measurement unit. More importantly, it appears that the number of lifts is one of the most important price predictors. The accumulated lengths of green trails has the most significant contribution to the price compared to the trail lengths at the other difficulty levels. And finally, the resorts that have more lodging options near the resort tend to be more expensive. The number of dinning options may also drive the expected ticket price but the effect is less significant when it is compared to the contribution from the lodging options.

## 5. Discussion

The analysis above shows that it is possible to build relatively successful lift ticket price prediction models using the linear regression approach. The performance of these models can be improved by including the information about the number of venues nearby, more specifically, by including the number of hotels and restaurants within a certain radius. This result suggests that investing into the off-mountain infrastructure is a meaningful option for increasing the perceived value of the resort experience and, thus, the value of a lift ticket.

While increasing the vertical drop of an existing resort might be a totally unrealistic feat, the resorts that have limited number of ski lifts or green trails might still bear a good potential for growth by improving on-mountain infrastructure. The resorts with the well-developed mountain terrain may consider investing into the local infrastructure by increasing the number of lodging and dinning options.

## 6. Conclusion

We have seen that including the number of nearby restaurants and, especially, the number of hotels near ski resorts improves the linear regression model performance. The improvement is relatively small but the effect is robust: off-mountain infrastructure clearly is priced-in into the ski lift tickets. Not all on- and off-mountain factors contribute equally to the price of the ticket. The regression models can provide clear directions for the resort infrastructure development.

## 7. Future directions

The regression model presented here can be improved further. Obviously, it might be beneficial to do a more detailed analysis of the venue types, filter the venue dataset to avoid double-counting, and remove any other inconsistencies. The 5 km radius used in this study could be optimized for better performance. Exploring residuals may reveal some important non-linear behavior that potentially can be included into the model. The ski resorts are also different in their natural aspects such as the correspondng length of the ski season. Thus, one may want to include other statistical parameters such as the average annual snowfall, or the average winter temperature. The ski resorts are spread across the states with significantly different costs of living. The cost of living difference could be build-in directily into the model. Many ski resorts are owned by the umbrella companies that own other resorts [9]. The owner companies might adhere to different policies when they determine their resort ticket prices. Thus, it would be interesting to see how 
one hot encoding of the resort owners can improve the prediction outcomes. Finally, clustering techniques applied to the resort datasets might reveal extra insights into the common factors that influence the lift ticket prices.


## References
1. [Why Skiing Is a Ridiculously Good Workout][1]
2. [How Did Lift Tickets Become So Expensive?][2]
3. [Is the ski industry on a slippery slope?][3]
4. [Ski resorts in the United States of America][4]
5. [North America Resort Lift Tickets - Most Current Prices][5]
6. [Comparison of North American ski resorts][6]
7. [Google Search][7]
8. [Foursquare Places API][8]
9. [Who Owns Which Mountain Resorts][9]

[1]: http://time.com/5118770/is-skiing-a-good-workout/
[2]: https://snowboarding.transworld.net/photos/when-did-ski-resort-lift-tickets-become-so-expensive/
[3]: https://www.bbc.com/news/business-42110566
[4]: https://www.skiresort.info/ski-resorts/usa
[5]: https://www.onthesnow.com/north-america/lift-tickets.html
[6]: https://en.wikipedia.org/wiki/Comparison_of_North_American_ski_resorts
[7]: https://www.google.com
[8]: https://developer.foursquare.com/
[9]: http://www.nsaa.org/press/industry-stats/industry-stats-pages/who-owns-which-mountain-resorts/


[correlations]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/correlations2.png "Correlation Matrix"
[scatter_elevations]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/scatter_elevations.png "Elevations and Vertical Drop vs Price Scatter Plot"
[scatter_ski_lifts]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/scatter_ski_lifts.png "Number of Ski Lifts vs Price Scatter Plot"
[scatter_trails]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/scatter_trails.png "Trail Lengths vs Price Scatter Plot"
[scatter_venues]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/scatter_venues.png "Number of Nearby Venues vs Price Scatter Plot"
[rms_score]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/rms_score.png "RMS Score Plot"
[r2_score]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/r2_score.png "R^2 Score Plot"
[train]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/train.png "Performance on Training Dataset"
[test]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/test.png "Performance on Test Dataset"
[coefficients]: https://github.com/patzinak/coursera-capstone-project/blob/master/report_figures/coefficients.png "Model Coefficients"
