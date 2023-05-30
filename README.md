# git-full-diff

Composite action to get the actual text diff between two commits

Right now, it only supports getting the added text on modified files, without any diff tags.

## Usage
To use it, you can simply add the following line on your GitHub Actions steps:
`uses: fernandosmither/git-full-diff@<TAG>`

Make sure to change TAG with the desired version. Eg: `git-full-diff@v1.3.2`
### Usage example:
.github/workflows/main.yaml:
```
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: fernandosmither/git-full-diff@v1.3.2
```
**To use this action, you must also invoke actions/checkout before with the tag fetch-depth: 0, as actions/checkout gives you by default a grafted git history only with the latest commit.**


For example:

If the `git diff` is:
```
diff --git a/README.md b/README.md
index eadb0f6..eb451f1 100644
--- a/README.md
+++ b/README.md
@@ -2,4 +2,8 @@
 
 # New added title
 
-Text!
\ No newline at end of file
+
+
+# This is a new header
+
+This is new text
```

You would get:
```


# This is a new header

This is new text
```

If you'd like a similar use case (for instance, showing only deleted text), please open an issue explaining your case and desired output, and I'll deliver ASAP.
