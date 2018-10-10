### head & cut commands (bash)

Date: 2018-09-20

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

### sorting a property of a struct (golang)

Date: 2018-07-17

https://play.golang.org/p/wVtZsfPZ_gB

### open (and create) a file on append mode (golang)

Date: 2018-07-17

```go
f, err := os.OpenFile(file, os.O_WRONLY|os.O_CREATE|os.O_APPEND, 0666)
if err != nil {
	return err
}
defer f.Close()

i += "\n"

_, err = f.WriteString(i)

return err
```

### prune on fetch (git)

```
git config --global fetch.prune true
```

After a PR is merged & you delete your branch from GitHub, the next time you
fetch from your repo, it automatically removes the now-merged branch.

Reference:
[tweet](https://twitter.com/andygoldstein/status/1046783561230163969?s=19)

### stash pull pop (git)

```
git pull --autostash
```

Stashes before pull and pops after.

Reference: [tweet](https://twitter.com/patao_/status/1046837635786915840?s=19)

### const enums (typescript)

`const enum`s are compiled as string literals while regular `enum`s are compiled
as a function with state.

```ts
const enum const_enum {
  foo = "foo",
  bar = "bar"
}

enum regular_enum {
  foo = "foo",
  bar = "bar"
}
```

[See compiled code](https://goo.gl/985iiV).

Reference: [tweet](https://twitter.com/sulco/status/1049361905914204161?s=21).
