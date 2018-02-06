## User Guide

The command-line usage of `ripgrep` doesn't differ much from other tools that
perform a similar function, so you probably already know how to use `ripgrep`.
The full details can be found in `rg --help`, but let's go on a whirlwind tour.

`ripgrep` detects when its printing to a terminal, and will automatically
colorize your output and show line numbers, just like The Silver Searcher.
Coloring works on Windows too! Colors can be controlled more granularly with
the `--color` flag.

One last thing before we get started: generally speaking, `ripgrep` assumes the
input it is reading to be UTF-8. However, if ripgrep notices a file is encoded as
UTF-16, then it will know how to search it. For other encodings, you'll need to
explicitly specify them with the `-E/--encoding` flag.

To recursively search the current directory, while respecting all `.gitignore`
files, ignore hidden files and directories and skip binary files:

```
$ rg foobar
```

The above command also respects all `.ignore` files, including in parent
directories. `.ignore` files can be used when `.gitignore` files are
insufficient. In all cases, `.ignore` patterns take precedence over
`.gitignore`.

To ignore all ignore files, use `-u`. To additionally search hidden files
and directories, use `-uu`. To additionally search binary files, use `-uuu`.
(In other words, "search everything, dammit!") In particular, `rg -uuu` is
similar to `grep -a -r`.

```
$ rg -uu foobar  # similar to `grep -r`
$ rg -uuu foobar  # similar to `grep -a -r`
```

(Tip: If your ignore files aren't being adhered to like you expect, run your
search with the `--debug` flag.)

Make the search case insensitive with `-i`, invert the search with `-v` or
show the 2 lines before and after every search result with `-C2`.

Force all matches to be surrounded by word boundaries with `-w`.

Search and replace (find first and last names and swap them):

```
$ rg '([A-Z][a-z]+)\s+([A-Z][a-z]+)' --replace '$2, $1'
```

Named groups are supported:

```
$ rg '(?P<first>[A-Z][a-z]+)\s+(?P<last>[A-Z][a-z]+)' --replace '$last, $first'
```

Up the ante with full Unicode support, by matching any uppercase Unicode letter
followed by any sequence of lowercase Unicode letters (good luck doing this
with other search tools!):

```
$ rg '(\p{Lu}\p{Ll}+)\s+(\p{Lu}\p{Ll}+)' --replace '$2, $1'
```

Search only files matching a particular glob:

```
$ rg foo -g 'README.*'
```

<!--*-->

Or exclude files matching a particular glob:

```
$ rg foo -g '!*.min.js'
```

Search and return paths matching a particular glob (i.e., `-g` flag in ag/ack):

```
$ rg -g 'doc*' --files
```

Search only HTML and CSS files:

```
$ rg -thtml -tcss foobar
```

Search everything except for Javascript files:

```
$ rg -Tjs foobar
```

To see a list of types supported, run `rg --type-list`. To add a new type, use
`--type-add`, which must be accompanied by a pattern for searching (`rg` won't
persist your type settings):

```
$ rg --type-add 'foo:*.{foo,foobar}' -tfoo bar
```

The type `foo` will now match any file ending with the `.foo` or `.foobar`
extensions.
