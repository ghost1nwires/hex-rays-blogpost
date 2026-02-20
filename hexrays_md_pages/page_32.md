# Hex-Rays Blog – Page 32

**Archived:** 2026-02-20 12:20
**URL:** https://hex-rays.com/blog/page/32

---

## 1. 

**Date:** Posted: Mar 19, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing](https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #31: Hiding and Collapsing

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing>)

I 

Igor Skochinsky ✦ Posted: Mar 19, 2021

![Igor’s tip of the week #31: Hiding and Collapsing](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

You may have come across the menu items View > Hide, Unhide but possibly never used them.

These commands allow you to hide, or _collapse_ and unhide/uncollapse parts of IDA’s output. They can be used in the following situations:

### Hiding instructions or data items

To make your database more compact and reduce clutter, you can opt to hide or replace some parts of the listing by short text:

  1.      1. Select some instructions or data items
     2. Invoke View > Hide (or press `Ctrl`+`Numpad-`)
     3. Enter the text with which to replace the selected area (and optionally pick a color)



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hide_insn-Jun-18-2024-08-39-37-7958-AM.png?width=687&height=419&name=hide_insn-Jun-18-2024-08-39-37-7958-AM.png)

The instructions/data are replaced by the entered text but are not removed from the database; you can reveal them using View > Unhide (or `Ctrl`+`Numpad+`).

### Hiding whole functions

You can also hide or collapse whole functions by using the Hide command while the cursor is on the function’s name:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hide_func-Jun-18-2024-08-39-38-1736-AM.png?width=763&height=387&name=hide_func-Jun-18-2024-08-39-38-1736-AM.png)

You may have already seen the “COLLAPSED FUNCTION” text for library functions detected by the FLIRT signatures (colored cyan in the function list and navigation bar). The actual implementation of library functions is rarely important for analyzing the program’s code so IDA collapses them to not distract the user. 

### Hiding structures and enums

Structure or enum definitions can be collapsed and uncollapsed similarly to functions.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hide_struct-Jun-18-2024-08-39-38-5411-AM.png?width=719&height=759&name=hide_struct-Jun-18-2024-08-39-38-5411-AM.png)

### Terse struct representation

When defining structure instances in data, IDA will by default try to display them in _terse_ form, with everything on one line. By using **Unhide** , you can have it printed in full, or verbose form, with each field on separate line and a comment with the field name.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hide_terse-Jun-18-2024-08-39-35-6976-AM.png?width=703&height=322&name=hide_terse-Jun-18-2024-08-39-35-6976-AM.png)

Conversely, you can use **Hide** to collapse a structure instance into a terse form (this may not work in some cases due to the specific structure’s layout).

### Collapsing blocks in decompiler

The decompiler also has similar but separate pair of actions. They are available in the context menu or via the `Numpad-` and `Numpad+` hotkeys. You can collapse compound operators, as well as the variable declaration block at the start of the function.

### More info:

[Hide](</products/ida/support/idadoc/599.shtml>) and [Unhide](</products/ida/support/idadoc/600.shtml>) (IDA)

[Collapse/uncollapse item](<https://www.hex-rays.com/products/decompiler/manual/cmd_collapse.shtml>) (Decompiler)

---

## 2. 

**Date:** Posted: Mar 12, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-30-quick-views](https://hex-rays.com/blog/igors-tip-of-the-week-30-quick-views)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #30: Quick views

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-30-quick-views>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-30-quick-views>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-30-quick-views>)

I 

Igor Skochinsky ✦ Posted: Mar 12, 2021

![Igor’s tip of the week #30: Quick views](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

IDA has three shortcuts as an alternative to some menus which could be cumbersome to navigate.

# Quick view

Probably the most commonly used, it is triggered by the shortcut `Ctrl`+`1` and shows the items under the View > Open subviews menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/quickview1-Jun-18-2024-09-05-39-5256-AM.png?width=506&height=603&name=quickview1-Jun-18-2024-09-05-39-5256-AM.png)

It can be especially useful for opening views which have no dedicated shortcut such as Notepad (although you can always assign a custom one via the [Shortcut editor](<https://www.hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>)).

### Quick debug view

Most useful during a debugging session, this one allows you to bypass navigating to the Debugger > Debugger windows menu by simply pressing `Ctrl`+`2`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/quickview2-Jun-18-2024-09-05-37-2647-AM.png?width=506&height=596&name=quickview2-Jun-18-2024-09-05-37-2647-AM.png)

### Quick plugins view

Last but not least, `Ctrl`+`3` opens the list of plugin menu items listed under the Edit > Plugins menu, allowing you to quickly invoke a specific plugin. Please note that this list does not necessarily include all installed plugins; some plugins add menu items elsewhere or may not have a menu item at all and work in an automatic fashion.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/quickview3-Jun-18-2024-09-05-38-6744-AM.png?width=442&height=491&name=quickview3-Jun-18-2024-09-05-38-6744-AM.png)

---

## 3. 

**Date:** Posted: Mar 5, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-29-color-up-your-ida](https://hex-rays.com/blog/igors-tip-of-the-week-29-color-up-your-ida)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #29: Color up your IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-29-color-up-your-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-29-color-up-your-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-29-color-up-your-ida>)

I 

Igor Skochinsky ✦ Posted: Mar 5, 2021

![Igor’s tip of the week #29: Color up your IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

For better readability, IDA highlights various parts of the disassembly listing using different colors; however these are not set in stone and you can modify most of them to suit your taste or situation. Let’s have a look at the different options available for changing colors in IDA.

### Themes

In case you are not aware, IDA supports changing the color scheme used for the UI (windows, controls, views and listings). The default theme uses light background but there are also two dark themes available. You can change the theme used via Options > Colors… (“Current theme” selector). Each theme then can be customized further by editing the colors in the tabs below. In the Disassembly tab, you can either select items from the dropdown, or click on them in the listing, then change the color by clicking the corresponding button.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colors_disasm-Jun-18-2024-08-47-13-1483-AM.png?width=971&height=732&name=colors_disasm-Jun-18-2024-08-47-13-1483-AM.png)

If you prefer editing color values directly, you can update many of them at once or even create a complete custom theme by following the directions on the [“CSS-based styling”](<https://www.hex-rays.com/products/ida/support/tutorials/themes/>) page.

### Coloring items

In addition to changing the whole theme or colors of individual listing components, you can also color whole lines (instructions or data) in the disassembly. This can be done using the menu Edit > Other > Color instruction… 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colors_insn-Jun-18-2024-08-47-13-8876-AM.png?width=488&height=189&name=colors_insn-Jun-18-2024-08-47-13-8876-AM.png)

This command changes the background of the lines assigned to the current address (you can also select several lines to color them all together).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colors_items-3.png?width=164&height=88&name=colors_items-3.png)

### 

### Coloring graph nodes

In the Graph View, you can color whole nodes (basic blocks) by clicking the first icon (Set node color) in the node’s header.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colors_node-Jun-18-2024-08-47-13-5786-AM.png?width=586&height=125&name=colors_node-Jun-18-2024-08-47-13-5786-AM.png)

After choosing the color, all instructions in the block will be colored and it will also be shown with the corresponding color in the graph overview.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colors_node_ovrw-Jun-18-2024-08-47-16-7270-AM.png?width=766&height=118&name=colors_node_ovrw-Jun-18-2024-08-47-16-7270-AM.png)

### Coloring functions

Instead of (or in addition to) marking up single instructions or basic blocks you can also color whole functions. This can be done in the Edit Function (`Alt`+`P`) dialog by clicking the corresponding button.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colors_func-Jun-18-2024-08-47-11-9275-AM.png?width=490&height=332&name=colors_func-Jun-18-2024-08-47-11-9275-AM.png)

Changing the color of a function colors all instructions contained in it (except those colored individually), as well as its entry in the [Functions list](<https://www.hex-rays.com/blog/igors-tip-of-the-week-28-functions-list/>).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/colors_funclist-Jun-18-2024-08-47-11-3165-AM.png?width=664&height=211&name=colors_funclist-Jun-18-2024-08-47-11-3165-AM.png)

---

## 4. 

**Date:** Posted: Feb 26, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list](https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #28: Functions list

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list>)

I 

Igor Skochinsky ✦ Posted: Feb 26, 2021

![Igor’s tip of the week #28: Functions list](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

The Functions list is probably one of the most familiar features of IDA’s default desktop layout. But even if you use it every day, there are things you may not be aware of.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funclist_ida-Jun-18-2024-09-05-15-6656-AM.png?width=830&height=707&name=funclist_ida-Jun-18-2024-09-05-15-6656-AM.png)

### Modal version

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funclist_modal-1024x551-Jun-18-2024-09-05-20-8017-AM.png?width=1024&height=551&name=funclist_modal-1024x551-Jun-18-2024-09-05-20-8017-AM.png)

Available via Jump > Jump to function… menu, or the `Ctrl`–`P` shortcut, the modal dialog lets you see the full width of the list as well as do some quick navigation, for example:

  1. To jump to the current function’s start, use `Ctrl`–`P`, `Enter`;
  2. To jump to the previous function, use `Ctrl`–`P`, `Up`, `Enter` (also available as JumpPrevFunc action: default shortcut is `Ctrl`–`Shift`–`Up`);
  3. To jump to the next function, use `Ctrl`–`P`, Down, Enter (also available as JumpNextFunc action: default shortcut is `Ctrl`–`Shift`–`Down`).



### Columns

As can be seen on the second screenshot, the Functions list has many more columns than Function name which is often the only one visible. They are described in the [corresponding help topic](<https://www.hex-rays.com/products/ida/support/idadoc/586.shtml>). By clicking on a column you can ask IDA to sort the whole list on that column. For example, you can sort the functions by size to look for largest ones – the bigger the function, the more chance it has a bug; or you may look for a function with the biggest Locals area since it may have many buffers on the stack which means potential overflows.

If you sort or filter the list, you may see the following message in the Output window:

`Caching 'Functions window'... ok`

Because sorting requires the whole list, IDA has to fetch it and re-sort on almost any change in the database since it may change the list. On big databases this can become quite slow so once you don’t need sorting anymore, it’s a good idea to use “Unsort” from the context menu.

### Synchronization

The list can be synchronized with the disassembly by selecting “Turn on synchronization” from the context menu. Once enabled, the list will scroll to the current function as you navigate in the database. You can also turn it off if you prefer to see a specific function in the list no matter where you are in the listing.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funclist_sync-Jun-18-2024-09-05-20-4499-AM.png?width=321&height=476&name=funclist_sync-Jun-18-2024-09-05-20-4499-AM.png)

### Folders

Since IDA 7.5, folders can be used to organize your functions. To enable, select “Show folders” in the context menu, then “Create folder with items…” to group selected items into a folder.

### Colors & styles

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funclist_colors-Jun-18-2024-09-05-16-6374-AM.png?width=613&height=21&name=funclist_colors-Jun-18-2024-09-05-16-6374-AM.png)

Some functions in the list may be colored. In most cases the colors match the legend in the navigation bar:

  * Cyan: Library function (i.e. a function recognized by a [FLIRT signature](<https://www.hex-rays.com/products/ida/tech/flirt/>) as a compiler runtime library function)
  * Magenta/Fuchsia: an external function thunk, i.e. a function implemented in an external module (often a DLL or a shared object)
  * Lime green: a function with metadata retrieved from the [Lumina database](<https://www.hex-rays.com/products/ida/lumina/>)



But there are also others:

  * Light green: function [marked as decompiled](<https://www.hex-rays.com/products/decompiler/manual/cmd_mark.shtml>)
  * Other: function with manually set color (via Edit function… or a plugin/script)



You may also see functions marked **in bold**. These are functions which have a defined prototype (i.e types of arguments, return value and calling convention). The prototype may be defined by the user ([Y hotkey](<https://www.hex-rays.com/products/ida/support/idadoc/1361.shtml>)), or set by the loader or a plugin (e.g. from the DWARF or PDB debug information).

### Multi-selection

By selecting multiple items you can perform some operations on all of them, for example:

  * Delete function(s)…: deletes the selected functions by removing the function info (name, bounds) from the database. The instructions previously belonging to the functions remain so this can be useful, for example, for combining incorrectly split functions.
  * Add breakpoint: adds a breakpoint to the first instruction of all selected functions. This can be useful for discovering which functions are executed when you trigger a specific functionality in the program being debugged.
  * Lumina: you can push or pull metadata only for selected functions.

---

## 5. 

**Date:** Posted: Feb 19, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer](https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #27: Fixing the stack pointer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer>)

I 

Igor Skochinsky ✦ Posted: Feb 19, 2021

![Igor’s tip of the week #27: Fixing the stack pointer](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

As explained in [Simplex method in IDA Pro](<https://www.hex-rays.com/blog/simplex-method-in-ida-pro/>), having correct stack change information is essential for correct analysis. This is especially important for good and correct decompilation. While IDA tries its best to give good and correct results (and we’ve made even more improvements since 2006), sometimes it can still fail (often due to wrong or conflicting information). In this post we’ll show you how to detect and fix problems such as:

**“ sp-analysis failed”**

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sp_analysis_failed-Jun-18-2024-09-02-38-1872-AM.png?width=568&height=550&name=sp_analysis_failed-Jun-18-2024-09-02-38-1872-AM.png)

**“positive sp value has been detected”**

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sp_positive_sp-Jun-18-2024-09-02-34-1663-AM.png?width=505&height=342&name=sp_positive_sp-Jun-18-2024-09-02-34-1663-AM.png)

Both examples are from the 32-bit build of notepad.exe from Windows 10 (version 10.0.17763.475) with PDB symbols from Microsoft’s public symbol server applied.

Note: in many cases the decompiler will try to recover and still produce reasonable decompilation but if you need to be 100% sure of the result it may be best to fix them.

### Detecting the source of the problem

The first steps to resolve them are usually:

  1. Switch to the disassembly view (if you were in the decompiler);
  2. Enable “Stack pointer” under “Disassembly, Disassembly line parts” in Options > General…;  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sp_disasmopt-Jun-18-2024-09-02-36-1466-AM.png?width=584&height=488&name=sp_disasmopt-Jun-18-2024-09-02-36-1466-AM.png)
  3. Look for unusual or unexpected changes in the SP value (actually it’s the SP _delta_ value) now added before each instruction.



To detect “unusual changes” we first need to know what is “usual”. Here are some examples:

  * push instructions should increase the SP delta by the number of pushed bytes (e.g. `push eax` by 4 and `push rbp` by 8)
  * conversely, pop instructions decrease it by the same amount
  * call instructions usually either decrease SP to account for the pushed arguments (`__stdcall` or `__thiscall` functions on x86), or leave it unchanged to be decreased later by a separate instruction
  * the values on both ends of a jump (conditional or unconditional) should be the same
  * the value at the function entry and return instructions should be 0
  * between prolog and epilog the SP delta should remain the same with the exception of small areas around calls where it can increase by pushing arguments but then should return back to “neutral” before the end of the basic block.



In the first example, we can see that `loc_406F9D` has the SP delta of `00C` and the first jump to it is also `00C`, however the _second_ one is `008`. So the problem is likely in that second block. Here it is separately:
    
    
    00C mov     ecx, offset dword_41D180
    00C call    _TraceLoggingRegister@4 ; TraceLoggingRegister(x)
    008 push    offset _TraceLogger__GetInstance____2____dynamic_atexit_destructor_for__s_instance__ ; void (__cdecl *)()
    00C call    _atexit
    00C pop     ecx
    008 push    ebx
    00C call    __Init_thread_footer
    00C pop     ecx
    008 jmp     short loc_406F9D

We can see that `00C` changes to `008` after the call to `_TraceLoggingRegister@4`. On the first glance it makes sense because the `@4` suffix denotes [`__stdcall` function](<https://docs.microsoft.com/en-us/cpp/cpp/stdcall>) with 4 bytes of arguments (which means it removes 4 bytes from the stack). However, if you actually go inside and analyze it, you’ll see that it does not use stack arguments but the register `ecx`. Probably the file has been compiled with [Link-time Code Generation](<https://docs.microsoft.com/en-us/cpp/build/reference/ltcg-link-time-code-generation>) which converted __stdcall to __fastcall to speed up the code.

In the second case the disassembly looks like following:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sp_analysis_positive-Jun-18-2024-09-02-37-8317-AM.png?width=555&height=414&name=sp_analysis_positive-Jun-18-2024-09-02-37-8317-AM.png)

Here, the problem is immediately obvious: the delta becomes negative after the call. It seems IDA decided that the function is subtracting 0x14 bytes from the stack while there are only three pushes (3*4 = 12 or 0xC). You can also go inside `StringCopyWorkerW` and observe that it ends with `retn 0Ch` – a certain indicator that this is the correct number.

### Fixing wrong stack deltas

How to actually fix the wrong delta depends on the specific situation but generally there are two approaches:

  1. Fix just the place(s) where things go wrong. For this, press `Alt`–`K` (Edit > Functions > Change stack pointer…) and enter the correct amount of the SP change. In the first example it should be 0 (since the function is not using any stack arguments) and in the second 12 or 0xc. Often this is the only option for indirect calls.
  2. If the same function called from multiple places causes stack unbalance issues, edit the function’s properties (`Alt`–`P` or Edit > Functions > Edit function… ) and change the “Purged bytes” value.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sp_purged-Jun-18-2024-09-02-33-1233-AM.png?width=490&height=332&name=sp_purged-Jun-18-2024-09-02-33-1233-AM.png)



This simple example shows that even having debug symbols does not guarantee 100% correct results and why giving override options to the user is important.

---

## 6. 

**Date:** Posted: Feb 12, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-26-disassembly-options-2](https://hex-rays.com/blog/igors-tip-of-the-week-26-disassembly-options-2)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #26: Disassembly options 2

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-26-disassembly-options-2>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-26-disassembly-options-2>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-26-disassembly-options-2>)

I 

Igor Skochinsky ✦ Posted: Feb 12, 2021

![Igor’s tip of the week #26: Disassembly options 2](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Continuing [from last week](<https://www.hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options/>), let’s discuss other disassembly options you may want to change. Here’s the options page again:

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

### Disassembly line parts

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disasm_parts_dialog-v2-3.png?width=282&height=169&name=disasm_parts_dialog-v2-3.png)

This group is for options which control the content of the main line itself. Here is an example of a line with all options enabled:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disasm_parts2-v2-Jun-18-2024-09-04-32-4612-AM.png?width=806&height=158&name=disasm_parts2-v2-Jun-18-2024-09-04-32-4612-AM.png)

The marked up parts are:

  1. The line prefix (address of the line).
  2. The stack pointer value or delta (relative to the value at the entry point). Enabling this can be useful when debugging problems like “sp-analysis failed”, “positive sp value has been detected”, or “call analysis failed”.
  3. Opcode bytes. The number entered in the “Number of opcode bytes” specifies the number displayed on a single line at most. If the instruction is longer, the rest is printed on the second line. If you prefer to truncate the extra bytes, enter a negative number (e.g. -4 will display 4 bytes at most, the rest will be truncated).
  4. Comments for instructions with a short description of what the instruction is doing (may not be available for all processors or all instructions).



### Display disassembly lines

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disasm_lines_dialog-v2-3.png?width=259&height=150&name=disasm_lines_dialog-v2-3.png)

This group of options control display of lines other than the actual line of the disassembly for a given address (_main line_).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disasm_parts_lines_all-v2-Jun-18-2024-09-04-33-1947-AM.png?width=741&height=276&name=disasm_parts_lines_all-v2-Jun-18-2024-09-04-33-1947-AM.png)

  1. Empty lines: this prints additional empty lines to make disassembly more readable, especially in text mode (e.g. between functions or before labels). Turn it off to fit more code on screen.
  2. Borders between data/code: displays the border line (`;------------`) whenever there is a stop in the execution flow (e.g. after an unconditional jump or a call to a non-returning function).
  3. Basic block boundaries: adds one more empty line at the end of each basic block (i.e. after a call or a branch).
  4. Source line numbers: displays source file name and line number if this information is available in the database (e.g. imported from the DWARF debug information).
  5. Try block lines: enables or disables display of information about exception handling recovered by parsing the exception handling metadata in the binary.

---

## 7. 

**Date:** Posted: Feb 5, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options](https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #25: Disassembly options

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options>)

I 

Igor Skochinsky ✦ Posted: Feb 5, 2021

![Igor’s tip of the week #25: Disassembly options](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

By default IDA’s disassembly listing shows the most essential information: disassembled instructions with operands, comments, labels. However, the layout of this information can be tuned, as well as additional information added. This can be done via the Disassembly Options tab available via Options > General… menu (or `Alt`–`O`, `G`).

### Text and Graph views options

If you open the options dialog in graph mode, you should have something like the following:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disopt_graph-Jun-18-2024-08-58-56-9342-AM.png?width=584&height=488&name=disopt_graph-Jun-18-2024-08-58-56-9342-AM.png)

And if you do it in text mode (use `Space` to switch), it will be different:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disopt_text-Jun-18-2024-08-58-56-6418-AM.png?width=584&height=488&name=disopt_text-Jun-18-2024-08-58-56-6418-AM.png)

As you may notice, some options are annotated with (graph) or (non-graph), denoting the fact that IDA keeps two sets of options for different modes of disassembly. To make the graphs look nicer, the defaults are tuned so that the nodes are relatively narrow, while the text mode can use the full width of the window and is spaced out more. However, you can still tweak the options of either mode to your preference and even save them as a named or default [desktop layout](<https://www.hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts/>).

### Line prefixes

One example of a setting which is different in text and graph modes is “Line prefixes” (enabled in text mode, disabled in graph mode). Prefix is the initial part of the disassembly line which indicates its address (e.g. `.text:00416280`). For example, you can enable it in the graph too or disable display of the segment name to save space.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disopt_prefix-e1612533308331-Jun-18-2024-08-58-55-2103-AM.png?width=759&height=360&name=disopt_prefix-e1612533308331-Jun-18-2024-08-58-55-2103-AM.png)

Or you can show offsets from start of the function instead of full addresses:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/disopt_funcoff-Jun-18-2024-08-58-59-1457-AM.png?width=721&height=254&name=disopt_funcoff-Jun-18-2024-08-58-59-1457-AM.png)

This can be convenient because you always know which function you’re currently analyzing.

---

## 8. 

**Date:** Posted: Jan 29, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-24-renaming-registers](https://hex-rays.com/blog/igors-tip-of-the-week-24-renaming-registers)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #24: Renaming registers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-24-renaming-registers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-24-renaming-registers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-24-renaming-registers>)

I 

Igor Skochinsky ✦ Posted: Jan 29, 2021

![Igor’s tip of the week #24: Renaming registers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

While [register highlighting](<https://www.hex-rays.com/blog/igor-tip-of-the-week-05-highlight/>) can help tracking how a register is used in the code, sometimes it’s not quite sufficient, especially if multiple registers are used by a complicated piece of code. In such situation you can try _register renaming_.

To rename a register:

  * place the cursor on it and press `N` or `Enter`, or
  * double-click it



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/renamereg1-Jun-18-2024-08-57-48-0271-AM.png?width=429&height=457&name=renamereg1-Jun-18-2024-08-57-48-0271-AM.png)

A dialog appears where you can specify:

  * new name to be used in the disassembly;
  * comment to be shown at the place of the new name’s definition;
  * range of addresses where to use the name. 



The address range defaults to the current function boundaries but you can either edit them manually or select a range before renaming (this can be tricky since the cursor needs to be on the register). The new range cannot cross function boundaries (registers can be renamed only inside a function). The new name and the comment are printed at the start of the specified range.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/renamereg2-Jun-18-2024-08-57-47-3793-AM.png?width=355&height=479&name=renamereg2-Jun-18-2024-08-57-47-3793-AM.png)

Even if you don’t rename registers yourself, you may encounter them in your databases. For example, the DWARF plugin can use the information available in the DWARF debug info to rename and comment registers used for storing local variables or function arguments.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/renamereg3-Jun-18-2024-08-57-48-4005-AM.png?width=464&height=516&name=renamereg3-Jun-18-2024-08-57-48-4005-AM.png)

To undo renaming and revert back to the canonical register name, rename it to an empty string.

See also: [Rename register](<https://www.hex-rays.com/products/ida/support/idadoc/1346.shtml>) in the IDA Help.

---

## 9. 

**Date:** Posted: Jan 22, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view](https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #23: Graph view

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view>)

I 

Igor Skochinsky ✦ Posted: Jan 22, 2021

![Igor’s tip of the week #23: Graph view](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Graph view is the default disassembly representation in IDA GUI and is probably what most IDA users use every day. However, it has some lesser-known features that can improve your workflow.

### Parts of the graph

The graph consists of _nodes_ (blocks) and _edges_ (arrows between blocks). Each node roughly corresponds to a basic block.

a **basic block** is a straight-line code sequence with no branches in except to the entry and no branches out except at the exit.  
(from [Wikipedia](<https://en.wikipedia.org/wiki/Basic_block>))

Edges indicate code flow between nodes and their color changes depending on the type of code flow:

  * conditional jumps/branches have two outgoing edges: green for branch taken and red for branch not taken (i.e. fall through to next address);
  * other kind of edges are blue;
  * edges which go backwards in the graph (which usually means they’re part of a loop) are thicker in width. 



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graph_edges-Jun-18-2024-09-01-41-8258-AM.png?width=734&height=451&name=graph_edges-Jun-18-2024-09-01-41-8258-AM.png)

### Keyboard controls

Even though the graph is best suited to mouse, you can still do some things using keyboard:

  *     * `W` to zoom out so the whole graph fits in the visible window area;
    * `1` to zoom back to 100%;
    * `Ctrl`–`Up` moves to the parent node;
    * `Ctrl`–`Down` moves to the child node  
(if there are several candidates in either case, a selector is displayed)



### Mouse controls

Besides the usual clicking around, a few less obvious mouse actions are possible:

  * double-click an edge to jump to the other side of it or hover to preview the target (source) node;
  * click and drag the background to pan the whole graph in any directions;
  * use the mouse wheel to scroll the graph vertically (up/down);
  * `Alt`+wheel to scroll horizontally (left/right);
  * `Ctrl`+wheel to zoom in/out



### Rearranging and grouping the nodes

If necessary, you can move some nodes around by dragging their titles. Edges can also be moved by dragging their bending points. Use “Layout graph” from the context menu to go back to the initial layout. 

Big graphs can be simplified by grouping:

  1. Select several nodes by holding down `Ctrl` and clicking the titles of multiple nodes or by click-dragging a selection box. The selected nodes will have a different color from others (cyan in default color scheme);
  2. Select “Group nodes” from the context menu and enter the text for the new node. IDA will replace selected nodes with the new one and rearrange the graph;
  3. You can repeat the process as many times as necessary, including grouping already-grouped nodes;
  4. Created groups can be expanded again temporarily or ungrouped completely, going back to separate nodes. Use the context menu or new icons in the group node’s title bar for this.



  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graph_group1-Jun-18-2024-09-01-42-6201-AM.png?width=739&height=673&name=graph_group1-Jun-18-2024-09-01-42-6201-AM.png)  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graph_group2-Jun-18-2024-09-01-44-7775-AM.png?width=629&height=670&name=graph_group2-Jun-18-2024-09-01-44-7775-AM.png)

**![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graph_group3-e1611320163937-Jun-18-2024-09-01-40-3163-AM.png?width=650&height=664&name=graph_group3-e1611320163937-Jun-18-2024-09-01-40-3163-AM.png)**

More info: [_Graph view_ in IDA Help](<https://www.hex-rays.com/products/ida/support/idadoc/42.shtml>) (also available via `F1` in IDA).

---

