# Hex-Rays Blog – Page 30

**Archived:** 2026-02-20 12:19
**URL:** https://hex-rays.com/blog/page/30

---

## 1. 

**Date:** Posted: May 28, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader](https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #41: Binary file loader

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader>)

I 

Igor Skochinsky ✦ Posted: May 28, 2021

![Igor’s tip of the week #41: Binary file loader](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

IDA supports more than 40 file formats out of box. Most of them are structured file formats – with defined headers and metadata – so they’re recognized and handled automatically by IDA. However, there are times when all you have is just a piece of a code without any headers (e.g. shellcode or raw firmware) which you want to analyze in IDA. In that case, you can use the binary loader. It is always available even if the file is recognized as another file format.

### Processor selection

Since raw binaries do not have metadata, IDA does not know which processor module to use for it, so you should pick the correct one. By default, the **metapc** (responsible for x86 and x64 disassembly) is selected, but you can choose another one from the list (double-click to change).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/binfile_default-3.png?width=723&height=452&name=binfile_default-3.png)

### Memory loading address

Without metadata, IDA also does not know at which address to place the loaded data, so you may need to help it. The _Loading segment_ and _Loading offset_ fields are valid for the x86 family only. If the code being loaded uses a flat memory model (such as 32-bit protected mode or 64-bit long mode), Loading segment should be left at 0 and the address specified in the Loading offset field.

Other processors such as ARM, MIPS, or PPC, do not use these fields but prompt for memory layout after you confirm the initial selection.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/binfile_memory-3.png?width=362&height=501&name=binfile_memory-3.png)

In this dialog you can specify where to place the data and whether to create an additional RAM section. By default the whole file is placed at address 0 in the ROM segment but you can specify a different one or load only a part of the file by changing the file offset and loading size.

### Code bitness

For processors where instruction decoding changes depending on current mode, such as PC (16-bit mode, 32-bit protected mode, or 64-bit long mode) or ARM (AArch32 or AArch64), you may get one more additional question.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/binfile_bitness-3.png?width=378&height=158&name=binfile_bitness-3.png)

### Start disassembling

Finally, the file is loaded, but IDA can’t decide how to disassemble it on its own.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/binfile_decoding-3.png?width=350&height=257&name=binfile_decoding-3.png)

As suggested by the dialog, you can use `C` (make code) to try decoding at locations which look like valid instructions. Typically, shellcode will have valid instructions at the beginning, and firmware for most processors either starts at the lowest address or uses a vector table (a list of addresses) pointing to code.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/binfile_shellcode1-e1622214211102-3.png?width=410&height=197&name=binfile_shellcode1-e1622214211102-3.png)  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/binfile_shellcode2-3.png?width=403&height=245&name=binfile_shellcode2-3.png)

In addition to shellcode or firmware, the binary file loader can be used to analyze other kinds of files using IDA’s powerful features for marking up and labeling data and code. For example, here’s a PNG file labeled and commented in IDA:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/binfile_png-3.png?width=515&height=284&name=binfile_png-3.png)

---

## 2. 

**Date:** Posted: May 21, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics](https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #40: Decompiler basics

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics>)

I 

Igor Skochinsky ✦ Posted: May 21, 2021

![Igor’s tip of the week #40: Decompiler basics](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

The Hex-Rays decompiler is one of the most powerful add-ons available for IDA. While it’s quite intuitive once you get used to it, it may be non-obvious how to start using it.

### Basic information

As of the time of writing (May 2021), the decompiler is _not_ included with the standard IDA Pro license; some editions of IDA Home and IDA Free include a [cloud decompiler](<https://www.hex-rays.com/products/idahome/ida-home-cloud-based-decompilers-beta-testing/>), but the offline version requires IDA Pro and must be purchased separately.  
The following decompilers are currently available:

  * x86 (32-bit)
  * x64 (64-bit)
  * ARM (32-bit)
  * ARM64 (64-bit)
  * PPC (32-bit)
  * PPC64 (64-bit)
  * MIPS (32-bit)



### Pick the matching IDA

The decompiler must be used with the matching IDA: 32-bit decompilers only work with 32-bit IDA (e.g. `ida.exe`) while 64-bit ones require `ida64`. If you open a 32-binary in IDA64 and press `F5`, you’ll get a warning:

![Warning: Please use ida \(not ida64\) to decompile the current file](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_ida64warn-Jun-18-2024-08-57-52-9836-AM.png?width=361&height=124&name=hr_ida64warn-Jun-18-2024-08-57-52-9836-AM.png)

If you try to decompile a file for which you do not have a decompiler, a different error is displayed:

![Warning: Sorry, you do not have a decompiler for the current file. You can decompile code for the following processor\(s\): ARM64, ARM, PPC, x64](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_warn2-Jun-18-2024-08-57-55-0638-AM.png?width=367&height=143&name=hr_warn2-Jun-18-2024-08-57-55-0638-AM.png)

### Invoking the decompiler

The decompiler can be invoked in the following ways:

  1. View > Open subviews > Generate pseudocode (or simply `F5`). This always opens a new pseudocode view (up to 26);
  2. `Tab` switches to the last active pseudocode view and decompiles current function. If there are none, a new view is opened just like with `F5`.  
Tab can also be used to switch from pseudocode back to the disassembly. Whenever possible, it tries to jump to the corresponding location in the other view.
  3. Full decompilation of the whole database can be requested via File > Produce file > Create C file… (hotkey `Ctrl`+`F5`). This command decompiles selected or all functions in the database (besides those marked as library functions) and writes the result to a text file.



### Changing options

Because of its origins as a standalone plugin, the decompiler’s options are not currently present in the Options menu but are accessed via Edit > Plugins > Hex-Rays Decompiler.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options-3.png?width=505&height=395&name=hr_options-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options0-Jun-18-2024-08-57-54-3676-AM.png?width=497&height=357&name=hr_options0-Jun-18-2024-08-57-54-3676-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options1-Jun-18-2024-08-57-54-0043-AM.png?width=620&height=461&name=hr_options1-Jun-18-2024-08-57-54-0043-AM.png)

This dialog changes options for the current database. To change them for all future files, edit `cfg/hexrays.cfg`. Instead of editing the file in IDA’s directory, you can create one with only changed options in the [user directory](<https://www.hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>). The available options are explained [in the manual](<https://www.hex-rays.com/products/decompiler/manual/config.shtml>).

---

## 3. 

**Date:** Posted: May 20, 2021  
**Link:** [https://hex-rays.com/blog/ida-celebrating-30-years-of-binary-analysis-innovation](https://hex-rays.com/blog/ida-celebrating-30-years-of-binary-analysis-innovation)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [history](<https://hex-rays.com/blog/tag/history>)

# IDA: celebrating 30 years of binary analysis innovation

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-celebrating-30-years-of-binary-analysis-innovation>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-celebrating-30-years-of-binary-analysis-innovation>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-celebrating-30-years-of-binary-analysis-innovation>)

Geoffrey Czokow ✦ Posted: May 20, 2021

![IDA: celebrating 30 years of binary analysis innovation](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida304-1024x576-3.png?width=650&name=ida304-1024x576-3.png)

Today, IDA turns thirty years old. In commemoration of the anniversary we’ll describe the beginnings and major milestones of the epic journey.

## Background

In the early 1990’s, DOS was the most popular OS for PCs which were majorly 8086 with occasional 80286 (80386 was still very expensive). Typical PC had at most 1MB of RAM leaving little space for intensive tasks. However, software development industry was growing quickly and there was a need for debugging and diagnostic tools. Aside from debuggers, disassemblers were mostly batch based (non-interactive). The most popular (and expensive) one was _Sourcer_ by V Communications. It had limited interactivity in that it accepted a “definition file” with a list of starting disassembly points, possible function names and segmentation info. After each change to the definition file the disassembly of the whole file had to be redone from scratch which could take a long time on the machines available at the time. Most of the runtime data was kept in memory (at most 640KB in DOS) so it could fail on big files.

There were some debuggers which could be used for disassembly but they did not really offer RE features such as custom names or comments so deep RE was often done in a text editor or by marking up printouts.

IDA offered a new paradigm. It could disassemble a file piece by piece, loading only the fragment which the user was looking at, and did not need to load the whole file into memory. Renaming and commenting was done “just in time” instead of redoing the whole disassembly on every change. The database saved all changes so the work could be performed incrementally over time. However, it took time for this approach to be appreciated by the users.

### Ilfak Guilfanov (from IDA 2.05 history file, circa 1994?):

> First idea about IDA was born in the fall of 1990. It took half an year to screw up enough courage and to start implementing it. In the beginning of 1991, in January, first code line was written. In April 1991 the first program was fully disassembled with IDA. IDA grew up and new ideas appeared. I wanted to create a built-in C-style language to control analysis of the program, to add more processors, to disassemble object files, to handle UNIX COFF files, to add more intelligence to IDA e t c…
> 
> Alas, all of this was not implemented. In July 1991 I stopped working at IDA almost completely, working at IDA only for fun. It was time to learn more about other computers, networks and other nice things. Today I would implement something based on client-server architecture with network support (I have a crazy idea about X-windows implementation) working under various operating systems – but I won’t. Enough for the moment. I really think that disassemblers and all the staff like this are becoming obsolete. People work with GUIs, write in C++ (IDA is written in C++ too, about 40000 lines); they adore VisualBasic and they debug in source codes. Today’s programmer even doesn’t know assembler language – and doesn’t need to know it.
> 
> But…
> 
> I hope that this product will be a help for you. If so, I’m glad. Hope, there are some people who need a tool like this. And if there is a need to add a new processor type to IDA (the same was with Intel 8085), I can do it fast enough.

As we all know, disassemblers did not (yet?) get obsolete. Most of the planned features did get added to IDA eventually, not in the least thanks to the users who supported IDA during the early years and spread word about IDA, but also thanks to the early distributors and supporters such as DataRescue.

### Pierre Vandevenne (DataRescue CEO), 2003:

> When I discovered IDA, it was $30. I knew how to recognize a good deal and walked to my bank in the middle of the night to drop the wire order in their mailbox (pre-internet age stuff). Very very few, an unbelievably small number of people, did the same thing at that time.
> 
> […]
> 
> Version 2.05 (which is the one I registered) was developed by Ilfak.
> 
> We starting distributing, supporting the development and advertising version 3.05 (which essentially was very close to 2.05). Then Ilfak joined our company, moved to Belgium and the GUI version saw the light of the day.

By 2008, the first commercial decompiler has been released, IDA’s development moved to a separate company, and first commercial plugins for IDA appeared. By now it is evident that binary analysis is far from a dying field and we hope that IDA will stay around to celebrate more anniversaries. And of course, we don’t plan to stop innovating.

## The complete timeline

Late 1990 

development starts   
(one of the core source files mentions being created on 25-Oct-1990) 

1991 

**IDA 0.1** (The program banner says “May 20” but it seems the actual release happened a day later) 

Archive

[ida01.zip](</wp-content/uploads/2021/05/ida01.zip>)
    
    
      Length     Date   Time    Name
     --------    ----   ----    ----
        24708  18-02-91 18:55   COMPRESS.EXE
        11451  21-05-91 22:21   COMTYPES.DOC
        76048  21-05-91 22:24   IDA.EXE
        57344  21-05-91 22:23   IDA.INT
         3581  06-05-91 17:36   IDAE.DOC
         3795  06-05-91 16:22   IDAR.DOC
         5976  27-05-91 18:17   README
        25080  18-02-91 19:07   REPAIR.EXE
     --------                   -------
       207983                   8 files

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida01d-150x150-3.png?width=150&height=150&name=ida01d-150x150-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/ida01d-3.png>) [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida01h-150x150-3.png?width=150&height=150&name=ida01h-150x150-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/ida01h-3.png>)

May 22: Windows 3.0 released by Microsoft ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/windows3-256x300-Jun-18-2024-08-58-04-4885-AM.png?width=100&name=windows3-256x300-Jun-18-2024-08-58-04-4885-AM.png) September 17: Linux release announcement by Linus Torvalds ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/linux-300x97-Jun-18-2024-08-58-20-1680-AM.png?width=100&name=linux-300x97-Jun-18-2024-08-58-20-1680-AM.png)

1993 

**IDA 1.8** (16/09/93) 

  * Turbo Vision instead of custom UI



1994 

**IDA 2.0**

  * IDC scripting language added
  * start of shareware distribution (mainly via FidoNet and BBS, some FTPs)
  * support for additional processors (8080, 8085, Z80)
  * support for the NE file format (16-bit Windows and later OS/2)



1994 

“Reverse Compilation Techniques” thesis by Cristina Cifuentes, [dcc decompiler](<http://program-transformation.org/Transform/DccDecompiler>)

1995 

August 15: Windows 95 released ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/2880px-Windows_95-300x63-Jun-18-2024-08-58-05-2387-AM.png?width=150&name=2880px-Windows_95-300x63-Jun-18-2024-08-58-05-2387-AM.png)

1996 

**IDA 3.6**

  * [FLIRT Standard Run-time Library Recognition](</products/ida/tech/flirt/in_depth>)
  * Win32 console version



[DataRescue starts distributing IDA in Europe](<http://www.datarescue.com/idabase/>)   
[CSO starts distributing IDA in North America](<https://groups.google.com/g/comp.os.ms-windows.programmer.vxd/c/nTxVQmze2KM/m/bRmXQP38D8kJ>)

1999 

**IDA 3.84** (07/03/99) 

  * Plugins support added in the SDK

**IDA 4.0** (21/09/99) 

  * Windows GUI version (text mode listing only). First appearance of now-classic IDA icon.

![IDA Pro by Ilfak Guilfanov](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida4_banner-3.png?width=434&height=129&name=ida4_banner-3.png)

2000 

**IDA 4.10** (19/06/2000) 

  * Type System (standard function prototypes)
  * PIT (parameter identification and tracking)



2001 

**IDA 4.17** (22/03/2001) 

  * Graphs and flowcharts using Wingraph



[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/4_17normal_view-150x150-3.gif?width=150&height=150&name=4_17normal_view-150x150-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/4_17normal_view-3.gif>) [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/4_17carthesian_fish_eye-150x150-3.gif?width=150&height=150&name=4_17carthesian_fish_eye-150x150-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/4_17carthesian_fish_eye-3.gif>) [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/4_17polar_fish_eye-150x150-3.gif?width=150&height=150&name=4_17polar_fish_eye-150x150-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/4_17polar_fish_eye-3.gif>)

[experiments with microcode](<http://www.datarescue.com/laboratory/vd.htm>)

2002 

April: Boomerang decompiler development starts <http://boomerang.sourceforge.net/2004.php>   
June: Desquirr decompiler plug-in released <http://desquirr.sourceforge.net/>

2003 

January: user-contributed Windows PE debugger plugin (Idbg)

**IDA 4.5** (12/02/03) 

  * Integrated debugger



[![IDA Win32 debugger](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida450_dbg-150x150-3.png?width=150&height=150&name=ida450_dbg-150x150-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/ida450_dbg-3.png>)

**IDA 4.6** (27/10/03) 

  * 64-bit address space support; AMD64 disassembly



May: first decompiler results on a real life trojan <http://www.datarescue.com/laboratory/vd2.htm>   
  


2004 

**IDA 4.7** (30/08/04) 

  * support for fragmented (chunked) functions
  * Linux console version
  * remote cross-patform debugging

**IDAPython 0.5.0** (07/08/04) released by Gergely Erdelyi   
  


September: IDAPython [presented at the Virus Bulletin conference](<https://archive.f-secure.com/weblog/archives/carrera_erdelyi_VB2004.pdf>)

2005 

**IDA 4.8** (15/03/05) 

  * 64-bit remote debugger



Hex-Rays SA is registered.   
Ilfak starts posting on hexblog.com   
December 14: WMF vulnerability [zero day attack](<https://www.lastwatchdog.com/selling-fake-antivirus-start/>)   
December 31: Ilfak’s [unofficial vulnerability hotfix](<https://www.hex-rays.com/blog/windows-wmf-metafile-vulnerability-hotfix/>) becomes very popular 

2006 

**IDA 5.0** (03/06) 

  * the built-in graph view



[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida50_graph-150x150-3.png?width=150&height=150&name=ida50_graph-150x150-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/ida50_graph-3.png>)

2007 

**IDA 5.1**

  * Intel Mac OS X debugger
  * Simplex based stack pointer analysis <https://www.hex-rays.com/blog/simplex-method-in-ida-pro>

**Hex-Rays decompiler beta** testing opens (11/05/07)   
**Hex-Rays Decompiler 1.0** (17/09/07) released   
**Hex-Rays Decompiler SDK** (25/10/07) released   
  


August 8: www.hex-rays.com opens 

2008 

January 1: Hex-Rays SA takes over development of IDA

**IDA 5.3**

  * Multithreaded debugging
  * iPhone, Symbian debuggers

**IDAPython 1.0.0** released 

  * development moves to [Google Code](<https://code.google.com/archive/p/idapython/>)



2009 

**IDA 5.4**

  * Bochs, GDB, WinDbg debuggers
  * IDAPython included with IDA

**IDA 5.5**

  * dockable windows
  * arrival of the now classsic IDA layout with the functions list to the left of disassembly

**IDA 5.6**

  * IDAPython support for Linux and Mac
  * 64-bit Linux and Mac debuggers
  * ARM Linux remote debugger
  * Appcall feature



2010 

**IDA 5.7**

  * Scripted plugins and processor modules
  * ARM decompiler

**IDA 6.0**

  * cross-platform Qt based GUI version for Windows, Linux & Mac

![](https://hex-rays.com/hubfs/Imported_Blog_Media/ida60_about-e1621512599522-3.png)

2011 

[Bug bounty program](</bugbounty/>) opens. First submissions and fixes for 6.0 and 5.7 

2012 

**IDA 6.3**

  * source-level debugging



2013 

**IDA 6.4**

  * ARM64 disassembly



2014 

**IDA 6.6**

  * x64 decompiler

**IDA 6.7**

  * Python bindings for the decompiler SDK



2015 

**IDA 6.9**

  * ARM64 decompiler
  * ARM64 Android debugger



2016 

**IDA 6.95**

  * iPhone debugger using official Apple debugserver
  * PPC decompiler



2017 

**IDA 7.0**

  * IDA is a native 64-bit executable for all platforms



2018 

**IDA 7.1**

  * decompiler microcode API opened

**IDA 7.2**

  * [Lumina](<https://hex-rays.com/products/ida/lumina>) function database



2019 

**IDA 7.3**

  * PPC64 decompiler
  * UNDO feature

**IDA 7.4**

  * Python 3 support



2020 

**IDA 7.5**

  * folder views
  * MIPS decompiler



2021 

**IDA 7.6**

  * native ARM64 macOS build



See also [detailed IDA changelists](<https://hex-rays.com/products/ida/news>)

Downloads

[ida01.zip](</wp-content/uploads/2021/05/ida01.zip>)

---

## 4. 

**Date:** Posted: May 14, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data](https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #39: Export Data

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data>)

I 

Igor Skochinsky ✦ Posted: May 14, 2021

![Igor’s tip of the week #39: Export Data](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

The Edit > Export Data command ( `Shift`\+ `E`) offers you several formats for extracting the selected data from the database:   


  * hex string (unspaced): `4142434400`
  * hex string (spaced): `41 42 43 44 00`
  * string literal: ABCD
  * C unsigned char array (hex): `unsigned char aAbcd[] = { 0x41, 0x42, 0x43, 0x44, 0x00 };`
  * C unsigned char array (decimal): `unsigned char aAbcd[] = { 65, 66, 67, 68, 0 };`
  * initialized C variable: `struc_40D09B test = { 16961, 17475 };` NB: this option is valid only in some cases, such as for structure instances or items with type information.
  * raw bytes [can be only saved to file]

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/export_data2-Jun-18-2024-09-05-11-9631-AM.png?width=501&height=593&name=export_data2-Jun-18-2024-09-05-11-9631-AM.png) Data in the selected format is shown in the preview text box which can be copied to the clipboard or saved to a file for further processing.

---

## 5. 

**Date:** Posted: May 10, 2021  
**Link:** [https://hex-rays.com/blog/announcing-version-7-6-for-ida-freeware](https://hex-rays.com/blog/announcing-version-7-6-for-ida-freeware)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Announcing version 7.6 for IDA Freeware!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/announcing-version-7-6-for-ida-freeware>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/announcing-version-7-6-for-ida-freeware>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/announcing-version-7-6-for-ida-freeware>)

Geoffrey Czokow ✦ Posted: May 10, 2021

![Announcing version 7.6 for IDA Freeware!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Hex-Rays is excited to announce that IDA Freeware has been upgraded to the latest IDA version (7.6), and now includes a cloud-based decompiler!

IDA Freeware is the free version of IDA Pro, introduced to provide individual users¹ with an opportunity to see IDA in action, supporting disassembly of x86 and x64 binaries. It is the go-to tool for anyone who wants to kickstart their reverse engineering experience!

### Highlights:

  * bumped version from 7.0 to 7.6, and therefore benefits from all the great additions that appeared in the meantime: 
    * dark mode
    * support for classifying data in folders
    * golang support
    * Apple Silicon support
    * faster rebasing
    * optional synchronized disassemly & decompilation
  * For the first time, IDA Freeware ships with a cloud-based, free decompiler for x64 binaries (the decompiler requires an internet connection, and lacks some of the more advanced features that the commercial decompiler provides.)



[Download now](<https://www.hex-rays.com/ida-free/#download>)

Learn more about [**IDA Freeware**](<https://www.hex-rays.com/ida-free/>) and see the [**main differences between our range of products**](<https://www.hex-rays.com/ida-pro/#main-differences-between-ida-editions>): IDA Pro, IDA Home, IDA Freeware.

1\. Work can be saved, but commercial use is not allowed

---

## 6. 

**Date:** Posted: May 7, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view](https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #38: Hex view

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view>)

I 

Igor Skochinsky ✦ Posted: May 7, 2021

![Igor’s tip of the week #38: Hex view](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In addition to the disassembly and decompilation (Pseudocode) views, IDA also allows you to see the actual, raw bytes behind the program’s instructions and data. This is possible using the Hex view, one of the views opened by default (or available in the View > Open subviews menu). ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_tab-Jun-18-2024-09-03-55-0733-AM.png?width=874&height=102&name=hexview_tab-Jun-18-2024-09-03-55-0733-AM.png) Even if you’ve used it before, there may be features you are not aware of. 

### Synchronization

Hex view can be synchronized with the disassembly view (IDA View) or Pseudocode (decompiler) view. This option is available in the context menu under “Synchronize with”. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_sync-3.png?width=294&height=187&name=hexview_sync-3.png) Synchronization can also be enabled or disabled in the opposite direction (i.e. from IDA View or Pseudocode window). When it is on, the views’ cursors move in lockstep: changing the position in one view updates it in the other. 

### Highlight

There are two types of highlight available in the Hex view. 

  1. the text match highlight is similar to the one we’ve seen [in the disassembly listing](<https://www.hex-rays.com/blog/igor-tip-of-the-week-05-highlight/>) and shows matches of the selected text anywhere on the screen. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_highlight-Jun-18-2024-09-03-55-5469-AM.png?width=676&height=112&name=hexview_highlight-Jun-18-2024-09-03-55-5469-AM.png)
  2. current item highlight shows the group of bytes that constitutes the current _item_ (i.e. an instruction or a piece of data). This can be an alternative way to track the instruction’s opcode bytes instead of [the disassembly option](<https://www.hex-rays.com/blog/igors-tip-of-the-week-26-disassembly-options-2/>). ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_highlight2-Jun-18-2024-09-03-58-7447-AM.png?width=745&height=87&name=hexview_highlight2-Jun-18-2024-09-03-58-7447-AM.png)



### Layout and data format

The default settings use the classic 16-byte lines with text on the right. You can change the format of individual items as well as the amount of items per line (either a fixed count or auto-fit). ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_format-Jun-18-2024-09-03-49-7869-AM.png?width=372&height=303&name=hexview_format-Jun-18-2024-09-03-49-7869-AM.png) ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_cols-3.png?width=262&height=210&name=hexview_cols-3.png)

### Text options

Text area at the right of the hex dump can be hidden or switched to another encoding if necessary. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_text-Jun-18-2024-09-03-49-4758-AM.png?width=367&height=206&name=hexview_text-Jun-18-2024-09-03-49-4758-AM.png)

### Editing (patching)

Hex view can be used as an alternative to the [Patch program menu](<https://www.hex-rays.com/blog/igors-tip-of-the-week-37-patching/>). To start patching, simply press `F2,` enter new values and press `F2` again to commit changes ( `Esc` to cancel editing). An additional advantage is that you can edit values in their native format (e.g. decimal or floating-point), or type text in the text area. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_patch-Jun-18-2024-09-03-56-8370-AM.png?width=447&height=212&name=hexview_patch-Jun-18-2024-09-03-56-8370-AM.png)

### Debugging

Default debugging desktop has two Hex Views, one for a generic memory view and one for the stack view (synchronized to the stack pointer). Both are variants of the standard hex view and so the above-described functionality is available but there are a few additional features available only during debugging: 

  1. Synchronization is possible not only with other views but also with a value of a register. Whenever the register changes, the position in the hex view will be updated to match (as long as it is a valid address). ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_syncdbg-Jun-18-2024-09-03-55-9038-AM.png?width=325&height=379&name=hexview_syncdbg-Jun-18-2024-09-03-55-9038-AM.png)
  2. A new command in the disassembly view’s context menu allows to open a hex view at the address of the operand under cursor. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexview_dbgjump-Jun-18-2024-09-03-51-2722-AM.png?width=411&height=370&name=hexview_dbgjump-Jun-18-2024-09-03-51-2722-AM.png)

---

## 7. 

**Date:** Posted: May 6, 2021  
**Link:** [https://hex-rays.com/blog/hello-world-from-the-new-hex-rays](https://hex-rays.com/blog/hello-world-from-the-new-hex-rays)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hello world from the new Hex-Rays!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hello-world-from-the-new-hex-rays>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hello-world-from-the-new-hex-rays>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hello-world-from-the-new-hex-rays>)

Geoffrey Czokow ✦ Posted: May 6, 2021

![Hello world from the new Hex-Rays!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Today, Hex-Rays announces its new corporate identity as part of IDA’s 30th anniversary celebration. This new look reflects a key milestone of Hex-Rays team in its journey to relentlessly evolve and move forward.

After three decades of hard work to develop and improve the best-of-breed binary analysis tools which brought our flagship product – IDA Pro – into the spotlight, we are excited to have this makeover as it helps capture what we stand for as a team, a company and more than that, a visual identity that reflects our journey of transformation. Join us to welcome this remarkable update and read on to see what’s been changed!

### Highlights:

  * **The new Hex-Rays logo** : we are definitely in love with it! Totally different from the previous ones, this new logo combines simple and clean typography with simplified and slick shapes that perfectly reflect the sense of maturity and modernity. It symbolizes the next stage of our journey: keep evolving and striving to equip our users with the most powerful binary analysis tools. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hex-rays-logo-quadri-3.png?width=450&name=hex-rays-logo-quadri-3.png)
  * **The new color palette** : The new logo aroused our gut feeling to introduce a new color palette. We tweaked the blue that has always been with Hex-Rays identity, made it more distinct and adopted the purple and orange accent to obtain the boldness we’re aiming for. This new palette, we believe, is more refined, distinctive, energetic and helps Hex-Rays stand out from the rest. [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colorpalette-1-Jun-18-2024-08-48-14-9161-AM.png?width=550&height=283&name=colorpalette-1-Jun-18-2024-08-48-14-9161-AM.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/colorpalettefull-3.png>)
  * **Brand new website** : We revamped our Hex-Rays website to fully align with the new brand identity, and, more importantly, provide our visitors a better experience when learning about Hex-Rays and our products. Make sure you tour around and enjoy these latest changes: 
    * The website’s better **user-experience** (UX) is the second most apparent thing after its modern and clean appearance. Experience a faster, easier-to-navigate and more user-friendly website with better access to our products, solutions and support center. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/home03-Jun-18-2024-08-48-15-6370-AM.png?width=548&height=320&name=home03-Jun-18-2024-08-48-15-6370-AM.png)
    * The not-to-be-missed **[Solutions page](<https://www.hex-rays.com/solutions>)** : We’ve been collecting a wide range of works and publications from IDA users and are glad to present them along with this new website launch. Head over to the page and see the collection of use cases where IDA Pro was showcased in complex cybersecurity workflows or critical problems in the software industry. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/solutions03-Jun-18-2024-08-48-12-7593-AM.png?width=550&height=294&name=solutions03-Jun-18-2024-08-48-12-7593-AM.png)
    * That most-wanted Support center has been reorganized: Resources including [**documentation**](<https://www.hex-rays.com/documentation/>), [**tutorials**](<https://www.hex-rays.com/tutorials/>), [**downloads**](<https://www.hex-rays.com/download-center/>) are now all centralized and categorized. We endeavor to provide our users and partners with the most useful and up-to-date product-wise information while easing your painful searching and surfing around. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/support-center-1-Jun-18-2024-08-48-15-2791-AM.png?width=550&height=415&name=support-center-1-Jun-18-2024-08-48-15-2791-AM.png)



In the upcoming months, we will be constantly upgrading other features in the website and keep updating our content with helpful blogs, up-to-date and insightful information about our Training, Plugin contest, newsletter and so on.

Ilfak Guilfanov, CEO of Hex-Rays, said “This update isn’t about changing who we are but more about refining and unifying as we are evolving and preparing for many changes ahead. Our new brand identity reflects our leading position in the field, it is clearer, simpler, more modern while still retains a positive connection with the company’s legacy”.

Hex-Rays team is happy to share the news with our valued users, customers, partners and hope you’ll enjoy this change as much as we did creating it. To the brand new look of Hex-Rays, and more to come!

_**TL; DR**_ : Today, Hex-Rays launches a new logo, and website too! Check them out at [**www.hex-rays.com**](<https://www.hex-rays.com>)

---

## 8. 

**Date:** Posted: Apr 30, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-37-patching](https://hex-rays.com/blog/igors-tip-of-the-week-37-patching)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #37: Patching

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-37-patching>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-37-patching>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-37-patching>)

I 

Igor Skochinsky ✦ Posted: Apr 30, 2021

![Igor’s tip of the week #37: Patching](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Although IDA is mostly intended to be used for static analysis, i.e. simply looking at unaltered binaries, there are times you do need to make some changes. For example, you can use it to fix up some obfuscated instructions to clean up the code flow or decompiler output, or change some constants used in the program.

### Patching bytes

Individual byte values can be patched via the Edit > Patch program > Change byte… command.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patch_menu-Jun-18-2024-08-40-53-4517-AM.png?width=499&height=137&name=patch_menu-Jun-18-2024-08-40-53-4517-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patch_bytes-Jun-18-2024-08-40-53-0040-AM.png?width=438&height=156&name=patch_bytes-Jun-18-2024-08-40-53-0040-AM.png)

You can change up to 16 bytes at a time but you don’t have to enter all sixteen – the remaining ones will remain unchanged.

### Assembling instructions

Edit > Patch program > Assemble… is available only for the x86 processor and currently only supports a subset of 32-bit x86 but it still may be useful in simple situations. For example, the `nop` instruction is the same in all processor mode so you can still use it to patch out unnecessary instructions.

### Patched bytes view 

Available either under Edit > Patch program or in View > Open subviews submenus, this list view shows the list of the patched locations in the database and allows you to revert changes in any of them.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patch_list-Jun-18-2024-08-40-52-7109-AM.png?width=487&height=165&name=patch_list-Jun-18-2024-08-40-52-7109-AM.png)

### Patching the input file

All the patch commands only affect the contents of the _database_. The input file always remains unaffected by any change in the database. But in the rare case when you do need to update the input file on disk, you can use Edit > Patch program > Apply patches to input file…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patch_input-300x170-Jun-18-2024-08-40-54-2970-AM.png?width=300&height=170&name=patch_input-300x170-Jun-18-2024-08-40-54-2970-AM.png)

### Creating a difference file

File > Produce file > Create DIF File… outputs a list of patched location into a simple text file which can then be used to patch the input file manually in a hex editor or using a third party tool.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patch_dif-300x117-Jun-18-2024-08-40-55-1401-AM.png?width=300&height=117&name=patch_dif-300x117-Jun-18-2024-08-40-55-1401-AM.png)

### Patching during debugging

During debugging, patching still does not affect the input file, however it does affect the _program memory_ if the location being patched belong to a currently mapped memory area. So you can, for example, change instructions or data to see how the program behaves in such situation.

### Third party solutions

If the basic patching features do not quite meet your requirements, you can try the following third party plugins:

  * [IDA Patcher](<https://github.com/iphelix/ida-patcher>) by Peter Kacherginsky, a submission to our [2014 plugin contest](<https://www.hex-rays.com/contests_details/contest2014/>)
  * [KeyPatch](<https://www.keystone-engine.org/keypatch/>) by the Keystone Engine project, a winner of the [2016 contest](<https://www.hex-rays.com/contests_details/contest2016/>)



See also: [IDA Help: Edit|Patch core submenu](<https://www.hex-rays.com/products/ida/support/idadoc/526.shtml>)

---

## 9. 

**Date:** Posted: Apr 28, 2021  
**Link:** [https://hex-rays.com/blog/ida-7-6-service-pack-1-released](https://hex-rays.com/blog/ida-7-6-service-pack-1-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.6 Service Pack 1 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-6-service-pack-1-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-6-service-pack-1-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-6-service-pack-1-released>)

Tra Tran ✦ Posted: Apr 28, 2021

![IDA 7.6 Service Pack 1 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Today, Hex-Rays announces the release of Service Pack 1 (SP1) for IDA 7.6.

We are glad to announce the release of IDA 7.6 Service Pack 1 today! This Service Pack is primarily a bug fix release for a few errors that might affect some users.

### How to request the new versions

As usual, the new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to support@hex-rays.com.

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist __](</products/ida/news/7_6sp1/>)[Get a quote __](</cgi-bin/quote.cgi>)

---

