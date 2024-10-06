# Wikipedia Article Traffic Analysis (2015-2024)
#### DATA-512 | Homework 1 | Professionalism & Reproducibility

## Project Goal
This project aims to construct, analyze, and publish a dataset of monthly article traffic for a select set of pages from English Wikipedia from July 1, 2015 through September 30, 2024. The dataset will provide insights into page view trends over time, allowing for analysis of article popularity and traffic patterns from desktop and mobile devices.

The motivation for project is to gain hands-on experience in adhering to best practices for open scientific research, ensuring reproducibility by providing accessible data, maintaining transparent documentation, and complying with licensing standards.

## Licenses

### Source Data
The source data for this project comes from the Wikimedia Foundation using their publicly exposed [Pageviews API](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html). 

[Wikimedia Foundation Terms of Use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use) allows free use of content while requiring attribution, preserving open licenses, and respecting copyright and user rights.
To ensure that this project complies with Wikimedia's Terms of Use, I've provided the required attribution referencing the original source of data.

### Code for Data Aquisition

Snippets of the code under [Wikipedia_Article_Traffic_Analysis_2015-2024.ipynb](Wikipedia_Article_Traffic_Analysis_2015-2024.ipynb) were taken from a code example developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons CC-BY license](https://creativecommons.org/licenses/by/4.0/)

## Documentation

#### API
* [Pageviews API](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html) : Page view analytics provides data about page views for Wikimedia projects. These endpoints serve data starting on July 1, 2015. 

#### Packages
* [urllib](https://docs.python.org/3/library/urllib.request.html) : For fetching data from Wikimedia's Pageviews API
* [pandas](https://pandas.pydata.org/docs/reference/index.html) : For processing the timeseries data fetched from the API calls
* [matplotlib](https://matplotlib.org/stable/api/index.html): For creating plots to provide an overview of analysis

## Generated Files

### Data files and corresponding schemas
Running the [Wikipedia_Article_Traffic_Analysis_2015-2024.ipynb](Wikipedia_Article_Traffic_Analysis_2015-2024.ipynb) creates multiple time series datasets of monthly activity for articles mentioned in [rare-disease_cleaned.AUG.2024.csv](rare-disease_cleaned.AUG.2024.csv), focussing exclusively on user pageview requests.
The resulting data should be structured as follows:
* Format: JSON files
* Organization: Use article titles (Page title) as keys for the time series data
* Content: Timeseries of monthly record data for each article

1. [rare-disease_monthly_cumulative_201501-202409.json](generated_files%2Frare-disease_monthly_cumulative_201501-202409.json): Has time series data of the monthly total (desktop + mobile) pageview activity,for each article in the above csv. \
Data Schema: 
```Text
{   
    string: [                         #Page title
        {       
            "project": string,        #Wikimedia domain and subdomain
            "article": string,        #Page title
            "granularity": string,    #Time interval between datapoints
            "timestamp": string,      #Timestamp in YYYYMMDDHH format
            "agent": string,          #Type of user agent
            "views": integer          #Number of page views
        }   
  ] 
}
```
2. [rare-disease_monthly_desktop_201501-202409.json](generated_files%2Frare-disease_monthly_desktop_201501-202409.json): Has time series data of the monthly desktop pageview activity,for each article in the above csv.\
   Data Schema:
```Text
{
    string: [                         #Page title
        {       
            "project": string,        #Wikimedia domain and subdomain
            "article": string,        #Page title
            "granularity": string,    #Time interval between datapoints
            "timestamp": string,      #Timestamp in YYYYMMDDHH format
            "agent": string,          #Type of user agent
            "views": integer          #Number of page views
        }   
  ] 
}
```
3. [rare-disease_monthly_mobile_201501-202409.json](generated_files%2Frare-disease_monthly_mobile_201501-202409.json): Has time series data of the monthly desktop pageview activity,for each article in the above csv.\
   Data Schema:
```Text
{
    string: [                           #Page title
        {
              "project": string,        #Wikimedia domain and subdomain
              "article": string,        #Page title
              "granularity": string,    #Time interval between datapoints
              "timestamp": string,      #Timestamp in YYYYMMDDHH format
              "agent": string,          #Type of user agent
              "views": integer          #Number of page views
        }
    ]
}
```
Note: For the purpose of this project, we are interested in monthly pageviews. Hence, granularity is set to "monthly" in all records.

### Plots
1. [max_avg_min_avg.png](generated_plots%2Fmax_avg_min_avg.png): This graph contains time series for the articles that have the highest average page requests and the lowest average page requests for desktop access and mobile access over the entire time series
2. [top_10_peak_page_views.png](generated_plots%2Ftop_10_peak_page_views.png): This graph contains time series for the top 10 article pages by largest (peak) page views over the entire time series by access type
3. [fewest_months_of_data.png](generated_plots%2Ffewest_months_of_data.png): This graph shows pages that have the fewest months of available data. It shows the 10 articles with the fewest months of data for desktop access and the 10 articles with the fewest months of data for mobile access.

## Issues encountered:
 The data acquisition code taken from Dr.David's work failed to encode the '/' in article titles like 'Sulfadoxine/pyrimethamine'. 
 Original code:
``` python
urllib.parse.quote(request_template['article'].replace(' ','_'))
```
Modified code (solution):
```python
urllib.parse.quote(request_template['article'].replace(' ','_'), safe='')
```
The safe='' parameter ensures all characters, including '/', are properly encoded. This modification was necessary to handle article titles containing '/' in the title.

## Considerations
1. Mobile Data: The Pageviews API separates mobile access types into two separate requests. The view counts in the generated file [rare-disease_monthly_mobile_201501-202409.json](generated_files%2Frare-disease_monthly_mobile_201501-202409.json) is the sum of both mobile web and mobile app traffic for a given article.
2. Data Gaps: Some articles may have missing data for certain months due to API limitations or data availability issues.
3. API Changes: The Wikimedia API may undergo changes over time, which could affect data consistency across the entire period.
4. Article Renames: If an article was renamed during the analysis period, it may appear as two separate entries.

Please note these considerations when using this project for research or analysis purposes.