# Hex-Rays Blog – Page 33

**Archived:** 2026-02-20 12:20
**URL:** https://hex-rays.com/blog/page/33

---

## 1. 

**Date:** Posted: Jan 15, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts](https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #22: IDA desktop layouts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts>)

I 

Igor Skochinsky ✦ Posted: Jan 15, 2021

![Igor’s tip of the week #22: IDA desktop layouts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

IDA’s default windows layout is sufficient to perform most standard analysis tasks, however it may not always be the best fit for all situations. For example, you may prefer to open additional views or to modify existing ones depending on your monitor size, specific tasks, or the binary being analyzed.

### Rearranging windows

The standard operation is mostly intuitive – click and drag the window title to dock the window elsewhere. While dragging, you will see the drop markers which can be used to dock the window next to another or as a tab. You can also release the mouse without picking any marker to make the window float independently.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/desktop_dock-e1610619932258-Jun-18-2024-08-50-03-6276-AM.png?width=885&height=623&name=desktop_dock-e1610619932258-Jun-18-2024-08-50-03-6276-AM.png)

### Docking a floating window

Once a window is floating, you can’t dock it again by dragging the title. Instead, hover the mouse just below to expose the drag handle which can be used to dock it again.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/desktop_float-e1610619770712-Jun-18-2024-08-49-58-9933-AM.png?width=396&height=143&name=desktop_float-e1610619770712-Jun-18-2024-08-49-58-9933-AM.png)

### Reset layout

If you want to start over, use Windows > Reset desktop to go back to the default layout.

### Saving and using custom layouts

The layout is saved automatically in the database, but if you want to reuse it later with a different one, use Windows > Save desktop… to save it under a custom name and later Windows > Load desktop… to apply it in another database or session. Alternatively, check the “Default” checkbox to make this layout default for all new databases.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/desktop_save1-300x127-Jun-18-2024-08-50-05-3928-AM.png?width=300&height=127&name=desktop_save1-300x127-Jun-18-2024-08-50-05-3928-AM.png)

### Debugger desktop

When debugging, the windows layout changes to add views which are useful for the debugger (e.g. debug registers, Modules, Threads). This can lead to crowded display on small monitors so rearranging them can become a frequent task.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/desktop_debug-Jun-18-2024-08-50-01-6350-AM.png?width=962&height=706&name=desktop_debug-Jun-18-2024-08-50-01-6350-AM.png)

This layout is separate from the disassembly-time one so if you want to persist a custom debugger layout, you need to save it during the debug session.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/desktop_save2-1-Jun-18-2024-08-50-02-1377-AM.png?width=324&height=137&name=desktop_save2-1-Jun-18-2024-08-50-02-1377-AM.png)

More info: [Desktops](</products/ida/support/idadoc/1418.shtml>) in the IDA Help.

---

## 2. 

**Date:** Posted: Jan 8, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida](https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #21: Calculator and expression evaluation feature in IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida>)

I 

Igor Skochinsky ✦ Posted: Jan 8, 2021

![Igor’s tip of the week #21: Calculator and expression evaluation feature in IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

When reverse-engineering, sometimes you need to perform some simple calculations. While you can always use an external calculator program, IDA has a built-in one. You can invoke it by pressing `?` or via View > Calculator.

The calculator shows the result in hex, decimal, octal, binary and as a character constant. This information is also duplicated in the Output window in case you need to copy it to somewhere else.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calc_simple-Jun-18-2024-08-58-37-5506-AM.png?width=910&height=271&name=calc_simple-Jun-18-2024-08-58-37-5506-AM.png)

In addition to plain numbers, you can use names from the database, as well as register values during debugging similarly to the “Jump to address” dialog from [the previous tip](<https://www.hex-rays.com/blog/igors-tip-of-the-week-20-going-places/>).

By the way, the number, address, or identifier under cursor is picked up automatically when you press `?` so there’s no need to copy or type it manually.

In fact, the expression evaluation feature is provided by the [IDC language](<https://www.hex-rays.com/products/ida/support/idadoc/157.shtml>) interpreter built-in into IDA. You can use expressions in almost any place in IDA that accepts numbers: Jump to address, Make array, User-defined offset and so on.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calc_useroff-Jun-18-2024-08-58-36-0860-AM.png?width=331&height=529&name=calc_useroff-Jun-18-2024-08-58-36-0860-AM.png)

You can also use any of the available[ IDC functions](<https://www.hex-rays.com/products/ida/support/idadoc/162.shtml>). For example, expressions like the following are possible during debugging:
    
    
    get_qword(__security_cookie)^RSP

---

## 3. 

**Date:** Posted: Dec 17, 2020  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places](https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #20: Going places

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places>)

I 

Igor Skochinsky ✦ Posted: Dec 17, 2020

![Igor’s tip of the week #20: Going places](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Even if you prefer to move around IDA by clicking, the `G` shortcut should be the one to remember. The action behind it is called simply “Jump to address” but it can do many more things than what can be guessed from the name.

### Jump to address

First up is the actual jumping to an address: enter an address value to jump to. You can prefix it with `0x` to denote hexadecimal notation but this is optional: in the absence of a prefix, the entered string is parsed as a hexadecimal number.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/jumpaddr-Jun-18-2024-08-43-02-2609-AM.png?width=334&height=141&name=jumpaddr-Jun-18-2024-08-43-02-2609-AM.png)

  


In architectures with segmented architecture (e.g. 16-bit x86), a _segment:offset_ syntax can be used. Segment can a be symbolic name (`seg001`, `dseg`) or hexadecimal (`F000`); the offset should be hexadecimal. If the current database contains both segmented and linear (flat) addressed segments (e.g. a legacy 16-bit bootloader with 32-bit protected mode OS image in high memory), a “segment” `0` can be used to force the usage of linear address (`0:1000000`).

### Jump relative to current location

If the entered value is prefixed with `+` or `-`, it is treated as _relative_ offset from the cursor’s position. Once again, the `0x` prefix is optional: `+100` jumps 256 bytes forward and `-10000` goes 64KiB(65536 bytes) backwards.

### Jump to a name

A name (function or global variable name, or a label) in the program can be entered to jump directly to it. Note that the raw name should be entered as it’s used in the program with any possible special symbols, for example `_main` for `main()` or `??2@YAPEAX_K@Z` for `operator new()`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/jumpname-Jun-18-2024-08-43-06-1305-AM.png?width=329&height=154&name=jumpname-Jun-18-2024-08-43-06-1305-AM.png)

### Jump to an expression

A C syntax expression can be used instead of a bare address or a name. Just like in C, the hexadecimal numbers must use the `0x` prefix – otherwise decimal is assumed. Names or the special keyword `here` can be used (and are resolved to their address). Some examples:

  * `here + 32*4`: skip 32 dwords. Equivalent to `+80`
  * `_main - 0x10`: jump to a position 0x10 bytes before the function main()
  * `f2 + (f4-f3)`: multiple symbols can be used for complicated situations



### Using registers

During debugging, you can use register names as variables, similarly to names in preceding examples. For example, you can jump to `EAX`, `RSP`, `ds:si`(16-bit x86), `X0+0x20`(ARM64) and so on. This works both in disassembly and the hex view.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/jumpreg-Jun-18-2024-08-43-05-8435-AM.png?width=312&height=99&name=jumpreg-Jun-18-2024-08-43-05-8435-AM.png)

---

## 4. 

**Date:** Posted: Dec 10, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-19-function-calls](https://hex-rays.com/blog/igor-tip-of-the-week-19-function-calls)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #19: Function calls

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-19-function-calls>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-19-function-calls>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-19-function-calls>)

I 

Igor Skochinsky ✦ Posted: Dec 10, 2020

![Igor’s tip of the week #19: Function calls](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

When dealing with big programs or huge functions, you may want to know how various functions interact, for example where the current function is called from and what other functions it calls itself. While for the former you can use “Cross-references to”, for the latter you have to go through all instructions of the function and look for calls to other functions. Is there a better way?

### Function calls view

This view, available via View > Open subviews > Function calls, offers a quick overview of calls to and from the current function. It is dynamic and updates as you navigate to different functions so it can be useful to dock it next to the listing to be always visible. Double-click any line in the caller or called list to jump to the corresponding address.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/funccalls-Jun-18-2024-09-05-07-2738-AM.png?width=709&height=505&name=funccalls-Jun-18-2024-09-05-07-2738-AM.png)

---

## 5. 

**Date:** Posted: Dec 10, 2020  
**Link:** [https://hex-rays.com/blog/ida-pro-on-apple-silicon](https://hex-rays.com/blog/ida-pro-on-apple-silicon)

[Back](<https://hex-rays.com/blog>)

[IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Mac](<https://hex-rays.com/blog/tag/mac>) [debugger](<https://hex-rays.com/blog/tag/debugger>)

# IDA Pro on Apple Silicon

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-on-apple-silicon>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-on-apple-silicon>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-on-apple-silicon>)

Troy Bowman ✦ Posted: Dec 10, 2020

![IDA Pro on Apple Silicon](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

IDA Pro for ARM64 is coming! We have ported all of IDA to run natively on Apple Silicon and it will be available in IDA 7.6.

Initial analysis shows that the hype is real. IDA is consistently performing much faster on M1 macs:

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

And a visual representation, for your viewing delight:

<https://www.hex-rays.com/wp-content/uploads/2020/12/split.mp4>

We have also ported the mac debugger to ARM64. So far it is working well:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/debugger-1024x682-Jun-18-2024-08-41-53-7152-AM.png?width=1024&height=682&name=debugger-1024x682-Jun-18-2024-08-41-53-7152-AM.png)

IDAPython also works with the latest ARM64 release of Python 3.9, including PyQt:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/python3-2-1024x423-Jun-18-2024-08-41-54-5070-AM.png?width=1024&height=423&name=python3-2-1024x423-Jun-18-2024-08-41-54-5070-AM.png)

There are still a few wrinkles to iron out but so far we are very pleased with the results. We hope this will satisfy your appetite until IDA 7.6 is released!

---

## 6. 

**Date:** Posted: Dec 3, 2020  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-18-decompiler-and-global-cross-references](https://hex-rays.com/blog/igors-tip-of-the-week-18-decompiler-and-global-cross-references)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #18: Decompiler and global cross-references

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-18-decompiler-and-global-cross-references>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-18-decompiler-and-global-cross-references>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-18-decompiler-and-global-cross-references>)

I 

Igor Skochinsky ✦ Posted: Dec 3, 2020

![Igor’s tip of the week #18: Decompiler and global cross-references](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Previously we’ve covered [cross-references](</blog/igor-tip-of-the-week-16-cross-references/>) in the disassembly view but in fact you can also consult them in the decompiler (pseudocode) view.

### Local cross-references

The most common shortcut (`X`) works similarly to disassembly: you can use it on labels, variables (local and global), function names, but there are some differences and additions:

  * for local variables, the list of cross-references shows _pseudocode lines_ instead of disassembly snippets.   
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_lvar-Jun-18-2024-08-51-26-7490-AM.png?width=463&height=218&name=xrefs_lvar-Jun-18-2024-08-51-26-7490-AM.png)
  * if you press `X` on an C statement keyword (e.g. `if`, `while`, `return`), all statements of the same type in the current function will be shown 

### ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_if-Jun-18-2024-08-51-26-0247-AM.png?width=459&height=163&name=xrefs_if-Jun-18-2024-08-51-26-0247-AM.png)




### Global cross-references

If you have a well-analyzed database with custom types used by the program and properly set up function prototypes, you can ask the decompiler to analyze all functions and build a list of cross-references to a structure field, an enum member or a whole local type. The default hotkey is `Ctrl`–`Alt`–`X`.

When you use it for the first time, the list may be empty or include only recently decompiled functions.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_global0-Jun-18-2024-08-51-23-7599-AM.png?width=733&height=226&name=xrefs_global0-Jun-18-2024-08-51-23-7599-AM.png)

To cover all functions, refresh the list from the context menu or by pressing `Ctrl`–`U`. This will decompile _all_ functions in the database and gather the complete list. The decompilation results are cached so next time you use the feature it will be faster.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_global-Jun-18-2024-08-51-27-7431-AM.png?width=727&height=501&name=xrefs_global-Jun-18-2024-08-51-27-7431-AM.png)

---

## 7. 

**Date:** Posted: Dec 1, 2020  
**Link:** [https://hex-rays.com/blog/python-3-9-support-for-ida-7-5](https://hex-rays.com/blog/python-3-9-support-for-ida-7-5)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [idapyswitch](<https://hex-rays.com/blog/tag/idapyswitch>)

# Python 3.9 support for IDA 7.5

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/python-3-9-support-for-ida-7-5>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/python-3-9-support-for-ida-7-5>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/python-3-9-support-for-ida-7-5>)

I 

Igor Skochinsky ✦ Posted: Dec 1, 2020

![Python 3.9 support for IDA 7.5](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Python 3.9 has been released fairly recently and it was a bit too short notice for us to ensure it works with IDA 7.5 Service Pack 3 (if you have tried it, you may have had a bad time.)

We have now added support for Python 3.9 in IDAPython. Here’s how you can get it to work (only if you _need_ Python 3.9; IDA already works fine with 3.8 or earlier versions):

  1. Make sure you are using IDA 7.5 SP3
  2. Download the appropriate package for your platform below
  3. Unpack the package in the directory of your IDA installation
  4. Run the `idapyswitch` binary (on Windows, from admin command prompt)



Everything should then work as intended.

Downloads

[ida75sp3_python39_win.zip](</wp-content/uploads/2020/12/ida75sp3_python39_win.zip>)

[ida75sp3_python39_linux.zip](</wp-content/uploads/2020/12/ida75sp3_python39_linux.zip>)

[ida75sp3_python39_mac.zip](</wp-content/uploads/2020/12/ida75sp3_python39_mac.zip>)

---

## 8. 

**Date:** Posted: Nov 27, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-17-cross-references-2](https://hex-rays.com/blog/igor-tip-of-the-week-17-cross-references-2)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #17: Cross-references 2

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-17-cross-references-2>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-17-cross-references-2>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-17-cross-references-2>)

I 

Igor Skochinsky ✦ Posted: Nov 27, 2020

![Igor’s tip of the week #17: Cross-references 2](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

### Cross references view

The [jump to xref](</blog/igor-tip-of-the-week-16-cross-references/>) actions are good enough when you have a handful of cross-references but what if you have hundreds or thousands? For such cases, the _Cross references_ view may be useful. You can open it using the corresponding item in the View > Open Subviews menu. IDA will gather cross-references to the current disassembly address and show them in a separate tab. It’s even possible to open several such views at the same time (for different addresses).

### ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_view-Jun-18-2024-09-07-17-1482-AM.png?width=896&height=428&name=xrefs_view-Jun-18-2024-09-07-17-1482-AM.png)

### Adding cross-references

In some cases you may need to add a manual cross-reference, for example to fix up an obfuscated function’s control flow graph or add a call cross-reference from an indirect call instruction discovered by debugging. There are several ways to do it.

  * In the Cross references view, choose “Add cross-reference…” from the context menu or press `Ins`. In the dialog, enter source and destination addresses and the xref type.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_add-Jun-18-2024-09-07-16-7685-AM.png?width=270&height=319&name=xrefs_add-Jun-18-2024-09-07-16-7685-AM.png)
  * For **indirect calls** in binaries for **PC** (x86/x64), **ARM** , or **MIPS** processors, you can use Edit > Plugins > Set callee address (`Alt`–`F11`).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_callee-300x168-Jun-18-2024-09-07-18-3891-AM.png?width=300&height=168&name=xrefs_callee-300x168-Jun-18-2024-09-07-18-3891-AM.png)
  * To add cross-references **programmatically** , use IDC or IDAPython functions[ `add_cref` and `add_dref`](</products/ida/support/idadoc/313.shtml>). Use the `XREF_USER` flag together with the xref type to ensure that your cross-reference is not deleted by IDA on reanalysis:  
`add_cref(0x100897E8, 0x100907C0, fl_CN|XREF_USER)  
add_dref(0x100A65CC, 0x100897E0, dr_O|XREF_USER)`

---

## 9. 

**Date:** Posted: Nov 20, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references](https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #16: Cross-references

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references>)

I 

Igor Skochinsky ✦ Posted: Nov 20, 2020

![Igor’s tip of the week #16: Cross-references](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

_cross-reference_ , n.  
A reference or direction in one place in a book or other source of information to information at another place in the same work  
(from [Wiktionary](<https://en.wiktionary.org/wiki/cross-reference>)) 

To help you during analysis, IDA keeps track of **cross-references**(or _xrefs_ for short) between different parts of the program. You can inspect them, navigate them or even add your own to augment the analysis and help IDA or the decompiler.

### Types of cross-references

There are two groups of cross-references:

  1. **code** cross-references indicate a relationship between two areas of code: 
     1. **jump** cross-reference indicates conditional or unconditional transfer of execution to another location.
     2. **call** cross-reference indicates a function or procedure call with implied return to the address following the call instruction.
     3. **flow** cross-reference indicates normal execution flow from current instruction to the next. This xref type is rarely shown explicitly in IDA but is used extensively by the analysis engine and plugin/script writers need to be aware of it.
  2. **data** cross-references are used for references to data, either from code or from other data items: 
     1. **read** cross-reference indicates that the data at the address is being read from.
     2. **write** cross-reference indicates that the data at the address is being written to.
     3. **offset** cross-reference indicates that the address the of the item is taken but not explicitly read or written.
     4. **structure** cross-references are added when a structure is used in the disassembly or embedded into another structure.



The cross-reference types may be denoted by single-letter codes which are described in IDA’s help topic [“Cross reference attributes”](</products/ida/support/idadoc/1305.shtml>).

### Inspecting and navigating cross-references

In the graph view, code cross-references are shown as edges (arrows) between code blocks. You can navigate by following the arrows visually or double-clicking.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_graph-Jun-18-2024-08-51-37-6897-AM.png?width=698&height=291&name=xrefs_graph-Jun-18-2024-08-51-37-6897-AM.png)

In text mode, cross-references to the current address are printed as comments at the end of the line. By default, maximum two references are printed; if there are more, ellipsis (…) is shown. You can increase the amount of printed cross-references in Options > General… Cross-references tab.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_text-Jun-18-2024-08-51-36-6507-AM.png?width=640&height=313&name=xrefs_text-Jun-18-2024-08-51-36-6507-AM.png)

Only explicit references are shown in comments; flow cross-references are not displayed in text mode. However, the _absence_ of a flow cross-reference (end of code execution flow) is shown by a dashed line; usually it’s seen after unconditional jumps or returns but can also appear after calls to non-returning functions.

To navigate to the source of the cross-reference, double-click or press Enter on the address in the comment.

### Shortcuts

`X` is probably the most common and useful shortcut: press it to see the list of cross-references to the **identifier under cursor**. Pick an item from the list to jump to it. The shortcut works not only for disassembly addresses but also for **stack variables** (in a function) as well as **structure** and **enum members**.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_strmem-Jun-18-2024-08-51-41-1668-AM.png?width=474&height=197&name=xrefs_strmem-Jun-18-2024-08-51-41-1668-AM.png)

`Ctrl`–`X` works similarly but shows the list of cross-references to the **current address** , regardless of where the cursor is in the line. For example, it is useful when you need to check the list of callers of the current function while being positioned on its first instruction.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_func-e1605794384705-Jun-18-2024-08-51-40-4065-AM.png?width=868&height=456&name=xrefs_func-e1605794384705-Jun-18-2024-08-51-40-4065-AM.png)

  


`Ctrl`–`J`, on the other hand, shows a list of cross-references **from the current address**. Having multiple cross-references from a single location to multiple others is a somewhat rare situation but one case where it’s useful is **switches**(table jumps): using this shortcut on the indirect jump instructions allows you to quickly see and jump to any of the switch cases.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefs_from-e1605795076913-Jun-18-2024-08-51-39-2661-AM.png?width=770&height=306&name=xrefs_from-e1605795076913-Jun-18-2024-08-51-39-2661-AM.png)

If you forget the shortcuts or simply prefer using the mouse, you can find the corresponding menu items in the Jump menu (and sometimes in the context menu).

---

