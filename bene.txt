import sys
import csv
import pandas as pd
import numpy as np
import re
import matplotlib
import matplotlib.pyplot as plt
matplotlib.style.use('ggplot')

if len(sys.argv) > 1:
    filename = sys.argv[1]
else:
    print("Error: %s needs to include name of csv file" % (sys.argv[0],))
    sys.exit()

df = pd.read_csv(filename, thousands=',')

df['BENLVL1'] = ""
df['BENLVLNEW'] = np.nan

print(df.columns)

# def unique(list1):
#         x = np.array(list1)
#         print(np.unique(x))

# uniqueValues = df['ACCTNO'].unique()
# uniqueCount = df['ACCTNO'].value_counts()
# print(uniqueValues)
# print(uniqueCount)

# for col in list(df):
#     print(col)
#     print(df[col].unique())

bene_dict = {4:25,1:100,2:50}

df['BENLVL1'] = df.groupby(["ACCTNO","ACTYPE","BENLVL","BENPCT"])[['BENNAME']].transform('count')

df['BENLVLNEW'] = df['BENLVL1'].map(bene_dict)
# print(df)

print(df.groupby(["ACCTNO","ACTYPE","BENLVL","BENPCT"])[['BENNAME']].transform('count'))

df.to_csv('bene.csv', index=False, header =True)