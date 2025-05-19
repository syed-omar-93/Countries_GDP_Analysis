import pandas as pd
#Import Countries GDP data file from Excel into Python and name the dataframe to be gd
#gd stands for GDP data
gd = pd.read_excel(
    'C:\\Users\\asus\\Downloads\\Countries GDP data.xlsx')
    gd = gd.set_index('Country Name')
    #List of columns to remove from gd dataframe 
Columns_to_remove = ['Country Code','Series Code','2022 [YR2022]']
#Removing unwanted columns from our dataframe 
gd.drop(Columns_to_remove, axis = 1, inplace = True)
#Renaming all the year columns in the dataframe
gd = gd.rename(columns = {'2018 [YR2018]':'2018', '2019 [YR2019]':'2019', '2020 [YR2020]':'2020', '2021 [YR2021]':'2021'})
#Converting the values in the 2018, 2019, 2020 amd 2021 columns to a numeric data type whilst ignoring any null values
gd['2018'] = pd.to_numeric(gd['2018'], errors = 'coerce')
gd['2019'] = pd.to_numeric(gd['2019'], errors = 'coerce')
gd['2020'] = pd.to_numeric(gd['2020'], errors = 'coerce')
gd['2021'] = pd.to_numeric(gd['2021'], errors = 'coerce')
#Filtering the gd dataframe to include GDP per capita values 
gd = gd[gd['Series Name'] == 'GDP per capita (current US$)']
#Checking the data types of all the columns in the gd dataframe
gd.dtypes
#Selecting only the 2018 to 2021 columns in the dataframe 
gd = gd[['2018','2019','2020','2021']]
#Round the numbers in the 2018, 2019, 2020 and 2021 columns to 0 decimal places
columns_to_round = ['2018', '2019', '2020', '2021']
gd.loc[:, columns_to_round] = gd[columns_to_round].round(0)
#Creating a new dataframe only having rows with no missing values
gd_cleaned = gd.dropna()
gd_cleaned = gd_cleaned.copy()  # Create a copy of the DataFrame
#Creating a new column called % change from 2018 to 2021 in this new dataframe
gd_cleaned['% change from 2018 to 2021'] = \
(gd_cleaned['2021'] - gd_cleaned['2018']) / gd_cleaned['2018'] * 100
gd_cleaned.dtypes
#Rounding the values of the % change column to 2 decimal places
gd_cleaned['% change from 2018 to 2021'] = \
gd_cleaned['% change from 2018 to 2021'].round()
gd_cleaned_desc = gd_cleaned.sort_values(by = '% change from 2018 to 2021', ascending = False)
Countries_highest_GDP_per_capita_change = gd_cleaned_desc.head(10)
gd_cleaned_asc = gd_cleaned.sort_values(by = '% change from 2018 to 2021', ascending = True)
Countries_most_negative_GDP_per_capita_change = gd_cleaned_asc.head(10)
Countries_most_negative_GDP_per_capita_change 
