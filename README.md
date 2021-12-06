# Team members:
- Nidhi Srinath (nsrinat1@uncc.edu)
- Kautilya Kondragunta (kkondrag@uncc.edu)
- Naimisha Churi (nchuri@uncc.edu)

# Data Sources:
- World Bank (https://data.worldbank.org)

# **Relational Analysis of Country-Wise Economic Growth and Academic Qualification levels**

Education is the act of learning and teaching knowledge through different mediums. It is the most important pillar of growth and development. It facilitates the progress of an individual. It also helps unify different ideologies, opinions and connects people belonging to different backgrounds. Increase in knowledge leads to increase in opportunities. Education also helps aid communication among a group of people which leads to exchange of ideas and implementations. When an individual’s education level increases, the cognitive capacity of the individual also increases. As necessary as education is for an individual, it also acts as an important factor in the development of a country. Education is a very important investment for a country. The growth of a country could be increased with the increase in the number of educated and skilled workers. An individual that possesses the ability to analyse information with the help of logic and critical thinking can contribute positively to increase productivity and efficiency in any task that they are involved in. With more educated people, the number of employment opportunities increases. Significantly higher education rates are responsible for increase in human capital. Education helps in the thriving of the socio-economic wellbeing of a nation. Through our project, we intend to understand how much of a correlation exists between the average education levels of a country and its gross domestic product. We also intend to bring a sense of comparison between the education levels of a country through the past decades. We attempt to understand the level of growth of a country’s gross domestic product by analysing the percentage of citizens that are educated at different levels such as primary, secondary, tertiary, bachelors, graduate, and doctoral.


# Data Description
The data used in this project has been acquired from World Bank's Open data [https://data.worldbank.org]. It has data on all kinds of development indices for every country recorded each year. The following data was 

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
- Data Understanding Phase
- Data Preparation Phase
- Data Exploration
- Modeling Phase
- Evaluation Phase

## Research Phase
Education is no doubt an important aspect that determines the future of the country. A country's economic growth is driven by new inventions and discoveries in various fields that yields a higher revenue for the better educated labour force and in turn for the country. This usually has a positive correlation with the country's Gross Domestic Product (GDP). A few recent studies that looked at the effect of educational expenditure as a proxy for education reported a positive growth in education wit increase in exenditure. 
> A recent a meta-analysis considered 29 papers that specifically look at the impact of government education expenditures on economic growth. Of these 29 studies, 14 report a positive and statistically significant effect of government expenditure on growth, 12 report a negative effect, and 3 report no statistically significant effect.

One other study claims that increase in govt' expenditure on education improved the completed years of education per pupil by `0.27` years, increased the mean wage levels by `7.25%` and a `3.67`% point reduction in the annual adult poverty levels. In this project we try to breakdown a country's spending on education and its correlation with the educational attainments in % of adult population (25+).

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
Right off the bat, the data we had in hand was very sparse with a lot of null values. We had to clean the data before be began to derive information from it. Since we had so much nullity, this project was mainly focused on the different methods we used to perform null value analysis and reduction.

- First we had to convert the time series data which had each year as a column to one `Year` column containing all the years for each country.
- We converted the columns to their appropriate data types.
- We dropped all the country-year pairs which had no data for all columns we needed data on [Lower Secondary, Upper Secondary, Bachelors, Masters, Doctoral]. This still has left us with a few null values in a few features!
- While we tried to solve that issue, we also found out that a few countries had lesser years of data. Like for example - India has only data for 2011. This might be because of the large pupolation in the country and the fact that the Census is once every 10 years. We filtered such countries out too.
- We're almost there! But, we still had a few countries which were missing a couple of years of attainment and GDP data. So, we turned towards Linear Regression to impute the missing values for those fields.
- Now, we have a fully filled dataset. As amazing as that sounds, we still had to do some feature engineering like deriving the ependiture on education in US Dollars from the %, renaming a few columns for standardization etc.

### Machine Learning
We realized that what we had was a task for an Unsupervised machine learning algorithm - To find clusters in our data and extract useful information from them. 
- Using Agglomerative Clustering from the performed clustering on the features that we are analyzing.
![image](https://user-images.githubusercontent.com/28112225/144758922-4186d5d8-1661-45a7-ae80-d98785ea114e.png)
- We can see that countries cluster primarily according to the investment of the country in Education. Which means that more the investment in education higher the number of people with higher levels of educational attainment which in turn contributes to the rise in the overall GDP of the countries.

### Evaluation
The Gross Domestic Product of the countries has been increasing in a linear fashion throughout the years. We also notice that the educational attainments also follow the same general trend of growing in a linear fashion. 
> Mexico showing correlation between masters attainment and GDP & USA's comparision between GDP and all attainment rates

![image](https://user-images.githubusercontent.com/28112225/144769313-d58ee8d5-5c03-4443-9c21-771f92aee4c4.png)
![image](https://user-images.githubusercontent.com/28112225/144769263-fac83145-5b13-499b-9a3d-f4cd2c4c396d.png)

Countries which has heavily invested in education has indeed shown far greater attainment rates. One such example is the United States. The economic standing of this country does reflect on their educational attainment levels, but is not limited to that. In our Exploratory Data Analysis we uncovered that a few countries (ex: Switzerland) inspite of not having a greater spending on education, still has higher attainment levels than most countries. 
> Countries showing irregular distributions of Masters Attainment and Expenditure on Education

![image](https://user-images.githubusercontent.com/28112225/144769349-17040890-e42b-4855-8ced-f10ee537d91f.png)


### More Visualizations
> ### The distributions of Doctoral attainment vs year for top 7 gdp countries. The output is as expected and USA with the highest GDP (denoted by a bigger marker - higher GDP) has the highest doctoral attainment. 

![image](https://user-images.githubusercontent.com/28112225/142276753-660f89a5-a6ec-4365-832d-21bfcc392aa0.png)


### Conclusion
Although the educational attainments and the Economic state of the country (measured in terms of GDP) are positively correlated, there are some outliers where the spending on education is not so great but still return good graduation rates. This exemplifies the fact that there could be more factors in play i.e socioeconomic factors like poverty rates, population count, literacy rates and maybe even the political landscapes of the country.


