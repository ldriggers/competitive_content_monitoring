# Competitor Sitemap Monitor

This project is a lightweight business intelligence system designed to track changes to competitors' websites by monitoring their XML sitemaps. It automatically fetches sitemap data, identifies new, updated, and removed URLs, and maintains a historical record of these changes.

## Overview

The system focuses on:
- Monitoring XML sitemaps from configured competitor domains.
- Detecting new, updated (based on `lastmod` dates), and removed URLs.
- Storing URL metadata and change history in Parquet files.
- Running daily via GitHub Actions for automated, cost-effective monitoring.

This initial version does **not** store the actual content of the web pages, only sitemap metadata.

## Project Structure

```
/
├── .github/
│   └── workflows/
│       └── daily_monitor.yml  # GitHub Actions workflow
├── competitive_content_monitoring/
│   ├── src/                     # Source code
│   │   ├── __init__.py
│   │   ├── config.py            # Handles loading and validating config.json
│   │   ├── sitemap_fetcher.py   # (To be created) Fetches sitemaps
│   │   ├── sitemap_parser.py    # (To be created) Parses sitemap XML
│   │   ├── data_processor.py    # (To be created) Processes changes and updates data
│   │   └── main.py              # (To be created) Main script orchestrating the tasks
│   ├── data/                    # Stores Parquet data files (created by the script)
│   │   └── .gitkeep
│   ├── requirements.txt         # Python dependencies
│   ├── config.json              # Configuration for domains to monitor
│   ├── PLAN.md                  # Project plan document
│   └── prd.md                   # Product requirements document
└── README.md                  # This file
```
(Note: The main project directory `competitive_content_monitoring` might be the root in your local setup if you cloned it directly, or a subdirectory if it's part of a larger workspace.)

## Setup

1.  **Clone the repository.**
2.  **Configure Domains**: Edit `competitive_content_monitoring/config.json` to specify the domains you want to monitor. Update the `user_agent` field to something relevant to your project (including a contact URL or email is good practice).
    ```json
    {
      "domains": [
        {
          "name": "Example Competitor 1",
          "domain": "example.com",
          "sitemap_url": "https://www.example.com/sitemap.xml"
        },
        {
          "name": "Example Competitor 2",
          "domain": "another-example.net",
          "sitemap_url": "https://www.another-example.net/sitemap_index.xml"
        }
      ],
      "user_agent": "YourProjectSitemapMonitor/1.0 (+http://your-contact-page-or-email)"
    }
    ```
3.  **Install Dependencies**: 
    ```bash
    pip install -r competitive_content_monitoring/requirements.txt
    ```

## Usage

-   **Manual Run (for testing)**: Navigate to the `competitive_content_monitoring` directory and run the main script (once `src/main.py` is implemented):
    ```bash
    python src/main.py
    ```
-   **Automated Runs**: The system is configured to run daily via GitHub Actions. Changes to the `data/` directory (containing Parquet files) will be committed back to the repository by the action.

## Data Output

The script will generate/update Parquet files in the `competitive_content_monitoring/data/` directory:
-   `sitemap_urls.parquet`: Contains all unique URLs found across sitemaps, with their metadata and first/last seen timestamps.
-   `url_changes.parquet`: Logs every detected change (new, updated, removed) with relevant details and timestamps.

These files can be analyzed using Python with libraries like Pandas.

## Contributing

(Details to be added if the project becomes open to contributions)

## License

(To be determined - e.g., MIT, Apache 2.0, or private) 