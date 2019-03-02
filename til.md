### head & cut commands (unix)

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

### map & filter JSON (unix jq)

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

## cut delimiter new line (unix)

```bash
$ cat file.txt | cut -d$'\\n' -f 1-3
```

Reference: [SO](https://stackoverflow.com/a/21757210/4530566).

## Rename a git branch (git)

```
$ git branch -m new-name
$ git branch -m old-name new-name
```

```
$ git push origin :old-name new-name
$ git push origin -u new-name
```

Reference:
[Rename a local and remote branch in git](https://multiplestates.wordpress.com/2015/02/05/rename-a-local-and-remote-branch-in-git/).

## Remove lines containing pattern (vim)

:boom:

```
:g/<pattern>/d
```

The opposite is also possible, remove all lines NOT containing the pattern.

```
:g!/<pattern>/d
:v/<pattern>/d
```

Reference:
[Delete all lines containing a pattern](http://vim.wikia.com/wiki/Delete_all_lines_containing_a_pattern).

## Pipe to command and also show in terminal (unix)

I copy stuff from commands output on a daly basis using xclip.

```bash
alias c='xclip -selection c'

echo 'foo' | c
```

The problem is te output doesn't show in the screen since `stdout` is piped.

I already knew about [tee](<https://en.wikipedia.org/wiki/Tee_(command)>). What
I learned is, anything that's written to `/dev/tty` is outputted to the
terminal.

```
echo 'foo' | tee /dev/tty | c
```

That would copy 'foo' but also show it in the terminal :ok_hand:

Reference:
[What is special about /dev/tty? (answer)](https://stackoverflow.com/a/8514853/4530566).

## Use TypeApplications to specialize type class instances (haskell)

Using
[TypeApplications](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeApplications)
we can specialize class instances in GHCi.

```
λ> :set -XTypeApplications

λ> :t (<*>) @((->) _)
(<*>) @((->) _) :: (w -> a -> b) -> (w -> a) -> w -> b

λ> :t (>>=)
(>>=) :: Monad m => m a -> (a -> m b) -> m b

λ> :t (<*>) @((->) _)
(<*>) @((->) _) :: (w -> a -> b) -> (w -> a) -> w -> b

λ> :t (>>=) @((->) _)
(>>=) @((->) _) :: (w -> a) -> (a -> w -> b) -> w -> b

λ> :t (<*>) @Maybe
(<*>) @Maybe :: Maybe (a -> b) -> Maybe a -> Maybe b

λ> :t (>>=) @Maybe
(>>=) @Maybe :: Maybe a -> (a -> Maybe b) -> Maybe b

λ> :t (<*>) @[]
(<*>) @[] :: [a -> b] -> [a] -> [b]

λ> :t (>>=) @[]
(>>=) @[] :: [a] -> (a -> [b]) -> [b]
```

Credit: [@mvaldesdeleon](https://twitter.com/mvaldesdeleon).

## Making your own shell is "easy" (shell)

```js
const readline = require("readline");
const { spawn } = require("child_process");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

rl.setPrompt("$ ");
rl.prompt();

rl.on("line", line => {
  const words = line.split(/\s+/);
  const bin = words[0];
  const args = words.slice(1);

  const child = spawn(bin, args);

  child.stdout.pipe(rl.output);
  child.stderr.pipe(rl.output);

  child.on("exit", () => {
    rl.prompt();
  });
});
```

Reference:
[gist.github.com/fvictorio/ca43f0f94...](https://gist.github.com/fvictorio/ca43f0f942fb1bf3b5025e0bc866f465)

Credit: [@tqbfjotld2](https://twitter.com/tqbfjotld2) /
[fvictorio](https://github.com/fvictorio).

## Convert/rezip directory to epub (zip, unix)

```bash
$ cd some-epub-directory
$ zip -rX "../$(basename "$(realpath .)").epub" mimetype $(ls|xargs echo|sed 's/mimetype//g')
```

Reference:
[Ebooks StackExchange: How to repack an epub file from command line](https://ebooks.stackexchange.com/a/7278).
