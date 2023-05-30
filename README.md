# git-full-diff

Composite action to get the actual text diff between two commits

Right now, it only supports getting the added text on modified files, without any diff tags.

## Usage
To use it, you can simply add the following line on your GitHub Actions steps:
`uses: fernandosmither/git-full-diff@<TAG>`

Then, you can access your diff with `${{ steps.<step_id>.outputs.diff }}`

Make sure to change TAG with the desired version. Eg: `git-full-diff@v1.0.0`
### Usage example:
> This example gets the full diff and outputs it.

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
      - id: git-full-diff
        uses: fernandosmither/git-full-diff@v1.0.2
      - name: display diff
        run: |
          EOF=$(dd if=/dev/urandom bs=15 count=1 status=none | base64)
          cat <<$EOF
          ${{ steps.git-full-diff.outputs.diff }}
          $EOF
        shell: bash
```
* **To use this action, you must also invoke actions/checkout before with the tag fetch-depth: 0, as actions/checkout gives you by default a grafted git history only with the latest commit.**
* You could simply display the output with `echo ${{ steps.git-full-diff.outputs.diff }}`. However, since in this case (and also probably yours) `${{ steps.git-full-diff.outputs.diff }}` is a multiline string you cannot simply echo it. The code example shows the cleanest way I found to accomplish this. If you have any suggestions to improve the code, let me know in an Issue!


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
