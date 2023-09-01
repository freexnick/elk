
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
## Status Code
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    def status_code = "";
    if(words[1] == ''){
        status_code = words[9];
    } else {
        status_code = words[8];
    }

     emit(Integer.parseInt(String.valueOf(status_code)))
    }
}
``` 

 ## Endpoint
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    def endpoint = "";
    if(words[1] == ""){
        endpoint = words[14];
    } else {
        endpoint = words[13];
    }
    
     emit(endpoint)
    }
}
```


## Channel
```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    def channel = "";
    if(words[1] == ""){
        channel = words[14];
    } else {
        channel = words[13];
    }
    if(channel != "" && channel.contains("/")){
    def url = channel.splitOnToken('/');
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
if(filepath.size() == 1 && filepath.value.contains('remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    def output = "";
    if(words[1] == ""){
        output = words[14];
    } else {
        output = words[13];
    }
    if(output.contains("/")){
    def url = output.splitOnToken('/');
    if(Integer.parseInt(String.valueOf(url.length)) > 4 && url[4] != ''){
       emit(url[4])
    }
    }
    }
}
```

## Latency

```
def filepath = doc['log.file.path'];
if(filepath.size() == 1 && filepath.value.contains('remotelogs/')){
    def message = doc['message.keyword'];
    if(message.size()==1){
    def words = message.value.splitOnToken(' ');
    def latency = "";
    if(words[1] == ""){
        latency = words[11];
    } else {
        latency = words[10];
    }

     emit(Integer.parseInt(String.valueOf(latency)))
    }
}
```