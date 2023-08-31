
# Columns

## Message
``` 
GET _cat/indices 
```
``` 
PUT .ds-filebeat-8.9.0-2023.08.23-000001/_mapping
{
  "properties": {
    "message" : {
      "type" : "match_only_text",
      "fields": {
        "keyword": {
          "type" : "keyword"
        }
      }
    }
  }
}
```
```
POST .ds-filebeat-8.9.0-2023.08.23-000001/_update_by_query?wait_for_completion=false
```
```
POST .ds-filebeat-8.9.0-2023.08.23-000001/_search
 {
  "size": 0,
  "aggs": {
    "type_promoted_count": {
      "cardinality": {
        "field": "message.keyword"
      }
    }
  }
}
```


 ## Endpoint
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('/remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    if(Integer.parseInt(String.valueOf(words.length)) > 13 && words[13] != '' && words[12] != ''){
       emit(words[12] + ' ' + words[13])
    }
    }
}
```
-  ##### OR
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('/remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    if(Integer.parseInt(String.valueOf(words.length)) > 13 && words[13] != ''){
       emit(words[13])
    }
    }
}
```


## Channel
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('/remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    if(Integer.parseInt(String.valueOf(words.length)) > 13){
    def url = words[13].splitOnToken('/');
      if(Integer.parseInt(String.valueOf(url.length)) > 3 && url[3] != ''){
       emit(url[3])
    }
    }
}
}
```

## Output 
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('/remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    if(Integer.parseInt(String.valueOf(words.length)) > 13){
    def url = words[13].splitOnToken('/');
      if(Integer.parseInt(String.valueOf(url.length)) > 4 && url[4] != ''){
       emit(url[4])
    }
    }
}
}
```