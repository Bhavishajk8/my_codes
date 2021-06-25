
# Data Analysis and Visualization of Cryptocurrency 


## Introduction
*The purpose of this project is to visualize the Cryptocurrency data*
- *With cryptocurrency becoming popular day by day we have taken some recent data to visualize it and understand them better.*
- *Data used for this projected is from the website - https://www.coingecko.com/en/api and the API used comes from rapid API https://rapidapi.com/coingecko/api/coingecko/endpoints*
- *The data that is being used for this project are taken from CoinGecko are top 10 cryptocurrency based on the ranking in terms of market capital.*
- *CoinGecko contains the details all the cryptocurrencies in the market along with its price, volume, market capitalization*
---
## Sources
- The source code came from CoinGecko API: https://rapidapi.com/coingecko/api/coingecko/endpoints
- The code retrieves data from CoinGecko: https://www.coingecko.com/en/api 
---

## Explanation of the Code
*We have taken two data sets and perform study on it :*
- *Bitcoin data for 8 days was taken in terms of USD.* 
- *Top 10 cryptocurrency based on their market capitalization*
*The code begins with the necessary package installation.*

```
import http.client
import json
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
import matplotlib.gridspec as gridspec
import seaborn as sns
```
*We then extract the data from the source using the API key by requesting the data from the host. We then store the data in a variable, read it, and then decode it so it can be used. We print the data at the end.*
```
import http.client

conn = http.client.HTTPSConnection("coingecko.p.rapidapi.com")

headers = {
    'x-rapidapi-key': "063ea1c81cmsh511c1e56374c14dp148a9fjsn29e3fe6cab8a",
    'x-rapidapi-host': "coingecko.p.rapidapi.com"
    }

conn.request("GET", "/coins/bitcoin/market_chart?vs_currency=usd&days=8", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```
- *NOTE 1: The data may change over time and the results may not be same everytime.*
```
crypto = json.loads(data)
crypto.keys()
```

*The data contains 'prices', 'market_caps', 'total_volumes’ for bitcoin in USD for 8 days. The timestamp is also given along with this. The data contains prices, market capital and total volume for every hour for the last 8 days. Unlike the global stock market cryptocurrency market prices run 24*7.*

*The numpy array is used to make day wise segregation of the data this is then used to plot dotted line graph for each day.*

![Image of Plot](https://github.com/IE-555/API/blob/adithya2597/ADITHYAA_BHAVISHA_MANANTEJ_DIKSHITD_cryptodata/images/Plot1.png?raw=true)
```
# Plotting value of bitcoin value for 8 days in an hourly interval.
price = np.asarray(crypto['prices'])
day_1 = price[:24,[1]]
day_2 = price[24:48,[1]]
day_3 = price[48:72,[1]]
day_4 = price[72:96,[1]]
day_5 = price[96:120,[1]]
day_6 = price[120:144,[1]]
day_7 = price[144:168,[1]]
day_8 = price[168:,[1]]
```
```
f,ax = plt.subplots(4,2,figsize = (15,12))
ax[0,0].set_title('Day 1')
ax[0,1].set_title('Day 2')
ax[1,0].set_title('Day 3')
ax[1,1].set_title('Day 4')
ax[2,0].set_title('Day 5')
ax[2,1].set_title('Day 6')
ax[3,0].set_title('Day 7')
ax[3,1].set_title('Day 8')
ax[0,0].plot(day_1, linestyle = '--',color = 'violet')
ax[0,1].plot(day_2, linestyle = '--',color = 'indigo')
ax[1,0].plot(day_3, linestyle = '--',color = 'blue')
ax[1,1].plot(day_4, linestyle = '--',color = 'g')
ax[2,0].plot(day_5, linestyle = '--',color = 'yellow')
ax[2,1].plot(day_6, linestyle = '--',color = 'orange')
ax[3,0].plot(day_7, linestyle = '--',color = 'r')
ax[3,1].plot(day_8, linestyle = '--',color = 'black')
ax[0,0].set_ylabel('Bitcoin value')
ax[0,1].set_ylabel('Bitcoin value')
ax[1,0].set_ylabel('Bitcoin value')
ax[1,1].set_ylabel('Bitcoin value')
ax[2,0].set_ylabel('Bitcoin value')
ax[2,1].set_ylabel('Bitcoin value')
ax[3,0].set_ylabel('Bitcoin value')
ax[3,1].set_ylabel('Bitcoin value')
plt.show()
f.savefig('Plot1.png')
```

- *NOTE 2: The data on the y axis is in terms of 10^12*

*We use the market capitalization data to plot a boxplot and strip plot for all the 8 days.*
```
market_caps = np.asarray(crypto['market_caps'])
day_1_mc = market_caps[:24,[1]]
day_2_mc = market_caps[24:48,[1]]
day_3_mc = market_caps[48:72,[1]]
day_4_mc = market_caps[72:96,[1]]
day_5_mc = market_caps[96:120,[1]]
day_6_mc = market_caps[120:144,[1]]
day_7_mc = market_caps[144:168,[1]]
day_8_mc = market_caps[168:,[1]]
marketdata=[day_1_mc,day_2_mc,day_3_mc,day_4_mc,day_5_mc,day_6_mc,day_7_mc,day_8_mc]

sns.set_style("whitegrid")
colors=[]
boxplot=sns.boxplot(data=marketdata,palette='Set2')
boxplot.axes.set_title("Market capitalization", fontsize=16)
boxplot = sns.stripplot(data=marketdata, marker="o", alpha=0.3, color="black")
boxplot.set_xlabel("Day", fontsize=12)
boxplot.set_ylabel("Values(in USD)", fontsize=12)
boxplot.set_xticklabels(['1','2','3','4','5','6','7','8'])
fig = boxplot.get_figure()
fig.savefig('Plot2.png')
```
*Finally, we visualize the data. We save our plot as a .png image:*
![Image of Plot](https://github.com/IE-555/API/blob/adithya2597/ADITHYAA_BHAVISHA_MANANTEJ_DIKSHITD_cryptodata/images/Plot2.png?raw=true)

*We now take the top 10 cryptocurrencies based on market capitalization and all the data related to it. We now take the code again and extract the data before we print it. The data is converted from the json format.*

```
import http.client

conn = http.client.HTTPSConnection("coingecko.p.rapidapi.com")

headers = {
    'x-rapidapi-key': "063ea1c81cmsh511c1e56374c14dp148a9fjsn29e3fe6cab8a",
    'x-rapidapi-host': "coingecko.p.rapidapi.com"
    }

conn.request("GET", "/coins/markets?vs_currency=usd&page=1&per_page=10&order=market_cap_desc", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```
```
crypto_2 = json.loads(data)
```
```
a = crypto_2[1]
b = crypto_2[2]
c = crypto_2[3]
d = crypto_2[4]
e = crypto_2[5]
f = crypto_2[6]
g = crypto_2[7]
h = crypto_2[8]
i = crypto_2[9]
a.keys()
```

```
currency_name1 =[a['id'],b['id'],c['id'],d['id'],e['id'],f['id'],g['id'],h['id'],i['id']]
market_cap =[a['market_cap'],b['market_cap'],c['market_cap'],d['market_cap'],e['market_cap'],f['market_cap'],g['market_cap'],h['market_cap'],i['market_cap']]
```
*We compare the cryptocurrency in terms of Market Capital (in billions) using the bar plot for the remaining 9 values as we have already compared bitcoin values in the previous plot.*

- *NOTE 3: The data on the y axis is in terms of 10^11*
```
plt.figure(figsize=(10, 5))
plt.bar(range(0,9),market_cap,color=['violet','indigo','blue','green','yellow','orange','red','black','hotpink'],edgecolor ='black',tick_label = currency_name1,width=0.7)
plt.grid(color='#95a5a6', linestyle='--', linewidth=2, axis='y', alpha=0.7)
plt.xlabel('Currency')
plt.ylabel('Market Capital (in billions)')
plt.title('Currency Sales')
plt.savefig('Plot3.png')
```

![Image of Plot](https://github.com/IE-555/API/blob/adithya2597/ADITHYAA_BHAVISHA_MANANTEJ_DIKSHITD_cryptodata/images/Plot3.png?raw=true)

```
high_24h_value =[a['high_24h'],b['high_24h'],c['high_24h'],d['high_24h'],e['high_24h'],f['high_24h'],g['high_24h'],h['high_24h'],i['high_24h']]
high_24h_value_data = np.asarray(high_24h_value)
current_price = [a['current_price'],b['current_price'],c['current_price'],d['current_price'],e['current_price'],f['current_price'],g['current_price'],h['current_price'],i['current_price']]
current_price_data = np.asarray(current_price)
high_24h = (high_24h_value_data - current_price_data)
```
```
plt.figure(figsize=(10, 5))
plt.plot(high_24h, 'H-',linestyle = '--' , color = 'green', mfc = 'red')
plt.xlabel('Currency')
plt.xticks(np.arange(9),currency_name1)
plt.ylabel('Change in highest 24hour with respect to current value (percentage)')
plt.savefig('Plot4.png')
```
*We now compare the change in highest 24hour with respect to current value and plot the graph.*
![Image of Plot](https://github.com/IE-555/API/blob/adithya2597/ADITHYAA_BHAVISHA_MANANTEJ_DIKSHITD_cryptodata/images/Plot4.png?raw=true)

---

## Using Terminal
- *Open a terminal window.*
- *Change directories to where API_cryptodata.py is saved.*
- Type the following command: 
```
python API_cryptodata.py
```
## Using Spyder/ Jupyter
- *Click on File->Open*
- *Choose directory where API_cryptodata is stored*
- *Click on run or press F5 on Spyder, Shift+Enter in Jupyter*

---

## Future scope
- *We can further do in depth and micro analysis of the currencies and minute wise and day wise to understand the purchase and selling pattern.*
* We can filter the data based on their incremental growth or fall in the value and sales

## Authors: **Bhavisha Jayantibhai Kondhol**, **Adithya Arun Kumar**, **Manan Tejas Shah**,**Dikshit Darji**
