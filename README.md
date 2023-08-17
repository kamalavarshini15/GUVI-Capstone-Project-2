# GUVI-Capstone-Project-2

**WORKFLOW**
1) importing necessary libraries

import pandas as pd
import mysql.connector as sql
import streamlit as st
import plotly.express as px
import os
import json
from streamlit_option_menu import option_menu

2) Cloning git hub data
   Code - !git clone https://github.com/PhonePe/pulse.git (for jupyter notebook)  _Note! - Cloning github may vary for different environments_

3) Data transformation
   Data present in json format is converted to Dataframe for user usability, and the data is retrieved by iterating through loops. Then the data frame is converted 
   to CSV. Packages like json , os and pandas were imported .

Sample Code :

path1 = "C:/Users/Kamalavarshini/Desktop/pulse/data/aggregated/transaction/country/india/state/"  _(Note! - the path from where data can be accessed might be local or  cloud server, but using local is preferable if you are using mysql as it uses local system)_
Agg_trans_list=os.listdir(path1)


clm_a1={'State':[], 'Year':[],'Quater':[],'Transacion_type':[], 'Transacion_count':[], 'Transacion_amount':[]}
for state in Agg_trans_list:
    p_state=path1+state+"/"
    Agg_year=os.listdir(p_state)
    for year in Agg_year:
        p_year=p_state+year+"/"
        Agg_year_list=os.listdir(p_year)
        for file in Agg_year_list:
            p_file=p_year+ file
            Data=open(p_file,'r')
            D =json.load(Data)
            for i in D['data']['transactionData']:
              Name=i['name']
              count=i['paymentInstruments'][0]['count']
              amount=i['paymentInstruments'][0]['amount']
              clm_a1['Transacion_type'].append(Name)
              clm_a1['Transacion_count'].append(count)
              clm_a1['Transacion_amount'].append(amount)
              clm_a1['State'].append(state)
              clm_a1['Year'].append(year)
              clm_a1['Quater'].append(int(file.strip('.json')))
#Succesfully created a dataframe
Agg_Trans=pd.DataFrame(clm_a1)


4) Connecting to Mysql Server
   mydb = sql.connect(host="localhost",
                   user="root",
                   password="    ",(Type your password)
                   database= "phonepe_pulse"
                  )
   mycursor = mydb.cursor(buffered=True)


   The above code is used for connecting to Mysql, then the query is written to store the data in form of table .

5) Creating Dashboard 

