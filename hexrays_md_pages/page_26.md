# Hex-Rays Blog – Page 26

**Archived:** 2026-02-20 12:18
**URL:** https://hex-rays.com/blog/page/26

---

## 1. 

**Date:** Posted: Dec 31, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-70-multiple-highlights-in-ida-7-7](https://hex-rays.com/blog/igors-tip-of-the-week-70-multiple-highlights-in-ida-7-7)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #70: Multiple highlights in IDA 7.7

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-70-multiple-highlights-in-ida-7-7>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-70-multiple-highlights-in-ida-7-7>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-70-multiple-highlights-in-ida-7-7>)

I 

Igor Skochinsky ✦ Posted: Dec 31, 2021

![Igor’s tip of the week #70: Multiple highlights in IDA 7.7](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

The last week’s post got preempted by the IDA 7.7 release so I’ll take this opportunity to highlight (ha ha) one of the [new features](<https://hex-rays.com/products/ida/news/7_7/>).

In previous IDA versions we already had [highlight](<https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight/>) with an option to lock it so it remains fixed while browsing the database. In IDA 7.7 it’s been improved so that you can have several highlights active at the same time!

### Setting highlights

Basic usage remains the same: highlight any string you want (by clicking on a word, dragging mouse, or with Shift-arrows), then click the _Lock/unlock current highlight_ button (initially displaying A on a yellow background).   
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hl77_1-3.png?width=212&height=75&name=hl77_1-3.png)

On the first glance the effect seems to be the same: the current highlight is locked and stays on as you browse. However, if you click on another word, you’ll see that the dynamic highlight now uses another color, and the lock button changes color too.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hl77_2-3.png?width=337&height=408&name=hl77_2-3.png)

Now, if you click the button again, the second highlight gets locked and the dynamic highlight switches to the next color. You can keep doing this up to the limit (currently 8 color slots).

### Removing highlights

Removing a locked highlight is pretty straightforward: click on a currently highlighted item in the listing and click on the toolbar button to unlock it. Alternatively, you can use the dropdown menu next to the button to see the currently assigned highlights and clear a specific one by picking the corresponding entry.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hl77_3-3.png?width=245&height=214&name=hl77_3-3.png)

### Changing highlight colors

The highlight colors, like most others, can be changed in the Options > Colors… dialog. Select one of the “Highlight background” entries in the “Background colors” dropdown, then click “Change color” to set the new color.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hl77_4-3.png?width=973&height=313&name=hl77_4-3.png)

### Shortcuts

As can be seen in the screenshot of the dropdown menu, each highlight color has a corresponding shortcut `Ctrl`+`Alt`+digit (digit=1,2,..8), which can be used to set or clear the corresponding highlight directly.

### Other views

The multiple highlight feature is available not only in the disassembly but also in other text-based views of IDA: Structures, Enums, Pseudocode, and even the Hex View, although some of them may be more or less useful that others.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hl77_5-3.png?width=505&height=525&name=hl77_5-3.png)

Hopefully you’ll find this little feature useful in your work!

---

## 2. 

**Date:** Posted: Dec 29, 2021  
**Link:** [https://hex-rays.com/blog/ida-7-7-qt-5-12-2-sources-and-build-scripts](https://hex-rays.com/blog/ida-7-7-qt-5-12-2-sources-and-build-scripts)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.7: Qt 5.12.2 sources & build scripts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-7-qt-5-12-2-sources-and-build-scripts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-7-qt-5-12-2-sources-and-build-scripts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-7-qt-5-12-2-sources-and-build-scripts>)

Arnaud Diederen ✦ Posted: Dec 29, 2021

![IDA 7.7: Qt 5.12.2 sources & build scripts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

A handful of our users have already requested information regarding the Qt 5.15.2 build, that is shipped with IDA 7.7.

The Qt sources used by IDA are: 

  * based on Qt 5.15.2, 
  * to which [the KDE Qt5 patch collection](<https://community.kde.org/Qt5PatchCollection>) has been added, 
  * plus a few custom patches/fixes ([here they are for reference](<https://hex-rays.com/wp-content/uploads/2021/12/qt-5.15.2-hexrays.patch_.zip>)) 



### Rebuilding Qt from source

In order to obtain compatible libs, the simplest way forward is to 

  * [download the full archive](<https://hex-rays.com/products/ida/support/freefiles/qt-5.15.2-full-IDA77.tar.bz2>) – it contains the original 5.15.2 source + KDE patches + Hex-Rays patches 
  * go to the `build/` directory 
  * type `python3 build.py -h`



Note: on Windows, that command should be issued from a Visual Studio 2019 command prompt.

---

## 3. 

**Date:** Posted: Dec 29, 2021  
**Link:** [https://hex-rays.com/blog/ida-7-7-out-of-the-box-support-for-macoss-dark-mode](https://hex-rays.com/blog/ida-7-7-out-of-the-box-support-for-macoss-dark-mode)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.7: out-of-the-box support for macOS’s “dark mode”

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-7-out-of-the-box-support-for-macoss-dark-mode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-7-out-of-the-box-support-for-macoss-dark-mode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-7-out-of-the-box-support-for-macoss-dark-mode>)

Arnaud Diederen ✦ Posted: Dec 29, 2021

![IDA 7.7: out-of-the-box support for macOS’s “dark mode”](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

## Before IDA 7.7

IDA versions up to and including 7.6 have been shipping with versions of the `Qt` toolkit, that didn’t support `macOS`‘s “dark mode”.

Due to popular demand, we came up with a solution in the form of a specific IDA _theme_ , called IDA’s “dark theme”:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida77_dark_theme_intro-1-3.png?width=1263&height=888&name=ida77_dark_theme_intro-1-3.png)

IDA’s dark theme has been very well received, and a lot of users have been using it for their work.

However, the problem with IDA’s dark theme is that it is not _native_ : it consists of a (fairly large) set of CSS statements meant to style widgets so they look as close to what a native dark mode would look like. It has been working reasonably well, but falls short in certain areas.

## IDA 7.7: support for macOS’s dark mode out-of-the-box

With IDA 7.7, we have moved to a newer version of Qt – one that supports the OS’s “dark mode”. Consequently, IDA should now follow the user’s preferences and switch to a dark palette, without the need to switch to IDA’s dark theme.

We therefore suggest that users who previously switched to IDA’s dark theme switch back to the “default” theme (see `Options > Colors`), and see if they don’t prefer it over the dark theme when macOS is running in dark mode.

Here is IDA 7.6’s dark theme: 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida77_dark_theme-3.png?width=1263&height=887&name=ida77_dark_theme-3.png)

…compared to IDA 7.7’s default theme, when macOS is running in dark mode: 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida77_dark_mode-3.png?width=1263&height=887&name=ida77_dark_mode-3.png)

(notice the dialog buttons, the input fields, the “Function”‘s title buttons, …)

## But I like my IDA white!

Immediately after 7.7 was shipped, we received requests from some users who are so used to IDA’s default (i.e., “white”) theme, that they never wanted to switch to IDA’s dark theme (even when macOS is running in dark mode!), and who find it inconvenient that IDA now automatically follows the OS’s natural color palette.

Fortunately, a knowledgeable user let us know about the following workaround; just issue the following commands in `Terminal`, and restart IDA:
    
    
    defaults write com.hexrays.ida NSRequiresAquaSystemAppearance -bool yes
    defaults write com.hexrays.ida64 NSRequiresAquaSystemAppearance -bool yes
    

and IDA will stick to the white default theme (i.e., it will not receive “dark mode” notifications anymore, and thus won’t switch to a dark palette.)

---

## 4. 

**Date:** Posted: Dec 24, 2021  
**Link:** [https://hex-rays.com/blog/ida-7-7-released](https://hex-rays.com/blog/ida-7-7-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.7 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-7-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-7-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-7-released>)

Holly Walsh ✦ Posted: Dec 24, 2021

![IDA 7.7 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Hex-Rays team is thrilled to announce the release of IDA version 7.7!

Our top-notch binary analysis tool IDA Pro’s latest version delivers new features and various enhancements. With updates on some key features, version 7.7 is expected to certainly improve the user experience.

Here are the highlight features and changes introduced in IDA 7.7:

  * iOS15 and macOS 12 support: Updates to how new OS versions are handled.
  * Clang-based C++ parser: IDA now supports an additional parser based on libclang.
  * Golang improvements: Now taking golang analysis to another level.
  * UI candy: Some updates to improve the UI.
  * New processors: Cadence Tensilica Xtensa and the Renesas RX series.
  * Type system: Basic type system support has been enabled for all processors.
  * Decompilers: Ported our decompiler to MIPS64 and various other improvements and fixes.



**And many more, see full updates here:<https://hex-rays.com/products/ida/news/7_7/>**

### How to request the new versions

As usual, the new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to support@hex-rays.com.

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

---

## 5. 

**Date:** Posted: Dec 17, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-69-split-expression](https://hex-rays.com/blog/igors-tip-of-the-week-69-split-expression)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #69: Split expression

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-69-split-expression>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-69-split-expression>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-69-split-expression>)

I 

Igor Skochinsky ✦ Posted: Dec 17, 2021

![Igor’s tip of the week #69: Split expression](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

While using the decompiler, sometimes you may have seen the item named _Split expression_ in the context menu. What does it do and where it can be useful? Let’s look at two examples where it can be applied.

### Structure field initialization

Modern compilers perform many optimizations to speed up code execution. One of them is merging two or more adjacent memory stores or loads into a single wide one. This often happens when writing to nearby structure fields.

For example, when you decompile a macOS program which uses [blocks](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithBlocks/WorkingwithBlocks.html>) and use our [Objective-C analysis plugin](<https://hex-rays.com/products/ida/support/idadoc/1687.shtml>) to analyze the supporting code in a function, you may observe pseudocode similar to the following:
    
    
     block.isa = _NSConcreteStackBlock;
    *(_QWORD *)&block.flags = 3254779904LL;
    block.invoke = sub_10000A159;
    block.descriptor = &stru_10001E0E8;
    block.lvar1 = self;

The `block` variable uses a structure created by the plugin which looks like this:
    
    
    struct Block_layout_10000A088
    {
      void *isa;
      int32_t flags;
      int32_t reserved;
      void (__cdecl *invoke)(Block_layout_10000A088 *block);
      Block_descriptor_1 *descriptor;
      _QWORD lvar1;
    };
    

As you can see, the compiler decided to initialize the two 32-bit `flags` and `reserved` fields in one go using a single 64-bit store. Although technically correct, the pseudocode looks somewhat ugly and not easy to understand at a glance. To tell the decompiler that this write should be treated as two separate ones, right-click the assignment and choose “Split expression”:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/split1-3.png?width=437&height=201&name=split1-3.png)

Once the pseudocode is refreshed, two separate assignments are displayed:
    
    
    block.isa = _NSConcreteStackBlock;
    block.flags = 0xC2000000;
    block.reserved = 0;
    block.invoke = sub_10000A159;
    block.descriptor = &stru_10001E0E8;
    block.lvar1 = self;

The newly 32-bit constant could, for example, be converted to hex or a set of flags using a custom enum.

This example is rather benign because the `reserved` field is set to 0 so the constant was already effectively 32-bit; other situations can be more involved when different distinct values are merged into one big constant.

If necessary, expressions can be split further (e.g. when one value is used to initialize 3 or more fields). You can also revert the split by choosing “Unsplit expression” in the context menu.

### 64-bit variables in 32-bit programs

When handling 64-bit values on processors with 32-bit registers, the compiler has to work with data in 32-bit pieces. This can lead to very verbose code if translated as-is, so our decompiler detects common patterns such as 64-bit math, comparisons or data manipulations and automatically creates 64-bit variables consisting of two 32-bit registers or memory locations. While our heuristics work well in most cases, there may be false positives, when two actually separate 32-bit variables get merged into a 64-bit one. In such situation, you can use “Split expression” on the 64-bit operations involving the variable to split the pair and recover proper, separate variables.

See also: [Hex-Rays interactive operation: Split/unsplit expression](<https://hex-rays.com/products/decompiler/manual/cmd_split.shtml>)

---

## 6. 

**Date:** Posted: Dec 14, 2021  
**Link:** [https://hex-rays.com/blog/hex-rays-is-moving-to-a-subscription-model](https://hex-rays.com/blog/hex-rays-is-moving-to-a-subscription-model)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-rays is moving to a Subscription model !

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-is-moving-to-a-subscription-model>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-is-moving-to-a-subscription-model>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-is-moving-to-a-subscription-model>)

Geoffrey Czokow ✦ Posted: Dec 14, 2021

![Hex-rays is moving to a Subscription model !](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

# Hex-Rays is moving to a subscription model

In 2022, we will update the catalogue of products available under our subscription model.

Our new bundles are **HEX-RAYS Base** , **HEX-RAYS Core** and **HEX-RAYS Ultra.** Those, in addition to our existing products**IDA Pro Standalone** and **IDA Home** will be available under the new subscription model only. 

IDA Home 

cloud-based x64, PPC64, MIPS64 or ARM64 decompiler (beta)   


IDA Pro Standalone 

available as a standalone product 

disassembly only (no decompilers). 

HEX-RAYS Base 

IDA Pro   
x86 & x64 decompilers   
  


HEX-RAYS Core 

IDA Pro   
x86, x64, ARM & ARM64 decompilers   
  


HEX-RAYS Ultra 

IDA Pro   
x86, x64, ARM, ARM64, PPC, PPC64, MIPS & MIPS64 decompilers 

With a subscription you will have access to the latest version of the purchased software, technical support by email, hot fixes for serious issues and vulnerabilities as well as participation in our user-only online forum.

Updated at: 17 Dec 2021

---

## 7. 

**Date:** Posted: Dec 10, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions](https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #68: Skippable instructions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions>)

I 

Igor Skochinsky ✦ Posted: Dec 10, 2021

![Igor’s tip of the week #68: Skippable instructions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In compiled code, you can sometimes find instructions which do not directly represent the code written by the programmer but were added by the compiler for its own purposes or due to the requirements of the environment the program is executing in.

### Skippable instruction kinds

Compiled functions usually have _prolog_ instructions at the start which perform various bookkeeping operations, for example:

  1. preserve volatile registers used in the function’s body;
  2. set up new stack frame for the current function;
  3. allocate stack space for local stack variables;
  4. initialize the stack cookie to detect buffer overflows;
  5. set up exception handlers for the current function.



In a similar manner, a function’s _epilog_ performs the opposite actions before returning to the caller.

In _switch_ patterns there may also be instructions which only perform additional manipulations to determine the destination of an indirect jump and do not represent the actual logic of the code.

To not spend time analyzing such boilerplate or uninteresting code and only show the “real” body of the function, the decompiler relies on processor modules to mark such instructions. 

### Showing skippable instructions

By default skipped instructions are not distinguished visually in any way. To enable their visualization, create a text file `idauser.cfg` with the following contents:
    
    
    #ifdef __GUI__
    PROLOG_COLOR = 0xE0E0E0 // grey
    EPILOG_COLOR = 0xE0FFE0 // light green
    SWITCH_COLOR = 0xE0E0FF // pink
    #endif

Place the file in the user directory (`%appdata%\Hex-Rays\IDA Pro` on Windows, `$HOME/.idapro` on Unix) and restart IDA or reload the database to the observe the effect in the disassembly listing.

Original disassembly:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/skipped_unmarked-3.png?width=372&height=643&name=skipped_unmarked-3.png)

After creating the configuration file:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/skipped_marked-3.png?width=360&height=647&name=skipped_marked-3.png)

As you can see, the first three and last two instructions are highlighted in the specified colors. These instructions will be skipped during decompilation.

### Modifying skippable instructions

There may be situations where you need to adjust IDA’s idea of skipped instructions. For example, IDA may fail to mark some register saves as part of prolog (this may manifest as accesses to uninitialized variables in the pseudocode). In that case, you can fix it manually:

  1. In the disassembly view, select the instruction(s) which should be marked;
  2. invoke Edit > Other > Toggle skippable instructions…;
  3. select the category (prolog/epilog/switch) and click OK.



In case of an opposite problem (IDA erroneously marked some instructions which do necessary work), perform the same actions, except there won’t be a dialog at step 3 – the instructions will be unmarked directly.

More info: [Toggle skippable instructions (Decompiler Manual)](<https://hex-rays.com/products/decompiler/manual/interactive.shtml#06>)

---

## 8. 

**Date:** Posted: Dec 3, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-67-decompiler-helpers](https://hex-rays.com/blog/igors-tip-of-the-week-67-decompiler-helpers)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #67: Decompiler helpers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-67-decompiler-helpers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-67-decompiler-helpers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-67-decompiler-helpers>)

I 

Igor Skochinsky ✦ Posted: Dec 3, 2021

![Igor’s tip of the week #67: Decompiler helpers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We’ve already described [custom types](<https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types/>) used in the decompiled code, but you may also encounter some unusual keywords resembling function calls. They are used by the decompiler to represent operations which it was unable to map to nice C code, or just to make the output more compact. They are listed in the `defs.h` header file that is provided with the decompiler (can be found in `plugins/hexrays_sdk/include` in your IDA directory) but here is a high level overview of the commonly seen ones.

### Partial access macros

Sometimes the code may access smaller parts of a big variable. To not pollute the code with multitudes of casts, the decompiler uses helper macros for this purpose. 

  1. `LOWORD(x),LOWORD(x),LODWORD(x)` return the lowest byte/word/dword of the variable `x` as an unsigned value;
  2. `HIWORD(x),HIWORD(x),HIDWORD(x)` return the corresponding high part;
  3. `BYTE1(x), BYTE2(x)` etc. return individual bytes in the memory order. The variable is considered to start at byte 0 in memory.
  4. same macros but with the S prefix (`SLOBYTE`, `SBYTE1` etc.) return signed values.



Note: this approach may lead to somewhat confusing situations on big endian processors like PPC. Because big-endian data is stored starting from the high byte, the low-order byte of it is stored at the highest memory address and so is accessed using the `HIBYTE` macro. For example, consider a 32-bit variable containing value 0x1A2B3C4D. It will be stored in memory in different order on little-endian(LE) and big-endian(BE) platforms:
    
    
     LE BE
    ┌──┬──┐
    │4D│1A├◄───LOBYTE
    ├──┼──┤
    │3C│2B├◄───BYTE1
    ├──┼──┤
    │2B│3C├◄───BYTE2
    ├──┼──┤
    │1A│4D├◄───HIBYTE
    └──┴──┘
    

### Combining values

Sometimes the compiler needs to represent the opposite operation: two values are combined to make a larger one. For this, “pair” macros are used:

  1. `__PAIR16__(high, low)` creates a 16-bit value from two 8-bit ones. Unlike partial accesses macros, it does not depend on the memory order but uses simple bit shifts, so the result is the same for little- and big-endian code. For example, `__PAIR16__(0x1A, 0x2B)` returns in `0x1A2B` in either situation;
  2. `__PAIR32__`, `__PAIR64__`, `__PAIR128__` perform the corresponding operation for bigger-sized values;
  3. `__SPAIR16__` etc. return signed values.



### Bit and flag manipulations

Some assembly instructions do not have simple C representation so custom helper functions are used.

  1. `__ROLn__(value, count)` and `__RORn__(value, count)` (n=1,2,4,8) represent n-byte left and right bit rotates;
  2. `__OFADD__` and `__OFSUB__` return the overflow flag of addition(subtraction) operation on two values.
  3. `__CFADD__` and `__CFSUB__` perform the same for carry flag.
  4. `__SETP__(x, y)` is used to represent the parity flag generated by expression x-y.



### Overflow-checking multiplications

Recent compilers started adding overflow checks in common situations. For example, when calling `operator new[]`, behind the scenes the compiler has to multiply the size of the elements by their count. If this operation overflows, wrong value may be produced, leading to under-allocation or allocation failure. Programmers may also add manual overflow checks. The following helper functions are used to represent such code patterns:

  1. `is_mul_ok(count, elsize)` represents overflow check on the result of `count*elsize`. It is presumed to return true if the overflow does not happen.
  2. `saturated_mul(count, elsize)` returns either the result of multiplication if it can be calculated safely, or the maximum unsigned integer value of the corresponding size (e.g. `0xFFFFFFFF`). The latter should ensure that the allocation fails in case of overflow. This pattern is commonly used in calls to `operator new[]` in recent versions of Visual C++.



### Value coercion

Sometimes the code treats the same underlying value as different types. For example, the famous inverse square root function from Quake treats a 32-bit floats as an integers and vice versa:
    
    
    float InvSqrt (float x){
        float xhalf = 0.5f*x;
        int i = *(int*)&x;
        i = 0x5f3759df - (i>>1);
        x = *(float*)&i;
        x = x*(1.5f - xhalf*x*x);
        return x;
    }

Although in the source code this conversion is represented using casts and dereferences, in the optimized code they may be replaced by simple moves between registers, especially when using SSE or AVX instructions which use the same registers to store both floating-point and integer values. Thus the decompiler has to use special macros to represent such code:

  1. `COERCE_FLOAT(v)`, `COERCE_DOUBLE(v)`, `COERCE_LONG_DOUBLE(v)` are used to treat the bit pattern of `v` as the corresponding floating-point type.
  2. `COERCE_UNSIGNED_INT(v)` and `COERCE_UNSIGNED_INT64(v)` are used for the opposite conversions.
  3. You may also see `SLODWORD` when a floating-point value is treated as a signed integer.



For example, here’s how pseudocode for the above function looks like when decompiled:
    
    
    double __cdecl InvSqrt(float a1)
    {
      float v2; // [esp+0h] [ebp-8h]
    
      v2 = a1 * 0.5;
      return (float)((1.5
                    - v2 * COERCE_FLOAT(0x5F3759DF - (SLODWORD(a1) >> 1)) * COERCE_FLOAT(0x5F3759DF - (SLODWORD(a1) >> 1)))
                   * COERCE_FLOAT(0x5F3759DF - (SLODWORD(a1) >> 1)));
    }

---

## 9. 

**Date:** Posted: Nov 26, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations](https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #66: Decompiler annotations

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations>)

I 

Igor Skochinsky ✦ Posted: Nov 26, 2021

![Igor’s tip of the week #66: Decompiler annotations](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When working with pseudocode in the decompiler, you may have noticed that variable declarations and hints have comments with somewhat cryptic contents. What do they mean?

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/annot1-3.png?width=641&height=312&name=annot1-3.png)

While meaning of some may be obvious, others less so, and a few appear only in rare situations.

### Variable location

The fist part of the comment is the variable location. For stack variables, this includes its location relative to the stack and frame pointers. For register variables — the register(s) used for storing its value.

In some cases, you may also see the [scattered argloc](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>) syntax. For example:
    
    
    struct12 v78; // 0:r2.8,8:^0.4

This denotes a 12-byte structure stored partially in registers (8 first bytes, beginning at `r2`), and on stack (4 last bytes, starting from stack offset 8).

### Variable attributes

After the location, there may be additional attributes printed as uppercase keywords. Here are the most common possibilities:

  * `BYREF`: address of this variable is taken somewhere in the current function (e.g. for passing to a function call);
  * `OVERLAPPED`: shown when the decompiler did not manage to separate all the variables so some of them ended up being stored in intersecting locations. Usually functions with such variables are also marked with the comment:  
`// local variable allocation has failed, the output may be wrong!`
  * `MAPDST`: another variable has been [mapped](<https://hex-rays.com/products/decompiler/manual/cmd_map_lvar.shtml>) to this one;
  * `FORCED`: this is an explicitly [forced variable](<https://hex-rays.com/products/decompiler/manual/cmd_force_lvar.shtml>).
  * `ISARG`: shown for function arguments (in mouse hint popups);



### User comment

Local variables may also have additional, user-defined comments which can be added using the `/` shortcut or the context menu:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/lvar_cmt-3.png?width=589&height=307&name=lvar_cmt-3.png)

If present, it will be printed at the end of the variable comment, after the annotations.

## Type annotations

In addition to local variables, decompiler can also show annotations in the hints for:

  * Structure and union fields. Offset and type is shown.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/annot_field-3.png?width=432&height=55&name=annot_field-3.png)
  * Global variables. Only the type is shown.
  * functions and function calls. The list of arguments as well as their locations is printed:



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/annot_func-3.png?width=557&height=141&name=annot_func-3.png)

---

