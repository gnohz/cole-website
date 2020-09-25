---
title: "Parsing Bash Output Into Json With jq"
date: 2020-08-06T15:04:26-04:00
draft: true
---

I needed to take the output of a `xfs_quota` and parse it into a `json` for another program to read. `jq` came in very handy.

The output of xfs_quota looks something like:
```
User quota on /srv/export/g_34166 (/dev/vdb)
                               Blocks                                          Inodes                                     Realtime Blocks
User ID          Used       Soft       Hard    Warn/Grace           Used       Soft       Hard    Warn/ Grace           Used       Soft       Hard    Warn/Grace
---------- -------------------------------------------------- -------------------------------------------------- --------------------------------------------------
root                8          0          0     00 [--------]          6          0          0     00 [--------]          0          0          0     00 [--------]
u_316301_g_34166        932          0          0     00 [--------]        274          0          0     00 [--------]          0          0          0     00 [--------]
u_270426_g_73222          0   19922944   20971520     00 [--------]          0      95000     100000     00 [--------]          0          0          0     00 [--------]
```

The output of the script looks something like:
```
{
  "version": 1,
  "timestamp": 1596732082.017182,
  "quotas": [
    {
      "block_limit": 1000,
      "file_limit": 400,
      "path": "/n/academic_homes/g_34166/u_316301_g_34166",
      "total_block_usage": 932,
      "total_file_usage": 274,
      "user": "u_316301_g_34166"
    },
    {
      "block_limit": 19922944,
      "file_limit": 95000,
      "path": "/n/academic_homes/g_73222/u_270426_g_73222",
      "total_block_usage": 0,
      "total_file_usage": 0,
      "user": "u_270426_g_73222"
    },
  ]
}


```

# How it works

This script is nice and short:

{{< highlight bash >}}
#!/usr/bin/env bash
xfs_path=/srv/export/g_34166
xfs_quota -x -c 'report -u -bir' $xfs_path | tr -s ' ' | grep u_ | jq -Rsn '
{"version": 1,
  "timestamp": now,
  "quotas":
  [inputs
    | . / "\n"
    | (.[] | select(length > 0) | . / " ") as $input
    | {"block_limit": ($input[2] | tonumber), "file_limit": ($input[7] | tonumber), "path": ("/n/academic_homes/g_" + ($input[0] | match("(.*g_?)(.*$)").captures[1].string) + "/" + $input[0]), "total_block_usage": ($input[1] | tonumber), "total_file_usage": ($input[6] | tonumber), "user": $input[0]}]}
'  > $xfs_path/share_root/.xfs_quota.json
{{</ highlight >}}

Breakdown:
1. piping into `tr -s " "` which replaces each sequence of a repeated character with a single occurrence of that character
2. piping into `grep u_` which gives only the lines with the user ids that we are interested in
3. piping into `jq` with instructions to
   - Output a json with keys for "version", "timestamp", and "quotas".
   - Set the "quotas" value to an array of objects which is populated by splitting the input by newline and then delimiting each line by spaces.

A [Stackoverflow post](https://stackoverflow.com/questions/44780761/converting-csv-to-json-in-bash) came in quite handy for getting me started.
