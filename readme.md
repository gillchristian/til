<h1 align="center">:mortar_board: Today I Learned :book:</h1>

<p align="center">Small things and tips I learn every day</p>

## Workflow

### Adding

Whenever I learn something new I add it here in `til.md`. The format is simple.

```md
## title (topic/s)

<!-- Content-->

Snippet, longer description and/or asciinema recording ...

<!-- Reference -->

Reference: [link to tweet/blogpost/slides/etc](#)
```

**Title**: Meaningful and concise, what I learned in a few words.

**Topic/s**: One or two (maybe three) topics. Alongside with the title, the
topics are a way to easily search. The less topics the easier it will be to
search, for example for JavaScript entries don't use "js" in ones and
"javascript" in others.

**Content**: A longer explanation of what I learned. A short description,
snippet, repl.it, [CodeSandbox](https://codesandbox.io/) and/or recording of the
terminal (using the awesome [asciinema](https://asciinema.org)). If I happen to
forget about this I can come back and quickly remember. _That's the whole
point!_

**Reference**: It's important to give credit to where/who I learned from. Also
the reference might give a longer/deeper explanation.

### Searching

I have this function in [my dotfiles](https://github.com/gillchristian/dotfiles)
to search by title and topic/s.

```bash
function til {
  cat til.md | rg '#' | cut -d ' ' -f 2- | rg -i $@
}
```

See it working here[here](https://asciinema.org/a/210702).

_NOTE_: I'm using the amazing [ripgrep](https://github.com/BurntSushi/ripgrep)
(rg) but you can replace it with your grep tool of choice.
