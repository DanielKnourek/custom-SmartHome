test-key
1-HfTwtXAFnDDJ44iyYiFJDnDw3ucfBdl1ogEGmUzkM5ahFotuXJzARxzNWin9u4RB

curl -X GET "http://dakara.local:81/api/v2.0/catalog" -H  "accept: */*" -H  "Authorization: Basic $(op read "op://Private/dakara admin UI api/credential")"

```BASH
# invalid SSL certificate
curl -X GET "https://192.168.0.21:444/api/v2.0/core/ping" -H  "accept: */*" -H  "Authorization: Basic $("op://Private/dakara admin UI api/pqnv4ggvjvbikqpywne7dga7xa")"

# using http
curl -X GET "http://192.168.0.21:81/api/v2.0/core/ping" -H  "accept: */*" -H  "Authorization: Basic $("op://Private/dakara admin UI api/pqnv4ggvjvbikqpywne7dga7xa")"
```
