# IA626_FinalProject_Energyanalysis


1.Initally loading the file into memory and printing first 5 rows

import csv
fn ='Heating_Gas_Consumption.csv'
f = open(fn,'r')
reader = csv.reader(f)
n=0
for row in reader:
    print(row)
    n+=1
    '''if n > 5:
        break'''


