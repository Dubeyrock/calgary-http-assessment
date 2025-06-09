# Calgary HTTP Log Analysis

## Overview
This repository contains my solution for the “Web Server Log Analysis” take‑home assessment. I analyze the Calgary HTTP dataset (≈1 year of Apache access logs) to extract key usage metrics and patterns.

## Approach
1. **Data Loading**  
   - Downloaded the compressed log from the FTP server.  
   - Stream‑read the `.gz` file using Python’s `gzip` module to minimize memory overhead.  
   - Implemented error handling for download failures and missing files.

2. **Data Cleaning & Parsing**  
   - Used a single regular expression to parse the Apache Common Log Format into components: host, timestamp, request line, status code, and bytes.  
   - Converted timestamp strings to Python `datetime` objects (with timezone) via `datetime.strptime`.  
   - Split the request line into method, resource path, and protocol.  
   - Extracted filenames and file extensions, defaulting missing or malformed values to empty strings.  
   - Coerced numeric fields (`status`, `bytes`) into integers; treated non‑numeric bytes as `0`.  
   - Filtered out any malformed or incomplete log entries.

3. **Feature Engineering**  
   - Added derived columns:  
     - `date_str` (formatted as `dd-MMM-yyyy`) for date‑based grouping.  
     - `hour` (0–23) for hourly traffic analysis.

4. **Analysis Questions (Q1–Q10)**  
   - Implemented each question as a standalone function in the notebook.  
   - Used Python’s standard library (`collections.Counter`, `defaultdict`) and `pandas` for grouping and aggregation where appropriate.  
   - Ensured reproducible sorting and formatting of outputs.

## Assumptions & Decisions
- **Malformed Lines**: Any line that doesn’t match the expected log regex or has unparsable timestamp/request is skipped.  
- **Missing Bytes**: Treated `-` or non‑digit bytes values as `0` when computing bandwidth.  
- **Empty Filenames**: If a request’s resource has no filename (e.g. just `/`), I included the raw resource string as the “filename” in counts.  
- **Time Zone**: The dataset’s timestamps include a timezone offset; I preserved it when parsing but normalized dates for grouping.

## Tools & Libraries
- **Python 3.9+**  
- **Standard Library**: `gzip`, `urllib.request`, `re`, `datetime`, `collections`  
- **Third‑Party**: `pandas` (for DataFrame operations in Parts 1–2), `jupyter` (notebook environment)

## Challenges
- **Memory Efficiency**: Loading a 52 MB text file into memory required careful use of generators/streaming to avoid excessive RAM usage.  
- **Parsing Variability**: Some log lines had unexpected formats; crafting a robust regex and fallback logic was critical.  
- **Date Formatting**: Converting timezone‑aware datetimes to a consistent `dd-MMM-yyyy` string required attention to Python’s `%z` specifier.

---

![31a4e93426e3b6a644052c84488ea3d0ae16fe0d5146fea39064ae7f_tmpn_5p6tze](https://github.com/user-attachments/assets/3548f60e-d292-4809-9dea-a540acf9ae1b)




