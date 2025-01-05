
# How to Create an Amazon Scraper in Python Using ScraperAPI

## Introduction

In this guide, we'll learn how to build an Amazon scraper using Python and [ScraperAPI](https://bit.ly/Scraperapi). Amazon, being one of the largest e-commerce platforms, provides a wealth of data, and this tutorial will show you how to extract it efficiently.

We'll create two scripts:
1. **`fetch.py`**: Fetches individual listing URLs and saves them to a text file.
2. **`parse.py`**: Extracts data from individual URLs and stores it in JSON format.

ScraperAPI will handle the challenges of dynamic rendering and IP blocking, making the entire process seamless. Let's dive in!

---

## Why ScraperAPI?

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Step 1: Fetching Amazon Listing URLs

First, we’ll create a script to fetch URLs from a specific Amazon category and store them in a text file.

### `fetch.py`

```python
import requests
from bs4 import BeautifulSoup

if __name__ == "__main__":
    headers = {
        'authority': 'www.amazon.com',
        'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36',
        'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8',
    }

    API_KEY = None
    links_file = 'links.txt'
    links = []

    with open('API_KEY.txt', encoding='utf8') as f:
        API_KEY = f.read().strip()

    URL_TO_SCRAPE = "https://www.amazon.com/s?i=electronics&rh=n%3A172541"
    payload = {'api_key': API_KEY, 'url': URL_TO_SCRAPE, 'render': 'false'}

    r = requests.get('http://api.scraperapi.com', params=payload, timeout=60)
    if r.status_code == 200:
        soup = BeautifulSoup(r.text, 'lxml')
        links_section = soup.select('h2 > .a-link-normal')
        for link in links_section:
            url = "https://amazon.com" + link['href']
            links.append(url)

    if links:
        with open(links_file, 'a+', encoding='utf8') as f:
            f.write('\n'.join(links))
        print("Links stored successfully.")
```

### Explanation:
- **Headers**: These mimic a browser request to avoid detection.
- **ScraperAPI**: Handles CAPTCHA and IP rotation to scrape data seamlessly.
- **Output**: Extracted URLs are saved in `links.txt`.

---

## Step 2: Parsing Product Data

Now that we have the product links, let’s parse detailed information such as the title, price, and features.

### `parse.py`

```python
import requests
from bs4 import BeautifulSoup

def parse(url):
    record = {}
    headers = {
        'authority': 'www.amazon.com',
        'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36',
    }

    payload = {'api_key': API_KEY, 'url': url, 'render': 'false'}
    r = requests.get('http://api.scraperapi.com', params=payload, timeout=60)

    if r.status_code == 200:
        soup = BeautifulSoup(r.text, 'lxml')
        title_section = soup.select_one('#productTitle')
        price_section = soup.select_one('#priceblock_ourprice')
        availability_section = soup.select_one('#availability')
        features_section = soup.select_one('#feature-bullets')
        asin_section = soup.find('link', {'rel': 'canonical'})

        record = {
            'title': title_section.text.strip() if title_section else None,
            'price': price_section.text.strip() if price_section else None,
            'availability': availability_section.text.strip() if availability_section else None,
            'features': features_section.text.strip() if features_section else None,
            'asin': asin_section['href'].split('/')[-1] if asin_section else None,
        }
    return record

if __name__ == "__main__":
    API_KEY = None
    with open('API_KEY.txt', encoding='utf8') as f:
        API_KEY = f.read().strip()

    result = parse("https://www.amazon.com/dp/B07B49QG1H")
    print(result)
```

### Explanation:
- **Fields Parsed**:
  - Title
  - Price
  - Availability
  - Features
  - ASIN (Amazon Standard Identification Number)
- **Output**: Data is returned as a Python dictionary, which can be saved in JSON format for further use.

---

## Advanced Use Cases

### Scraping Reviews
You can extend this script to extract reviews, ratings, and other user-generated data.

### Automating Tasks
Schedule your script to run periodically for price monitoring, inventory checks, or competitive analysis.

---

## Key Benefits of ScraperAPI

- **Proxy Management**: Automatically rotates IPs to avoid detection.
- **CAPTCHA Handling**: No interruptions due to CAPTCHA challenges.
- **Ease of Use**: Simple API integration for Python developers.
- **Scalability**: Handles millions of requests without hassle.

Start optimizing your scraping workflows today 👉 [Sign up now](https://bit.ly/Scraperapi).

---

## Conclusion

This tutorial demonstrated how to use Python and ScraperAPI to scrape Amazon efficiently. From fetching product URLs to extracting detailed information, ScraperAPI simplifies the entire process while overcoming common challenges like IP blocking and CAPTCHAs.

Enhance this script to suit your needs, whether it’s building a price tracker or creating a comprehensive ASIN scraper. With ScraperAPI, you can focus on your data, not the scraping process.

---

## Disclaimer

Web scraping must be performed ethically and in compliance with Amazon’s terms of service. Always respect the website’s policies and use the data responsibly.
