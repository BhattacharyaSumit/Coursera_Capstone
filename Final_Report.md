
 # Customer Segmentation for _Retail Businesses_

This is the age of Startups. People all over the world are interested in making their own busines/ self-employment groom. What I have found in my research is that many "Retail Shops" are coming up in the markets of different places in the world. My idea is to do a **Segmentation Analysis** of the customers,in Europe, based on their frequency of buying, and last buying date.

My idea of this project is fostered by the news that FourSquare has recently  introduced a new API for 'StartUps and New Businesses'.   
Here I am providing a link to the news that I found...
[*Here is the link*](https://venturebeat.com/2018/04/12/foursquare-launches-places-api-for-startups-and-small-businesses/)
Using this API, businesses can access location based on data.

## Data Section

I will be using the Dataset of [*Online Retail*](https://archive.ics.uci.edu/ml/datasets/Online+Retail#) obtaind from  **UCI Machine Learning Repository**. 

This dataset contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based online retail store.The company mainly sells all-occasion gifts. Most of the customers of the company are wholesalers.
Through this Analysis, I want to come to a conclusion of Customer Clusters that can help small-scale businesses to bloom.

### _Description of the Data_

The Dataset contains 8 features and 541909 observations.  The 8 features(or attributes) are -

**InvoiceNo:** Invoice number. A 6-digit integral number uniquely assigned to each transaction.

**StockCode:** Product code. A 5-digit integral number uniquely assigned to each distinct product.

**Description:** Product name. 

**Quantity:** The quantities of each product per transaction.

**InvoiceDate:** Invice Date and time. 

**UnitPrice:** Unit price(in Sterling)

**CustomerID:** Customer number. A 5-digit integral number uniquely assigned to each customer. 

**Country:** Country name (European)

## Methodology Section

In my report, I have tried to do an analysis of the Dataset mentioned above.

#### Step 1 (Reading the data from into the Jupyter Notebook)

Here, I have used Pandas for this purpose.


#### Step 2 

With the aim of segmenting the customers on the basis of 

a) Recency
b) Frequency,

I have created the columns of Invoice Day and Cohort Day.
 
 _Cohort is a group of people sharing a group of characteristics_
 

A column for Cohort Index is required. Cohort Index tells us the number of days/months since the last transaction with the customer

#### Step 3 (Segmenting the Customers)

Here, I have made a DataFrame of Recent and Frequent customers. The logic is that I have assumed that I am working on the dataset right after the last day of the transaction. The difference between the transaction date of a customer and the overall last date of transaction will give us the Recency of the Customer. For the number of times an Invoice is issued for a single Customer ID is the Frequency of the Customer.


Now, on the basis of Recency and Frequency, _'R'_ and _'F'_ pentiles are deciced by me.

So, Customers have been divided into 5 groups (R) based on Recency and 5 groups (F) based on Frequency.
Then, the RF score is calculated (*'RF_SC'*).

Based on the RF score, the Customers are segmented into **Top**, **Mid** and **Low** Segments.

#### Step 4 (Clustering the Customers based on KMeans)

Now, for verification purpose of correct grouping of Customers, I have applied KMeans Clustering. So that no variables impose extra influence on the model, I have standardized the variables (Recent and Frequency). As proper standardization requires the variables to unskewed, I have log-transformed the variables to remove skewedness before standardizing.

Having done this, I have applied KMeans on the dataset (with variables Recent and Frequency) with 10 different groupings from 1 through 10. This is done to find the optimal number of clusters using Elbow plot.
From the Elbow plot, 3 clusters appeared to be the optimal number of clusters.

KMeans Method very nicely categorized the Customers into 3 clusters.

Based on the plots of the Customers based on the groupings, I decided to take the Segmentation based on Cohort Analyis to be optimum

#### Step 5

In this step, I have fetched the information for _Target Customers_ (as obtained by Segmentation) from the original Dataset based on the CustomerID. Then, I fetched the Countries of the most frequent customers.

Using the 'RESTCOUNTRIES' API , I have fetched the capitals of the these countries.

On the internet, I found that capitals of most of the European countries are the financial capitals as well. This is the reason I chose the capitals.

Using the _geopy_ package, I got the latitudes and longitudes of the capitals.

#### Step 6 (FourSquare API)

With the Capitals and their Latitudes & Longitudes in hand, it becomes easy to fetch the details of and around the location using **FourSquare**.
So, I made a call to the FourSquare API to fetch the details of the Location and Venue of the trending places near the capitals where people might be interested in Purchasing Items.

##### Additional Step 
 
   I made a Map of the Target Regions for presentability



## Result Section

The result that I could draw from this analysis:

There are many customers in Europe ,especially in the regions in the Western part of Europe founded by the above analysis. Small and Medium Businesses can definitely try to target these regions more so as to grow to large scale Businesses.

## Discussion Section

Through this analysis , I could find out that :
    
   a) Segmentation of the Customers based on Optimal Cohort Analysis and Optimal Clustering of the Customers
      based on KMeans produce almost the same result. It depends upon the analyzer to choose between the 2 
      methods for final analysis. We can always try both of these on the Dataset before arriving at a conclusion.
    
   b) Although not evident from the Map, but the Dataset gives us a fair idea that most of the "Top" Customers 
      are from United Kingdom. So, United Kingdom can act as a Marketing HotSpot for the New Businesses.

## Conclusion Section

The DataSet gives us a fair idea of the European Purchasing Trend in 2010 - 2011.  The Market is growing. Data-Science plays an important role in any Business and can leveraging the power of it, we can always foster the growth of industries. 
I hope, my report is of help to the business owners.

   ### -**Report By**
   ## Sumit Bhattacharya
   ### Dated: 17th Dec, 2018
