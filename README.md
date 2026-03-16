# Sql-Data-Cleaning-
Sql Data Cleaning EDA Process

```sql

use world_layoffs;
SELECT 
    *
FROM
    layoffs;
    
-- remove duplicates
-- standarized the data
-- null vlaues or blank values
-- remove any column

CREATE TABLE staging_layoffs LIKE layoffs;

insert into staging_layoffs
select * from layoffs;

SELECT 
    *
FROM
    staging_layoffs;



with duplicate_cte as(select *, row_number() over(partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, stage, country, funds_raised_millions) as row_num
from staging_layoffs
)
delete from duplicate_cte
where row_num>1;

CREATE TABLE `staging_layoffs2` (
  `company` text,
  `location` text,
  `industry` text,
  `total_laid_off` int DEFAULT NULL,
  `percentage_laid_off` text,
  `date` text,
  `stage` text,
  `country` text,
  `funds_raised_millions` int DEFAULT NULL,
  `row_num` int
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

SELECT 
    *
FROM
    STAGING_LAYOFFS2;

INSERT INTO staging_layoffs2
select * , row_number() over(partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, stage, country, funds_raised_millions) as row_num
from staging_layoffs;

delete from staging_layoffs2
where row_num>1;

select company, trim(company)
from staging_layoffs2;

update staging_layoffs2
set company= trim(company);

select distinct industry
from staging_layoffs2;



 
