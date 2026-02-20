# Hex-Rays Blog – Page 21

**Archived:** 2026-02-20 12:16
**URL:** https://hex-rays.com/blog/page/21

---

## 1. 

**Date:** Posted: Aug 16, 2022  
**Link:** [https://hex-rays.com/blog/ida-teams-documentation-published](https://hex-rays.com/blog/ida-teams-documentation-published)

[Back](<https://hex-rays.com/blog>)

[IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Collaboration](<https://hex-rays.com/blog/tag/collaboration>) [Teamwork](<https://hex-rays.com/blog/tag/teamwork>)

# IDA Teams: Documentation published

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-teams-documentation-published>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-teams-documentation-published>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-teams-documentation-published>)

A 

Alex Petrov ✦ Posted: Aug 16, 2022

![IDA Teams: Documentation published](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We have recently announced the launch of IDA Teams, our new product that allows teams of analysts to work together. The heart of IDA remains unchanged, but your team now has better tools to publish their discoveries to the rest of the colleagues and benefit from the work done by other group members. To better understand what IDA Teams is capable of, we have published its documentation on our website.

If you are curious, we encourage you to check the new files: <https://hex-rays.com/documentation/>   
Want to know more about the available bundles or request a demo? Visit: <https://hex-rays.com/ida-teams/>

---

## 2. 

**Date:** Posted: Aug 12, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information](https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #102: Resetting decompiler information

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information>)

I 

Igor Skochinsky ✦ Posted: Aug 12, 2022

![Igor’s tip of the week #102: Resetting decompiler information](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

While working with pseudocode, you may make various changes to it, for example:

  * [add comments](<https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output/>)
  * [rename local variables and change their types](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)
  * [collapse code blocks](<https://hex-rays.com/blog/igors-tip-of-the-week-100-collapsing-pseudocode-parts/>)
  * [map variables](<https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables/>)
  * mark [skippable instructions](<https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions/>)
  * [split expressions](<https://hex-rays.com/blog/igors-tip-of-the-week-69-split-expression/>)
  * [adjust variadic arguments](<https://hex-rays.com/blog/igors-tip-of-the-week-101-decompiling-variadic-function-calls/>)
  * select [union members](<https://hex-rays.com/blog/igors-tip-of-the-week-75-working-with-unions/>)
  * and so on



If the results of some actions do not look better, you can always undo, but what to do if you discover a problem long after the action which caused it?

In fact, there is a way to reset specific or all user customizations at once.

### Reset decompiler information

By Invoking Edit > Other > Reset decompiler information… you get the following dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-reset1-3.png?width=196&height=250&name=hr-reset1-3.png)

Here, you can pick what kinds of information to reset. The fist several options reset information specific to the current function while the last one also resets caches, such as the microcode and pseudocode caches, for all functions, as well as the [global cross references](<https://hex-rays.com/blog/igors-tip-of-the-week-18-decompiler-and-global-cross-references/>) cache.

See also:

[Decompiler Manual: Interactive operation](<https://www.hex-rays.com/products/decompiler/manual/interactive.shtml#07>)

---

## 3. 

**Date:** Posted: Aug 5, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-101-decompiling-variadic-function-calls](https://hex-rays.com/blog/igors-tip-of-the-week-101-decompiling-variadic-function-calls)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #101: Decompiling variadic function calls

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-101-decompiling-variadic-function-calls>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-101-decompiling-variadic-function-calls>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-101-decompiling-variadic-function-calls>)

I 

Igor Skochinsky ✦ Posted: Aug 5, 2022

![Igor’s tip of the week #101: Decompiling variadic function calls](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

[Variadic functions](<https://en.wikipedia.org/wiki/Variadic_function>) are functions which accept different number of arguments depending on the needs of the caller. Typical examples include `printf` and `scanf` in C and C++ but there are other functions, or even some custom ones (specific to the binary being analyzed). Because each call of a variadic function may have a different set of arguments, they need special handling in the decompiler. In many cases the decompiler detects such functions and their arguments automatically but there may be situations where user intervention is required.

### Changing the function prototype

For standard variadic functions IDA usually applies the prototype from a [type library](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>) but if there’s a non-standard function or IDA did not detect that a function is variadic, you can do it manually. For example, a decompiled prototype of unrecognized variadic function on ARM64 may look like this:
    
    
    void __fastcall logfunc(
            const char *a1,
            __int64 a2,
            __int64 a3,
            __int64 a4,
            __int64 a5,
            __int64 a6,
            __int64 a7,
            __int64 a8,
            char a9)
    

But when you inspect the call sites, you see that most of the passed arguments are marked as possibly uninitialized (orange color):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/variadic_badproto-3.png?width=1187&height=222&name=variadic_badproto-3.png)

The first argument looks like a format string so the rest are likely variadic. So we can try to [change the prototype](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>) to:
    
    
    void logfunc(const char *, ...);

which results in clean decompilation:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/variadic_goodproto-3.png?width=878&height=117&name=variadic_goodproto-3.png)

### Adjusting variadic arguments

With correct prototypes, decompiler usually can guess the actual arguments passed to each invocation of the function. However, in some cases the autodetection can misfire, especially if the function uses non-standard format specifiers or does not use a format string at all. In such case, you can adjust the actual number of arguments being passed to the call. This can be done via the context menu commands “Add variadic argument” and “Delete variadic argument”, or the corresponding shortcuts Numpad `+` and Numpad `-`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/variadic_addremove-3.png?width=939&height=209&name=variadic_addremove-3.png)

### Variadic calls and tail branch optimization

In some rare situations you may run into the following issue: when trying to add or remove variadic arguments, the decompiler seems to ignore the action. This may occur in functions subjected to a specific optimization. For example, here’s pseudocode of a function which seems to have two calls to a logging function:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/variadic_tailbranch1-3.png?width=1200&height=250&name=variadic_tailbranch1-3.png)

The decompiler has decided that `a3` is also passed to the calls, however we can see that the format strings do not have any format specifiers so a3 is a false positive and should be removed. However, using “Delete variadic argument” on the first call seems to have no effect. What’s happening?

This is one of the rare cases where switching to disassembly can clear things up. By pressing `Tab`, we can see a curious picture in the disassembly: there is only one call!

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/variadic_tailbranch2-3.png?width=891&height=623&name=variadic_tailbranch2-3.png)

This is an example of so-called _tail branch merging_ optimization, where the same function call is reused with different arguments. For better code readability, the decompiler detects this situation and creates a duplicate call statement with the second set of arguments. Because the information about the number of variadic arguments is attached to the actual call instruction, it can’t be changed for the “fake” call inserted by the decompiler. You can change it for the “canonical” one which can be discovered by pressing `Tab` on the call (`BL` instruction). Removing the argument there affects both calls in the pseudocode.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/variadic_tailbranch3-3.png?width=1192&height=684&name=variadic_tailbranch3-3.png)

If you’re curious to see the “original” code, it can be done by turning off “Un-merge tail branch optimization” in the decompiler’s [Analysis Options 1](<https://hex-rays.com/blog/igors-tip-of-the-week-56-string-literals-in-pseudocode/>).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/tailbranch_opt1-3.png?width=280&height=461&name=tailbranch_opt1-3.png)

With it off, there is only one call just like in the disassembly, at the cost of an extra `goto` and some local variables:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/tailbranch_opt2-3.png?width=933&height=284&name=tailbranch_opt2-3.png)

See also:

[Hex-Rays interactive operation: Add/del variadic arguments (hex-rays.com)](<https://www.hex-rays.com/products/decompiler/manual/cmd_variadic.shtml>)

---

## 4. 

**Date:** Posted: Aug 3, 2022  
**Link:** [https://hex-rays.com/blog/ida-8-0-qt-5-12-2-sources-build-scripts](https://hex-rays.com/blog/ida-8-0-qt-5-12-2-sources-build-scripts)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.0: Qt 5.12.2 sources & build scripts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-0-qt-5-12-2-sources-build-scripts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-0-qt-5-12-2-sources-build-scripts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-0-qt-5-12-2-sources-build-scripts>)

Arnaud Diederen ✦ Posted: Aug 3, 2022

![IDA 8.0: Qt 5.12.2 sources & build scripts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

A handful of our users have already requested information regarding the Qt 5.15.2 build, that is shipped with IDA 8.0.

The Qt sources used by IDA are: 

  * based on Qt 5.15.2, 
  * to which [the KDE Qt5 patch collection](<https://community.kde.org/Qt5PatchCollection>) has been added, 
  * plus a few custom patches/fixes 



### Rebuilding Qt from source

In order to obtain compatible libs, the simplest way forward is to 

  * [download the full archive](<https://hex-rays.com/products/ida/support/freefiles/qt-5.15.2-full-IDA80.tar.bz2>) – it contains the original 5.15.2 source + KDE patches + Hex-Rays patches 
  * go to the `build/` directory 
  * type `python3 build.py -h`



Note: on Windows, that command should be issued from a Visual Studio 2019 command prompt.

---

## 5. 

**Date:** Posted: Aug 1, 2022  
**Link:** [https://hex-rays.com/blog/ida-8-0-released](https://hex-rays.com/blog/ida-8-0-released)

[Back](<https://hex-rays.com/blog>)

[IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.0 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-0-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-0-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-0-released>)

Geoffrey Czokow ✦ Posted: Aug 1, 2022

![IDA 8.0 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Hex-Rays team is thrilled to announce the release of IDA version 8.0!

As with every release, IDA Pro and IDA Home gained many new features and enhancements, including:

  * Support for iOS16 and recent macOS releases
  * Golang improvements: significantly improved support for Golang 1.17 and 1.18 binaries
  * Better function discovery with the new “patfind” plugin
  * FLIRT patterns directly from an IDA database, using the new “makepat” plugin
  * Decompilers: support for outlined functions, and a new ARC decompiler (IDA Teams-only)



See full updates here: <https://hex-rays.com/products/ida/news/8_0/>

## IDA Teams

In addition to those new features & improvements, this 8.0 release sees the introduction, and first-ever availability of [IDA Teams](</ida-teams/>)

[IDA Teams](</ida-teams/>) is our brand new, collaboration-ready product meant to facilitate teamwork across teams of reverse-engineers. We’re very excited by this first release – and we have great plans for its future, too!

![ida Teams](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida_teams_logo-Jun-18-2024-08-54-56-8563-AM.png?width=200&name=ida_teams_logo-Jun-18-2024-08-54-56-8563-AM.png)

### Getting IDA Teams

Interested in IDA Teams? We offer the following upgrade options to corporate users. Your options depend on your licenses at the release date:

  * if you have IDA Pro floating licenses: you can convert those, free of charge, to IDA Teams licenses. This offer is valid until 2022-10-31.
  * if you have IDA Pro non-floating licenses: it’s possible to upgrade those to IDA Teams as well, but the upgrade is not 100% free. Please contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) for a quote. Specify the number of seats you require, and what bundle you are interested in (see <https://www.hex-rays.com/ida-teams/>.) We offer a significant discount until 2022-10-31.
  * if you do not have licenses yet, or wish to acquire new licenses: please contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) for a quote.



Note that IDA Teams is not an add-on for the existing IDA Pro licenses: it is a new product. IDA Teams uses a subscription-based licensing model.

## How to request the new versions

As usual, the new versions of IDA Pro and IDA Home are free for users with an active support plan. Please use the “Help, Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can submit your ida.key to <https://www.hex-rays.com/updida> and our servers will prepare new download links for all your licenses. Please forgive us if your request will take some time to be processed, especially today, after the release. Thanks for your patience.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to [support@hex-rays.com](<mailto:support@hex-rays.com>)

Please consider that only IDA Pro & IDA Home (i.e., not IDA Teams) can be renewed through this usual channel at the moment. Contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) if you are interested in receiving a quote for IDA Teams!

### Has your support plan expired?

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

---

## 6. 

**Date:** Posted: Jul 29, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-100-collapsing-pseudocode-parts](https://hex-rays.com/blog/igors-tip-of-the-week-100-collapsing-pseudocode-parts)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #100: Collapsing pseudocode parts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-100-collapsing-pseudocode-parts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-100-collapsing-pseudocode-parts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-100-collapsing-pseudocode-parts>)

I 

Igor Skochinsky ✦ Posted: Jul 29, 2022

![Igor’s tip of the week #100: Collapsing pseudocode parts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When working with big functions in the decompiler, it may be useful to temporarily hide some parts of the pseudocode to analyze the rest. While currently it’s not possible to hide arbitrary lines [like in disassembly](<https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing/>), you can hide specific sections of it.

### Collapsing local variable declarations

While the local variable declarations are useful to see the overall layout of the stack frame and other [interesting info](<https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations/>), in big functions they may take up a lot of valuable screen estate. To get them out of the way, you can use “Collapse declarations…” from the context menu, or the `-` key on the numpad.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hxcollapse1-3.png?width=454&height=360&name=hxcollapse1-3.png)

This replaces the declarations with a single comment line. To show them again, use “Uncollapse declarations..” or the numpad `+` key.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hxcollapse2-3.png?width=617&height=251&name=hxcollapse2-3.png)

To always collapse the declarations by default, set COLLAPSE_LVARS option in `cfg/hexrays.cfg`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hxcollapse5-3.png?width=929&height=455&name=hxcollapse5-3.png)

### Collapsing statements

Compound statements can be collapsed too: `if` and `switch` statements, as well as `for`, `while`, and `do` loops. This can be done using the “Collapse item” context menu command, or the same numpad `-` shortcut.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hxcollapse3-3.png?width=333&height=272&name=hxcollapse3-3.png)

After collapsing, the whole statement is replaced by one line with the keyword and ellipsis:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hxcollapse4-3.png?width=306&height=120&name=hxcollapse4-3.png)

And can be uncollapsed again from context menu or the numpad `+` key.

You can use this approach to progressively hide analyzed code and tackle long functions piece by piece.

See also

[Hex-Rays interactive operation: Hide/unhide C statements (hex-rays.com)](<https://www.hex-rays.com/products/decompiler/manual/cmd_hide.shtml>)

---

## 7. 

**Date:** Posted: Jul 22, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-99-enums](https://hex-rays.com/blog/igors-tip-of-the-week-99-enums)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #99: Enums

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-99-enums>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-99-enums>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-99-enums>)

I 

Igor Skochinsky ✦ Posted: Jul 22, 2022

![Igor’s tip of the week #99: Enums](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

In IDA, an _enum_ (from “enumeration”) is a set of symbolic constants with numerical values. They can be thought of as a superset of C/C++ enum types and preprocessor defines.

These constants can be used in disassembly or pseudocode to replace specific numbers or their combinations with symbolic names, making the listing more readable and understandable. 

### Creating enums manually

The Enums view is a part of the default IDA desktop layout, but it can also be opened via View > Open subviews > Enumerations, or the shortcut `Shift`–`F10`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums1-3.png?width=1027&height=388&name=enums1-3.png)

To add a new enum, use “Add enum…” from the context menu, or the shortcut `Ins` (`I` on Macs).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums2-3.png?width=323&height=423&name=enums2-3.png)

In the dialog you can specify the name, width (size in bytes), and numerical radix for the symbolic constants.

Once the enum has been created, you can start adding constants to it. For this, use “Add enum member…” from the context menu, or the shortcut `N`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums3-3.png?width=597&height=570&name=enums3-3.png)

An enum may have multiple constants with the same value but the names of all constants must be unique.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums4-3.png?width=421&height=169&name=enums4-3.png)

### Creating enums via Local Types

Local Types view can also be used for creating enums. Simply press `Ins`, write a C syntax definition in the text box and click OK.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums5-3.png?width=462&height=352&name=enums5-3.png)

To make the enum available in the Enums view, so that it can be used in the disassembly, use “Synchronize to idb” from the context menu, or simply double-click the newly added enum type.

### Importing enums from type libraries

Instead of creating an enum from scratch, you can also make use of type libraries shipped with IDA, which include enums from system headers and SDKs. If you know a name of the enum or one of its members, you can check if they’re present in the loaded type libraries. For this, use one of the two link buttons available in the “Add enum” dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums6-3.png?width=323&height=423&name=enums6-3.png)

If you click one or the other, IDA will show you the list of all enums or members(symbols) available in the currently loaded [type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>). 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums7-3.png?width=921&height=632&name=enums7-3.png)

If you know the standard enum name beforehand, simply enter it in the “Add enum” dialog and IDA will automatically import it if a match is found in a loaded type library.

### Using enums

Enums can be used to replace (almost) any numerical value in the disassembly or pseudocode by a symbolic constant. This can be done from the context menu on a number:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums8-3.png?width=536&height=529&name=enums8-3.png)

Or by pressing the shortcut M, which shows a chooser:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums10-3.png?width=874&height=212&name=enums10-3.png)

The list of enum members is automatically narrowed down to those matching the number in the disassembly/pseudocode.

To see the value of the symbolic constant after conversion, [hover the mouse](<https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida/>) over it:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums11-3.png?width=740&height=255&name=enums11-3.png)

See also:

[IDA Help: Enums window](<https://www.hex-rays.com/products/ida/support/idadoc/594.shtml>)

[IDA Help: Convert operand to symbolic constant (enum)](<https://www.hex-rays.com/products/ida/support/idadoc/473.shtml>)

---

## 8. 

**Date:** Posted: Jul 18, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options](https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #98: Analysis options

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options>)

I 

Igor Skochinsky ✦ Posted: Jul 18, 2022

![Igor’s tip of the week #98: Analysis options](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

The autoanalysis engine is the heart of IDA’s disassembly functionality. In most cases it “just works” but in rare situations tweaking it may be necessary.

### Analysis options

The generic analysis options are available in Options > General, Analysis tab, Kernel Options 1..3.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/analysis1-3.png?width=584&height=511&name=analysis1-3.png)![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/analysis2-3.png?width=835&height=461&name=analysis2-3.png)

The same settings are also available at the initial load time.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/analysis3-3.png?width=608&height=485&name=analysis3-3.png)

You can even turn off the autoanalysis completely by unchecking the “Enabled” checkbox. This can be useful, for example, if you have some custom analysis scripts or plugins specific to the target and want to run them before IDA’s own analysis. After running the scripts, analysis can be re-enabled to handle the remaining parts of the binary.

### Processor-specific options

Some processor modules have additional options which are accessible via the “Processor options” button.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/analysis4-3.png?width=919&height=460&name=analysis4-3.png)

### Toggling autoanalysis

In some situations where IDA does not behave correctly or even getting in your way, instead of looking for a specific setting to disable, it may suffice to quickly disable autoanalysis, perform some action, then enable it again for default behavior. For example, if you try to use the technique described in the [tip #87](<https://hex-rays.com/blog/igors-tip-of-the-week-87-function-chunks-and-the-decompiler/>) without deleting the function first, you may find yourself fighting the autoanalysis:

  1. try to truncate the function at the start of the shared tail using “Set Function End” (shortcut `E`);
  2. IDA truncates the main function and creates a separate tail chunk;
  3. because the function (head chunk) and the tail are adjacent, the autoanalysis immediately merges them back into one big function.



To prevent the undesired merging, you can disable autoanalysis, perform the necessary manipulations, then re-enable it. This can be done by unchecking the “Enabled” checkbox in the Options dialog but there is a faster way: autoanalysis indicator button on the toolbar.

It is usually either a yellow circle with hourglass (autoanalysis in progress) or green circle (autoanalysis idle, waiting for user action). If you click the button, it will turn into a crossed red circle to indicate that autoanalysis has been disabled.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/analysis5-3.png?width=360&height=61&name=analysis5-3.png)

Click on the crossed circle again to re-enable autoanalysis.

See also:

[IDA Help: Analysis options (hex-rays.com)](<https://www.hex-rays.com/products/ida/support/idadoc/620.shtml>)

[Igor’s tip of the week #09: Reanalysis – Hex Rays (hex-rays.com)](<https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis/>)

---

## 9. 

**Date:** Posted: Jul 8, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-97-cross-reference-depth](https://hex-rays.com/blog/igors-tip-of-the-week-97-cross-reference-depth)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #97: Cross reference depth

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-97-cross-reference-depth>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-97-cross-reference-depth>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-97-cross-reference-depth>)

I 

Igor Skochinsky ✦ Posted: Jul 8, 2022

![Igor’s tip of the week #97: Cross reference depth](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We have covered basic usage of [cross-references](<https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references/>) [before](<https://hex-rays.com/blog/igor-tip-of-the-week-17-cross-references-2/>), but there are situations where they may not behave as you may expect.

### Accessing large data items

If there is a large structure or an array and the code reads or writes data deep inside it, you may not see cross-references from that code listed at the structure definition.

Example

For example, in the Microsoft CRT function `__report_gsfailure`, there are writes to the fields `_Rip` and `_Rsp` of the `ContextRecord` variable (an instance of a structure `_CONTEXT`), but if we check the cross-references to `ContextRecord`, we will not see those writes listed.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefdepth1-3.png?width=939&height=222&name=xrefdepth1-3.png)

This happens because these fields are situated rather far from the start of the structure (offsets `0x98` and `0xF8`).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefdepth4-3.png?width=682&height=184&name=xrefdepth4-3.png)

As a speed optimization, IDA only checks for direct accesses into large data items up to a limited depth. The default value is 16(0x10), so any accesses beyond that offset will not be shown. The value for current database can be changed via Options > General… Cross-references tab.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefdepth2-3.png?width=582&height=242&name=xrefdepth2-3.png)

For example, after setting it to 256, the accesses to `_Rip` and `_Rsp` are shown in the cross-references to `ContextRecord` :

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/xrefdepth3-3.png?width=875&height=318&name=xrefdepth3-3.png)

To change the limit for all new databases, change the parameter `MAX_TAIL` in `ida.cfg`.

See also: 

[IDA Help: Cross References Dialog](<https://www.hex-rays.com/products/ida/support/idadoc/607.shtml>)

---

