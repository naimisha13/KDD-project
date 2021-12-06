# Team members:
- Nidhi Srinath (nsrinat1@uncc.edu)
- Kautilya Kondragunta (kkondrag@uncc.edu)
- Naimisha Churi (nchuri@uncc.edu)

# Data Sources:
- World Bank (https://data.worldbank.org)

# **Relational Analysis of Country-Wise Economic Growth and Academic Qualification levels**


Education is the act of acquiring and teaching knowledge via various channels.  It is the most crucial pillar of growth and development that supports an individual's advancement. It also serves to unite various views and opinions, as well as connect people from various backgrounds. Increased knowledge leads to more opportunities. Education also facilitates communication among a group of individuals, resulting in the sharing of ideas and implementations. When an individual's educational proficiency increases, the cognitive capacity of the individual also increases. As vital as education is for individuals, it also plays a significant role in the growth of a country. Education is a critical investment for every country. A country's progress can be accelerated by increasing the number of educated and competent individuals. An individual with the capacity to analyze information using logic and critical thinking may positively contribute to increased productivity and efficiency in any work in which they are involved. The amount of job prospects grows as individuals get more educated. Higher education rates are largely responsible for the rise in human capital. Education contributes to a nation's socioeconomic well-being. We hope to learn how much of a correlation exists between a country's average education levels and its GDP through this study. We also seek to provide a feeling of comparison between a country's educational levels throughout the last decades. We try to determine the amount of growth in a country's GDP by analyzing the percentage of residents who are educated at various levels such as elementary, secondary, tertiary, bachelors, graduate, and doctorate.


# Data Description
The data used in this project has been acquired from World Bank's Open data [https://data.worldbank.org]. It has data on all kinds of development indices for every country, recorded each year. The following data was 

- `LowerSecondaryAttainment` - Educational attainment, at least completed upper secondary, population 25+, total (%) (cumulative)
- `UpperSecondaryAttainment`- Educational attainment, at least completed upper secondary, population 25+, total (%) (cumulative)
- `BachelorsAttainment`	- Educational attainment, Bachelors or equivalent, population 25+, total (%) (cumulative)
- `MastersAttainment	`- Educational attainment, Masters or equivalent, population 25+, total (%) (cumulative)
- `DoctoralAttainment ` - Educational attainment, Doctoral or equivalent, population 25+, total (%) (cumulative)
- `Population	`
- `GDP`- Gross Domestic Product ($.US) 
- `% _expenditure_gdp` - Portion of government expenditure on education (% of GDP).

# CRISP-DM Process
- Research Phase
- Data Understanding 
- Data Preparation 
- Data Exploration
- Modelling 
- Evaluation 

## Research Phase
Education is no doubt an important aspect that determines the future of the country. A country's economic growth is driven by new inventions and discoveries in various fields that yield higher revenue for the better-educated labor force and, in turn, for the country. This usually has a positive correlation with the country's Gross Domestic Product (GDP). A few recent studies that looked at the effect of educational expenditure as a proxy for education reported positive growth in education with an increase in expenditure. 
> A recent meta-analysis considered 29 papers that specifically looked at the impact of government education expenditures on economic growth. Of these 29 studies, 14 reported a positive and statistically significant effect of government expenditure on growth, 12 reported a negative effect, and 3 reported no statistically significant effect.

Another study claims that an increase in government expenditure on education improved the completed years of education per pupil by `0.27` years, increased the mean wage levels by `7.25%` and led to a `3.67` percent reduction in the annual adult poverty levels. In this project, we try to break down a country's spending on education and its correlation with the educational attainments of a certain percentage of the adult population (25+).

## Data Understanding and EDA
The data used in the project is classified as follows:
- Attainments for each level of education are denoted with `attain_*.csv` in the `data` folder.
- GDP data for each country (`country_gdp_millions.csv`)
- Expeniture on education (% GDP) (`expenditure_total_gdp.csv`)
- Each dataset has the following columns 
  1. Country Name
  2. Country Code
  3. Indicator Name
  4. Indicator Code
  5. Indicator Values for Each year [1960-2019]

### Data Preparation
Right off the bat, the data we had in hand was very sparse with a lot of null values. We had to clean the data before we began to derive information from it. Since we had so much nullity, this project was mainly focused on the different methods we used to perform null value analysis and reduction.

- First, we had to convert the time series data which had each year as a column to one `Year` column containing all the years for each country.
- We converted the columns to their appropriate data types.
- We dropped all the country-year pairs which had no data for all columns we needed data on [Lower Secondary, Upper Secondary, Bachelors, Masters, Doctoral]. This still has left us with a few null values in a few features!
- While we tried to solve that issue, we also found out that a few countries had lesser years of data. Like for example - India has only data for 2011. This might be because of the large population in the country and the fact that the Census is once every 10 years. We filtered such countries out too.
- We're almost there! But, we still had a few countries which were missing a couple of years of attainment and GDP data. 
- We first considered the possibility of using imputation techniques like `ffill` and `bfill`- but, it wouldn't really make sense to use the previous or next values for a feature to fill the null values. 
- Then we considered `SimpleImputer` which also wouldn't be plausible in our case as mean, median or another simple imputing criterion wouldn't quite preserve the story.
- `IterativeImputer` considers the values of all the features in the data to impute missing values. But the problem is that the DoctoralAttainment range for most countries lies within the 10% range. Using IterativeImputer filled the null Doctoral values with really high values which don't preserve the story as well. We could use a workaround to set the lows and highs for each column, but we're looking for a better approach. Also, Iterative Imputer needs some data to figure out the relation between the other features and the column with null values and if we give an almost empty data frame as an input to it, we can see that it returns null values again.
- So, we turned towards Linear Regression to impute the missing values for those fields. Linear Regression trains a model on the known values for that country by using the previous and later years' values to predict the values for the null values for a certain year for that particular country. 
- Now, we have a fully filled dataset. As amazing as that sounds, we still had to do some feature engineering like deriving the expenditure on education in US Dollars from the %, renaming a few columns for standardization, etc.

![image](https://im3.ezgif.com/tmp/ezgif-3-d88a4669659b.gif)

### Machine Learning
We realized that what we had was a task for an Unsupervised machine learning algorithm - To find clusters in our data and extract useful information from them. We are using Hierarchical Agglomerative clustering (HAC).
- HAC provides an insight into the similarity level between any two data points. It uses a bottom-up approach, where each country acts as a singleton cluster and works its way up by merging with the next "least distant" data point. This way it merges until all the small clusters merge into one big cluster encompassing all the subclusters.
- HAC can be useful to derive how "distant" two countries are in terms of similarity, and we can extract intermediate clusters at any threshold.
- Using Agglomerative Clustering we performed clustering on the features that we are analyzing.
![image](https://user-images.githubusercontent.com/28112225/144758922-4186d5d8-1661-45a7-ae80-d98785ea114e.png)
- We can see that countries cluster primarily according to the investment of the country in Education. This means that the more the investment in education higher the number of people with higher levels of educational attainment which in turn contributes to the rise in the overall GDP of the countries.

### Evaluation
The Gross Domestic Product of the countries has been increasing linearly throughout the years. We also notice that the educational attainments also follow the same general trend of growth in a linear fashion. 
> Mexico showing a correlation between masters attainment and GDP & USA's comparison between GDP and all attainment rates

![image](https://user-images.githubusercontent.com/28112225/144769313-d58ee8d5-5c03-4443-9c21-771f92aee4c4.png)
![image](https://user-images.githubusercontent.com/28112225/144769263-fac83145-5b13-499b-9a3d-f4cd2c4c396d.png)

Countries which has heavily invested in education have indeed shown far greater attainment rates. One such example is the United States. The economic standing of this country does reflect on their educational attainment levels but is not limited to that. In our Exploratory Data Analysis, we uncovered that a few countries (ex: Switzerland) despite not having greater spending on education, still has higher attainment levels than most countries. 

### More Visualizations
> The distributions of Doctoral attainment vs year for top 7 GDP countries. The output is as expected and the USA with the highest GDP (denoted by a bigger marker - higher GDP) has the highest doctoral attainment. 

![image](https://user-images.githubusercontent.com/28112225/142276753-660f89a5-a6ec-4365-832d-21bfcc392aa0.png)


### Conclusion
Although the educational attainments and the Economic state of the country (measured in terms of GDP) are positively correlated, there are some outliers where the spending on education is not so great but still returns good graduation rates. This exemplifies the fact that there could be more factors in play i.e socioeconomic factors like poverty rates, population count, literacy rates, and maybe even the political landscapes of the country.


