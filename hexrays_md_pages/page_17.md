# Hex-Rays Blog – Page 17

**Archived:** 2026-02-20 12:15
**URL:** https://hex-rays.com/blog/page/17

---

## 1. 

**Date:** Posted: Jan 20, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-124-scripting-examples](https://hex-rays.com/blog/igors-tip-of-the-week-124-scripting-examples)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [plugin](<https://hex-rays.com/blog/tag/plugin>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [IDC](<https://hex-rays.com/blog/tag/idc>)

# Igor’s Tip of the Week #124: Scripting examples

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-124-scripting-examples>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-124-scripting-examples>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-124-scripting-examples>)

I 

Igor Skochinsky ✦ Posted: Jan 20, 2023

![Igor’s Tip of the Week #124: Scripting examples](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Although IDA was initially created for interactive usage and tries to automate as much of the tedious parts of RE as possible, it still cannot do everything for you and doing the still necessary work manually can take a long time. To alleviate this, IDA ships with IDC and IDAPython scripting engines, which can be used for automating some repetitive tasks. But it can be difficult to know where to start, so let’s see where you can find some examples to get started.

### IDC samples

Although IDC is quite old fashioned, it has the advantage of being built-in into IDA and does not require any additional software. It is also the only scripting language available in [IDA Free](<https://www.hex-rays.com/ida-free/>). For some sample IDC scripts, see the `idc` directory in IDA’s install location:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scripting_idc-3.png?width=684&height=381&name=scripting_idc-3.png)

Please note that some of these files are not stand-alone scripts but are used by IDA for various tasks such as [customized startup actions](<https://hex-rays.com/blog/igors-tip-of-the-week-116-ida-startup-files/>) (`ida.idc`, `onload.idc`) or [batch analysis](<https://hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood/>) (`analysis.idc`).

A few user-contributed scripts are also available under the “User contributions” section in our [Download center](<https://hex-rays.com/download-center/>). Note that due to their age and the big [API refactoring](<https://hex-rays.com/products/ida/news/7_0/docs/api70_porting_guide/>) which unified IDA API and IDC, some of them may need adjustments to run in recent IDA versions.

### IDAPython examples

IDAPython project had examples from the beginning, and you can find them [in the source repository](<https://github.com/idapython/src/tree/master/examples>), but we’re also shipping them with IDA, in the `python/examples` directory.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scripting_python-3.png?width=399&height=377&name=scripting_python-3.png)

The provided `index.html` can be opened in a browser to see the list of the examples with short descriptions and also a list of used IDAPython APIs/keywords to help you find examples of a specific API’s usage.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scripting_python2-3.png?width=646&height=791&name=scripting_python2-3.png)

There are also countless examples of IDAPython scripts and plugins created by our users. Some of then can be found on our [plugin contest pages](<https://hex-rays.com/contests/>) and [plugin repository](<https://plugins.hex-rays.com/>), while even more might be found on code-sharing websites (GitHub, GitLab etc.), or individual authors’ websites and blogs. Oftentimes, searching for an API name on the Web can bring you to examples of its usage.

In addition to the examples made just for demonstration purposes, there are a few Python-based loaders and processors modules shipped with IDA. They can be found by looking for `.py` files under `loader` and `procs` directories of IDA.

---

## 2. 

**Date:** Posted: Jan 19, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-diaphora](https://hex-rays.com/blog/plugin-focus-diaphora)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: Diaphora

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-diaphora>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-diaphora>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-diaphora>)

A 

Alex Petrov ✦ Posted: Jan 19, 2023

![Plugin focus: Diaphora](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

**This is a guest entry written by Joxean Koret from Activision. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author.**

## Diaphora: The most advanced Free and Open Source Binary Diffing Tool

[Diaphora](<https://github.com/joxeankoret/diaphora>) is an Open Source IDA plugin for doing binary diffing (usually called bindiffing, for short). In a nutshell, binary diffing is a reverse engineering technique used to find either the similarities or the differences between various pieces of software, in binary form. The technique was most likely invented by Thomas Dullien (Halvar Flake), author of the very first publicly available bindiffing tool called BinDiff.

I published this Open Source project, **Diaphora** , in 2015 and I have been testing and updating for every single minor IDA version since these times, which means from version 6.6 to the current 8.2 (as of the time of writing this blog post).

In this blog post I will discuss, briefly, how **Diaphora** works and, more in depth, show example usages. Let’s start…

### Introduction

This is how bindiffing works, in general: Two or more binaries are analysed and features about each function found in the binary are extracted. Then, the extracted functions are matched, using a set of heuristics, and compared, using some comparison function, to determine how close or different they are.

This is a brief list of some example `features` that can be extracted from functions:

  * A "hash" for the CFG (Control Flow Graph).
  * The literal constants (strings, non common numbers).
  * The RVA (Relative Virtual Address) and size in bytes.



Some example heuristics used by **Diaphora** to compare 2 functions can be the following:

  * The functions are big enough and their MD5 are the same.
  * The functions are big enough and the flow graphs are the same.
  * The pseudo-code for both big enough functions are the same.



### The tool’s GUI

When we execute the `diaphora.py `script it shows the following dialogue:

![Diaphora's GUI](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/diaphora-gui-3.png?width=1000&height=612&name=diaphora-gui-3.png) Diaphora’s GUI 

Here we can select the SQLite database to export the current database, a secondary SQLite database previously exported to diff against it, the memory addresses to limit what should be exported, as well as enable or disable many different options, like if we want to use the decompiler, what do we want to export, which heuristics we want to use, what do we want to exclude, etc.

### How Diaphora works

**Diaphora** , as pretty much any other binary diffing tool, works this way:

  * First, we export the databases (the binaries) that we want to compare.
  * Then, we diff both generated databases to find matches between them.
  * Optionally, we can import matches from one binary to another.



In short, we have to export our binaries, then diff the binaries and, optionally, import everything from one database to the other.

###  Binary diffing use cases

The most common binary diffing use cases (ie, why reverse engineers use bindiffing tools for our day-day to job) are the following ones:

  * Patch diffing. A binary or set of binaries have been patched (for example, to fix some vulnerability) and reverse engineers diff both binaries in order to see which changes were made in the old, unpatched, version comparing it with the latest version.
  * Porting work. A reverse engineer works in some binary version for some time and, later on, the vendor publishes a new version of the binary. The reverse engineer diffs the old and new versions to see what has been modified, added, removed, etc… and also to port their work: comments, function names, enums, structs, etc…
  * Importing symbols from static libraries. A reverse engineer starts their work with some binary and notices it uses some specific Open Source library that is statically linked in the binary. The reverse engineer compiles or downloads a binary version of the Open Source library with full symbols, diffs against the binary that embeds this library and then imports the matches so they don’t need to waste their time reverse engineering Open Source software, and also gets enums, structs, function names and prototypes imported in the binary.



### Example usages

Let’s see examples with Diaphora of some of the previously mentioned potential use-cases:

  * Patch diffing. We will diff CVE-2020-0674, a vulnerability patched in the Microsoft’s JScript engine.
  * Importing symbols from static libraries. We will compile the source code of one version of the SQLite3 engine, diff against a binary embedding it (`sqldiff` from some Ubuntu version) and then import the matches.



####  Patch Diffing CVE-2020-0674

According to Mitre, CVE-2020-0674 was a remote code execution vulnerability in the way that the scripting engine handled objects in memory in Internet Explorer, aka ‘Scripting Engine Memory Corruption Vulnerability’. We will work with 2 `jscript.dll` binaries with the following SHA256 hashes:

  * 408cb1604d003f38715833a48485b6a4e620edf163fb59aef792595866e4796b
  * c115d15807b96dcb9871ebc69618ef77473f1451c427e7349f9aa3c72891ddc2



Now, let’s load in IDA the first binary (408cb1604d003f38715833a48485b6a4e620edf163fb59aef792595866e4796b), let the auto-analysis finish and then run `diaphora.py` from within IDA, leave all options by default and click "OK"; Diaphora will start to export all functions, structs, enums, comments, etc… from the binary and store it in one SQLite database (which will be named by default `408cb1604d003f38715833a48485b6a4e620edf163fb59aef792595866e4796b.sqlite`).

![Diaphora exporting](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/diaphora-export-3.png?width=594&height=169&name=diaphora-export-3.png) Diaphora exporting 

When Diaphora finishes exporting, close the database and open the next binary, c115d15807b96dcb9871ebc69618ef77473f1451c427e7349f9aa3c72891ddc2. As before, first let IDA perform initial auto-analysis, run again the script `diaphora.py` and, this time, select in the 2 field shown in the dialogue the previous SQLite database that we exported (remember that Diaphora works with SQLite database, not directly with IDA databases) as shown in the picture below:

![Diaphora diff against dialogue](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/diaphora-diff-against-3.png?width=1682&height=1037&name=diaphora-diff-against-3.png) Diaphora diff against dialogue 

And, then, leave everything by default and press "OK". Diaphora will export the current binary and as soon as it finishes doing so it will start the diffing process. It will show a dialogue that will be updated from time to time telling us which heuristic is being executed:

![Diaphora running heuristics](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/diaphora-diffing-3.png?width=642&height=145&name=diaphora-diffing-3.png) Diaphora running heuristics 

After a while, Diaphora will finish finding matches and then it will show a set of choosers (IDA dockable list windows) showing the "Best", "Partial" and "Unmatched" functions:

![Diffing results tabs](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/diaphora-diff-results-3.png?width=2562&height=540&name=diaphora-diff-results-3.png) Diffing results tabs 

In the "Best matches" tab we have all the functions that Diaphora matched and found no relevant change whatsoever. In the "Partial matches" tab we have all the functions the Diaphora matched but changes were made between the 2 binaries. There are also 2 other tabs: "Unmatched in primary" and "Unmatched in secondary". These tabs show those remaining functions from both binaries for which Diaphora found no appropriate match.

We will focus on the "Partial matches" tab, which is the one that shows us what was changed between the 2 binaries. Let’s select the function `GcAlloc::SetMark` and then right click over it and select from the popup menu the option "Diff pseudo-code":

![Diffing `GcAlloc::SetMark`](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/diff-gcalloc-setmark-pseudo-3.png?width=2314&height=851&name=diff-gcalloc-setmark-pseudo-3.png) Diffing `GcAlloc::SetMark`

As we can see here only a single character seems to be changed: instead of checking if `GcContext::IsLegacyGCEnabled()` returns true it now does the opposite. It seems that with this patch they are deprecating the "legacy garbage collector" feature. We can also diff the assembly if we want by doing right click over the the select function match and then selecting "Diff assembly" from the popup menu; it will show the following:

![Jz changed to Jnz](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/flip-jz-jnz-3.png?width=2306&height=604&name=flip-jz-jnz-3.png) Jz changed to Jnz 

As expected, a conditional jump was changed (from `jz` to `jnz`). Now, let’s take a look to another function in the partial matches set, `GcContext::InitIsLegacyGCEnabled()`. This time, instead of choosing to diff assembly or pseudo-code we will select from the popup menu the option "Show pseudo-code patch":

![Registry key changed](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/registry-diff-patch-3.png?width=2319&height=417&name=registry-diff-patch-3.png) Registry key changed 

As we can see, Microsoft changed some registry key to enable/disable the legacy garbage collector in the JScript engine. If we were interested in just how Microsoft mitigated or disabled this feature, we are done. The patch is a bit more complex than that and it involves how JScript variables are "scavenged", but it’s out of the scope of this blog post showing how to use Diaphora.

### Importing symbols from static libraries

Let’s see another common usage of binary diffing tools: importing symbols (function names, enums and structs) from Open Source libraries that were statically linked into some binary. For this example (and for legal reasons) we will use the following binaries:

  * Our own compiled version of the SQLite3 engine with symbols.
  * One `sqldiff` binary from Ubuntu that was statically linked with `sqlite3.c`.



We will start by compiling SQLite3: download the sources amalgamation from [their website](<https://sqlite.org>), and simply compile it like this:
    
    
      $ gcc -O2 shell.c sqlite3.c -g -o sqlite3

Then, open the resulting binary in IDA, let the auto-analysis finish and when it’s done run the script `diaphora.py`, leave all options by default and press OK. It will take sometime because SQLite3 is a big project, even when it’s an embeddable engine. When Diaphora finishes exporting everything from the `sqlite3` binary to the corresponding `sqlite3.sqlite` Diaphora database, close the binary and now load `sqldiff`.

![The sqldiff binary before importing anything](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sqldiff-before-3.png?width=3839&height=1542&name=sqldiff-before-3.png) The sqldiff binary before importing anything 

As always, let’s IDA finish its initial work and when it’s done run again `diaphora.py`, and in the 2nd field select the `sqlite3.sqlite` database that we exported before and just press "OK". After some time, 5 minutes in my testing machine, it will finish exporting & diffing and will show the list of matched in chooser windows (or tabs, if you prefer).

![Results diffing sqlite3 against sqldiff](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sqlite3-to-sqldiff-3.png?width=3069&height=1580&name=sqlite3-to-sqldiff-3.png) Results diffing sqlite3 against sqldiff 

In this example we have 237 functions that were matched by Diaphora with a similarity ratio of 1.0, which means that these functions were not changed. If we go to the partial matches we will see that we have almost ~1,000 functions matched:

![Partial results between sqlite3 and sqldiff](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sqlite3-to-sqldiff-partial-3.png?width=2052&height=1582&name=sqlite3-to-sqldiff-partial-3.png) Partial results between sqlite3 and sqldiff 

OK, so we have some (initial) good results, let’s start importing matches so we can, later on, work on the `sqldiff` binary without having to reverse engineer whatever was in `sqlite3.c`: go to the "Best matches" tab, right click on the chooser and select from the popup menu the option "Import all data for sub_* functions" (this option will import everything that is in the `sqlite3` binary for function matches starting with the IDA’s auto-generated prefix "sub_"). When asked by Diaphora with the following dialogue just press "Yes":

![Import dialogue](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/really-import-3.png?width=1105&height=175&name=really-import-3.png) Import dialogue 

Diaphora will start importing structs, enums, comments in the pseudo-code and assembly (if any), type libraries, etc… and after some time it will show something like the following:

![Initial import matches](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/initial-import-matches-types-3.png?width=3838&height=1577&name=initial-import-matches-types-3.png) Initial import matches 

We have some initial matches and local types to start working on, however, we can make it even better by importing some (or all) the partial matches results so, let’s to this tab and select all results that have, at least, a similarity ratio of 0.600 (the value from which I have no doubts taking a brief look to some matches that they are reliable ones) and then right click on the chooser and select from the popup menu the option "Import selected sub_*":

![Import selected sub_*](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/import-selected-sub-3.png?width=1998&height=1676&name=import-selected-sub-3.png) Import selected sub_* 

When asked if we want to import press "Yes" and it will start importing everything related to the new functions (like global variables referenced by them, function comments, prototypes and names) and, after a while, it will finish doing so. And now, as you can see in the picture below, we will have many more functions renamed in our IDA database and we could start working already on this binary without having to waste our time reverse engineering the embedded SQLite3 database:

![New functions](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/new-functions-decompiled-with-structs-3.png?width=2526&height=1584&name=new-functions-decompiled-with-structs-3.png) New functions 

##  Final notes

In this guest post we have shown only the tip of the iceberg, just some of the most basic features of Diaphora, and we excluded many other features, like scripting, automation, adding user-defined heuristics, etc… because otherwise it would be a too big blog post but, hopefully what we described will help you in your current and future reverse engineering projects and, if you have any question or doubt, you can contact the Diaphora author, Joxean Koret, by opening an issue in github or sending an e-mail to [admin @joxeakoret.com](<mailto:admin%20@joxeakoret.com>).

Happy reversing!

PS: The screenshots in this blog post were taken with a currently not yet published development version of Diaphora (what will be the 3.0 version of this project). However, basically everything but the number of function matches should be the same as one could get using the current public version of Diaphora.

---

## 3. 

**Date:** Posted: Jan 13, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-123-opcode-bytes](https://hex-rays.com/blog/igors-tip-of-the-week-123-opcode-bytes)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #123: Opcode bytes

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-123-opcode-bytes>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-123-opcode-bytes>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-123-opcode-bytes>)

I 

Igor Skochinsky ✦ Posted: Jan 13, 2023

![Igor’s Tip of the Week #123: Opcode bytes](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When disassembling, you are probably more interested in seeing the code (disassembly or pseudocode) rather than the raw file data, but there may be times you need to see what actually lies behind the instructions.

One option is to use [the Hex View](<https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view/>), possibly docked and synchronized with IDA View.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_highlight2-Jun-18-2024-08-50-27-0725-AM.png?width=745&height=87&name=hexview_highlight2-Jun-18-2024-08-50-27-0725-AM.png)

But probably a simpler solution is the [disassembly option](<https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options/>) _Number of opcode bytes_.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/opcode_bytes1-3.png?width=584&height=511&name=opcode_bytes1-3.png)

By setting it to a non-zero value, IDA will use the specified number of columns to display the bytes of the instructions at the start of the disassembly line.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/opcode_bytes2-3.png?width=649&height=283&name=opcode_bytes2-3.png)

If the instruction is longer than the specified number of bytes, extra lines will be used to display the remainder of the opcode:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/opcode_bytes3-3.png?width=825&height=441&name=opcode_bytes3-3.png)

If you prefer to have IDA simply truncate the long opcodes instead of using extra lines, specify a negative value (e.g. -4).

### Showing opcode bytes by default

If you prefer to always see opcode bytes, you can use the `OPCODE_BYTES` setting in `ida.cfg` (either the one in your IDA install, or the override in [user directory](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>)). This enables opcode bytes in the text view only; for the graph view use the setting `GRAPH_OPCODE_BYTES`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/opcode_bytes4-3.png?width=1046&height=533&name=opcode_bytes4-3.png)

Another possibility is set up the opcode bytes (and other disassembly options) as you like and save the current [desktop layout as default](<https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts/>); it will be used for all new databases.

See also:

[IDA Help: Text Representation Dialog](<https://www.hex-rays.com/products/ida/support/idadoc/605.shtml>)

[Igor’s tip of the week #38: Hex view – Hex Rays](<https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view/>)

[Igor’s tip of the week #25: Disassembly options](<https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options/>)

[Igor’s tip of the week #22: IDA desktop layouts](<https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts/>)

---

## 4. 

**Date:** Posted: Jan 6, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load](https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #122: Manual load

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load>)

I 

Igor Skochinsky ✦ Posted: Jan 6, 2023

![Igor’s Tip of the Week #122: Manual load](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

To save on analysis time and database size, by default IDA only tries to load relevant parts of the binary (e.g. those that are expected or known to contain code). However, there may be cases when you want to see more, or even everything the binary contains. You can always load the file as plain binary and mark it up manually, using IDA as a sort of a hybrid hex editor, but this way you lose the features handled by the built-in loaders such as names from the symbol table, automatic function boundaries from the file metadata and so on. So it may be interesting to have more granular control over the file loading process.

To support such scenarios, IDA offers the _Manual load_ checkbox in the initial load dialog.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/manual_load1-3.png?width=968&height=548&name=manual_load1-3.png)

What happens when the option is checked depends on the loader. For example, the PE loader may allow you to pick another load base (image base), choose which sections to load, and whether to parse some optional metadata which could, for example, be corrupted and result in bad analysis.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/manual_load2-3.png?width=798&height=395&name=manual_load2-3.png)

The ELF loader behaves in a similar manner

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/manual_load3-3.png?width=747&height=395&name=manual_load3-3.png)

If you want IDA to always load all PE sections, you can edit `cfg/pe.cfg` and set the option `PE_LOAD_ALL_SECTIONS`:
    
    
    // Always load all sections of a PE file?
    // If no, sections like .reloc and .rsrc are skipped
    
    PE_LOAD_ALL_SECTIONS = YES
    

See also:

[IDA Help: Load file dialog](<https://www.hex-rays.com/products/ida/support/idadoc/242.shtml>)

---

## 5. 

**Date:** Posted: Jan 5, 2023  
**Link:** [https://hex-rays.com/blog/ida-8-2-qt-5-15-2-sources-build-scripts](https://hex-rays.com/blog/ida-8-2-qt-5-15-2-sources-build-scripts)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.2: Qt 5.15.2 sources & build scripts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-2-qt-5-15-2-sources-build-scripts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-2-qt-5-15-2-sources-build-scripts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-2-qt-5-15-2-sources-build-scripts>)

Arnaud Diederen ✦ Posted: Jan 5, 2023

![IDA 8.2: Qt 5.15.2 sources & build scripts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

A handful of our users have already requested information regarding the Qt 5.15.2 build, that is shipped with IDA 8.2.

The Qt sources used by IDA are:

  * based on Qt 5.15.2,
  * to which [the KDE Qt5 patch collection](<https://community.kde.org/Qt5PatchCollection>) has been added,
  * plus a few custom patches/fixes



### Rebuilding Qt from source

In order to obtain compatible libs, the simplest way forward is to

  * [download the full archive](<https://hex-rays.com/products/ida/support/freefiles/qt-5.15.2-full-IDA82.tar.bz2>) – it contains the original 5.15.2 source + KDE patches + Hex-Rays patches
  * go to the `build/` directory
  * type `python3 build.py -h`



Note: on Windows, that command should be issued from a Visual Studio 2019 command prompt.

---

## 6. 

**Date:** Posted: Dec 30, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-121-limiting-search-to-an-address-range](https://hex-rays.com/blog/igors-tip-of-the-week-121-limiting-search-to-an-address-range)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #121: Limiting search to an address range

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-121-limiting-search-to-an-address-range>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-121-limiting-search-to-an-address-range>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-121-limiting-search-to-an-address-range>)

I 

Igor Skochinsky ✦ Posted: Dec 30, 2022

![Igor’s Tip of the Week #121: Limiting search to an address range](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

When performing a [search](<https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida/>) in IDA, it by default starts from the current position and continues up to the maximum address in the database (or to the minimal for searches “Up”). This works well enough for small to average files, but can get pretty slow for big ones, or especially in case of debugging where the database may include not just the input file but also multiple additional modules loaded at runtime.

To skip areas you’re not interested in and improve the speed, you can limit the search to an address range. For this, IDA relies on selection. For example, consider this disassembly snippet:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_range1-3.png?width=720&height=286&name=search_range1-3.png)

If you perform a binary search for the value `93`, the instruction at `00000514` will be found:
    
    
    Searching down CASE-INSENSITIVELY for binary pattern:
    	93
    Search completed. Found at 00000514.

However, if you select a range which does not include that address before invoking the search, the search will fail:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_range2-3.png?width=821&height=281&name=search_range2-3.png)
    
    
    Searching down CASE-INSENSITIVELY for binary pattern:
    	93
    Search failed.
    Command "AskBinaryText" failed

Selecting large areas with the mouse or by holding Shift can be quite tedious, so it may be more convenient to use the [anchor selection](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>):

  1. Move to the start or end of the intended selection and invoke Edit > Begin selection (or press `Alt`–`L` ).
  2. Navigate to the other end of the selection using any means (cursor keys, Jump actions, Functions or Sgments window, Navigation bar etc.).
  3. Invoke the binary search command. The search will be performed in the selection only.

---

## 7. 

**Date:** Posted: Dec 26, 2022  
**Link:** [https://hex-rays.com/blog/ui-candy](https://hex-rays.com/blog/ui-candy)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [IDA](<https://hex-rays.com/blog/tag/ida>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [IDA 8.2](<https://hex-rays.com/blog/tag/ida-8-2>)

# Styling IDA listings background with CSS

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ui-candy>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ui-candy>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ui-candy>)

A 

Alex Petrov ✦ Posted: Dec 26, 2022

![Styling IDA listings background with CSS](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

For most IDA widgets, a custom background was already possible using standard Qt stylesheets ([examples](<https://doc.qt.io/qt-5/stylesheet-examples.html>), [reference](<https://doc.qt.io/qt-5/stylesheet-reference.html>)). But since [the IDA 8.2 release](<https://hex-rays.com/products/ida/news/8_2/>) you can also do it for disassembly listings! (and “Structures”, “Enums”, “Pseudocode”, …)

To achieve this, you would typically want to define a new theme that extends an existing one and adds only the necessary CSS code.

# IDA themes

If you inspect a fresh IDA install, you will notice the following folder structure in IDA’s installation directory:
    
    
    themes/  
    ├── _base  
    │   └── theme.css  
    ├── darcula  
    │   └── theme.css  
    ├── dark  
    │   ├── icons  
    │   │   ├── expand.png  
    │   │   └── spacer.png  
    │   └── theme.css  
    └── default      
        └── theme.css  

Each folder in there represents a theme that can be selected through `Options > Colors…`.

In order to add a new theme, you have 2 options:

  1. Add it in the above IDA installation directory, or
  2. Add it to the user’s [`$IDAUSR`](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>)



Picking option #1 will make the theme available for all users on the machine but might require admin rights. Moreover, the theme will not be picked up by future versions of IDA if you install those in other folders.

That’s why we recommend using option #2, and will be storing our theme resources there.

# Defining a new theme

We will extend the existing “default” theme only to add a floral background to listings.

On a Linux machine, where the default value of `$IDAUSR` is `~/.idapro/`, this would typically look like this:
    
    
    $ tree ~/.idapro/themes/  
    /home/<user>/.idapro/themes/  
    └── flowers      
    	├── flowers.png      
    	└── theme.css  

where `flowers.png` is a variation of the file available [here](<https://www.publicdomainpictures.net/en/view-image.php?image=466419&picture=watercolor-floral-vintage-art>), and `theme.css` contains:
    
    
    $ cat ~/.idapro/themes/flowers/theme.css  
    @importtheme "default";    
    
    CustomIDAMemo  
    {      
    	qproperty-line-bg-default: rgba(255, 255, 255, 0);
    	background: white url("$RELPATH/flowers.png");
    	background-attachment: fixed;      
    	background-repeat: none;      
    	background-position: bottom right;  
    }    
    
    ecfringe_t  
    {      
    	background: transparent;  
    }    
    
    TextArrows  
    {      
    	background: transparent;  
    }    
    
    IDAViewHost  
    {      
    	background: white;  
    }  

With this in place, the user can now pick the `flowers` theme from IDA’s `Options > Colors…` dialog and enjoy working with the new theme:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/customized-3.png?width=1030&height=730&name=customized-3.png)

# Resources

The image and CSS resources presented in this blog post can be downloaded [here](<https://hex-rays.com/wp-content/uploads/2022/12/flowers.zip>). Enjoy!

Take a look at one of our past tutorials on [ CSS-based styling ](<https://hex-rays.com/products/ida/support/tutorials/themes/>)

---

## 8. 

**Date:** Posted: Dec 23, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-120-set-call-type](https://hex-rays.com/blog/igors-tip-of-the-week-120-set-call-type)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #120: Set call type

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-120-set-call-type>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-120-set-call-type>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-120-set-call-type>)

I 

Igor Skochinsky ✦ Posted: Dec 23, 2022

![Igor’s Tip of the Week #120: Set call type](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Previously we’ve described how to use available type info to make decompilation of calls more precise [when you have type information](<https://hex-rays.com/blog/igors-tip-of-the-week-119-force-call-type/>), but there may be situations where you don’t have it or the existing type info does not quite match the actual call arguments, and you still want to adjust the decompiler’s guess.

One common example is variadic functions (e.g. `printf`, `scanf` and several others from the C runtime library, as well as custom functions specific to the binary being analyzed). The decompiler knows about the standard C functions and tries to analyze the format string to guess the actually passed arguments. However, such guessing can still fail and show wrong arguments being passed.

For simple situations, [adjusting variadic arguments](<https://hex-rays.com/blog/igors-tip-of-the-week-101-decompiling-variadic-function-calls/>) may work, but it’s not always enough. For example, some calling conventions pass floating-point data in different registers from integers, so the decompiler needs to know which arguments are floating-point and which are not. You can, of course, change the prototype of the function to make the additional arguments explicit instead of variadic, but this affects all call sites instead of just the one you need.

Another difficulty can arise when dealing with the `scanf` family functions. Because the variadic arguments to such functions are usually passed by address, any variable type may be used for a specific format specifier. Consider the following example source code:
    
    
    struct D
    {
      int d;
      int e;
    };
    
    
    #include 
    int main()
    {
     D d;
     scanf("%d", &d.d);
    }

When we decompile the compiled binary, even after creating the struct and changing the local variable type, the following output is shown:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calltype1-3.png?width=579&height=133&name=calltype1-3.png)

We get `&d` instead of `&d.d` because `d` is situated at the very start of the structure so both expressions are equivalent on the binary level. To get the desired expression, we need to hint the decompiler that the extra argument is actually an `int *`. This can be done using the “Set call type…” action from the context menu on the call site:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calltype2-3.png?width=307&height=338&name=calltype2-3.png)

We can explicitly specify type of the extra argument:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calltype3-3.png?width=694&height=99&name=calltype3-3.png)

The decompiler takes it into account and uses the proper expression to match the new prototype:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calltype4-3.png?width=577&height=134&name=calltype4-3.png)

See also: [Hex-Rays interactive operation: Set call type](<https://www.hex-rays.com/products/decompiler/manual/cmd_set_call_type.shtml>)

---

## 9. 

**Date:** Posted: Dec 22, 2022  
**Link:** [https://hex-rays.com/blog/plugin-focus-ipyida](https://hex-rays.com/blog/plugin-focus-ipyida)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: IPyIDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-ipyida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-ipyida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-ipyida>)

A 

Alex Petrov ✦ Posted: Dec 22, 2022

![Plugin focus: IPyIDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

**This is a guest entry written by Marc-Étienne Léveillé. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author.**

## IPyIDA – a better console for IDA Pro using IPython and Jupyter Notebook

Unlike most plugins, [IPyIDA](<https://github.com/eset/ipyida>) is not meant to solve a reverse engineering problem but to help you solve reverse engineering problems. IPyIDA integrates the power of [Jupyter‘s](<https://jupyter.org/>) [IPython](<https://ipython.org/>) console into IDA Pro, making prototyping and Python plugin and script development more friendly. A subgoal is also to support all environments and make it as easy as possible for users to install and use IPyIDA. After it’s installed using a few clicks and keystrokes, `Shift-. ` opens the IPython console right into IDA Pro, regardless of your version of IDA Pro, Python or operating system.

Eight years ago, we released the first version of IPyIDA, which was the first cross-platform, Python-only solution to integrate IPython inside IDA Pro. As time passed, we fixed bugs along the way as they were reported, and added support for newer versions of IDA Pro as they were released. Today, we finally release version 2.0, packed with new features, such as support for [Jupyter Notebook](<https://jupyter-notebook.readthedocs.io/en/latest/>), better display of pointers and byte strings, and the ability to click on addresses as in the built-in IDA console.

## Some history

To be fair, we are not the first to solve part of this problem. There was (and still is) an existing project with a similar goal: [IDA IPython](<https://github.com/james91b/ida_ipython>). However, not everyone is using IDA Pro on Windows, which was the only platform supported by IDA IPython at the time we started working on IPyIDA. Furthermore, it wasn’t easy to install and required users to compile the plugin themselves and manage dependencies. We figured there might be a better way and having solved the various problems for ourselves, why not share it with everyone else?

We don’t see ourselves as builders, but rather as plumbers connecting existing projects and making them talk to each other. We’re not saying this is without pain: the parts we used were not necessarily meant to work together when they were originally built and they make assumptions that must be “fixed” by our plumbing.

## The basics

**Installing IPyIDA**

IPyIDA provides a [script](<https://github.com/eset/ipyida/blob/stable/install_from_ida.py>) to install IPyIDA and its dependencies. We could write step-by-step instructions but why not make a script if the instructions are simply a bunch of commands to run one after another? The script also takes care of some edge cases to support different environments. The script is commented so anyone can read it and use it as instructions if they prefer to turn off the autopilot and fine-tune what’s required to run IPyIDA.

IPyIDA is distributed as a Python [package on PyPI](<https://pypi.org/project/ipyida/>) and sources are on [ESET’s GitHub](<https://github.com/eset/ipyida/>).

Although not mandatory, it is recommended to set up a virtual environment to run IDA Python. We provide instructions for creating and activating such environment in [README.virtualenv.adoc](<https://github.com/eset/ipyida/blob/master/README.virtualenv.adoc>).

**Using IPyIDA**

After IPyIDA is loaded,`Shift-.` opens an IPython console where you have access to all the IDA Python modules. None of them are imported into the default namespace; you’ll need to import them first. To have anything loaded in the console when it is started, you can create a ` /ipyidarc.py `file, which is loaded by IPyIDA.

All IPython features should work, including [tab-completion](<https://ipython.readthedocs.io/en/stable/interactive/tutorial.html#tab-completion>), [online help](<https://ipython.readthedocs.io/en/stable/interactive/tutorial.html#exploring-your-objects>), and even [graphics](<https://ipython.readthedocs.io/en/stable/interactive/plotting.html#rich-outputs>) and [plots](<https://nbviewer.org/github/ipython/ipython/blob/1.x/examples/notebooks/Part%203%20-%20Plotting%20with%20Matplotlib.ipynb>).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image2-Jun-18-2024-08-54-06-3963-AM.png?width=1094&height=716&name=image2-Jun-18-2024-08-54-06-3963-AM.png) ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image3-Jun-18-2024-08-54-03-6541-AM.png?width=931&height=1047&name=image3-Jun-18-2024-08-54-03-6541-AM.png)

There are two independent components to IPyIDA: the Jupyter kernel, running inside IDA Python, and the QtConsole, which fits nicely inside a regular IDA Pro panel. The latter simply acts as a frontend that connects to the kernel. This means you may also use other frontends to connect to the kernel from outside IDA Pro, such as a Jupyer Console inside a terminal session, using the `jupyter console --existing` command.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image7-Jun-18-2024-08-54-04-8897-AM.png?width=2134&height=2100&name=image7-Jun-18-2024-08-54-04-8897-AM.png)

Here are a few examples where IPyIDA is useful while working with IDA Python:

  * Pressing tab at any moment will try to complete what is being typed or suggest possible variable, function, method, or constant names. If [Jedi](<https://jedi.readthedocs.io/>) is installed, tab-completion is even more powerful.
  * Ending the line with a question mark (`?`) will show the documentation for the prompted object. In the screenshot above, `sark.Function?` displays the docstring and the prototype for the class constructor. Using two question marks (`??`) additionally shows the Python source code as well.
  * Using Qt’s [QWidget::grab()](<https://doc.qt.io/qt-6/qwidget.html#grab>) method, it is possible to create a screenshot of any IDA Pro panel. The example above finds the “Graph overview” panel using [find_widget](<https://www.hex-rays.com/products/ida/support/idapython_docs/ida_kernwin.html#ida_kernwin.find_widget>) to display the control flow graph. [Pillow](<https://pillow.readthedocs.io/>) is required to convert Qt’s bitmap to a format compatible with the Jupyter console.
  * It is possible the invoke external shell commands by starting the input with an exclamation mark (`!`). In the example above, `pip` is invoked to install [Sark](<https://github.com/tmr232/Sark>) before using it.
  * And many more! Jupyter has a lot to offer and a large community of users. IPyIDA is just a tool to bring all these features to simplify your workflow.



## New features in IPyIDA 2.0

**Jupyter Notebook, finally!**

Adding support for Jupyter Notebook was the most requested feature, both from colleagues at ESET and from users who reached out to us online.

The existing solutions required starting IDA Pro from a Notebook, which didn’t seem intuitive and not exactly how we wanted things to work. Jupyter Notebook is designed to manage the life cycle of a kernel, from its creation to termination. There is no built-in way to connect to an existing Jupyter kernel. This happens to be a problem for [all Jupyter kernels embedded in applications](<https://github.com/ipython/ipython/issues/4066>), that would like to be able to create notebooks that connects back to them.

Our solution to this problem was to create a proxy kernel, which Jupyter Notebook can create and destroy at will. This proxy simply connects back to our embedded kernel, or any other running kernel, and proxies all messages. More plumbing!

Since this solution should work not only for IPyIDA but also for other embedded kernels, we have released this proxy kernel as an independent Python package: [jupyter-kernel-proxy](<https://pypi.org/project/jupyter-kernel-proxy/>).

Using this proxy, it is possible to create a new Jupyter Notebook from the IPyIDA console using the `%open_notebook` magic command. IPyIDA takes care of installing dependencies and starting a server if necessary, then opens a browser to a new `.ipynb` file right where your `.idb` is. This session should be connected to the kernel running in IDA.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image4-loop-3.gif?width=800&height=519&name=image4-loop-3.gif)

Note that session history is not transferred to the notebook: the animated GIF above opens an existing notebook that is already filled with content.

Enjoy sharing Notebook files created from IDA Pro! The notebook we created for this article is available here: <https://gist.github.com/eset-research/5f6ca054e887cea61eb4b5e049134ccb>.

**Other new things**

Playing with byte arrays is something fairly common when extracting parts of an analyzed binary file. We made it more convenient to see their value by displaying a hex dump when the array doesn’t contain ASCII-only content.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image5-Jun-18-2024-08-54-01-9376-AM.png?width=937&height=1069&name=image5-Jun-18-2024-08-54-01-9376-AM.png)

It is quite common that addresses are in the input or output of Python commands. In these cases, IPyIDA now displays the more meaningful symbol name and offset next to such pointers.

Furthermore, IPyIDA now supports Ctrl-clicking (Cmd-click on macOS) to jump to the address in the disassembly view. This behavior is similar to double-clicking on an address or variable names in IDA Pro’s built-in console.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image6-3.gif?width=792&height=670&name=image6-3.gif)

We hope you’ll embrace IPyIDA and feel free to contribute and report (or fix!) any issue you encounter. We’ll do our best to get back to you in a timely manner. Happy reversing and Python scripting in IDA!

---

