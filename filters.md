# Filters
## Extension 
```
{
  "bool": {
    "minimum_should_match": 1,
    "should": [
      {
        "wildcard": {
          "message": "*.mpd*"
        }
      },
      {
        "wildcard": {
          "message": "*.h3u8*"
        }
      }
    ]
  }
}
```
## Latency
```
{
  "range": {
    "latency": {
      "gte": 2000
    }
  }
}
```

-  ##### OR
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
        emit(Integer.parseInt(String.valueOf(words[11])));
    }
}
```