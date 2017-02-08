# Fetch and merge without a checkout

With a fast-forward merge you can use

`git fetch <remote> <sourceBranch>:<destinationBranch>`

If your merge is not fast-forward you can add a `+` in front of the refspec.

`git fetch <remote> +<sourceBranch>:<destinationBranch>`


[source](http://stackoverflow.com/a/17722977)
