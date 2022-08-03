# Scrap data from Ebay for Wireless Headphones

from bs4 import BeautifulSoup
import requests
import csv
import pandas as pd

url = "https://www.ebay.com/sch/i.html?_nkw=headphones+bluetooth&_sop=12"
p = requests.get(url)
soup = BeautifulSoup(p.content,'html.parser')
print(p)
content = soup.find_all('li', class_="s-item s-item__pl-on-bottom")
fheader = ["productname","price","sold","orginal"]

itemfull = []

for item in content:
  l1 = []
  productname = item.find('h3', class_="s-item__title")
  price = item.find('span', class_="s-item__price")
  sold = item.find('span', class_="s-item__hotness s-item__itemHotness")
  orginal = item.find('span', class_="STRIKETHROUGH")
  l1.append(productname.text)
  if(price is not None):
    l1.append(price.text)
  else:
    l1.append("Price is NA")
  if(sold is not None):
    l1.append(sold.text)
  else:
    l1.append("No Sold Price")
  if(orginal is not None):
    l1.append(orginal.text)
  else:
    l1.append("No Orginal price")
  itemfull.append(l1)

pd.DataFrame(itemfull).to_csv("ebay.csv",header=fheader)
