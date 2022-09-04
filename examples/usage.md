## Usage

### Given

`tests/a.txt`:

```txt
abc
def
ghi
```

`tests/b.txt`:

```txt
lorem ipsum dolor sit amet
```

### Use the last arguments separated by double hyphens

Command:

```sh
ddx -X sed 's/e/E/g' -- tests/*.txt
```

Output:

```diff
--- tests/a.txt
+++ tests/a.txt
@@ -1,3 +1,3 @@
 abc
-def
+dEf
 ghi
--- tests/b.txt
+++ tests/b.txt
@@ -1 +1 @@
-lorem ipsum dolor sit amet
+lorEm ipsum dolor sit amEt
```

### Use the last arguments as an input file

Command:

```sh
ddx -x sed 's/e/E/g' tests/a.txt
```

```diff
--- tests/a.txt
+++ tests/a.txt
@@ -1,3 +1,3 @@
 abc
-def
+dEf
 ghi
```

### As filter

Command:

```sh
ddx -F sed 's/e/E/g' <tests/a.txt
```

```diff
--- <stdin>
+++ <stdout>
@@ -1,3 +1,3 @@
 abc
-def
+dEf
 ghi
```

### With `find`

Command:

```sh
find tests -name '*.txt' | ddx sed 's/e/E/g'
# or
find tests -name '*.txt' -print0 | ddx -0 sed 's/e/E/g'
# or
find tests -name '*.txt' -exec ddx -X sed 's/e/E/g' -- '{}' +
```

### With `fd`

Command:

```sh
fd '\.txt$' tests | ddx sed 's/e/E/g'
# or
fd -0 '\.txt$' tests | ddx -0 sed 's/e/E/g'
# or
fd '\.txt$' tests -X ddx -X sed 's/e/E/g' --
```

### With `rg`

Command:

```sh
rgdiff () {
    PAT="$1"
    REP="$2"
    shift 2
    rg -0l "$PAT" "$@" | ddx -0 rg "$PAT" -r "$REP" -IN --passthru
}
rgdiff 'e' 'E' tests/ -g '*.txt'
```
