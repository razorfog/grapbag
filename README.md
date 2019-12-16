# jsonpaths 

This simple commandline tool reads a JSON object from a file or from stdin and creates a corresponding json object with the keys in a flat-hierarchy. The object hieararchy is denoted with a dot-notation.

For example, the input of:

```json
{
    "a": 1,
    "b": true,
    "c": {
        "d": 3,
        "e": "test"
    }
}
```

Results in the output of:

```json
{
    "a": 1,
    "b": true,
    "c.d": 3,
    "c.e": "test"
}
```

**jsonpaths** does not support array objects - only standard json values and objects.
