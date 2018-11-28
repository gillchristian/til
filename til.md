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

### sorting a property of a struct (golang)

https://play.golang.org/p/wVtZsfPZ_gB

### open (and create) a file on append mode (golang)

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

### fix "does not support indexing" error (golang)

```go
func foo(m *map[string]string) {
  m["foo"] // invalid operation: m["foo"] (type *map[string]string does not support indexig)
}

func foo(m *map[string]string) {
  (*m)["foo"] // Yay!!!
}
```

`m` is a pointer, can't be indexed. First needs to be dereferenced.

Reference: [article](https://flaviocopes.com/golang-does-not-support-indexing/)

### color words diff (git)

```
git diff --color-words
```

[asciinema](https://asciinema.org/a/209841)

### map & filter JSON (bash jq)

I wanted to find the link for the "category theory" book in this JSON (~300
`item`s).

```json
{
  "reads": 15,
  "added": 268,
  "items": [
    "https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/",
    "https://twitter.com/old_sound/status/1002223622470201345?s=19",
    "https://erikbern.com/2018/05/02/interviewing-is-a-noisy-prediction-problem.html",
    "http://johnbender.us/2012/02/29/faster-javascript-through-category-theory/"
    "http://blog.codepipes.com/testing/software-testing-antipatterns.html",
    "https://mrpandey.github.io/d3graphTheory/index.html",
    ...
  ]
}
```

[jq](https://stedolan.github.io/jq/) to the rescue :muscle:

```bash
$ cat ~/.reading-list | jq '.items | map(select(contains("category")))'
[
  "https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/",
  "http://johnbender.us/2012/02/29/faster-javascript-through-category-theory/"
]
```

### Change default terminal with update-alternatives & x-terminal-emulator (unix)

I installed [alacritty](https://github.com/jwilm/alacritty) manually with Cargo.
That means `x-terminal-emulator` doesn't show it as one of the options so it
cannot be used as the defualt terminal.

Normally it could be updated like so:

```
$ sudo update-alternatives --config x-terminal-emulator
There are 7 choices for the alternative x-terminal-emulator (providing /usr/bin/x-terminal-emulator).

  Selection    Path                                      Priority   Status
------------------------------------------------------------
* 0            /usr/bin/terminator                        50        auto mode
  1            /usr/bin/gnome-terminal.wrapper            40        manual mode
  2            /usr/bin/koi8rxterm                        20        manual mode
  3            /usr/bin/lxterm                            30        manual mode
  4            /usr/bin/terminator                        50        manual mode
  5            /usr/bin/uxterm                            20        manual mode
  6            /usr/bin/xterm                             20        manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

Which means `terminator` is my default terminal (opens when pressing
`Ctrl+Alt+t`).

```
update-alternatives - maintain symbolic links determining default commands
```

It is possible to add new entries to `update-alternatives`:

```bash
sudo update-alternatives --install <link> <name> <path> <priority>
```

For `alacritty` it was:

```bash
sudo update-alternatives --install $(which x-terminal-emulator) x-terminal-emulator $(which alacritty) 60
```
