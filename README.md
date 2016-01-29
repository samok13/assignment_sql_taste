# assignment_sql_taste
A delicious appetizer of SQL-ey goodness


#################################

tutorial.us_housing_units

10 results with information on all columns
SELECT *
  FROM tutorial.us_housing_units 
LIMIT 10

Housing starts in the Midwest
SELECT 
  year, 
  month, 
  month_name, 
  midwest
  FROM tutorial.us_housing_units 

All housing starts in December since 1985
SELECT *
 FROM tutorial.us_housing_units 
WHERE year >= 1985
AND month_name LIKE 'December' 

All housing starts in the second half of the year since 1990
SELECT *
 FROM tutorial.us_housing_units 
WHERE year >= 1990
AND month >= 6 

All rows where housing starts were above 30,000 in the South region
SELECT south
 FROM tutorial.us_housing_units 
WHERE south >= 30

The sum of housing starts across all regions for each row
SELECT 
  year, 
  month,
  month_name,
  west, 
  south, 
  midwest,
  northeast,
  south + west + midwest + northeast AS sum
 FROM tutorial.us_housing_units 

All rows where the sum of all housing starts is above 70,000 Note: You can't use an alias in a WHERE clause.
SELECT 
  year, 
  month,
  month_name,
  west, 
  south, 
  midwest,
  northeast,
  south + west + midwest + northeast AS sum
 FROM tutorial.us_housing_units 
WHERE (south + west + midwest + northeast) >= 70

All rows where the sum of all housing starts is between 50-80k
SELECT 
  year, 
  month,
  month_name,
  west, 
  south, 
  midwest,
  northeast,
  south + west + midwest + northeast AS sum
 FROM tutorial.us_housing_units 
WHERE (south + west + midwest + northeast) BETWEEN 50 AND 80

The average of all housing starts across all regions for each row
SELECT 
  year, 
  month,
  month_name,
  west,
  south, 
  midwest,
  northeast,
  (south + west + midwest + northeast)/4 AS ave
 FROM tutorial.us_housing_units 

All rows where the housing starts in the South are above the sum of the other three regions
SELECT 
  year, 
  month,
  month_name,
  west,
  south, 
  midwest,
  northeast
 FROM tutorial.us_housing_units 
WHERE south > (northeast + west + midwest)

The percentage of housing starts that occur in each region since 1990 Note: Use an alias to title the new columns appropriately

Should I sum the columns?

SELECT 
  year, 
  month,
  month_name,
  west,
  west/(west+south+midwest+northeast)*100 AS westperc,
  south, 
  south/(west+south+midwest+northeast)*100 AS southperc,
  midwest,
  midwest/(west+south+midwest+northeast)*100 AS midwestperc,
  northeast,
  northeast/(west+south+midwest+northeast)*100 AS northeastperc
 FROM tutorial.us_housing_units 
WHERE year >= 1990

#########################################

tutorial.billboard_top_100_year_end

Note: Use single quotes ' instead of double quotes " for LIKE and similar queries since the Mode tool is very particular about its syntax. Double quotes are used to specify column names, so you might get a "column XYZ does not exist" error if you mess this up.

All rows where Elvis Presley had a song on the top 100 charts
SELECT *
  FROM tutorial.billboard_top_100_year_end
WHERE artist LIKE 'Elvis Presley'

All rows where the artist's name contained "Tony" (not case sensitive)
SELECT *
  FROM tutorial.billboard_top_100_year_end
WHERE artist ILIKE '%tony%'

All rows where the song title contained the word "love" in any way
SELECT *
  FROM tutorial.billboard_top_100_year_end
WHERE song_name ILIKE '%love%'

All rows where the artist's name begins with the letter "A"
SELECT *
  FROM tutorial.billboard_top_100_year_end
WHERE artist ILIKE 'A%'

The top 3 songs from each year between 1960-1969
SELECT *
  FROM tutorial.billboard_top_100_year_end
WHERE year_rank <= 3
AND year BETWEEN 1960 AND 1960


All rows where either Elvis Presley, The Rolling Stones, or Van Halen were the artist
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist IN ('Rolling Stones', 'Van Halen', 'Elvis Presley')


All rows from 1970 where the songs were ranked 10-20th
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year = 1970
 AND year_rank BETWEEN 10 AND 20

All rows from the 1990's where Madonna was not ranked 10-100th
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year BETWEEN 1990 AND 1999
 AND artist LIKE 'Madonna'
 AND year_rank BETWEEN 1 and 9

All rows from 1985 which do not include Madonna or Phil Collins in the group.
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year = 1985
 AND "group" NOT LIKE 'Madonna' AND "group" NOT LIKE 'Phil Collins'

All number 1 songs in the data set.
SELECT song_name
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank = 1

All rows where the artist is not listed
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist IS NULL

All of Madonna's top 100 hits ordered by their ranking (1 to 100)
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist LIKE 'Madonna'
 ORDER BY year_rank

All of Madonna's top 100 hits ordered by their ranking within each year
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist LIKE 'Madonna'
 ORDER BY year, year_rank

Every number 1 song since 1990 followed by every number 2 song since 1990 and number 3 song since 1990. (Hint: Multiple ordering)
SELECT *
  FROM tutorial.billboard_top_100_year_end
WHERE year >= 1990
AND year_rank <= 3
ORDER BY year_rank, year

#########################################
Intermediate--STILL TO DO
tutorial.billboard_top_100_year_end

Which artist has had the most appearances on the top 100 list?

SELECT artist, COUNT(artist) AS artist_count
  FROM tutorial.billboard_top_100_year_end
GROUP BY artist
ORDER BY artist_count DESC

Which artist has had the most #1 hits? How many?

SELECT artist, COUNT(artist) AS artist_at_num_1
  FROM tutorial.billboard_top_100_year_end
WHERE year_rank = 1
GROUP BY artist
ORDER BY artist_count DESC

What is the highest position ever reached by Phil Collins?

What is the average position reached by Michael Jackson?
Madonna's average position when she actually reached the top 10
List the top 10 artists based on their number of appearances on this list (and what that number is) since 1985
The total count of top 10 hits written by either Elvis, Madonna, the Beatles, or Elton John



