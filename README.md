# DataCleaningProject
I've done this project in order to excercise Data Cleaning Through SQL which was published and created by Shuki Molk. 
I've added a link to the excercise and now I'm starting with the description that Shuki published on a LinkedIn post:

------------------------------------------------------------------------------------------------------------------------------------------------------------
"TL;DR - Free practice in anomaly detection.
As you probably know, one of the fundamental and most important tasks when working with data is to identify errors, 
mismatches, anomalies, and any other form of contamination.
Now, one of the issues with practice exercises, is the data usually comes clean and ready to use 
(and rightfully so, assuming the goal is to practice a subject that is not anomaly detection and data cleansing). 
However, the table in the following link, well... I'm happy to say it's a horrible table full of many errors.
I originally created this exercise for the current class of Upscale Analytics, but I enjoyed working on it so much, that I'm happy to share it with you too.
Enjoy!
Shuki"
------------------------------------------------------------------------------------------------------------------------------------------------------------

Now after the excercise introduction I'm writing the comments I've written throughout the SQL in order to explain each query and syntax:

-- Intro part 1 --

-- Before writing the code itself I will start with understanding the data and its components--
-- How many rows, different departments and duplicates there are --

				select * from dbo.DataErrors

				---- 859 rows --

				select distinct id from dbo.DataErrors

				---- 854 rows --

				select distinct productname from dbo.DataErrors

				---- 727 rows --

				select distinct departmentname from dbo.DataErrors

				-- 8 rows of departments overall: Meat and Poultry, Fruits, Fish, Bakery, Packaged Foods, Beverages, Dairy, Vegetables

----------------------------------------------------------------------------------------------------

-- After the primal data inquiry I've decided to divide this project into missions in order to work properly --
-- Each section will be labels as a mission --

--			Missions:

--			1. Finding distinct ids and productnames and validating all values are indeed distinct --
--			2. Finding the product name for each product in the list and adding them to our main table --
--			3. Making a reference according to a real accurate groceries list from online webpages --
--			4. Changing the department names according to product names for the departments we already have --
--			5. Changing the department names accoring to the main product names which have been found on stage 2 --
--			6. Validating the correct department name for each product and validating all modifications are correct --
--			7: Validating there are no duplications of products with the same trxdate --
--			8: Adding the real USD/NIS rates and the accurate trxdate values to our final table --

-----------------------------------------------------------------------------------------------------
-- Mission 1a: checking for null values throughout the table and adding a table for optional new ID values --

-- Before editing the data I built a new table in which all editing will be done, apart from the original table --
-- First of all we need to ensure there are no null values in the new table we're building --

-- From the first inquiry of the valuse in the tables I found out there is only 1 null value which will be omitted afterwards --
-- Besides that only product no. 627 has null values in the trxdate column, though we might be able to find this value by extracting the excahnge rate of this product --
-- Later on we will compare it to other products with the same rate and weekday and find out that way the rexdate --

select min(id), max(id) from dbo.DataCorrections2
min_id=1, max_id=1000

---- I created another table which contains all the numbers from 1 to 1000. --
---- Since there are 854 distinct ids between 1 and 1000 it means there are many missing numbers in the ID column --

-----------------------------------------------------------------------------------------------------

-- Mission 1b: finding distinct ids and products with duplications --
-- This stage is needed for removing or re-editing duplications and preventing a situation of a duplicated row and / or id number  --

---- First quering of the duplicated ids - realizing which ids are recurring and whether they referring to the same product --

-- Finding the product names for the duplicated ids we found in the previous table --

---- Throughout this query I found out the excat id duplications and product names in the original table -- 
---- For example, the product 'Wine - Prosecco Valdobiaddene' has 2 ids: 156 and 689 -- 

---- In order to save further work and eliminate duplications which I found out very early I will update new ids right now --
---- After those changes I will validate those product names and numbers are unique --

-------------------------------------------------------------------------------------------------------
						
-- the product 'Wine - Prosecco Valdobiaddene' already has a distinct id --
-- therefore I created a new id for the second product with the same name --

----------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------

-- Mission 2 --

-- In this part I've found out which product names have duplicates and which not --
-- I have also seperated between the main product names and the whole product names --

-----------------------------------------------------------------------------------------------------

-- Mission 2b: Adding the main names of each product to another table which will be used afterwards -- 


-----------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------

-- Mission 3: Updating the departments of each product according to a real grocery list downloading from the internet --

-- Throughout this mission there is a validation check needed to find out where there is no product name --
-- Since there are only 40 out of 873 rows - which is less than 5 percent - I find it ok to accept this gap --
-- The final re-editing will be done in the next stage when each product will be dealt specifically -- 

-- For 245 out of 872 rows there is a match between the original table and the table I've uploaded --
-- It doesn't solve the product department issue complete but it can make some of the work easier for next stages --

-----------------------------------------------------------------------------------------------------

-- Mission 3b: Creating the main table which will be used in order to modify the department names afterwards --

-- There are 874 rows in the new table I've created -- 
-- There are some duplications in this table because some products have more than one possible department --
-- Later on I will drop out the irrelevant rows after validating the correct department --

------------------------------------------------------------------------------------------

-- Mission 3 validating stage --

-- Validating the main columns in the final table are the same as in the original table --
-- This validation is needed to show there are no duplications of ids in both tables --

--------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------

-- Mission 4a: Narrowing the duplications for products with more than one possible department --

-- from this validation check for most of the ingredients we can see there are ids and products that have 2 departments --
-- this is needed because each product needs to be checked individually and not just up to the main product name --
-- Another result of this check it that those duplications only apply to 3 departments --
-- It will make us easier to handle that in a leter stage --

-------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------

-- Now I'm going to find out whether there are products whose names needs adjusting and correcting --
-- The first stage will be validating the department names I've added from GroceryList table are correct --

-- since the meat is one of the double departments I'm first handling the products which have double departments --

-- Upon checking on the internet for the exact pictures of each product I found out that 1 product doesn't fit --
-- The product 'Chicken - Soup Base' doesn't belong in the meat department, but all 11 other products do belong in the meat department --

-- 12 rows were added into #classified_ids_a --
-- In order to avoid a double work I created another table which contains all the IDs I've already classified. --
-- In that way I can assure I will not overrun the work I've done in this stage --

--------------------------------------------------------------------------------------

-- Now I'm doing the same check of doubles I found in the meat department for fish department --

-- After a quick checking i found out all of those products are under the definition of fish --

---------------------------------------------------------------------------------------

-- Mission 4b: Validating product names are accurate for each department we haven't checked in Mission 4a --

-- Validating for packaged foods department --

-- Right now I am validating the values currently under 'packaged foods' department are accurate  --
-- After that validation I found out 5 ids need to be re-classified as vegetables --

-- In order to avoid a double work I created another table which contains all the IDs I've already classified. --
-- In that way I can assure I will not overrun the work I've done in this stage --


-- Mission 4 Final Validation: Finding out which row numbers should be handled manually --
-- This table will be used to avoid updating those rows in the next stage --

--------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------

-- Mission 5: Changing the product names according to the main product names found on mission 2 --

-- This is the stage where I classify most of the products according to the main product's name --
-- Although for some product it can't be for sure it is way more efficient than going manually for each product --
-- There I will test for each main product 2 or 3 samples and decide to classify upon those samples --
-- For example, this first main product - Appetizer - has many departments so I won't classify it now but later --
-- The rest of the checking and validation will be done afterwards at the letters stage --
-- Since the main table dbo.DataCorrections2 has no rn column but the temp table has them it will be done in 2 phases --
-- In the first phase I will find out which ids are in each row number, then I will update the main table ids --

--------------------------------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------

-- Mission 6: Making a drill-down for each product and validating all the previous modifications are correct --

-- After classfying many products according to their main name I'm doing now a drill-down for each specific orpduct --
-- To do so I'm classifiyng all product names according to their starting letter to make it easier and more accurate --

-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------

-- Mission 7: Validating there are no duplications of products with the same trxdate --

-- this temp table is used for finding the names of the products who have duplications and might need re-editing --

-- After making a table of all the products who appear more than once I'm finding out all the necccesary details for the prior table --


-- this is the list of products with duplication we will need to edit afterwards --
-- as we've seen before there are 727 unique products where the table has a total of 859 rows. --
-- from the #temp_full_product_details table I found out there are 121 products with duplications. --
-- That means there are 132 duplicated values all over this table. --
-- However most products with duplication have different trxdates, so I made a table only for products with same trxdate --

-- Now we can see that for 'Vodka - Hot, Lnferno' there is one trxdate and one trxdate with null value --
-- For 'Scrubbie - Scotchbrite Hand Pad' there are 3 rows with the same id --

-- the sum of sales for 'Scrubbie - Scotchbrite Hand Pad' is 7884 --
-- I will add a new row for this product with the total sum of sales in order to avoid confusion later on --

-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------

-- Mission 8: Adding the real USD/NIS rate to our final table --

-- I've used those functions to find out the date ranges for our data in order to retrive the exact data from internet sources --
-- In order to set the time right I've searched the internet for the exchange rates in the relevant times --
-- I've also uploaded a table and combined the actual excjange rates with the data we already have --

-- Mission 8a: Updating trxweekday value for rows which refer to Saturday or Sunday --

-- Mission 8b: Finding whether the date value for one column which has null date value can be calculated --

--delete from dbo.DeptCorrections
--where id = 627 

-- Mission 8c: Adding the updated trxdate values to create a final table --
