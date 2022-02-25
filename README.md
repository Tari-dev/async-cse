[![CircleCI](https://circleci.com/gh/crrapi/async-cse.svg?style=svg)](https://circleci.com/gh/crrapi/async-cse)
[![Build Status](https://travis-ci.org/crrapi/async-cse.png?branch=master)](https://travis-ci.org/crrapi/async-cse)
[![Codestyle](https://img.shields.io/badge/code%20style-black-000000.svg)](https://img.shields.io/badge/code%20style-black-000000.svg)
[![PyPI version](https://badge.fury.io/py/async-cse.svg)](https://badge.fury.io/py/async-cse)
[![Issues](https://img.shields.io/github/issues/crrapi/async-cse.svg?colorB=00FFFF)](https://img.shields.io/github/issues/crrapi/async-cse.svg?colorB=00FFFF)
[![LICENSE](https://img.shields.io/pypi/l/async-cse.svg)](https://img.shields.io/pypi/l/async-cse.svg)
[![Downloads](https://img.shields.io/pypi/dd/async-cse.svg)](https://img.shields.io/pypi/dd/async-cse.svg)
[![Python](https://img.shields.io/pypi/pyversions/async-cse.svg)](https://img.shields.io/pypi/pyversions/async-cse.svg)
# async-cse
Asyncio API wrapper for the [Google Custom Search JSON API](https://developers.google.com/custom-search/v1/overview).
# Installation

#### Want stable releases?
`pip3 install -U async_cse`
#### Living on the edge? Want hotfixes?
`pip3 install -U git+https://github.com/crrapi/async-cse`

# Usage
```python
import async_cse

client = async_cse.Search("Your API Key") # create the Search client (uses Google by default!)
results = await client.search("Python", safesearch=False) # returns a list of async_cse.Result objects
first_result = results[0] # Grab the first result
print(first_result.title, first_result.description, first_result.url, first_result.image_url) # Title, snippet, URL, and Image URL (if specified)

await client.close() # Run this when cleaning up.
```
# Getting image results
To get image results with the default engine, use `image_search=True` when searching, like so:
```python
await client.search("Python", safesearch=False, image_search=True) # returns a list of async_cse.Result objects
```

# Custom search engines
To use Search objects with a custom search engine, provide the ID of the search engine.
```python
async_cse.Search("Your API Key", engine_id="015786823554162166929:mywctwj8es4")
```

# SafeSearch
SafeSearch can also be turned off by setting `safesearch=False` when using the `search()` method.

# Getting an API key
You can get an API key by going [here](https://developers.google.com/custom-search/v1/overview) and scrolling down to the **API key** section.
![API key](https://i.imgur.com/pHXFiI8.png "Getting an API key")

# (New in 0.3.0) API Key Shuffling
Key shuffling may be used as a fail-safe for keys that run out of requests, effectively giving you +100 requests for each key passed.\
Just pass a list of keys when instantiating your `Search` client.\
Here's a demonstration of this:
```python
import async_cse

client = async_cse.Search(["API Key 1", "API Key 2", "API Key 3"]) # Multiple keys as a list
await client.search("Python") # Uh oh, one of these keys doesn't work! Trying again with another...
```
When `async-cse` detects a non-working key, it will remove it from its internal list and retry the search.
