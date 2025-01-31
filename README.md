# csv_optimizer (v0.20)

`csv_optimizer` is a Python utility for loading CSV files into Pandas while optimizing memory usage. It assigns appropriate data types based on a dataset sample, reducing unnecessary memory consumption, which can be enormous for large datasets. Instead of loading the full dataset at once, it processes the file in chunks (default: 1000 rows per chunk) and determines the most appropriate `dtype` from a sampled fraction (default: 10% of the complete dataset).

## Features
- Uses chunking to efficiently process large datasets.
- Detects and assigns `int`, `float`, `category`, `datetime`, and `boolean` data types.
- Handles missing values in integer and boolean columns with configurable options.
- Reduces memory usage compared to Pandas' default `read_csv()` behavior.
- Supports different encodings.

## Installation
Install the package locally:
```
pip install -e .
```
via PyPI:
```
pip install csv_optimizer
```

## Usage

*Basic Example*

```
from csv_optimizer import load_optimized_dataframe

df = load_optimized_dataframe("data.csv")
print(df.info())
```

## Additional Options

```
df = load_optimized_dataframe(
    "data.csv",
    sample_fraction=0.1,          # Sample size for type detection (default: 10%)
    chunksize=1000,               # Number of rows per chunk when reading the CSV (default: 1000)
    use_float_for_nan_ints=False, # Store NaN-containing integer columns as Int64 or float32
    use_float_for_nan_bools=False,# Store NaN-containing Boolean columns as boolean or float32
    encoding="utf-8"              # File encoding (default: 'latin1')
)
```

Note on `use_float_for_nan_ints` and `use_float_for_nan_bools`: If a column contains NaNs, NumPy's default behavior is to store the column as float32. This will use less memory than Pandas' nullable Int64 or Boolean. However, users can choose what is more important in their workflow: less memory usage but 'incorrect' `dtype` or correct `dtype` but additional memory overhead.

## CSV processing

1. Reads the CSV file in chunks (default: 1000 rows per chunk) to improve efficiency when handling large files.
2. Loads a sample (default: 10%) of the dataset to determine optimal data types.
3. Detects column types and assigns the most efficient dtype:
   - Converts categorical-like columns to `category`
   - Optimizes integer columns (`int8`, `int16`, `int32`, `int64`)
   - Uses `float32` where possible for floating-point numbers
   - Supports `datetime` parsing (Still relies on trial and error, leading to some warning messages.)
   - Detects Boolean columns (`bool` or Pandas nullable `boolean`)
   - Allows user-defined handling for NaN-containing columns
4. Applies optimized `dtypes` when loading the full dataset.

## Development & Contributions

Feel free to contribute!

```
git clone https://github.com/timmueller0/csv_optimizer.git
cd csv_optimizer
pip install -e .
```
