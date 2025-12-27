# Puma Women’s Shoes Scraper

## Overview
This project scrapes product data from the **Puma India – Women’s Shoes** section.  
Since the website loads content dynamically using JavaScript, a browser-based scraping approach was required.

The scraper loads the page, extracts product details, scrolls to load more items, and continuously saves the data into a CSV file.

---

## Dataset
The script generates a CSV file named:


### Fields Scraped
- Product Name  
- Category  
- Sale Price  
- Original Price  
- Discount Percentage  
- Discount Value  
- Image URL  
- Total Colours Available  
- Absolute Product URL  

The CSV file is saved in the **root directory** and is updated **after every scroll cycle**, ensuring no data is lost during execution.

---

## Scraping Approach

1. **Browser Automation**
   - Used **Selenium with Chrome** to handle JavaScript-rendered content.
   - Static scraping tools like `requests` were not sufficient.

2. **Initial Load Scraping**
   - The scraper waits for the page to fully load.
   - Extracts the initially visible products (around 24 items).

3. **Scroll → Wait → Scrape Loop**
   - The page is scrolled gradually to trigger lazy loading.
   - After each scroll:
     - The DOM is parsed again
     - Newly loaded products are extracted
     - Products are deduplicated using their absolute URLs
     - The CSV file is saved immediately

4. **Stopping Condition**
   - Scraping stops when:
     - No new products load after multiple scroll attempts, or
     - The predefined limit of **100 products** is reached

This avoids relying on page height, which is unreliable for this website.

---

## Challenges Faced

### Dynamic Content Loading
- Product listings load only after JavaScript execution.

**How it was handled:**  
Selenium was used to render the page, and BeautifulSoup was used to parse the loaded HTML after each scroll.

---

### False End-of-Page Detection
- Page height does not always change even when new products load.

**How it was handled:**  
The scraper tracks the **number of products scraped** instead of page height and continues scrolling until the count stops increasing.

---

### Bot Sensitivity
- Puma restricts aggressive or unnatural scraping behaviour.

**How it was handled:**  
- Added delays between scrolls  
- Used stable `data-test-id` attributes instead of dynamic class names  
- Limited the scraping speed and total product count  

---

## Limitations
- The scraper is intentionally limited to **100 products**.
- Reviews, ratings, and size availability are not scraped.
- Frequent or high-volume scraping may trigger bot protection.
- Requires Chrome and ChromeDriver to be installed.

---

## Conclusion
This scraper reliably extracts structured product data from a dynamic e-commerce website.  
The approach is stable, incremental, and fault-tolerant, with data being saved after every scroll cycle.
