# WEB-CRAWLER

#packages used to run different functions
import requests
from bs4 import BeautifulSoup
import pandas as pd


url = 'https://www.flipkart.com/laptops/~buyback-guarantee-on-laptops-/pr?sid=6bo%2Cb5g&uniqBStoreParam1=val1&wid=11.productCard.PMU_V2.'
source_code = requests.get(url)   #fetching the data of the site
plain_text = source_code.text

products=[] #List to store name of the product
prices=[] #List to store price of the product
ratings=[] #List to store rating of the product
soup = BeautifulSoup(plain_text,"html.parser")
for a in soup.findAll('a',href=True, attrs={'class':'_31qSD5'}):
    name=a.find('div', attrs={'class':'_3wU53n'})
    price=a.find('div', attrs={'class':'_1vC4OE _2rQ-NK'})
    rating=a.find('div', attrs={'class':'hGSR34'})
    products.append(name.text)
    prices.append(price.text)
    ratings.append(rating.text)
print(products)

#code for storing data in .csv file
df = pd.DataFrame({'Product Name':products,'Price':prices,'Rating':ratings})
df.to_csv('products.csv', index=False, encoding='utf-8')
