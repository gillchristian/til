### head & cut commands (bash)

`head` output the first part of files

`cut` remove sections from each line of files

```
$ curl -ILs https://example.com
HTTP/1.1 200 OK
Content-Encoding: gzip
Accept-Ranges: bytes
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Wed, 26 Sep 2018 11:41:40 GMT
Etag: "1541025663"
Expires: Wed, 03 Oct 2018 11:41:40 GMT
Last-Modified: Fri, 09 Aug 2013 23:54:35 GMT
Server: ECS (dca/24A0)
X-Cache: HIT
Content-Length: 606

$ curl -ILs https://example.com | head -n 1
HTTP/1.1 200 OK

$ curl -ILs https://example.com | head -n 1 | cut -d ' ' -f 2
200
```
