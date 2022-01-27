---
label: Configuring Playground Time Out
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -1
---

# Vesting Example

## Configuring Playground Time Out

Lecture 3, Part 1

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=sLMhsqiWeGU&list=PLNEK_Ejlx3x2zxcfoVGARFExzOHwXFCCL&index=1)

Depending on the performance of your local machine, the playground may take such
a long time to compile and evaluate that the process times out and reports an
error. You can increase the timeout when starting the server with the following
CLI parameter.

```bash
plutus-playground-server -i 120s
```

That will set the timeout to 120 seconds, for example.
