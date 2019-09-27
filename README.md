# Web-Scrapping
It's a program code on Python which will fetch the product name , price and the product link from Flipkart

from bs4 import BeautifulSoup as soup
from urllib.request import urlopen as uReq


l=[]
for i in range (1,51):
    l.append(i)

filename="flipkart.csv"
f=open(filename,"w")
headers="product_name,price\n"
f.write(headers)

for page in l:
    my_url="https://www.flipkart.com/search?q=mobiles&sid=tyy%2C4io&as=on&as-show=on&otracker=AS_QueryStore_OrganicAutoSuggest_0_6_na_na_pr&otracker1=AS_QueryStore_OrganicAutoSuggest_0_6_na_na_pr&as-pos=0&as-type=RECENT&suggestionId=mobiles&requestId=451f4f84-a206-4ee7-923b-63b41d0bba1b&as-backfill=on&page={}".format(page)
    print(page)
    uclient=uReq(my_url)
    page_html=uclient.read()
    uclient.close()
    page_soup=soup(page_html, "html.parser")
    containers= page_soup.findAll("div",{"class": "_3O0U0u"})
    container=containers[0]
    for container in containers:
        product_name=container.div.img["alt"]
    
        price_container=container.findAll("div",{"class": "col col-5-12 _2o7WAb"})
        price=price_container[0].text.strip()
        
        link=container.div.a["href"]
    
        #print(product_name)
        #print(price)
    
        trim_price=''.join(price.split(','))
        rupees=trim_price.split("₹")
        add_rs="Rs." + rupees[1]
        split_price=add_rs.split('E')
        ncost_price=split_price[0]
        split_ncost=ncost_price.split('N')
        final_price=split_ncost[0]
    
        print(product_name.replace(",","|") + "," + str(final_price)+ "," + "www.flipkart.com" + link + "\n")
        f.write(product_name.replace(",","|") + "," + str(final_price)+ "," + "www.flipkart.com" + link + "\n")
f.close()
