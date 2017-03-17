# restcli
A dumb generic rest client written in python, honestly I think all this can be done with httpie already..

# usage

```
Usage: restcli [options] [URL]

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -d, --debug           debug mode
  -v VERB, --verb=VERB  verb to use: get, post, delete. default=get
  -j JSONFILE, --json=JSONFILE  json document to post
```

## get request

```
$ restcli 'http://localhost:9200/_count?pretty'
status code: 200
{
    "_shards": {
        "failed": 0,
        "successful": 0,
        "total": 0
    },
    "count": 0
}
```

## get request with payload

```
$ cat text.json
{
    "query": {
        "match_all": {}
    }
}
```

```
$ restcli -j ./text.json 'http://localhost:9200/_count?pretty'
status code: 200
{
    "_shards": {
        "failed": 0,
        "successful": 0,
        "total": 0
    },
    "count": 0
}
```

## post request with payload

```
$ cat 1.json
{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
```

```
$ restcli -j 1.json -v post 'http://localhost:9200/megacorp/employee/1'
status code: 201
{
    "_id": "1",
    "_index": "megacorp",
    "_shards": {
        "failed": 0,
        "successful": 1,
        "total": 2
    },
    "_type": "employee",
    "_version": 1,
    "created": true,
    "result": "created"
}
```

## delete request

```
$ restcli -v delete 'http://localhost:9200/megacorp/employee/1'
status code: 200
{
    "_id": "1",
    "_index": "megacorp",
    "_shards": {
        "failed": 0,
        "successful": 1,
        "total": 2
    },
    "_type": "employee",
    "_version": 2,
    "found": true,
    "result": "deleted"
}
```

```
$ restcli 'http://localhost:9200/megacorp/employee/1'
status code: 404
{
    "_id": "1",
    "_index": "megacorp",
    "_type": "employee",
    "found": false
}
```
