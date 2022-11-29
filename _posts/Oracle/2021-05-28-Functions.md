---
layout: post
title : Functions
author: Mégane Vilain
category: Oracle
---

## To date

```sql
TO_DATE( string1 [, format_mask] [, nls_language] )

```

### Examples

```sql
TO_DATE('2003/07/09', 'yyyy/mm/dd')

TO_DATE('070903', 'MMDDYY')

TO_DATE('20020315', 'yyyymmdd')

TO_DATE('2015/05/15 8:30:25', 'YYYY/MM/DD HH:MI:SS')
```
