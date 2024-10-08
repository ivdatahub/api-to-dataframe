# API to DataFrame
Python library that simplifies obtaining data from API endpoints by converting them directly into Pandas DataFrames. This library offers robust features, including retry strategies for failed requests.

[![Github](https://img.shields.io/badge/-Go_to_Github_repository-05122A?style=flat&logo=github)](https://github.com/ivdatahub/api-to-dataframe)

[![PyPI - Status](https://img.shields.io/pypi/status/api-to-dataframe?style=for-the-badge&logo=pypi)](https://pypi.org/project/api-to-dataframe/)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/api-to-dataframe?style=for-the-badge&logo=pypi)](https://pypi.org/project/api-to-dataframe/)
[![PyPI - Version](https://img.shields.io/pypi/v/api-to-dataframe?style=for-the-badge&logo=pypi)](https://pypi.org/project/api-to-dataframe/#history)

![PyPI - Python Version](https://img.shields.io/pypi/pyversions/api-to-dataframe?style=for-the-badge&logo=python)

[![CI](https://img.shields.io/github/actions/workflow/status/ivdatahub/api-to-dataframe/CI.yaml?&style=for-the-badge&logo=githubactions&cacheSeconds=60&label=Tests+and+pre+build)](https://github.com/ivdatahub/api-to-dataframe/actions/workflows/CI.yaml)
[![CD](https://img.shields.io/github/actions/workflow/status/ivdatahub/api-to-dataframe/CD.yaml?&style=for-the-badge&logo=githubactions&cacheSeconds=60&event=release&label=Package_publication)](https://github.com/ivdatahub/api-to-dataframe/actions/workflows/CD.yaml)

[![Codecov](https://img.shields.io/codecov/c/github/ivdatahub/api-to-dataframe?style=for-the-badge&logo=codecov)](https://app.codecov.io/gh/ivdatahub/api-to-dataframe)

## Project Stack

![Python](https://img.shields.io/badge/-Python-05122A?style=flat&logo=python)&nbsp;
![Docker](https://img.shields.io/badge/-Docker-05122A?style=flat&logo=docker)&nbsp;
![Poetry](https://img.shields.io/badge/-Poetry-05122A?style=flat&logo=poetry)&nbsp;
![GitHub Actions](https://img.shields.io/badge/-GitHub_Actions-05122A?style=flat&logo=githubactions)&nbsp;
![CodeCov](https://img.shields.io/badge/-CodeCov-05122A?style=flat&logo=codecov)&nbsp;
![pypi](https://img.shields.io/badge/-pypi-05122A?style=flat&logo=pypi)&nbsp;
![pandas](https://img.shields.io/badge/-pandas-05122A?style=flat&logo=pandas)&nbsp;
![pytest](https://img.shields.io/badge/-pytest-05122A?style=flat&logo=pytest)&nbsp;


## Installation

To install the package using pip, use the following command:

```sh
pip install api-to-dataframe
```

To install the package using poetry, use the following command:

```sh
poetry add api-to-dataframe
```

## User Guide

``` python
## Importing library
from api_to_dataframe import ClientBuilder, RetryStrategies

# Create a client for simple ingest data from API (timeout 1 second)
client = ClientBuilder(endpoint="https://api.example.com")

# if you can define timeout with LINEAR_RETRY_STRATEGY and set headers:
headers = {
    "application_name": "api_to_dataframe"
}
client = ClientBuilder(endpoint="https://api.example.com"
                        ,retry_strategy=RetryStrategies.LINEAR_RETRY_STRATEGY
                        ,connection_timeout=2
                        ,headers=headers)

# if you can define timeout with EXPONENTIAL_RETRY_STRATEGY and set headers:
client = ClientBuilder(endpoint="https://api.example.com"
                        ,retry_strategy=RetryStrategies.EXPONENTIAL_RETRY_STRATEGY
                        ,connection_timeout=10
                        ,headers=headers
                        ,retries=5
                        ,initial_delay=10)


# Get data from the API
data = client.get_api_data()

# Convert the data to a DataFrame
df = client.api_to_dataframe(data)

# Display the DataFrame
print(df)
```

## Important notes:
* **Opcionals Parameters:** The params timeout, retry_strategy and headers are opcionals.
* **Default Params Value:** By default the quantity of retries is 3 and the time between retries is 1 second, but you can define manually.
* **Max Of Retries:** For security of API Server there is a limit for quantity of retries, actually this value is 5, this value is defined in lib constant. You can inform any value in RETRIES param, but the lib only will try 5x.
* **Exponential Retry Strategy:** The increment of time between retries is time passed in **initial_delay** param * 2 * the retry_number, e.g with initial_delay=2

    RetryNumber  | WaitingTime
    ------------ | -----------
    2            |  2s
    2            |  4s
    3            |  6s
    4            |  8s
    5            |  10s
* **Linear Retry Strategy:** The increment of time between retries is time passed in **initial_delay**
e.g with initial_delay=2

    RetryNumber  | WaitingTime
    ------------ | -----------
    1            |  2s
    2            |  2s
    3            |  2s
    4            |  2s
    5            |  2s
