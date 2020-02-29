# ETL-Project
Participants: Allyson Taylor, Benedicta Sumalatha, James Martin, and Michael Davis

Title: Does Weather in Indiana Impact If and When a Home Is Sold?

## Aims:

•	AIM 1: Evaluate existing data from the Indiana Property Disclosure datasets.

•	AIM 2: Analyze data for weather, home sales, and Census data to find correlations within each dataset. 

•	AIM 3: Design tables and utilize Open Weather Map (API), Kaggle (CSV), U.S. Census (API). 

•	AIM 4: Uitilize Analysis tools such as: Json | Python | CSV | pgAdmin 4

•	AIM 5: Provide data on if weather impacts the home sales in Indiana.


## Impact and Intended Results


The Hoosier State that is proud to have all four seasons of the year. No matter what time of year it is, you’ll find something fun to do outdoors thanks to our Midwestern location. Lately Indiana has been one of the top growth states of the USA in population. Since Indiana experiences all four seasons, the question on the table is to discover how weather impacts this growth. 

Our goal is to provide the impact of home sales in Indiana based on weather utilizing the CSV from: https://www.kaggle.com/shoreviewanalytics/indiana-property-sales-disclosure.

We will look at dates, temperature changes, conditions, buyer location per county, sale prices, population count, head income and medium home value. 

Our goal is to provide the impact of home sales in Indiana based on weather. We will look at dates, temperature changes, conditions, buyer location per county, sale prices, population count, head income and medium home value. 

# Average Temperatures

As you might guess, January is typically the coldest month and July the hottest. Here are average temperatures for each month. 

![Average Temperatures](https://visitindiana.com/adportal/Content/FileUploads/cms/weather/weather_chart_temp.png)


# Monthly Precipitation

While kids do get to enjoy snow days in Indiana, the summer months bring the most precipitation. Check out the monthly averages.

![Monthly Precipitation](https://visitindiana.com/adportal/Content/FileUploads/cms/weather/weather_chart_precip.png)

## The Plan

# Steps to Extract and Transform the Census Data

Extraction - Extract: your original data sources and how the data was formatted (CSV, JSON, pgAdmin 4, etc).
Open Weather Now (API)
Indiana Home Sales (Kaggle csv)
U.S. Census (API)

The extracted housing related data source was the U.S. Census and was aquired using an API. The specific data set selected was from was the 2018 American Community Survey 5-Year Survey U.S. Census ACS and the data points we selected came from the following table: U.S. Census Variables

![Census Data](https://github.com/allysontalyor/ETL-Project/blob/master/Images/ACS_Screen_Shot.png)
Steps to Extract and Transform the Home Price Data:

Extract

•	The first step is to read in the original CSV file.  This data set came from kaggle, and is titled: Indiana - Property Sales Disclosure.  The link is: https://www.kaggle.com/shoreviewanalytics/indiana-property-sales-disclosure.  This file is 2GB!

•	The file was downloaded directly from the website.


Transform

•	This dataset contains 87 columns, including data on the buyer, seller, sales price, lot description, tax information, and sales date.

•	The following columns were selected: Parcel1_Acreage, PropStreet, PropCity, PropState, PropZip, C6_Sales_price_Assessor, Buyer1Street, Buyer1City, Buyer1State, Buyer1ZIP, Conveyance_Date

•	A .dropna() was performed on the data to drop any rows that contain an NaN.

•	There were many inconsistencies in the Property City column, including misspellings and differences in the cases of the letters.

•	We obtained weather data for 107 Indiana cities.  Only data for those cities was included in the final home price dataset.  To do this, the city list was copied over from the weather jupyter notebook.  Then the cities needed to be converted to upper case letters to match up with the city entries in the home price dataset.  A for loop was setup to include only those 107 cities. 

•	Columns were renamed to be more descriptive.  New column names: Acreage, Property Street, Property City, Property State, Property Zip, Sale Price, Buyer Street, Buyer City, Buyer State, Buyer Zip, Conveyance Date.

•   We discovered 1,934 duplicates on home_prices.csv file

•	Duplicate data was dropeed using: .drop_duplicates() 

•	Finally, the cleaned file was saved as a csv.


## Steps to Extract and Transform the Weather Data:


Extract

•	Our original plan was to get data for the specific location and day of each home sale using an API call from Open Weather Map.  Historical data was unavailable without a paid subscription.  This was true of many other weather websites as well.

•	Weather data was scraped from the following website: https://www.usclimatedata.com/climate/indiana/united-states/3184

•	Chromedriver was used to collect html data from the website and BeautifulSoup was used to extract parts of the data that we needed.

•	The first piece of information that was needed was the url endpoint for each city to obtain weather data from the tables.  This was done by scraping the endpoints.

•	The following code was used to obtain data from the tables of average weather data.  An empty dataframe titled average_monthly was created to hold the scraped data.  A for loop was created to cycle through the urls and the pandas function .read_html() was used to collect the data from the tables on the page.  A column was added to include the name of the city.  This was done by splitting the url string using the pandas function .split(“/”), and then indexing the component containing the name of the city.  

average_monthly = pd.DataFrame()

for url in url_list:
    main_url = "https://www.usclimatedata.com" + url
    tables = pd.read_html(main_url)
    df_1 = tables[0]
    df_2 = tables[1]
    df_3 = pd.merge(df_1,df_2, on = "Unnamed: 0",how="inner")
    url_split = url.split("/")
    df_3["City"] = url_split[2].upper()
    average_monthly = average_monthly.append(df_3)

![U.S. Climate Data](https://github.com/allysontalyor/ETL-Project/blob/master/Images/U.S.%20Climate%20Data.png)



Transform

•	The columns were transposed and then the files merged back together so that the averaged weather data are columns.

•	A County column was added so that the census data and the weather table can be joined. The home price data was joined via the city name.

•	Please note that in this dataframe some “NaN” values were intentionally left in.  Data is present for all cities for the average high and low temperatures, but snowfall data is only available for 64 of the 107 cities.  



# NOTES

•	The final csv file for the home price data:  home_price_final.csv

•	The final csv file for the weather data:  average_indiana_weather.csv

•	All work to extract and transform the home price data is in:  Home_Price_Data.ipynb

•	All work to extract and transform the weather data is in:  weather_data.ipynb

# Potential Future Questions to Researh:
1. Does Indiana monthly temperature impact monthly growth?
2. Where in the Unites States are most people moving to Indiana from? 
3. What counties in Indiana experience the most growth per year? 
4. Which season of the year provides a decline in yearly population growth? 
