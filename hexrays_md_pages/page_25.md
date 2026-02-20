# Hex-Rays Blog – Page 25

**Archived:** 2026-02-20 12:17
**URL:** https://hex-rays.com/blog/page/25

---

## 1. 

**Date:** Posted: Feb 4, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-75-working-with-unions](https://hex-rays.com/blog/igors-tip-of-the-week-75-working-with-unions)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #75: Working with unions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-75-working-with-unions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-75-working-with-unions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-75-working-with-unions>)

I 

Igor Skochinsky ✦ Posted: Feb 4, 2022

![Igor’s tip of the week #75: Working with unions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

In C, [union](<https://en.cppreference.com/w/c/language/union>) is a type similar to a struct but in which all members (possibly of different types) occupy the same memory, overlapping each other. They are used, for example, when there is a need to interpret the same data in different ways, or to save memory when storing data of different types (this is common in scripting engines, among others). IDA and the decompiler fully support unions and include definitions of commonly used ones in the standard [type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>), so they may be already present in the analyzed binaries.

### Creating unions

Assembly-level unions can be created in the Structures window by enabling “create union” checkbox when adding a new “structure”.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/union_create-3.png?width=370&height=243&name=union_create-3.png)

You can also use the [Local Types](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>) editor to create a union using C syntax.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/union_create2-3.png?width=462&height=352&name=union_create2-3.png)

### Using unions in disassembly

In disassembly, unions can be used similarly to structures. For example, when a member is referenced as an offset from a register, you can use the context menu’s “Structure offset” submenu or the `T` hotkey. The difference is that you may see multiple “paths” for the same offset, representing alternative union members, so you can pick one most suitable for the specific use case.

### Example: OLE automation

OLE Automation is a COM-based set of APIs commonly used to implement scripting in Microsoft and other applications. One of the basic types used in it is the [`VARIANT`](<https://docs.microsoft.com/en-us/windows/win32/api/oaidl/ns-oaidl-variant>) aka `VARIANTARG` structure, which can contain different types of values by embedding a union of different typed fields inside it.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/union_variant-3.png?width=439&height=458&name=union_variant-3.png)

For example, if we have an instruction `mov eax, [edx+8]` and we know that `edx` points to an instance of `VARIANTARG`, using `T` on the second operand shows us multiple versions of the union field, so we can pick the one most relevant to the specific code path taken.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/union_stroff-3.png?width=788&height=600&name=union_stroff-3.png)

### Changing the union field used

After you (or IDA) selected a union field, you can change it by going through the struct selection again (e.g. the `T` hotkey). But if the parent structure should remain the same, you can change only the union member by using the command Edit > Structs > Select union member… (hotkey `Alt`–`Y`). This can be especially useful when a structure with embedded union is placed on the stack, because you can’t use the normal structure offset commands there (the offset inside the instruction is based on the stack or frame pointer which does not point to the beginning of the structure).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/union_select-3.png?width=907&height=364&name=union_select-3.png)

### Unions in decompiler

Because the decompiler can do dataflow analysis, in many cases it can pick up the most suitable union field by matching the expected type of the variable used by the code. For example, in the snippet below the decompiler picked the correct field for the argument passed to `SysAllocString`, because it knows that the function expects an argument of type `const OLECHAR *` , which is compatible with the `BSTR bstrVal` field of the union.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/union_decompiler-3.png?width=443&height=209&name=union_decompiler-3.png)

However, for the other reference the `iVal` filed was selected. While it is compatible for the use case (comparing against zero), by looking at the code it’s obvious that the code is interpreting a boolean variant value (this can be made more clear by replacing the number 11 by the symbolic constant `VT_BOOL`). This means that `boolVal` is a more logical choice, and we can pick it by using “Select union field…” from the context menu, or the same `Alt`–`Y` hotkey as for disassembly.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/union_decompiler_sel-3.png?width=871&height=632&name=union_decompiler_sel-3.png)

More info:

[IDA Help: Select union member](<https://www.hex-rays.com/products/ida/support/idadoc/498.shtml>)  
[Hex-Rays interactive operation: Select union field](<https://hex-rays.com/products/decompiler/manual/cmd_select_union_field.shtml>)

---

## 2. 

**Date:** Posted: Jan 28, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-74-parameter-identification-and-tracking-pit](https://hex-rays.com/blog/igors-tip-of-the-week-74-parameter-identification-and-tracking-pit)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #74: Parameter identification and tracking (PIT)

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-74-parameter-identification-and-tracking-pit>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-74-parameter-identification-and-tracking-pit>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-74-parameter-identification-and-tracking-pit>)

I 

Igor Skochinsky ✦ Posted: Jan 28, 2022

![Igor’s tip of the week #74: Parameter identification and tracking \(PIT\)](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Many features of IDA and other disassemblers are taken for granted nowadays but it’s not always been the case. As one example, let’s consider automatic variable naming.

### A little bit of history

In the [first versions](<https://hex-rays.com/about-us/our-journey/>), IDA did not differ much from a dumb disassembler with comments and renaming and showed pretty much raw instructions with numerical offsets. To keep track of them users often had to add manual comments.

A few versions later, support for stack variables appeared. They initially had dummy names (`var_4`, `var_C` etc.) but could be renamed by the user which eased the reverse engineering process. However, this could still be tedious in big programs.

Next, [FLIRT](<https://hex-rays.com/products/ida/tech/flirt/>) was added, which helped identify standard library functions. Now the user did not need to analyze boilerplate code from the compiler runtime libraries but only the code written by the programmer. Having identified library functions also helped in picking names for variables: most library functions had known prototypes so the variables used for their arguments could be renamed accordingly.

However, this process was still manual, could it not be automated?

And indeed, this is what happened in [IDA 4.10](<https://hex-rays.com/products/ida/news/4_x/>), with the addition of the type system and [standard type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>). Now the identified library or imported functions could be matched to their prototypes in the type library and their arguments commented and/or renamed. For the arguments using a complex type (e.g. a structure), the stack variable could also be changed to use that type.

### In practice

As a current example, let’s have a look at a Win32 program which calls `CreateWindowExA`.

First, with everything disabled:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pit_cfg_none-3.png?width=298&height=461&name=pit_cfg_none-3.png)
    
    
    mov     eax, [ebp-20h]
    push    dword ptr [ebp+8]
    sub     eax, [ebp-28h]
    push    dword ptr [ebx+1Ch]
    push    eax
    mov     eax, [ebp-24h]
    sub     eax, [ebp-2Ch]
    push    eax
    push    dword ptr [ebp-28h]
    push    dword ptr [ebp-2Ch]
    push    dword ptr [ebp-8]
    push    edi
    push    offset aEdit    ; "edit"
    push    edi
    call    ds:CreateWindowExA
    

Next, with stack variables:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pit_cfg_stkvar-3.png?width=298&height=461&name=pit_cfg_stkvar-3.png)
    
    
    mov     eax, [ebp+var_20]
    push    [ebp+arg_0]
    sub     eax, [ebp+var_28]
    push    dword ptr [ebx+1Ch]
    push    eax
    mov     eax, [ebp+var_24]
    sub     eax, [ebp+var_2C]
    push    eax
    push    [ebp+var_28]
    push    [ebp+var_2C]
    push    [ebp+var_8]
    push    edi
    push    offset aEdit    ; "edit"
    push    edi
    call    ds:CreateWindowExA

Stack variables are created but use dummy names. We could consult the [function’s documentation](<https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-createwindowexa>) and rename and retype them manually. But instead we can enable argument propagation and [reanalyze the function](<https://hex-rays.com/blog/igor-tip-of-the-week-17-cross-references-2/>):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pit_cfg_all-3.png?width=298&height=461&name=pit_cfg_all-3.png)
    
    
    mov     eax, [ebp+Rect.bottom]
    push    [ebp+hMenu]     ; hMenu
    sub     eax, [ebp+Rect.top]
    push    dword ptr [ebx+1Ch] ; hWndParent
    push    eax             ; nHeight
    mov     eax, [ebp+Rect.right]
    sub     eax, [ebp+Rect.left]
    push    eax             ; nWidth
    push    [ebp+Rect.top]  ; Y
    push    [ebp+Rect.left] ; X
    push    [ebp+dwStyle]   ; dwStyle
    push    edi             ; lpWindowName
    push    offset aEdit    ; "edit"
    push    edi             ; dwExStyle
    call    ds:CreateWindowExA
    

Now, all arguments are renamed and all instructions initializing them are commented. The `Rect` variable was renamed and typed thanks to another place in the same function: 
    
    
    lea     eax, [ebp+Rect]
    push    eax             ; lpRect
    push    ebx             ; hWnd
    call    ds:GetClientRect

Here, IDA recognized that the `lea` instruction effectively takes an address of a struct so the stack variable should be the struct itself and not just a pointer. Thanks to this, the field references are clearly identified in the other snippet.

### Recursive propagation

In fact, PIT is not limited to single functions: if any of the function’s own arguments are renamed or retyped thanks to the type information, this information is propagated up the call tree. For example, `arg_0` from the second snippet is a function argument which was renamed to `hMenu`, so this information is used by the caller:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pit_propagate-3.png?width=436&height=244&name=pit_propagate-3.png)

---

## 3. 

**Date:** Posted: Jan 26, 2022  
**Link:** [https://hex-rays.com/blog/future-idapython-versions-will-be-python3-only](https://hex-rays.com/blog/future-idapython-versions-will-be-python3-only)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Future IDAPython versions will be Python3-only

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/future-idapython-versions-will-be-python3-only>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/future-idapython-versions-will-be-python3-only>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/future-idapython-versions-will-be-python3-only>)

Arnaud Diederen ✦ Posted: Jan 26, 2022

![Future IDAPython versions will be Python3-only](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Even though Python 2 has been end-of-life’d on January 1st, 2020, we have until now been providing IDAPython builds that can run on a Python 2 runtime.

But usage of Python 2 runtimes has been discouraged for a while now by the Python community, and official downloads for Python 2 for certain systems simply on which IDA runs, don’t exist (e.g., `Aarch64 macOS`)

Furthermore, as Python 3 evolves (and IDAPython evolves as well), retaining that compatibility has an ever-increasing cost.

## IDA 7.8 will ship with IDAPython-on-Python3 only

In order to free us of that cost, we have decided to drop support for Python 2 in the next IDA releases (7.8 onward.) We trust that, by now, the impact for our users will be minimal.

---

## 4. 

**Date:** Posted: Jan 24, 2022  
**Link:** [https://hex-rays.com/blog/2022-ida-training-session-registration-is-now-open](https://hex-rays.com/blog/2022-ida-training-session-registration-is-now-open)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Programming](<https://hex-rays.com/blog/tag/programming>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [idatraining](<https://hex-rays.com/blog/tag/idatraining>)

# 2022 IDA Training session: Registration is now open!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/2022-ida-training-session-registration-is-now-open>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/2022-ida-training-session-registration-is-now-open>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/2022-ida-training-session-registration-is-now-open>)

Holly Walsh ✦ Posted: Jan 24, 2022

![2022 IDA Training session: Registration is now open!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

The first [2022 IDA training session](</products/ida/training/>) will take place online from 16-20 and 23-25 May 2022 , from 9am **Pacific Standard Time.**

The session is devised to help professional reverse engineers master IDA Pro, the de-facto industry standard reverse engineering tool and take their reversing skills to the next level. Provided by the experts behind IDA, the training offers its participants unique opportunity to dissect the famous disassembler, understand its internals and upgrade their analysis with IDA’s limitless capabilities.

The first five days (16-20 May) will be devoted to the **[standard IDA training](</training/#standard>)**. The following three days (23-25 May) to a course on **[advanced training (Programming for IDA)](</training/#advanced>)**.

Training will be both theoretical and practical. After each theoretical section hands-on exercises will be carried out so as to master thorough understanding of concepts and methods. Training material is always updated to include the latest additions to IDA.

Detailed information including [course programs, cost and registration forms](</training/>) can be found in our dedicated training page. If needed, additional information may be requested by emailing our [sales](<mailto:sales@hex-rays.com>) team.

[Learn more about the course](</training/>) [Book a seat](<https://hex-rays.com/wp-content/uploads/2022/01/hexrays_training_2022-05.pdf>)

---

## 5. 

**Date:** Posted: Jan 21, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-73-output-window-and-logging](https://hex-rays.com/blog/igors-tip-of-the-week-73-output-window-and-logging)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #73: Output window and logging

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-73-output-window-and-logging>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-73-output-window-and-logging>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-73-output-window-and-logging>)

I 

Igor Skochinsky ✦ Posted: Jan 21, 2022

![Igor’s tip of the week #73: Output window and logging](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Output window is part of IDA’s default desktop layout and shows various messages from IDA and possibly third-party components (plugins, processor modules, scripts…). It also contains the Command-line interface (CLI) input box.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outwin_1-3.png?width=845&height=175&name=outwin_1-3.png)

### Opening the Output window

Although it is present by default, it is possible to close this window, or use a [desktop layout](<https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts/>) without it. If this happens, one way to restore it is to use Windows > Reset desktop to bring the layout to the initial state. But you can also use:

  * Windows > Output window (shortcut `Alt`+`0`), to (re)open it and focus on the text box (for example, to select text for copying);
  * Windows > Focus command line (Shortcut `Ctrl`+`.`) to switch to the CLI input field, which also re-opens the Output window if it was closed.



### Context menu

There are several actions available in the text box of the Output window, which can be consulted by opening the context menu:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outwin_2-3.png?width=191&height=254&name=outwin_2-3.png)

For example, similarly to other IDA windows, you can [search](<https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida/>) for text using `Alt`+`T`/`Ctrl`+`T` shortcuts, or clear the current text to easier see output of a script you’re planning to run.

### Timestamps

Starting from [IDA 7.7](<https://hex-rays.com/products/ida/news/7_7/>), you can turn on timestamps for every message printed to the Output window. They are stored independently from the text so can be turned on or off at any point and affect all (past and future) messages in the current IDA session.

### Navigation

Double-clicking on an address or identifier in Output window will jump to the corresponding location (if it exists) in the last active disassembly, pseudocode, or Hex view. This can be useful when writing quick scripts: just print addresses or names of interest using `msg()` function and double-click to inspect them in the disassembly listing.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outwin_3-3.png?width=690&height=897&name=outwin_3-3.png)

### Logging to file

Logging of the messages in Output window to a file can be especially useful when using IDA in [batch mode](<https://hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood/>), but also in other situations (e.g. debugging scripts or plugins). The following options exist to enable it:

  1. set [environment variable](<https://www.hex-rays.com/products/ida/support/idadoc/1375.shtml>) `IDALOG` to a filename. If the path is not absolute, the file will be created in the current directory. All IDA run afterwards will append output to the same file, so it can contain information from multiple runs.
  2. pass the `-L<file>` [command line switch](<https://www.hex-rays.com/products/ida/support/idadoc/417.shtml>) to IDA. Note that it has to precede the input filename.
  3. On-demand, one-time saving can be done via “Save to file” context menu command (shortcut `Ctrl`+`S`).



Note: if you have enabled timestamps in IDA, they will be added in the log file too (and in all future IDA sessions). There is currently no possibility to turn timestamps on or off via environment variable or command line switch.

---

## 6. 

**Date:** Posted: Jan 18, 2022  
**Link:** [https://hex-rays.com/blog/ida-7-7-service-pack-1-released](https://hex-rays.com/blog/ida-7-7-service-pack-1-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.7 Service Pack 1 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-7-service-pack-1-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-7-service-pack-1-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-7-service-pack-1-released>)

Geoffrey Czokow ✦ Posted: Jan 18, 2022

![IDA 7.7 Service Pack 1 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Hex-Rays announces the release of IDA Service Pack 1 (SP1) for IDA 7.7.

This Service Pack is primarily a bugfix release for a few errors that might affect some users.

### How to request the new versions

As usual, the new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to [support@hex-rays.com](<mailto:support@hex-rays.com>).

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist __](</products/ida/news/7_7sp1/>)[Get a quote __](</cgi-bin/quote.cgi>)

---

## 7. 

**Date:** Posted: Jan 14, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-72-more-string-literals](https://hex-rays.com/blog/igors-tip-of-the-week-72-more-string-literals)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #72: More string literals

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-72-more-string-literals>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-72-more-string-literals>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-72-more-string-literals>)

I 

Igor Skochinsky ✦ Posted: Jan 14, 2022

![Igor’s tip of the week #72: More string literals](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We’ve covered basics of working with string constants (aka string literals) [before](<https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings/>) but IDA support additional features which may be useful in some situations.

### Exotic string types

Pascal and derived languages (such as Delphi) sometimes employ string literals which start with the length followed by the characters. Similarly to the wide (Unicode) strings, they can be created using the corresponding buttons in the Options > String literals… dialog or the Edit > Strings submenu.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_pascal_1-3.png?width=732&height=346&name=strlit_pascal_1-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_pascal_2-3.png?width=516&height=19&name=strlit_pascal_2-3.png)

Some OS or embedded firmware can employ a byte other than 0 as string terminator. When analyzing such binary, you can set this up in the Options > General…, Strings tab (also accessible via Options > String literals…, “Manage defaults” link.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_charterm-3.png?width=879&height=505&name=strlit_charterm-3.png)

As a common variation of this type, DOS type strings (terminated with the `$` character) have their own entry in the Edit > Strings menu.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_dos_1-3.png?width=518&height=475&name=strlit_dos_1-3.png)

### Changing string length

For already-created string literals, you can use the `*` shortcut to edit them as if they were an array and adjust “Array size” to change the length of the string.

See also:

[Unicode strings and custom encodings](<https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings/>)

[How to format multiple strings placed together](<https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays/>).

---

## 8. 

**Date:** Posted: Jan 10, 2022  
**Link:** [https://hex-rays.com/blog/go2pat-a-tool-for-building-signatures-for-golang-binaries](https://hex-rays.com/blog/go2pat-a-tool-for-building-signatures-for-golang-binaries)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# go2pat: a tool for building signatures for Golang binaries

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/go2pat-a-tool-for-building-signatures-for-golang-binaries>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/go2pat-a-tool-for-building-signatures-for-golang-binaries>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/go2pat-a-tool-for-building-signatures-for-golang-binaries>)

Arnaud Diederen ✦ Posted: Jan 10, 2022

![go2pat: a tool for building signatures for Golang binaries](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

As part of our effort to improve the analysis of Go programs, we included FLIRT signatures from functions for the Go runtime and standard library in the recently-released IDA 7.7.

Those signatures, that support Go runtimes versions 1.10 through 1.16 (for x64 architectures, on Windows, Linux & Mac), can greatly improve the workflow of users as they allow them to quickly identify library functions (which can usually be ignored). And, since Go executables are statically linked, large parts of the binaries can quickly be marked as library code.

Alas, we cannot reasonably do that for all combinations of Go runtime versions, CPU architectures, and OSes. That is why we today we are providing a new tool – `go2pat` – to enable users to generate patterns (that can then be built into signatures by using `sigmake`) from Go distributions for different architectures & operating systems.

We have made the `go2pat` tool part of the [flair77.zip](<https://hex-rays.com/products/ida/support/ida/flair77.zip>) set of utilities. Be sure to have a look at `go/go2pat/go2pat.md` for instructions!

---

## 9. 

**Date:** Posted: Jan 7, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-71-decompile-as-call](https://hex-rays.com/blog/igors-tip-of-the-week-71-decompile-as-call)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #71: Decompile as call

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-71-decompile-as-call>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-71-decompile-as-call>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-71-decompile-as-call>)

I 

Igor Skochinsky ✦ Posted: Jan 7, 2022

![Igor’s tip of the week #71: Decompile as call](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Although the Hex-Rays decompiler was originally written to deal with compiler-generated code, it can still do a decent job with manually written assembly. However, such code may use non-standard instructions or use them in non-standard ways, in which case the decompiler may fail to produce equivalent C code and has to fall back to `_asm` statements.

### Analyzing system code

As an example, let’s have a look at this function from a PowerPC firmware.
    
    
    ROM:00000C8C sub_C8C:                                # CODE XREF: ROM:00000B1C↑p
    ROM:00000C8C                                         # sub_CF0+44↓p ...
    ROM:00000C8C
    ROM:00000C8C .set back_chain, -0x18
    ROM:00000C8C .set var_C, -0xC
    ROM:00000C8C .set sender_lr,  4
    ROM:00000C8C
    ROM:00000C8C     stwu      r1, back_chain(r1)
    ROM:00000C90     mflr      r0
    ROM:00000C94     stmw      r29, 0x18+var_C(r1)
    ROM:00000C98     stw       r0, 0x18+sender_lr(r1)
    ROM:00000C9C     addi      r31, r3, 0
    ROM:00000CA0     mflr      r3
    ROM:00000CA4     addi      r30, r3, 0
    ROM:00000CA8     bl        sub_1264
    ROM:00000CAC     lis       r29, 0x40 # '@'
    ROM:00000CB0     lhz       r29, -0x2C(r29)
    ROM:00000CB4     mtsprg0   r29
    ROM:00000CB8     not       r11, r31
    ROM:00000CBC     slwi      r11, r11, 16
    ROM:00000CC0     or        r31, r11, r31
    ROM:00000CC4     mtsprg1   r31
    ROM:00000CC8     mtsprg2   r30
    ROM:00000CCC     mftb      r3
    ROM:00000CD0     addi      r30, r3, 0
    ROM:00000CD4     mtsprg3   r30
    ROM:00000CD8     bl        sub_1114
    ROM:00000CD8 # End of function sub_C8C
    

The code seems to be using Special Purpose Register General (`sprg0`/1/2/3) for its own purposes, probably to store some information for exception processing. Because system instructions are generally not encountered in user-mode code, they are not supported by the decompiler out-of-box and the default output looks like this:
    
    
    void __fastcall __noreturn sub_C8C(int a1)
    {
      int v1; // lr
    
      _R30 = v1;
      sub_1264();
      _R29 = (unsigned __int16)word_3FFFD4;
      __asm { mtsprg0   r29 }
      _R31 = (~a1 << 16) | a1;
      __asm
      {
        mtsprg1   r31
        mtsprg2   r30
        mftb      r3
      }
      _R30 = _R3;
      __asm { mtsprg3   r30 }
      sub_1114();
    }

Although the instructions themselves are shown as `_asm` statements, the decompiler could detect the registers used by them and created pseudo variables (`_R29`, `_R30`, `_R31`) to represent the operations performed. However, it is possible to get rid of `_asm` blocks with a bit of manual work.

### Decompile as call

It is possible to tell the decompiler that specific instructions should be treated as if they were function calls. You can even use a [custom calling convention](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>) to specify the exact input/output registers of the pseudo function. Let’s try it for the unhandled instructions.

  1. In the disassembly view, place the cursor on the instruction (e.g. `mtsprg0 r29`);
  2. Invoke Edit > Other > Decompile as call…  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decompile_call1-3.png?width=694&height=99&name=decompile_call1-3.png)
  3. Enter the prototype, taking into account input/output registers. In our example we’ll use:  
`void __usercall mtsgpr0(unsigned int value<r29>);`
  4. Repeat for the remaining instructions, for example:  
`void __usercall mtsgpr1(unsigned int<r31>);`  
`void __usercall mtsgpr2(unsigned int<r30>);  
void __usercall mtsgpr3(unsigned int<r30>)  
int __usercall mftb<r3>();`
  5. Refresh the decompilation if it’s not done automatically.



We get something like this:
    
    
    void __fastcall __noreturn sub_C8C(int a1)
    {
      unsigned int v1; // lr
    
      sub_1264();
      mtsgpr0((unsigned __int16)word_3FFFD4);
      mtsgpr1((~a1 << 16) | a1);
      mtsgpr2(v1);
      mtsgpr3(mftb());
      sub_1114();
    }

No more `_asm` blocks! The only remaining wrinkle is the mysterious variable v1 which is marked in orange (“value may be undefined”).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decompile_call2-3.png?width=592&height=255&name=decompile_call2-3.png)

if we look at the assembly, we’ll see that the `r30` passed to `mtsprg2` originates from `r3` set by the `mflr r3` instruction. The instruction reads value of the `lr` (link register), which contains the return address to the caller and thus by definition has no determined value. However, we can use a pseudo function such as GCC’s [`__builtin_return_address`](<https://gcc.gnu.org/onlinedocs/gcc/Return-Address.html>)by specifying this prototype for the `mflr r3` instruction:   
`void * __builtin_return_address ();`

NB: We do not need to use `__usercall` here because `r3` is already the default location for a return value in the PPC ABI.

Finally, the decompilation is looking nice and tidy:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decompile_call3-3.png?width=453&height=264&name=decompile_call3-3.png)

### Complex situations

If you want to automate the process of applying prototypes to instructions, you can use a decompiler plugin or script. For example, see the [vds8](<https://github.com/idapython/src/blob/master/examples/hexrays/vds8.py>) decompiler SDK sample (also shipped with IDA), which handles some of the `SVC` calls in ARM code. In even more complicated cases, such as when some arguments can’t be represented by custom calling convention, or the semantics are better represented by something other than a function call (e.g. the instruction affects multiple registers), you can use a “microcode filter” to generate custom microcode which would then be optimized and converted to C code by the decompiler engine. A great example is the excellent [microAVX plugin](<https://github.com/gaasedelen/microavx/>) by Markus Gaasedelen.

See also: [Decompile as call](<https://hex-rays.com/products/decompiler/manual/interactive.shtml#08>) in the decompiler manual.

---

