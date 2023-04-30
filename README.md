# IA626_FinalProject_Energyanalysis

## ENERGY AND TEMPERATURE ANALYSIS IN NYC

### INTRODUCTION 
This is a repo which describes about dataset which contains information about Energy usage depending on snow fall perception and temprature in NYC.

#### DATA COLLECTION
Extracted heating usage data set for 10years(2013-2023) from https://opendata.cityofnewyork.us/data/ which contains details about energy usage data based on 4 boroughs(Queens, Brooklyn, Bronx, Manhattan) in NYC.

These attributes provide a wide range as a user to introspect throughly and make decision on the data.
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
 
    
        import csv
        # importing csv Heating_Gas_Consumption.csv 
        fn ='Heating_Gas_Consumption.csv'
        f = open(fn,'r')
        reader = csv.reader(f)
        n=0
        for row in reader:
            print(row)
            
 ['Development Name', 'Borough', 'Account Name', 'Location', 'Meter AMR', 'Meter Scope', 'TDS #', 'EDP', 'RC Code', 'Funding Source', 'AMP #', 'Vendor Name', 'UMIS BILL ID', 'Revenue Month', 'Service Start Date', 'Service End Date', '# days', 'Meter Number', 'Estimated', 'Current Charges', 'Rate Class', 'Bill Analyzed', 'Consumption (Therms)', 'ES Commodity', 'Underlying Utility']
['OCEAN BAY APARTMENTS (OCEANSIDE)', 'QUEENS', 'OCEAN BAY APARTMENTS (OCEANSIDE)', 'BLD 1', '', 'BACKUP GENERATOR', '51', '573', 'Q005100', 'FEDERAL', 'NY005010980P', 'National Grid LI', '12206094', '2023-01', '12/30/2022', '01/31/2023', '32', '4973285', 'N', '58.42', '250 Gas Non Resid General Use', 'Yes', '9', 'UTILITY GAS', 'NatGrid LI']
['MELROSE', 'BRONX', 'MELROSE', 'BLD 06', 'NONE', 'BLD 1-8', '28', '523', 'B002800', 'FEDERAL', 'NY005010280P', 'CONSOLIDATED EDISON COMPANY OF NY', '12204326', '2023-01', '12/23/2022', '01/25/2023', '33', '3890439', 'N', '121355.55', 'TRMDHDF', 'Yes', '106950', 'UTILITY GAS', 'ConEd']
['#N/A', 'NON DEVELOPMENT FACILITY', 'NDF - PSA 5, 221 EAST 123 ST', 'BLD 05', 'NONE', '', '#N/A', '999', '1011500', 'NON-DEVELOPMENT', '', 'CONSOLIDATED EDISON COMPANY OF NY', '12141969', '2023-01', '12/23/2022', '01/25/2023', '33', '3409056', 'Y', '1867.27', 'TR II NR', 'Yes', '1670', 'UTILITY GAS', 'ConEd']
['WEBSTER', 'BRONX', 'WEBSTER', 'BLD 04', 'NONE', 'BLD 1-5', '141', '231', 'B014100', 'FEDERAL', 'NY005011410P', 'CONSOLIDATED EDISON COMPANY OF NY', '12146442', '2023-01', '12/23/2022', '01/25/2023', '33', '3382738', 'N', '136273.15', 'TRMDHDF', 'Yes', '120186', 'UTILITY GAS', 'ConEd']
['FHA REPOSSESSED HOUSES (GROUP IX)', 'FHA', 'FHA REPOSSESSED HOUSES (GROUP IX)', '341 BERRIMAN STREET', 'AMR', '', '283', '376', 'Q028300', 'FEDERAL', 'NY005012090P', 'National Grid NYC', '12218362', '2023-01', '01/13/2023', '01/24/2023', '11', '115063', 'N', '503.59', 'T1B TRAN RES HEAT', 'Yes', '587', 'UTILITY GAS', 'NatGrid NYC']
            
### Total number of rows in the main data set
 
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
        
  Total number of rows: 230214
  
### Cleaning data and creating subset with main attributes and removing Null values 
                         
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
 
### Once the data is cleaned and subset of cleaned data created loading the data into dataframes using pandas 
            
        import pandas as pd

        # Import dataset1
        df1 = pd.read_csv('Subset_Heating_Gas_Consumption.csv')

        # Import dataset2
        bro = pd.read_csv('Bronx_new.csv')
        
### Drop unnecessary columns and rename any columns that need to be changed
        df1.drop(columns=['Meter AMR', 'Meter Scope', 'TDS #','EDP', 'RC Code'], inplace=True)

### Changing data type to datetime(ns) for revenue month 
        df1['Revenue Month']=pd.to_datetime(df1['Revenue Month'])

        df1['Service Start Date']=pd.to_datetime(df1['Service Start Date'])
        
        df1['Service End Date']=pd.to_datetime(df1['Service End Date'])
         
        bro['DATE']=pd.to_datetime(bro['DATE'])
       
### renaming the common coloumn names before merging the data files   
        df1=df1.rename(columns={'Service End Date': 'DATE'})
        
### Drop unnecessary columns and rename any columns that need to be changed
                bro.drop(columns=['ELEVATION', 'PRCP_ATTRIBUTES', 'SNOW_ATTRIBUTES','SNWD', 'SNWD_ATTRIBUTES', 'TOBS' , 'TOBS_ATTRIBUTES', 'WT01', 'WT01_ATTRIBUTES', 'WT04', 'WT04_ATTRIBUTES', 'WT06',  'WT06_ATTRIBUTES'], inplace=True)

                bro
                
 ![

    

        
        




  
  


