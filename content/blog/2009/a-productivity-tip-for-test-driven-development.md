---
title: A productivity tip for test-driven development
date: "2009-05-03"
url: /blog/2009/05/03/a-productivity-tip-for-test-driven-development/
categories:
  - Programming
---
If you code by writing tests that fail, and then fixing the tests by writing the code, then you might find yourself switching to a terminal, running the test, ad nauseum. Part 1 of my tip is to run the test in a loop that takes a single keystroke to trigger:

```
$ while read line; do clear; perl MyTestScript.t; done
```

This works with any language, not just perl---just replace the test command with the right one. ALT-TAB, press Enter, ALT-TAB back to your editor.

Part 2 of my tip is to make it really easy to drop into the debugger if you want. Notice the small change here:

```
$ while read line; do clear; perl $line MyTestScript.t; done
```

Now instead of pressing Enter, you can type "-d" and press Enter. Presto, you're in the debugger. This also works for any language that has a built-in debugger. Of course, you can also pass any other arguments you want, such as enabling profiling.


