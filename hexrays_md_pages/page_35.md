# Hex-Rays Blog – Page 35

**Archived:** 2026-02-20 12:21
**URL:** https://hex-rays.com/blog/page/35

---

## 1. 

**Date:** Posted: Sep 25, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood](https://hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #08: Batch mode under the hood

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood>)

I 

Igor Skochinsky ✦ Posted: Sep 25, 2020

![Igor’s tip of the week #08: Batch mode under the hood](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

We’ve briefly covered batch mode [last time](<https://hex-rays.com/blog/igor-tip-of-the-week-07-ida-command-line-options-cheatsheet/>) but the basic functionality is not always enough so let’s discuss how to customize it.

### Basic usage

To recap, batch mode can be invoked with this command line:
    
    
    ida -B -Lida.log <other switches> <filename>

IDA will load the file, wait for the end of analysis, and write the full disassembly to `<filename>.asm`

### How it works

In fact, `-B` is a shorthand for `-A -Sanalysis.idc:`

  * `-A`: enable autonomous mode (answer all queries with the default choice).
  * `-Sanalysis.idc:` run the script `analysis.idc` after loading the file.



You can find `analysis.idc` in the `idc` subdirectory of IDA install. In IDA 7.5 it looks as follows:
    
    
    static main()
    {
    	// turn on coagulation of data in the final pass of analysis
    	set_inf_attr(INF_AF, get_inf_attr(INF_AF) | AF_DODATA | AF_FINAL);
    	// .. and plan the entire address space for the final pass
    	auto_mark_range(0, BADADDR, AU_FINAL);
    	msg("Waiting for the end of the auto analysis...\n");
    	auto_wait();
    	msg("\n\n------ Creating the output file.... --------\n");
    	auto file = get_idb_path()[0:-4] + ".asm";
    	auto fhandle = fopen(file, "w");
    	gen_file(OFILE_ASM, fhandle, 0, BADADDR, 0); // create the assembler
    	file
    	msg("All done, exiting...\n");
    	qexit(0); // exit to OS, error code 0 - success
    }
    

Thus, to modify the behavior of the batch mode you can:

  * Either modify the standard `analysis.idc`
  * Or specify a different script using `-S<myscript.idc>`



For example, to output an LST file (it includes address prefixes), change the [gen_file](</products/ida/support/idadoc/244.shtml>) call:
    
    
    gen_file(OFILE_LST, fhandle, 0, BADADDR, 0);

### Batch decompilation

If you have the [decompiler](</products/decompiler/>) for the target file’s architecture, you can also run it in [batch mode](</products/decompiler/manual/batch.shtml>).

For example, to decompile the whole file:
    
    
    ida -Ohexrays:outfile.c:ALL -A <filename>

To decompile only the function `main`:
    
    
    ida -Ohexrays:outfile.c:main -A <filename>

This uses the functionality built-in into the decompiler plugin which works similarly to the `analysis.idc` script (wait for the end of autoanalysis, then decompile the specified functions to `outfile.c`).

### Customizing batch decompilation

If the default functionality is not enough, you could write a plugin to drive the decompiler via its [C++ API](</products/decompiler/sdk/>). However, for scripting it’s probably more convenient to use Python. Similarly to IDC, Python scripts can be used with the `-S` switch to be run automatically after the file is loaded.

A sample script is attached to this post. Use it as follows:
    
    
    ida -A -Sdecompile_entry_points.py -Llogfile.txt <filename>

### Speeding up batch processing

In the examples so far we’ve been using the `ida` executable which is the full GUI version of IDA. Even though the UI is not actually displayed in batch mode, it still has to load and initialize all the dependent UI libraries which can take non-negligible time. This is why it is often better to use the text-mode executable (`idat`) which uses lightweight text-mode UI. However, it still needs a terminal even in batch mode. In case you need to run it in a situation without a terminal (_e.g._ run it in background or from a daemon), you can use the following approach:

  1. set environment variable `TVHEADLESS=1`
  2. redirect output



For example:
    
    
    TVHEADLESS=1 idat -A -Smyscript.idc file.bin >/dev/null &

Downloads

[decompile_entry_points.py](</wp-content/uploads/2020/09/decompile_entry_points.py>)

---

## 2. 

**Date:** Posted: Sep 23, 2020  
**Link:** [https://hex-rays.com/blog/hex-rays-plugin-contest-looking-back-at-a-10-year-journey](https://hex-rays.com/blog/hex-rays-plugin-contest-looking-back-at-a-10-year-journey)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-Rays plugin contest: looking back at a 10-year journey

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-plugin-contest-looking-back-at-a-10-year-journey>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-plugin-contest-looking-back-at-a-10-year-journey>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-plugin-contest-looking-back-at-a-10-year-journey>)

Fabrice Ovidio ✦ Posted: Sep 23, 2020

![Hex-Rays plugin contest: looking back at a 10-year journey](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

The Hex-Rays plugin Contest was an initiative by the experts behind IDA Pro, the state-of-the-art binary analysis tool. The contest, still taking place each year, encourages IDA users to create innovative and useful extensions for IDA and/or the Decompiler. 2019 marked its 10-year celebration.

Hex-Rays deeply appreciates all participants for spending time and making this contest an incredible journey. A small recap is presented below with our enormous gratitude to the limitless creativity of our precious IDA users.

## The journey began…

Started back in 2009 when IDA was at version 5.5, the very first edition of Hex-Rays’ plugin contest already received a considerable amount of submissions. It scaled up over the years and attracted more and more attention from the whole reverse engineering community. The amount of submissions and contestants increased and so did our satisfaction, seeing how IDA could be powered and extended with such creative and interesting add-ons.

## An inspiring contest for and by IDA users

10 years later, a lot has changed and evolved since that first prize was awarded, especially for IDA itself. The Hex-Rays team still carefully selects the best plugins for each contest. In 10 years, the numbers of submissions and contestants each year tripled and these 10 years were full of insights and improvements. We are glad that the contest has always enabled IDA users to take further out-of-the-box steps and to enhance IDA’s and the Decompiler’s functionalities and capabilities with their creativity. These innovative plugins can without a doubt serve the whole reverse engineering community.

![contest statistics](https://hex-rays.com/hubfs/Imported_Blog_Media/PLUGIN-CONTEST-10-years-3-3.png)

## A significant and continuous source of innovation

At this important milestone of the plugin contest, we gathered to select the most influential plugins and highlight their main contributions throughout the years. These plugins won praise first because they greatly extend the capabilities of IDA but also because they inspired us to further improve IDA. We are grateful to the authors and believe the contributions deserved to be remembered.

> These are our picks but it was difficult to make up our minds and select just a few to place under the spotlight: it’s important to stress how many other fantastic plugins we have had the privilege of reviewing across the years. 

#### 2009 – IDADWARF by Vincent Rasneur from DenyAll

[IDADWARF](<https://www.hex-rays.com/contests_details/contest2009/#1>) imports DWARF debug information into IDA databases. After seeing the results of this plugin’s work, we knew that we simply had to offer this functionality out of box and the official DWARF support shipped in IDA 6.4 (2013).

![IDADWARF 2009](https://hex-rays.com/hubfs/Imported_Blog_Media/dwarf2-2009-3.gif)

#### 2013 – hexrays_tools and 2016 – Command Palette & hexlight by Milan Bohacek

[hexrays_tools](<https://www.hex-rays.com/contests_details/contest2013/#hrtools>) partially motivated us to publish the microcode in IDA 7.1 (2018). Milan is one of our most active participants and helped us realize how useful even small usability improvements can be. Since then, several of the features from his plugin have been implemented directly in IDA and the Decompiler. Thank you, Milan!

![hexrays_tools 2016](https://hex-rays.com/hubfs/Imported_Blog_Media/2013-hexraystool-3.png)

![command-palette 2016](https://hex-rays.com/hubfs/Imported_Blog_Media/2016-command-palette-3.png)

![highlight 2016](https://hex-rays.com/hubfs/Imported_Blog_Media/2016-hexrays-highlight-3.png)

#### 2015 – Dynamic IDA Enrichment (DIE) by Yaniv Balmas

Even though [DIE](<https://www.hex-rays.com/contests_details/contest2015/#die>) is not actively maintained anymore, requires Python 2.7 and an unmaintained version of Sark, it was one of the plugins that showed an interesting use of IDA’s debugger: extracting information from a running process. Over the years, several other similar plugins were submitted, such as [funcap](</contests_details/contest2013/#funcap>) by Andrzej Dereszowski (2013) and [dereferencing](</contests_details/contest2019/#dereferencing>) by Daniel Garcia Gutierrez (2019).

![DIE 2016](https://hex-rays.com/hubfs/Imported_Blog_Media/die-2015-3.png)

#### 2017 – Lighthouse by Markus Gaasedelen

This plugin is very simple to install and use. Written in PyQt, [Lighthouse](<https://www.hex-rays.com/contests_details/contest2017/#lighthouse>) allows users to visualize execution traces and perform ‘sets’ operations on them. This can be helpful in dynamic analysis. Lighthouse highlights how important PyQt is and the amazing things that can be built on top of it. Today, PyQt is a first-class citizen of IDA and ships with it.

![Lighthouse 2017](https://hex-rays.com/hubfs/Imported_Blog_Media/Lighthouse-2017-3.png)

#### 2018 – IDArling by Alexandre Adamski and Joffrey Guilbon

Together with a few other plugins trying to achieve the same goal, [IDArling](<https://www.hex-rays.com/contests_details/contest2018/#idarling>) helped us realize that there is a very strong demand for teamwork/collaboration in IDA.

* * *

## Postscript

These days, whilst waiting for the 2020 plugin contest results and standing at the threshold of its next decade, we would like to take this opportunity to thank all the contestants who participated and helped the contest reach this point. This contest remains a playground for and by IDA users, inspires them to demonstrate creativity, to dare to think beyond the limits and to give back to the community.

_Here’s to the next 10 years of inspiration and innovation!_

---

## 3. 

**Date:** Posted: Sep 18, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-07-ida-command-line-options-cheatsheet](https://hex-rays.com/blog/igor-tip-of-the-week-07-ida-command-line-options-cheatsheet)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #07: IDA command-line options cheatsheet

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-07-ida-command-line-options-cheatsheet>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-07-ida-command-line-options-cheatsheet>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-07-ida-command-line-options-cheatsheet>)

I 

Igor Skochinsky ✦ Posted: Sep 18, 2020

![Igor’s tip of the week #07: IDA command-line options cheatsheet](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Most IDA users probably run IDA as a stand-alone application and use the UI to configure various options. However, it is possible to pass command-line options to it to automate some parts of the process. The [full set of options](</products/ida/support/idadoc/417.shtml>) is quite long so we’ll cover the more common and useful ones.

__In the examples below,`ida` can be replaced by `ida64` for 64-bit files, or `idat` (`idat64`) for console (text-mode) UI.

### Simply open a file in IDA

`ida <filename>`

`<filename>` can be a new file that you want to disassemble or an existing database. This usage is basically the same as using File > Open or dropping the file onto IDA’s icon. You still need to manually confirm the options in the Load File dialog or any other prompts that IDA displays, but the initial splash screen is skipped.

__If you use any additional command-line options, make sure to put them _before_ the filename or they’ll be ignored.

### Open a file and auto-select a loader

`ida -T<prefix> <filename>`

Where `<prefix>` is a _unique_ prefix of the loader description shown in the Load file dialog. For example, when loading a .NET executable, IDA proposes the following options:

  * Microsoft.Net assembly
  * Portable executable for AMD64 (PE)
  * MS-DOS executable (EXE)
  * Binary file



For each of them, the corresponding`-T` option could be:

  * `-TMicrosoft`
  * `-TPortable`
  * `-TMS`
  * `-TBinary`



When the prefix contains a space, use quotes. For example, to load the first slice from a fat Mach-O file:

`ida "-TFat Mach-O File, 1" file.macho`

In case of archive formats like ZIP, you can specify the archive member to load after a colon (and additional loader names nested as needed). For example, to load the main dex file from an .apk (which is a zip file):

`ida -TZIP:classes.dex:Android file.apk`

However, it is usually better to pick the APK loader at the top level (especially in the case of multi-dex files)

`ida -TAPK file.apk`

When `-T` is specified, the initial load dialog is skipped and IDA proceeds directly to loading the file using the specified loader (but any additional prompts may still be shown).

### Auto-accept any prompts, informational messages or warnings

Sometimes you just want to load the file and simply accept all default settings. In such case you can use the -A switch:

`ida -A <filename>`

This will load the file using _autonomous,_ or _batch,_ mode, where IDA will not display any dialog but accept the default answer in all cases.

__In this mode**no** interactive dialogs will show up after loading is finished (e.g not even “Rename” or “Add comment”). To restore interactivity, execute [`batch(0)`](</products/ida/support/idadoc/287.shtml>) statement in the IDC or Python console at the bottom of IDA’s window.

### Batch disassembly

This is an extension of the previous section and is invoked using the -B switch:

`ida -B <filename>`

IDA will load the file using all default options, wait for the end of auto-analysis, output the disassembly to `<filename>.asm` and exit after saving the database.

### Binary file options

When loading raw binary files, IDA cannot use any of the metadata that is present in higher-level file formats like ELF, PE or Mach-O. In particular, the _processor type_ and _loading address_ cannot be deduced from the file and have to be provided by the user. To speed up your workflow, you can specify them on the command line:

`ida -p<processor> -B<base> <filename>`

`<processor>` is one of the [processor types](</products/ida/support/idadoc/618.shtml>) supported by IDA. Some processors also support options after a colon.

`<base>` is the hexadecimal load base _in paragraphs_ (16-byte quantities). In practice, it means that you should remove the last zero from the full address.

For example, to load a big-endian MIPS firmware at linear address 0xBFC00000:

`ida -pmipsb -bBFC0000 firmware.bin`

A Cortex-M3 firmware mapped at 0x4000:

`ida -parm:ARMv7-M -b400 firmware.bin`

### Logging

When IDA is running autonomously, you may miss the messages that are usually printed in the Output window but they may contain important informational messages, errors, or warnings. To keep a copy of the messages you can use the `-L` switch:

`ida -B -Lida_batch.log <filename>`

---

## 4. 

**Date:** Posted: Sep 11, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-06-release-notes](https://hex-rays.com/blog/igor-tip-of-the-week-06-release-notes)

[Back](<https://hex-rays.com/blog>)

[shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #06: IDA Release notes

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-06-release-notes>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-06-release-notes>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-06-release-notes>)

I 

Igor Skochinsky ✦ Posted: Sep 11, 2020

![Igor’s tip of the week #06: IDA Release notes](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

With [every IDA release](</products/ida/news/>), we publish detailed release notes describing various new features, improvements and bugfixes. While some of the additions are highlighted and therefore quite visible, others are not so obvious and may require careful reading. Having a closer look at these release notes, you will be surprised to see many small but useful features added through different IDA versions.

**A couple of good examples can be:**

### Register definition and use

Added in IDA 7.5, these actions allow you to quickly jump between various uses of a register.

> • UI: added actions to search for register definition or register use (Shift+Alt+Up, Shift+Alt+Down) 

`Shift`–`Alt`–`Up`: find the previous location where the selected register is **defined** (written to).

`Shift`–`Alt`–`Down`: find the next location where the selected register is **used** (read from or partially overwritten).

These actions are especially useful in big functions compiled with high optimization level where the distance between definition and use can be quite big so tracking registers visually using [standard highlight](</blog/igor-tip-of-the-week-05-highlight/>) is not always feasible.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/regdefuse-3.png)

In the above screenshot, you can see that `Alt`–`Up` jumps to the closest highlight substring match while `Shift`–`Alt`–`Up` finds where rbx was changed (`ebx` is the low part of `rbx` so the `xor` instruction changes `rbx`).

These actions are currently implemented for a limited number of processors (x86/x64, ARM, MIPS), but may be extended to others if we get more requests.

### Jump to previous or next function

> \+ ui: added shortcuts Ctrl+Shift+Up/Ctrl+Shift+Down to jump to the start of the previous/next function 

Added in IDA 7.2, these are minor but very useful shortcuts, especially in large binaries with many big functions.

By the way, if standard shortcuts are tricky to use, you can always [set custom ones](</blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>) using a key combination you prefer.

---

## 5. 

**Date:** Posted: Sep 4, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight](https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #05: Highlight

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight>)

I 

Igor Skochinsky ✦ Posted: Sep 4, 2020

![Igor’s tip of the week #05: Highlight](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In [IDA](<https://www.hex-rays.com/products/ida/>), **highlight** is the dynamic coloring of a word or number under the cursor as well as all matching substrings on the screen. In the default color scheme, a yellow background color is used for the highlight.

Highlight is updated when you click on a non-whitespace location in the listing or move the cursor with the arrow keys. Highlight is _not_ updated (remains the same) when:

  * moving the cursor with `PgUp`, `PgDn`, `Home`, `End`;
  * scrolling the listing with mouse wheel or scroll bar;
  * using Jump commands or clicking in the navigation band (unless the cursor happens to land on a word at the new location);
  * highlight is locked by the LockHighlight action (it is one of the handful of actions which are only available as a toolbar button by default). 

![](https://hex-rays.com/hubfs/Imported_Blog_Media/highlight_lock-3.png)




### Register highlight

For some processors, highlighted registers are treated in a special way: not only is the same register highlighted but also any register which contains it or is a part of it. For example, on x86_x64, if `ax` is selected, then `al`, `ah`, `eax` and `rax` get highlighted too.

### ![](https://hex-rays.com/hubfs/Imported_Blog_Media/highlight_reg-3.png)

### Manual highlight

In addition to the automatic highlight by clicking on a word/number, you can also select an arbitrary substring using mouse or keyboard and it will be used to highlight all matching sequences on the screen. For manual highlight, only exactly matching substrings are highlighted — there is no special handling for the registers.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/highlight_manual-3.png)

### Highlight navigation

You can quickly jump between highlighted matches using `Alt`–`Up` and `Alt`–`Down`. This works even if the closest match is not on screen — IDA will look for next match in the selected direction.

Highlight is available not only in the disassembly listing but in most text-based IDA subviews: Pseudocode, Hex View, Structures and Enums.

---

## 6. 

**Date:** Posted: Aug 28, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-04-more-selection](https://hex-rays.com/blog/igor-tip-of-the-week-04-more-selection)

[Back](<https://hex-rays.com/blog>)

[shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #04: More selection!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-04-more-selection>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-04-more-selection>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-04-more-selection>)

I 

Igor Skochinsky ✦ Posted: Aug 28, 2020

![Igor’s tip of the week #04: More selection!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

In the [previous post](</blog/igor-tip-of-the-week-03-selection-in-ida/>) we talked about the basic usage of selection in IDA. This week we’ll describe a few more examples of actions affected by selection.

### Firmware/raw binary analysis

When disassembling a raw binary, IDA is not always able to detect code fragments and you may have to resort to trial & error for finding the code among the whole loaded range which can be a time-consuming process. In such situation the following simple approach may work for initial reconnaissance:

  1. Go to the start of the database (`Ctrl`–`PgUp`);
  2. Start selection (`Alt`–`L`);
  3. Go to the end (`Ctrl`–`PgDn`). You can also go to a specific point that you think may be the end of code region (e.g. just before a big chunk of zeroes or FF bytes);
  4. Select Edit > Code or press `C`. You’ll get a dialog asking what specific action to perform:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/select_code-3.png)
  5. Click “Force” if you’re certain there are mostly instructions in the selected range, or “Analyze” if there may be data between instructions.
  6. IDA will go through the selected range and try to convert any undefined bytes to instructions. If there is indeed valid code in the selected area, you might see functions being added to the Functions window (probably including some false positives).




### Structure offsets

Another useful application of selection is applying structure offsets to multiple instructions. For example, let’s consider this function from a UEFI module:
    
    
    .text:0000000000001A64 sub_1A64        proc near               ; CODE XREF: sub_15A4+EB↑p
    .text:0000000000001A64                                         ; sub_15A4+10E↑p
    .text:0000000000001A64
    .text:0000000000001A64 var_28          = qword ptr -28h
    .text:0000000000001A64 var_18          = qword ptr -18h
    .text:0000000000001A64 arg_20          = qword ptr  28h
    .text:0000000000001A64
    .text:0000000000001A64                 push    rbx
    .text:0000000000001A66                 sub     rsp, 40h
    .text:0000000000001A6A                 lea     rax, [rsp+48h+var_18]
    .text:0000000000001A6F                 xor     r9d, r9d
    .text:0000000000001A72                 mov     rbx, rcx
    .text:0000000000001A75                 mov     [rsp+48h+var_28], rax
    .text:0000000000001A7A                 mov     rax, cs:gBS
    .text:0000000000001A81                 lea     edx, [r9+8]
    .text:0000000000001A85                 mov     ecx, 200h
    .text:0000000000001A8A                 call    qword ptr [rax+50h]
    .text:0000000000001A8D                 mov     rax, cs:gBS
    .text:0000000000001A94                 mov     r8, [rsp+48h+arg_20]
    .text:0000000000001A99                 mov     rdx, [rsp+48h+var_18]
    .text:0000000000001A9E                 mov     rcx, rbx
    .text:0000000000001AA1                 call    qword ptr [rax+0A8h]
    .text:0000000000001AA7                 mov     rax, cs:gBS
    .text:0000000000001AAE                 mov     rcx, [rsp+48h+var_18]
    .text:0000000000001AB3                 call    qword ptr [rax+68h]
    .text:0000000000001AB6                 mov     rax, [rsp+48h+var_18]
    .text:0000000000001ABB                 add     rsp, 40h
    .text:0000000000001ABF                 pop     rbx
    .text:0000000000001AC0                 retn
    .text:0000000000001AC0 sub_1A64        endp
    

If we know that `gBS` is a pointer to `EFI_BOOT_SERVICES`, we can convert accesses to it (in the call instructions) to structure offsets. It can be done for each access manually but is tedious. In such situation the selection can be helpful. If we select the instructions accessing the structure and press `T` (structure offset), a new dialog pops up:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/sel_stroff-3.png)

You can select which register is used as the base, which structure to apply and even select which specific instructions you want to convert.

After selecting `rax` and `EFI_BOOT_SERVICES`, we get a nice-looking listing:
    
    
    .text:0000000000001A64 sub_1A64        proc near               ; CODE XREF: sub_15A4+EB↑p
    .text:0000000000001A64                                         ; sub_15A4+10E↑p
    .text:0000000000001A64
    .text:0000000000001A64 Event           = qword ptr -28h
    .text:0000000000001A64 var_18          = qword ptr -18h
    .text:0000000000001A64 Registration    = qword ptr  28h
    .text:0000000000001A64
    .text:0000000000001A64                 push    rbx
    .text:0000000000001A66                 sub     rsp, 40h
    .text:0000000000001A6A                 lea     rax, [rsp+48h+var_18]
    .text:0000000000001A6F                 xor     r9d, r9d        ; NotifyContext
    .text:0000000000001A72                 mov     rbx, rcx
    .text:0000000000001A75                 mov     [rsp+48h+Event], rax ; Event
    .text:0000000000001A7A                 mov     rax, cs:gBS
    .text:0000000000001A81                 lea     edx, [r9+8]     ; NotifyTpl
    .text:0000000000001A85                 mov     ecx, 200h       ; Type
    .text:0000000000001A8A                 call    [rax+EFI_BOOT_SERVICES.CreateEvent]
    .text:0000000000001A8D                 mov     rax, cs:gBS
    .text:0000000000001A94                 mov     r8, [rsp+48h+Registration] ; Registration
    .text:0000000000001A99                 mov     rdx, [rsp+48h+var_18] ; Event
    .text:0000000000001A9E                 mov     rcx, rbx        ; Protocol
    .text:0000000000001AA1                 call    [rax+EFI_BOOT_SERVICES.RegisterProtocolNotify]
    .text:0000000000001AA7                 mov     rax, cs:gBS
    .text:0000000000001AAE                 mov     rcx, [rsp+48h+var_18] ; Event
    .text:0000000000001AB3                 call    [rax+EFI_BOOT_SERVICES.SignalEvent]
    .text:0000000000001AB6                 mov     rax, [rsp+48h+var_18]
    .text:0000000000001ABB                 add     rsp, 40h
    .text:0000000000001ABF                 pop     rbx
    .text:0000000000001AC0                 retn
    .text:0000000000001AC0 sub_1A64        endp
    

### Forced string literals

When some code is referencing a string, IDA is usually smart enough to detect it and convert referenced bytes to a literal item. However, in some cases the automatic conversion does not work, for example:

  * string contains non-ASCII characters
  * string is not null-terminated



A common example of the former is Linux kernel which uses a special byte sequence to mark different categories of kernel messages. For example, consider this function from the `joydev.ko` module:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/sel_joydev-3.png)

IDA did not automatically create a string at 1BC8 because it starts with a non-ASCII character. However, if we select the string’s bytes and press `A` (Convert to string), a string is created anyway:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/sel_joydev2-3.png)

### Creating structures from data

This action is useful when dealing with structured data in binaries. Let’s consider a table with approximately this layout of entries:
    
    
    struct copyentry {
     void *source;
     void *dest;
     int size;
     void* copyfunc;
    };
    

While such a structure can always be created manually in the Structures window, often it’s easier to format the data first then create a structure which describes it. After creating the four data items, select them and from the context menu, choose “Create struct from selection”:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/sel_struct1-Jun-18-2024-08-40-25-9167-AM.png)

IDA will create a structure representing the selected data items which can then be used to format other entries in the program or in disassembly to better understand the code working with this data.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/sel_struct2-3.png)

---

## 7. 

**Date:** Posted: Aug 21, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida](https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida)

[Back](<https://hex-rays.com/blog>)

[shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #03: Selection in IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida>)

I 

Igor Skochinsky ✦ Posted: Aug 21, 2020

![Igor’s tip of the week #03: Selection in IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

This week’s post is about **selecting items** in IDA and **what you can do** with the selection.

As a small change from the previous posts with mainly keyboard usage, we’ll also use the mouse this time!

### Actions and what they are applied to

When an action is performed in IDA, by default it is applied only to _the item under the cursor_ or to _the current address_ (depending on the action). However, sometimes you might want to perform the action on _more items_ or to _an address range_ , for example to:

  * undefine a range of instructions;
  * convert a range of undefined bytes to a string literal if IDA can’t do it automatically (_e.g._ string is not null-terminated);
  * create a function from a range of instructions with some data in the middle (_e.g._ when you get the dreaded “The function has undefined instruction/data at the specified address” error);
  * export disassembly or decompilation of only selected functions instead of the whole file;
  * copy to clipboard a selected fragment of the disassembly.



### Selecting in IDA

The simplest ways to select something in IDA are the same as in any text editor:

  * click and drag with the mouse (you can also scroll with the wheel while keeping the left button pressed);
  * hold down `Shift` and use the cursor navigation keys ( ← ↑ → ↓ `PgUp` `PgDn` `Home` `End` etc.).



However, this can quickly become tiring if you need to select a huge block of the listing (_e.g._ several screenfuls). In that case, the _anchor selection_ will be of great use.

### Using the anchor selection

  1. Move to the start of the intended selection and select Edit > Begin selection (or use the `Alt`–`L` shortcut).
  2. Navigate to the other end of the selection using any means (cursor keys, Jump actions, Functions window, Navigation bar etc.).
  3. Perform the action (via context menu, keyboard shortcut, or global menu). It will be applied to the selection from the anchor point to the current position.



### Examples

Some of the actions that use selection:

  * Commands in the File > Produce file submenu (create .ASM, .LST, HTML or .C file)
  * Edit > Export data (`Shift`–`E`)



![Export Data dialog screenshot](https://hex-rays.com/hubfs/Imported_Blog_Media/export_data-3.png)

Some more complicated actions requiring selection will be discussed in the forthcoming posts. Stay tuned and see you next Friday!

---

## 8. 

**Date:** Posted: Aug 14, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them](https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them)

[Back](<https://hex-rays.com/blog>)

[shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #02: IDA UI actions and where to find them

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them>)

I 

Igor Skochinsky ✦ Posted: Aug 14, 2020

![Igor’s tip of the week #02: IDA UI actions and where to find them](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

In the previous post we described how to quickly invoke some of IDA’s commands using the keyboard. However, sometimes you may need to perform a specific action many times and if it doesn’t have a default hotkey assigned it can be tedious to click through the menus. Even the accelerator keys help only so much. Or it may be difficult to discover or find a specific action in the first place (some actions do not even have menu items). There are **two IDA features** that would help here:

### The shortcut editor

The editor is invoked via Options > Shortcuts… and allows you to see, add, and modify shortcuts for almost all UI actions available in IDA.

![shortcut editor](https://hex-rays.com/hubfs/Imported_Blog_Media/shotcut_editor-3.png)

The dialog is non-modal and shows which actions are available for the current view (currently disabled ones are struck out) so you can try clicking around IDA and see how the set of available actions changes depending on the context.

To assign a shortcut, select the action in the list then type the key combination in the “Shortcut:” field (on Windows you can also click the “Record” button and press the desired shortcut), then click “Set” to save the new shortcut for this and all future IDA sessions. Use “Restore” to restore just this action, or “Reset” to reset all actions to the default state (as described in `idagui.cfg`).

### The command palette

Command palette (default shortcut is `Ctrl`–`Shift`–`P`) is similar to the Shortcut editor in that it shows the list of all IDA actions but instead of changing shortcuts you can simply _invoke_ the action.

![palette jump](https://hex-rays.com/hubfs/Imported_Blog_Media/palette_jump-3.png)

The filter box at the bottom filters the actions that contain the typed text with fuzzy matching and is focused when the palette is opened so you can just type the approximate name of an action and press `Enter` to invoke the best match.

---

## 9. 

**Date:** Posted: Aug 7, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida](https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida)

[Back](<https://hex-rays.com/blog>)

[shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #01: Lesser-known keyboard shortcuts in IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida>)

I 

Igor Skochinsky ✦ Posted: Aug 7, 2020

![Igor’s tip of the week #01: Lesser-known keyboard shortcuts in IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Today, Hex-Rays is excited to launch a special blog series where Igor, one of the experts behind IDA, will provide useful tips and functionalities of IDA that are not always known or less obvious to its users.

The first episode of this blog series covers the most useful keyboard shortcuts that will certainly speed up your IDA experience.

So, we hope you enjoy this first post and tune in every Friday to read **Igor’s tip of the week**!

* * *

This week’s tip will be about using the keyboard in IDA. Nowadays, while most actions can be carried out using the mouse, it can still be much faster and more efficient to use the keyboard. IDA first started as a DOS program, long before GUI and mouse became common, which is why you can still do most of the work without touching the mouse! While most of common shortcuts can be found in the cheat sheet ([HTML](<https://www.hex-rays.com/wp-content/static/products/ida/idapro_cheatsheet.html>), [PDF](<https://www.hex-rays.com/wp-content/static/products/ida/support/freefiles/IDA_Pro_Shortcuts.pdf>)), there remains some which are less obvious, but _incredibly useful_!

### Text input dialog boxes (_e.g._ Enter Comment or Edit Local Type)

![use ctrl-enter](https://hex-rays.com/hubfs/Imported_Blog_Media/dlg_textedit-3.png)

You can use `Ctrl`–`Enter` to confirm (  OK ) or `Esc` to dismiss (Cancel) the dialog. This works regardless of the button arrangement (which can differ depending on the platform and/or theme used).

### Quick menu navigation

If you hold down `Alt` on Windows (or enable a system option), you should see underlines under the menu item names.

![IDA menu with underlined accelerator keys](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/menu_accel-3.png?width=447&height=385&name=menu_accel-3.png)

You can press the underlined letter (also known as “accelerator”) while holding down `Alt` to open that menu, and _then_ press the underlined letter of the specific menu item to trigger it. The second step will work even if you release `Alt`. For example, to execute “Search > Not function” (which has no default hotkey), you can press `Alt`–`H`, `F`. Although there may be no underlines on Linux or Mac, the same key sequence should still work. If you don’t have access to a Windows IDA and don’t want to bruteforce accelerator keys manually, you can check the `cfg/idagui.cfg` file which describes IDA’s default menu layout and all assigned accelerators (prefixed with &). 

### Dialog box navigation

In addition to OK/Cancel buttons, many of IDA’s dialog boxes have checkboxes, radio buttons or edit fields. You can use the standard `Tab` key to navigate between them and `Space` bar to toggle, however, similarly to the menus, most dialog box controls in IDA have accelerator shortcuts. You can use `Alt` on Windows to reveal them but, unlike menus, they work even _without_ `Alt`. For example. to quickly exit IDA discarding any changes made since opening the database, use this key sequence: 

  * `Alt`–`X` (or `Alt`–`F4`) to show the “Save database” dialog
  * `D` to toggle the “DON’T SAVE the database” checkbox
  * `Enter` or `Alt`–`K` (or `K`) to confirm (OK)



![dialog exit](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dlg_exit-3.png?width=257&height=286&name=dlg_exit-3.png)

NOTE: a few dialogs are excluded from this feature, for example the Options-General… dialog, also Script Command (`Shift`–`F2`) or other dialogs with a text edit box. In such dialogs you have to hold down `Alt` to use accelerators.

---

