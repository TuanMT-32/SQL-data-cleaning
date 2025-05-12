# SQL-data-cleaning
SQL data cleaning is the process of using SQL (Structured Query Language) to identify and correct issues within raw datasets stored in databases. The goal is to ensure that the data is accurate, consistent, and reliable before it is used for reporting, analytics, or fed into business applications and machine learning models.

In real-world scenarios, raw data often contains errors such as duplicates, missing values, typos, inconsistent formats, or irrelevant information. SQL provides powerful tools to clean and transform this data directly within the database.

Let's inspect the initial rows to analyze the data in its original format:

![image](https://github.com/user-attachments/assets/12f379b0-ba19-4722-802e-9a869157ccee)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

## I. COPY THE TABLE

### Create a New Table for Cleaning

Let's generate a new table where we can manipulate and restructure the data without modifying the original dataset.

### Copy the Values from the Original Table

![image](https://github.com/user-attachments/assets/ecf4f424-dcde-4873-b179-943ef1cfe805)

## II. START CLEANING

Here are the basic issues I always start with when it comes to data cleaning:

**A. Remove Duplicates:** Check for and remove duplicates, especially if the dataset is not expected to contain any.

**B. Standardize the Data:** Address inconsistent letter casing, spelling errors, formatting issues, or numbers that fall outside a realistic range.

**C. Null or Blank Values:** Determine whether missing values can be filled in or if the row should be removed from the dataset.

---------------------------------------------------------------------------

### A. Finding and Removing Duplicates

There are several ways to find duplicates, but based on the course I took, this is a simple method using a subquery to identify duplicate rows.

![image](https://github.com/user-attachments/assets/2d5f154e-c231-4edc-88be-1e485dbecef8)

Here's the results:           
|total_duplicates_row|
|--------------------|
|4020|

Here is an example of duplicate rows found in the original dataset:

![image](https://github.com/user-attachments/assets/48bb38b7-bfa1-4b86-ad9b-98611136f8b7)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|Obed MacCaughen|27|divorced|omaccaughen1o@naver.com|815-990-8611|7069 Valley Edge Alley,Joliet,Illinois|Junior Executive|1/27/2012|
|Obed MacCaughen|27|married|omaccaughen1o@naver.com|815-990-8611|7069 Valley Edge Alley,Joliet,Illinois|Junior Executive|1/27/2012|
|Seymour Lamble|27|married|slamble81@amazon.co.uk|979-346-7243|90691 Veith Place,Bryan,Texas|Budget/Accounting Analyst IV|12/26/2017|
|Seymour Lamble|27|single|slamble81@amazon.co.uk|979-346-7243|90691 Veith Place,Bryan,Texas|Budget/Accounting Analyst IV|12/26/2017|
|georges prewett|47|married|gprewettfl@mac.com|571-157-7766|76 Graedel Road,Alexandria,Virginia|Paralegal|10/5/2015|
|georges prewett|47|single|gprewettfl@mac.com|571-157-7766|76 Graedel Road,Alexandria,Virginia|Paralegal|10/5/2015|
|Maddie Morrallee|39|married|mmorralleemj@wordpress.com|712-853-2968|9339 Straubel Center,Sioux City,Iowa|Software Engineer II|1/29/2020|
|Maddie Morrallee|39|divorced|mmorralleemj@wordpress.com|712-853-2968|9339 Straubel Center,Sioux City,Iowa|Software Engineer II|1/29/2020|
|Garrick Reglar|66|divorced|greglar4r@answers.com|214-314-5437|6 Dottie Drive,Dallas,Texas|Safety Technician I|11/22/2015|
|Garrick Reglar|66|single|greglar4r@answers.com|214-314-5437|6 Dottie Drive,Dallas,Texas|Safety Technician I|11/22/2015|

 **Make sure you back up your data or test with a SELECT before running DELETE!** 
 
  _(You can do create another table by re-doing the copy the table step above)_
  
To delete the duplicate rows, because I am using SQLite, so every rows has an internal RowID, here is the query:
![image](https://github.com/user-attachments/assets/22f8d67e-5482-4285-9caa-32fe342a945c)

And after running delete, I run the same query above to find the total duplicate rows, and here should be the result. 

|total_duplicate_rows|
|--------------------|
|0|

But don’t let the total row count fool you — you should always double-check against the original dataset. In this case, I used the same query to check for duplicate rows, but changed the source table to _club_member_info_cleaned_ table.

![image](https://github.com/user-attachments/assets/a1ed9c8d-de29-432a-9ccf-749055ec821b)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|Haskell Braden|322|divorced|hbradenri@freewebs.com|510-963-9848|35005 Waubesa Crossing,Berkeley,California|Dental Hygienist|11/4/2015|
|Haskell Braden|32|divorced|hbradenri@freewebs.com|510-963-9848|35005 Waubesa Crossing,Berkeley,California|Dental Hygienist|11/4/2015|

There is still duplicate row because of wrong input of age. So just delete it and where are done with the removing duplicate.

![image](https://github.com/user-attachments/assets/9b4161ec-8f3e-4611-bd3c-9a0a788bfab5)

And let's check again using the same query above. And the results should be blank because there are no duplicate left.

![image](https://github.com/user-attachments/assets/a1ed9c8d-de29-432a-9ccf-749055ec821b)
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|

Perfects! Let's move on to the next step.

### B. STANDARIZE THE DATA

From the original dataset, you can see that there are many names with blank spaces and inconsistent formatting. So let’s fix that first and see what the data looks like after trimming and converting it to uppercase.

![image](https://github.com/user-attachments/assets/c94c4945-9280-45c5-8c4e-ff46a6172382)
|full_name|clean_name|
|---------|----------|
|addie lush|ADDIE LUSH|
|      ROCK CRADICK|ROCK CRADICK|
|Sydel Sharvell|SYDEL SHARVELL|
|Constantin de la cruz|CONSTANTIN DE LA CRUZ|
|  Gaylor Redhole|GAYLOR REDHOLE|
|Wanda del mar       |WANDA DEL MAR|
|Joann Kenealy|JOANN KENEALY|
|   Joete Cudiff|JOETE CUDIFF|
|mendie alexandrescu|MENDIE ALEXANDRESCU|
| fey kloss|FEY KLOSS|

Beatiful! Now let's update it to our cleaned table:

![image](https://github.com/user-attachments/assets/ab753f71-272a-4f6b-a912-68e85bf565cd)

Let's check it:

![image](https://github.com/user-attachments/assets/849b13ae-c0a9-4494-b83e-6c135dd95c24)

|full_name|
|---------|
|ADDIE LUSH|
|ROCK CRADICK|
|SYDEL SHARVELL|
|CONSTANTIN DE LA CRUZ|
|GAYLOR REDHOLE|
|WANDA DEL MAR|
|JOANN KENEALY|
|JOETE CUDIFF|
|MENDIE ALEXANDRESCU|
|FEY KLOSS|

And we can repeate this process with the rest of the column. Now let's move on to the last step.

### C.  NULL VALUES OR BLANK VALUES

One of the most common ways to find blank values is by using IS NULL. Let’s run the query, as we noticed earlier that some marital status values are blank in the original dataset—so we’ll use that column for this check.

![image](https://github.com/user-attachments/assets/f2588d11-fd1e-40b3-ac48-53beea4e2932)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|

From the query above, we got an empty result — but clearly, there are blank values in the data. There are two ways to think about this. First, the values might not actually be NULL. But since that doesn’t seem correct, the more likely explanation is that the cells contain empty strings — that is, strings with zero characters (''), which are not the same as NULL. So let’s try again with this query:

![image](https://github.com/user-attachments/assets/924f9780-c9b4-4868-9342-698eb50a2601)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|CONSTANTIN DE LA CRUZ|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|NIXIE JANUARY|44||njanuaryp@youtu.be|415-318-7190|65 Stephen Circle,San Francisco,California|Chemical Engineer|7/29/2017|
|DANILA TEAGUE|42||dteague39@nydailynews.com|610-889-3130|9 Sugar Way,Philadelphia,Pennsylvania|Administrative Assistant III|8/29/2021|
|TYRONE SHILLUM|25||tshillum57@sina.com.cn|502-336-9009|698 Sundown Circle,Frankfort,Kentucky|Structural Engineer|4/10/2018|
|PRENTISS EPTON|42||pepton59@oracle.com|303-233-8382|58217 Holmberg Avenue,Boulder,Colorado|Professor|6/21/2014|
|DENY GRAINGER|55||dgrainger7g@skyrock.com|650-380-1663|5 Corry Hill,Los Angeles,California|Budget/Accounting Analyst IV|3/29/2016|
|NICCOLO CROSSER|46||ncrosserjv@imgur.com|954-707-4900|0155 Kensington Avenue,Hollywood,Florida|Technical Writer|11/11/2015|
|DUSTY BACCUS|48||dbaccus8j@elegantthemes.com|763-502-9649|5893 Milwaukee Plaza,Loretto,Minnesota|Computer Systems Analyst II|9/30/2017|
|MARJY RAIN|27||mrain9b@feedburner.com|713-699-1324|568 Oneill Way,Houston,Texas|Analyst Programmer|2/14/2022|
|NOBIE BOLDERO|51||nbolderobx@blinklist.com|915-591-9005|34 Vera Plaza,San Juan, Puerto Rico|Account Coordinator|7/14/2021|

Let's find out which column contains empty value

![image](https://github.com/user-attachments/assets/2bdfb09f-92e7-440e-8731-1b2bb8c212bd)

Based on this query, the columns that contain empty values are Marital Status, Phone, Age, and Job Title. These are all key data fields, but since only 70 out of 2,000 rows are affected, this is a case where, as a data analyst, you might decide that these rows are not significant enough to skew your dataset.

![image](https://github.com/user-attachments/assets/10321f13-ad20-4029-b93c-d3a78d4101db)
|COUNT(*)|
|--------|
|70|

![image](https://github.com/user-attachments/assets/39fac05d-8e86-4d2f-875f-2fd66760f673)
|COUNT(*)|
|--------|
|2000|

So in this case, my judment is to delete those data with null values

![image](https://github.com/user-attachments/assets/3ebc9c6c-ae16-411b-9bdb-18c9b1c3d60f)

Let's check the table data again

![image](https://github.com/user-attachments/assets/7c0ac24c-618d-4b35-8773-355eb36fe4f4)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|

It should be empty now. And that is my data cleaning process let's take a look again of the difference between the original and the cleaned data

**The original data:**

![image](https://github.com/user-attachments/assets/057ab2d0-23e5-4b6c-a997-29f2732ca9ff)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

**The cleaned data:**

![image](https://github.com/user-attachments/assets/611494d9-1d95-4cd9-a4a6-d363c8d4d6f0)

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|WANDA DEL MAR|44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|JOANN KENEALY|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|JOETE CUDIFF|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|MENDIE ALEXANDRESCU|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
|FEY KLOSS|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|
|DARWIN VENTAM|42|married|dventama@uol.com.br|203-993-0118|2254 Express Hill,New Haven,Connecticut|Chemical Engineer|3/12/2017|

### THAT IS THE END OF MY CLEANING PROCESS. THANK YOU FOR READING MY LOG ^^. I HOPE THIS IN ANYWAY HELP YOU WITH YOUR PROCESS ASWELL. WISHING YOU A LOT OF HAPINESS ON YOUR JOURNEY <3. 
