# Exploratory Data Analysis and RFM Segmentation using Snowflake SQL


## Creating the Database

```sql
-- Creating the Database
CREATE DATABASE SALES;
USE DATABASE SALES;
-- Creating the table
CREATE OR REPLACE TABLE SALES_SAMPLE_DATA (
    ORDERNUMBER NUMBER (8, 0),
    QUANTITYORDERED NUMBER (8,2),
    PRICEEACH NUMBER (8,2),
    ORDERLINENUMBER NUMBER (3, 0),
    SALES NUMBER (8,2),
    ORDERDATE VARCHAR (16),
    STATUS VARCHAR (16),
    QTR_ID NUMBER (1,0),
    MONTH_ID NUMBER (2,0),
    YEAR_ID NUMBER (4,0),
    PRODUCTLINE VARCHAR (32),
    MSRP NUMBER (8,0),
    PRODUCTCODE VARCHAR (16),
    CUSTOMERNAME VARCHAR (32),
    PHONE VARCHAR (16),
    ADDRESSLINE1 VARCHAR (64),
    ADDRESSLINE2 VARCHAR (64),
    CITY VARCHAR (16),
    STATE VARCHAR (16),
    POSTALCODE VARCHAR (16),
    COUNTRY VARCHAR (24),
    TERRITORY VARCHAR (24),
    CONTACTLASTNAME VARCHAR (16),
    CONTACTFIRSTNAME VARCHAR (16),
    DEALSIZE VARCHAR (10) 
);

--Inspecting Data
SELECT * FROM SALES_SAMPLE_DATA LIMIT 10;
```
### Results:
| ORDERNUMBER | QUANTITYORDERED | PRICEEACH | ORDERLINENUMBER | SALES    | ORDERDATE | STATUS  | QTR_ID | MONTH_ID | YEAR_ID | PRODUCTLINE | MSRP | PRODUCTCODE | CUSTOMERNAME             | PHONE            | ADDRESSLINE1                  | ADDRESSLINE2  | CITY   | STATE  | POSTALCODE | COUNTRY  | TERRITORY | CONTACTLASTNAME | CONTACTFIRSTNAME | DEALSIZE |
|-------------|-----------------|-----------|-----------------|----------|-----------|---------|--------|----------|---------|-------------|------|-------------|--------------------------|------------------|-------------------------------|---------------|--------|--------|------------|----------|-----------|-----------------|------------------|----------|
| 10,107      | 30              | 95.7      | 2               | 2,871    | 24/2/03   | Shipped | 1      | 2        | 2003    | Motorcycles | 95   | S10_1678    | Land of Toys Inc.        | 2125557818       | 897 Long Airport Avenue       | NYC           | NY     | 10022  | USA        | NA       | Yu        | Kwai            | Small            |          |
| 10,121      | 34              | 81.35     | 5               | 2,765.9  | 7/5/03    | Shipped | 2      | 5        | 2003    | Motorcycles | 95   | S10_1678    | Reims Collectables       | 26.47.1555       | 59 rue de l'Abbaye            | Reims         | 51100  | France | EMEA       | Henriot  | Paul      | Small           |                  |          |
| 10,134      | 41              | 94.74     | 2               | 3,884.34 | 1/7/03    | Shipped | 3      | 7        | 2003    | Motorcycles | 95   | S10_1678    | Lyon Souveniers          | +33 1 46 62 7555 | 27 rue du Colonel Pierre Avia | Paris         | 75508  | France | EMEA       | Da Cunha | Daniel    | Medium          |                  |          |
| 10,145      | 45              | 83.26     | 6               | 3,746.7  | 25/8/03   | Shipped | 3      | 8        | 2003    | Motorcycles | 95   | S10_1678    | Toys4GrownUps.com        | 6265557265       | 78934 Hillside Dr.            | Pasadena      | CA     | 90003  | USA        | NA       | Young     | Julie           | Medium           |          |
| 10,159      | 49              | 100       | 14              | 5,205.27 | 10/10/03  | Shipped | 4      | 10       | 2003    | Motorcycles | 95   | S10_1678    | Corporate Gift Ideas Co. | 6505551386       | 7734 Strong St.               | San Francisco | CA     | USA    | NA         | Brown    | Julie     | Medium          |                  |          |
| 10,168      | 36              | 96.66     | 1               | 3,479.76 | 28/10/03  | Shipped | 4      | 10       | 2003    | Motorcycles | 95   | S10_1678    | Technics Stores Inc.     | 6505556809       | 9408 Furth Circle             | Burlingame    | CA     | 94217  | USA        | NA       | Hirano    | Juri            | Medium           |          |
| 10,180      | 29              | 86.13     | 9               | 2,497.77 | 11/11/03  | Shipped | 4      | 11       | 2003    | Motorcycles | 95   | S10_1678    | Daedalus Designs Imports | 20.16.1555       | 184, chausse de Tournai       | Lille         | 59000  | France | EMEA       | Rance    | Martine   | Small           |                  |          |
| 10,188      | 48              | 100       | 1               | 5,512.32 | 18/11/03  | Shipped | 4      | 11       | 2003    | Motorcycles | 95   | S10_1678    | Herkku Gifts             | +47 2267 3215    | Drammen 121, PR 744 Sentrum   | Bergen        | N 5804 | Norway | EMEA       | Oeztan   | Veysel    | Medium          |                  |          |
| 10,201      | 22              | 98.57     | 2               | 2,168.54 | 1/12/03   | Shipped | 4      | 12       | 2003    | Motorcycles | 95   | S10_1678    | Mini Wheels Co.          | 6505555787       | 5557 North Pendale Street     | San Francisco | CA     | USA    | NA         | Murphy   | Julie     | Small           |                  |          |
| 10,211      | 41              | 100       | 14              | 4,708.44 | 15/1/04   | Shipped | 1      | 1        | 2004    | Motorcycles | 95   | S10_1678    | Auto Canal Petit         | (1) 47.55.6555   | 25, rue Lauriston             | Paris         | 75016  | France | EMEA       | Perrier  | Dominique | Medium          |                  |          |

## Unique Values
```sql
select distinct status from SALES_SAMPLE_DATA;
```
### Results:
| STATUS     |
|------------|
| Shipped    |
| Disputed   |
| In Process |
| Cancelled  |
| On Hold    |
| Resolved   |
```sql
select distinct year_id from SALES_SAMPLE_DATA;
```
### Results:
| YEAR_ID |
|---------|
| 2003    |
| 2004    |
| 2005    |
```sql
select distinct PRODUCTLINE from SALES_SAMPLE_DATA;
```
### Results:
| PRODUCTLINE      |
|------------------|
| Motorcycles      |
| Classic Cars     |
| Trucks and Buses |
| Vintage Cars     |
| Planes           |
| Ships            |
| Trains           |
```sql
select distinct COUNTRY from SALES_SAMPLE_DATA;
```
### Results:
| COUNTRY     |
|-------------|
| USA         |
| France      |
| Norway      |
| Finland     |
| Austria     |
| UK          |
| Spain       |
| Singapore   |
| Japan       |
| Italy       |
| Sweden      |
| Denmark     |
| Belgium     |
| Philippines |
| Germany     |
| Switzerland |
| Ireland     |
| Australia   |
| Canada      |
```sql
select distinct DEALSIZE from SALES_SAMPLE_DATA;
```
### Results:
| DEALSIZE |
|----------|
| Small    |
| Medium   |
| Large    |
```sql
select distinct TERRITORY from SALES_SAMPLE_DATA;
```
### Results:
| TERRITORY |
|-----------|
| NA        |
| EMEA      |
| APAC      |
| Japan     |

## Exploratory Data Analysis


### Grouping Sales by Product Line
``` sql
-- Grouping sales by product line to understand the distribution of sales across different product categories.

select PRODUCTLINE, ROUND(sum(sales),0) AS Revenue, COUNT(DISTINCT ORDERNUMBER) AS NO_OF_ORDERS
from SALES_SAMPLE_DATA
group by PRODUCTLINE
order by 3 desc;
```
### Results:
| PRODUCTLINE      | REVENUE   | NO_OF_ORDERS |
|------------------|-----------|--------------|
| Classic Cars     | 3,797,679 | 190          |
| Vintage Cars     | 1,778,697 | 163          |
| Motorcycles      | 1,151,329 | 70           |
| Trucks and Buses | 1,056,547 | 68           |
| Ships            | 651,089   | 60           |
| Planes           | 910,313   | 54           |
| Trains           | 217,392   | 43           |



### Total Revenue by Year
``` sql

select YEAR_ID, sum(sales) Revenue
from SALES_SAMPLE_DATA
group by YEAR_ID
order by 2 desc;
```
### Results:
| YEAR_ID | REVENUE      |
|---------|--------------|
| 2004    | 4,412,321.07 |
| 2003    | 3,359,238.69 |
| 2005    | 1,791,486.71 |



### Revenue by Deal Size
``` sql

select  DEALSIZE,  sum(sales) Revenue
from SALES_SAMPLE_DATA
group by DEALSIZE
order by 2 desc;
```
### Results:
| DEALSIZE | REVENUE      |
|----------|--------------|
| Medium   | 5,815,484.87 |
| Small    | 2,505,263.91 |
| Large    | 1,242,297.69 |



### City with Highest Sales in a Specific Country
``` sql
--What city has the highest number of sales in a specific country
select city, sum (sales) Revenue
from SALES_SAMPLE_DATA
where country = 'UK'
group by city
order by 2 desc;
```
### Results:
| CITY       | REVENUE    |
|------------|------------|
| Manchester | 157,807.81 |
| London     | 124,823.54 |
| Liverpool  | 118,008.27 |
| Cowes      | 78,240.84  |



### Best Selling Product in the United States
```sql
--What is the best product in United States?
select country, YEAR_ID, PRODUCTLINE, sum(sales) Revenue
from SALES_SAMPLE_DATA
where country = 'USA'
group by  country, YEAR_ID, PRODUCTLINE
order by 4 desc;
```
### Results:
| COUNTRY | YEAR_ID | PRODUCTLINE      | REVENUE    |
|---------|---------|------------------|------------|
| USA     | 2004    | Classic Cars     | 560,448.26 |
| USA     | 2003    | Classic Cars     | 558,544.09 |
| USA     | 2004    | Vintage Cars     | 301,982.35 |
| USA     | 2004    | Motorcycles      | 287,243.09 |
| USA     | 2003    | Vintage Cars     | 266,141.82 |
| USA     | 2004    | Trucks and Buses | 230,219.17 |
| USA     | 2005    | Classic Cars     | 225,645.87 |
| USA     | 2005    | Vintage Cars     | 189,631.73 |
| USA     | 2003    | Motorcycles      | 178,108.95 |
| USA     | 2004    | Planes           | 177,431.76 |
| USA     | 2003    | Trucks and Buses | 125,794.86 |
| USA     | 2004    | Ships            | 102,595    |
| USA     | 2003    | Planes           | 90,016.44  |
| USA     | 2005    | Planes           | 60,984.69  |
| USA     | 2003    | Ships            | 58,237.59  |
| USA     | 2005    | Motorcycles      | 55,019.66  |
| USA     | 2005    | Ships            | 48,855.55  |
| USA     | 2005    | Trucks and Buses | 41,828.39  |
| USA     | 2003    | Trains           | 28,304.13  |
| USA     | 2004    | Trains           | 25,551.06  |
| USA     | 2005    | Trains           | 15,398.37  |



### Best Month for Sales in a Specific Year
```sql
----What was the best month for sales in a specific year? How much was earned that month? 
select  MONTH_ID, sum(sales) Revenue, count(ORDERNUMBER) Frequency
from SALES_SAMPLE_DATA
where YEAR_ID = 2003
group by  MONTH_ID
order by 2 desc;
```
### Results:
| MONTH_ID | REVENUE    | FREQUENCY |
|----------|------------|-----------|
| 11       | 985,828.35 | 282       |
| 10       | 541,033.18 | 150       |
| 9        | 263,973.36 | 76        |
| 12       | 261,876.46 | 70        |
| 8        | 197,809.3  | 58        |
| 4        | 197,390.35 | 57        |
| 7        | 187,731.88 | 50        |
| 6        | 168,082.56 | 46        |
| 3        | 164,755.9  | 48        |
| 2        | 140,836.19 | 41        |
| 5        | 131,599.9  | 41        |
| 1        | 118,321.26 | 35        |

```sql

select  MONTH_ID, sum(sales) Revenue, count(ORDERNUMBER) Frequency
from SALES_SAMPLE_DATA
where YEAR_ID = 2004
group by  MONTH_ID
order by 2 desc;
```
### Results:
| MONTH_ID | REVENUE    | FREQUENCY |
|----------|------------|-----------|
| 11       | 949,097.21 | 266       |
| 10       | 552,924.25 | 159       |
| 8        | 418,168.92 | 119       |
| 12       | 372,802.66 | 110       |
| 7        | 327,144.09 | 91        |
| 2        | 311,419.53 | 86        |
| 1        | 291,581.95 | 84        |
| 6        | 286,674.22 | 85        |
| 9        | 285,756.99 | 81        |
| 5        | 273,438.39 | 74        |
| 4        | 174,845.62 | 52        |
| 3        | 168,467.24 | 48        |
```sql
-- Finding the best month for sales in a specific year (e.g., 2005) and calculating revenue and frequency.
select  MONTH_ID, sum(sales) Revenue, count(ORDERNUMBER) Frequency
from SALES_SAMPLE_DATA
where YEAR_ID = 2005
group by  MONTH_ID
order by 2 desc;
```
### Results:
| MONTH_ID | REVENUE    | FREQUENCY |
|----------|------------|-----------|
| 5        | 457,861.06 | 120       |
| 3        | 374,262.76 | 106       |
| 2        | 358,186.18 | 97        |
| 1        | 339,543.42 | 99        |
| 4        | 261,633.29 | 56        |




### Product Sales in a Specific Month
```sql
-- Identifying the top-selling product line in a specific month (e.g., November 2004).
select  MONTH_ID, PRODUCTLINE, sum(sales) Revenue, count(ORDERNUMBER)
from SALES_SAMPLE_DATA
where YEAR_ID = 2004 and MONTH_ID = 11
group by  MONTH_ID, PRODUCTLINE
order by 3 desc;
```
### Results:
| MONTH_ID | PRODUCTLINE      | REVENUE    | COUNT(ORDERNUMBER) |
|----------|------------------|------------|--------------------|
| 11       | Classic Cars     | 336,600.26 | 95                 |
| 11       | Vintage Cars     | 207,908.15 | 57                 |
| 11       | Motorcycles      | 140,830.11 | 37                 |
| 11       | Planes           | 106,161.14 | 32                 |
| 11       | Trucks and Buses | 81,906.44  | 21                 |
| 11       | Ships            | 57,227.56  | 19                 |
| 11       | Trains           | 18,463.55  | 5                  |


## RFM Segmentation


### Calculating RFM Scores
```sql
-- Creating a view for RFM segmentation based on Recency (R), Frequency (F), and Monetary (M) scores

-- Step 1: Calculate RFM metrics for each customer
CREATE OR REPLACE VIEW rfm_segment AS
WITH CTE1 AS
	(
		-- Calculate MonetaryValue, AvgMonetaryValue, Frequency, last_order_date, and Recency for each customer
		SELECT 
			CUSTOMERNAME, 
			ROUND(sum(sales), 0) AS MonetaryValue,
			ROUND(avg(sales), 0) AS AvgMonetaryValue,
			count(DISTINCT ORDERNUMBER) AS Frequency,
			MAX(TO_DATE(ORDERDATE, 'DD/MM/YY')) AS last_order_date,
			(SELECT MAX(TO_DATE(ORDERDATE, 'DD/MM/YY')) FROM SALES_SAMPLE_DATA) AS max_order_date,
			DATEDIFF('DAY', MAX(TO_DATE(ORDERDATE, 'DD/MM/YY')), (SELECT MAX(TO_DATE(ORDERDATE, 'DD/MM/YY')) FROM SALES_SAMPLE_DATA)) AS Recency
		FROM SALES_SAMPLE_DATA
		GROUP BY CUSTOMERNAME
	),

-- Step 2: Calculate RFM scores using NTILE to segment customers into quartiles
rfm_calc AS
(
	SELECT C.*,
		NTILE(4) OVER (ORDER BY Recency DESC) AS rfm_recency,
		NTILE(4) OVER (ORDER BY Frequency ASC) AS rfm_frequency,
		NTILE(4) OVER (ORDER BY MonetaryValue ASC) AS rfm_monetary
	FROM CTE1 C
)

-- Step 3: Calculate the total RFM score and create a combined RFM score category
SELECT 
	R.*,
	(rfm_recency + rfm_frequency + rfm_monetary) AS rfm_total_score,
	CONCAT(CAST(rfm_recency AS VARCHAR), CAST(rfm_frequency AS VARCHAR), CAST(rfm_monetary AS VARCHAR)) AS rfm_score_category
FROM rfm_calc R;

-- Display the results of RFM segmentation
SELECT * FROM rfm_segment;
```
### Results:
| CUSTOMERNAME                   | MONETARYVALUE | AVGMONETARYVALUE | FREQUENCY | LAST_ORDER_DATE | MAX_ORDER_DATE | RECENCY | RFM_RECENCY | RFM_FREQUENCY | RFM_MONETARY | RFM_TOTAL_SCORE | RFM_SCORE_CATEGORY |
|--------------------------------|---------------|------------------|-----------|-----------------|----------------|---------|-------------|---------------|--------------|-----------------|--------------------|
| Boards & Toys Co.              | 9,129         | 3,043            | 2         | 2005-02-08      | 2005-05-31     | 112     | 3           | 1             | 1            | 5               | 311                |
| Atelier graphique              | 24,180        | 3,454            | 3         | 2004-11-25      | 2005-05-31     | 187     | 2           | 3             | 1            | 6               | 231                |
| Auto-Moto Classics Inc.        | 26,479        | 3,310            | 3         | 2004-12-03      | 2005-05-31     | 179     | 2           | 2             | 1            | 5               | 221                |
| Microscale Inc.                | 33,145        | 3,314            | 2         | 2004-11-03      | 2005-05-31     | 209     | 2           | 1             | 1            | 4               | 211                |
| Royale Belge                   | 33,440        | 4,180            | 4         | 2005-01-10      | 2005-05-31     | 141     | 3           | 4             | 1            | 8               | 341                |
| Double Decker Gift Stores, Ltd | 36,019        | 3,002            | 2         | 2004-01-22      | 2005-05-31     | 495     | 1           | 1             | 1            | 3               | 111                |
| Cambridge Collectables Co.     | 36,164        | 3,288            | 2         | 2004-05-08      | 2005-05-31     | 388     | 1           | 2             | 1            | 4               | 121                |
| West Coast Collectables Co.    | 46,085        | 3,545            | 2         | 2004-01-29      | 2005-05-31     | 488     | 1           | 1             | 1            | 3               | 111                |
| Men 'R' US Retailers, Ltd.     | 48,048        | 3,432            | 2         | 2004-01-09      | 2005-05-31     | 508     | 1           | 1             | 1            | 3               | 111                |
| CAF Imports                    | 49,642        | 3,819            | 2         | 2004-03-19      | 2005-05-31     | 438     | 1           | 1             | 1            | 3               | 111                |
| Signal Collectibles Ltd.       | 50,219        | 3,348            | 2         | 2004-02-10      | 2005-05-31     | 476     | 1           | 1             | 1            | 3               | 111                |
| Mini Auto Werke                | 52,264        | 3,484            | 3         | 2005-03-10      | 2005-05-31     | 82      | 3           | 2             | 1            | 6               | 321                |
| Iberia Gift Imports, Corp.     | 54,724        | 3,648            | 2         | 2004-10-06      | 2005-05-31     | 237     | 1           | 1             | 1            | 3               | 111                |
| Online Mini Collectables       | 57,198        | 3,813            | 2         | 2004-09-10      | 2005-05-31     | 263     | 1           | 1             | 1            | 3               | 111                |
| Gift Ideas Corp.               | 57,294        | 3,015            | 3         | 2004-12-04      | 2005-05-31     | 178     | 3           | 3             | 1            | 7               | 331                |
| Clover Collections, Co.        | 57,756        | 3,610            | 2         | 2004-09-16      | 2005-05-31     | 257     | 1           | 1             | 1            | 3               | 111                |
| Australian Gift Network, Co    | 59,469        | 3,965            | 3         | 2005-02-02      | 2005-05-31     | 118     | 3           | 2             | 1            | 6               | 321                |
| Australian Collectables, Ltd   | 64,591        | 2,808            | 3         | 2005-05-09      | 2005-05-31     | 22      | 4           | 3             | 1            | 8               | 431                |
| Auto Assoc. & Cie.             | 64,834        | 3,602            | 2         | 2004-10-11      | 2005-05-31     | 232     | 1           | 1             | 1            | 3               | 111                |
| Classic Gift Ideas, Inc        | 67,507        | 3,215            | 2         | 2004-10-14      | 2005-05-31     | 229     | 1           | 1             | 1            | 3               | 111                |
| Osaka Souveniers Co.           | 67,605        | 3,380            | 2         | 2004-04-13      | 2005-05-31     | 413     | 1           | 1             | 1            | 3               | 111                |
| Daedalus Designs Imports       | 69,052        | 3,453            | 2         | 2004-02-21      | 2005-05-31     | 465     | 1           | 1             | 1            | 3               | 111                |
| Alpha Cognac                   | 70,488        | 3,524            | 3         | 2005-03-28      | 2005-05-31     | 64      | 3           | 2             | 2            | 7               | 322                |
| Diecast Collectables           | 70,860        | 3,937            | 2         | 2004-04-26      | 2005-05-31     | 400     | 1           | 2             | 2            | 5               | 122                |
| Quebec Home Shopping Network   | 74,205        | 3,373            | 3         | 2005-05-01      | 2005-05-31     | 30      | 4           | 2             | 2            | 8               | 422                |
| Mini Wheels Co.                | 74,476        | 3,546            | 3         | 2004-11-18      | 2005-05-31     | 194     | 2           | 3             | 2            | 7               | 232                |
| Marseille Mini Autos           | 74,936        | 2,997            | 3         | 2005-01-06      | 2005-05-31     | 145     | 3           | 3             | 2            | 8               | 332                |
| Petit Auto                     | 74,973        | 2,999            | 3         | 2005-05-30      | 2005-05-31     | 1       | 4           | 3             | 2            | 9               | 432                |
| Canadian Gift Exchange Network | 75,239        | 3,420            | 2         | 2004-10-22      | 2005-05-31     | 221     | 2           | 1             | 2            | 5               | 212                |
| Classic Legends Inc.           | 77,795        | 3,890            | 3         | 2004-11-21      | 2005-05-31     | 191     | 2           | 3             | 2            | 7               | 232                |
| giftsbymail.co.uk              | 78,241        | 3,009            | 2         | 2004-11-01      | 2005-05-31     | 211     | 2           | 1             | 2            | 5               | 212                |
| Lyon Souveniers                | 78,570        | 3,929            | 3         | 2005-03-17      | 2005-05-31     | 75      | 3           | 2             | 2            | 7               | 322                |
| Norway Gifts By Mail, Co.      | 79,224        | 3,301            | 2         | 2004-08-21      | 2005-05-31     | 283     | 1           | 2             | 2            | 5               | 122                |
| Super Scale Inc.               | 79,472        | 4,675            | 2         | 2004-05-04      | 2005-05-31     | 392     | 1           | 2             | 2            | 5               | 122                |
| Mini Caravy                    | 80,438        | 4,234            | 3         | 2005-04-14      | 2005-05-31     | 47      | 4           | 2             | 2            | 8               | 422                |
| Collectables For Less Inc.     | 81,578        | 3,399            | 3         | 2005-01-20      | 2005-05-31     | 131     | 3           | 2             | 2            | 7               | 322                |
| Signal Gift Stores             | 82,751        | 2,853            | 3         | 2004-11-29      | 2005-05-31     | 183     | 2           | 3             | 2            | 7               | 232                |
| Gifts4AllAges.com              | 83,210        | 3,200            | 3         | 2005-05-06      | 2005-05-31     | 25      | 4           | 3             | 2            | 9               | 432                |
| Tekni Collectables Inc.        | 83,228        | 3,963            | 3         | 2005-04-03      | 2005-05-31     | 58      | 4           | 2             | 2            | 8               | 422                |
| Motor Mint Distributors Inc.   | 83,682        | 3,638            | 3         | 2004-11-17      | 2005-05-31     | 195     | 2           | 3             | 2            | 7               | 232                |
| Mini Classics                  | 85,556        | 3,291            | 2         | 2004-10-15      | 2005-05-31     | 228     | 2           | 1             | 2            | 5               | 212                |
| Collectable Mini Designs Co.   | 87,489        | 3,500            | 2         | 2004-02-26      | 2005-05-31     | 460     | 1           | 1             | 2            | 4               | 112                |
| Vitachrome Inc.                | 88,041        | 3,522            | 3         | 2004-11-05      | 2005-05-31     | 207     | 2           | 3             | 2            | 7               | 232                |
| Stylish Desk Decors, Co.       | 88,805        | 3,416            | 3         | 2004-12-03      | 2005-05-31     | 179     | 3           | 3             | 2            | 8               | 332                |
| Auto Canal Petit               | 93,171        | 3,451            | 3         | 2005-04-07      | 2005-05-31     | 54      | 4           | 2             | 3            | 9               | 423                |
| Cruz & Sons Co.                | 94,016        | 3,616            | 3         | 2004-11-16      | 2005-05-31     | 196     | 2           | 3             | 3            | 8               | 233                |
| Amica Models & Co.             | 94,117        | 3,620            | 2         | 2004-09-09      | 2005-05-31     | 264     | 1           | 1             | 3            | 5               | 113                |
| La Corne D'abondance, Co.      | 97,204        | 4,226            | 3         | 2004-11-20      | 2005-05-31     | 192     | 2           | 3             | 3            | 8               | 233                |
| FunGiftIdeas.com               | 98,924        | 3,805            | 3         | 2005-03-03      | 2005-05-31     | 89      | 3           | 2             | 3            | 8               | 323                |
| Toms Spezialitten, Ltd         | 100,307       | 3,858            | 2         | 2004-10-16      | 2005-05-31     | 227     | 2           | 1             | 3            | 6               | 213                |
| Heintze Collectables           | 100,596       | 3,726            | 2         | 2004-10-22      | 2005-05-31     | 221     | 2           | 1             | 3            | 6               | 213                |
| Gift Depot Inc.                | 101,895       | 4,076            | 3         | 2005-05-05      | 2005-05-31     | 26      | 4           | 3             | 3            | 10              | 433                |
| Marta's Replicas Co.           | 103,080       | 3,818            | 2         | 2004-10-13      | 2005-05-31     | 230     | 1           | 1             | 3            | 5               | 113                |
| Oulu Toy Supplies, Inc.        | 104,370       | 3,262            | 3         | 2005-01-31      | 2005-05-31     | 120     | 3           | 2             | 3            | 8               | 323                |
| Toys4GrownUps.com              | 104,562       | 3,485            | 3         | 2005-01-12      | 2005-05-31     | 139     | 3           | 2             | 3            | 8               | 323                |
| Mini Creations Ltd.            | 108,951       | 3,113            | 3         | 2005-01-07      | 2005-05-31     | 144     | 3           | 2             | 3            | 8               | 323                |
| Toys of Finland, Co.           | 111,250       | 3,708            | 3         | 2005-02-09      | 2005-05-31     | 111     | 3           | 2             | 3            | 8               | 323                |
| Herkku Gifts                   | 111,640       | 3,850            | 3         | 2004-09-03      | 2005-05-31     | 270     | 1           | 3             | 3            | 7               | 133                |
| Suominen Souveniers            | 113,961       | 3,799            | 3         | 2005-01-06      | 2005-05-31     | 145     | 3           | 4             | 3            | 10              | 343                |
| Handji Gifts& Co               | 115,499       | 3,208            | 4         | 2005-04-23      | 2005-05-31     | 38      | 4           | 4             | 3            | 11              | 443                |
| Baane Mini Imports             | 116,599       | 3,644            | 4         | 2004-11-05      | 2005-05-31     | 207     | 2           | 4             | 3            | 9               | 243                |
| Vida Sport, Ltd                | 117,714       | 3,797            | 2         | 2004-08-30      | 2005-05-31     | 274     | 1           | 2             | 3            | 6               | 123                |
| UK Collectables, Ltd.          | 118,008       | 4,069            | 3         | 2005-04-08      | 2005-05-31     | 53      | 4           | 2             | 3            | 9               | 423                |
| Tokyo Collectables, Ltd        | 120,563       | 3,768            | 4         | 2005-04-22      | 2005-05-31     | 39      | 4           | 4             | 3            | 11              | 443                |
| Technics Stores Inc.           | 120,783       | 3,552            | 4         | 2005-01-05      | 2005-05-31     | 146     | 3           | 4             | 3            | 10              | 343                |
| Diecast Classics Inc.          | 122,138       | 3,940            | 4         | 2005-05-30      | 2005-05-31     | 1       | 4           | 4             | 4            | 12              | 444                |
| Online Diecast Creations Co.   | 131,685       | 3,873            | 3         | 2004-11-04      | 2005-05-31     | 208     | 2           | 3             | 4            | 9               | 234                |
| Scandinavian Gift Ideas        | 134,259       | 3,533            | 3         | 2005-03-03      | 2005-05-31     | 89      | 3           | 2             | 4            | 9               | 324                |
| Reims Collectables             | 135,043       | 3,294            | 5         | 2005-03-30      | 2005-05-31     | 62      | 4           | 4             | 4            | 12              | 444                |
| Rovelli Gifts                  | 137,956       | 2,874            | 3         | 2004-11-12      | 2005-05-31     | 200     | 2           | 3             | 4            | 9               | 234                |
| L'ordine Souveniers            | 142,601       | 3,656            | 3         | 2005-05-10      | 2005-05-31     | 21      | 4           | 3             | 4            | 11              | 434                |
| Saveley & Henriot, Co.         | 142,874       | 3,485            | 3         | 2004-03-02      | 2005-05-31     | 455     | 1           | 3             | 4            | 8               | 134                |
| Danish Wholesale Imports       | 145,042       | 4,029            | 5         | 2005-04-15      | 2005-05-31     | 46      | 4           | 4             | 4            | 12              | 444                |
| Salzburg Collectables          | 149,799       | 3,745            | 4         | 2005-05-17      | 2005-05-31     | 14      | 4           | 4             | 4            | 12              | 444                |
| Corporate Gift Ideas Co.       | 149,883       | 3,656            | 4         | 2005-02-23      | 2005-05-31     | 97      | 3           | 4             | 4            | 11              | 344                |
| Souveniers And Things Co.      | 151,571       | 3,295            | 4         | 2005-05-29      | 2005-05-31     | 2       | 4           | 4             | 4            | 12              | 444                |
| Anna's Decorations, Ltd        | 153,996       | 3,348            | 4         | 2005-03-09      | 2005-05-31     | 83      | 3           | 4             | 4            | 11              | 344                |
| AV Stores, Co.                 | 157,808       | 3,094            | 3         | 2004-11-17      | 2005-05-31     | 195     | 2           | 3             | 4            | 9               | 234                |
| The Sharp Gifts Warehouse      | 160,010       | 4,000            | 4         | 2005-04-22      | 2005-05-31     | 39      | 4           | 4             | 4            | 12              | 444                |
| Land of Toys Inc.              | 164,069       | 3,348            | 4         | 2004-11-15      | 2005-05-31     | 197     | 2           | 4             | 4            | 10              | 244                |
| Dragon Souveniers, Ltd.        | 172,990       | 4,023            | 5         | 2005-03-02      | 2005-05-31     | 90      | 3           | 4             | 4            | 11              | 344                |
| La Rochelle Gifts              | 180,125       | 3,399            | 4         | 2005-05-31      | 2005-05-31     | 0       | 4           | 4             | 4            | 12              | 444                |
| Muscle Machine Inc             | 197,737       | 4,120            | 4         | 2004-12-01      | 2005-05-31     | 181     | 2           | 4             | 4            | 10              | 244                |
| Australian Collectors, Co.     | 200,995       | 3,654            | 5         | 2004-11-29      | 2005-05-31     | 183     | 2           | 4             | 4            | 10              | 244                |
| Mini Gifts Distributors Ltd.   | 654,858       | 3,638            | 17        | 2005-05-29      | 2005-05-31     | 2       | 4           | 4             | 4            | 12              | 444                |
| Euro Shopping Channel          | 912,294       | 3,522            | 26        | 2005-05-31      | 2005-05-31     | 0       | 4           | 4             | 4            | 12              | 444                |

_CUSTOMERNAME:_ The name of the customer or company.

_MONETARYVALUE:_ Total monetary value spent by the customer on purchases.

_AVGMONETARYVALUE:_ Average monetary value per purchase.

_FREQUENCY:_ Number of distinct orders made by the customer.

_LAST_ORDER_DATE:_ Date of the last order made by the customer.

_MAX_ORDER_DATE:_ Maximum date of orders within the dataset.

_RECENCY:_ The number of days since the last order was made.

_RFM_RECENCY:_ Recency score segment (1 to 4) based on quartiles.

_RFM_FREQUENCY:_ Frequency score segment (1 to 4) based on quartiles.

_RFM_MONETARY:_ Monetary score segment (1 to 4) based on quartiles.

_RFM_TOTAL_SCORE:_ Total RFM score, sum of recency, frequency, and monetary scores.

_RFM_SCORE_CATEGORY:_ Combined RFM score category, formed by concatenating recency, frequency, and monetary scores.

The RFM segmentation is utilized to group customers into segments based on their purchasing behavior. Higher RFM scores generally indicate more valuable customers who have made recent, frequent, and high-value purchases.

### RFM Customer Segmentation
```sql
-- Count the number of unique RFM score categories
select count (distinct rfm_score_category) from rfm_segment; -- 36 unique rfm scores

-- Select customer information and assign customer segments based on RFM scores
select CUSTOMERNAME , rfm_recency, rfm_frequency, rfm_monetary,
	case 
		when rfm_score_category in (111, 112, 121, 122, 123, 132, 211, 212, 114, 141) then 'lost_customers'  -- Customers with low recency and low frequency, lost customers
		when rfm_score_category in (133, 134, 143, 244, 334, 343, 344, 144) then 'slipping_away' -- High spenders who haven't purchased lately, slipping away
		when rfm_score_category in (311, 411, 331) then 'new_customers' -- New customers with recent purchases
		when rfm_score_category in (222, 231, 221,  223, 233, 322) then 'potential_churners' -- Customers showing signs of churn risk
		when rfm_score_category in (323, 333,321, 341, 422, 332, 432) then 'active_customers' -- Active customers with frequent but lower-value purchases
		when rfm_score_category in (433, 434, 443, 444) then 'loyal_customers' -- Loyal customers with high recency, frequency, and monetary value
        else 'Other' -- Other customer segment
	end as Customer_Segment
from rfm_segment;
```
### Results:
| CUSTOMERNAME                   | RFM_RECENCY | RFM_FREQUENCY | RFM_MONETARY | CUSTOMER_SEGMENT           |
|--------------------------------|-------------|---------------|--------------|----------------------------|
| Boards & Toys Co.              | 3           | 1             | 1            | new customers              |
| Atelier graphique              | 2           | 3             | 1            | potential churners         |
| Auto-Moto Classics Inc.        | 2           | 2             | 1            | potential churners         |
| Microscale Inc.                | 2           | 1             | 1            | lost_customers             |
| Royale Belge                   | 3           | 4             | 1            | active                     |
| Double Decker Gift Stores, Ltd | 1           | 1             | 1            | lost_customers             |
| Cambridge Collectables Co.     | 1           | 2             | 1            | lost_customers             |
| West Coast Collectables Co.    | 1           | 1             | 1            | lost_customers             |
| Men 'R' US Retailers, Ltd.     | 1           | 1             | 1            | lost_customers             |
| CAF Imports                    | 1           | 1             | 1            | lost_customers             |
| Signal Collectibles Ltd.       | 1           | 1             | 1            | lost_customers             |
| Mini Auto Werke                | 3           | 2             | 1            | active                     |
| Iberia Gift Imports, Corp.     | 1           | 1             | 1            | lost_customers             |
| Online Mini Collectables       | 1           | 1             | 1            | lost_customers             |
| Gift Ideas Corp.               | 3           | 3             | 1            | new customers              |
| Clover Collections, Co.        | 1           | 1             | 1            | lost_customers             |
| Australian Gift Network, Co    | 3           | 2             | 1            | active                     |
| Australian Collectables, Ltd   | 4           | 3             | 1            | Other                      |
| Auto Assoc. & Cie.             | 1           | 1             | 1            | lost_customers             |
| Classic Gift Ideas, Inc        | 1           | 1             | 1            | lost_customers             |
| Osaka Souveniers Co.           | 1           | 1             | 1            | lost_customers             |
| Daedalus Designs Imports       | 1           | 1             | 1            | lost_customers             |
| Alpha Cognac                   | 3           | 2             | 2            | potential churners         |
| Diecast Collectables           | 1           | 2             | 2            | lost_customers             |
| Quebec Home Shopping Network   | 4           | 2             | 2            | active                     |
| Mini Wheels Co.                | 2           | 3             | 2            | Other                      |
| Marseille Mini Autos           | 3           | 3             | 2            | active                     |
| Petit Auto                     | 4           | 3             | 2            | active                     |
| Canadian Gift Exchange Network | 2           | 1             | 2            | lost_customers             |
| Classic Legends Inc.           | 2           | 3             | 2            | Other                      |
| giftsbymail.co.uk              | 2           | 1             | 2            | lost_customers             |
| Lyon Souveniers                | 3           | 2             | 2            | potential churners         |
| Norway Gifts By Mail, Co.      | 1           | 2             | 2            | lost_customers             |
| Super Scale Inc.               | 1           | 2             | 2            | lost_customers             |
| Mini Caravy                    | 4           | 2             | 2            | active                     |
| Collectables For Less Inc.     | 3           | 2             | 2            | potential churners         |
| Signal Gift Stores             | 2           | 3             | 2            | Other                      |
| Gifts4AllAges.com              | 4           | 3             | 2            | active                     |
| Tekni Collectables Inc.        | 4           | 2             | 2            | active                     |
| Motor Mint Distributors Inc.   | 2           | 3             | 2            | Other                      |
| Mini Classics                  | 2           | 1             | 2            | lost_customers             |
| Collectable Mini Designs Co.   | 1           | 1             | 2            | lost_customers             |
| Vitachrome Inc.                | 2           | 3             | 2            | Other                      |
| Stylish Desk Decors, Co.       | 3           | 3             | 2            | active                     |
| Auto Canal Petit               | 4           | 2             | 3            | Other                      |
| Cruz & Sons Co.                | 2           | 3             | 3            | potential churners         |
| Amica Models & Co.             | 1           | 1             | 3            | Other                      |
| La Corne D'abondance, Co.      | 2           | 3             | 3            | potential churners         |
| FunGiftIdeas.com               | 3           | 2             | 3            | active                     |
| Toms Spezialitten, Ltd         | 2           | 1             | 3            | Other                      |
| Heintze Collectables           | 2           | 1             | 3            | Other                      |
| Gift Depot Inc.                | 4           | 3             | 3            | loyal                      |
| Marta's Replicas Co.           | 1           | 1             | 3            | Other                      |
| Oulu Toy Supplies, Inc.        | 3           | 2             | 3            | active                     |
| Toys4GrownUps.com              | 3           | 2             | 3            | active                     |
| Mini Creations Ltd.            | 3           | 2             | 3            | active                     |
| Toys of Finland, Co.           | 3           | 2             | 3            | active                     |
| Herkku Gifts                   | 1           | 3             | 3            | slipping away, cannot lose |
| Suominen Souveniers            | 3           | 4             | 3            | slipping away, cannot lose |
| Handji Gifts& Co               | 4           | 4             | 3            | loyal                      |
| Baane Mini Imports             | 2           | 4             | 3            | Other                      |
| Vida Sport, Ltd                | 1           | 2             | 3            | lost_customers             |
| UK Collectables, Ltd.          | 4           | 2             | 3            | Other                      |
| Tokyo Collectables, Ltd        | 4           | 4             | 3            | loyal                      |
| Technics Stores Inc.           | 3           | 4             | 3            | slipping away, cannot lose |
| Diecast Classics Inc.          | 4           | 4             | 4            | loyal                      |
| Online Diecast Creations Co.   | 2           | 3             | 4            | Other                      |
| Scandinavian Gift Ideas        | 3           | 2             | 4            | Other                      |
| Reims Collectables             | 4           | 4             | 4            | loyal                      |
| Rovelli Gifts                  | 2           | 3             | 4            | Other                      |
| L'ordine Souveniers            | 4           | 3             | 4            | loyal                      |
| Saveley & Henriot, Co.         | 1           | 3             | 4            | slipping away, cannot lose |
| Danish Wholesale Imports       | 4           | 4             | 4            | loyal                      |
| Salzburg Collectables          | 4           | 4             | 4            | loyal                      |
| Corporate Gift Ideas Co.       | 3           | 4             | 4            | slipping away, cannot lose |
| Souveniers And Things Co.      | 4           | 4             | 4            | loyal                      |
| Anna's Decorations, Ltd        | 3           | 4             | 4            | slipping away, cannot lose |
| AV Stores, Co.                 | 2           | 3             | 4            | Other                      |
| The Sharp Gifts Warehouse      | 4           | 4             | 4            | loyal                      |
| Land of Toys Inc.              | 2           | 4             | 4            | slipping away, cannot lose |
| Dragon Souveniers, Ltd.        | 3           | 4             | 4            | slipping away, cannot lose |
| La Rochelle Gifts              | 4           | 4             | 4            | loyal                      |
| Muscle Machine Inc             | 2           | 4             | 4            | slipping away, cannot lose |
| Australian Collectors, Co.     | 2           | 4             | 4            | slipping away, cannot lose |
| Mini Gifts Distributors Ltd.   | 4           | 4             | 4            | loyal                      |
| Euro Shopping Channel          | 4           | 4             | 4            | loyal                      |


