# vsign
A browser-based, zero-installation HTML tool for advanced 3-source financial reconciliation, featuring intelligent data parsing and automated commission analysis.
# ⭐ Advanced Reconciliation Pro v2 (Care4Sign)

A powerful, browser-based, no-backend-required tool for complex financial reconciliation. This application is specifically designed to compare three distinct data sources—a primary sales report, a payment confirmation file, and a commission statement—to identify discrepancies with high precision.

It runs entirely in your web browser, ensuring that your sensitive financial data never leaves your computer.


*(Replace this with an actual screenshot of the application)*

---

## ✨ Key Features

-   **Three-Source Comparison**: Simultaneously reconciles a primary report (S1), a paid source (S2), and a commission source (S3).
-   **Intelligent Data Parsing**: Automatically extracts the 8-digit Application ID from a free-text "Remarks" column in the commission file using regular expressions.
-   **Automated Commission Analysis**:
    -   Calculates the GST-exclusive base amount from your primary report's total price.
    -   Compares the actual commission received (from S3) against a desired percentage of the base amount.
    -   Aggregates commission amounts for the same App ID, even if they are split across multiple rows (e.g., 'commission' and 'buy back').
-   **Zero-Installation, 100% Local**: A single HTML file that runs in any modern browser. No web server, no installation, and no data is ever uploaded to the internet.
-   **User-Friendly Interface**:
    -   Simple drag-and-drop or file-picker for data sources.
    -   Smart column mapping with auto-detection for common header names.
    -   User settings (column mappings) are saved in the browser's `localStorage` for convenience.
-   **Detailed & Actionable Reports**: Generates a categorized summary of all discrepancies, making it easy to pinpoint specific issues.
-   **Excel Export**: Export the complete, detailed analysis report to a single `.xlsx` file, with each discrepancy type on its own sheet.

---

## 🚀 How It Works: The Reconciliation Logic

The tool performs a series of precise, independent comparisons to ensure data integrity and accuracy across all sources.

1.  **Source 1 (Your Report)**: The primary list of all transactions. Assumed to be the "source of truth" for what *should have* happened. Contains the App ID and the final amount paid (including GST).
2.  **Source 2 (Paid Source)**: A file confirming which transactions have been paid. Contains the App ID and the amount paid.
3.  **Source 3 (Commission Source)**: The commission statement. This file has unique parsing requirements:
    -   The **App ID** is an 8-digit number hidden within a "Remarks" column.
    -   The **Commission Amount** is summed up from rows where the "Remarks" contain keywords like `commission` or `buy back`.

The analysis is then performed in two main stages:

-   **S1 vs. S2 (Payment Reconciliation)**: Checks if every transaction in S1 exists in S2 and vice-versa. It also validates that the final amounts match perfectly.
-   **S1 vs. S3 (Commission Reconciliation)**: For every transaction in S1, it calculates the expected commission and compares it to the actual commission received in S3.

---

## 🛠️ How to Use

1.  **Download**: Download the `index.html` file from this repository.
2.  **Open**: Open the `index.html` file directly in your web browser (e.g., Chrome, Firefox, Edge).
3.  **Upload Files**:
    -   Upload your main report into the **Source 1** box.
    -   Upload your payment confirmation file into the **Source 2** box.
    -   Upload your commission statement into the **Source 3** box.
4.  **Map Columns**: The tool will try to auto-select the correct columns. Verify that all required columns (`*`) are correctly mapped for each source. Pay special attention to:
    -   **S3 Remarks**: The column containing the App ID and keywords.
    -   **S3 Amount**: The column with the commission value (e.g., `Amount` or `Credit`, not `Amount(Debit)`).
5.  **Set Options**: Adjust the "Desired Commission Percentage" if needed.
6.  **Analyze**: Click the **"Analyze Data"** button. The tool will process the files locally.
7.  **Review & Export**:
    -   Review the categorized results directly on the page. Each box shows a summary of a specific issue type.
    -   Click the **"Export Full Report to Excel"** button to download a detailed `.xlsx` file for further investigation or record-keeping.

---

## 📋 The Analysis Windows

The tool will generate a report highlighting the following potential issues, grouped by category:

#### Data Integrity Issues
-   **Duplicates in S1/S2/S3**: Finds records with the same App ID within the same file.
-   **S3 Rows with Invalid App ID**: Lists rows in the commission file where an 8-digit App ID could not be found in the Remarks column.
-   **S3 Commission Records Missing in S1/S2**: Highlights commission payments in S3 for App IDs that don't exist in your primary report.

#### S1 vs S2: Payment Reconciliation
-   **S1 Record Missing in S2**: Transactions from your report that are not marked as paid in Source 2.
-   **S2 Record Missing in S1**: Payments received for transactions that are not in your primary report.
-   **Amount Mismatch (S1 vs S2)**: Records where the final amount differs between your report and the paid source.

#### S1 vs S3: Commission Reconciliation
-   **S1 Records with Missing Commission in S3**: Transactions that exist in your report but have no corresponding commission entry in Source 3.
-   **Commission Exceeds Desired Rate**: Flags cases where the commission paid is higher than the expected percentage.
-   **Commission Below Desired Rate**: Flags cases where the commission paid is lower than expected.

---

## ⚙️ Technical Details

-   **Frontend**: Vanilla JavaScript (ES6+), HTML5, CSS3. No frameworks.
-   **Dependencies**:
    -   [SheetJS/xlsx](https://github.com/SheetJS/sheetjs) for parsing `.xlsx`, `.xls`, and `.csv` files.
    -   [Lodash](https://lodash.com/) for utility functions and deep object comparison.
-   **Data Storage**: Utilizes the browser's `localStorage` to remember column mappings between sessions, improving usability.

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

This tool is provided as-is for internal analysis purposes. While it is designed to be accurate, it is not a substitute for professional accounting software or a formal audit. Always double-check critical results against official records.
