---
layout: post
title: "Ripping Family DVDs"
---

I've been working on a fun little project lately. A few years ago, my mom had all of our old family tapes converted to DVDs, which have since sat in the basement at their house gathering dust. I decided a few months back that I should probably digitize them somehow. This is my current process.

The main tool I'm using for this is a handy program called [Handbrake](https://handbrake.fr/). In case you haven't used it before, Handbrake is free and open source software used for converting video to and from pretty much any format you'd like. Most useful for me, it offers both DVD ripping *and* a [command line interface](https://handbrake.fr/docs/en/latest/cli/command-line-reference.html)!

When ripping these DVDs, I use two different methods depending on how the chapters in each DVD are setup. If the DVD has 12 separate chapters (determined by looking at the slip of paper provided in the case) I use the CLI. If the chapters are not split into 12 distinct parts, I use the GUI.

## CLI

In my case, the CLI consists of a PowerShell program that I adapted from a StackOverflow post which, unfortunately, I can't find at the moment:

```powershell
for ($chapter=1; $chapter -le {0}; $chapter++) {
    &"{1}" --input F:\ --chapter $chapter --preset Normal --output "{2}"
}
[System.Console]::Beep(1000, 300);
[System.Console]::Beep(1000, 300);
[System.Console]::Beep(1000, 300);
cmd /c pause | out-null;
```

- {0}: number of chapters on the DVD. In this case, 12.
- {1}: location of your Handbrake .exe file. In this case, it's ```D:\Videos\Family Videos\HandBrakeCLI-1.1.2-win-x86_64\HandBrakeCLI.exe```.
- {2}: output filename. In this case, it's ```{0}_$chapter.mp4```, where {0} is the DVD number from the company that made the DVDs.

For each DVD, I just change the DVD number in this file and run it with PowerShell. Easy peasy.

When I need to use the Handbrake GUI, it's a little weirder.

## GUI

The Handbrake GUI is fantastic, but for this particular use case it's a little unwieldy. When one of the DVDs isn't split into even chapters, the process changes. Using the GUI, I have to manually split the DVD into the right-sized chunks.