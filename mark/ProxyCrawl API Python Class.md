
# ProxyCrawl API Python Class

## Introduction

The ProxyCrawl API Python class is a lightweight, dependency-free tool that serves as a wrapper for the ProxyCrawl API. It simplifies web scraping by providing methods to extract data from websites effectively.

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

### Installation

You can install the ProxyCrawl API Python class using one of the following methods:

1. Download the Python class from the [GitHub repository](https://github.com/crawlbase-source/crawlbase-python).
2. Use PyPi's package manager:

   ```bash
   pip install proxycrawl
   ```

### Importing the Class

After installation, you can import the required modules:

```python
from proxycrawl import CrawlingAPI, ScraperAPI, LeadsAPI, ScreenshotsAPI, StorageAPI
```

---

## Upgrading to Version 3

Version 3 introduces `CrawlingAPI` as the preferred interface over `ProxyCrawlAPI`. While `ProxyCrawlAPI` remains usable, upgrading is recommended. Test the upgrade before deploying to production.

### Initialization Example

```python
api = CrawlingAPI({ 'token': 'YOUR_PROXYCRAWL_TOKEN' })
```

Pass the URL and any options as defined in the [API documentation](https://proxycrawl.com/docs).

### Example Usage

#### GET Request

```python
response = api.get('https://www.facebook.com/britneyspears')
if response['status_code'] == 200:
    print(response['body'])
```

#### POST Request

Send data in JSON or string format along with optional parameters:

```python
response = api.post('https://producthunt.com/search', { 'text': 'example search' })
if response['status_code'] == 200:
    print(response['body'])
```

#### JavaScript Rendering

To scrape JavaScript-heavy websites like React or Angular-based sites:

```python
response = api.get('https://www.nfl.com')
if response['status_code'] == 200:
    print(response['body'])
```

You can also pass additional JavaScript-specific options:

```python
response = api.get('https://www.freelancer.com', { 'page_wait': 5000 })
if response['status_code'] == 200:
    print(response['body'])
```

---

## Original Status

The response includes both the original HTTP status and the ProxyCrawl status. Learn more in the [ProxyCrawl documentation](https://proxycrawl.com/docs).

```python
response = api.get('https://craiglist.com')
print(response['headers']['original_status'])
print(response['headers']['pc_status'])
```

---

## Screenshots API

The Screenshots API allows you to capture website screenshots with ease. Initialize the API using your token:

```python
screenshots_api = ScreenshotsAPI({ 'token': 'YOUR_SCREENSHOT_TOKEN' })
```

You can optionally specify file paths or set `store=true` to get the `screenshot_url` in the returned headers.

---

## Storage API

The Storage API lets you interact with data stored in the ProxyCrawl dashboard. Initialize it using your token:

```python
storage_api = StorageAPI({ 'token': 'YOUR_NORMAL_TOKEN' })
```

### Example Operations

#### Fetch RIDs

Retrieve a list of RIDs:

```python
rids = storage_api.rids()
print(rids)
```

Specify a limit for the number of RIDs:

```python
storage_api.rids(100)
```

#### Get Total Document Count

Get the total number of documents in your storage area:

```python
total_count = storage_api.totalCount()
print(total_count)
```

#### Custom Timeout

You can define a custom timeout in seconds when initializing the API:

```python
api = CrawlingAPI({ 'token': 'TOKEN', 'timeout': 120 })
```

---

## Important Notes

- For additional details, refer to the [ProxyCrawl API documentation](https://proxycrawl.com/docs).
- This package is no longer maintained or supported. For the latest updates, use the [Crawlbase Python Package](https://github.com/crawlbase-source/crawlbase-python).

---

**Start scraping smarter today with [ScraperAPI](https://bit.ly/Scraperapi). Get structured data without hassle!**
