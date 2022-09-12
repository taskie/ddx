# ddx

a differential operator for CLI tools.

## Usage

### Basic usage

```sh-session
$ cat 1.txt
foo
$ echo 1.txt | ddx sed 's/foo/bar/g'
--- 1.txt
+++ 1.txt
@@ -1 +1 @@
-foo
+bar
$ echo 1.txt | ddx sed 's/foo/bar/g' | patch -p0
patching file 1.txt
$ cat 1.txt
bar
```

### With a single argument

```sh
ddx -x sed 's/foo/bar/g' 1.txt
```

### With multiple arguments

```sh
ddx -X sed 's/foo/bar/g' -- *.txt
```

### As a filter

```sh
ddx -F sed 's/foo/bar/g' <1.txt
```

### With `find`

```sh
find . -name '*.txt' | ddx sed 's/foo/bar/g'
# or
find . -name '*.txt' -print0 | ddx -0 sed 's/foo/bar/g'
# or
find . -name '*.txt' -exec ddx -x sed 's/foo/bar/g' '{}' ';'
# or
find . -name '*.txt' -exec ddx -X sed 's/foo/bar/g' -- '{}' +
```

### With `fd`

```sh
fd '\.txt$' | ddx sed 's/foo/bar/g'
# or
fd -0 '\.txt$' | ddx -0 sed 's/foo/bar/g'
# or
fd '\.txt$' -x ddx -x sed 's/foo/bar/g'
# or
fd '\.txt$' -X ddx -X sed 's/foo/bar/g' --
```

### With `rg`

```sh
rgdiff() {
    pat="$1"
    rep="$2"
    shift 2
    rg -0l "$pat" "$@" | ddx -0 rg "$pat" -r "$rep" -IN --passthru
}

rgdiff foo bar -g '*.txt'
```
