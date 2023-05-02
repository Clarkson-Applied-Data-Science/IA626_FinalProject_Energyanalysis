# IA626_FinalProject_Energyanalysis   
Contributers: INDU SRI DHARAVATH - dharavi@clarkson.edu
              SANDEEP KUMAR LINGAMPALLY - lingamsk@clarkson.edu
## ENERGY AND TEMPERATURE ANALYSIS IN NYC

### INTRODUCTION 
This is a repo which describes about dataset which contains information about Energy usage depending on snow fall perception and temprature in NYC.

#### DATA COLLECTION
Extracted heating usage data set for 10years(2013-2023) from https://opendata.cityofnewyork.us/data/ which contains details about energy usage data based on 4 boroughs(Queens, Brooklyn, Bronx, Manhattan) in NYC.

These attributes provide a wide range as a user to introspect throughly and make decision on the data.
### Data Description
Analysis based on main data Attributes like:

1. Borough
2. Service Start Date & Service End Date
3. Current Charges
4. Consumption (Therms)
5. Latitude and Longitude 
6. Perception of snow fall
7. Temperature Max & Temperature Min
8. Vendor Name
9. Funding Source

### Initally loading the CSV main data file into memory and printing all the attributes in the data
 ``` python
    
      
        import csv
        # importing csv Heating_Gas_Consumption.csv 
        fn ='Heating_Gas_Consumption.csv'
        f = open(fn,'r')
        reader = csv.reader(f)
        n=0
        for row in reader:
            print(row)
 ```       
 ['Development Name', 'Borough', 'Account Name', 'Location', 'Meter AMR', 'Meter Scope', 'TDS #', 'EDP', 'RC Code', 'Funding Source', 'AMP #', 'Vendor Name', 'UMIS BILL ID', 'Revenue Month', 'Service Start Date', 'Service End Date', '# days', 'Meter Number', 'Estimated', 'Current Charges', 'Rate Class', 'Bill Analyzed', 'Consumption (Therms)', 'ES Commodity', 'Underlying Utility']
['OCEAN BAY APARTMENTS (OCEANSIDE)', 'QUEENS', 'OCEAN BAY APARTMENTS (OCEANSIDE)', 'BLD 1', '', 'BACKUP GENERATOR', '51', '573', 'Q005100', 'FEDERAL', 'NY005010980P', 'National Grid LI', '12206094', '2023-01', '12/30/2022', '01/31/2023', '32', '4973285', 'N', '58.42', '250 Gas Non Resid General Use', 'Yes', '9', 'UTILITY GAS', 'NatGrid LI']
['MELROSE', 'BRONX', 'MELROSE', 'BLD 06', 'NONE', 'BLD 1-8', '28', '523', 'B002800', 'FEDERAL', 'NY005010280P', 'CONSOLIDATED EDISON COMPANY OF NY', '12204326', '2023-01', '12/23/2022', '01/25/2023', '33', '3890439', 'N', '121355.55', 'TRMDHDF', 'Yes', '106950', 'UTILITY GAS', 'ConEd']
['#N/A', 'NON DEVELOPMENT FACILITY', 'NDF - PSA 5, 221 EAST 123 ST', 'BLD 05', 'NONE', '', '#N/A', '999', '1011500', 'NON-DEVELOPMENT', '', 'CONSOLIDATED EDISON COMPANY OF NY', '12141969', '2023-01', '12/23/2022', '01/25/2023', '33', '3409056', 'Y', '1867.27', 'TR II NR', 'Yes', '1670', 'UTILITY GAS', 'ConEd']
['WEBSTER', 'BRONX', 'WEBSTER', 'BLD 04', 'NONE', 'BLD 1-5', '141', '231', 'B014100', 'FEDERAL', 'NY005011410P', 'CONSOLIDATED EDISON COMPANY OF NY', '12146442', '2023-01', '12/23/2022', '01/25/2023', '33', '3382738', 'N', '136273.15', 'TRMDHDF', 'Yes', '120186', 'UTILITY GAS', 'ConEd']
['FHA REPOSSESSED HOUSES (GROUP IX)', 'FHA', 'FHA REPOSSESSED HOUSES (GROUP IX)', '341 BERRIMAN STREET', 'AMR', '', '283', '376', 'Q028300', 'FEDERAL', 'NY005012090P', 'National Grid NYC', '12218362', '2023-01', '01/13/2023', '01/24/2023', '11', '115063', 'N', '503.59', 'T1B TRAN RES HEAT', 'Yes', '587', 'UTILITY GAS', 'NatGrid NYC']
            
### Total number of rows in the main data set
``` python 
                import csv
                fn ='Heating_Gas_Consumption.csv'
                f = open(fn,'r')
                reader = csv.reader(f)
                n=0
                for row in reader:
                    if n % 1000000 == 0:
                        print("Row:",n)
                    n+=1
                print("Total number of rows:",n)
 ```       
  Total number of rows: 230214
  
### Cleaning data and creating subset with main attributes and removing Null values 
``` python 
                         
                        #New subset csv file with cleaned data
                        import csv
                        import datetime
                        fn ='Heating_Gas_Consumption.csv'
                        f = open(fn,'r')
                        reader = csv.reader(f)
                        #n = 0
                        #clear destination file
                        with open('Subset_Heating_Gas_Consumption.csv','w') as f1:
                            f1.write('')

                        with open('Subset_Heating_Gas_Consumption.csv','a') as f1:
                            writer = csv.writer(f1, delimiter=',', lineterminator='\n')
                            header = next(reader)
                            writer.writerow(header)
                            for i,row in enumerate(reader):
                                dts=row[14]
                                dte=row[15]

                                #print(dt)
                                try:
                                    dtstart=datetime.datetime.strptime(dts,'%m/%d/%Y')
                                    dtend=datetime.datetime.strptime(dte,'%m/%d/%Y')
                                except Exception as e:
                                    pass

                                #print(dtstart.year,dtend.year)
                                if i > 0 and row[0] != '#N/A' and row[1] != 'FHA' and row[1] != 'NON DEVELOPMENT FACILITY' and row[1]!= 'STATEN ISLAND'and dtstart.year >= 2013 and dtend.year >= 2013:
                                    writer.writerow(row)
                                i += 1
                               # if i > 10:
                                #    break'''
``` 
### Once the data is cleaned and subset of cleaned data created loading the data into dataframes using pandas 
``` python 
            
        import pandas as pd

        # Import dataset1
        df1 = pd.read_csv('Subset_Heating_Gas_Consumption.csv')

        # Import dataset2
        bro = pd.read_csv('Bronx_new.csv')
```        
  <img width="1152" alt="Initial load data" src="https://user-images.githubusercontent.com/123268832/235383729-2d7def19-f685-46ba-a4fb-045eeff59147.png">

        
### Drop unnecessary columns and rename any columns that need to be changed
``` python 

        df1.drop(columns=['Meter AMR', 'Meter Scope', 'TDS #','EDP', 'RC Code'], inplace=True)
 ```
<img width="1244" alt="Dropped Columns" src="https://user-images.githubusercontent.com/123268832/235383870-9e35355f-64f9-4b13-b57a-65a850f93929.png">


### Changing data type to datetime(ns) for revenue month 
``` python 

        df1['Revenue Month']=pd.to_datetime(df1['Revenue Month'])

        df1['Service Start Date']=pd.to_datetime(df1['Service Start Date'])
        
        df1['Service End Date']=pd.to_datetime(df1['Service End Date'])
         
        bro['DATE']=pd.to_datetime(bro['DATE'])
 ```       
### Loading brooklyn data into df3 and cleaning the data as there are double quotes all over the dataset
``` python 

        DF3 = pd.read_csv('Brooklyn.csv',low_memory=False)
        DF3 = DF3.replace('"', '', regex=True)
        DF3.columns = DF3.columns.str.strip()
        DF3
 ```       
### Renaming and cleaning few columns to match the key values in all the data sets
``` python 

         Brooklyn = Brooklyn.rename(columns={'Development Name': 'Development_Name', 'Borough': 'Borough', 'Account Name': 'Account_Name',
        'Location': 'Location', 'Funding Source': 'Funding_Source', 'AMP #': 'AMP_Number', 'Vendor Name': 'Vendor_Name', 'UMIS BILL ID': 'UMIS_BILL_ID',
        'Revenue Month': 'Revenue_Month' , 'Service Start Date': 'Service_Start_Date', 'DATE': 'DATE', '# days':'Number_of_days', 'Meter Number': 'Meter_Number', 'Estimated': 'Estimated', 
        'Current Charges': 'Current_Charges', 'Rate Class': 'Rate_Class', 'Bill Analyzed': 'Bill_Analyzed', 'Consumption (Therms)': 'Consumption_Therms', 'ES Commodity': 'ES_Commodity',
        'Underlying Utility': 'Underlying_Utility', 'STATION': 'STATION', 'NAME': 'NAME', 'LATITUDE': 'LATITUDE', 'LONGITUDE': 'LONGITUDE', 'ELEVATION': 'ELEVATION', 'PRCP': 'PRCP', 'PRCP_ATTRIBUTES':'PRCP_ATTRIBUTES','SNOW': 'SNOW',
        'TMAX': 'TMAX', 'TMAX_ATTRIBUTES': 'TMAX_ATTRIBUTES', 'TMIN': 'TMIN', 'TMIN_ATTRIBUTES': 'TMIN_ATTRIBUTES'
       })

       Brooklyn
 ```
   <img width="1286" alt="Screenshot 2023-04-30 at 9 54 35 PM" src="https://user-images.githubusercontent.com/123268832/235389821-ff702786-a085-42c3-8d6c-18a254019ec3.png">

    

 ### Dropping unwanted coloumns to avoid extra data coloumns while merging the data 
``` python 
 
        DF4 = DF3.drop(['AWND', 'AWND_ATTRIBUTES', 'DAPR', 'DAPR_ATTRIBUTES', 'MDPR', 'MDPR_ATTRIBUTES',
                          'PGTM', 'PGTM_ATTRIBUTES', 'PSUN', 'PSUN_ATTRIBUTES', 'SNOW_ATTRIBUTES', 'SNWD',
                          'SNWD_ATTRIBUTES', 'TAVG', 'TAVG_ATTRIBUTES', 'TOBS', 'TOBS_ATTRIBUTES', 'TSUN',
                          'TSUN_ATTRIBUTES', 'WDF2', 'WDF2_ATTRIBUTES', 'WDF5', 'WDF5_ATTRIBUTES', 'WESD',
                          'WESD_ATTRIBUTES', 'WESF', 'WESF_ATTRIBUTES', 'WSF2', 'WSF2_ATTRIBUTES', 'WSF5',
                          'WSF5_ATTRIBUTES', 'WT01', 'WT01_ATTRIBUTES', 'WT02', 'WT02_ATTRIBUTES', 'WT03',
                          'WT03_ATTRIBUTES', 'WT04', 'WT04_ATTRIBUTES', 'WT05', 'WT05_ATTRIBUTES', 'WT06',
                          'WT06_ATTRIBUTES', 'WT08', 'WT08_ATTRIBUTES', 'WT09', 'WT09_ATTRIBUTES', 'WT11',
                          'WT11_ATTRIBUTES', 'WT13', 'WT13_ATTRIBUTES', 'WT14', 'WT14_ATTRIBUTES', 'WT15',
                          'WT15_ATTRIBUTES', 'WT16', 'WT16_ATTRIBUTES', 'WT18', 'WT18_ATTRIBUTES', 'WT19',
                          'WT19_ATTRIBUTES', 'WT22', 'WT22_ATTRIBUTES'], axis=1)
                          
         new_que = que.drop(['AWND', 'AWND_ATTRIBUTES', 'DAPR', 'DAPR_ATTRIBUTES', 'MDPR', 'MDPR_ATTRIBUTES',
                  'PGTM', 'PGTM_ATTRIBUTES', 'SNOW_ATTRIBUTES', 'SNWD',
                  'SNWD_ATTRIBUTES', 'TAVG', 'TAVG_ATTRIBUTES', 'WDF2', 'WDF2_ATTRIBUTES', 'WDF5', 'WDF5_ATTRIBUTES', 'WESD',
                  'WESD_ATTRIBUTES', 'WESF', 'WESF_ATTRIBUTES', 'WSF2', 'WSF2_ATTRIBUTES', 'WSF5',
                  'WSF5_ATTRIBUTES', 'WT01', 'WT01_ATTRIBUTES', 'WT02', 'WT02_ATTRIBUTES', 'WT03',
                  'WT03_ATTRIBUTES', 'WT04', 'WT04_ATTRIBUTES', 'WT05', 'WT05_ATTRIBUTES', 'WT06',
                  'WT06_ATTRIBUTES', 'WT08', 'WT08_ATTRIBUTES', 'WT09', 'WT09_ATTRIBUTES', 'WT13',
                  'WT13_ATTRIBUTES', 'WT14', 'WT14_ATTRIBUTES', 'WT15', 'WT15_ATTRIBUTES','WT16',
                  'WT16_ATTRIBUTES', 'WT18', 'WT18_ATTRIBUTES', 'WT22', 'WT22_ATTRIBUTES'], axis=1) 

```

         
       
### renaming the common coloumn names before merging the data files
``` python 

             df1=df1.rename(columns={'Service End Date': 'DATE'})

             Manhattan = Manhattan.rename(columns={'Development Name': 'Development_Name', 'Borough': 'Borough', 'Account Name': 'Account_Name',
             'Location': 'Location', 'Funding Source': 'Funding_Source', 'AMP #': 'AMP_Number', 'Vendor Name': 'Vendor_Name', 'UMIS BILL ID': 'UMIS_BILL_ID',
             'Revenue Month': 'Revenue_Month' , 'Service Start Date': 'Service_Start_Date', 'DATE': 'DATE', '# days':'Number_of_days', 'Meter Number': 'Meter_Number', 'Estimated': 'Estimated', 
             'Current Charges': 'Current_Charges', 'Rate Class': 'Rate_Class', 'Bill Analyzed': 'Bill_Analyzed', 'Consumption (Therms)': 'Consumption_Therms', 'ES Commodity': 'ES_Commodity',
             'Underlying Utility': 'Underlying_Utility', 'STATION': 'STATION', 'NAME': 'NAME', 'LATITUDE': 'LATITUDE', 'LONGITUDE': 'LONGITUDE', 'ELEVATION': 'ELEVATION', 'PRCP': 'PRCP', 'PRCP_ATTRIBUTES':'PRCP_ATTRIBUTES','SNOW': 'SNOW',
             'TMAX': 'TMAX', 'TMAX_ATTRIBUTES': 'TMAX_ATTRIBUTES', 'TMIN': 'TMIN', 'TMIN_ATTRIBUTES': 'TMIN_ATTRIBUTES'
             })

             Queens = Queens.rename(columns={'Development Name': 'Development_Name', 'Borough': 'Borough', 'Account Name': 'Account_Name',
             'Location': 'Location', 'Funding Source': 'Funding_Source', 'AMP #': 'AMP_Number', 'Vendor Name': 'Vendor_Name', 'UMIS BILL ID': 'UMIS_BILL_ID',
             'Revenue Month': 'Revenue_Month' , 'Service Start Date': 'Service_Start_Date', 'DATE': 'DATE', '# days':'Number_of_days', 'Meter Number': 'Meter_Number', 'Estimated': 'Estimated', 
             'Current Charges': 'Current_Charges', 'Rate Class': 'Rate_Class', 'Bill Analyzed': 'Bill_Analyzed', 'Consumption (Therms)': 'Consumption_Therms', 'ES Commodity': 'ES_Commodity',
             'Underlying Utility': 'Underlying_Utility', 'STATION': 'STATION', 'NAME': 'NAME', 'LATITUDE': 'LATITUDE', 'LONGITUDE': 'LONGITUDE', 'ELEVATION': 'ELEVATION', 'PRCP': 'PRCP', 'PRCP_ATTRIBUTES':'PRCP_ATTRIBUTES','SNOW': 'SNOW',
             'TMAX': 'TMAX', 'TMAX_ATTRIBUTES': 'TMAX_ATTRIBUTES', 'TMIN': 'TMIN', 'TMIN_ATTRIBUTES': 'TMIN_ATTRIBUTES'
             })
```             
### Drop unnecessary columns and rename any columns that need to be changed
``` python 

              bro.drop(columns=['ELEVATION', 'PRCP_ATTRIBUTES', 'SNOW_ATTRIBUTES','SNWD', 'SNWD_ATTRIBUTES', 'TOBS' , 'TOBS_ATTRIBUTES', 'WT01', 'WT01_ATTRIBUTES', 'WT04', 'WT04_ATTRIBUTES', 'WT06',  'WT06_ATTRIBUTES'], inplace=True)

              bro
 ```               
<img width="1248" alt="Bro" src="https://user-images.githubusercontent.com/123268832/235383985-1f2e4420-d7a0-4de8-b545-ca7a8c3ce5be.png">

### Changing DATE to datetime
``` python 

    bro['DATE']=pd.to_datetime(bro['DATE'])
    bro['DATE']
```    
 ## segregating borough data into a seperate data frames
``` python 
 
    df_Bronx=df1[df1['Borough']=='BRONX']
    df_Bronx
 ```   
<img width="1271" alt="Screenshot 2023-04-30 at 9 20 16 PM" src="https://user-images.githubusercontent.com/123268832/235387516-c3ec89fb-5796-4217-bff5-74292159a07b.png">

``` python 

     df_Brooklyn=df1[df1['Borough']=='BROOKLYN']
     df_Brooklyn
  ```   
     
 <img width="1260" alt="Screenshot 2023-04-30 at 9 23 21 PM" src="https://user-images.githubusercontent.com/123268832/235387659-e39954e6-77e1-428b-8d0e-2e9abb557b8e.png">
 
``` python 
    
      df_Manhattan=df1[df1['Borough']=='MANHATTAN']
      df_Manhattan
  ```     
  <img width="1262" alt="Screenshot 2023-04-30 at 9 24 29 PM" src="https://user-images.githubusercontent.com/123268832/235387742-9f8bb25c-88ae-4b3e-b914-48840f20996c.png">
  
``` python 
      
      df_Queens=df1[df1['Borough']=='QUEENS']
      df_Queens
  ```     
  <img width="1262" alt="Screenshot 2023-04-30 at 9 26 41 PM" src="https://user-images.githubusercontent.com/123268832/235387947-b92b1c8b-d2e6-489b-87c8-846494aeed73.png">
   
   
 ## Saving data into csv file from the data dframe
``` python 
 
    bronx.to_csv('bronx_merged.csv', index=False)
    Brooklyn.to_csv('brooklyn_merged.csv', index=False)
    Manhattan.to_csv('manhattan_merged.csv', index=False)
    Queens = pd.merge(df_Queens,new_que,on='DATE',how='inner')
```    
 ## Inserting the merged data of bronx into the database(ETL) 
``` python 
 
        import csv
        import pymysql
        import time
        import datetime
        with open('bronx_merged.csv',"r",encoding='Latin-1') as f:
            data = [{k: str(v) for k, v in row.items()}
                for row in csv.DictReader(f,skipinitialspace=True)]
        #print(data[0].keys())
        #print(len(data[0]))

        conn = pymysql.connect(host='mysql.clarksonmsda.org', port=3306, user='ia626',
                                passwd='ia626clarkson', db='ia626', autocommit=True)
        cur = conn.cursor(pymysql.cursors.DictCursor)

        #Dropping the table if it exist
        sql = '''DROP TABLE IF EXISTS `Heating_gas_bronx`;'''
        cur.execute(sql)

       #Creating the table and normalizing the column names with appropriate field types
       sql = '''CREATE TABLE IF NOT EXISTS `Heating_gas_bronx` (
         `id` int(7) NOT NULL AUTO_INCREMENT,
         `Development_Name` varchar(50) NOT NULL,
         `Borough` varchar(50) NOT NULL,
         `Account_Name` varchar(50) NOT NULL,
         `Location` varchar(50) NOT NULL,
         `Funding_Source` varchar(50) NOT NULL,
         `AMP_Number` varchar(50) NOT NULL,
         `Vendor_Name` varchar(50) NOT NULL,
         `UMIS_BILL_ID` varchar(50) NOT NULL,
         `Revenue_Month` datetime NOT NULL,
         `Service_Start_Date` datetime NOT NULL,
         `Date` datetime NOT NULL,
         `Number_of_days` int(10) NOT NULL,
         `Meter_Number` varchar(50) NOT NULL,
         `Estimated` varchar(10) NOT NULL,
         `Current_Charges` decimal(18,12) NOT NULL,
         `Rate_Class` varchar(50) NOT NULL,
         `Bill_Analyzed` varchar(50) NOT NULL,
         `Consumption_Therms` decimal(18,12) NOT NULL,
         `ES_Commodity` varchar(50) NOT NULL,
         `Underlying_Utility` varchar(50) NOT NULL,
         `STATION` varchar(50) NOT NULL,
         `NAME` varchar(50) NOT NULL,
         `LATITUDE` decimal(18,12) NOT NULL,
         `LONGITUDE` decimal(18,12) NOT NULL,
         `PRCP` decimal(18,12) NOT NULL,
         `SNOW` decimal(18,12) NOT NULL,
         `TMAX` decimal(18,12) NOT NULL,
         `TMAX_ATTRIBUTES` varchar(50) NOT NULL,
         `TMIN` decimal(18,12) NOT NULL,
         `TMIN_ATTRIBUTES` varchar(50) NOT NULL,
         PRIMARY KEY (`id`)
       ) ENGINE=MyISAM DEFAULT CHARSET=latin1 AUTO_INCREMENT=1 ;'''
       cur.execute(sql)

       sql = '''INSERT INTO `Heating_gas_bronx` (`Development_Name`, `Borough`, `Account_Name`,
        `Location`, `Funding_Source`, `AMP_Number`, `Vendor_Name`, `UMIS_BILL_ID`,
        `Revenue_Month`, `Service_Start_Date`, `DATE`, `Number_of_days`, `Meter_Number`, `Estimated`, 
        `Current_Charges`, `Rate_Class`, `Bill_Analyzed`, `Consumption_Therms`, `ES_Commodity`,
        `Underlying_Utility`, `STATION`, `NAME`, `LATITUDE`, `LONGITUDE`, `PRCP`, `SNOW`,
        `TMAX`, `TMAX_ATTRIBUTES`, `TMIN`, `TMIN_ATTRIBUTES`) VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s);'''

       n=0
       bs = 500
       tokens=[]
       for row in data:
         tokens.append([row['Development_Name'],row['Borough'],row['Account_Name'],row['Location'],
                   row['Funding_Source'],row['AMP_Number'],row['Vendor_Name'],row['UMIS_BILL_ID'],
                   row['Revenue_Month'],row['Service_Start_Date'],row['DATE'],row['Number_of_days'],
                   row['Meter_Number'],row['Estimated'],row['Current_Charges'],row['Rate_Class'],
                   row['Bill_Analyzed'],row['Consumption_Therms'],row['ES_Commodity'], row['Underlying_Utility'],
                   row['STATION'],row['NAME'],row['LATITUDE'],row['LONGITUDE'],row['PRCP'],row['SNOW'],row['TMAX'],
                   row['TMAX_ATTRIBUTES'],row['TMIN'],row['TMIN_ATTRIBUTES']])
           #print(tokens)
         if len(tokens) >= bs:
             cur.executemany(sql,tokens)
             conn.commit()
             tokens = []
         n+=1
       if len(tokens) > 0:
           cur.executemany(sql,tokens)  #Uploading all rows into the sql
           conn.commit()
```
  Inserting merged data of brooklyn, Manhattan and Queen Similar to Bronx borough
 


  ## Data Visualization 
   
   ## 1) Bronx
   
  ### Plotting average current_charges consumption_therms of bronx borough through each day in a month 
 
 <img width="800" alt="Bronx_1" src="https://user-images.githubusercontent.com/126725860/235343044-2a336853-949b-411a-8a37-d7c2af73a265.png">
 
 From above visualization avg data of bronx we undestand that in the month of march the current charges are high because the consumption in therms 
 
 ### Average of consumption_Therms by Vendor_name and Month 
 <img width="686" alt="Bronx_vendor" src="https://user-images.githubusercontent.com/123268832/235382161-49febbf8-be1e-4db2-bb61-21fedbce2eea.png">
 
  ## 2) Brooklyn
  
  ###  Average of current Charges by Development_Name in Brooklyn
  
  <img width="701" alt="Bronx_develop" src="https://user-images.githubusercontent.com/123268832/235382394-d279163b-68f1-4a30-94d1-cefe24856b4b.png">
  
  ###  Average of PRCP by Month in Brooklyn
  
  <img width="730" alt="Bronxpcrp_2" src="https://user-images.githubusercontent.com/123268832/235382526-bd5efdb3-6455-4fb2-b297-64ea49d17892.png">
  
  ## 3) Manhattan
  
  ###  Average of Temperature MIN and Average of Temperature MAX by Month in Manhattan 
   <img width="682" alt="Manhattan_Tmin" src="https://user-images.githubusercontent.com/123268832/235382779-9a0fe64b-d90d-428b-a581-4626c49ca1f1.png">
   
  ### Average of Current_Charges by Development_Name and Month in Manhattan
  <img width="678" alt="manhattan_develop" src="https://user-images.githubusercontent.com/123268832/235382902-45397277-67d6-451f-94b9-4a5a0b67f2ec.png">
  
  ## 4) Queens 
  
  ### Average of PRCP and Average of SNOW by Month in Queens
  <img width="682" alt="Queens_snow" src="https://user-images.githubusercontent.com/123268832/235383024-6c795eb2-e723-44ca-b9dd-965938171d48.png"> 
  
  ### Average of Current_Charges and Average of Consumption_Therms by Month in Queens
  <img width="683" alt="Queens_charge" src="https://user-images.githubusercontent.com/123268832/235383163-010d0b9f-9622-4c58-bca3-8c15eecdedf1.png">
  
  ### Conclusion

By analyzing this dataset, one can gain insights into the energy usage patterns in different boroughs of New York City, how they vary over time, and how they are affected by factors such as temperature, snowfall and Precipitation. This information can be valuable for making decisions related to energy management and policy.

Overall, the dataset appears to be comprehensive and includes a diverse set of attributes that can be useful for conducting detailed analyses. However, the specific conclusions that can be drawn from the data will depend on the specific research questions and methods used in the analysis.
  
Through regerous cleaning and merging of the above datasets, we get a confined data specific to the months form `MARCH` to `AUGUST` and from that one can clearly understand that the `LATE WINTERS` mark the highest useage of the Heating Gas specifically the month of `MARCH` marks the highest. In addition to that we can also see, the `CURRENT CHARGES` are also high on those months and so with this we can conclude that the `CURRENT CHARGES` is directly propotional to the `CONSUMPTION THERMS` useage.
 
 



    

        
        




  
  


