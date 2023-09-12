# inst.old.reprex

When installing an R package, folders within `inst` whose names end in "old" get filtered out and are not installed. This issue is not restricted to first-level subdirectories of `inst`.

For example, in this package we have the following directory structure in `inst`:

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

---

Thanks to @bbolker, I have an answer! From the [Writing R Extensions page](https://cran.r-project.org/doc/manuals/R-exts.html):

> In addition, directories from source control systems[57] or from
eclipse[58], directories with names check, chm, or ending .Rcheck or Old
or old and files GNUMakefile[59], Read-and-delete-me or with base names
starting with ‘.#’, or starting and ending with ‘#’, or ending in ‘~’,
‘.bak’ or ‘.swp’, are excluded by default[60]

Wonderful!

Note 60:

> see tools:::.hidden_file_exclusions and tools:::get_exclude_patterns()
for further excluded files and file patterns, respectively.
