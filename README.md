# Music Store Analysis with SQL  

## Project Overview  
This project uses SQL to analyze a music store's database to gain insights into customer behavior, sales trends, and the most popular music tracks, genres, and artists. The analysis helps answer key business questions and supports decision-making for promotional strategies.  

## Features  
1. Identify the senior-most employee based on job titles.  
2. Analyze the number of invoices by country to find the most active markets.  
3. Determine the cities with the highest revenue generation for targeted promotions.  
4. Identify top customers who spend the most.  
5. Find the most popular music genres and top artists in the dataset.  
6. Explore track duration patterns and find tracks longer than the average duration.  
7. Analyze customer spending by artist.  
8. Discover the most popular genre in each country.  
9. Identify top-spending customers by country.  

## Dataset Structure  
The database contains the following key tables:  
- **Employee**: Information about employees and their job levels.  
- **Invoice**: Details of customer purchases, including total amounts and billing details.  
- **Customer**: Personal details of customers, including email and location.  
- **Track**: Information on music tracks, including genre and duration.  
- **Genre**: Details about music genres.  
- **Artist and Album**: Information about artists and their albums.  

## SQL Queries  

1. **Senior-most Employee**  
   Find the senior-most employee based on job levels.  

   ```sql
   SELECT * 
   FROM employee 
   ORDER BY levels DESC 
   LIMIT 1;  ```

2. **Countries with Most Invoices**
 Find the countries with the most invoices
```sql
SELECT COUNT(*) AS c, billing_country 
FROM invoice 
GROUP BY billing_country 
ORDER BY c DESC;
```







