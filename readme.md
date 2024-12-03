# **secfi Library**
**secfi** is a Python library designed to simplify access to SEC (U.S. Securities and Exchange Commission) filings and perform basic web scraping of the retrieved documents.

- [Installation](#installation)
- [Features](#features)
  - [1. `getCiks()`](#1-getciks)
  - [2. `getFils(ticker: str)`](#2-getfils-ticker-str)
  - [3. `scrapLatest(ticker: str, form: str)`](#3-scraplatest-ticker-str-form-str)
  - [4. `scrap(url: str, timeout: int = 15)`](#4-scrap-url-str-timeout-int--15)
- [Notes](#notes)
- [License](#license)


```bash
# Installation
pip install secfi
```

## Features

### 1. `getCiks()`
Fetches a DataFrame of all company tickers and their corresponding Central Index Keys (CIKs).

```python
import secfi

ciks = secfi.getCiks()
print(ciks.head())
```

**Returns:**
A DataFrame with columns:
- `cik_str` – The raw CIK string.
- `title` – The company name.
- `cik` – The CIK padded to 10 digits (for SEC queries).

| ticker | cik_str  | title                        | cik        |
|--------|----------|------------------------------|------------|
| NVDA   | 1045810  | NVIDIA CORP                 | 0001045810 |
| AAPL   | 320193   | Apple Inc.                  | 0000320193 |
| MSFT   | 789019   | MICROSOFT CORP              | 0000789019 |
| AMZN   | 1018724  | AMAZON COM INC              | 0001018724 |
| GOOGL  | 1652044  | Alphabet Inc.               | 0001652044 |
| ...    | ...      | ...                          | ...        |

---

### 2. `getFils(ticker: str)`
Fetches recent filings for a specific company by its ticker.

```python
filings = secfi.getFils("AAPL")
print(filings.head())
```

**Parameters:**
- `ticker` (str): The company's ticker symbol.

**Returns:**
A DataFrame like:

| accessionNumber      | filingDate | reportDate | acceptanceDateTime     | form     | filmNumber | size    | isXBRL | url                                           |
|----------------------|------------|------------|-------------------------|----------|------------|---------|--------|-----------------------------------------------|
| 0001018724-24-000161 | 2024-11-01 | 2024-09-30 | 2024-10-31T18:44:54.000Z | 10-Q     | 241416538  | 9185722 | 1      | [Link](https://www.sec.gov/Archives/edgar/data/0001018724-24-000161) |
| 0001018724-24-000130 | 2024-08-02 | 2024-06-30 | 2024-08-01T18:39:50.000Z | 10-Q     | 241168331  | 8114974 | 1      | [Link](https://www.sec.gov/Archives/edgar/data/0001018724-24-000130) |
| 0001018724-24-000083 | 2024-05-01 | 2024-03-31 | 2024-04-30T18:38:29.000Z | 10-Q     | 24899170   | 7428154 | 1      | [Link](https://www.sec.gov/Archives/edgar/data/0001018724-24-000083) |
| 0001104659-24-045910 | 2024-04-11 | 2024-05-22 | 2024-04-10T18:14:29.000Z | DEF 14A  | 24836785   | 8289378 | 1      | [Link](https://www.sec.gov/Archives/edgar/data/0001104659-24-045910) |
| 0001018724-24-000008 | 2024-02-02 | 2023-12-31 | 2024-02-01T18:48:30.000Z | 10-K     | 24588330   | 12110804| 1      | [Link](https://www.sec.gov/Archives/edgar/data/0001018724-24-000008) |
| 0001018724-23-000018 | 2023-10-27 | 2023-09-30 | 2023-10-26T18:36:51.000Z | 10-Q     | 231351529  | 7894342 | 1      | [Link](https://www.sec.gov/Archives/edgar/data/0001018724-23-000018) |
| ...                  | ...        | ...        | ...                     | ...      | ...        | ...     | ...    | ...                                           |


---

### 3. `scrapLatest(ticker: str, form: str)`
Retrieves the textual content of the latest SEC filing of a specific form type for a given ticker.

```python
text = secfi.scrapLatest("AAPL", "10-K")
print(text[:500])  # Preview the first 500 characters
```

**Parameters:**
- `ticker` (str): The company's ticker symbol.
- `form` (str): The form type to retrieve (e.g., "10-K", "8-K").

**Returns:**
A string containing the cleaned text content of the filing.

---

### 4. `scrap(url: str, timeout: int = 15)`
Scrapes the textual content of a given URL.

```python
content = secfi.scrap("https://example.com")
print(content[:500])  # Preview the first 500 characters
```

**Parameters:**
- `url` (str): The URL to scrape.
- `timeout` (int): Timeout for the HTTP request (default is 15 seconds).

**Returns:**
The cleaned text content of the URL or an error message if the request fails.

---

## Notes
- The library uses a custom `User-Agent` to comply with SEC API requirements.
- Ensure that requests to the SEC website respect their usage policies and rate limits.

## License
This project is open source and available under the MIT License.




