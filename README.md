# inst.old.reprex

When installing an R package, folders within `inst` whose names end in "old" get filtered out and are not installed. This issue is not restricted to first-level subdirectories of `inst`.

For example, in this package we have the following directory strucuture in `inst`:

```
inst
|- old
|- young-old
|- models
   |- gold
   |- old-young
   |- old
   |- older
   |- young-old-young
   |- young-old
   |- young
```
If we install the package with 

```
remotes::install_github("papsti/inst.old.reprex")
```

and then do 

```
list.files(system.file(package = "inst.old.reprex"))
list.files(system.file("models", package = "inst.old.reprex"))
```

to list installed package files, we see that only

```
models
|- old-young
|- older
|- young-old-young
|- young
```

survived the install.

Based on these example directory names, it seems that folders ending with "old" somehow get filtered out. 

However, the `.Rbuildignore` file (default from `usethis::create_package()`) contains only

```
^.*\.Rproj$
^\.Rproj\.user$
```

so the missing folders aren't being ignored there.

Why/where are folders ending with "old" getting filtered out during install? I can't find documentation of this feature (bug?) online.
