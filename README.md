# Tiki Product Crawler

An asynchronous Python crawler that collects product data for **200,000 product IDs** from the Tiki e-commerce API and saves clean, analysis-ready JSON files.

## What it does

1. Reads a list of product IDs from a CSV file.
2. Calls the product API concurrently, with a limit on parallel requests.
3. Keeps only the useful fields: `id`, `name`, `url_key`, `price`, `description`, `images_url`.
4. Cleans the HTML in product descriptions (removes scripts, styles and tags, and normalises whitespace).
5. Saves the results in batches of **1,000 products per file** (`products_000.json`, `products_001.json`, ...).

## Reliability features

| Feature | How |
|---|---|
| Fast | `asyncio` + `aiohttp`, with concurrency capped by a semaphore |
| Rate-limit safe | HTTP 429 handled with exponential backoff and random jitter |
| Resumable | Processed IDs are saved, so a restart continues where it stopped |
| Error tracking | 404s and failures are logged to `not_found_ids.json` and `failed_products.json` |

## Tech stack

Python · asyncio · aiohttp · aiofiles · pandas · regex

## How to run

```bash
pip install aiohttp aiofiles pandas
# Set the path to your product-ID CSV in main.py (list_id = pd.read_csv(...))
python main.py
```
