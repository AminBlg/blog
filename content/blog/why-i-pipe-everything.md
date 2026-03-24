+++
title = "why i pipe everything"
date = 2026-03-18
description = "unix philosophy and the art of small tools"
[taxonomies]
tags = ["linux", "systems"]
+++

the unix pipe is the most elegant abstraction in computing and i will die on this hill. the idea that you can take the output of one program and feed it directly into another -- that programs should be small, focused, and composable -- is so powerful that we have spent fifty years failing to improve on it. every time someone builds a monolithic tool that does everything, they are reinventing a worse version of `cat | grep | sort | uniq -c | sort -rn`.

here are some pipes i actually use:

```bash
# what processes are eating my memory right now
ps aux --sort=-%mem | head -20

# find the 10 largest files in a directory tree
find . -type f -exec du -h {} + | sort -rh | head -10

# quick frequency analysis of http status codes from logs
cat access.log | awk '{print $9}' | sort | uniq -c | sort -rn

# pull down json api data and extract what i need
curl -s "https://api.github.com/users/sotch/repos" \
  | jq '.[] | {name: .name, stars: .stargazers_count}' \
  | jq -s 'sort_by(.stars) | reverse | .[:5]'

# monitor a log file and get desktop notifications on errors
tail -f /var/log/app.log | grep --line-buffered "ERROR" \
  | while read line; do notify-send "error" "$line"; done
```

the beauty is that none of these tools know about each other. `jq` does not know it is being fed by `curl`. `sort` does not care whether its input came from a file or a process or a network socket. they just read stdin and write stdout. this is the contract, and it is beautiful in its simplicity. text in, text out. everything is a stream. the composability emerges from the constraint.

modern software has largely forgotten this lesson. applications want to own the entire pipeline. they want to be the editor AND the compiler AND the debugger AND the version control AND the deployment platform. and then they wonder why everything is fragile and slow and impossible to extend. meanwhile i am over here piping `ripgrep` into `fzf` into `xargs` and solving problems in five seconds that would take a gui application three clicks and a loading spinner.

i think there is something philosophically important about building systems from small, independent parts that communicate through simple interfaces. it is how good teams work. it is how ecosystems work. the pipe is not just a unix feature. it is a design principle. and if your tool cannot participate in a pipeline, i probably do not want to use it.
