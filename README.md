# 🌍Global Layoff Analysis with SQL📊
This project showcases my ability to perform comprehensive data exploration and analysis using MySQL. It highlights my proficiency in querying, and analyzing datasets to uncover meaningful insights that drive decision-making.
# 📋Project Overview
This project leverages SQL to uncover critical insights into global layoffs 📊, highlighting affected industries 🏭, top companies 🏢, and temporal trends 📅. It provides actionable intelligence for business professionals 💼, HR strategists 🧑‍💻, and policymakers 🏛️ to address workforce challenges. With layoffs being a pressing global issue 🌍, this analysis is vital for understanding economic and industry trends, aiding in better hiring and retention strategies ✅.
# 🛠Tool Used
- MySQL [Download Here](MySQL.com)

# 🔍📊 Exploratory Data Analysis (EDA)

#### 1. Find the maximum laid off and maximum percentage laid off.

``` SQL
SELECT *
	FROM layoffs_staging2;

	SELECT MAX(total_laid_off),
	MAX(percentage_laid_off)
	FROM layoffs_staging2;
```

#### 2. Retrieve records where all employees have been laid off and order the results by the total layoffs in descending order.

``` SQL
SELECT *
	FROM layoffs_staging2
	WHERE percentage_laid_off = 1
	ORDER BY total_laid_off DESC;
```

#### 3.  Retrieve records where all employees have been laid off and order the results by funds raised in descending order.

``` SQL
SELECT *
	FROM layoffs_staging2
	WHERE percentage_laid_off = 1
	ORDER BY funds_raised_millions DESC;
```

#### 4. Retreive the total number of layoffs for each company, sums them up, and sorts the results by the total number of layoffs fro each company in descending order.

``` SQL
SELECT company, sum(total_laid_off) AS sum_total
	FROM layoffs_staging2
	GROUP BY company
	ORDER BY 2 DESC;
```

#### 5. Find the earliest and latest dates from the layoffs_staging2 table.

```SQL
SELECT MIN(`date`), MAX(`date`)
	FROM layoffs_staging2;
```

#### 6. Retreive the total number of layoffs for each industry, sums them up, and sorts the results by the total number of layoffs for each industry in descending order.

``` SQL
SELECT industry, sum(total_laid_off) AS sum_total
	FROM layoffs_staging2
	GROUP BY industry
	ORDER BY 2 DESC;
```

#### 7. Retreive the total number of layoffs for each country, sums them up, and sorts the results by the total number of layoffs for each country in descending order.

``` SQL
SELECT country, sum(total_laid_off) AS sum_total
	FROM layoffs_staging2
	GROUP BY country
	ORDER BY 2 DESC;
```

#### 8. Retreive the total number of layoffs for each year, grouped by year, and sorts the results in descending order of the year.

``` SQL
SELECT year(`date`), sum(total_laid_off) AS sum_total
	FROM layoffs_staging2
	GROUP BY year(`date`)
	ORDER BY 1 DESC;
```

#### 9. calculate the total number of layoffs for each stage and sorts the results by stage in descending order.

``` SQL
SELECT stage, sum(total_laid_off) AS sum_total
	FROM layoffs_staging2
	GROUP BY stage
	ORDER BY 1 DESC;
```
#### 10. Find the total layoffs for each month, excluding rows with a null date, and sorts the results by the month in ascending order.

``` SQL
 SELECT SUBSTRING(`date`, 1,7) AS `month`, SUM(total_laid_off)
	FROM layoffs_staging2
    WHERE SUBSTRING(`date`, 1,7) IS NOT NULL
    GROUP BY `month`
    ORDER BY 1 ASC;
```

#### 11. calculate the total layoffs for each month, then add a running total of layoffs by month.

``` SQL
WITH rolling_total AS
    (
 SELECT SUBSTRING(`date`,1,7) AS `month`, SUM(total_laid_off) AS total_off
	FROM layoffs_staging2
    WHERE SUBSTRING(`date`,1,7) IS NOT NULL
    GROUP BY `month`
    ORDER BY 1 ASC
    )
    SELECT `month`, total_off,
    SUM(total_off) OVER(ORDER BY `month`) AS rolling_total
    FROM rolling_total;
```

#### 12. Retrieve the company names, the year of the layoff, and the total number of layoffs for each company, then group the data by company and year, and sort the results by the total number of layoffs in descending order.

``` SQL
SELECT company, year(`date`) AS `year`, sum(total_laid_off) AS sum_total
    FROM layoffs_staging2
     GROUP BY company, year(`date`)
     ORDER BY 3 DESC;
```

#### 13. Calculate the total layoffs by company and year, rank the companies by the number of layoffs for each year, and retrieve the top 5 companies with the most layoffs for each year.

``` SQL

 SELECT company, year(`date`) AS `year`, sum(total_laid_off) AS sum_total
    FROM layoffs_staging2
     GROUP BY company, year(`date`)
     ORDER BY 3 DESC;
     
      WITH company_year (company, `year`, total_laid_off) AS
     (
      SELECT company, year(`date`) AS `year`, sum(total_laid_off) AS sum_total
    FROM layoffs_staging2
     GROUP BY company, year(`date`)
     ), cte_final AS
     (
     SELECT *,
     DENSE_RANK() OVER (PARTITION BY `year` ORDER BY total_laid_off DESC) AS `rank`
     FROM company_year
     WHERE `year` IS NOT NULL
     )
     SELECT *
     FROM cte_final
     WHERE `rank` <= 5;
     ```
    

    

