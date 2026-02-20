# Hex-Rays Blog – Page 16

**Archived:** 2026-02-20 12:14
**URL:** https://hex-rays.com/blog/page/16

---

## 1. 

**Date:** Posted: Mar 3, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-130-source-line-numbers](https://hex-rays.com/blog/igors-tip-of-the-week-130-source-line-numbers)

[Back](<https://hex-rays.com/blog>)

[idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #130: Source line numbers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-130-source-line-numbers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-130-source-line-numbers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-130-source-line-numbers>)

I 

Igor Skochinsky ✦ Posted: Mar 3, 2023

![Igor’s Tip of the Week #130: Source line numbers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Debug information, whether present in the binary or [loaded separately](<https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols/>), can contain not only symbols such as function or variable names, but also mapping of binary’s instructions to the original source files. It can be used by IDA’s debugger for [source-level debugging](<https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging/>), but what if you want to see this mapping during static analysis?

### Enabling source line number display

Assuming the line number info was available and has been imported, it can be enabled in the Options > General… dialog, Disassembly tab:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srclin1-3.png?width=781&height=706&name=srclin1-3.png)

Once enabled, IDA will add automatic comments with the file name and line number in the disassembly listing:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srclin2-3.png?width=1121&height=940&name=srclin2-3.png)

To enable this for all new databases by default, change `SHOW_SOURCE_LINNUM` setting in `ida.cfg`.

### Importing line numbers from DWARF

DWARF debug format can also include line number information, but by default it’s skipped because it’s rarely needed in the database itself and can take a long time to load for big files. If you do need it, you should enable the corresponding option when prompted by IDA:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srclin3-3.png?width=339&height=522&name=srclin3-3.png)

To always import line numbers from DWARF debug info, enable `DWARF_IMPORT_LNNUMS` in `cfg/dwarf.cfg`.

See also:

[Igor’s tip of the week #55: Using debug symbols](<https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols/>)

[Igor’s tip of the week #85: Source-level debugging](<https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging/>)

---

## 2. 

**Date:** Posted: Feb 24, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-129-searching-for-text-in-database](https://hex-rays.com/blog/igors-tip-of-the-week-129-searching-for-text-in-database)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #129: Searching for text in database

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-129-searching-for-text-in-database>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-129-searching-for-text-in-database>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-129-searching-for-text-in-database>)

I 

Igor Skochinsky ✦ Posted: Feb 24, 2023

![Igor’s Tip of the Week #129: Searching for text in database](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Using the [string list](<https://hex-rays.com/blog/igors-tip-of-the-week-128-strings-list/>) is one way to look for text in the binary but it has its downsides: building the list takes time for big binaries, some strings may be missing initially so you may need several tries to get the options right, and then you need to actually find what you need in the list.

If you already know the text you want to find (e.g. from the output of the program), there is a quicker way.

### Using binary search for text

The binary search action can be invoked via Search > Sequence of bytes… menu, or the `Alt`–`B` shortcut. Although its primary use is for binding known byte sequences, you can also use it for finding text embedded in the binary. For this, surround the text string with double quotes (`"`). The closing quote is optional.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/txtsearch1-3.png?width=515&height=346&name=txtsearch1-3.png)

Once a quote is present in the input box, the _String encoding_ dropdown is enabled. It allows you to choose in which [encoding](<https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings/>)(s) to look for the string.

After confirming, IDA will print in the Output window the exact byte patterns it’s looking for:
    
    
    Searching down CASE-INSENSITIVELY for binary patterns:
    UTF-8: 4A 61 6E
    UTF-16LE: 4A 00 61 00 6E 00
    UTF-32LE: 4A 00 00 00 61 00 00 00 6E 00 00 00
    Search completed. Found at 1001A9C4.

You can also mix string literals and byte values. For example, to find “Jan” but not “January”, add `0` for the C string terminator:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/txtsearch3-3.png?width=1082&height=489&name=txtsearch3-3.png)

To continue the search, use Search > Next sequence of bytes…, or shortcut `Ctrl`–`B`.

See also:

[Igor’s tip of the week #48: Searching in IDA](<https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida/>)

[IDA Help: Search for substring in the file](<https://www.hex-rays.com/products/ida/support/idadoc/579.shtml>)

[IDA Help: Binary string format](<https://www.hex-rays.com/products/ida/support/idadoc/528.shtml>)

---

## 3. 

**Date:** Posted: Feb 21, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-capa-explorer](https://hex-rays.com/blog/plugin-focus-capa-explorer)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: Capa Explorer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-capa-explorer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-capa-explorer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-capa-explorer>)

A 

Alex Petrov ✦ Posted: Feb 21, 2023

![Plugin focus: Capa Explorer](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

**This is a guest entry written by Mike Hunhoff, Moritz Raabe, and Willi Ballenthin from the Mandiant FLARE Team. Their views and opinions are their own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the authors.**

## capa explorer: Focus Your Reverse Engineering Efforts in IDA Pro 

[capa explorer ](<https://github.com/mandiant/capa/tree/master/capa/ida/plugin>)is an IDA Pro plugin that automatically identifies capabilities in programs using an extensible set of rules. With capa explorer, you can inspect matches and focus your reverse engineering on the most relevant code. The plugin highlights common malware functionality, can identify algorithms, and helps you write new capa detection rules. We love using capa explorer because it integrates the Mandiant FLARE team’s [capa](<https://github.com/mandiant/capa>) functionality seamlessly into IDA Pro. 

### Using capa explorer

Once installed, you can open capa explorer in IDA Pro by navigating to _Edit_ > _Plugins_ > _FLARE capa explorer_ or using the keyboard shortcut `Alt` – `F5`. See the end of this post for details on how to install capa explorer and how to configure the rules path initially. capa explorer has two views accessible via the tabs located at the top of the plugin window: 

  1. _Program Analysis_ to see identified capabilities 
  2. _Rule Generator_ to write new capa rules 

If you only want to find program capabilities, you will use the _Program Analysis_ view. 

#### Program Analysis

capa explorer’s _Program Analysis_ displays the capabilities that were matched in a program. (see Figure 1) 

This view enables you to: 

  * Filter for specific capability matches via the search bar
  * Navigate IDA’s _Disassembly_ view to the location of each match by double-clicking the address
  * Limit matches to the function currently displayed in IDA’s _Disassembly_ view 
  * Show matches by function
  * And [more…](<https://github.com/mandiant/capa/tree/master/capa/ida/plugin#tips-for-program-analysis>)

![Figure 1: Program Analysis view showing capa matches and their locations](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image1-3.png?width=1000&height=612&name=image1-3.png) Figure 1: Program Analysis view showing capa matches and their locations 

To start analysis, click the _Analyze_ button located at the bottom of the plugin pane. capa explorer caches the analysis results in your database so you can instantly load matches during subsequent invocations. The _Settings_ button enables you to configure various plugin options, including the path to your capa rules and the behavior at startup (start analysis automatically or manually). 

#### Program Analysis

capa explorer’s _Rule Generator_ provides an interactive interface that enables you to easily write new capa rules using features extracted directly from a program (see Figure 2). 

![Figure 2: Rule Generator view to write and test capa ](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image2-Jun-18-2024-08-41-21-2315-AM.png?width=1000&height=612&name=image2-Jun-18-2024-08-41-21-2315-AM.png) Figure 2: Rule Generator view to write and test capa rules 

As you write a rule, the _Rule Generator_ automatically verifies the rule is syntactically correct and matches as expected. You can save the rule directly to your local file system using the _Save_ button. The _Settings_ button enables you to configure _Rule Generator_ settings including the rule author name and default rule scope. For an extensive writeup on using the _Rule Generator_ to easily write and test new capa rules, see our [previous blog post](<https://www.mandiant.com/resources/blog/capa-2-better-stronger-faster>). 

### Exploring program capabilities

Let’s look at two examples of typical malware analysis workflows. These will show how capa explorer quickly focuses reverse engineering on the most relevant and interesting parts of a program. The examples include both a Windows PE file and a Linux ELF file to demonstrate how capa explorer’s results abstract over platform differences and can help to analyze both familiar and unfamiliar file formats.

#### Example 1: Trojanized PuTTY Executable 

The file [putty.exe](<https://www.virustotal.com/gui/file/1492fa04475b89484b5b0a02e6ba3e52544c264c294b57210404b96b65e63266>) is a 3.83 MB 64-bit Windows PE file with over 2600 functions. The program is based on the PuTTY source code but may contain malicious functionality. Manually analyzing a large and complex file can be very time consuming. We use capa explorer within IDA Pro to quickly identify and analyze suspicious capabilities. Our recommended setup displays IDA’s _Disassembly_ view and capa explorer side by side as shown in Figure 3.  ![Figure 3: IDA's Disassembly view and capa explorer side by side](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image3-Jun-18-2024-08-41-16-9080-AM.png?width=1000&height=612&name=image3-Jun-18-2024-08-41-16-9080-AM.png) Figure 3: IDA’s Disassembly view and capa explorer side by side 

capa explorer yields hundreds of matches for this sample. Due to the program size the initial analysis takes a moment but is still faster than by hand. The results shown under _Program Analysis_ quickly highlight suspicious capabilities and point us to the associated code locations. Among the capa results, we see a match for “contain an embedded PE file”. Figure 4  shows the expanded details with a match at virtual address `0x140108050`.

![Figure 4: Breakdown of suspicious 'contain an embedded PE file' match](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image4-3.png?width=1000&height=612&name=image4-3.png) Figure 4: Breakdown of suspicious “contain an embedded PE file” match 

Double-clicking the highlighted address navigates IDA’s _Disassembly_ view to the location of the embedded PE file. There is one cross reference to the embedded PE file bytes from the function at virtual address `0x140032FB0`.

We pivot to the identified function and select _Limit results to current function_. This filters the results to show only capabilities found in the function currently displayed in IDA’s _Disassembly_ view. The results quickly highlight further suspicious behavior found in the current function. Figure 5 shows capa explorer’s summary of the function’s key capabilities.

![Figure 5: capa results for the function starting at virtual address
0x140032FB0](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image5-Jun-18-2024-08-41-19-1776-AM.png?width=1000&height=612&name=image5-Jun-18-2024-08-41-19-1776-AM.png) Figure 5: capa results for the function starting at virtual address 0x140032FB0 

Additional analysis in IDA confirms that the function creates a new directory, copies the file `colorcpl.exe` to this directory, and writes the embedded PE file to `colorui.dll`. Subsequently, the sample executes the file `colorcpl.exe`, which in turn loads the dropped DLL. Additionally, the function schedules a task to establish persistence. See this [Mandiant blog post](<https://www.mandiant.com/resources/blog/dprk-whatsapp-phishing>) for further analysis of this trojanized PuTTY application and related samples. 

This is just one approach to dissecting suspected trojanized files using capa explorer. Another technique we have used successfully is comparing capa matches for a known good file against matches for a suspected trojanized file. In the above example, the known good file does not contain capa hits for an embedded PE file, the directory and file creation, or the task scheduling used for persistence.

#### Example 2: Linux Backdoor

The [ELF file](<https://www.virustotal.com/gui/file/a8acda5ddfc25e09e77bb6da87bfa157f204d38cf403e890d9608535c29870a0>) is a Linux backdoor that supports a small set of commands. We analyze the program with capa explorer and get a quick overview of the file’s capabilities by selecting _Show matches by function_. This groups the rule matches by distinct function and helps us rapidly identify key functions and program capabilities (see Figure 6).  ![Figure 6: capa results grouped by function for an ELF backdoor](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image6-3.png?width=1000&height=612&name=image6-3.png) Figure 6: capa results grouped by function for an ELF backdoor 

We find that _Show matches by function_ helps us to understand what a function may do at a high level. For example, capa explorer shows that the function located at virtual address `0x4047E0` implements capabilities for collecting system information (see Figure 7). 

![Figure 7: capa results indicate system information collection in the function located at 0x4047E0](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image7-Jun-18-2024-08-41-20-5389-AM.png?width=1000&height=612&name=image7-Jun-18-2024-08-41-20-5389-AM.png) Figure 7: capa results indicate system information collection in the function located at 0x4047E0 

The search bar above the results pane lets us quickly focus on specific matches. For example, searching for “file-system” reveals functions that may read or write files (see Figure 8). Inspecting these locations further helps identify relevant host-based indicators like accessed files. 

![Figure 8: filtering capa results for 'file-system' to quickly extract host-based indicators](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image8-3.png?width=1000&height=612&name=image8-3.png) Figure 8: filtering capa results for “file-system” to quickly extract host-based indicators 

By searching for “communication” we see that the program may communicate using raw sockets and which functions implement the sending and receiving of data (see Figure 9). Here further analysis can quickly uncover network-based indicators. 

![Figure 9: filtering capa results for 'communication' to identify network-based indicators](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image9-3.png?width=1000&height=612&name=image9-3.png) Figure 9: filtering capa results for “communication” to identify network-based indicators 

Finally, searching for “process” reveals that the sample may create new and stop existing processes (see Figure 10). 

![Figure 10: filtering capa results for 'process'](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image10-3.png?width=1000&height=612&name=image10-3.png) Figure 10: filtering capa results for “process” 

We navigate IDA’s disassembly view to the function at virtual address `0x4041D0`, deselect _Show matches by function_ , and select _Limit results to current function_ to view the function’s matches. Expanding each entry provides an assembly level breakdown detailing how the match was made. To quickly see the relevant code parts, we can select the checkbox next to an entry and capa explorer will highlight the corresponding address in IDA’s _Disassembly_ view (see Figure 11). The _Reset Selections_ button located at the bottom of the plugin pane removes these selections and highlights. 

![Figure 11: expanding capa results and highlighting features for an identified capability](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/image11-3.png?width=1000&height=612&name=image11-3.png) Figure 11: expanding capa results and highlighting features for an identified capability 

### Installation and Configuration 

You can use Python _pip_ to install capa and its dependencies from PyPI: 

  1. Using the Python interpreter configured for your IDA installation, run `pip install flare-capa`
  2. Copy the [capa_explorer.py](<https://raw.githubusercontent.com/mandiant/capa/master/capa/ida/plugin/capa_explorer.py>) plugin file to your [IDA plugins directory](<https://hex-rays.com/blog/igors-tip-of-the-week-103-sharing-plugins-between-ida-installs/>)



When you first analyze a program capa explorer requires you to specify a directory path containing capa rules. capa explorer provides a GitHub link (located in the rules prompt and under settings) from which you can download and extract the [official capa rules](<https://github.com/mandiant/capa-rules>) that are compatible with your plugin version. The rules directory path that you provide is saved for future runs and can be updated in the capa explorer settings.

Check out [capa explorer’s documentation on GitHub](<https://github.com/mandiant/capa/tree/master/capa/ida/plugin#getting-started>) for additional usage details and the most recent installation steps.

### Now it’s Your Turn

We hope capa explorer is as helpful to you as it is to us. We would love to hear about your experience using the plugin. What does capa explorer do well? What needs to be improved? Feel free to open an issue with your feedback on our [GitHub](<https://github.com/mandiant/capa/issues>) repository.

As maintainers of the [official capa rules repository](<https://github.com/mandiant/capa-rules>) the Mandiant FLARE team invites anyone to contribute rules, ideas, and feedback. Your input can improve the analysis of thousands of reverse engineers and security analysts around the world.

---

## 4. 

**Date:** Posted: Feb 17, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-128-strings-list](https://hex-rays.com/blog/igors-tip-of-the-week-128-strings-list)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #128: String list

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-128-strings-list>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-128-strings-list>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-128-strings-list>)

I 

Igor Skochinsky ✦ Posted: Feb 17, 2023

![Igor’s Tip of the Week #128: String list](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

When exploring an unfamiliar binary, it may be difficult to find interesting places to start from. One common approach is to check what strings are present in the program – this might give some hints about its functionality and maybe some starting places for analysis. While you can scroll through the listing and look at the strings as you come across them, it is probably more convenient to see them all in one place. IDA offer this functionality as the _Strings_ view.

### Opening String list

To open the list, use the menu View > Open subviews > Strings, or the shortcut `Shift`–`F12`. Note that the first time IDA will scan the whole database so it may take some time on big files. If you have a really big binary, it may be useful to [select a range](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>) before invoking the command will so that the scan is limited to the selection.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strings1-3.png?width=1017&height=510&name=strings1-3.png)

The view includes the string’s address, length (in characters, including the terminating one), type (e.g. `C` for standard 8-bit strings or `C16` for Unicode (UTF-16)), and the text of the string. Double-clicking an entry will jump to the string in the binary, and you can, for example, check the [cross-references](<https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references/>) to see where it’s used.

### String list options

The default settings are somewhat conservative so if you think some items are missing (or, conversely, you see a lot of useless entries), changing scan options can be useful. For this, use “Setup..” from the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strings2-3.png?width=257&height=454&name=strings2-3.png)

  * _Display only defined strings_ will have IDA include only explicitly defined string literals (e.g. strings discovered in a middle of undefined areas won’t be included).
  * _Ignore instructions/data definitions_ makes IDA look for text inside code or non-string data.
  * _Strict ASCII (7-bit) strings_ option shows only strings with characters in the basic ASCII range. 
  * _Allowed string types_ lets you choose what string types you are interested in.
  * _Minimal string length_ sets the lower limit on the length the string must have to be included in the list. Raising the limit may be useful to filter out false positives.



Note that you will likely need to invoke “Rebuild…” from the context menu to refresh the list after changing the options.

See also: [IDA Help: Strings window](<https://www.hex-rays.com/products/ida/support/idadoc/1379.shtml>)

---

## 5. 

**Date:** Posted: Feb 10, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-127-changing-function-bounds](https://hex-rays.com/blog/igors-tip-of-the-week-127-changing-function-bounds)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #127: Changing function bounds

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-127-changing-function-bounds>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-127-changing-function-bounds>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-127-changing-function-bounds>)

I 

Igor Skochinsky ✦ Posted: Feb 10, 2023

![Igor’s Tip of the Week #127: Changing function bounds](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

When analyzing regular, well-formed binaries, you can usually rely on IDA’s autoanalysis to create functions and detect their boundaries correctly. However, there may be situations when IDA’s guesses need to be adjusted.

### Non-returning calls

One example could be calls to [non-returning functions](<https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions/>). Let’s say a function has been misdetected by IDA as non-returning:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funcbounds1-3.png?width=692&height=612&name=funcbounds1-3.png)

But on further analysis you realize that it actually returns and remove the no-return flag. However, IDA has already truncated the function after the call and now you need to extend it to include the code after call. How to do it?

### Recreating the function

This is probably the quickest approach which can be used in simple situations:

  1. Go to the start of the function (for example, by double-clicking the function in the [Functions list](<https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list/>)), or via key sequence `Ctrl`–`P`, `Enter`.
  2. Delete the function (from the Functions list), or `Ctrl`–`P`, `Del`. If you were in Graph view, IDA will switch to the text view.
  3. Create it again (Create function… from context menu), or press `P`.



This works well if the changes were enough to fix the original problem. You may need to repeat this a few times when fixing problems one by one. Note that deleting the function may destroy some of the information attached to it (such as the function comment), so this is not always the best choice.

### Editing function bounds

The _Edit function_ dialog has fields for function’s start and end addresses:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funcbounds2-3.png?width=656&height=413&name=funcbounds2-3.png)

They can be edited to expand or shrink the function, but there are some limitations:

  1. The new function bounds may not intersect with another function or a [function chunk](<https://hex-rays.com/blog/igors-tip-of-the-week-86-function-chunks/>). They also may not cross a segment boundary.
  2. The function start must be a valid instruction.



Keep in mind that the end address is exclusive, i.e. it is the address **after** the last instruction of the function.

### Changing the function end

To move the current or preceding function’s end only, you can use the hotkey `E` (Set function end). If there is a function or a chunk at the current address, it is truncated to end just after the current instruction. If the current address does not belong to a function, the nearest preceding function or chunk is extended instead. If the extension causes function chunks to be immediately next to each other, they’re merged together.

For example, consider this situation:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funcbounds3-3.png?width=954&height=573&name=funcbounds3-3.png)

The instructions in the red rectangle should be part of the function but they’re currently “independent” (this can also be seen by the color of the address prefix which is brown and not black like for instructions inside a function). To make them part of the function, we can move its end to the last one (`0027FD6A`). Putting the cursor there and invoking Edit > Functions > Set function end (shortcut `E`) will move the function end from `0027FD44` to `0027FD6A`. Because this makes the function adjacent to its own chunk, IDA merges the chunk with the function and the function is expanded to cover all newly reachable instructions.

See also: 

[IDA Help: Edit Function](<https://www.hex-rays.com/products/ida/support/idadoc/485.shtml>)

[IDA Help: Set Function End](<https://www.hex-rays.com/products/ida/support/idadoc/487.shtml>)

---

## 6. 

**Date:** Posted: Feb 3, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions](https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #126: Non-returning functions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions>)

I 

Igor Skochinsky ✦ Posted: Feb 3, 2023

![Igor’s Tip of the Week #126: Non-returning functions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Some functions in programs do not return to caller: well-known examples include C runtime functions like `exit()`, `abort()`, `assert()` but also many others. Modern compilers can exploit this knowledge to optimize the code better: for example, the code which would normally follow such a function call does not need to be generated which decreases the program size. Other functions, which call non-returning functions unconditionally also become non-returning, which can lead to further optimizations.

### Well-known functions

IDA uses function names to mark well-known non-returning functions. The list of such names is stored in the file `cfg/noret.cfg`, which can be edited to add more names if necessary:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/noret1-3.png?width=833&height=401&name=noret1-3.png)

### Marking non-returning functions manually

Instead of editing `noret.cfg`, you can also mark a function as non-returning manually on a case-by-case basis. This can be done by editing function properties: _Edit > Functions > Edit Function…_ in the main menu, _Edit Function…_ in the context menu or the `Alt`–`P` shortcut.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/noret2-3.png?width=349&height=371&name=noret2-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/noret3-3.png?width=490&height=329&name=noret3-3.png)

Another option is to edit the function’s prototype and add the [`__noreturn` keyword](<https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes/>).

### Identifying no-return calls

Incorrectly identified non-returning calls may lead to various problems during analysis: functions being truncated too early; decompiled pseudocode missing big parts of the function and so on. One option is to inspect each function being called to see if it has the  _Does not return_ flag set (or `Attributes: noreturn` mentioned in a comment) but this can take a long time with many calls. So there are indicators which may be easier to spot:

  * In the text view, look for dashed line after a call; it indicates a break in the code flow which means that the execution does not continue after the call, i.e. it _does not return_.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/noret4-3.png?width=573&height=176&name=noret4-3.png)
  * In the graph view, when a node which ends with a call has no outgoing edge, this means that the call does not return.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/noret5-3.png?width=459&height=194&name=noret5-3.png)
  * In the pseudocode it’s not always obvious, but calls to no-ret functions usually end a compound statement or the whole function. You can also switch to the disassembly if the function looks suspiciously short and look for the above tell-tales.



### Enabling or disabling no-return analysis

If you find that IDA’s treatment of non-returning functions does not work well with your specific binary or set of binaries, you can turn it off. This can be done in the first set of the [analysis options](<https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options/>) at the initial load time or afterwards. Conversely, you can enable it for processors which do not enable it by default.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/noret6-3.png?width=298&height=461&name=noret6-3.png)

If you need to permanently enable or disable it for all new databases, edit the `ANALYSIS` value in `ida.cfg` to include or not the `AF_ANORET` flag. NB: you should edit the value under `#ifdef` for the specific processor you need.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/noret7-3.png?width=563&height=453&name=noret7-3.png)

See also: [IDA Help: Function flags](<https://www.hex-rays.com/products/ida/support/idadoc/1729.shtml>)

---

## 7. 

**Date:** Posted: Jan 27, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation](https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #125: Structure field representation

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation>)

I 

Igor Skochinsky ✦ Posted: Jan 27, 2023

![Igor’s Tip of the Week #125: Structure field representation](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When dealing with structure instances in disassembly, sometimes you may want to change how IDA displays them, but how to do it is not always obvious. Let’s have a look at some examples.

### Win32 section headers

Let’s say you have loaded the PE file header using [manual load](<https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load/>), or found an embedded PE file in your binary, and want to format its PE header nicely. Thanks to the [standard type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>), you can import standard Win32 structures such as `[IMAGE_NT_HEADERS](<https://learn.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_nt_headers32>)` or [IMAGE_SECTION_HEADER](<https://learn.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_section_header>) and apply them to the header area:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/str_repr1-3.png?width=583&height=480&name=str_repr1-3.png)

However, because the `Name` field is declared simply as a `BYTE` array in the original structure, IDA shows them as bytes instead of nice readable string. Without the struct, we could use the Create string (`A`) command, but it is also possible to show the string as part of the structure instance.

### Changing structure field representation

To change how a specific fiield should be formatted in the disassembly, go to it in the structure definition in the Structures window and use Edit or the context menu. For example, use the String (`A`) action to have IDA format the Name byte array as a string.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/str_repr2-3.png?width=662&height=459&name=str_repr2-3.png)

When you edit an imported structure for the first time, you may get this warning:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/str_repr3-3.png?width=435&height=197&name=str_repr3-3.png)

Because the field type representation cannot be specified in Local Types, we have to edit the structure, so answer Yes to continue. A dialog to specify the string length will be displayed, just confirm it:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/str_repr4-3.png?width=394&height=442&name=str_repr4-3.png)

The field will gain a comment indicating that the array is now a string:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/str_repr5-3.png?width=598&height=242&name=str_repr5-3.png)

And the struct instances in the binary will now show the first field as a string:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/str_repr6-3.png?width=778&height=259&name=str_repr6-3.png)

In addition to strings, you can ofcourse change representation of other structure fields similarly to [operand representation](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>) for instructions. For example, you can change the `SizeOfRawData` field to be printed in decimal instead of the default hex.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/str_repr7-3.png?width=813&height=660&name=str_repr7-3.png)

See also: 

[IDA Help: Assembler level and C level types](<https://www.hex-rays.com/products/ida/support/idadoc/1042.shtml>)

[Igor’s tip of the week #46: Disassembly operand representation](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>)

---

## 8. 

**Date:** Posted: Jan 25, 2023  
**Link:** [https://hex-rays.com/blog/deobfuscation-with-goomba](https://hex-rays.com/blog/deobfuscation-with-goomba)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [decompiler](<https://hex-rays.com/blog/tag/decompiler>) [deobfuscation](<https://hex-rays.com/blog/tag/deobfuscation>) [obfuscation](<https://hex-rays.com/blog/tag/obfuscation>)

# Hands-Free Binary Deobfuscation with gooMBA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/deobfuscation-with-goomba>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/deobfuscation-with-goomba>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/deobfuscation-with-goomba>)

A 

Alex Petrov ✦ Posted: Jan 25, 2023

![Hands-Free Binary Deobfuscation with gooMBA](https://hex-rays.com/hubfs/Maintenon%20-%20grey%20waves%20-%202x.png)

![Alt title: The \(uint8\)\(\(\(119*x + 1\) ^ \(137*x + 2\)\) + 2*\(\(119*x + 1\) & \(137*x + 2\)\)\) Musketeers
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gooMBA_1-3.gif?width=512&height=342&name=gooMBA_1-3.gif)

**The gooMBA plugin, as well as this blog post, was written by our intern Garrett Gu. You can view the plugin source on[GitHub](<https://github.com/HexRaysSA/goomba>). gooMBA is maintained by Hex-Rays, and will be incorporated in the next IDA release. **

## Hands-Free Binary Deobfuscation with gooMBA

At Hex-Rays SA, we are constantly looking for ways to improve the usefulness of our state-of-the-art decompiler solution. We achieve this by monitoring for new trends in anti-reversing technology, keeping up with cutting-edge research, and brainstorming ways to innovate on existing solutions.

Today we are excited to introduce a new [**Hex-Rays decompiler**](<https://hex-rays.com/decompiler/>) feature, **gooMBA** , which should greatly simplify the workflow of reverse-engineers working with obfuscated binaries, especially those using Mixed Boolean-Arithmetic (MBA) expressions. Our solution combines algebraic and program synthesis techniques with heuristics for best-in-class performance, integrates directly into the [**Hex-Rays decompiler**](<https://hex-rays.com/decompiler/>), and provides a bridge to an SMT-solver to prove the correctness of simplifications.

## MBA Obfuscation Overview

### What Is MBA?

A Mixed Boolean-Arithmetic (MBA) expression combines arithmetic (e.g. `addition` and `multiplication`) and boolean operations (e.g. bitwise `OR`, `AND`, `XOR`) into a single expression. These expressions are often made extremely complex in order to make it difficult for reverse-engineers to determine their true meaning.

For instance, here is an example of an MBA obfuscation found in a decompilation listing. Note the combination of `bitshift`, `addition`, `subtraction`, `multiplication`, `XOR`, `OR`, and `comparison operators` within one expression.
    
    
    v1 = 715827883LL * (char)((((unsigned __int64)(-424194301LL * (a1 >> 4)) >> 35)+(-424194301LL * (a1 >> 4) < 0)) *  a1);
    v2 = (char)(((((((unsigned  __int64)(-424194301LL * (a1 >> 4)) >> 35) + (-424194301LL * (a1  >> 4) < 0)) * a1 - 48 * ((v1 >> 35) + (v1  < 0))) ^ 0x28) + 111) | 0x33);
    v3 = 818089009LL * (char)(((((((unsigned  __int64)(-424194301LL * (a1 >> 4)) >> 35) + (-424194301LL * (a1  >> 4) < 0)) * a1 - 48 * ((v1 >> 35) + (v1  < 0))) ^ 0x28) + 111) | 0x33);
    v4 = (4 * (v2 - 21 * ((v3 >> 34) + (v3  >> 63)))) & 0xF4 | 8;
    return (v4 - ((v4 / 0x81) & 0x7F | ((v4 /  0x81) << 7))) ^ 0xE;

For reference, the above code always returns `0x89`.

MBA is also used as a name for a _semantics-preserving obfuscation_ technique, which replaces simple expressions found in the source program with much more complicated MBA expressions. MBA obfuscation is called _semantics-preserving_ since it only changes the syntax of the expression, not the underlying semantics — the input/output behavior of the expression should remain the same before and after.

### Why is MBA Reversing Difficult?

A decompiler can be thought of as a massive simplification engine — it reduces the mental load of the reverse engineer by transforming a complex binary program into a vastly simplified higher-level readable format. It partially achieves this through _equivalences_ , special pattern-matching rules derived from mathematical properties such as the commutativity, distributivity, and identity. For instance, the following simplification can be performed by applying the distributive property and identity property.

2a + 3(a+0) = 5a

Both boolean functions and arithmetic functions on integers are very well studied, and there is an abundance of simplification techniques and algorithms developed for each. MBA obfuscators exploit the fact that many of these equivalences and techniques break down when the two function types are _combined_. For instance, we all know that integer multiplication distributes over addition, but note that the same does not hold over the bitwise `XOR`:

3·(2 ⊕ 1) = 3·3 = 9

(3 ⊕ 2)·(3⊕1) = 1·2 = 2

Advanced Computer Algebra Systems (CAS) such as Sage and Mathematica allow users to simplify arithmetic expressions, but their algorithms break down when we start introducing bitwise operations into our inputs.

Furthermore, although Satisfiability Modulo Theories (SMT) solvers such as z3 do often support both arithmetic and boolean operations on computer integers, they do not perform simplification — at least not for any human definition of "simplicity." Rather, their only goal is to prove or disprove the input formula; as a result, they are useful in proving a simplification correct, but not in deriving the simplification to begin with.

### MBA Obfuscation Techniques

The core idea behind MBA obfuscation is that a complex, but semantically equivalent, MBA expression can be substituted for simpler expressions in the source program. For instance, one technique that can be used for MBA generation is the repeated application of simple MBA identities, such as:

x+y=(x|y)+(x&y)  
x+y=2(x|y)-(x⊕y)  
x|y=(¬x|y)-¬x  
x-y=x+¬y+1

Many of these identities are available in the classic book _Hacker’s Delight_ , but there are an effectively unbounded number of them. For instance, _Reichenwallner et al_. easily generated 1,000 distinct MBA substitutions for ` x+y ` alone.

There are also many more sophisticated techniques that can be used for MBA generation, such as applying invertible functions and point functions. The number of invertible functions in computer integers is similarly unbounded. By simply choosing and applying any invertible function followed by its inverse, then applying rewriting rules to mix up the order of operations, an MBA generator can create extremely complex expressions effortlessly.

### Effects of MBA Obfuscation

Besides the obvious effect of making decompilation listings longer and more complex for humans to understand, there are a few other effects which this form of obfuscation can have on the binary analysis process.

For instance, _dataflow/taint analysis_ is a static analysis technique that can be used to automatically search for potentially exploitable parts of a program (such as an unsanitized dataflow from untrusted user input into a SQL query). MBA obfuscation can be used to complicate dataflow analysis, by introducing arbitrary unrelated variables into the MBA expression without modifying its semantics. It then becomes extremely difficult to deduce whether or not the newly introduced variable has an effect on the expression’s final value.

An extreme example of this false dataflow technique is known as _opaque predicates_ , whose original expressions have no semantic data inflows (i.e. they are constant). In other words, they always evaluate to a constant, regardless of their (potentially many) inputs. These opaque predicates can then be used for branching, creating false connections in the control-flow graph in addition to the dataflow graph.

## Prior Work

Over the years, many algorithms have been developed to simplify MBA expressions. These include pattern matching, algebraic methods, program synthesis, and machine learning methods.

### Pattern Matching

Since one of the core techniques involved in MBA generation is the application of rewrite rules, it seems natural to simply match and apply the same rewrite rules in the reverse direction. Indeed, this is precisely what earlier tools such as [SSPAM](<https://github.com/quarkslab/sspam>) did.

There are several issues with pattern matching methods. Firstly, there are a massive number of possible rewrite rules, and proprietary binary obfuscators are unlikely to reveal what rules they use. In addition, at any given moment an expression might contain multiple subexpressions that each match a pattern, and the order in which we perform these simplifications matters! Performing one simplification might prevent a more optimal simplification from appearing down the line. If we were to attempt every possible ordering of optimizations, our search space quickly becomes exponential. As a result, we considered pure pattern-matching methods to be infeasible for our purposes of simplifying complex MBA expressions.

### Algebraic Methods

[Arybo](<https://github.com/quarkslab/arybo>) is an example of an MBA simplifier that relies entirely on algebraic methods. It splits both inputs and outputs into their individual bits and simplifies each bit of the output individually. It’s clear that this method comes with some limitations. For a 64-bit expression, the program outputs 64 individual boolean functions, and it then becomes quite difficult for a human to combine these functions back into a single simplified expression. Notably, the built-in z3 bitvector simplifier also outputs a vector of boolean functions, since this representation is more useful for its main goal of proving whether or not a statement holds.

Other algebraic algorithms for solving MBA expressions which do not split the expression into individual bits also exist. For instance, [MBA-Blast](<https://github.com/softsec-unh/MBA-Blast>) and [MBA-Solver](<https://github.com/softsec-unh/MBA-Solver>) use a transformation between n-bit MBA expressions and 1-bit boolean expressions. For _linear_ MBAs (which we will describe in more detail later), this transformation is well-behaved, and a lookup table can trivially be used to simplify the corresponding boolean expression.

[SiMBA](<https://github.com/DenuvoSoftwareSolutions/SiMBA>), another algorithm published by Denuvo researchers in 2022, uses a similar approach to MBA-Blast and MBA-Solver, but additionally makes the observation that the transformation to 1-bit boolean expressions is not necessary for correctness; rather, the authors prove that it is sufficient to simply limit the domains of all input variables to 0/1. As a result, their algorithm yields much better performance; however, it’s important to note that the algorithm still relies on the algebraic structure of _linear_ MBA expressions, and as a result will not work on all MBA expressions found in the wild.

### Program Synthesis

Program synthesis is the act of generating programs that provably fulfill some useful criteria. In the case of MBA-deobfuscation, our task is to generate simpler programs that are provably semantically equivalent to the provided obfuscated program. In short, two programs are considered semantically equivalent if they yield identical side effects and identical outputs on every possible set of inputs. For the MBA expressions we consider, the expressions have no side effects or branching, so we are just left with the requirement that the simplified expression must yield the same output for every possible set of inputs.

One core observation made by synthesis-based tools such as [Syntia](<https://github.com/RUB-SysSec/syntia>), [QSynthesis](<https://github.com/quarkslab/qsynthesis>), and [msynth](<https://github.com/mrphrazer/msynth>) is that for many real-world programs, the underlying semantics are relatively simple. After all, it is much more common to calculate the sum of two numbers `x+y`, than the result of say, ` 4529*(x>>(y^(11-~x)))`. Thus, for the most part, we only need to consider synthesizing relatively simple programs. To be clear, this is still a massive number of programs, but it at least makes the problem tractable.

The main technique used by QSynth and msynth is an _offline enumerate synthesis primitive guided by top-down breadth-first search_. In simpler terms, these tools take advantage of precomputation, generating and storing a massive database of candidate expressions known as an _oracle_ , searchable by their input/output behavior. Then, when asked to simplify a new expression, they analyze its input/output behavior and use it to perform a lookup in the oracle.

Essentially, the input/output behavior of any expression is summarized by running the candidate expression with various inputs (some random, some specially chosen like `0` or `0xffffffff`), collecting the resulting outputs, and hashing them into a single number. We refer to this number as a _fingerprint_ , and the oracle can be thought of as a multimap from _fingerprints_ to _expressions_. The simplification is then performed by calculating the fingerprint of the expression to be simplified, then looking up the fingerprint in the oracle for simpler equivalent expressions.

### Machine Learning

Tools such as Syntia and [NeuReduce](<https://aclanthology.org/2020.findings-emnlp.56/>) use machine learning and reinforcement learning techniques to search for semantically equivalent expressions on the spot. However, we found that Syntia’s success rate was quite low — only around 15% on linear MBA expressions, and NeuReduce appeared to only have been evaluated on linear MBA expressions (on which it reported a 75% success rate), which are already solvable 100% of the time through algebraic approaches such as MBA-Blast and SiMBA.

## Goals for gooMBA

When designing **gooMBA** , we had the following goals in mind:

  * **Correctness** — Obviously, a tool that outputs nonsense is useless, so we should strive to generate correct simplifications whenever feasible. When a true proof of correctness is infeasible, the tool should try to verify the results to a reasonable degree of certainty.
  * **Speed** — [**The Hex-Rays decompiler**](<https://hex-rays.com/decompiler/>) is well-known in the industry for its speed. Likewise, the tool should strive for the highest performance possible. However, we are obviously willing to sacrifice a couple of seconds in machine-computation time if it means saving a human analyst hours of work.
  * **Integration** — The decompiler plugin should be able to optionally disappear into the background. Ideally, the user should be able to forget that they are even analyzing an obfuscated program and focus only on the work at hand.



## Our Approach

Since there is no single way to generate MBA expressions, we decided to incorporate multiple deobfuscation algorithms into our final design and leave room for more in the future. Our tool, **gooMBA** , can be split into the following parts: microcode tree walking, simplification engine, SMT proofs of correctness, and heuristics.

Below is a drawing of our overall approach:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gooMBA_approach_1-3.png?width=783&height=533&name=gooMBA_approach_1-3.png)

Since we found the SMT stage to be the most time-consuming, we run several hundred random test cases on candidate simplifications before attempting a proof.

### Microcode Tree Walking

Before we can attempt simplification, we must first find potential MBA-obfuscated expressions in the binary. [**The Hex-Rays decompiler**](<https://hex-rays.com/decompiler/>) converts binaries into an intermediate form known as _microcode_ , and continuously propagates variable values downward until a certain complexity limit is reached. Since MBA-expressions can be extremely complex (but notably, not so complex that they hinder performance), we increase the complexity limit when the MBA deobfuscator is invoked in order to maximize the complexity of expressions we can analyze. We then perform a simple tree-search through all expressions found in the program, starting with the most complex top-level expressions, and falling through to simpler subexpressions if they fail to simplify.

### Simplification Engine

Our MBA simplification engine is split into three parts, each handling a subset of MBA expressions. We refer to these three parts as the Simple Linear Algorithm, Advanced Linear Algorithm, and the Synthesis Oracle Lookup.

We can think of each one of these three parts as a self-contained module: the obfuscated expression goes in one end, and a set of candidate expressions (each simpler than the obfuscated expression) comes out of the other end. At this stage, these expressions are simply guesses, and may or may not be correct.

One important thing to note is that all three of our subengines are considered _black-box_ , i.e. they do not care about the _syntactic_ characteristics of the expression being simplified, only its _semantic_ properties — i.e. how the outputs change depending on the input values.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gooMBA_Simplification_Engine_1-3.png?width=864&height=292&name=gooMBA_Simplification_Engine_1-3.png)

### Simple Linear Algorithm

One of the fastest and easiest types of expressions we can simplify are those that reduce to a linear equation, i.e.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gooMBA_SLA-3.png?width=357&height=100&name=gooMBA_SLA-3.png)

Note that constants fall under this category as well. We can simplify these easily by emulating the expression we are trying to simplify, first using zeroes for every input variable. This would tell us the value of `a0`. We can then emulate the instruction once again, this time using zeroes for every input variable except ` x1`. Combined with the previously found value, this tells us the value of `a1`. We can repeat the process until we’ve obtained all the necessary coefficients. Note that the algorithm can also efficiently detect when a variable needs to be `zero- or sign- extended`; we can simply try the value `-1` for each variable and see which of the `zero- or sign-extended` versions of the linear equation matches the output value. It can be shown in this case that both checks will succeed if and only if both sign- and zero-extension are semantically acceptable.

### Advanced Linear Algorithm

Reichenwallner et al. showed that there is also a fast algorithm, namely SiMBA, to simplify _linear_ _MBA_ expressions, defined as those which can be written as a linear combination of bitwise expressions, i.e.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gooMBA_ALA-3.png?width=328&height=55&name=gooMBA_ALA-3.png)

Where each `ei(x1,...,xn) `is a bitwise expression. For instance, `2*(x&y)` is a linear MBA expression, but neither `(x & 0x7)` nor ` (x>>3)` are linear MBA expressions, since neither ` (x & 0x7)` nor ` (x >> 3)` are bitwise or can be written as the linear combination of bitwise expressions.

Essentially, the algorithm works by deriving an equivalent representation consisting of linear combinations of only bitwise conjunctions, e.g. `4 + 2*x + 3*x + 5*(x&y)`. Without going into too much detail, we can recall that every boolean function has a single canonical full `DNF `form (i.e. it can be written as an `OR` of `ANDs` formula), which can then be easily translated into a linear combination of conjunctions. Therefore, every linear MBA expression can be written as a linear combination of conjunctions by simply applying the aforementioned transformation to each individual bitwise function, then combining terms.

Now, this linear combination of `ANDs` can be easily solved using a similar technique we described in the previous section, with the difference being that we must evaluate every possible combination of ` 0/1` value inputs, not just the inputs containing `zero or one 1-values`. Without going into too much detail, the coefficients can then be solved through a system of 2n linear equations of 2n variables, where each variable in the linear system represents one of the conjunctions of original variables, and each equation represents a possible `0/1` assignment to the original variables. We improve upon the algorithm proposed by Reichenwallner et al. by making further observations on the structure of the coefficients in the system and applying the forward substitution technique, yielding a simpler and faster solver.

Finally, Reichenwallner et al. apply an 8-step refinement procedure to find simpler representations, involving more bitwise operations than just conjunction. We found this refinement procedure reasonable and only applied a few tweaks in our implementation.

### Synthesis Oracle Lookup

The algebraic engines are great for deriving constants when the expression’s semantics fulfill a certain structural quality, namely that they are equivalent to a linear combination of bitwise functions. However, we found that non-linear MBAs are also common in real-world binaries. In order to handle these cases, it is necessary to implement a more general algorithm that does not rely on algebraic properties of the input expression.

QSynth (2020, David, et al.) and later msynth (2021, Blazytko, et al.) both rely on a precomputed _oracle_ which contains an indexed list of expressions generated through an enumerative search procedure. These expressions are searchable by what we refer to as _fingerprints_ , which can intuitively be understood as a numeric representation of a function’s `I/O behavior`.

In order to generate a function fingerprint, we begin by generating _test cases_ , which are assignments of possible inputs to the function. For instance, if we had three variables, a possible test case would be `(x=100, y=0, z=-1)`. Then, we feed each one of these test cases into the function being analyzed; for instance, the expression `"x - y + z"` would yield the output value `99` for the previous test case. Finally, we collect all the outputs and hash them into a single number to get the fingerprint. Now we can look up the fingerprint in the oracle and find a function that is possibly semantically equivalent to the analyzed function.

Note that two functions that are indeed semantically equivalent will always yield the same fingerprints (since they will give the same outputs on the test cases). Therefore, if our oracle is exhaustive enough, it should be possible to find equivalences for many MBA-obfuscated expressions. A large precomputed oracle which can be used with goomba is available here: <https://hex-rays.com/products/ida/support/freefiles/goomba-oracle.7z>

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gooMBA_SOL-3.png?width=1350&height=427&name=gooMBA_SOL-3.png)

### SMT Proofs of Correctness

In order to have full confidence in the correctness of our simplifications, we feed both the simplified and original expressions into a satisfiability modulo theories (SMT) solver. Without going into too much detail, we translate IDA’s internal intermediate representation into the SMT language, then confirm that there is no value assignment that causes the two expressions to differ. (In other words, `a != b is UNSAT`.) If the proof succeeds, then we have full faith that the substitution can be performed without changing the semantics of the decompilation. We use the z3 theorem prover provided by Microsoft Research for this purpose.

### Heuristics

We found that invoking the SMT solver leads to unreliable performance, since the solver often times out or takes an unreasonable amount of time to prove equivalences. In order to avoid invoking the solver too often, we use heuristics at various points in our analysis. For instance, we detect whether an expression appears to be an MBA expression before trying to simplify it. In addition, every time before we invoke the SMT solver, we generate random test cases and emulate both the input and simplified expressions to ensure they return the same values. We found the latter heuristic to improve performance up to 1,000x in many cases.

## Evaluation

We evaluated **gooMBA** on the dataset of linear MBA-obfuscated expressions on MBA-Solver’s GitHub repository, an assortment of real-world examples from VirusTotal that appeared to be MBA-obfuscated, and an MBA-obfuscated sample object file from Apple’s FairPlay DRM solution. In terms of correctness, we find what we expect — **gooMBA** , being a combination of multiple algorithms, is able to cover more cases than each algorithm individually.

In terms of performance, we find that **gooMBA** competes very favorably against state-of-the-art linear MBA solvers, and is able to simplify all of the 1,000 or so examples from MBA-Solver much faster than SiMBA. Note that the comparison is not strictly fair, since SiMBA accepts input expressions as a string, and **gooMBA** accepts them as decompilation IR; regardless, we claim that accepting decompilation IR leads to a superior user experience with less possibility for human error.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gooMBA_runtime-3.png?width=624&height=385&name=gooMBA_runtime-3.png)

Compared to msynth, the difference is even more dramatic. On the mba_challenge file provided on msynth’s GitHub repo, we measured the runtime to take around 1.87s per expression. In contrast, our equivalent algorithm took just 0.0047s to run, with the z3 proof taking 0.1s.

## Future Work

We have presented **gooMBA** , a deobfuscator that integrates directly into the [**Hex-Rays decompiler**](<https://hex-rays.com/decompiler/>) in [**IDA Pro**](<https://hex-rays.com/ida-pro/>). This is a meaningful usability trait, since competing tools are typically standalone and require inputting the expression manually or interpreting obtuse outputs. However, this feature also presents some difficulties. For instance, we do not yet perform any use-def analysis or variable propagation beyond what’s already performed by the decompiler. The plugin also currently operates in a purely non-interactive manner, and we believe that adding some interactivity (e.g. allowing the user to choose from a list of simplifications, runs proofs in the background, etc.) would greatly benefit usability.

Some potential areas of improvement for **gooMBA** are: sign extensions are not handled uniformly across all simplification strategies, point function analysis is limited, the simplification oracle is limited by necessity, and use-def analysis can be strengthened to extract expressions spread across basic blocks.

Finally, it’s important to note that MBA obfuscation and deobfuscation are constantly evolving. We based our algorithm choices and implementations on the most promising research on the cutting-edge, but acknowledge that more effective solutions may appear in the future. For instance, though we found that machine learning techniques for MBA-solving have historically underperformed competing methods, machine learning seems like a good candidate for NP-hard problems such as MBA simplification, and we are watching this space for new solutions.

## References

  1. Blazytko, Tim, et al. "Syntia: Synthesizing the semantics of obfuscated code." 26th USENIX Security Symposium (USENIX Security 17). 2017.
  2. Blazytko, Tim, et al. "msynth." https://github.com/mrphrazer/msynth. 2021.
  3. David, Robin, Luigi Coniglio, and Mariano Ceccato. "Qsynth-a program synthesis based approach for binary code deobfuscation." BAR 2020 Workshop. 2020.
  4. Feng, Weijie, et al. "Neureduce: Reducing mixed boolean-arithmetic expressions by recurrent neural network." Findings of the Association for Computational Linguistics: EMNLP 2020. 2020.
  5. Liu, Binbin, et al. "MBA-Blast: Unveiling and Simplifying Mixed Boolean-Arithmetic Obfuscation." 30th USENIX Security Symposium (USENIX Security 21). 2021.
  6. Quarkslab. "SSPAM: Symbolic Simplification with PAttern Matching." https://github.com/quarkslab/sspam. 2016.
  7. Quarkslab. "Arybo." https://github.com/quarkslab/arybo. 2016.
  8. Reichenwallner, Benjamin, and Peter Meerwald-Stadler. "Efficient Deobfuscation of Linear Mixed Boolean-Arithmetic Expressions." Proceedings of the 2022 ACM Workshop on Research on offensive and defensive techniques in the context of Man At The End (MATE) attacks. 2022.
  9. Xu, Dongpeng, et al. "Boosting SMT solver performance on mixed-bitwise-arithmetic expressions." Proceedings of the 42nd ACM SIGPLAN International Conference on Programming Language Design and Implementation. 2021.



[**Visit our Plugin Repository**](<https://plugins.hex-rays.com/>)

---

## 9. 

**Date:** Posted: Jan 24, 2023  
**Link:** [https://hex-rays.com/blog/ida-8-2-service-pack-1-released](https://hex-rays.com/blog/ida-8-2-service-pack-1-released)

[Back](<https://hex-rays.com/blog>)

[IDA](<https://hex-rays.com/blog/tag/ida>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.2 Service Pack 1 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-2-service-pack-1-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-2-service-pack-1-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-2-service-pack-1-released>)

A 

Alex Petrov ✦ Posted: Jan 24, 2023

![IDA 8.2 Service Pack 1 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Service Pack 1 (SP1) for IDA 8.2 is now available. This is primarily a bugfix release.

### How to request the new versions

All new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively, you can [submit your ida.key](<https://www.hex-rays.com/updida/>), and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially shortly after the release. Please be patient.

If you have not received anything within an hour or so, send us a message to [support@hex-rays.com](<mailto:support@hex-rays.com>).

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist](<https://hex-rays.com/products/ida/news/8_2sp1>) [Get a quote](</cgi-bin/quote.cgi>)

---

