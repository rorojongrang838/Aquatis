
# How to Scrape Etsy Product Data with Python

## Introduction

Etsy has become a $30 billion e-commerce giant, boasting over 4.4 million active sellers and nearly 82 million buyers, according to [Statista](https://www.statista.com/statistics/409375/etsy-active-buyers/). As one of the largest online marketplaces for unique, handmade, and creative goods, Etsy offers valuable insights for businesses.

If you're looking to break into a specific industry, analyze product trends, or gather pricing data, building an Etsy web scraper can save you time and uncover lucrative opportunities.

In this tutorial, we'll use Python's `Requests` and `BeautifulSoup` libraries to scrape Etsy product data, step by step.

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Etsy, Amazon, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## TL;DR: Complete Code

Here’s the complete code snippet for those in a hurry:

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

etsy_products = []

for x in range(1, 251):
    response = requests.get(
        f'https://api.scraperapi.com?api_key=SCRAPE9837861&url=https://www.etsy.com/c/clothing/boys-clothing/costumes?ref=pagination&explicit=1&page={x}')
    soup = BeautifulSoup(response.content, 'html.parser')

    products = soup.select('li.wt-show-md.wt-show-lg')
    for product in products:
        product_name = product.select_one('h3.v2-listing-card__title').text.strip()
        product_price = product.select_one('span.currency-value').text
        product_url = product.select_one('li.wt-list-unstyled div.js-merch-stash-check-listing a.listing-link')['href']
        etsy_products.append({
            'name': product_name,
            'price': product_price,
            'URL': product_url
        })

df = pd.DataFrame(etsy_products)
df.to_csv('etsy-products.csv', index=False)
```

### **Note:** 
Replace `SCRAPE9837861` with your ScraperAPI key.

---

## Step-by-Step Guide to Building an Etsy Scraper

### **Step 1: Install Dependencies**

To get started, install the required libraries:

```bash
pip install requests beautifulsoup4 pandas
```

Once installed, create a new directory for your project and a file named `etsy-scraper.py` to house your script.

---

### **Step 2: Download Etsy HTML for Parsing**

Start by sending an HTTP request to Etsy to get the raw HTML for the page you want to scrape:

```python
import requests
from bs4 import BeautifulSoup

response = requests.get(
    'https://www.etsy.com/c/clothing/boys-clothing/costumes?ref=pagination&explicit=1&page=1')
```

---

### **Step 3: Use CSS Selectors to Target Data**

To extract specific data from the HTML, such as product names, prices, and URLs, you'll need the correct CSS selectors. Use browser inspection tools (Ctrl+Shift+C on Windows or Cmd+Shift+C on Mac) or a tool like SelectorGadget.

Here are the key CSS selectors for the elements on the page:
- **Product Name**: `.v2-listing-card__title`
- **Product Price**: `.currency-value`
- **Product URL**: `div.js-merch-stash-check-listing a.listing-link`

---

### **Step 4: Parse and Extract Data**

Use `BeautifulSoup` to parse the HTML and extract the desired data:

```python
soup = BeautifulSoup(response.content, 'html.parser')
products = soup.select('li.wt-show-md.wt-show-lg')

for product in products:
    product_name = product.select_one('h3.v2-listing-card__title').text.strip()
    product_price = product.select_one('span.currency-value').text
    product_url = product.select_one('div.js-merch-stash-check-listing a.listing-link')['href']

    print(f"Name: {product_name}, Price: {product_price}, URL: {product_url}")
```

---

### **Step 5: Export Data to a CSV File**

Instead of printing the data to the console, store it in a Pandas DataFrame and save it as a CSV:

```python
import pandas as pd

etsy_products = []

for product in products:
    product_name = product.select_one('h3.v2-listing-card__title').text.strip()
    product_price = product.select_one('span.currency-value').text
    product_url = product.select_one('div.js-merch-stash-check-listing a.listing-link')['href']
    etsy_products.append({
        'name': product_name,
        'price': product_price,
        'URL': product_url
    })

df = pd.DataFrame(etsy_products)
df.to_csv('etsy-products.csv', index=False)
```

---

## Scraping Etsy at Scale

Now that we’ve successfully scraped one page, let’s scale it up to scrape multiple pages in the category.

### **Step 6: Loop Through Pagination**

Etsy paginates its results, and the page number is included in the URL as a query parameter (e.g., `?page=2`). To scrape all pages, use a loop:

```python
for x in range(1, 251):  # Assuming there are 250 pages
    response = requests.get(
        f'https://www.etsy.com/c/clothing/boys-clothing/costumes?ref=pagination&explicit=1&page={x}')
    # Add parsing logic here
```

---

### **Step 7: Avoid Getting Blocked with ScraperAPI**

To avoid IP bans and CAPTCHAs while scraping multiple pages, integrate ScraperAPI. ScraperAPI automates IP rotation, handles CAPTCHAs, and manages headers for you.

Modify your request to route through ScraperAPI:

```python
response = requests.get(
    f'https://api.scraperapi.com?api_key=SCRAPE9837861&url=https://www.etsy.com/c/clothing/boys-clothing/costumes?ref=pagination&explicit=1&page={x}')
```

Replace `SCRAPE9837861` with your ScraperAPI key.

---

### **Step 8: Combine Everything**

Here’s the full script for scraping Etsy at scale:

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

etsy_products = []

for x in range(1, 251):
    response = requests.get(
        f'https://api.scraperapi.com?api_key=SCRAPE9837861&url=https://www.etsy.com/c/clothing/boys-clothing/costumes?ref=pagination&explicit=1&page={x}')
    soup = BeautifulSoup(response.content, 'html.parser')

    products = soup.select('li.wt-show-md.wt-show-lg')
    for product in products:
        product_name = product.select_one('h3.v2-listing-card__title').text.strip()
        product_price = product.select_one('span.currency-value').text
        product_url = product.select_one('div.js-merch-stash-check-listing a.listing-link')['href']
        etsy_products.append({
            'name': product_name,
            'price': product_price,
            'URL': product_url
        })

df = pd.DataFrame(etsy_products)
df.to_csv('etsy-products.csv', index=False)
```

---

## Conclusion

With this guide, you’ve learned how to:
1. Scrape product names, prices, and URLs from Etsy using Python.
2. Use `BeautifulSoup` and CSS selectors to parse HTML.
3. Export scraped data to a CSV file.
4. Scale your scraper to handle multiple pages with pagination.
5. Use ScraperAPI to bypass anti-scraping measures.

Start applying these techniques to other Etsy categories or e-commerce platforms to unlock valuable data insights.

---

**Scrape smarter, not harder! Let ScraperAPI handle the heavy lifting so you can focus on extracting the data you need. 👉 [Start your free trial now!](https://bit.ly/Scraperapi)**

Happy scraping!
