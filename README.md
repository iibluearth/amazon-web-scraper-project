# 🛒 Amazon Web Scraper Project

This project is a **Python-based Amazon price tracker** that scrapes product details (title and price) from locally saved Amazon product pages (`.txt` or `.html` files). It exports the results into **CSV/Excel files**, runs on a **daily schedule**, and can even **send email reports**.

🔗 **[View Data Whisperer Funny Science Analyst Software Engineer T-Shirt](https://www.amazon.com/Data-Whisperer-Funny-Science-Software/dp/B07L85PM3K/ref=sr_1_43?crid=15UVVSN239FDK&dib=eyJ2IjoiMSJ9.zTJ7b_RGZJyx9vSA5hv059bJeEtarnmm2ASvXooJ7b7MBdJLeStf2pOVVNq59z1OZr6xQYIi-3zJYDOvKms0CBCdmPKHwz9SODAqXYMRH9YHEjlTocQan-ctGHraK-8v7zJTZsLFnuFKJu1so-ec_1SyzeHOLfI8rmWlMJYlGUNO3Tyo9bhFRVkf1tPAogfdeia87RTIcXKmI7Ge5nBmt7YRNZDq4bIERGoEQB9KR-f2fD2B2gWLXQjsDFHEefhW3HYpdnDTAaDGEbW9uRYA6gZSjwS4pQK-n-rR2K_Ibmg.1JEw1LhRonoBsda8l_D38GzppAEGOh-rzohMUJ5GVag&dib_tag=se&keywords=data+funny&qid=1759000516&sprefix=data+funny+%2Caps%2C117&sr=8-43)

---

## 📂 Project Structure

```
amazon_web_scraper_project/
│── amazon_page.txt              # Example saved Amazon page
│── amazon_prices.csv            # CSV log of scraped data
│── amazon_prices.xlsx           # Excel export of scraped data
│── amazon_scraper.py            # Main scraper script
│── pages/                       # Folder containing saved Amazon TXT/HTML files
```

---

## 🛠️ Features

- ✅ **Scrape Product Info** – Extracts product title & price from saved Amazon pages

- ✅ **CSV Logging** – Appends scraped data with date/time & source file

- ✅ **Excel Export** – Converts CSV log to Excel format with Pandas

- ✅ **Daily Automation** – Runs the scraper loop every 24 hours

- ✅ **Email Reports** – Sends email updates using Gmail SMTP

---

## 🚀 How It Works

### 1. Import Libraries

```
from bs4 import BeautifulSoup
import os, csv, datetime, time, pandas as pd, smtplib
```
---

### 2. Scraper Function

Reads a saved Amazon page (`.txt` or `.html`) and extracts product title + price.

```
def get_price_from_txt(txt_file):
    with open(txt_file, "r", encoding="utf-8") as f:
        html_content = f.read()
    soup = BeautifulSoup(html_content, "lxml")

    title = soup.find("span", {"id": "productTitle"})
    price = soup.find("span", {"class": "a-price-whole"})

    title_text = title.get_text(strip=True) if title else "N/A"
    price_text = price.get_text(strip=True) if price else "N/A"

    return title_text, price_text
```

### ✅ Check If Passed:

```
title, price = get_price_from_txt("amazon_page.txt")
print("Title:", title)
print("Price:", price)
```

### ✅ Example Output:

```
Title: Data Whisperer Funny Science Analyst Software Engineer T-Shirt
Price: 14.
```

---

### 3. Export Data to CSV

Initializes a CSV log with headers:

```
csv_file = "amazon_prices.csv"
with open(csv_file, "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerow(["Date", "Time", "Product Title", "Price", "Source File"])
```

---

### 4. Process Multiple Files

Loops through all `.txt` and `.html` files in the `pages/` folder:

```
folder_path = r"C:\Users\name\my_notebook.ipynb\amazon_web_scaper_project\pages"

for file in os.listdir(folder_path):
    if file.endswith((".txt", ".html")):
        title, price = get_price_from_txt(os.path.join(folder_path, file))
        now = datetime.datetime.now()
        with open(csv_file, "a", newline="", encoding="utf-8") as f:
            writer = csv.writer(f)
            writer.writerow([now.date(), now.time(), title, price, file])
```

## ✅ Example Output:

```
💾 Saved from page1.txt: Data Whisperer Funny Science Analyst Software Engineer T-Shirt | 14.
💾 Saved from page2.html: Data Whisperer Funny Science Analyst Software Engineer T-Shirt | 14.
```

---

### 5. Run Daily

Keeps the scraper running every 24 hours:

```
while True:
    print("⏳ Running Amazon scraper...")
    # scraping logic
    print("✅ Done! Waiting 24 hours...\n")
    time.sleep(86400)  # 1 day
```

---

### 6. Load with Pandas

```
df = pd.read_csv("amazon_prices.csv")
print(df.head())
```

---

### 7. Save as Excel

```
excel_file = "amazon_prices.xlsx"
df.to_excel(excel_file, index=False, engine="openpyxl")
```

---

### 8. Send Email Report

```
sender_email = "email@gmail.com"
app_password = "16-character code"
receiver_email = "email@gail.com"

server = smtplib.SMTP_SSL("smtp.gmail.com", 465)
server.login(sender_email, app_password)
server.sendmail(sender_email, receiver_email, "Subject: Test\n\nThis is a test email from Python!")
server.quit()
```

### ✅ Example Output:

```
📧 Email sent successfully!
```

---

## 📧 Email Setup

1. Enable 2FA on Gmail
2. Create an App Password (16-character code)
3. Replace sender_email, app_password, and receiver_email in the script

---

## 📊 Example Output
CSV File (`amazon_prices.csv`)

```
| Date       | Time     | Product Title                                         | Price | Source File |
| ---------- | -------- | ----------------------------------------------------- | ----- | ----------- |
| 2025-09-29 | 12:34:56 | Data Whisperer Funny Science Analyst Software T-Shirt | 14    | page1.txt   |
```

Excel File (`amazon_prices.xlsx`) – Same data, Excel-friendly format.

---

## ⚠️ Notes

- This scraper works with saved Amazon pages, not live requests (Amazon blocks direct scrapers).
- Save product pages manually as .txt or .html for input.
- Email reporting requires a Gmail App Password (with 2FA enabled).
