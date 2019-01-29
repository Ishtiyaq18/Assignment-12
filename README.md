# Assignment-12

# Importing all necessary libraries

import numpy as np
import pandas as pd
from pandas import DataFrame, Series
import sqlite3 as db
from pandasql import sqldf
pysqldf=lambda q:sqldf(q, globals())
from pandasql import sqldf

# Customizing the index, removing the '-' from column names

print(sqladb.columns)
import re
sqladb.columns = [re.sub("[-]", "_", col) for col in sqladb.columns]
print(sqladb.columns)

# Printing the list of all unique values

print(sqladb.education.unique())
print(sqladb.workclass.unique())
print(sqladb.relationship.unique())
print(sqladb.sex.unique())
print(sqladb.marital_status.unique())
print(sqladb.race.unique())

# Database with the revised column names
sqladb.head()


# 1. Select 10 records from the adult sqladb

sqladb=pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data',names=['age', 
'workclass', 'fnlwgt', 'education', 'education_num','marital_status', 'occupation', 'relationship', 'race', 'sex',
'capital_gain', 'capital_loss','hours_per_week', 'native_country','>50K,<=50K'])


#Selecting 10 records using the lambda fuction declared earlier

pysqldf('SELECT * FROM sqladb LIMIT 10;')

# 2. Show me the average hours per week of all men who are working in private sector

q = """ select sex,workclass,avg(hours_per_week) from sqladb where sex =' Male' and workclass =' Private' 
group by sex  ; """
pysqldf(q)

# 3. Show me the frequency table for education, occupation and relationship, separately

print('Education table')
q = """ select education,count(education) as Frequency from sqladb group by education ; """
pysqldf(q)
print('Occupation table')
q = """ select occupation,count(occupation) as Frequency from sqladb group by occupation ; """
pysqldf(q)
print('Relationship table')
q = """ select relationship,count(relationship) as Frequency from sqladb group by relationship ; """
pysqldf(q)

# 4. Are there any people who are married, working in private sector and having a masters degree
# Since the question require people who are married, I'm excluding people who are currently not married like divorced 
# or seperated etc
q = """ select count(*) as count_of_people from sqladb where marital_status != ' Never-married and Divorced and 
Separated and Widowed' and education =' Masters' and workclass =' Private' ;""" 
pysqldf(q)

# 5. What is the average, minimum and maximum age group for people working in different sectors

q  = """ select avg(age),max(age),min(age),workclass from sqladb group by workclass ;"""
pysqldf(q)

# 6. Calculate age distribution by country

q = """select native_country,max(age),min(age),avg(age) from sqladb group by native_country;"""
pysqldf(q)

# 7. Compute a new column as 'Net-Capital-Gain' from the two columns 'capital-gain' and 'capital-loss'

q = """ select (capital_gain - capital_loss) as Net_Capital_Gain from sqladb;"""
pysqldf(q)



