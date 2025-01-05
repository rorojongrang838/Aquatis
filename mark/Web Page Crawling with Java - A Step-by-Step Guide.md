
# Web Page Crawling with Java - A Step-by-Step Guide

## Introduction

Web page crawling, also known as web scraping, is a powerful technique for extracting data from websites. In this guide, we’ll explore how to build a web crawler using Java's `HttpClient`. We'll implement a `Crawler` class capable of synchronous and asynchronous crawling, configure crawling depths, extract links from pages, and store crawled files in a directory. Additionally, we'll validate the crawler's functionality with JUnit 5 tests. Let’s dive into the fascinating world of web page crawling with Java!

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## Prerequisites

To follow this tutorial, ensure you have the following:

- Java Development Kit (JDK) installed.
- Apache Maven for dependency management. Download it from the [official Maven website](https://maven.apache.org/download.cgi) or install via [SDKMAN](https://sdkman.io/sdks#maven).
- Clone the example repository to your local environment:

```bash
git clone https://github.com/dmakariev/examples.git
cd examples/java-core/crawler
```

## Creating a Maven Project

### Steps to Generate the Maven Project

1. Open your terminal and navigate to the directory where you want to create your project.
2. Run the following command to generate a Maven project structure:

```bash
mvn archetype:generate -DgroupId=com.makariev.examples.core -DartifactId=crawler \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DarchetypeVersion=1.4 \
-DinteractiveMode=false
```

This will create a basic Maven project with a sample Java class. The `groupId` and `artifactId` are specified as per your project requirements.

---

## Implementing the Crawler

### The `Crawler` Class

The `Crawler` class will handle both synchronous and asynchronous crawling. It will allow you to:

- Configure the crawling depth.
- Extract links from web pages.
- Save crawled pages to a specified directory.

---

### Link Extraction

To extract links from crawled pages, we'll use two approaches:

1. **PlainLinkExtractor**: A simple implementation using regular expressions.
2. **JsoupLinkExtractor**: A robust solution using the popular Jsoup library.

#### PlainLinkExtractor

The `PlainLinkExtractor` provides a basic link extraction mechanism using regex. Note: While suitable for basic scenarios, regular expressions can struggle with complex HTML structures. For production use, consider more advanced parsing tools.

#### JsoupLinkExtractor

The `JsoupLinkExtractor` utilizes the Jsoup library to simplify HTML parsing and link extraction. This approach is more reliable and handles modern web structures effectively.

---

### Writing JUnit 5 Tests

To ensure our crawler works as expected, we'll create unit tests using JUnit 5. These tests will verify both synchronous and asynchronous crawling, as well as link extraction.

#### Example: Testing the `Crawler` Class

Below is an example of JUnit 5 tests for the `Crawler` class:

```java
package com.makariev.examples.core;

import java.io.IOException;
import java.net.URI;
import java.nio.file.Files;
import java.nio.file.Path;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;

public class CrawlerTest {

    private static final Path CRAWL_DIR = Path.of("crawled-pages");

    @Test
    void testCrawlWebPagesSynchronouslyPlain() throws IOException {
        final Crawler crawler = new Crawler(new PlainLinkExtractor());
        crawler.crawlSynchronously(URI.create("https://example.com"), 2);

        // Verify that crawled pages are saved in the directory
        assertThat(Files.list(CRAWL_DIR).count()).isGreaterThanOrEqualTo(1);
    }

    @Test
    void testCrawlWebPagesAsynchronouslyPlain() throws IOException {
        final Crawler crawler = new Crawler(new PlainLinkExtractor());
        crawler.crawlAsynchronously(URI.create("https://example.com"), 2);

        // Verify that crawled pages are saved in the directory
        assertThat(Files.list(CRAWL_DIR).count()).isGreaterThanOrEqualTo(1);
    }

    @Test
    void testCrawlWebPagesSynchronouslyJSoup() throws IOException {
        final Crawler crawler = new Crawler(new JsoupLinkExtractor());
        crawler.crawlSynchronously(URI.create("https://example.com"), 2);

        // Verify that crawled pages are saved in the directory
        assertThat(Files.list(CRAWL_DIR).count()).isGreaterThanOrEqualTo(1);
    }

    @Test
    void testCrawlWebPagesAsynchronouslyJSoup() throws IOException {
        final Crawler crawler = new Crawler(new JsoupLinkExtractor());
        crawler.crawlAsynchronously(URI.create("https://example.com"), 2);

        // Verify that crawled pages are saved in the directory
        assertThat(Files.list(CRAWL_DIR).count()).isGreaterThanOrEqualTo(1);
    }
}
```

---

## Running the Tests

To execute the tests, run the following command from the project's root directory:

```bash
mvn test
```

JUnit 5 and AssertJ will validate the tests, providing detailed output on pass/fail statuses.

---

## Conclusion

In this tutorial, we explored web page crawling with Java's `HttpClient`. We implemented a versatile `Crawler` class that supports both synchronous and asynchronous crawling. Additionally, we:

- Configured crawling depths.
- Extracted links using both regular expressions and Jsoup.
- Saved crawled data to a specified directory.

Web page crawling is a valuable skill for data collection, competitive analysis, and more. With the tools and techniques covered here, you can build scalable web scrapers tailored to your needs.

---

Ready to take your scraping projects to the next level? **[Sign up for ScraperAPI](https://bit.ly/Scraperapi)** and access millions of data points without worrying about proxies or blocks!

---

![Coffee Break](https://www.makariev.com/assets/img/blog/scifi-coffee-2.jpg)
