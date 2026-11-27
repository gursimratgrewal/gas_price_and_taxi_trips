# NYC Yellow Taxi Activity and Weekly Gasoline Prices

## Team Members

| Team member | Responsibilities |
| --- | --- |
| Esteban Castro | Define research questions and identify Spark operations for the analysis. |
| Brian Rice | Coordinate the project, organize meetings, and compare analysis approaches and cloud costs. |
| Seth Prisament | Obtain and inspect taxi data, upload it to Google Cloud Storage, and collaborate on preprocessing and joins. |
| Gursimrat Grewal | Obtain and inspect gasoline price data, upload it to Google Cloud Storage, and collaborate on preprocessing and joins. |

## Problem Statement

This project will examine how weekly gasoline prices are associated with yellow taxi activity and trip costs in New York City. We plan to use Apache Spark to combine NYC Yellow Taxi Trip Records with weekly NYC gasoline prices and analyze changes in trip volume, distance, duration, fares, and cost per mile.

Our research questions are:

1. How are weekly gasoline prices associated with the number of taxi trips?
2. Are higher gasoline prices associated with shorter average trip distances?
3. Are higher gasoline prices associated with higher trip costs or costs per mile?
4. Does the relationship between trip volume and gasoline prices vary by pickup location?
5. How do estimated fuel costs vary across gasoline price periods?

The analysis will describe associations. Establishing that gasoline prices cause changes in taxi activity would require additional evidence and controls. Fuel costs will be estimates based on an explicitly stated fuel-efficiency assumption, because the supplied trip schema does not contain fuel consumption.

## Project Status and Scope

The notebooks have not been created yet. The repository structure and execution sequence below describe the proposed implementation; preprocessing, joins, and analysis are planned work.

**October 2025 through October 2026**, inclusive. As checked on November 20, 2026, the [TLC download page](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) lists 2026 files through September. October 2026 remains part of the intended scope, subject to publication. The final downloaded month range and corresponding record counts will be documented when ingestion is complete.

## Data Sources with URLs

| Dataset | Provider | URL |
| --- | --- | --- |
| Yellow Taxi Trip Records | New York City Taxi and Limousine Commission (TLC) | [TLC Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) |
| Gasoline Retail Prices Weekly Average by Region: Beginning January 2017 | New York State Energy Research and Development Authority (NYSERDA), via NY Open Data | [Weekly Regional Gasoline Prices](https://data.ny.gov/Energy-Environment/Gasoline-Retail-Prices-Weekly-Average-by-Region-Be/nqur-w4p7) |

For taxi data, expand the relevant year under **Trip Record Data Download Links** and select **Yellow Taxi Trip Records** for each required month. For gasoline prices, export the NY Open Data dataset and retain the dates covering the taxi analysis period, including the Monday that starts the first included week.

## Data Description

### Yellow Taxi Trip Records

Each row represents an individual taxi trip. The dataset includes pickup and drop-off timestamps, pickup and drop-off taxi zone IDs, trip distance, passenger count, payment type, fare, tips, taxes, tolls, and surcharges. Monthly downloads are provided in Parquet format. See the [TLC source page](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

The HW1 contract reports the following for the **March 2026 sample**:

- **Records:** 3,952,451
- **Reported file size:** approximately 80 MB

These figures come from HW1 and have not been independently remeasured. They describe the sample, rather than the entire planned monthly collection. The reported file size should not be treated as a verified uncompressed size.

### Weekly Gasoline Prices

Each row contains a weekly date and average gasoline prices for regions across New York State. This project will use **`Date`** and **`New York City Average ($/gal)`**. The source identifies dates by the Monday of each week and reports prices in dollars per gallon. See the [NY Open Data dataset](https://data.ny.gov/Energy-Environment/Gasoline-Retail-Prices-Weekly-Average-by-Region-Be/nqur-w4p7).

The HW1 contract reports:

- **Records in the downloaded snapshot:** 510
- **Reported file size:** approximately 68 KB

The analysis will filter the gasoline data to the weeks needed for the selected taxi months.

## Schema

### Taxi Input Schema

Actual input schemas will be inspected and reconciled across monthly files during ingestion.

| Column | Reported data type |
| --- | --- |
| `VendorID` | integer |
| `tpep_pickup_datetime` | timestamp_ntz |
| `tpep_dropoff_datetime` | timestamp_ntz |
| `passenger_count` | long |
| `trip_distance` | double |
| `RatecodeID` | long |
| `store_and_fwd_flag` | string |
| `PULocationID` | integer |
| `DOLocationID` | integer |
| `payment_type` | long |
| `fare_amount` | double |
| `extra` | double |
| `mta_tax` | double |
| `tip_amount` | double |
| `tolls_amount` | double |
| `improvement_surcharge` | double |
| `total_amount` | double |
| `congestion_surcharge` | double |
| `Airport_fee` | double |
| `cbd_congestion_fee` | double |

### Gasoline Input Schema

The types below reflect the pandas representation reported in HW1. During Spark ingestion, `Date` will be parsed and cast to a date for joining, and price columns will be cast to numeric types.

| Column Name                    | Data Type         |
|--------------------------------|-------------------|
| Date                           | Date (MM/DD/YYYY) |
| New York State Average ($/gal) | double  |
| Albany Average ($/gal)         | double  |
| Batavia Average ($/gal)        | double  |
| Binghamton Average ($/gal)     | double  |
| Buffalo Average ($/gal)        | double  |
| Dutchess Average ($/gal)       | double  |
| Elmira Average ($/gal)         | double  |
| Glens Falls Average ($/gal)    | double  |
| Ithaca Average ($/gal)         | double  |
| Kingston Average ($/gal)       | double  |
| Nassau Average ($/gal)         | double  |
| New York City Average ($/gal)  | double  |
| Rochester Average ($/gal)      | double  |
| Syracuse Average ($/gal)       | double  |
| Utica Average ($/gal)          | double  |
| Watertown Average ($/gal)      | double  |
| White Plains Average ($/gal)   | double  |

## Repository Structure

```
gas_price_and_taxi_trips/
├── README.md
└── import_gas_taxi.ipynb

1. `01_import_gas_taxi.ipynb`:  Loads the NYC Yellow Taxi trip records and the weekly NY gasoline price CSV from Google Cloud Storage into Spark and inspects their schemas. |

```
