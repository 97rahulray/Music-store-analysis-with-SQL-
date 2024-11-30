# Music Store Analysis with SQL  
![Music Logo] [https://github.com/97rahulray/Music-store-analysis-with-SQL-/blob/main/Apple%20Music%20App.png]

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
3.  **Top 3 Invoice Totals**
   Retrieve the top three invoice totals.

```sql
SELECT total 
FROM invoice 
ORDER BY total DESC 
LIMIT 3;  
```
4.**City with Highest Revenue**
Find the city with the highest revenue.

```sql
SELECT SUM(total) AS invoice_total, billing_city 
FROM invoice 
GROUP BY billing_city 
ORDER BY invoice_total DESC;  
```
5.**Best Customer**
Identify the customer who spent the most money.

```sql
SELECT customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) AS total 
FROM customer 
JOIN invoice ON customer.customer_id = invoice.customer_id 
GROUP BY customer.customer_id 
ORDER BY total DESC 
LIMIT 1;  
```
6.**Rock Music Listeners**
Find all customers who purchased Rock music tracks.

```sql
SELECT DISTINCT email, first_name, last_name 
FROM customer 
JOIN invoice ON customer.customer_id = invoice.customer_id 
JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id 
WHERE track_id IN ( 
    SELECT track_id 
    FROM track 
```
7.**Top Rock Artists**
Identify the top 10 artists with the most Rock tracks.

```sql
SELECT artist.artist_id, artist.name, COUNT(artist.artist_id) AS number_of_songs 
FROM track 
JOIN album ON album.album_id = track.album_id 
JOIN artist ON artist.artist_id = album.artist_id 
JOIN genre ON genre.genre_id = track.genre_id 
WHERE genre.name LIKE 'Rock' 
GROUP BY artist.artist_id 
ORDER BY number_of_songs DESC 
LIMIT 10;  

```
8.**Tracks Longer Than Average**
Find tracks longer than the average duration.

```sql
SELECT name, milliseconds 
FROM track 
WHERE milliseconds > ( 
    SELECT AVG(milliseconds) AS avg_track_length 
    FROM track 
) 
ORDER BY milliseconds DESC;  

```
9.**Spending by Customers on Artists**
Analyze customer spending by artist.

```sql
WITH best_selling_artist AS ( 
    SELECT artist.artist_id AS artist_id, artist.name AS artist_name, SUM(invoice_line.unit_price * invoice_line.quantity) AS total_sales 
    FROM invoice_line 
    JOIN track ON track.track_id = invoice_line.track_id 
    JOIN album ON album.album_id = track.album_id 
    JOIN artist ON artist.artist_id = album.artist_id 
    GROUP BY 1 
    ORDER BY 3 DESC 
    LIMIT 1 
) 
SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name, SUM(il.unit_price * il.quantity) AS amount_spent 
FROM invoice i 
JOIN customer c ON c.customer_id = i.customer_id 
JOIN invoice_line il ON il.invoice_id = i.invoice_id 
JOIN track t ON t.track_id = il.track_id 
JOIN album alb ON alb.album_id = t.album_id 
JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id 
GROUP BY 1, 2, 3, 4 
ORDER BY 5 DESC;  
```
10.**Popular Genre by Country**
Find the most popular genre in each country.

```sql
WITH popular_genre AS ( 
    SELECT COUNT(invoice_line.quantity) AS purchases, customer.country, genre.name, genre.genre_id, 
    ROW_NUMBER() OVER (PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity) DESC) AS RowNo 
    FROM invoice_line 
    JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id 
    JOIN customer ON customer.customer_id = invoice.customer_id 
    JOIN track ON track.track_id = invoice_line.track_id 
    JOIN genre ON genre.genre_id = track.genre_id 
    GROUP BY 2, 3, 4 
    ORDER BY 2 ASC, 1 DESC 
) 
SELECT * 
FROM popular_genre 
WHERE RowNo <= 1;  
```
11.**Top Customer by Country**
Identify the customer who spent the most in each country.

```sql
WITH Customer_with_country AS ( 
    SELECT customer.customer_id, first_name, last_name, billing_country, SUM(total) AS total_spending, 
    ROW_NUMBER() OVER (PARTITION BY billing_country ORDER BY SUM(total) DESC) AS RowNo 
    FROM invoice 
    JOIN customer ON customer.customer_id = invoice.customer_id 
    GROUP BY 1, 2, 3, 4 
    ORDER BY 4 ASC, 5 DESC 
) 
SELECT * 
FROM Customer_with_country 
WHERE RowNo <= 1;  
```










