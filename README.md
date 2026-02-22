# AI-Powered Invoice Data Extraction Pipeline

An end-to-end Machine Learning proof of concept (PoC) designed to extract, structure, and classify data from an unstructured invoice image. This project demonstrates the integration of Optical Character Recognition (OCR) with state-of-the-art Natural Language Processing (NLP) models to automate complex data entry and parsing tasks.

## Scope and Computational Limitations

Please note that this repository currently features a pipeline optimized to process a **single invoice**. 

The architecture relies on heavy, state-of-the-art transformer models (`facebook/bart-large-mnli` and `deepset/roberta-base-squad2`). Due to the significant computational constraints of running these large models locally on a standard CPU, batch processing multiple invoices is not feasible in this environment without extended inference times or GPU acceleration. Therefore, this project serves strictly as a functional PoC to demonstrate the methodology and accuracy of the extraction logic.

## Features

* **Intelligent Data Extraction:** Utilizes Question-Answering (QA) models to extract highly specific metadata (Invoice Number, Date, Tax IDs, IBAN).
* **Entity Parsing:** Accurately identifies and separates Seller and Client information (Name, Street, City, State, ZIP) from raw text blocks.
* **Zero-Shot Item Classification:** Automatically categorizes line items into predefined classes (`Hardware Device`, `Software License`, `Service Labor`, `Shipping Fee`).
* **Hybrid Logic Engine:** Combines AI predictions with deterministic business rules (Regex and keyword overrides) to correct edge cases and ensure maximum accuracy.
* **Structured Export:** Generates a human-readable text report (`.txt`) and a database-ready flat file (`.csv`).

## Tech Stack

* **Language:** Python
* **Computer Vision / OCR:** For extracting raw text and bounding boxes from the initial image.
* **NLP & Deep Learning:** `transformers` (Hugging Face), `PyTorch`
* **Models:**
    * `deepset/roberta-base-squad2`: Utilized for precise Question Answering and entity extraction.
    * `facebook/bart-large-mnli`: Utilized for Zero-Shot text classification of line items.
* **Data Manipulation:** `pandas`, `re` (Regular Expressions)

## How It Works

1. **Text Recognition (OCR):** The raw invoice image is scanned to extract text blocks and their spatial coordinates.
2. **Context Assembly:** Text is grouped into logical zones (Header, Seller, Client, Items) based on their coordinate proximity.
3. **Question Answering (QA):** The RoBERTa model queries the grouped text to find specific entities (e.g., "What is the IBAN number?").
4. **Classification & Business Rules:** Line items are passed to the BART model for categorization. A secondary business rule layer corrects any domain-specific edge cases.
5. **Output Generation:** The assembled, structured data is exported to `.csv` and `.txt` formats for downstream database integration.
