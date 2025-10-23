# AW Data Engineering Dataset Repository

This repository packages a curated subset of the AdventureWorks sample dataset for experimentation with data engineering, analytics, and reporting workflows.  The CSV extracts can be used to practice building ETL pipelines, dimensional models, dashboards, or data quality checks without needing access to a SQL Server backup of the original AdventureWorks database.

## Repository structure

```text
AW-DATA-ENGINEERING/
├── Data/
│   ├── AdventureWorks_Calendar.csv
│   ├── AdventureWorks_Customers.csv
│   ├── AdventureWorks_Product_Categories.csv
│   ├── AdventureWorks_Product_Subcategories.csv
│   ├── AdventureWorks_Products.csv
│   ├── AdventureWorks_Returns.csv
│   ├── AdventureWorks_Sales_2015.csv
│   ├── AdventureWorks_Sales_2016.csv
│   ├── AdventureWorks_Sales_2017.csv
│   └── AdventureWorks_Territories.csv
├── Reference Scripts/
│   └── git.json
├── testing          # simple text placeholder
└── testing123       # simple text placeholder
```

*The `testing` and `testing123` files are plain-text placeholders retained from the original project. They do not participate in the data model.*

## Dataset overview

| File | Grain | Key fields | Description |
| --- | --- | --- | --- |
| `AdventureWorks_Calendar.csv` | Day | `Date` | Calendar table containing individual calendar dates for time-based analyses. |
| `AdventureWorks_Customers.csv` | Customer | `CustomerKey` | Customer dimension with demographic attributes such as name, contact information, income band, education, occupation, and homeowner flag. |
| `AdventureWorks_Product_Categories.csv` | Product category | `ProductCategoryKey` | High-level product categories (e.g., Bikes, Clothing) referenced by subcategories. |
| `AdventureWorks_Product_Subcategories.csv` | Product subcategory | `ProductSubcategoryKey`, `ProductCategoryKey` | Mid-level product groupings that bridge products to categories. |
| `AdventureWorks_Products.csv` | Product | `ProductKey`, `ProductSubcategoryKey` | Detailed product dimension including SKU, name, model, description, color, size, cost, and list price. |
| `AdventureWorks_Sales_2015.csv`<br>`AdventureWorks_Sales_2016.csv`<br>`AdventureWorks_Sales_2017.csv` | Sales order line | `OrderNumber`, `ProductKey`, `CustomerKey`, `TerritoryKey` | Yearly fact tables of sales order lines, with order dates, stock dates, line numbers, and quantities sold. Combine the three files to analyze trends across multiple years. |
| `AdventureWorks_Returns.csv` | Return transaction | `ReturnDate`, `ProductKey`, `TerritoryKey` | Records of returned items, useful for blending with sales data to study net revenue or return rates. |
| `AdventureWorks_Territories.csv` | Sales territory | `SalesTerritoryKey` | Geography dimension containing region, country, and group assignments for each sales territory. |

The data originates from the public AdventureWorks sample database distributed by Microsoft and has been exported to CSV for convenience.

## Reference scripts

The `Reference Scripts/git.json` file enumerates the relative GitHub paths for each dataset. It can be used to automate downloads or validate that all source files are present when orchestrating data ingestion jobs.

## Getting started

1. **Clone the repository** (or download it as a ZIP archive) to access the CSV files locally.
2. **Load the data** into your tooling of choice. Example using Python and Pandas:

   ```python
   import pandas as pd
   from pathlib import Path

   data_dir = Path("Data")

   sales_2017 = pd.read_csv(data_dir / "AdventureWorks_Sales_2017.csv")
   products = pd.read_csv(data_dir / "AdventureWorks_Products.csv")
   customers = pd.read_csv(data_dir / "AdventureWorks_Customers.csv")

   # Join sales to product and customer details
   sales_with_dims = (sales_2017
                      .merge(products, on="ProductKey", how="left")
                      .merge(customers, on="CustomerKey", how="left"))

   print(sales_with_dims.head())
   ```

3. **Explore and model** the data. Suggested exercises include:
   - Building a star schema in your preferred database engine.
   - Practicing data quality tests (e.g., ensuring product keys are present in the product dimension).
   - Creating dashboards that track revenue, order quantities, and return rates across regions and time.

## Data quality notes

* CSV files retain the formatting from their original export. For example, currency fields include a dollar sign and trailing space (e.g., `"$90,000 "`), and some text columns may span multiple lines when viewed raw. Clean or normalize these fields as needed during ingestion.
* Dates are stored as U.S. formatted strings (MM/DD/YYYY). Convert them to your preferred date type when loading into analytical tools.

## Contributing

Contributions that enhance documentation, add data dictionaries, or supply helper scripts are welcome. Please open an issue describing the proposed change before submitting a pull request. When contributing new data, document the source and processing steps to maintain provenance.

## License

The AdventureWorks dataset is provided under Microsoft's sample data license. Treat the included CSV extracts accordingly and avoid using them for production workloads that require proprietary data.
