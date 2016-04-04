# Sample Usage 
* Deploy the 'security' webapp module, sample useage below

### Authenticate and receive OAuth access token
* Authenticate via OAuth password grantType and receive OAuth access token and refresh token 

```bash
[tut-bookmarks]$curl -X POST -vu android-bookmarks:123456 http://localhost:8080/oauth/token -H "Accept: application/json" -d "password=password&username=jlong&grant_type=password&scope=write&client_secret=123456&client_id=android-bookmarks"
Note: Unnecessary use of -X or --request, POST is already inferred.
* STATE: INIT => CONNECT handle 0x6000572e0; line 1103 (connection #-5000)
* Added connection 0. The cache now contains 1 members
*   Trying 127.0.0.1...
* STATE: CONNECT => WAITCONNECT handle 0x6000572e0; line 1156 (connection #0)
* Connected to localhost (127.0.0.1) port 8080 (#0)
* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x6000572e0; line 1253 (connection #0)
* STATE: SENDPROTOCONNECT => DO handle 0x6000572e0; line 1271 (connection #0)
* Server auth using Basic with user 'android-bookmarks'
> POST /oauth/token HTTP/1.1
> Host: localhost:8080
> Authorization: Basic YW5kcm9pZC1ib29rbWFya3M6MTIzNDU2
> User-Agent: curl/7.47.0
> Accept: application/json
> Content-Length: 113
> Content-Type: application/x-www-form-urlencoded
>
* upload completely sent off: 113 out of 113 bytes
* STATE: DO => DO_DONE handle 0x6000572e0; line 1350 (connection #0)
* STATE: DO_DONE => WAITPERFORM handle 0x6000572e0; line 1477 (connection #0)
* STATE: WAITPERFORM => PERFORM handle 0x6000572e0; line 1487 (connection #0)
* HTTP 1.1 or later with persistent connection, pipelining supported
< HTTP/1.1 200 OK
* Server Apache-Coyote/1.1 is not blacklisted
< Server: Apache-Coyote/1.1
< Access-Control-Allow-Origin: http://localhost:9000
< Access-Control-Allow-Methods: POST,GET,OPTIONS,DELETE
< Access-Control-Max-Age: 3600
< Access-Control-Allow-Credentials: true
< Access-Control-Allow-Headers: Origin,Accept,X-Requested-With,Content-Type,Access-Control-Request-Method,Access-Control-Request-Headers,Authorization
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
< Cache-Control: no-store
< Pragma: no-cache
< Content-Type: application/json;charset=UTF-8
< Transfer-Encoding: chunked
< Date: Tue, 29 Mar 2016 13:44:46 GMT
<
* STATE: PERFORM => DONE handle 0x6000572e0; line 1645 (connection #0)
* Curl_done
* Connection #0 to host localhost left intact
{"access_token":"aea1cc7e-de0e-4ee1-a80a-07573db95766","token_type":"bearer","refresh_token":"97dea961-b464-42c5-8897-1bb09d513731","expires_in":43199,"scope":"write"}
```

### Authenticate with OAuth Access/Bearer token and list bookmarks
```bash
[tut-bookmarks]$curl -v http://127.0.0.1:8080/bookmarks -H "Authorization: Bearer aea1cc7e-de0e-4ee1-a80a-07573db95766"
* STATE: INIT => CONNECT handle 0x6000572e0; line 1103 (connection #-5000)
* Added connection 0. The cache now contains 1 members
*   Trying 127.0.0.1...
* STATE: CONNECT => WAITCONNECT handle 0x6000572e0; line 1156 (connection #0)
* Connected to 127.0.0.1 (127.0.0.1) port 8080 (#0)
* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x6000572e0; line 1253 (connection #0)
* STATE: SENDPROTOCONNECT => DO handle 0x6000572e0; line 1271 (connection #0)
> GET /bookmarks HTTP/1.1
> Host: 127.0.0.1:8080
> User-Agent: curl/7.47.0
> Accept: */*
> Authorization: Bearer aea1cc7e-de0e-4ee1-a80a-07573db95766
>
* STATE: DO => DO_DONE handle 0x6000572e0; line 1350 (connection #0)
* STATE: DO_DONE => WAITPERFORM handle 0x6000572e0; line 1477 (connection #0)
* STATE: WAITPERFORM => PERFORM handle 0x6000572e0; line 1487 (connection #0)
* HTTP 1.1 or later with persistent connection, pipelining supported
< HTTP/1.1 200 OK
* Server Apache-Coyote/1.1 is not blacklisted
< Server: Apache-Coyote/1.1
< Access-Control-Allow-Origin: http://localhost:9000
< Access-Control-Allow-Methods: POST,GET,OPTIONS,DELETE
< Access-Control-Max-Age: 3600
< Access-Control-Allow-Credentials: true
< Access-Control-Allow-Headers: Origin,Accept,X-Requested-With,Content-Type,Access-Control-Request-Method,Access-Control-Request-Headers,Authorization
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
< Set-Cookie: JSESSIONID=D51D2160953EC5E493B845B106F60849; Path=/; HttpOnly
< Content-Type: application/json
< Transfer-Encoding: chunked
< Date: Tue, 29 Mar 2016 14:04:15 GMT
<
* STATE: PERFORM => DONE handle 0x6000572e0; line 1645 (connection #0)
* Curl_done
* Connection #0 to host 127.0.0.1 left intact
* Expire cleared
{"_embedded":{"bookmarkResourceList":[{"bookmark":{"id":15,"uri":"http://bookmark.com/1/jlong","description":"A description"},"_links":{"bookmark-uri":{"href":"http://bookmark.com/1/jlong"},"bookmarks":{"href":"http://127.0.0.1:8080/bookmarks"},"self":{"href":"http://127.0.0.1:8080/bookmarks/15"}}},{"bookmark":{"id":16,"uri":"http://bookmark.com/2/jlong","description":"A description"},"_links":{"bookmark-uri":{"href":"http://bookmark.com/2/jlong"},"bookmarks":{"href":"http://127.0.0.1:8080/bookmarks"},"self":{"href":"http://127.0.0.1:8080/bookmarks/16"}}}]}}
```


### Authenticate with OAuth Access/Bearer token and list specific bookmark
* List just bookmark with id of 15

```bash
curl http://127.0.0.1:8080/bookmarks/15 -H "Authorization: Bearer aea1cc7e-de0e-4ee1-a80a-07573db95766"
{"bookmark":{"id":15,"uri":"http://bookmark.com/1/jlong","description":"A description"},"_links":{"bookmark-uri":{"href":"http://bookmark.com/1/jlong"},"bookmarks":{"href":"http://127.0.0.1:8080/bookmarks"},"self":{"href":"http://127.0.0.1:8080/bookmarks/15"}}}
```

### POST new bookmark as JSON
* Authenticate with OAuth access/bearer token 
* Note we get back a response of HTTP 201 created
 
```bash 
[tut-bookmarks]$curl -v -X POST http://127.0.0.1:8080/bookmarks -d '{"uri":"http://dave.dl.ac.uk","description":"daves page"}' -H "Content-Type: application/json" -H "Authorization: Bearer aea1cc7e-de0e-4ee1-a80a-07573db95766"
Note: Unnecessary use of -X or --request, POST is already inferred.
* STATE: INIT => CONNECT handle 0x6000572e0; line 1103 (connection #-5000)
* Added connection 0. The cache now contains 1 members
*   Trying 127.0.0.1...
* STATE: CONNECT => WAITCONNECT handle 0x6000572e0; line 1156 (connection #0)
* Connected to 127.0.0.1 (127.0.0.1) port 8080 (#0)
* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x6000572e0; line 1253 (connection #0)
* STATE: SENDPROTOCONNECT => DO handle 0x6000572e0; line 1271 (connection #0)
> POST /bookmarks HTTP/1.1
> Host: 127.0.0.1:8080
> User-Agent: curl/7.47.0
> Accept: */*
> Content-Type: application/json
> Authorization: Bearer aea1cc7e-de0e-4ee1-a80a-07573db95766
> Content-Length: 57
>
* upload completely sent off: 57 out of 57 bytes
* STATE: DO => DO_DONE handle 0x6000572e0; line 1350 (connection #0)
* STATE: DO_DONE => WAITPERFORM handle 0x6000572e0; line 1477 (connection #0)
* STATE: WAITPERFORM => PERFORM handle 0x6000572e0; line 1487 (connection #0)
* HTTP 1.1 or later with persistent connection, pipelining supported
< HTTP/1.1 201 Created
* Server Apache-Coyote/1.1 is not blacklisted
< Server: Apache-Coyote/1.1
< Access-Control-Allow-Origin: http://localhost:9000
< Access-Control-Allow-Methods: POST,GET,OPTIONS,DELETE
< Access-Control-Max-Age: 3600
< Access-Control-Allow-Credentials: true
< Access-Control-Allow-Headers: Origin,Accept,X-Requested-With,Content-Type,Access-Control-Request-Method,Access-Control-Request-Headers,Authorization
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
< Set-Cookie: JSESSIONID=4E71BE7DC2826AA3342073AB0E2D1057; Path=/; HttpOnly
< Location: http://127.0.0.1:8080/bookmarks/17
< Content-Length: 0
< Date: Tue, 29 Mar 2016 14:36:35 GMT
<
* STATE: PERFORM => DONE handle 0x6000572e0; line 1645 (connection #0)
* Curl_done
* Connection #0 to host 127.0.0.1 left intact
* Expire cleared
```


### Note how new bookmark has been added
* List again the bookmarks and note that new bookmark has been added 

```bash 
[tut-bookmarks]$curl http://127.0.0.1:8080/bookmarks -H "Authorization: Bearer aea1cc7e-de0e-4ee1-a80a-07573db95766"
{"_embedded":{"bookmarkResourceList":[{"bookmark":{"id":15,"uri":"http://bookmark.com/1/jlong","description":"A description"},"_links":{"bookmark-uri":{"href":"http://bookmark.com/1/jlong"},"bookmarks":{"href":"http://127.0.0.1:8080/bookmarks"},"self":{"href":"http://127.0.0.1:8080/bookmarks/15"}}},{"bookmark":{"id":16,"uri":"http://bookmark.com/2/jlong","description":"A description"},"_links":{"bookmark-uri":{"href":"http://bookmark.com/2/jlong"},"bookmarks":{"href":"http://127.0.0.1:8080/bookmarks"},"self":{"href":"http://127.0.0.1:8080/bookmarks/16"}}},{"bookmark":{"id":17,"uri":"http://dave.dl.ac.uk","description":"daves page"},"_links":{"bookmark-uri":{"href":"http://dave.dl.ac.uk"},"bookmarks":{"href":"http://127.0.0.1:8080/bookmarks"},"self":{"href":"http://127.0.0.1:8080/bookmarks/17"}}}]}}
```



### Request with unsupported media type
* Note we get back 414 HTTP error 

```bash  
[tut-bookmarks]$curl -v -X POST http://127.0.0.1:8080/bookmarks --data "uri=http://dave.dl.ac.uk&description=daves page"  -H "Authorization: Bearer aea1cc7e-de0e-4ee1-a80a-07573db95766"
...
{"timestamp":1459261443025,"status":415,"error":"Unsupported Media Type","exception":"org.springframework.web.HttpMediaTypeNotSupportedException","message":"Content type 'application/x-www-form-urlencoded' not supported","path":"/bookmarks"}
```


