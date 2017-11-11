# NSR_Pyber_HW
Pyber homework

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import os

df = pd.read_csv("raw_data/city_data.csv")
df2 = pd.read_csv("raw_data/ride_data.csv")

merge_df = pd.merge(df, df2, on="city")
types = merge_df.groupby(['type'])
merge_df.head()
totals = pd.DataFrame(merge_df)
total_count = totals['fare'].count()
total_fares = totals['fare'].sum()

drop_df = merge_df.drop_duplicates('city')
s = drop_df['driver_count']

grouped_merge_df = merge_df.groupby(['type', 'city']).fare.agg(['count', 'mean'])
grouped_merge_df.columns = ['Total Rides', 'Average Fare']
grouped_merge_df.head()

rural = grouped_merge_df.loc['Rural']
urban = grouped_merge_df.loc['Urban']
suburban = grouped_merge_df.loc['Suburban']
rural

plt.scatter(rural['Total Rides'], rural['Average Fare'], s=s, color='gold', edgecolors='black')
plt.scatter(urban['Total Rides'], urban['Average Fare'], s=s, color='lightcoral', edgecolors='black')
plt.scatter(suburban['Total Rides'], suburban['Average Fare'], s=s, color='lightskyblue', edgecolors='black')
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare($)')
plt.title('Pyber Ride Sharing Data (2016)')
plt.grid()
plt.show()

total_rides = grouped_merge_df['Total Rides'].count
grouped_df = merge_df.groupby(['type']).fare.agg(['count', 'sum'])
grouped_df.columns = ['Total Rides', 'Total Fares']
grouped_df['Total Rides Percentage'] = grouped_df['Total Rides'] / total_count * 100
grouped_df['Total Fares Percentage'] = grouped_df['Total Fares'] / total_fares * 100
grouped_df.head()

drop_df = merge_df.drop_duplicates('city')
drivers_type = drop_df.groupby(['type']).driver_count.agg(['sum'])
drivers_type.columns = ['Driver Counts']
total_drivers = drop_df['driver_count'].sum()
drivers_type['Total Driver Percentage'] = drivers_type['Driver Counts'] / total_drivers * 100
drivers_type

labels= grouped_df.index.values
sizes=grouped_df['Total Rides Percentage']
colors=['gold','lightskyblue','lightcoral']
explode = [0,0,0.1]
plt.pie(sizes,labels=labels,colors=colors ,explode=explode, autopct="%1.1f%%", shadow=True,startangle=140, wedgeprops = {'linewidth': 3} )
plt.axis('tight')
plt.title('% of Total Rides by City Type')
plt.show()

labels= grouped_df.index.values
sizes=grouped_df['Total Fares Percentage']
plt.pie(sizes,labels=labels,colors=colors ,explode=explode, autopct="%1.1f%%", shadow=True,startangle=140, wedgeprops = {'linewidth': 3} )
plt.axis('tight')
plt.title('% of Total Fares by City Type')
plt.show()

labels= drivers_type.index.values
sizes= drivers_type['Total Driver Percentage']
plt.pie(sizes,labels=labels,colors=colors ,explode=explode, autopct="%1.1f%%", shadow=True,startangle=140, wedgeprops = {'linewidth': 3} )
plt.axis('tight')
plt.title('% of Total Drivers by City Type')
plt.show()
