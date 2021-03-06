# `globstandard`: The missing glob standard

IEEE and POSIX forgot to define a standard, so here is the standard. Following `globstandard` will allow your users to re-use the globs they know without unexpected behavior.

## Glob Primer

"Globs" are the patterns you type when you do stuff like `ls *.js` on the command line, or put `build/*` in a `.gitignore` file.

Before parsing the path part patterns, braced sections are expanded into a set. Braced sections start with `{` and end with `}`, with any number of comma-delimited sections within. Braced sections may contain slash characters, so `a{/b/c,bcd}` would expand into `a/b/c` and `abcd`.

The following characters have special magic meaning when used in a path portion:

-   `*` Matches 0 or more characters in a single path portion
-   `?` Matches 1 character
-   `[...]` Matches a range of characters, similar to a RegExp range. If the first character of the range is `!` or `^` then it matches any character not in the range.
-   `!(pattern|pattern|pattern)` Matches anything that does not match any of the patterns provided.
-   `?(pattern|pattern|pattern)` Matches zero or one occurrence of the patterns provided.
-   `+(pattern|pattern|pattern)` Matches one or more occurrences of the patterns provided.
-   `*(a|b|c)` Matches zero or more occurrences of the patterns provided
-   `@(pattern|pat*|pat?erN)` Matches exactly one of the patterns provided
-   `**` If a "globstar" is alone in a path portion, then it matches zero or more directories and subdirectories searching for matches. **It does not crawl symlinked directories.**

### Dots

If a file or directory path portion has a `.` as the first character, then it will not match any glob pattern unless that pattern's corresponding path part also has a `.` as its first character.

For example, the pattern `a/.*/c` would match the file at `a/.b/c`. However the pattern `a/*/c` would not, because `*` does not start with a dot character.

You can make glob treat dots as normal characters by setting `dot:true` in the options.

### Basename matching option

**Default: false**

When basename matching is enabled, `**/*/` is inserted to the beginning of any pattern not beginning with `/` or `./`.

Recommended naming:

-   camelCase: `matchBase`
-   CLI arg: `--match-base` or `-b`
-   In a pattern: `mypattern~~B`

## Not yet in this standard yet

-   character escaping
-   separate exclusions array
-   try and see editor
-   table of compliance and gotchas in popular tools
