# Power Outages Analysis
This is a homework for EECS 398 at U-M 

## 1: Introduction
This analysis makes use of the Power Outage dataset, which tracks power outages in the US between the years 2000-2016. Though at first glance, this dataset seems rather technical, it is important in understanding power outage trends across the US, and the qualities that have remained constant and changed over the years. There are 1533 rows, each corresponding to a different power outage. There are 55 columns in this dataset, most of which are categorical, and corresponding to a specific characteristic relating to power outages. 
	The purpose of this analysis is to answer explore trends in power outage severity over time centered around the analytical question: 
	
 **What are the characteristics of high impact power outages? How have these factors and characteristics changed over time, and based on this, how can energy companies best allocate their resources to helping customers?**
 
	The column OUTAGE.DURATION is important here because it measures how severe a power outage is. The longer the power outage, the more impactful it is to residents, and the more revenue is lost for the company. So it is interesting to explore the factors that contribute to longer power outages, and if it is possible to predict the length of a power outage given other data. OUTAGE.DURATION is a quantitative continuous variable that is measured in minutes, with a min of 1 minute and mean of 108,653 minutes. 
We also explore OUTAGE.DURATION’s change over time using the YEAR column. With the acceleration of climate change in the past years, simple graphs showing the number of outages per year and average severity per year is useful. Also, understanding how a region comes into play adds another layer of complexity: the categorical CLIMATE.REGION column breaks down outages into 1 of 12 US regions. For our model, we used a few other quantitative variables, but our main analytical question and exploration was centered around these variables and analyzing trends over time. 

## 2: Data Cleaning and Exploratory Data Analysis
