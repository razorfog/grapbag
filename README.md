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

## Install jsonpaths

* *jsonpaths* is written in *node* (javascript). It was developed with version 13.2.0 and was also tested with version 8.9.1.
If you don't have node already [follow this link to install it](https://nodejs.org/en/download/) (getting at least version 8.9.1).

* Clone this repo: `$ git clone https://github.com/razorfog/lootbag.git`

* Install the dependencies. `$ npm install`

## Running jsonpaths.

By default *jsonpaths* reads from stdin and writes to stdout. It expects a valid JSON definition as input. *jsonpaths* comes with some sample json files for testing.

Example usages:
* Reads json from stdin writes to stdout.
```bash
$ ./jsonpaths < test1.json
{
  "a": 1,
  "b": true,
  "c.d": 3,
  "c.e": "test"
}
```
* Name json file on commandline with *--json*
```bash
$ ./jsonpaths --json test1.json
```
* Direct output to named file with *--out*
```bash
$ ./jsonpaths --json test1.json --out testresult.json
```

## Handling empty objects.

By default *jsonpaths* ignores fields that have an empty object as a value. Included in the repo is `emptyobj.json`.
```json
{
 "fruit": {
   "apple": {},
    "banana": {},
    "peach": "ripe" 
 },
 "pets": {
   "dog": {
     "little": false,
     "big": true,
     "hats": {}
   },
   "cat": {
     "calico": {},
     "tabby": {
       "orange": {},
       "tiger": {}
     }
    }
  }
}
```
*jsonpaths* ignores the fields with the empty object values. Example output:
```bash
$ cat emptyobj.json | ./jsonpaths
{
  "fruit.peach": "ripe",
  "pets.dog.little": false,
  "pets.dog.big": true
}
```
Note that *fruit.apple*, *fruit.banana* and others are ommitted. To include fields that have empty object values use the `-e` or `--empty` flag. With `-e` these fields are included with a value of *"{}"*:
```bash
$ ./jsonpaths -j emptyobj.json -e
{
  "fruit.apple": "\"{}\"",
  "fruit.banana": "\"{}\"",
  "fruit.peach": "ripe",
  "pets.dog.little": false,
  "pets.dog.big": true,
  "pets.dog.hats": "\"{}\"",
  "pets.cat.calico": "\"{}\"",
  "pets.cat.tabby.orange": "\"{}\"",
  "pets.cat.tabby.tiger": "\"{}\""
}
```

## Running with npm
You can also run several version using *npm*.
* runs *jsonpaths* with no args (ready to read from stdin)
  `$ npm start`
* runs *jsonpaths* with `test1.json`, results to stdout:
  `$ npm run example1`
* runs *jsonpaths* with `emptyobj.json`, omits fields with empty objects:
  `$ npm run emptyobj`
* runs *jsonpaths* with `emptyobj.json`, includes fields with empty objects
  `$ npm run includeempty`
* runs the test program - compares output of `test1.json` with expected result `test1paths.json`
  `$ npm test`

## Command line usage
```bash
$ ./jsonpaths -h
Usage: jsonpaths [options]

Takes a JSON object as input and outputs the path to every terminal value.
By default JSON is read from stdin. Use -j to read the JSON from named file.
Output is written to stdout, or use -o to name the output file.

Options:
  -j, --json <jsonfile>   Read JSON object from ths file (def: stdin)
  -o, --out <resultfile>  Write paths to "resultfile" (def: stdout)
  -e, --empty             Include fields with empty object values as "{}" (def: ignore field)
  -h, --help              output usage information
```



