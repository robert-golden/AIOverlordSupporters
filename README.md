# Project Overview

This report contains Deloitte’s recommendations regarding Computing Vision’s new movie studio. We processed and analyzed the provided datasets to understand the role of runtime, genre, and MPAA rating in relation to box office sales. We recommend that Computing Vision focus on creating movies roughly 105 to 140 minutes in length, in the action genre, and with a MPAA rating of PG or PG-13.


# Instructions for Navigating Repository 

This file contains the links to the necessary information for understanding the project and our data. It also contains the broad business overview, a break down of our analysis, and the business recommendations we have for Computing Vision. 

The raw datasets can be found in the [zippedData Folder](https://github.com/jumholtz/AIOverlordSupporters/tree/main/zippedData)

Link to the [dataset ERD](https://github.com/jumholtz/AIOverlordSupporters/blob/main/movie_data_erd.jpeg)

Link to Jupyter
[Notebook](https://github.com/jumholtz/AIOverlordSupporters/blob/main/Notebook.ipynb)

Link to Jupyter Notebook as a [PDF](https://github.com/jumholtz/AIOverlordSupporters/blob/main/Jupyter%20Notebook%20PDF.pdf)

Link to PowerPoint [Presentation](https://github.com/jumholtz/AIOverlordSupporters/blob/main/Capstone%20Presentation.pdf)


# Business Understanding

Computing Vision is a relative newcomer to the film production market, having little to no experience producing movies; for this reason, Computing Vision selected Deloitte to advise them on the creation of an initial, data-driven film production strategy. 

As Computing Vision is forming a new movie studio, we have three general recommendations about operating in the film industry that relate to budgets, revenue, and marketing. First, Computing Vision should allocate generous budgets for their film projects. Failure to appropriately budget could expose film projects to risk; underfunded projects will encourage staff to cut corners, sacrificing film quality to come in on or under budget. We recommend a “quality over quantity” approach, where few films are in development, but are well-funded. Related to a movie’s budget is its revenue, which is critical to the success of Computing Vision’s studio. Second, revenue can be split into several categories, first dealing with time spent in cinemas, and second dealing with global markets. While it is true that movies make money twice – when they are first released in cinemas and again when released for home theatres – we recommend that Computing Vision focus on the initial ticket sales at the box office. We likewise recommend that Computing Vision focus on the domestic film market, as crafting films that appeal to a global audience could be somewhat ambitious for a new studio. While a generous budget will help ensure that film quality is high enough to achieve satisfactory ticket sales, we must also account for how Computing Vision will market their movies. Third, the goal of marketing is to generate excitement, create a good first impression, and motivate viewers to see the movie in theaters. Computing Vision’s marketing strategy should consist of both traditional approaches such as interviews with entertainment and advertisements, as well as exploiting informal communication networks such as word-of-mouth and other grassroots campaigns. By exploiting informal communication networks, such as grassroots social media campaigns (i.e., hashtag campaigns, memes, etc.) and engaging with fan communities online or in-person, these less-expensive advertising techniques can take advantage of social contagion to spread awareness of upcoming films. 

Therefore, with these three factors of business, we generated three questions to seek answers to in the data provided. Deloitte seeks to answer the following three questions: 1. What genre of movies should Computing Vision produce? 2. How long should Computing Vision’s movies be? 3. What age demographic, in terms of MPAA rating, should be targeted? First, we seek to identify genres that will achieve the greatest average or median box office revenue for Computing Vision. Second, we seek to identify whether shorter or longer movies correlate with higher box office revenue. Third, we seek to identify whether Computing Vision will achieve higher box office revenue by appealing to a more mature audience or by appealing to a broader age range.

# Data Understanding and Analysis

Computing Vision provided Deloitte with five spreadsheets and a database to process, analyze and from which to extract our insights. We initially standardized the spreadsheets to consist of the comma-separated values (.csv) filetype, to ensure that the spreadsheets’ data can be extracted using similar methods in Python. From there, we focused our analysis on the Rotten Tomatoes Movie Information dataset, known as the rt_movie_info spreadsheet as it contained the necessary box office, genre, and movie length information.

Genre To address the first question, we began by using the rt_movie_info.tsv. We converted the .tsv to a .csv. Utilizing the Pandas read_csv method, we created an initial data frame. We extracted two columns, genre and box office, and created a new data frame with these columns. For certain movies, the box office sales were not recorded on the spreadsheet. We proceeded to drop all rows without a value in the box office column. We proceeded by sorting the data to find the most common genres. We sorted the data frame by genre and displayed the results from most movies within a genre to least. Some of the genres were paired with other genres. After examining the genres, both on their own and in combination with others, the most common genres include action, horror, romance, comedy, and drama. We then found the average box office sales for each genre. To best understand these findings, we plotted the average revenue for each genre on a bar graph. We found from this data that on average action movies had the greatest revenue. A one-tailed T-test was performed using the stats.ttest method to decide whether the null hypothesis should be rejected. The test was set up to find out if the sample mean for action movie box office revenue (“act”) was statistically significantly higher than that of the population mean (“pop”).

Runtime & Box Office Sales We investigated the second question using the data in rt.movie_info.tsv, one of the files that was converted to a .csv file in Excel for ease of access. Then, we used the Pandas read_csv method to extract the data from the file, which we stored as the rt_movie_info dataframe. We recognized from a cursory investigation that the data in the spreadsheet had several issues that we could address in the data cleaning process. First was that there were numerous rows that featured null values, particularly in the “runtime” and “box_office” columns. This would significantly complicate the reliability of our sample if they were retained. We removed all rows with at least one null value using the Pandas method .dropna(). We were left with 235 rows in our sample, which had a complete set of information. Second, we extracted the unprocessed data of “runtime” into a list called runtime_raw. Upon inspection of this list, we recognized that entries in the “runtime” column had the word “minutes” after each runtime. Python would recognize this as a string, and we had to remove these characters; we also recognized that there were no movies that were over 999 minutes. As a result, we created a for-loop to iterate through each “runtime” value and used the .str[:3] method to strip the first three characters from each row of the “runtime” column. For movies with runtimes under 100 minutes, we then used another for-loop with the .replace() method to remove the whitespace and not replace it with a character. This cleaned data was appended to a list called runtime_clean. Third, we had to complete a similar operation in the “box_office” column as in the “runtime” column. The box office sales data included commas which would likely be interpreted as strings in Python; we created a similar list called box_office_raw to extract the values from the “box_office” column. We cleaned this data using a for-loop, using the .replace() method to remove commas from each value and replace the comma with nothing. The cleaned data were appended to the box_office_clean list. Fourth, we created two new columns for the rt_movie_info dataframe consisting of our runtime_clean and box_office_clean lists. We then initialized a new data frame called df2 using only the “runtime_clean” and “box_office_clean” columns from the rt_movie_info data frame by using the .copy() method in Pandas.

MPAA Rating To retrieve the data for MPAA rating, the column “rating” was called. This column contained information on whether the film was rated R, PG-13, PG, G, NC-17, or NR (not rated). Each rating had its own variable created (e.g., R would be “R”) and when called, each variable would return only the data for films with the corresponding rating. To calculate means and medians for the eventual box office revenue, even more variables were named. Each rating has a variable for average box office revenue (“r” if rating was R), and each had a variable for median (“r_med” if rating was R) box office revenue. These ratings were positioned against the box office revenue (denoted by the column “box_office”) and then a matplot graph was used to visualize the results.

# Data Visualizations

Runtime

Scatter plot of the relationship between runtime and Box Office Revenue

![Runtime_Visualization](https://raw.githubusercontent.com/jumholtz/AIOverlordSupporters/main/zippedData/Images%20of%20Visualizations/RuntimeSalesLog.png)

Genre

Top Genres by Box Office Revenue

![Top_Genres](https://github.com/jumholtz/AIOverlordSupporters/blob/main/zippedData/Images%20of%20Visualizations/Genre%20Revenue.png)

Median Box Office Revenue of the top Genres

![Median_Genre](https://github.com/jumholtz/AIOverlordSupporters/blob/main/zippedData/Images%20of%20Visualizations/Median%20values%20of%20Genre%20Revenue.PNG)

MPAA Rating

Average Box Office Revenue of MPAA Ratings

![Average_MPAA](https://github.com/jumholtz/AIOverlordSupporters/blob/main/zippedData/Images%20of%20Visualizations/Average%20Revenue%20by%20MPAA%20rating.png)

Median Box Office Revenue of MPAA Ratings

![Median_MPAA](https://github.com/jumholtz/AIOverlordSupporters/blob/main/zippedData/Images%20of%20Visualizations/Median%20Revenue%20by%20MPAA%20rating.png)



# Statistical Communication

Genre We calculated both the median and mean for all five genres. The means ranged from $27,433,818.52 to $76,067,853.49. Horror had the lowest revenue, and action had the highest revenue. We then found the median for all five genres. This method is less affected by outliers. We found in this data that the median ranged from $10,279,192 to $43,800,000. For median, drama had the lowest median revenue and romance had the highest median revenue.

Runtime & Box Office Sales We generated descriptive statistics on the relationship between runtime, or the length of a movie in minutes, and box office sales in US Dollars. During this time, we determined the maximum values, the 75th percentile, median, mean, 25th percentile, minimum values, interquartile range, standard deviation, and constructed a confidence interval for both runtime and box office sales. Our descriptive statistics for runtime revealed several insights regarding the typical length of movies. Movie runtimes range from slightly over one hour at 67 minutes to over three hours at 188 minutes. Our median runtime is 105 minutes versus a mean runtime of slightly over 106 minutes, which indicates that whatever outliers are present in the runtime column are not having an extreme effect on the measure of central tendency. We selected an alpha of 0.05, so we constructed a 95% confidence interval. Based off the confidence interval, we believe that the true population mean lay between roughly 70.37 and 142.96 minutes. This confidence interval is a key part of what drives our recommendations regarding the ideal range of movie runtimes. Next, we calculated descriptive statistics for box office sales. Descriptive Statistics for Runtime Mean runtime: 106.66382978723404

Maximum runtime: 188 75th Percentile: 117.0 Median runtime: 105.0 25th Percentile: 93.0 Minimum runtime: 67 Interquartile Range: 24.0 Std. Dev. of runtime: 18.147124581299227 95% of observations should lay between: 70.36958062463557 - 142.9580789498325

Relative to runtime, the box office sales data feature greater spread and more extreme values. The highest box office sales measured $368,000,000, while the lowest sales were $363. While the median sales were $15,536,310.00, the mean was calculated as $41,958,400.02; this indicates that outliers present in the data were having a noticeable effect on the measure of central tendency. The standard deviation and confidence interval are of questionable utility for this dataset, as the outliers appear to have drawn the standard deviation so high that our 95% confidence interval suggests that the true population mean for box office sales ranges between a loss of roughly $83 million and a profit of $139 billion dollars. With a basic understanding of the runtime and box office sales, we should next turn our attention to examining the relationship between the two datasets. Descriptive Statistics for Box Office Sales
Mean box office sales: 41958400.02127659

Maximum sales: 368000000 75th Percentile: 52649522.5 Median sales: 15536310.0 25th Percentile: 2302444.5 Minimum sales: 363 Std. Dev of Sales: 62630155.51836797 Interquartile Range: 50347078.0 95% of observations should lay between: -83301911.01545934 - 139206163961.8349

In order to better understand the relationship between runtime and box office sales, we calculated the correlation coefficient. The correlation coefficient for runtime and box office sales is 0.312157, which indicates that there is a moderate-strength, positive correlation between the two variables. That is, movies with a longer runtime are typically associated with greater sales at the box office. While the correlation coefficient does not have the same predictive power as a regression analysis, the confidence interval generated for runtime can be used as a set of guidelines to avoid diminishing returns on investment.

Correlation Coefficient runtime_clean box_office_clean runtime_clean 1.000000 0.312157 box_office_clean 0.312157 1.000000

# Conclusion

Through the data analysis, we found the following three recommendations to maximize potential revenue for Computing Visions. In terms of genre, we would advise Computing Visions to create a film within the action genre. Action had the greatest revenue on average, and when we analyzed the medians, action also had one of the most box office sales. Next, we recommend that the runtime for the film does not exceed 140 minutes. We would also advise that the film is not shorter than seventy minutes. The optimal length is between 105 minutes and 140 minutes. Our final recommendation is to create a film within the MPAA rating of PG-13. Although PG films tied PG-13, when we found the median, PG-13 performs on average better than any other MPAA rating in terms of revenue. One limitation of the data is that it only included revenue. Once Computing Visions begins producing films, we could analyze the profit by comparing the budgets for each film to the revenue to further determine how to increase Computing Vision’s profits.