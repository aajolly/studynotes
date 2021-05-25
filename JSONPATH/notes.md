## JSONPATH

### Introduction
JsonPath is a query language for reading JSON data. It has use cases in Kubernetes where you may want to extract information using it in conjunction with Kubectl.


Examples demonstrating JsonPath

```json
{
    "store": {
        "book": [
            {
                "category": "reference",
                "author": "Nigel Rees",
                "title": "Sayings of the Century",
                "price": 8.95
            },
            {
                "category": "fiction",
                "author": "Evelyn Waugh",
                "title": "Sword of Honour",
                "price": 12.99
            },
            {
                "category": "fiction",
                "author": "Herman Melville",
                "title": "Moby Dick",
                "isbn": "0-553-21311-3",
                "price": 8.99
            },
            {
                "category": "fiction",
                "author": "J. R. R. Tolkien",
                "title": "The Lord of the Rings",
                "isbn": "0-395-19395-8",
                "price": 22.99
            }
        ],
        "bicycle": {
            "color": "red",
            "price": 19.95
        }
    },
    "expensive": 10
}
```
The Json data above has the following characteristics:
- It is a dictionary consisting of dictionary and also has a list(array) inside of it.

#### Querying Dictionaries
If I want to find out the price of the bicycle, I use the below query
```
$.store.bicycle.price
```
Result
```
[
    19.95
]
```

$ -> Is the root element of the Json document.
. -> Dot specifies the child. 

**Note:** 
- Jsonpath expressions can use dot-notation or bracket-notation. My personal preference is dot-notation.
- Jsonpath always returns a list.


#### Querying Lists/Arrays
If I want to find out the price of the book "Sword of Honour", I use the below query
```
$.store.book[1].price
```

Result

```
[
    12.99
]
```

[] -> Brackets are used with arrays, an index number within the array returns the element at that position. Positioning starts at 0.

The above queries were static in nature and if my Json document changes, the results may be different. I can use filter expressions to ensure I always get the data I want.

```
$.store.book[?(@.title == 'Sword of Honour')].price
```

Result
```
[
    12.99
]
```

?() -> Is the If expression used to filter information.
@ -> Represents each element in the list.

Once it finds a match, it'll print the price of the book I'm interested in. This way, I don't have to worry about index position.

- Working with lists, I may require to get information of the last book, I could use the following query

```
$.store.book[-1:]
```

```
[
    "category": "fiction",
    "author": "J. R. R. Tolkien",
    "title": "The Lord of the Rings",
    "isbn": "0-395-19395-8",
    "price": 22.99
]
```

- Use the comma to extrat specific items from the list

```
$.store.book[0,2]
```

Result
```
[
    {
                "category": "reference",
                "author": "Nigel Rees",
                "title": "Sayings of the Century",
                "price": 8.95
            },
            {
                "category": "fiction",
                "author": "Herman Melville",
                "title": "Moby Dick",
                "isbn": "0-553-21311-3",
                "price": 8.99
            }
]
```

- Use Colon to extract a subset from the list

Positive numbering for top to bottom

```
$.store.book[0:2]
```

Result
```
[
    {
                "category": "reference",
                "author": "Nigel Rees",
                "title": "Sayings of the Century",
                "price": 8.95
            },
            {
                "category": "fiction",
                "author": "Evelyn Waugh",
                "title": "Sword of Honour",
                "price": 12.99
            },
            {
                "category": "fiction",
                "author": "Herman Melville",
                "title": "Moby Dick",
                "isbn": "0-553-21311-3",
                "price": 8.99
            },
]
```

Negative numbering for extracting information from the end of the list
```
$.store.book[-2:0]
```

The above query will fetch last 2 elements of the list and for ease of use, 0 can be dropped so the query will be `$.store.book[-2:0]`

Result
```
[
    {
                "category": "fiction",
                "author": "Herman Melville",
                "title": "Moby Dick",
                "isbn": "0-553-21311-3",
                "price": 8.99
            },
            {
                "category": "fiction",
                "author": "J. R. R. Tolkien",
                "title": "The Lord of the Rings",
                "isbn": "0-395-19395-8",
                "price": 22.99
            }
]
```

#### Use of Wildcards
The following wilcards can be used with Jsonpath

If I want to list all the authors from the json document above, I could use the below query

```
$.store.book[*].author
```

Result
```
[
    "Nigel Rees",
    Evelyn Waugh",
    "Herman Melville",
    "J. R. R. Tolkien"
]
```

### JsonPath in Kubernetes

Kubectl supports jsonpath to extract information. When Kubectl communicates with `kube-api-server`, it receives information in json format and converts it in human readable format.

When using jsonpath with kubectl, it is easier to extract information in json format first and then writing the query to get started.

Below is a query to get the cpu capacity of pods

```
kubectl get pods -o=jsonpath='{.items[*].status.capacity.cpu}'
```

**Note:**
- No need to mention root element `$`
- Query enclosed within `'{}'`
- Separate queries can be combined together

#### Range command
The range command can be used to iterate over a list.

```
kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name} {"\t"} {.status.capacity.cpu}{"\n"}{end}'
```
Result
```
master 4
node01 4
```
#### Custom Columns in kubectl
Kubectl supports custom columns which can be used to get custom outputs.

Syntax for custom-columns
```
kubectl get nodes -o=custom-columns=<COLUMN NAME>:<JSON PATH>
```

The above information is not very useful as it doesn't specific the column names.

I can use the below command instead

```
kubectl get nodes -o=custom-columns=NODE:.metadata.name,CPU:.status.capacity.cpu
```

Result
```
NODE    CPU
master  4
node01  4
```

#### Sort-by for JsonPath
Kubectl supports sort-by option to sort the output by jsonpath query.

For example, I could sort the output of kubectl get nodes by name as below

```
kubectl get nodes --sort-by=.metadata.name
```

Result
```
NAME    STATUS  ROLES   AGE     VERSION
master  Ready   master  15m     v1.18.0
node01  Ready   <none>  14m     v1.18.0
```