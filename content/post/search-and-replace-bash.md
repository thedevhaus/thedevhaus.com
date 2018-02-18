---
title: "Search and Replace Bash"
date: 2018-02-17T15:28:19+11:00
draft: false
weight: 10
tags : ["linux", "unix", "bash"]
---
# Search/replace string recursively in Linux

```bash
grep -rnw ./*.* -e "string-to-search"
```

## Replace all occurrences bash

```bash
sed -i 's/FROM/WITH/g' filenamePattern
```
For  e.g.

```bash
sed -i 's/ash@gmail.com/someone@outloo.com/g' *.txt
```