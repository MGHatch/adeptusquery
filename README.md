# Boolean text search using Eldar

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Installing

You can install the method by typing:
```
pip install eldar
```

### Basic usage

```python
from eldar import Query


# build list
documents = [
    "Gandalf is a fictional character in Tolkien's The Lord of the Rings",
    "Frodo is the main character in The Lord of the Rings",
    "Ian McKellen interpreted Gandalf in Peter Jackson's movies",
    "Elijah Wood was cast as Frodo Baggins in Jackson's adaptation",
    "The Lord of the Rings is an epic fantasy novel by J. R. R. Tolkien"]

eldar = Query('("gandalf" OR "frodo") AND NOT ("movie" OR "adaptation")')

# use `filter` to get a list of matches:
print(eldar.filter(documents))
# >>> ["Gandalf is a fictional character in Tolkien's The Lord of the Rings",
#     'Frodo is the main character in The Lord of the Rings']

# call to see if the text matches the query:
print(eldar(documents[0]))
# >>> True

# by default, words must match. Thus, "movie" != "movies":
print(eldar(documents[2]))
# >>> True
```


You can also use it to mask Pandas DataFrames:
```python
from eldar import Query
import pandas as pd


# build dataframe
df = pd.DataFrame([
    "Gandalf is a fictional character in Tolkien's The Lord of the Rings",
    "Frodo is the main character in The Lord of the Rings",
    "Ian McKellen interpreted Gandalf in Peter Jackson's movies",
    "Elijah Wood was cast as Frodo Baggins in Jackson's adaptation",
    "The Lord of the Rings is an epic fantasy novel by J. R. R. Tolkien"],
    columns=['content'])

# build query object
eldar = Query('("gandalf" OR "frodo") AND NOT ("movie" OR "adaptation")')

# eldar's call returns True if the text matches the query.
# You can filter a dataframe using pandas mask syntax:
df = df[df.content.apply(eldar)]
print(df)
```

### Parameters

There are three parameters that you can adjust in the query builder.
By default:
```python
Query(..., ignore_case=True, ignore_accent=True, match_word=True)
```
Let the query be ```query = '"movie"'```:

* If `ignore_case` is True, the documents "Movie" and "movie" will be matched. If False, only "movie" will be matched.
* If `ignore_accent` is True, the documents "mövie" will be matched.
* If `match_word` is True, the document will be tokenized and the query terms will have to match exactly. If set to False, the documents "movies" and "movie" will be matched. Setting this option to True may slow down the query.

### Wildcards

Queries also support `*` as wildcard character. Wildcard matches any number (including none) of alphanumeric characters.

```python
from eldar import Query


# sample document and query with multiple wildcards:
document = "Gandalf is a fictional character in Tolkien's The Lord of the Rings"
eldar = Query('"g*dal*"')

# call to see if the text matches the query:
print(eldar(document))
# >>> True
```

## License

This package is MIT licensed.
