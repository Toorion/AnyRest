# AnyRest 2.0

(AnyRest 1.0 was introduced here: https://sourceforge.net/p/anyrest/code/ci/master/tree/)

### The best alternative to build query-based REST application

AnyRest simple fast and effective way to provide REST features to your application. 
Like GraphQL AnyRest have specific query language, but instead of GraphQL used json based queryies and doesn`t 
require specilal query parser on client-side or server-side.


## Basic AnyRest query structure

```
[ // async query blocks
    [ // sync query blocks
        {query1},
        {query2},
        {query3},
        ...
    ],
    [
        ...
    ]
]
```

As described below, queries can be executed asynchronously as well as sequentially.
First level queries parts (Batch) are executed asynchronously, but within a Batch each query is executed after the previous one.


## Queries

```
{
    "action":"add",
    "entity":"example",
    ...
}
```

Each Query should include "action" (which identify the kind of operation to be execute)
And "entity" is the entity to perform the operation on.


### Add data - action "add"

```
{
    "action":"add",
    "entity":"example",
    "fields": ["Field1", "Field2", "Field3", ["Field4: ["Field4_1", "Field4_2"]]],
    "values": [1, "Value 1", 1.1, ["DateForField4_1", "DataForField4_2]]
}

```

Adding the data also required list of fields to add ("fields") and "values" to add
"fields" and "values" can be nested if your entity support it [^1]


### Update data - action "upd"

```
{
    "action":"upd",
    "entity":"example",
    "fields": ["Field1", "Field2", "Field3"],
    "values": [2, "Value 2", 2.1],
    "filter": {"Field1": 1}
}

```

Update same like add, but required "filter" field for identify which records should be updated


### Resolve data - action "get"

```
{
    "action":"get",
    "entity":"example",
    "fields": ["Field1", "Field2", "Field3", ["Field4: ["Field4_1", "Field4_2"]]],
    "filter": {"Field1": 1},
    "limit": 20,
    "offset": 20
}

```

Resolving fields required only list of returned "fields" and "filter"
Fields in list can be nested, if "entity" support it [^1]
For pagination request "limit" and "offset" is allowed

### Delete records - action "del"

```
{
    "action":"del",
    "entity":"example",
    "filter": {"Field1": 1}
}

```

Delete records required only "filter" to identify which records should be deleted

[^1]: Check language-specific manual of AnyRest