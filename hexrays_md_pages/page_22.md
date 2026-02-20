# Hex-Rays Blog – Page 22

**Archived:** 2026-02-20 12:16
**URL:** https://hex-rays.com/blog/page/22

---

## 1. 

**Date:** Posted: Jul 7, 2022  
**Link:** [https://hex-rays.com/blog/vulnerability-fix-2022-07-07](https://hex-rays.com/blog/vulnerability-fix-2022-07-07)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [vulnfix](<https://hex-rays.com/blog/tag/vulnfix>)

# Vulnerability fix 2022-07-07

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/vulnerability-fix-2022-07-07>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/vulnerability-fix-2022-07-07>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/vulnerability-fix-2022-07-07>)

Geoffrey Czokow ✦ Posted: Jul 7, 2022

![Vulnerability fix 2022-07-07](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

A friendly heads-up to IDA users: we just published a vulnerability fix for a **potential double-free during DWARF parsing**.

Please grab it from [www.hex-rays.com/vulnfix/](<https://hex-rays.com/vulnfix/>) and replace the original files with those you will find in the archive.

---

## 2. 

**Date:** Posted: Jul 4, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-96-loading-additional-files](https://hex-rays.com/blog/igors-tip-of-the-week-96-loading-additional-files)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #96: Loading additional files

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-96-loading-additional-files>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-96-loading-additional-files>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-96-loading-additional-files>)

I 

Igor Skochinsky ✦ Posted: Jul 4, 2022

![Igor’s tip of the week #96: Loading additional files](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Although most of the time IDA is used to work on single, self-contained file (e.g. an executable, library, or a firmware image), this is not always the case. Sometimes the program may refer to or load additional files or data, and it may be useful to have that data in the database and analyze it together with the original file.

### Load Additional Binary File

For simple cases where you have a raw binary file with the contents you want to add to the database, you can use File > Load file > Additional binary file…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/loadbin1-3.png?width=909&height=296&name=loadbin1-3.png)

Please note that any file you select will be treated as raw binary, even for formats otherwise supported by IDA (e.g. PE/ELF/Mach-O). Once you select a file, IDA will show you the dialog to specify where exactly you want to load it:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/loadbin2-3.png?width=445&height=288&name=loadbin2-3.png)

_Loading segment_ and _Loading offset_ together specify the location where you want to load the file’s data. By default, IDA tries to pick the values which are located just after the end of the last segment of the database in such a way that the newly loaded data starts at offset 0 in the new segment. However, if you are working with **flat memory** layout binary such as the case with most of modern OSes, you should instead set the **segment** value to **0** and **offset** to the linear address where you need to have the data loaded.

_File offset in bytes_ and _Number of bytes_ specify what part of the file you need loaded. With the default values IDA will load the whole file from the beginning, but you can also change them to load only a part of it.

_Create segments_ is checked by default because in most cases the file is being loaded into a new address range which does not exist in the database. If you’ve already created a segment for the file’s data or plan to do it after loading the bytes, you can uncheck this checkbox.

See also:

[IDA Help: Load Additional Binary File](<https://www.hex-rays.com/products/ida/support/idadoc/1372.shtml>)

[Igor’s tip of the week #41: Binary file loader](<https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader/>)

[Several files in one IDB, part 3](<https://hex-rays.com/blog/several-files-in-one-idb-part-3/>)

---

## 3. 

**Date:** Posted: Jun 24, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets](https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #95: Offsets

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets>)

I 

Igor Skochinsky ✦ Posted: Jun 24, 2022

![Igor’s tip of the week #95: Offsets](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

As we’ve [mentioned before](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>), the same numerical value can be used represented in different ways even if it’s the same bit pattern on the binary level. One of the representations used in IDA is offset.

### Offsets

In IDA, an _offset_ is a numerical value which is used as an address (either directly or as part of an expression) to refer to another location in the program.

The term comes from the keyword used in MASM (Microsoft Assembler) to distinguish an address expression from a variable.

For example:
    
    
    mov eax, g_var1

Loads the value from the location `g_var1` into register `eax`. In C, this would be equivalent to using the variable’s value.

While 
    
    
    mov eax, offset g_var1

Loads the _address_ of the location `g_var1` into `eax`. In C, this would be equivalent to taking the variable’s address.

On the binary level, the second instruction is equivalent to moving of a simple integer, e.g.: 
    
    
    mov eax, 0x40002000

However, during analysis the offset form is obviously preferred, both for readability and because it allows you to see cross-references to variables and be able to quickly identify other places where the variable is used.

In general, distinguishing integer values used in instructions from addresses is impossible without whole program analysis or runtime tracing, but the majority of cases can be handled by relatively simple heuristics so usually IDA is able to recover offset expressions and add cross-references. However, in some cases they may fail or produce false positives so you may need to do it manually.

### Converting values to offsets

All options for converting to offsets are available under Edit > Operand type > Offset:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offset_menu-3.png?width=782&height=608&name=offset_menu-3.png)

In most modern, flat-memory model binaries such as ELF, PE, Mach-O, the first two commands are equivalent, so you can usually use shortcut `O` or `Ctrl`–`O`.

The most common/applicable options are also shown in the context (right-click) menu:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offset_ctx-3.png?width=582&height=265&name=offset_ctx-3.png)

### Fixing false positives

There may be cases when IDA’s heuristics convert a value to an offset when it’s not actually being used as one. One common example is bitwise operations done with values which happen to be in the range of the program’s address space, but it can also happen for data values or simple data movement, like on the below screenshot.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offset_fp1-3.png?width=552&height=92&name=offset_fp1-3.png)

In this example, IDA has converted the second operand of the `mov` instruction to an offset because it turned out to match a program address. However, we can see that it is being moved into a location returned by the call to `__errno` function. This is a common way compilers implement setting of the `errno` pseudo-variable (which can be thread-specific instead of a global), so obviously that operand should be a number and not an offset. Besides being a wrong representation, this also lead to bogus cross-references:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offset_fp2-3.png?width=875&height=193&name=offset_fp2-3.png)

You have the following options to fix the false positive:

  1. Press `O` or `Ctrl`–`O` to reset the “offset” attribute of the operand and let IDA show the default representation (hex). Note that the number will be printed in orange to hint that its value falls into the address space of the program, i.e. it is [suspicious](<https://hex-rays.com/blog/igors-tip-of-the-week-90-suspicious-operand-limits/>);
  2. Use `Q`/`#` (for hex), `H` (for decimal), or select the corresponding option from the context menu to explicitly mark the operand as a number and also avoid flagging it as suspicious;
  3. If you have created an enumeration to represent such numbers as symbolic constants, you can use the `M` shortcut or the context menu to convert it to a symbolic constant.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offset_fp3-3.png?width=797&height=318&name=offset_fp3-3.png)

See also:

[IDA Help: Edit|Operand types|Offset submenu](<https://www.hex-rays.com/products/ida/support/idadoc/1381.shtml>)

[IDA Help: Edit|Operand types|Number submenu](<https://www.hex-rays.com/products/ida/support/idadoc/1382.shtml>)

---

## 4. 

**Date:** Posted: Jun 23, 2022  
**Link:** [https://hex-rays.com/blog/2022-september-ida-training-session-registrations-are-now-open](https://hex-rays.com/blog/2022-september-ida-training-session-registrations-are-now-open)

[Back](<https://hex-rays.com/blog>)

[Training](<https://hex-rays.com/blog/tag/training>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [idatraining](<https://hex-rays.com/blog/tag/idatraining>)

# 2022 September IDA training session: Registrations are now open!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/2022-september-ida-training-session-registrations-are-now-open>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/2022-september-ida-training-session-registrations-are-now-open>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/2022-september-ida-training-session-registrations-are-now-open>)

Geoffrey Czokow ✦ Posted: Jun 23, 2022

![2022 September IDA training session: Registrations are now open!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

![Hex-Rays training icon](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/training-session-logo-1-3.png?width=150&name=training-session-logo-1-3.png)

The next [2022 IDA training course](</products/ida/training/>) will take place online 12–16 and 19-21 September 2022, CEST time.

  * **[standard training](</training/#standard>)** : (12-16 September) aims to teach standard knowledge about IDA by demonstrating its use to analyze binary programs on modern operating systems.
  * **[advanced training](</training/#programming>)** : (19-21 September) intended for experienced IDA users who want to take advantage of its open architecture by extending and improving it.



The training session offers its participants an opportunity to improve their analysis with IDA’s limitless capabilities with theoretical and practical courses. After each theoretical section hands-on exercises will be carried out so as to master thorough understanding of concepts and methods. Training material is always updated to include the latest additions to IDA.

Detailed information including course programs, cost and registration forms can be found in our dedicated [training page](</training/>). If needed, additional information may be requested by emailing our [sales](<mailto:sales@hex-rays.com>) team.

[Visit the training page](</training/>) [Book your seat](<https://hex-rays.com/training/hexrays_training_2022-09.pdf>)

---

## 5. 

**Date:** Posted: Jun 17, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-94-variable-sized-structures](https://hex-rays.com/blog/igors-tip-of-the-week-94-variable-sized-structures)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #94: Variable-sized structures

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-94-variable-sized-structures>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-94-variable-sized-structures>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-94-variable-sized-structures>)

I 

Igor Skochinsky ✦ Posted: Jun 17, 2022

![Igor’s tip of the week #94: Variable-sized structures](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Variable-sized structures is a construct used to handle binary structures of variable size with the advantage of compile-time type checking.

### In source code

Usually such structures use a layout similar to following:
    
    
    struct varsize_t
    {
     // some fixed fields at the start
     int id;
     size_t datalen;
     //[more fields]
     unsigned char data[];// variable part
    };
    

In other words, a fixed-layout part at the start and an array of unspecified size at the end.

Some compilers do not like `[]` syntax so `[0]` or even `[1]` may be used too. At runtime, the space for the structure is allocated using the full size, and the array can be accessed as if it had expected size. For example:
    
    
    struct varsize_t* allocvar(int id, void *data, size_t datalen);
    {
     size_t fullsize = sizeof(varsize_t)+datalen+1;
     struct varsize_t *var = (struct varsize_t*) malloc(fullsize);
     var->id = id;
     var->datalen = datalen;
     memcpy(var->data, data, datalen);
     var->data[datalen]=0;
     return var;
    }
    

Can such structs be handled by IDA? Yes, but there are some peculiarities you may need to be aware of.

### In the decompiler

In the decompiler everything is pretty simple: just add the struct using C syntax to [Local Types](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>) and use it for types of local variables and function arguments. The decompiler automatically detects accesses to the variable part and represents them accordingly.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/varstruct_decompiler-3.png?width=439&height=166&name=varstruct_decompiler-3.png)

### In disassembly

However, disassembly view is trickier. You can import the struct from Local Types to the IDB Structures, or create one manually by explicitly adding an array of 0 elements at the end:
    
    
    00000000 varsize_t struc ; (sizeof=0x8, align=0x4, copyof_1, variable size)
    00000000 id dd ?
    00000004 datalen dd ?
    00000008 data db 0 dup(?)
    00000008 varsize_t ends
    

But when you have instances of such structs in data area, using this definition only covers the fixed part. To extend the struct, use `*` (Create/resize array action) and specify the full size of the struct.

### Example

Recent Microsoft compilers add so-called “COFF group” info to the PE executables. It is currently not fully parsed by IDA but is labeled in the disassembly listing with the comment `IMAGE_DEBUG_TYPE_POGO`:

`.rdata:004199E4 ; Debug information (IMAGE_DEBUG_TYPE_POGO)`  
`.rdata:004199E4 dword_4199E4 dd 0 ; DATA XREF: .rdata:004196BC↑o`  
`.rdata:004199E8 dd 1000h, 25Fh, 7865742Eh, 74h, 1260h, 0BCh, 7865742Eh, 69642474h, 0`  
`.rdata:00419A0C dd 1320h, 11BE2h, 7865742Eh, 6E6D2474h, 0`  
`.rdata:00419A20 dd 12F10h, 12Ch, 7865742Eh, 782474h, 13040h, 164h, 7865742Eh, 64792474h`  
`.rdata:00419A20 dd 0`  
`.rdata:00419A44 dd 14000h, 11Ch, 6164692Eh, 35246174h, 0`  
`.rdata:00419A58 dd 1411Ch, 4, 6330302Eh, 6766h, 14120h, 4, 5452432Eh, 41435824h, 0`  
`.rdata:00419A7C dd 14124h, 4, 5452432Eh, 41435824h, 41h, 14128h, 1Ch, 5452432Eh, 55435824h`

On expanding the array or looking at the hex view, it becomes apparent that it stores info about the original section names of the executable, before they are merged by the linker. So it can be useful to format this info. It seems to consist of a list of following structures:
    
    
    struct section_info
    {
      int start; // RVA
      int size;
      char name[]; // zero-terminated
    };
    

The string is padded with zeroes if necessary to align each struct on a 4-byte boundary.

After creating a local type and importing the struct to IDB, we can undefine the array created by IDA and start creating struct instances in the area using Edit > Struct var… (`Alt`–`Q`). However, only the fixed part is covered by default:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/varstruct1-3.png?width=647&height=339&name=varstruct1-3.png)

To extend the struct, press `*` and enter full size. For example, the first one should be 14 (8 for the fixed part and 6 for “.text” and terminating zero), although you can also use the suggested 16:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/varstruct2-3.png?width=394&height=442&name=varstruct2-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/varstruct3-3.png?width=520&height=137&name=varstruct3-3.png)

Now the struct has correct size and covers the string but it is printed as hex bytes and not text. Why and how to fix it?

When IDA converts C type to assembly-level (IDB) struct, it only relies on the sizes of C types, because on the assembly level there is [no difference between a a byte and character](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>). Thus a char array is the same as a byte array. However, you can still apply additional representation flags to influence formatting of the structure. For example, you can go to the imported definition in Structures list and mark the `name` field as a string literal, either from context menu or by pressing `A`:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/struct_string-3.png?width=571&height=400&name=struct_string-3.png)

The field is now commented correspondingly and the data instances show the string as text:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/struct_string2-3.png?width=595&height=119&name=struct_string2-3.png)![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/varstruct4-3.png?width=627&height=87&name=varstruct4-3.png)

In fact, once you mark the field as string, newly declared instances will be automatically sized by IDA using the zero terminator.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/varstruct5-3.png?width=637&height=364&name=varstruct5-3.png)

See also:

[Variable Length Structures Tutorial](<https://docs.hex-rays.com/user-guide/disassembler/varstr-tutorial>)  
[IDA Help: Convert to array](<https://www.hex-rays.com/products/ida/support/idadoc/455.shtml>)  
[IDA Help: Assembler level and C level types](<https://www.hex-rays.com/products/ida/support/idadoc/1042.shtml>)  
[IDA Help: Structures window](<https://www.hex-rays.com/products/ida/support/idadoc/593.shtml>)

---

## 6. 

**Date:** Posted: Jun 16, 2022  
**Link:** [https://hex-rays.com/blog/what-is-qscripts](https://hex-rays.com/blog/what-is-qscripts)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# What is QScripts?

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/what-is-qscripts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/what-is-qscripts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/what-is-qscripts>)

Geoffrey Czokow ✦ Posted: Jun 16, 2022

![What is QScripts?](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

**This is a guest entry written by[Elias Bachaalany](<https://twitter.com/0xeb>). His views and opinions are his own, and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to him.**

_ida-qscripts_ or _QScripts_ is a productivity plugin for better/faster scripting and coding workflow/experience for IDA.

IDA provides two methods to script: script files and script snippets.

  * **Script snippets** (`Shift`–`F2`) is found under the “File/Script Command…” menu. One writes a snippets and presses `Alt`–`R` to run the script. If the focus is shifted away from that window, but that window is still open, you can press `Ctrl`–`Shift`–`X`” to execute the last focused snippet.  
This workflow is fast because you just press a hotkey and run your code.
  * **Script files** : The other way to script for IDA is to write script files and run them in IDA. This workflow is not ideal for quick prototyping because one must edit the script, save it, switch to IDA’s “Recent Scripts” window (`Alt`–`F7`) and double-click again on that script to re-run it.  
Requiring the user to switch back and forth between the IDE and IDA each time to test the new script is not fun and that’s where ida-qscripts comes into play.



For native programming (writing plugins using the C++ SDK), the development workflow is equally slow. You have to build your plugin, deploy it in the plugins folder, and often times restart IDA for your native plugin’s new version to be loaded.

With QScripts, all you need to do is activate a script and it will be monitored for changes. When the script (its dependencies or trigger file) is modified, then qscripts will automatically re-run the script for you.

## QScripts features and options

The following are some of the important features and options in QScripts:

![qscripts-options](https://hex-rays.com/hubfs/Imported_Blog_Media/qscripts-options-3.png)

  * Allow QScripts execution to be undo-able: this option allows you to undo the script’s side effects by pressing `Ctrl`–`Z` in IDA. This option is not enabled by default. Use this option if your plugin or script modifies the database and there’s possibility of incorrectly modifying or patching the IDB.
  * Script monitor interval: this adjusts the file modification time polling frequency of the scripts on disk. Unfortunately, because IDA SDK does not have file change notification APIs, we use basic timers to poll and query file modification times.
  * Clear the output window: Automatically issues a `msg_clear()` call before the script is executed.
  * [Dependencies](<https://github.com/0xeb/ida-qscripts#working-with-dependencies>) support: QScripts understands dependencies ([Python packages](<https://github.com/0xeb/ida-qscripts/blob/current/test_scripts/pkg-dependency/README.html>) for example). If your main script or any of its dependencies are changed, then everything is reloaded correctly. You do not need to restart IDA to have your other Python dependencies refreshed for example.
  * Custom trigger file: it is possible to specify and monitor a [trigger file](<https://github.com/0xeb/ida-qscripts#using-qscripts-with-trigger-files>). This file can be anything, when it changes, your script is re-run. This mechanism is used to support [native development](<https://github.com/0xeb/ida-qscripts#using-qscripts-with-compiled-code>) (C++ plugins).



## Installing QScripts

QScripts is an open-source project hosted at <https://github.com/0xeb/ida-qscripts>. Grab a build from the [releases page](<https://github.com/0xeb/ida-qscripts/releases/>) and place it in your plugins folder. Alternatively, just build it from sources.

# QScripts and script development

Install QScripts and open it with the default `Alt`–`Shift`–`F9` hotkey:

![QScripts main UI](https://hex-rays.com/hubfs/Imported_Blog_Media/qscripts-ui-1-3.png)

If the window is empty, it means that you never ran scripts before. Just press `Ins` to browse a script and load it. Double-click on the script to activate it. Below, we can see _[hello.py](<http://hello.py>)_ activated:

![Activated hello.py](https://hex-rays.com/hubfs/Imported_Blog_Media/qscripts-ui-active-3.png)

Any changes to this file will cause qscripts to run it automatically. Like this, you don’t have to leave your editor. The ideal setup is to have IDA and your editor side by side:

![IDA and Sublime Text editor side by side](https://hex-rays.com/hubfs/Imported_Blog_Media/qscripts-sxs-3.png)

To stop the script monitor, just select your script and press `Ctrl`–`D`:

![Context menu](https://hex-rays.com/hubfs/Imported_Blog_Media/qscripts-ctx-menu-3.png)

## Dependencies

If your scripts have dependencies, then you must create and configure an additional file with the same name and extension as your input script with the additional _.deps.qscripts_ suffix.

For example, _[hello.py](<http://hello.py>)_ ‘s dependency file should be called _hello.py.deps.qscripts_ :


/reload import importlib; importlib.reload($basename$) myotherscript.py 


This tells qscripts that _[hello.py](<http://hello.py>)_ depends on _[myotherscript.py](<http://myotherscript.py>)_. If _[myotherscript.py](<http://myotherscript.py>)_ also has its own _myotherscript.py.deps.qscripts_ then _[hello.py](<http://hello.py>)_ will inherit its dependent’s dependencies.

# QScripts and native plugins development

If you prefer to prototype in C++ using the full power of the IDA SDK, then qscripts also lets you stay in your favorite IDE and edit/build/run the plugin continuously. Check the video below for a demo.

Your browser does not support the video tag. 

First, let’s discuss some background information to explain how _hot-reloading_ works in IDA.

## Background

IDA plugins use a plugin descriptor structure called `plugin_t`. It has the `flags` field which can have the `PLUGIN_UNL` flag that allows _hot-reloading_ of the plugins in a edit/build/run fashion.


class plugin_t { public: int version; ///< Should be equal to #IDP_INTERFACE_VERSION int flags; ///< \ref PLUGIN_ /// \defgroup PLUGIN_ Plugin features /// Used by plugin_t::flags //@{ #define PLUGIN_MOD 0x0001 ///< Plugin changes the database. ///< IDA won’t call the plugin if ///< the processor module prohibited any changes. #define PLUGIN_DRAW 0x0002 ///< IDA should redraw everything after calling the plugin. #define PLUGIN_SEG 0x0004 ///< Plugin may be applied only if the current address belongs to a segment #define PLUGIN_UNL 0x0008 ///< Unload the plugin immediately after calling ‘run’. ///< This flag may be set anytime. ///< The kernel checks it after each call to ‘run’ ///< The main purpose of this flag is to ease ///< the debugging of new plugins. … 


When IDA runs, the plugin’s binary (`*.so, 64*.dll, *.dll`) will be loaded and held by IDA. Once you invoke the plugin the first time, its `run()` will be invoked, then IDA will unload the plugin. This will be your chance to re-compile the modified plugin before re-invoking it in IDA (from the menu) or programmatically via the `load_and_run_plugin()`.

## Using QScripts

Now that we know how _hot-reloading_ works in IDA, we can leverage qscripts and its trigger files feature to allow native plugins development and prototyping in a convenient manner.

To begin, create a simple Python script with the following contents:


# hello_cpp.py # Give the linker time to finish flushing the binary import time time.sleep( 1) # Load your plugin and pass any arg value you want idaapi.load_and_run_plugin( ‘YOUR_NATIVE_PLUGIN_NAME_GOES_HERE’, 0) 


Then also write its dependency file with a trigger file that is the actual compiled plugin binary. For instance:


; hello_cpp.py.deps.qscripts /triggerfile /keep $env:IDASDK$\bin\plugins\hello_cpp.dll 


Finally, activate _hello_cpp.py_ from qscripts.

Now, everytime the compiled plugin binary changes, the Python script will ask IDA to load and run it again!

## Native templates to work with

Conveniently, qscripts ships with a bunch of templates that lets you focus on the code rather than on the mechanics/plumbing code of a plugin:

  * plugin_template: this template hides the plugin plumbing code in _driver.cpp_ and exposes [main.cpp](<https://github.com/0xeb/ida-qscripts/blob/current/test_scripts/trigger-native/plugin_template/main.cpp>) where you implement your plugin logic in a simple function.
  * plugin_triton: this is a [template](<https://github.com/0xeb/ida-qscripts/blob/current/test_scripts/trigger-native/plugin_triton/main.cpp>) for writing a plugin that uses the [Triton DBA framework](<https://github.com/jonathansalwan/Triton>).
  * loader_template: this [template](<https://github.com/0xeb/ida-qscripts/blob/current/test_scripts/trigger-native/loader_template/main.cpp>) is a mock file loader module for IDA. It has a mock `load_file()` and `accept_file()` callbacks. Implement and quickly test these methods before you transfer this code to a real file loader module.



That’s all, happy and productive coding!

---

## 7. 

**Date:** Posted: Jun 10, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-93-com-reverse-engineering-and-com-helper](https://hex-rays.com/blog/igors-tip-of-the-week-93-com-reverse-engineering-and-com-helper)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #93: COM reverse engineering and COM Helper

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-93-com-reverse-engineering-and-com-helper>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-93-com-reverse-engineering-and-com-helper>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-93-com-reverse-engineering-and-com-helper>)

I 

Igor Skochinsky ✦ Posted: Jun 10, 2022

![Igor’s tip of the week #93: COM reverse engineering and COM Helper](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

COM aka Component Object Model is the technology used by Microsoft (and others) to create and use reusable software components in a manner independent from the specific language or vendor. It uses a stable and well-defined ABI which is mostly compatible with Microsoft C++ ABI, allowing easy implementation and usage of COM components in C++.

### COM basics

COM components and their interfaces are identified by [UUID aka GUID](<https://en.wikipedia.org/wiki/Universally_unique_identifier>) – unique 128-bit IDs usually represented as string of several hexadecimal number groups. For example, `{00000000-0000-0000-C000-000000000046}` represents `IUnknown` – the base interface which must be implemented by any COM-conforming component. 

Each COM interface provides a set of functions in a way similar to a C++ class. On the binary level, this is represented by a structure with function pointers, commonly named `<name>Vtbl`. For example, here’s how the `IUnknown` is laid out:
    
    
    struct IUnknownVtbl
    {
      HRESULT (__stdcall *QueryInterface)(IUnknown *This, const IID *const riid, void **ppvObject);
      ULONG (__stdcall *AddRef)(IUnknown *This);
      ULONG (__stdcall *Release)(IUnknown *This);
    };
    struct IUnknown
    {
      struct IUnknownVtbl *lpVtbl;
    };
    

IDA’s standard type libraries include most of the COM interfaces defined by the Windows SDKs, so you can import these structures from them. Here’s how to do it manually:

  1. Open the Structures window (`Shift`–`F9`);
  2. Use “Add struct type…” from the context menu, or `Ins`;
  3. Type the name of the interface and/or its vtable (e.g. `IUnknownVtbl`) and click OK. If the interface is known, it will be imported from the type library automatically. If you are not sure it is available, you can click “Add standard structure” and use incremental search (start typing the name) to check if it’s present in the list of available types.



Once imported, the struct can be used, for example, to label indirect calls performed using the interface pointer.

How to know which interface is being used in the code? There are multiple ways it can be done, but one common approach is to use the `[CoCreateInstance](<https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-cocreateinstance>)` API. It returns a pointer to the interface defined by the interface ID (IID) which is a kind of GUID. You can check what IID is used, then search for it in Windows SDK headers and hopefully find the interface name.

For example, consider this call:
    
    
    .text:30961A4D push    eax                             ; ppv
    .text:30961A4E push    offset riid                     ; riid
    .text:30961A53 push    1                               ; dwClsContext
    .text:30961A55 push    esi                             ; pUnkOuter
    .text:30961A56 push    offset rclsid                   ; rclsid
    .text:30961A5B mov     [ebp+ppv], esi
    .text:30961A5E call    ds:CoCreateInstance
    

If we follow `riid`, we can see that it’s been formatted by IDA nicely as an instance of the `IID` structure:
    
    
    .text:30961C18 riid dd 0EC5EC8A9h                           ; Data1
    .text:30961C18                                         ; DATA XREF: sub_30961A2E+20↑o
    .text:30961C18                                         ; sub_30DECD76+1D↓o
    .text:30961C18 dw 0C395h                               ; Data2
    .text:30961C18 dw 4314h                                ; Data3
    .text:30961C18 db 9Ch, 77h, 54h, 0D7h, 0A9h, 35h, 0FFh, 70h; Data4
    

In the text form, this corresponds to `EC5EC8A9-C395-4314-9C77-54D7A935FF70`, but since it’s quite awkward to convert from the struct representation, a quick way is to search for `EC5EC8A9` and see if you can find a match.

There is one in `wincodec.h`: 
    
    
        MIDL_INTERFACE("ec5ec8a9-c395-4314-9c77-54d7a935ff70")
        IWICImagingFactory : public IUnknown
        {
        public:
            virtual HRESULT STDMETHODCALLTYPE CreateDecoderFromFilename( 
                /* [in] */ __RPC__in LPCWSTR wzFilename,
                /* [unique][in] */ __RPC__in_opt const GUID *pguidVendor,
                /* [in] */ DWORD dwDesiredAccess,
                /* [in] */ WICDecodeOptions metadataOptions,
                /* [retval][out] */ __RPC__deref_out_opt IWICBitmapDecoder **ppIDecoder) = 0;
        [....]        
    

Now that we know we’re dealing with `IWICImagingFactory`, we can import `IWICImagingFactoryVtbl` and use it to label the calls made later by dereferencing the `ppv` variable:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/com_before-3.png?width=970&height=531&name=com_before-3.png)

IDA uses type information of the structure’s function pointer to label and [propagate argument information](<https://hex-rays.com/blog/igors-tip-of-the-week-74-parameter-identification-and-tracking-pit/>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/com_after-3.png?width=544&height=288&name=com_after-3.png)

While this process works, it is somewhat tedious and error prone. Is there something better?

### COM helper

IDA ships with a standard plugin which can automate some parts of the process. If you invoke Edit > Plugins > COM Helper, it shows a little help about what it does:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comhelper_info-3.png?width=369&height=203&name=comhelper_info-3.png)

Invoke the menu again to re-enable it. The default state is _on_ for new databases, so normally you do not need to do that. With plugin enabled, we can do the following:

  1. Undefine/delete the IID instance at `riid`.
  2. Redefine it as a GUID (Alt-Q, choose “GUID”).



If the GUID is known, the instance is renamed to `CLSID_<name>`, and the corresponding `<name>Vtbl` is imported into the database automatically (if available in loaded type libraries). You can then use it to resolve the indirect calls from the interface pointer.

### Extending the known interface list

To detect known GUIDs, on Windows the COM Helper uses the registry (`HKLM\Software\Classes\Interface`subtree). If the GUID is not found in registry (or not running on Windows), the file `cfg/clsid.cfg` in IDA’s install directory is consulted. It is a simple text file with the list of GUIDs and corresponding names. If you are dealing with lesser-known interfaces, you can add their GUIDs to this file so that they can be labeled nicely.

---

## 8. 

**Date:** Posted: Jun 3, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-92-address-details](https://hex-rays.com/blog/igors-tip-of-the-week-92-address-details)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #92: Address details

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-92-address-details>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-92-address-details>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-92-address-details>)

I 

Igor Skochinsky ✦ Posted: Jun 3, 2022

![Igor’s tip of the week #92: Address details](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

The address details pane is a rather recent addition to IDA so probably not many users are familiar with it yet. However, it can be a quite useful addition to the standard workflow, permitting you to perform some common tasks faster.

### Address details view

On invoking View > Open subview > Address details (you can also use the [Quick view](<https://hex-rays.com/blog/igors-tip-of-the-week-30-quick-views/>) selector), a new pane appears, by default on the right side of the main window. Obviously, it can be moved and docked elsewhere if you prefer. It will automatically update itself using the current address in any address-based view (IDA View, Hex View, Pseudocode).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/adetails1-3.png?width=1163&height=831&name=adetails1-3.png)

The pane consists of three sections, each of which can be collapsed and expanded using the triangle icon in the top left corner.

### Name section

This is basically a non-modal version of the standard Rename address dialog (`N` hotkey). It allows you to quickly rename locations by entering a new name in the edit box as well as view and change various name attributes.

### Flags section

This is an expanded version of the [Print internal flags](<https://hex-rays.com/blog/igors-tip-of-the-week-91-item-flags/>) action, however currently it does not provide details of instruction operands.

### Data inspector section

This section shows how the bytes at the current address can be interpreted in various formats: integers, floating-point values, or string literals. It can be especially useful when exploring unknown file formats or arrays of unknown data in programs.

---

## 9. 

**Date:** Posted: May 27, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-91-item-flags](https://hex-rays.com/blog/igors-tip-of-the-week-91-item-flags)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #91: Item flags

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-91-item-flags>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-91-item-flags>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-91-item-flags>)

I 

Igor Skochinsky ✦ Posted: May 27, 2022

![Igor’s tip of the week #91: Item flags](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

When changing [operand representation](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>), you may need to check what are the operand types currently used by IDA for a specific instruction. In some cases it is obvious (e.g. for offset or character type), but the hex and default, for example, look exactly the same in most processors so it’s not easy to tell them apart just by look. 

Internally, this information is stored by IDA in the _item flags_. To check the current flags of an instruction (or any other address) in the database, use View > Print internal flags (hotkey `F`) .

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/flags1-3.png?width=343&height=395&name=flags1-3.png)

Wen you invoke this action, IDA prints flags for the current address to the Output window. It only prints info about non-default operand types — the default ones are omitted (except for [suspicious operands](<https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables/>) which are printed as _void_).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/flags2-3.png?width=790&height=182&name=flags2-3.png)

_code_ and _flow_ are generic instruction flags: they mean that the current item is marked as code (instruction) and the execution reaches it from the previous address (this is the case for most instructions in the program). 

Whenever IDA prints information about the second operand (number 1 since they are counted from 0), the operands 2,3…6 (even if they do not actually exist) are also printed as having the same type. This happens because of a limitation in IDA: it originally supported user-specified representation only for two operands (0 and 1) and this limitation is not completely lifted yet as of IDA 7.7.

Besides operand types, the feature may show other low-level info about the current address: for example, the type information if it’s set for current location, the function arguments layout similarly to what you can see in [decompiler annotations](<https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations/>), structure name for structure data items, and so on.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/flags3-3.png?width=398&height=115&name=flags3-3.png)

---

