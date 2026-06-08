Key-Value databases are special types of data storage that work in a key-value way, like **hashmaps** (use hash and hash-trees as an underlying structure).
Because of this all operations are based on the passed key, and they usually include as a base:
- **get**(key);
- **put**(key, value).
So, as it is obvious, these types of data storage are **particularly optimized to retrieve or append a single value by passing the key**, not multiple values (even if it is in fact possible).


As most of the other types of NoSQL there's no real fixated schema for the data, elements could be missing stuff.

**Document stores** are an **extension** of this, where values are essentially documents.