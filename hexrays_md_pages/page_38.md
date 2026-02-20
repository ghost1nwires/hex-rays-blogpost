# Hex-Rays Blog – Page 38

**Archived:** 2026-02-20 12:22
**URL:** https://hex-rays.com/blog/page/38

---

## 1. 

**Date:** Posted: Apr 11, 2019  
**Link:** [https://hex-rays.com/blog/hack-of-the-day-2-command-line-interface-helpers](https://hex-rays.com/blog/hack-of-the-day-2-command-line-interface-helpers)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hack of the day #2: Command-Line Interface helpers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hack-of-the-day-2-command-line-interface-helpers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hack-of-the-day-2-command-line-interface-helpers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hack-of-the-day-2-command-line-interface-helpers>)

Arnaud Diederen ✦ Posted: Apr 11, 2019

![Hack of the day #2: Command-Line Interface helpers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

### The problem

The “command-line input” (CLI), situated at the bottom of IDA’s window, is a very powerful tool to quickly execute commands in the language that is currently selected.

Typically, that language will be `Python`, and one can use helpers such as `idc.here()` to retrieve the address of the cursor location.

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cli-300x182-3.png?width=300&height=182&name=cli-300x182-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/cli-3.png>)

However, when some debuggers such as `GDB` or `WinDbg` are used, the CLI can be switched to one specific to the debugger being used, thereby providing a way to input commands that will be sent the debugger backend.

Alas, when one is debugging using `GDB` (for example), Python-specific helpers such as `idc.here()` are not available in that CLI anymore.

That means users will have to typically copy information from the listings, and then paste it into the CLI, which is very tedious in addition to being error-prone.

### A first approach

An experienced IDA user recently came up to us with this issue, and suggested that we implement some “variable substitution”, before the text is sent to the backend (be it a debugger, or Python)

For example, the markers:

  * `$!` would be replaced with the current address,
  * `$[` with the address of the beginning of the current selection,
  * `$]` with the address of the end of the current selection
  * …



### Where the first approach falls short

We were very enthusiastic about this idea at first, but we quickly realized that this would open a can of worms, which we didn’t feel comfortable opening.

Here are some of the reasons:

  * It’s unclear how things such as an address should be represented. Should it be `0xXXXXXXXX`, `#XXXXXXXX`, or even decimal? Depending on who will receive the text to execute, this matters
  * Whatever markers (such as `$!`) we support, it will never meet all the needs of all our users. It’s probably better if whatever solution we bring, doesn’t rely on a hard-coded set of substitutions.
  * Should expansion take place in string literals?



All-in-all, we decided that it might get very messy, very quickly, and that this first approach of implementing expension in IDA itself, is probably not the strongest idea.

However, the idea is just too good to give up about entirely, and perhaps we can come up with something “lighter”, that could be implemented in IDA 7.2 already (and even before, in fact), and would be helpful most of the time.

### A second approach

IDA ships with `PyQt5`, a set of Python Qt bindings which lets us take advantage of pretty much all the features offered by Qt.

For example, it’s possible to place a “filter” on top of the CLI’s input field, that will perform the expansion, in-place.

The benefits of this are approach are:

  * it will already work in existing IDA releases
  * users can easily extend the set of markers that are recognized
  * it’s written in Python, thus won’t require recompilation when improved
  * since the expansion is performed in-place, it’s clear what is going to be sent to the backend



What follows, is a draft of how this could be done. It currently:

  * only expands `$!` into the current address, and
  * formats addresses as `0xXXXXXXXX`



Perhaps someone will find this useful, and improve on it… (don’t hesitate to contact us at [support@hex-rays.com](<mailto:support@hex-rays.com>) for suggestions!)
    
    
    import re
    from PyQt5 import QtCore, QtGui, QtWidgets
    import ida_kernwin
    dock = ida_kernwin.find_widget("Output window")
    if dock:
        py_dock = ida_kernwin.PluginForm.FormToPyQtWidget(dock)
        line_edit = py_dock.findChild(QtWidgets.QLineEdit)
        if line_edit:
            try:
                line_edit.removeEventFilter(kpf)
            except:
                pass
            class filter_t(QtCore.QObject):
                def eventFilter(self, obj, event):
                    if event.type() == QtCore.QEvent.KeyRelease:
                        self.expand_markers(obj)
                    return QtCore.QObject.eventFilter(self, obj, event)
                def expand_markers(self, obj):
                    text = obj.text()
                    ea = ida_kernwin.get_screen_ea()
                    exp_text = re.sub(r"\$!", "0x%x" % ea, text)
                    if exp_text != text:
                        obj.setText(exp_text)
            kpf = filter_t()
            line_edit.installEventFilter(kpf)
            print("All set")
    

#### Update (April 25th, 2019)

Elias Bachaalany has a follow-up blog post about this topic: <http://0xeb.net/2019/04/climacros-ida-productivity-tool/>

---

## 2. 

**Date:** Posted: Feb 27, 2019  
**Link:** [https://hex-rays.com/blog/hack-of-the-day-1-decompiling-selected-functions](https://hex-rays.com/blog/hack-of-the-day-1-decompiling-selected-functions)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hack of the day #1: Decompiling selected functions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hack-of-the-day-1-decompiling-selected-functions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hack-of-the-day-1-decompiling-selected-functions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hack-of-the-day-1-decompiling-selected-functions>)

Arnaud Diederen ✦ Posted: Feb 27, 2019

![Hack of the day #1: Decompiling selected functions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

## Intended audience

IDA 7.2 users, who have experience with IDAPython and/or the decompiler.

## The problem

As you may already know, the decompilers allow not only decompiling the current function (shortcut `F5`) but also all the functions in the database (shortcut `Ctrl+F5`).A somewhat less-well known feature of the “multiple” decompilation, is that if a range is selected (for example in the `IDA View-A`), only functions within that range will be decompiled. Alas this is not good enough for the use-case of one of users, who would like to be able to select entries in the list provided by the `Functions window`, and decompile those (the biggest difference with the “`IDA View-A` range” approach, is that there can be gaps in the selection — functions that the user doesn’t want to spend time decompiling.)

## The solution

Although IDA doesn’t provide a built-in solution for this particular use-case (it cannot cover them all), we can use IDA’s scriptability to come up with the following IDAPython script, which should offer a very satisfying implementation of the idea described above: 
    
    
    import ida_kernwin
    import ida_funcs
    import ida_hexrays
    class decompile_selected_t(ida_kernwin.action_handler_t):
        def activate(self, ctx):
            out_path = ida_kernwin.ask_file(
                True,
                None,
                "Please specify the output file name");
            if out_path:
                eas = []
                for pfn_idx in ctx.chooser_selection:
                    pfn = ida_funcs.getn_func(pfn_idx)
                    if pfn:
                        eas.append(pfn.start_ea)
                ida_hexrays.decompile_many(out_path, eas, 0)
            return 1
        def update(self, ctx):
            if ctx.widget_type == ida_kernwin.BWN_FUNCS:
                return ida_kernwin.AST_ENABLE_FOR_WIDGET
            else:
                return ida_kernwin.AST_DISABLE_FOR_WIDGET
    ACTION_NAME = "decompile-selected"
    ida_kernwin.register_action(
        ida_kernwin.action_desc_t(
            ACTION_NAME,
            "Decompile selected",
            decompile_selected_t(),
            "Ctrl+F5"))
    class popup_hooks_t(ida_kernwin.UI_Hooks):
        def finish_populating_widget_popup(self, w, popup):
            if ida_kernwin.get_widget_type(w) == ida_kernwin.BWN_FUNCS:
                ida_kernwin.attach_action_to_popup(
                    w,
                    popup,
                    ACTION_NAME,
                    None)
    hooks = popup_hooks_t()
    hooks.hook()

---

## 3. 

**Date:** Posted: Nov 28, 2018  
**Link:** [https://hex-rays.com/blog/ida-7-2-the-mac-rundown](https://hex-rays.com/blog/ida-7-2-the-mac-rundown)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>) [OSX](<https://hex-rays.com/blog/tag/osx>) [Mac](<https://hex-rays.com/blog/tag/mac>) [debugger](<https://hex-rays.com/blog/tag/debugger>) [iOS](<https://hex-rays.com/blog/tag/ios>)

# IDA 7.2 – The Mac Rundown

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-2-the-mac-rundown>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-2-the-mac-rundown>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-2-the-mac-rundown>)

Troy Bowman ✦ Posted: Nov 28, 2018

![IDA 7.2 – The Mac Rundown](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We posted an addendum to the release notes for IDA 7.2: [The Mac Rundown](</products/ida/7_2/the_mac_rundown/>). It dives much deeper into the Mac-specific features introduced in 7.2, and should be great reference material for users interested in reversing the latest Apple binaries. It’s packed full of hints, tricks, and workarounds.

We hope you will find it quite useful!

---

## 4. 

**Date:** Posted: Nov 6, 2018  
**Link:** [https://hex-rays.com/blog/ida-7-2-qt-5-6-3-configure-options-patch](https://hex-rays.com/blog/ida-7-2-qt-5-6-3-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 7.2: Qt 5.6.3 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-2-qt-5-6-3-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-2-qt-5-6-3-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-2-qt-5-6-3-configure-options-patch>)

Arnaud Diederen ✦ Posted: Nov 6, 2018

![IDA 7.2: Qt 5.6.3 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.3 build, that is shipped with IDA 7.2.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.3\configure.bat "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.3-x64"`
    * Note that you will have to build with Visual Studio 2015 or newer, to obtain compatible libs 
  * Linux: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-64" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.3-x64"`
  * Mac OSX: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-clang" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.3-x64"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2018/11/qt-5.6.3_full-IDA72.zip>). You should therefore apply it to your own Qt 5.6.3 sources before compiling, in order to obtain similar binaries (`patch -p 1 < path/to/qt-5_6_3_full-IDA72.patch`) Note that this patch should work without any modification, against the 5.6.3 release as found [there](<https://download.qt.io/archive/qt>). You may have to fiddle with it, if your Qt 5.6.3 sources come from somewhere else.

---

## 5. 

**Date:** Posted: Sep 19, 2018  
**Link:** [https://hex-rays.com/blog/hex-rays-microcode-api-vs-obfuscating-compiler](https://hex-rays.com/blog/hex-rays-microcode-api-vs-obfuscating-compiler)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-Rays Microcode API vs. Obfuscating Compiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-microcode-api-vs-obfuscating-compiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-microcode-api-vs-obfuscating-compiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-microcode-api-vs-obfuscating-compiler>)

Julien De Bona ✦ Posted: Sep 19, 2018

![Hex-Rays Microcode API vs. Obfuscating Compiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

**This is a guest entry written by Rolf Rolles from[Mobius Strip Reverse Engineering](<http://www.msreverseengineering.com/>). His views and opinions are his own, and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to him.**

In this entry, we’ll investigate an in-the-wild malware sample that was compiled by an obfuscating compiler to hinder analysis. We begin by examining its obfuscation techniques and formulating strategies for removing them. Following a brief detour into the Hex-Rays CTREE API, we find that the newly-released microcode API is more powerful and flexible for our task. We give an overview of the microcode API, and then we write a Hex-Rays plugin to automatically remove the obfuscation and present the user with a clean decompilation.

The plugin is [open source](<https://github.com/RolfRolles/HexRaysDeob>) and weighs in at roughly 4KLOC of heavily-commented C++. Additionally, we are also releasing a helpful plugin for aspiring microcode plugin developers called the [Microcode Explorer](<https://github.com/RolfRolles/HexRaysDeob/blob/master/MicrocodeExplorer.cpp>), which will also be distributed with the Hex-Rays SDK in subsequent releases. In brief, for the sample we’ll explore in this entry, its assembly language code looks like this:

![ObfuscatedASM](https://hex-rays.com/hubfs/Imported_Blog_Media/ObfuscatedASM-3.png)

That function’s Hex-Rays decompilation looks like this:

![Obfuscated](https://hex-rays.com/hubfs/Imported_Blog_Media/Obfuscated-Jun-18-2024-08-48-53-0551-AM.png)

Once our deobfuscation plugin is installed, it will automatically rewrite the decompilation to look like this:

![Deobfuscated](https://hex-rays.com/hubfs/Imported_Blog_Media/Deobfuscated-3.png)

# Initial Investigation

The [sample we’ll be examining](<https://www.virustotal.com/#/file/0ac399bc541be9ecc4d294fa3545bbf7fac4b0a2d72bce20648abc7754b3df24/detection>) was given to me by a student in my [SMT-based binary analysis class](<http://www.msreverseengineering.com/training-classes/>). The binary looks clean at first. IDA’s navigation bar doesn’t immediately indicate tell-tale signs of obfuscation:

![Figure-Binary-Navigation](https://hex-rays.com/hubfs/Imported_Blog_Media/Figure-Binary-Navigation-3.png)

The binary is statically linked with the ordinary Microsoft Visual C runtime, indicating that it was compiled with Visual Studio:

![Figure-Binary-CRT](https://hex-rays.com/hubfs/Imported_Blog_Media/Figure-Binary-CRT-3.png)

And finally, the binary has a RICH header, indicating that it was linked with the Microsoft Linker:

![Figure-Binary-RICH](https://hex-rays.com/hubfs/Imported_Blog_Media/Figure-Binary-RICH-3.png)

Thus far, the binary seems normal. However, nearly any function’s assembly and decompilation listings immediately tells a different tale, as shown in the figures at the top of this entry. We can see constants with high entropy, redundant computations that an ordinary compiler optimization would have removed, and an unusual control flow structure.

## Pattern-Based Obfuscation

In the decompilation listing, we see repeated patterns:

![Fig-ctree-Text-MulSub1And](https://hex-rays.com/hubfs/Imported_Blog_Media/Fig-ctree-Text-MulSub1And-3.png)

The underlined terms are identical. With a little thought, we can determine that the underlined sequence always evaluates to 0 at run-time, because:

  * `x` is either even or odd, and `x-1` has the opposite parity
  * An even number times an odd number is always even
  * Even numbers have their lowest bit clear
  * Thus, AND by `1` produces the value `0`



That the same pattern appears repeatedly is an indication that the obfuscating compiler has a repertoire of patterns that it introduces into the code prior to compilation.

## Opaque Predicates

Another note about the previous figure is that the topmost occurrence of the `x*(x-1) & 1` pattern is inside of an `if`-statement with an AND-compound conditional. Given that this expression always evaluates to zero, the AND-compound will fail and the body of the if-statement will never execute. This is a form of obfuscation known as opaque predicates: conditional branches that in fact are not conditional, but can only evaluate one way or the other at runtime.

## Control-Flow Flattening

The obfuscated functions exhibit unusual control flow. Each contains a `switch` statement in a loop (though the “switch statement” is [compiled via binary search](<http://www.msreverseengineering.com/blog/2014/6/23/switch-as-binary-search-part-0>) instead of with a table). This is evidence of a well-known form of obfuscation called “control flow flattening”. In brief, it works as follows:

  1. Assign a number to each basic block.
  2. The obfuscator introduces a **block number variable** , indicating which block should execute. 
  3. Each block, instead of transferring control to a successor with a branch instruction as usual, updates the block number variable to its chosen successor. 
  4. The ordinary control flow is replaced with a switch statement over the block number variable, wrapped inside of a loop. 



The following animation illustrates the control-flow flattening process:

Your browser does not support the video element. Kindly update it to latest version. 

Here’s the assembly language implementation of control flow flattening switch for a small function.

![ASMFlattened](https://hex-rays.com/hubfs/Imported_Blog_Media/ASMFlattened-3.png)

On the first line, `var_1C` — the block number variable mentioned above — is initialized to some random-looking number. Immediately following that is a series of comparisons of `var_1C` against other random-looking numbers. (`var_1C` is copied into `var_20`, and `var_20` is used for comparisons after the first.) The targets of these equality comparisons are the original function’s basic blocks. Each one updates `var_1C` to indicate which block should execute next, before branching back to the code just shown, which will then perform the equality comparisons and select the corresponding block to execute. For blocks with one successor, the obfuscator simply assigns `var_1C` to a constant value, as in the following figure.

![ASMMovUpdate](https://hex-rays.com/hubfs/Imported_Blog_Media/ASMMovUpdate-3.png)

For blocks with two possible successors (such as if-statements), the obfuscator introduces x86 `CMOV` instructions to set `var_1C` to one of two possible values, as shown below:

![ASMCMOVUpdate](https://hex-rays.com/hubfs/Imported_Blog_Media/ASMCMOVUpdate-3.png)

Graphically, each function looks like this:

![Fig1-FlattenedCFG](https://hex-rays.com/hubfs/Imported_Blog_Media/Fig1-FlattenedCFG-3.png)

In the figure above, the red and orange nodes are the switch-as-binary-search implementation. The blue nodes are the original basic blocks from the function (subject to further obfuscation). The purple node at the bottom is the loop back to the beginning of the switch-as-binary-search construct (the red node).

## Odd Stack Manipulations

Finally, we can also see that the obfuscator manipulates the stack pointer in unusual ways. Particularly, it uses `__alloca_probe` to reserve stack space for function arguments and local variables, where a normal compiler would, respectively, use the `push` instruction and reserve space for all local variables at once in the prologue.

![Alloca](https://hex-rays.com/hubfs/Imported_Blog_Media/Alloca-3.png)

IDA has built-in heuristics to determine the numeric argument to `__alloca_probe` and track the effects of these calls upon the stack pointer. However, the output of the obfuscator leaves IDA unable to determine the numeric argument, so IDA cannot properly track the stack pointer.

### Aside: Where did this Binary Come From?

I am not entirely sure how this binary was produced. [Obfuscator-LLVM](<https://github.com/obfuscator-llvm/obfuscator/wiki>) also uses pattern-based obfuscation and control flow flattening, but Obfuscator-LLVM has different patterns than this sample, and there are some superficial differences with how control flow flattening is implemented. Also, Obfuscator-LLVM does not generate opaque predicates, nor the `alloca`-related obfuscation. And, needless to say, the fact that the binary includes the Microsoft CRT and a RICH header is also puzzling. If you have any further information about this binary, please contact me.

Update: following [discussions on twitter](<https://twitter.com/RolfRolles/status/1042375000588599296>) with an Obfuscator-LLVM developer and another knowledgeable individual, in fact, the obfuscating compiler in question is Obfuscator-LLVM, which has been integrated with the Microsoft Visual Studio toolchain. The paragraph above falsely stated that Obfuscator-LLVM used different patterns and did not insert opaque predicates. The author regrets these errors. In theory, the plugin we develop in this entry might work for other binaries produced by the same compilation process, or even for Obfuscator-LLVM in general, but this theory has not been tested and no guarantees are offered.

# Plan of Attack

Now that we’ve seen the obfuscation techniques, let’s break them. 

A maxim I’ve learned doing deobfuscation is that the best results come from working at the same level of abstraction that the obfuscator used. For obfuscators that work on the assembly-language level, historically my best results have come in using techniques that represent the obfuscated code in terms of assembly language. For obfuscators that work at the source- or compiler internal-level, my best results have come from using a decompiled representation. So, for this obfuscator, a Hex-Rays plugin seemed among our best options.

The investigation above illuminated four obfuscation techniques for us to contend with:

  * Pattern-based obfuscation
  * Opaque predicates
  * Alloca-related stack manipulation
  * Control flow flattening



The first two techniques are implemented via pattern substitutions inside of the obfuscating compiler. Pattern-based deobfuscation techniques, for all their downsides, tend to work well when the obfuscator itself employed a repertoire of patterns — especially a limited one — as seems to be the case here. So, we will attack these via pattern matching and replacement.

The `alloca`-related stack manipulation is the simplest technique to bypass. The obfuscator’s non-standard constructs have thwarted IDA’s ordinary analysis surrounding calls to `__alloca_probe`, and hence the obfuscation prevented IDA from properly accounting for the stack differentials induced by these calls. To break this, we will let Hex-Rays do most of the work for us. For every function that calls `__alloca_probe`, we will use the API to decompile it, and then at every call site to `__alloca_probe`, we will extract the numeric value of its sole argument. Finally, we will use this information to create proper stack displacements within the disassembly listing. The [code for this](<https://github.com/RolfRolles/HexRaysDeob/blob/master/AllocaFixer.cpp>) is very straightforward.

As for control flow flattening, this is the most complicated of the transformations above. We’ll get back to it later.

# First Approach: Using the CTREE API

I began my deobfuscation by examining the decompilation of the obfuscated functions and cataloging the obfuscated patterns therein. The following is a partial listing:

![ObfPatterns](https://hex-rays.com/hubfs/Imported_Blog_Media/ObfPatterns-3.png)

Though I later switched to the [Hex-Rays microcode API](<https://www.hex-rays.com/products/ida/7.1/index.shtml>), I started with the CTREE API, the one that has been available since the first releases of the Hex-Rays SDK. It is overall simpler than the microcode API, and has IDAPython bindings where the microcode API currently does not.

The CTREE API provides a data structure representation of the decompiled code, from which the decompilation listing that is presented to the user is generated. Thus, there is a direct, one-to-one correspondence between the decompilation listing and the CTREE representation. For example, an if-statement in the decompilation listing corresponds to a CTREE data structure of type `cif_t`, which contains a pointer to a CTREE data structure of type `cexpr_t` representing the `if`-statement’s conditional expression, as well as a pointer to a CTREE data structure of type `cinsn_t` representing the body of the `if`-statement.

We will need to know how our patterns are represented in terms of CTREE data structures. To assist us, the VDS5 sample plugin from the Hex-Rays SDK helpfully displays the graph of a function’s CTREE data structures. (The third-party plugin [HexRaysCodeXplorer](<https://github.com/REhints/HexRaysCodeXplorer>) implements this functionality in terms of IDA’s built-in graphing capabilities, whereas the VDS5 sample uses the external WinGraph viewer.) The following figure shows decompilation output (in the top left) and its corresponding CTREE representation in graphical form. Hopefully, the parallels between them are clear.

![Fig-ctree-Graph-MulSub1And-WithDecompilation](https://hex-rays.com/hubfs/Imported_Blog_Media/Fig-ctree-Graph-MulSub1And-WithDecompilation-3.png)

To implement our pattern-based deobfuscation rules, we simply need to write functions to locate instances within the function’s CTREE of the data types associated with the obfuscated patterns, and replace them with CTREE versions of their deobfuscated equivalents. For example, to match the `(x-1) * x & 1 ` pattern we saw before, we determine the CTREE representation and write an `if`-statement that matches it, as follows:

![Fig-ctree-Python-MulSub1And](https://hex-rays.com/hubfs/Imported_Blog_Media/Fig-ctree-Python-MulSub1And-3.png)

( 

In practice, these rules should be written more generically when possible. I.e., multiplication and bitwise AND are commutative; the pattern matching code should be able to account for this, and match terms with the operands swapped. Also, see the [open-source project HRAST](<https://github.com/sibears/HRAST>) for an IDAPython framework that offers a less cumbersome approach to pattern-matching and replacement.)

The only point of subtlety in replacing obfuscated CTREE elements with deobfuscated equivalents is that each CTREE expression has associated type information, and we must carefully ensure that our replacements are of the proper type. The easiest solution is simply to copy the type information from the CTREE expression we’re replacing.

## First Major CTREE Issue: Compiler Optimizations

Cataloging the patterns and writing match and replace functions for them was straightforward. However, after having done so, the decompilation showed obvious opportunities for improvement by application of standard compiler optimizations, as shown in the following animation.

Your browser does not support the video element. Kindly update it to latest version. 

This perplexed me at first. I knew that Hex-Rays already implemented these compiler optimizations, so I was confused that they weren’t being applied in this situation. Igor Skochinsky suggested that, while Hex-Rays does indeed implement these optimizations, that they take place during the microcode phase of decompilation, and that these optimizations don’t happen anymore once the CTREE representation has been generated. Thus, I would either have to port my plugin to the microcode world, or write these optimizations myself on the CTREE level. I set the issue aside for the time being and continued with the other parts of the project.

# Control Flow Unflattening via the CTREE API

Next, I began working on the control flow unflattening portion. I envisioned this taking place in three stages. My final solution included none of these steps, so I won’t devote a lot of print space to my early plan. But, I’ll discuss the original idea, and the issues that lead me to my final solution.

  1. Starting from the switch-as-binary-search implementation, rebuild an actual `switch` statement (rather than a mess of nested `if` and `goto` statements). 
  2. Examine how each switch case updates the block number variable to recover the original control flow graph. I.e., each update to the block number variable corresponds to an edge from one block to its numbered target. 
  3. Given the control flow graph, reconstruct high-level control flow structures such as loops, `if`/`else` statements, `break`, `continue`, `return`, and so on. 



I began by writing a CTREE-based component to reconstruct switch statements from obfuscated functions. The basic idea — inspired by the assembly language implementation — is to identify the variable that represents the block number to execute, find equality comparisons of this variable against constant numbers, and extract these numbers (these are the case labels) as well the address of the code that executes if the comparison matches (these are the bodies of the case statements).

This proved more difficult than I expected. Although the assembly language implementations had a predictable structure, Hex-Rays had applied transformations to the high-level control flow which made it difficult to extract the information I was after, as we can see in the following figure.

![Fig-ctree-Unflatten-Unhelpful](https://hex-rays.com/hubfs/Imported_Blog_Media/Fig-ctree-Unflatten-Unhelpful-3.png)

We see above the introduction of a strange `while` loop in the inner `switch`, and the final `if`-statement has been inverted to a `!=` conditional rather than a `==` conditional, which might seem a more logical translation of the assembly code. The example above doesn’t show it, but sometimes Hex-Rays rebuilds small `switch` statements that cover portions of the larger `switch`. Thus, our `switch` reconstruction logic must take into account that these transformations might have taken place.

For ordinary decompilation tasks, these transformations would have been valuable improvements to the output; but in my unusual situation, it meant my switch recovery algorithm was basically fighting against these transformations. My first attempt at rebuilding switches had a lot of cumbersome corner cases, and overall did not work very well.

## Control Flow Reconstruction

Still, I pressed on. I started thinking about how to rebuild high-level control flow structure (`if` statements, `while` loops, `returns`, etc.) from the recovered control flow graph. While it seemed like a fun challenge, I quickly realized that Hex-Rays obviously already includes this functionality. Could I re-use Hex-Rays’ existing algorithms to do that?

Another conversation with Igor lead to a similar answer as before: in order to take advantage of Hex-Rays’ built-in control flow structuring algorithms, I would need to operate at the microcode level instead of the CTREE level. At this point, all of my issues seemed to be pointing me toward the newly-available microcode API. I bit the bullet and started over with the project using the microcode API.

# Overview of the Hex-Rays Microcode API

My first order of business was to read the SDK’s `hexrays.hpp`, which now includes the microcode API. I’ll summarize some of my findings here; I have provided some more, optional information in an appendix.

At Igor’s suggestion, I compiled the VDS9 plugin included with the Hex-Rays SDK. This plugin demonstrates how to generate microcode for a given function (using the `gen_microcode()` API) and print it to the output window (using `mbl_array_t::print()`).

## Microcode API Data Structures

For my purposes, the most important things to understand about the microcode API were four key data structures:

  1. `minsn_t`, microcode instructions.
  2. `mop_t`, operands for microcode instructions.
  3. `mbl_array_t`, which contains the graph for the microcode function.
  4. `mblock_t`, the basic blocks within the microcode graph, which contain the instructions, and the edges between the blocks. 



For the first two points, Ilfak has given an [overview presentation about the microcode instruction set](<https://www.hex-rays.com/wp-content/uploads/2019/12/recon2018.ppt>). For the second two points, he has [published a blog entry](<http://www.hexblog.com/?p=1232>) showing graphically how all of these data structures relate to one another. Aspiring microcode API plugin developers would do well to read those entries; the latter includes many nice figures such as this one:

[![](https://hex-rays.com/hubfs/Imported_Blog_Media/mba-Jun-18-2024-08-48-50-1269-AM.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/mba-Jun-18-2024-08-48-50-1269-AM.png>)

## Microcode Maturity

As Hex-Rays internally optimizes and transforms the microcode, it moves through so-called “maturity phases”, indicated by an enumerated element of type `mba_maturity_t`. For example, immediately after generation, the microcode is said to be at maturity `MMAT_GENERATED`. After local optimizations have been performed, the microcode moves to maturity `MMAT_LOCOPT`. After performing analysis of function calls (such as deciding which pushes onto the stack correspond to which called function), the microcode moves to maturity `MMAT_CALLS`. When generating microcode via the `gen_microcode()` API, the user can specify the desired maturity level to which the microcode should be optimized.

## The Microcode Explorer Plugin

Examining the microcode at various levels of maturity is an informative and impressive undertaking that I recommend for all would-be microcode API plugin developers. It sheds light on which transformations take place in which order, and the textual output is easy to comprehend. At the start of this project, I spent a good bit of time reading through microcode dumps at various levels of maturity.

Though the microcode dump output is very nice and easy to read, its output does not show the low-level details of how the microcode instructions and operands are represented — which is critical information for writing microcode plugins. As such, to understand the low-level representation, I wrote functions to dump `minsn_t` instructions and `mop_t` operands in textual form.

For the benefit of would-be microcode plugin developers, I created a plugin I call the [Microcode Explorer](<https://github.com/RolfRolles/HexRaysDeob/blob/master/MicrocodeExplorer.cpp>). With your cursor within a function, run the plugin. It will ask you to select a decompiler maturity level:

![MEMaturity](https://hex-rays.com/hubfs/Imported_Blog_Media/MEMaturity-3.png)

Once the user makes a selection, the plugin shows a custom viewer in IDA with the microcode dump at the selected maturity level.

![MMAT_LOCOPT](https://hex-rays.com/hubfs/Imported_Blog_Media/MMAT_LOCOPT-3.png)

The microcode dump is mostly non-interactive, but it does offer the user two additional features. First, pressing `G` in the custom viewer will display a graph of the entire microcode representation. For example:

![MicrocodeExplorerBlockGraph](https://hex-rays.com/hubfs/Imported_Blog_Media/MicrocodeExplorerBlockGraph-3.png)

Second, the Microcode Explorer can display the graph for a selected microinstruction and its operands, akin to the VDS5 plugin we saw earlier which displayed a graph of a function’s CTREE representation. Simply position your cursor on any line in the viewer and press the `I` key.

![MEGraph](https://hex-rays.com/hubfs/Imported_Blog_Media/MEGraph-3.png)

The appendix discusses the microcode instruction set in more detail, and I recommend that aspiring microcode API plugin developers read it.

# Pattern Deobfuscation with the Microcode API

Once I had a basic handle on the microcode API instruction set, I began by porting my CTREE-level pattern matching and replacement code to the microcode API. This was more laborious due to the more elaborate nature of the microcode API, and the fact I had to write it in C++ instead of Python. All in all, the porting process was mostly straightforward. The code can be found [here](<https://github.com/RolfRolles/HexRaysDeob/blob/master/PatternDeobfuscate.cpp>), and here’s an example of a pattern match and replacement.

![MicrocodePatternReplace](https://hex-rays.com/hubfs/Imported_Blog_Media/MicrocodePatternReplace-3.png)

Also, I needed to know how to integrate my pattern replacement with the rest of Hex-Rays’ decompiler infrastructure. It was easy enough to write and test my pattern replacement code against the data returned by the `gen_microcode()` API, but doing so has no effect on the decompilation listing that the user ultimately sees (since the decompiler calls `gen_microcode()` internally, and we don’t have access to the `mbl_array_t` that it generates).

The VDS10 SDK sample illustrates how to integrate pattern-replacement into the Hex-Rays infrastructure. In particular, the SDK defines an “instruction optimizer” data type called `optinsn_t`. The virtual method `optinsn_t::func()` is given a microinstruction as input. That method must inspect the provided microinstruction and try to optimize it, returning a non-zero value if it can. Once the user installs their instruction optimizer with the SDK function `install_optinsn_handler()`, their custom optimizer will be called periodically by the Hex-Rays decompiler kernel, thus achieving integration that ultimately affects the user’s view of the decompilation listing.

You may recall that a major impetus for moving the pattern-matching to the microcode world was that, after the replacements had been performed, Hex-Rays had an opportunity to improve the code further via standard compiler optimizations. We showed what we expected the result of such optimizations would be, but no optimizations had been applied when we wrote our pattern-replacement with the CTREE API. By moving to the microcode world, now we do get the compiler optimizations we desire.

After installing our pattern-replacement hook, here’s the decompilation listing for the compiler optimization animation shown earlier:

![PatCompOptsApplied](https://hex-rays.com/hubfs/Imported_Blog_Media/PatCompOptsApplied-3.png)

That’s exactly the result we had been expecting. Great! I didn’t have to code those optimizations myself after all.

### Aside: Tricky Issues with Pattern Replacement in the Microcode World

When we wrote our CTREE pattern matching and replacement code, we targeted a specific CTREE maturity level, which lead to predictable CTREE data structures implementing the patterns. In the microcode world, as discussed more in the appendix, the microcode implementation changes dramatically as it matures. Furthermore, our instruction optimizer callback gets called all throughout the maturity lifecycle. Some of our patterns won’t yet be ready to match at earlier maturity phases; we’ll have to write our patterns targeting the lowest maturity level at which we can reasonably match them.

While porting my CTREE pattern replacement code to the microcode world, at first I also adopted my strategy from the CTREE world of generating my pattern replacement objects from scratch, and inserting them into the microcode atop the terms I wanted to replace. However, I experienced a lot of difficulty in doing so. Since I was new to the microcode API, I did not have a clear mental picture of what Hex-Rays internally expected about my microcode objects, which lead to mistakes (internal errors and a few crashes). I quickly switched strategies such that my replacements would modify the existing microinstruction and microoperand objects, rather than generating my own, which reduced my burden of generating correct `minsn_t` and `mop_t` objects (since this strategy allowed me to start from valid objects).

# Control Flow Unflattening, Overview

To recap, control flow flattening eliminates direct block-to-block control flow transfers. The flattening process introduced a “block number variable” which determines the block that should execute at each step of the function’s execution. Each flattened function’s control flow structure has been changed into a switch over the block number variable, which ultimately shepherds execution to the correct block. Every block must update the block number variable to indicate the block that should execute next after the current one (where conditional branches are implemented via conditional move instructions, updating the block number variable to the block number of either the taken branch, or of the non-taken branch).

The control flow unflattening process is conceptually simple. Put simply, our task is to rebuild the direct block-to-block control flows, and in so doing, eliminate the control flow switch mechanism. Implementation-wise, unflattening is integrated with the Hex-Rays decompiler kernel in a similar fashion to how we integrated pattern-matching. Specifically, we register an `optblock_t` callback object with Hex-Rays, such that our unflattener will be automatically invoked by the Hex-Rays kernel, providing a fully automated experience for the user. 

The next chapter will discuss the implementation in more depth.

In the following subsections, we’ll show an overview of the process pictorially. Just three steps are all we need to remove the control flow flattening. Once we rebuild the original control flow transfers, all of Hex-Rays’ existing machinery for control flow restructuring will do the rest of the work for us. This was perhaps my favorite result from this project; all I had to do was re-insert proper control flow transfers, and Hex-Rays did everything else for me automatically.

## Step #1: Determine Flattened Block Number to Hex-Rays Block Number Mapping

Our first task is to determine which flattened block number corresponds to which Hex-Rays `mblock_t`. The following figure is the microcode-level representation for a small function’s control flow switch: 

![Unflatten1](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten1-3.png)

Hex-Rays is currently calling the block number variable `ST14_4.4`. If that variable matches `0xCBAD6A23`, the `jz` instruction on block @2 transfers control to block @6. Similarly, `0x25F52EB5` corresponds to block @9, and `0x31B8F0BC` corresponds to block @10. The information just described is the mapping between flattened block numbers and Hex-Rays block numbers. (Of course, our plugin will need to extract it automatically.)

## Step #2: Determine Each Flattened Block’s Successors

Next, for each flattened block, we need to determine the flattened block numbers to which it might transfer control. Flattened blocks may have one successor if their original control flow was unconditional, or two potential successors if their original control flow was conditional. First, here’s the microcode from block @9, which has one successor. (Line 9.3 has been truncated because it was long and its details are immaterial.)

![Unflatten2-1](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten2-1-3.png)

We can see on line 9.4 that this block updates the block number variable to `0xCBAD6A23`, before executing a `goto` back to the control flow switch (on the Hex-Rays block numbered @2). From what we learned in step #1, we know that, by setting the block number variable to this value, the next trip through the control flow switch will execute the Hex-Rays `mblock_t` numbered @6.

The second case is when a block has two possible successors, as does Hex-Rays block @6 in the following figure.

![Unflatten2-2](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten2-2-3.png)

Line 8.0 updates the block number variable with the value of `eax`, before line 8.1 executes a `goto` back to the control flow switch at Hex-Rays block @2. If the `jz` instruction on line 6.4 is taken, then `eax` will have the value `0x31B8F0BC` (obtained on line 6.1). If the `jz` instruction is not taken, then `eax` will contain the value `0x25F52EB5` from the assignment on line 7.0. Consulting the information we obtained in step #1, this block will transfer control to Hex-Rays block @10 or @9 during the next trip through the control flow switch.

## Step #3: Insert Control Transfers Directly from Source Blocks to Destinations

Finally, now that we know the Hex-Rays `mblock_t` numbers to which each flattened block shall pass control, we can modify the control flow instructions in the microcode to point directly to their successors, rather than going through the control flow switch. If we do this for all flattened blocks, then the control flow switch will no longer be reachable, and we can delete it, leaving only the function’s original, unflattened control flow. Continuing the example from above, in the analysis in step #2, we determined that Hex-Rays block @9 ultimately transferred control to Hex-Rays block @6. Block @9 ended with a `goto` statement back to the control flow switch located on block @2. We simply modify the target of the existing `goto` statement to point to block @6 instead of block @2, as in the following figure. (Note that we also deleted the assignment to the block number variable, since it’s no longer necessary.)

![Unflatten3-1](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten3-1-3.png)

The case where a block has two potential successors is slightly more complicated, but the basic idea is the same: altering the existing control flow back to the control flow switch to point directly to the Hex-Rays targeted blocks. Here’s Hex-Rays block @6 again, with two possible successors.

![Unflatten2-2](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten2-2-3.png)

To unflatten this, we will: 

  1. Copy the instructions from block @8 onto the end of block @7.
  2. Change the `goto` instruction on block @7 (which was just copied from block @8) to point to block @9 (since we learned in step #1 that `0x25F52EB5` corresponds to block @9). 
  3. Update the `goto` target on block @8 to block @10 (since we learned in step #1 that `0x31B8F0BC` corresponds to block @10). 



We can also eliminate the update to the block number variable on line 8.0, and the assignments to `eax` on lines 6.1 and 7.0.

That’s it! As we make these changes for every basic block targeted by the control flow switch, the control flow switch dispatcher will lose all of its incoming references, at which point we can prune it from the Hex-Rays microcode graph, and then the flattening will be gone for good.

# Control Flow Unflattening, In More Detail

As always, the real world is messier than curated examples. The remainder of this section details the practical engineering considerations that go into implementing unflattening as a fully-automated procedure.

## Heuristically Identifying Flattened Functions

It turns out that a few non-library functions within the binary were not flattened. I had enough work to do simply making my unflattening code work for flattened functions, such that I did not need the added hassle of tracking down issues stemming from spurious attempts to unflatten non-flattened functions.

Thus, I devised a heuristic for determining whether or not a given function was flattened. I basically just asked myself which identifying characteristics the flattened functions have. I looked at the microcode for a control flow switch:

![Unflatten1](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten1-3.png)

Two points came to mind:

  1. The functions compare one variable — the block number variable — against numeric constants in `jz` and `jg` instructions 
  2. Those numeric constants are highly entropic, appearing to have been pseudorandomly generated



With that characterization, the algorithm for heuristically determining whether a function was flattened practically wrote itself.

  1. Iterate through all microinstructions within a function. For this, the SDK handily provides the `mbl_array_t::for_all_topinsns` function, to be used with a class called `minsn_visitor_t`. 
  2. For every `jz` and `jg` instruction that compares a variable to a number, record that information in a list. 
  3. After iteration, choose the variable that had been compared against the largest number of constants.
  4. Perform an entropy check on the constants. In particular, count the number of bits set and divide by the total number of bits. If roughly 50% of the bits were set, decide that the function has been flattened. 



You can see the implementation in the [code](<https://github.com/RolfRolles/HexRaysDeob/blob/master/CFFlattenInfo.cpp>) — specifically the `JZInfo::ShouldBlacklist()` method.

## Simplify the Graph Structure

The flattened functions sometimes have jumps leading directly to other jumps, or sometimes the microcode translator inserts `goto` instructions that target other `goto` instructions. For example, in the following figure, block 4 contains a single `goto` instruction to block 8, which in turn has a `goto` instruction to block 15.

![GotoToGoto](https://hex-rays.com/hubfs/Imported_Blog_Media/GotoToGoto-3.png)

These complicate our later book-keeping, so I decided to eliminate `goto`-to-`goto` transfers. I.e. if block @X ends with a `goto` @N instruction, and block @N contains a single `goto` @M instruction, update the `goto` @N to `goto` @M. In fact, we apply this process recursively; if block @M contained a single `goto` @P, then we would update `goto` @N to `goto` @P, and so on for any number of chained `gotos`.

The Hex-Rays SDK sample VDS11 does what was just described in the last paragraph. [My code](<https://github.com/RolfRolles/HexRaysDeob/blob/master/TargetUtil.cpp>) is similar, but a bit more general, and therefore a bit more complicated. It also handles the case where a block falls through to a block with a single `goto` — in this case, it inserts a new `goto` onto the end of the leading block, with the same destination as the original `goto` instruction in the trailing block.

## Extract Block Number Information

In step #1 of the unflattening procedure described previously, we need to know:

  * Which variable contains the block number
  * Which block number corresponds to which Hex-Rays microcode block



When heuristically determining whether a function appears to have been flattened, we already found the variable with the most conditional comparisons, and the numbers it was compared against. Are we done? No — because as usual, there are complications. Many of the flattened functions use two variables, not one, for block number-related purposes. For those that use two, the function’s basic blocks update a different variable than the one that is compared by the control flow switch construct. I call this the **block update variable**. and I renamed my terminology for the other one to the **block comparison variable**. Toward the beginning of the control flow switch, the value of the block update variable is copied into the block comparison variable, after which all subsequent comparisons reference the block comparison variable. For example, see the following figure:

![TwoBlockVariables](https://hex-rays.com/hubfs/Imported_Blog_Media/TwoBlockVariables-3.png)

In the above, block @1 is the function’s prologue. The control flow switch begins on block @2. Notice that block @1 assigns a numeric value to a variable called `ST18_4.4`. Note that the first comparison in the control flow switch, on line 2.3, compares against this variable. Note also that line 2.1 copies that variable into another variable called `ST14_4.4`, which is then used for the subsequent comparisons (as on line 3.1, and all control flow switch comparisons thereafter). Then, the function’s flattened blocks update the variable `ST18_4`:

![UpdateSecondVariable](https://hex-rays.com/hubfs/Imported_Blog_Media/UpdateSecondVariable-3.png)

(Confusingly, the function’s flattened blocks update both variables — however, only the assignment to the block update variable `ST18_4.4` is used. The block comparison variable, `ST14_4.4`, is redefined on line 2.1 above before its value is used.)

So, we actually have three tasks: 

  1. Determine which variable is the block comparison variable (which we already have from the entropy check).
  2. Determine if there is a block update variable, and if so, which variable it is.
  3. Extract the numeric constants from the `jz` comparisons against the block comparison variable to determine the flattened block number to Hex-Rays `mblock_t` number mapping. 



I quickly examined all of the flattened functions to see if I could find a pattern as to how to locate the block update variable. It was simple enough: for any variable assigned a numeric constant value in the first block, see if it is later copied into the block comparison variable. There should be only one of these. It was easy to code using similar techniques to the entropy check, and it worked reliably.

The code for reconstructing the flattened Hex-Rays block number mapping is nearly identical to the code used for heuristically identifying flattened functions, and so we don’t need to say anything in particular about it.

## Unflattening

From the above, we now know which variable is the block update variable (or block comparison variable, if there is none). We also know which flattened block number corresponds to which Hex-Rays `mblock_t` number. For every flattened block, we need to determine the number to which it sets the block update variable. We walk backwards, from the end of the flattened block region, looking for assignments to the block update variable. If we find an assignment from another variable, we recursively begin tracking the other variable. If we find a number, we’re done.

As described previously, flattened blocks come in two cases: 

  1. The flattened block always sets the block update variable to a single value (corresponding to an unconditional branch). 
  2. The flattened block uses an x86 `CMOV` instruction to set the block update variable to one of two possible values (corresponding to a conditional branch). 



In the first case, our job is simply to find one number. For example, the following flattened block falls into case #1 from above:

![Unflatten2-1](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten2-1-3.png)

In this case, the block update variable is `ST14_4.4`. Our task is to find the numeric assignment on line 9.4. In concert with the flattened block number Hex-Rays `mblock_t` number mapping we extracted from the previous step, we can now change the `goto` on the final line to the proper Hex-Rays `mblock_t` number.

The following flattened block falls into the second case: 

![Unflatten2-2](https://hex-rays.com/hubfs/Imported_Blog_Media/Unflatten2-2-3.png)

Our job is to determine that `ST14_4.4` might be updated to either `0xCBAD6A23` or `0x25F52EB5` on lines 6.0 and 7.0, respectively.

### Complication: Flattened Blocks Might Contain Many Hex-Rays Blocks

This part of the project forced me to contend with a number of complications, some of which aren’t shown by the examples above.

One complication is that a flattened block may be implemented by more than one Hex-Rays `mblock_t` as in the first case above, or more than three Hex-Rays `mblock_t` objects in the second case above. In particular, Hex-Rays splits basic blocks on function call boundaries — so there may be any number of Hex-Rays `mblock_t` objects for a single flattened block. Since we need to work backwards from the end of a flattened region, how do we know where the end of the region is? I solved this problem by computing the function’s [dominator tree](<https://en.wikipedia.org/wiki/Dominator_\(graph_theory\)>) and finding the block dominated by the flattened block header that branches back to the control flow switch.

### Complication: Data-Flow Tracking

Finding the numeric values assigned to the block update variable ranges from trivial to “mathematically hard”. I wound up cheating in the mathematically hard cases.

Sometimes Hex-Rays’ constant propagation algorithms make our lives easy by creating a microinstruction that directly moves a numeric constant into the block update variable. A slightly less simple, but still easy, case is when the assignment to the block update variable involves a number being copied between a few registers or stack variables along the way. As long as there aren’t any errant memory writes to clobber saved values on the stack, it’s easy enough to follow the chain of mov instructions backwards back to the original constant value.

To handle both of these cases, I wrote a function that starts at the bottom of a block and searches for assignments to the block number variable in the backwards direction. For assignments from other variables, it resumes searching for assignments to those variables. Once it finally finds a numeric assignment, it succeeds. 

However, there is a harder case for which the above algorithm will not work. In particular, it will not work when the flattened blocks perform memory writes through pointers, for which Hex-Rays cannot determine legal pointer value sets. Hex-Rays, quite reasonably, can not and does not perform constant propagation across memory values if there are unknown writes to memory in the meantime. Such transformations would break the decompilation listing and cause the analyst not to trust the tool. And yet, this part of the project presents us with the very problem of constant propagation across unknown memory writes.

Here’s an example of the hard case manifesting itself. At the beginning of a flattened block, we see the two destination block numbers being written into registers, and then saved to stack variables.

![HardCase1](https://hex-rays.com/hubfs/Imported_Blog_Media/HardCase1-3.png)

Later on, the flattened block has several memory writes through pointers. 

![HardCase2](https://hex-rays.com/hubfs/Imported_Blog_Media/HardCase2-3.png)

Finally, at the end of the block, the destination block numbers — which were spilled to stack variables at the beginning of the flattened block — are then loaded from their stack slots, and used in a conditional block number update.

![HardCase3](https://hex-rays.com/hubfs/Imported_Blog_Media/HardCase3-3.png)

The problem this presents us is that we need, or Hex-Rays needs, to formally prove that the memory writes in the middle did not overwrite the saved block update numbers. In general, pointer aliasing is an undecidable problem, meaning it is impossible to write an algorithm to solve every instance of it. So instead, I cheated. When my numeric definition scanner encounters an instruction whose memory side effects cannot be bounded, I go to the beginning of the flattened block region and scan forwards looking for numeric assignments to the last variables I was tracking before encountering an unbounded memory reference. I.e., in the three assembly snippets above, I jump to the first one and find the numeric assignments to `var_B4` and `var_BC`. This is a hack; it’s unsafe, and could very well break. But, it happens to work for every function in this sample, and will likely work for every sample compiled by this obfuscating compiler.

# Appendix: More about the Microcode API

What follows are some topics about the Microcode API that I thought were important enough to write up, but I did not want them to alter the narrative flow. Perhaps you can put off reading this appendix until you get around to writing your first microcode plugin.

## The Microcode Verifier

Chances are good that if you’re going use the microcode API, you probably will be modifying the microcode objects described in the previous section. This is murky territory for third-party plugin developers, especially those of us who are new to the microcode API, since modifying the microcode objects in an illegal fashion can lead to crashes or internal errors.

To aid plugin developers in diagnosing and debugging issues stemming from illegal modifications, the microcode API offers “verification”, which is accessible in the API through a method called `mbl_array_t::verify()`. (The other objects also support verification, but their individual `verify()` methods are not currently exposed through the API.) Basically, `mbl_array_t::verify()` applies a comprehensive set of test suites to the microcode objects (such as `mblock_t`, `minsn_t`, and `mop_t`). 

For one example of verification, Hex-Rays has a set of assumptions about the legal operand types for its microinstructions. The m_add instruction must have at least two operands, and those operands must be the same size. m_add can optionally store the result in a “destination” operand; if this is the case, certain destination types are illegal (e.g., in C, it does not make any sense to have a number on the left-hand side of an assignment statement, as in `1 = x + y;`. The analogous concept in the microcode world, storing the result of an addition into a number, also does not make sense and should be rejected as illegal.)

The source code for the `verify()` methods is included in the Hex-Rays SDK under `verifier\verify.cpp`. (There is an analogous version for the CTREE API under `verifier\cverify.cpp`.) When the verifier detects an illegal condition, it raises a numbered “internal error” within IDA, as in the following screenshot. The plugin developer can search for this number within the verifier source code to determine the source of the error.

![IntErr](https://hex-rays.com/hubfs/Imported_Blog_Media/IntErr-3.png)

The verifier source code is, in my opinion, the best and most important source of documentation about Hex-Rays’ internal expectations. It touches on many different parts of the microcode API, and provides examples of how to call certain API functions that may not be covered by the other example plugins in the SDK. Wading through internal errors, tracking them down in the verifier, and learning Hex-Rays’ expectations about the microcode objects (as well as how it verifies them) is a rite of passage for any would-be microcode API plugin developer.

## Intermediate Representations and the Microcode Instruction Set

If you’ve ever studied compilers, you are surely familiar with the notion of an intermediate representation. The `minsn_t` and `mop_t` data types, taken together, are the intermediate represention used in the microcode phase of the Hex-Rays decompiler.

If you’ve studied compilers at an advanced level, you might be familiar with the idea that compilers frequently use more than one intermediate representation. For example, [Muchnick](<https://www.amazon.com/Advanced-Compiler-Design-Implementation-Muchnick/dp/1558603204>) describes a compiler archetype using three intermediate representations, that he respectively calls HIR (“high-level” intermediate representation), MIR (“mid-level”), and LIR (“low-level”). HIR resembles a high-level language such as C, which supports nested expressions. I.e., in C, one may perform multiple operations in a single statement, such as `a = ((b + c) * d) / e`. On the other hand, low-level languages such as LIR or assembly generally can only perform one operation per statement; to represent the same code in a low-level language, we would need at least three statements (ADD, MUL, and DIV). LIR is basically a “pseudo-assembly language”.

So then, given that the Hex-Rays microcode API has only intermediate representation, which type is it — is it closer to HIR, or is it closer to LIR? The answer is, it uses a clever design to simulate both HIR and LIR! As the microcode matures, it is gradually transformed from a LIR-like representation, with only one operation per statement, to a HIR-like representation, with arbitrarily many operations per statement. Let’s take a closer look with the microcode explorer.

When first generating the microcode (i.e., microcode maturity level `MMAT_GENERATED`), we can see that the microcode looks a lot like an assembly language. Notice that each microinstruction has two or three operands apiece, and each operand is something like a number, register name, or name of a global variable. I.e., this is what we would call LIR in a compiler back-end.

![MMAT_GENERATED](https://hex-rays.com/hubfs/Imported_Blog_Media/MMAT_GENERATED-3.png)

Shortly thereafter in the maturity pipeline, in the `MMAT_LOCOPT` phase, we can see that the microcode representation for the same code in the same function is already quite different. In the figure below, many of the lines in the bottom half have complex expressions inside them, instead of the simple operands we saw just previously. I.e., we are no longer dealing with LIR.

![MMAT_LOCOPT](https://hex-rays.com/hubfs/Imported_Blog_Media/MMAT_LOCOPT-3.png)

Finally, at the highest level of microcode maturity, `MMAT_LVARS`, the same code has shrunk down to three lines, with the final one being so long that I had to truncate it to fit it reasonably into the picture:

![MMAT_LVARS](https://hex-rays.com/hubfs/Imported_Blog_Media/MMAT_LVARS-3.png)

## Microinstructions and Microoperands

That’s a pretty impressive trick — supporting multiple varieties of compiler IRs with a single set of data types. How did they do it? Let’s look more carefully at the internal representations of microinstructions and microoperands to figure it out.

Respectively, microinstructions and microoperands are implemented via the `minsn_t` and `mop_t` classes. Here again is the graph representation for a microinstruction: 

![MEGraph](https://hex-rays.com/hubfs/Imported_Blog_Media/MEGraph-3.png)

In the figure above, the top-level microcode instruction is shown in the topmost node. It is represented by an instruction of type `m_and`, which in this case uses three comma-separated operands, of type `mop_d` (result of another instruction), `mop_n` (a number), and `mop_r` (destination is a register). The `mop_d` operand is a compound instruction with two expressions joined together with a bitwise OR — thus, it corresponds to a microinstruction of type `m_or`, whose operands themselves are respectively the result of bitwise AND and bitwise XOR operands, and as such, these operands are of type `mop_d`, instructions respectively of type `m_and` and `m_xor`. The inputs to the AND and XOR operators are all stack variables, i.e., micro-operands of type `mop_S`.

Now we can see how the microcode API supports such dramatic differences in microcode representation using the same underlying data structures. Specifically, the example above makes use of the `mop_d` microoperand type, which refers to the result of another microinstruction. I.e., microinstructions contain microoperands, and microoperands can contain microinstructions (which then contain other microoperands, which may recursively contain other microinstructions, etc). This technique allows the same data structures to represent both HIR- and LIR-like representations. The initial microcode generation phase does not generate `mop_d` operands. Subsequent maturity transformations introduce them in order to build a higher-level representation.

The proper name for this language design technique is mutual recursion: where one category of a grammar refers to another category, and the second refers back to the first. I found this design technique very elegant and clever. Apart from using different data structures at each level of representation, I can’t think of any cleaner ways to accommodate multi-level representations. That said, this type of programming is mostly common only among people with serious professional experience with programming language theory and compiler internals. Ordinary developers would do well to study some programming language theory if they want to make good use of the microcode API.

---

## 6. 

**Date:** Posted: Jun 15, 2018  
**Link:** [https://hex-rays.com/blog/microcode-in-pictures](https://hex-rays.com/blog/microcode-in-pictures)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Microcode in pictures

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/microcode-in-pictures>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/microcode-in-pictures>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/microcode-in-pictures>)

Ilfak Guilfanov ✦ Posted: Jun 15, 2018

![Microcode in pictures](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Since a picture is worth thousand words below are a few drawings for your perusal. Let us start at the top level, with the **mbl_array_t** class, which represents the entire microcode object:

![diagram](https://hex-rays.com/hubfs/Imported_Blog_Media/mba-Jun-18-2024-09-06-48-5129-AM.png)

The above picture does not show the control flow graph. For that we use predecessor and successor lists:

![diagram](https://hex-rays.com/hubfs/Imported_Blog_Media/cfg-3.png)

Pay attention to the block types here, they tell us how many outgoing edges must be present. Then, each basic block (**mblock_t**) contains a list of instructions:

![diagram](https://hex-rays.com/hubfs/Imported_Blog_Media/block-3.png)

Instructions (**minsn_t**) can be nested, and the next drawing shows how it looks like:

![diagram](https://hex-rays.com/hubfs/Imported_Blog_Media/insn-3.png)

As you see, conceptually things are quite simple. But the devil is in the details, as usual 🙂

---

## 7. 

**Date:** Posted: May 25, 2018  
**Link:** [https://hex-rays.com/blog/idapython-wrappers-are-only-wrappers](https://hex-rays.com/blog/idapython-wrappers-are-only-wrappers)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDAPython: wrappers are only wrappers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/idapython-wrappers-are-only-wrappers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/idapython-wrappers-are-only-wrappers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/idapython-wrappers-are-only-wrappers>)

Arnaud Diederen ✦ Posted: May 25, 2018

![IDAPython: wrappers are only wrappers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

# Intended audience

IDAPython developers who enjoy the occasional headache, leaky abstraction enthousiasts, or simply the curious.

# TL;DR

IDAPython wraps C++ types, and the lifecycle of C++ objects (and in particular members of larger objects) is not necessarily the same as that of the Python wrapper object that is wrapping it.

# The problem

One of our users reported IDA crashes when an IDAPython script of theirs. The user came up with a very simple way to reproduce the issue (thank you!), showing that this had to do with accessing the `parents` member of a `ida_hexrays.ctree_visitor_t` instance.

Here is (an even more simplified version of) the script the user sent us:
    
    
    from ida_hexrays import *
    my_parents = None
    class my_visitor_t(ctree_visitor_t):
        def __init__(self, func):
            ctree_visitor_t.__init__(self, CV_PARENTS)
        def visit_expr(self, i):
            global my_parents
            if self.parents is not None:
                my_parents = self.parents
            return 0
    def my_cb(event, *args):
        if event == hxe_print_func:
            f = args[0]
            my_visitor_t(f).apply_to(f.body, None)
            import gc
            gc.collect()
            my_parents.front() # will crash
        return 0
    install_hexrays_callback(my_cb)
    

Note: I threw a `gc.collect()` in there, to make crashes more likely.

The script above is provided in its entirety for the sake of completeness, but really the important lines are only the following:
    
    
        def visit_expr(self, i):
            global my_parents
            if self.parents is not None:
                my_parents = self.parents
        (...)
            my_visitor_t(f).apply_to(f.body, None)
            my_parents.front() # will crash
    

# Details, details, details

Since this issue is non-trivial, I’ll try and provide a step-by-step explanation, hopefully as clear as can be, by annotating the important lines of code mentioned above:
    
    
            my_visitor_t(f)
    

Create a `my_visitor_t` instance. That is a subclass of the `ctree_visitor_t` type, which means it eventually extends a C++ object of type `ctree_visitor_t`.

When the underlying C++ `ctree_visitor_t` object is created, its member named `parents` (a `ctree_items_t` vector) is initialized. For the sake of the example, let’s say the C++ `ctree_visitor_t` instance is located at memory `0x1000` and the `parents` member is placed at memory `0x100C`.
    
    
            .apply_to(f.body, None)
    

Call `ctree_visitor_t::apply_to`. Thanks to SWiG “magic”, C++ virtual method calls will be properly redirected and our `my_visitor_t.visit_expr` method will be called for each `cexpr_t` in the tree, as expected.
    
    
            if self.parents is not None:
    

Access `self.parents`. This will create a Python _wrapper_ object. The key here is to understand that it’s a wrapper object which is backed by the real, C++ `ctree_items_t` instance.

For example, any access to the object returned by `self.parents`, will in fact translate to an access into the C++ `ctree_items_t` vector, so if one were to write, e.g., `self.parents.size()` (or even `len(self.parents)`), it’s actually the real underlying C++ `ctree_items_t` instance’s `size()` method that will end up being called.
    
    
            my_parents = self.parents

Another access to self.parents, and another Python wrapper will be created (once again backed by the actual `ctree_items_t` vector)

[Note: the fact that another wrapper is created is not a problem (in fact since it went out of scope, the previous wrapper might already have been garbage collected!)]

Once again, for the sake of the example, let’s say the wrapping `PyObject` instance is placed in memory, at `0xB000`. That wrapper is then bound to the global variable `my_parents`, causing its python refcount to increase to 2. Past that line, the refcount will drop back to 1 (again, because of scope logic), which means that Python wrapper object will remain alive.
    
    
    [...apply_to() returns, and we are now back to the `my_cb` function...]

At this point, it’s likely `my_visitor_t(f)` has just been garbage collected since nobody keeps a reference to it.

That means:

  * the `my_visitor_t` instance has been destroyed, which means
  * the underlying `ctree_visitor_t` C++ object located at memory `0x1000` has been deleted, which in turn means
  * its `parents` object, which was located at memory `0x100C`, is now invalid
        
        my_parents.front()
        




We are now calling `front()` on the `my_parents` Python object. If you recall, that `my_parents` object is a Python wrapper object located in memory at `0xB000`. That wrapper object still has a refcount of (at least) 1, and is thus alive.

What is not quite alive anymore, however, is the actual C++ `ctree_items_t` vector, which was deleted as part of deleting the C++ `ctree_visitor_t` it belonged to.

In other words, we have a perfectly valid Python wrapper object, that has a dangling pointer to a member of a freshly-deleted C++ object.

# The solution

The solution is, in terms of effort, rather simple: make a copy of the vector:
    
    
            - my_parents = self.parents
            + my_parents = ctree_items_t(self.parents)
    

since it doesn’t belong to the C++ `ctree_visitor_t` object, this copy won’t be thrashed when it is deleted.

---

## 8. 

**Date:** Posted: May 22, 2018  
**Link:** [https://hex-rays.com/blog/deobfucsating-xored-strings](https://hex-rays.com/blog/deobfucsating-xored-strings)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Deobfuscating xor'ed strings

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/deobfucsating-xored-strings>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/deobfucsating-xored-strings>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/deobfucsating-xored-strings>)

Hex-Rays ✦ Posted: May 22, 2018

![Deobfuscating xor'ed strings](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

A few days ago a customer sent us a sample file. The code he sent us was using a very simple technique to obfuscate string constants by building them on the fly and using ‘xor’ to hide the string contents from static disassembly:  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/asm-300x225-3.png?width=300&height=225&name=asm-300x225-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/asm-3.png>)  
The decompiler recovered most of the xor’ed values but some of them were left obfuscated:  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/c1-3.png?width=255&height=185&name=c1-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/c1-3.png>)  
After some investigation it turned out that it is a shortcoming our the decompiler: the value propagation (or constant folding) can not handle the situation when an unusual part of a value is used in another expression. For example, if an instruction defines a four byte value, the second byte of the value can not be propagated to other expressions. More standard cases, like the low or high two bytes, or even just one byte, are handled well.  
It seems that compilers never leave such constants unpropagated, this is why we did not encounter this case before.  
Let us write a short decompiler plugin that would handle this situation and propagate a part of a constant into another expression. The idea is simple: as soon as we find a situation when a constant is used in a binary operation like xor, we will try to find the definition of the second operand, and if it is a constant, then we will propagate it. Graphically it will look like this:
    
    
    mov #N, var.4           ; put a 4 byte constant into var
    ...
    xor var @1.1, #M, var2.1 ; xor the second byte of var
    

is converted into
    
    
    mov #N, var.4
    ...
    xor #N>>8, #M, var2.1
    

The resulting xor will then automatically get optimized by the decompiler. However, to speed up things (to avoid another loop of optimization rules), we will call the optimize_flat() function ourselves.  
Please note that we do not rely on the instruction opcode: the xor opcode can be replaced by any other binary operation, our logic will still work correctly.  
Also we do not rely on the operand sizes (well, to speed up things we do not handle operands wider that 1 byte because they are handled fine by the decompiler).  
Also we can handle not only the second byte, but any byte of the variable.  
The final version of the plugin can be downloaded [here](</wp-content/uploads/2018/05/vds16.zip>). It is fully automatic, you just need to drop it into the plugins/ directory.  
And the decompiler output looks nice now:  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/c2-3.png?width=193&height=53&name=c2-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/c2-3.png>)  
We could further improve the output and convert these assignments into a call to the strcpy() function, but this is left as an exercise for our dear readers 😉  
P.S. Naturally, we will improve the decompiler to handle this case. The next version will include this improvement.

---

## 9. 

**Date:** Posted: May 22, 2018  
**Link:** [https://hex-rays.com/blog/decompiling-floating-point](https://hex-rays.com/blog/decompiling-floating-point)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Decompiling floating point

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/decompiling-floating-point>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/decompiling-floating-point>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/decompiling-floating-point>)

Ilfak Guilfanov ✦ Posted: May 22, 2018

![Decompiling floating point](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

It is a nice feeling, when, after long debugging nights, your software  
finally runs and produces meaningful results. Another hallmark is when other users  
start to use it and obtain useful results. Usually this period is very busy: lots  
of new bugs are discovered and fixed, unforeseen corner cases are handled.  
Then another period starts: when users come back  
for more copies,with more ideas, request more functionality, etc. This is what is happening  
with the decompiler now and I feel it is time to update you with the latest news.

  
In short, things go well. We currently can handle floating point instructions  
for Borland and Visual Studio, and some GCC generated stuff. Problems remain (especially with optimized code)but we advance well. Below are a couple of samples. The first one is very simple. The following assembly function:

_my_sincos proc near    
  
arg_0 = qword ptr 8    
  
push ebp    
mov ebp, esp    
fld [ebp+ arg_0 ]  
fsincos  
fxch st(1)  
fmul st, st  
fxch st(1)  
fmul st, st  
faddp st(1), st  
fsqrt  
mov esp, ebp  
pop ebp    
retn  
_my_sincos endp 

is converted into the following one-liner:

long double __cdecl my_sincos(double a1)  
{  
return sqrt (sin ( a1 ) * sin ( a1 ) + cos ( a1 ) * cos ( a1 ));  
} 

Pretty simple, you may say… Well, here’s a longer one (sorry for the length of the assembler listing, please scroll down):

?ld_ull_test@@YAOO_K@Z proc near    
  
var_40 = qword ptr -40h    
var_38 = qword ptr -38h    
var_30 = qword ptr -30h    
var_28 = qword ptr -28h    
var_20 = qword ptr -20h    
var_18 = qword ptr -18h    
var_10 = qword ptr -10h    
var_8 = qword ptr -8    
arg_0 = qword ptr 8    
arg_8 = qword ptr 10h    
  
push ebp    
mov ebp, esp    
sub esp, 28h    
mov eax, dword ptr [ebp+ arg_8 + 4 ]  
push eax ; int   
mov ecx, dword ptr [ebp+ arg_8 ]  
push ecx ; int   
sub esp,  8  
fld [ebp+ arg_0 ]  
fstp [esp+ 38h + var_38 ]  
call ?ld_ull_add@@YAOO_K@Z   
add esp,  10h  
fstp [ebp+ arg_0 ]  
sub esp,  8  
fld [ebp+ arg_0 ]  
fstp [esp+ 30h + var_30 ]  
call ?ld_ull_cvt@@YA_KO@Z   
add esp,  8  
mov dword ptr [ebp+ arg_8 ], eax  
mov dword ptr [ebp+ arg_8 + 4 ], edx  
mov edx, dword ptr [ebp+ arg_8 + 4 ]  
push edx ; int   
mov eax, dword ptr [ebp+ arg_8 ]  
push eax ; int   
sub esp,  8  
fld [ebp+ arg_0 ]  
fstp [esp+ 38h + var_38 ]  
call ?ld_ull_sub@@YAOO_K@Z   
add esp,  10h  
fstp [ebp+ arg_0 ]  
mov ecx, dword ptr [ebp+ arg_8 + 4 ]  
push ecx ; int   
mov edx, dword ptr [ebp+ arg_8 ]  
push edx ; int   
sub esp,  8  
fld [ebp+ arg_0 ]  
fstp [esp+ 38h + var_38 ]  
call ?ld_ull_mul@@YAOO_K@Z   
add esp,  10h  
fstp [ebp+ arg_0 ]  
mov eax, dword ptr [ebp+ arg_8 ]  
mov ecx, dword ptr [ebp+ arg_8 + 4 ]  
mov dword ptr [ebp+ var_8 ], eax  
mov dword ptr [ebp+ var_8 + 4 ], ecx  
mov edx, dword ptr [ebp+ var_8 + 4 ]  
mov dword ptr [ebp+ var_10 + 4 ], edx  
and dword ptr [ebp+ var_8 + 4 ],  7FFFFFFFh  
fild [ebp+ var_8 ]  
and dword ptr [ebp+ var_10 + 4 ],  80000000h  
mov dword ptr [ebp+ var_10 ],  0  
fild [ebp+ var_10 ]  
fchs  
faddp st(1), st  
fcomp [ebp+ arg_0 ]  
fnstsw ax  
test ah,  41h  
jnz short loc_F0A  
mov eax, dword ptr [ebp+ arg_8 + 4 ]  
push eax ; int   
mov ecx, dword ptr [ebp+ arg_8 ]  
push ecx ; int   
sub esp,  8  
fld [ebp+ arg_0 ]  
fstp [esp+ 38h + var_38 ]  
call ?ld_ull_div@@YAOO_K@Z   
add esp,  10h  
fstp [ebp+ arg_0 ]  
jmp short loc_F2D  
; —————————————————————————  
loc_F0A: ; int   
push  0  
push  4D2h ; int   
mov edx, dword ptr [ebp+ arg_8 + 4 ]  
push edx ; int   
mov eax, dword ptr [ebp+ arg_8 ]  
push eax ; int   
sub esp,  8  
fld [ebp+ arg_0 ]  
fstp [esp+ 40h + var_40 ]  
call ?ld_ull_calc@@YAOO_K0@Z   
add esp,  18h  
fstp [ebp+ arg_0 ]  
loc_F2D:  
mov ecx, dword ptr [ebp+ arg_8 + 4 ]  
push ecx ; int   
mov edx, dword ptr [ebp+ arg_8 ]  
push edx ; int   
sub esp,  8  
fld [ebp+ arg_0 ]  
fstp [esp+ 38h + var_38 ]  
call ?ld_ull_cmpeq@@YA_NO_K@Z   
add esp,  10h  
movzx eax, al  
test eax, eax  
jz short loc_F83  
mov ecx, dword ptr [ebp+ arg_8 ]  
mov edx, dword ptr [ebp+ arg_8 + 4 ]  
mov dword ptr [ebp+ var_18 ], ecx  
mov dword ptr [ebp+ var_18 + 4 ], edx  
mov eax, dword ptr [ebp+ var_18 + 4 ]  
mov dword ptr [ebp+ var_20 + 4 ], eax  
and dword ptr [ebp+ var_18 + 4 ],  7FFFFFFFh  
fild [ebp+ var_18 ]  
and dword ptr [ebp+ var_20 + 4 ],  80000000h  
mov dword ptr [ebp+ var_20 ],  0  
fild [ebp+ var_20 ]  
fchs  
faddp st(1), st  
fstp [ebp+ var_28 ]  
jmp short loc_F89  
; —————————————————————————  
loc_F83:  
fld [ebp+ arg_0 ]  
fstp [ebp+ var_28 ]  
loc_F89:  
fld [ebp+ var_28 ]  
mov esp, ebp    
pop ebp    
retn  
?ld_ull_test@@YAOO_K@Z endp 

The above code is translated into:

double __cdecl ld_ull_test(double a1, __int64 a2)  
{  
double v2 ;  //  st7@1  
double v4 ;  //  [sp+18h] [bp-28h]@5  
double v5 ;  //  [sp+48h] [bp+8h]@1  
double v6 ;  //  [sp+48h] [bp+8h]@1  
double v7 ;  //  [sp+48h] [bp+8h]@2  
unsigned __int64 v8 ;  //  [sp+50h] [bp+10h]@1  
v6  = ld_ull_add ( a1 ,  a2 );  
v8  = ld_ull_cvt ( v6 );  
v2  = ld_ull_sub ( v6 ,  v8 );  
v5  = ld_ull_mul ( v2 ,  v8 );  
if ( ( double ) v8 < =  v5  )  
v7  = ld_ull_calc ( v5 ,  v8 , 1234i64);  
else  
v7  = ld_ull_div ( v5 ,  v8 );  
if ( ld_ull_cmpeq ( v7 ,  v8 ) )  
v4  = ( double ) v8 ;  
else  
v4  =  v7 ;  
return  v4 ;  
} 

I strongly prefer the second listing to the first. In fact, the more I use  
the decompiler, the less I want to return to the assembly level (this means that  
you may expect source level debugging and other similar improvements in the future 😉

In order to handle floating point, we also had to improve many other aspects  
of the decompiler. Here are the things I remember offhand:

  * We changed the stack variable allocation mechanism to use data flow information.  
In practice this means that reused stack frame slots are recognized and multiple  
variables are created for them. No more funny casts because of a stack slot reuse!

  * The stack variables are considered as first class citizens by the propagation and  
other algorithms. Previous versions of the decompiler were optimizing registers  
but stack variables were not optimized much. In practice: shorter and cleaner output.  
This improvement, combined with the previous one, allows us to handle reused  
function stack arguments very smoothly. It goes without saying that aliased  
stack variables are still not optimized (unfortunately, it can not be done  
automatically)

  * Made the optimization rules more robust and more efficient 
  * Added more rules to remove unnecessary casts 
  * Add a new algorithm to recognize call arguments 
  * Better user interface (as usual, improving ui is always a good idea 😉 



This list could go on with more details but let’s stop here.  
Since there are some substantial changes, we will make a beta testing for the next  
release. It is not that far away now – probably even this month!

---

