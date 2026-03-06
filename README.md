# 🎮 Steam Deals & Quality Analysis (RAWG API)

## Project Overview

This project combines **dynamic web scraping** and **API data enrichment** to analyze the relationship between game discounts on Steam and their critical reception (Metacritic scores). We extracted data from 500 games across different sorting methods to identify "hidden gems"high-quality games with significant discounts.

## Technical Stack

-   **Web Scraping:** `selenium` for handling infinite scroll and dynamic content on Steam.
-   **API Integration:** `httr2` and `jsonlite` to communicate with the **RAWG API**.
-   **Data Wrangling:** `tidyverse` and `stringr` using **Regular Expressions (Regex)** for advanced title normalization.
-   **Validation:** Custom functions to audit price consistency and data integrity.

------------------------------------------------------------------------

## How to Reproduce

Follow these steps to execute the full pipeline:

### 1. Environment Setup

Ensure you have **R** and **RStudio** installed. Install the necessary libraries by running this in your console:

```{r}
install.packages(c("selenium", "httr2", "xml2", "tidyverse", "lubridate",
                   "DataExplorer", "ggplot2", "jsonlite"))
```

### 2. API Key Configuration

For security and reproducibility, the API key must be stored in a local file that the script is programmed to read:

1.  Create a plain text file named **`api_key.txt`** in the root directory of the project.
2.  Paste the **RAWG API Key** (provided in the submission notes) inside this file and save it.
3.  Our code is configured to read this file automatically using `readLines()`.
4.  *Note: We have included `api_key.txt` in the `.gitignore` file to following security standards, ensuring sensitive credentials are never leaked to version control.*

### 3. Execution Order

This project has been consolidated into a single, automated pipeline for ease of use:

1.  **`main_analysis.qmd`**: Open this file and click **"Render"** (or run all chunks). 
    * It will automatically launch the Selenium scraper.
    * It will clean the raw data and perform the RAWG API enrichment.
    * It will generate the final statistical plots and the consolidated dataset.

---

### Project Structure & Output

| File | Description |
| :--- | :--- |
| **`main_analysis.qmd`** | The complete end-to-end script (Scraper + API + Analysis). |
| **`api_key.txt`** | (User-created) File containing the RAWG credentials. |
| **`final_data_steam_rawg.csv`** | The final enriched dataset generated after execution. |

---

### Key Technical Solutions

* **Dynamic Scraping**: We implemented a `while` loop with JavaScript execution (`window.scrollTo`) to bypass Steam's infinite scroll and successfully reach the 500-game target.
* **Regex Normalization**: Steam titles are "noisy" (e.g., "Game Name™ (Special Edition)"). We built a **normalization pipeline** using `gsub()` and `trimws()` to strip symbols and subtitles, increasing our API match success rate significantly.
* **Data Validation**: We created a `validate_price_math()` function to cross-reference `original_price`, `discount_pct`, and `current_price`, ensuring the scraped data is mathematically sound before proceeding with the analysis.
