# Hex-Rays Blog – Page 13

**Archived:** 2026-02-20 12:13
**URL:** https://hex-rays.com/blog/page/13

---

## 1. 

**Date:** Posted: Jun 16, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-144-macros-and-simplified-instructions](https://hex-rays.com/blog/igors-tip-of-the-week-144-macros-and-simplified-instructions)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #144: Macros and simplified instructions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-144-macros-and-simplified-instructions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-144-macros-and-simplified-instructions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-144-macros-and-simplified-instructions>)

I 

Igor Skochinsky ✦ Posted: Jun 16, 2023

![Igor’s Tip of the Week #144: Macros and simplified instructions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Many processors (especially RISC based) use instruction sets with fixed size (most commonly 4 bytes). Among examples are ARM, PPC, MIPS and a few others. This is also obvious in the disassembly when observing the instructions’ addresses – they increase by a fixed amount:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros1-3.png?width=733&height=464&name=macros1-3.png)

However, occasionally you may come across larger instructions:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros2-3.png?width=971&height=115&name=macros2-3.png)

What is this? Does A64 ISA have 8-byte instructions?

In fact, if you check [ARM’s documentation](<https://developer.arm.com/documentation/dui0801/e/A64-General-Instructions/ADRL-pseudo-instruction?lang=en>), you’ll discover that ADRL is a _pseudo-instruction_ which generates two machine instructions, `ADRP` and `ADD`. IDA combines them to provide more compact disassembly and improve cross-references.

In IDA’s terminology, a pseudo-instruction which replaces several simpler instructions is called a _macro_ instruction.

### Disabling macros

If you prefer to see the actual instructions, you can disable macros. This can be done in the Kernel Options 3 group of settings:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros3-3.png?width=847&height=716&name=macros3-3.png)

And now IDA no longer uses ADRL:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros4-3.png?width=894&height=139&name=macros4-3.png)

As can be seen in this example, it can produce misleading disassembly (ADRP can only use page-aligned addresses which is why it seems to refer to some unrelated string)

### Simplified instructions

In addition to macros, sometimes IDA may transform single instructions to improve readability or make their behavior more obvious. For example, on ARM some instructions have _preferred disassembly_ form and by default IDA uses it.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros5-3.png?width=1249&height=870&name=macros5-3.png)

Instruction simplification feature is usually controlled by a processor-specific option.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros6-3.png?width=909&height=467&name=macros6-3.png)

Other disassembly improvements

Some processor modules may have other options which may change disassembly to improve readability even if it sometimes means the resulting listing is not strictly conforming. For example, MIPS has an option to simplify instructions which use the global register `$gp` which usually has a fixed value and using it makes disassembly much easier to read:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros7-3.png?width=889&height=648&name=macros7-3.png)

If you are curious about what the options in the dialog do, clicking “Help” shows a short explanation:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/macros8-3.png?width=645&height=583&name=macros8-3.png)

See also:

[Igor’s Tip of the Week #137: Processor modes and segment registers](<https://hex-rays.com/blog/igors-tip-of-the-week-137-processor-modes-and-segment-registers/>)

[Igor’s tip of the week #98: Analysis options](<https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options/>)

---

## 2. 

**Date:** Posted: Jun 15, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-heimdallr](https://hex-rays.com/blog/plugin-focus-heimdallr)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: Heimdallr

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-heimdallr>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-heimdallr>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-heimdallr>)

A 

Alex Petrov ✦ Posted: Jun 15, 2023

![Plugin focus: Heimdallr](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

**This is a guest entry written by Robert from Interrupt Labs. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author.**

## Heimdallr: Deep links into IDA Databases

When reverse engineering in IDA, I find it useful to take notes on my findings to help re-enforce my understanding of a program and make the information more searchable to me and my teammates. However, due to the constant update cycles of modern software, this knowledge base gradually drifts out of date with reality. This makes it difficult to find your way back to the source of this knowledge, as especially on large platforms such as iOS, it can be impossible to find precisely which IDB you grabbed a code snippet from.

Heimdallr is an open-source IDA plugin that enables the creation of deep links into an IDA database. This allows a reverse engineer to copy out a segment of code that includes a link back to exactly where it came from. This means that if you need to check your work later or just want to shoot it over to a teammate to get their thoughts, they can jump to exactly that location with one click.

Heimdallr is made up of two parts – the `heimdallr-ida` IDA plugin and the `heimdallr-client` Python script. The client identifies which IDA database the URL is referring to and issues a request to the `heimdallr-ida` plugin to jump to the requested location and view. The plugin adds the hotkeys that allow the links to be easily generated within IDA and runs a server which allows the client to change the current position and view within a database.

Heimdallr can be obtained here: <https://github.com/interruptlabs/heimdallr-ida>

## Using Heimdallr

### Copying Links

Heimdallr registers two hotkeys for grabbing information out of IDA. This information is put directly into your clipboard so you can save it anywhere.

  * `ctrl`+`alt`+`n` puts a link to the current IDA cursor into the clipboard  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/slack-example-3.png?width=1428&height=188&name=slack-example-3.png) This is useful for giving a teammate the location of what you’re looking at so they can jump over and give you feedback. 

  * `ctrl`+`shift`+`n` puts the currently selected snippet disassembly or decompilation, with a link below it in the markdown format into the clipboard
        
        ```
        if ( *(_DWORD *)a1 !=13 )
        
        {
          v13 = (const char *)sub_1000A4254();
          sub_10021AFE8("E475: Invalid argument: %s", v13);
          return 0LL;
        }  
        
        ```
        [vim.i64:0x1001656a8]
        

This is useful for taking notes where you want to reference what your analysis was based on but keep all the relevant information in line with your notes.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/obsidian-example-3.png?width=1482&height=598&name=obsidian-example-3.png)



### Getting you back

When you click a link, the system will run `heimdallr-client`, which takes the URL and attempts to find an IDA instance that has the database open. If no current instance matches, `heimdallr-client` will look through IDA's recently opened files to identify the required file. If that fails, the user can configure a common directory for `heimdallr-client` to search for IDBs. 

This strategy was chosen over absolute file paths as it allows many users to share links between themselves, without the need to have a common file directory structure, and without compromising how quickly a link can be opened. The link format was also carefully designed to be human-readable – so that even if the reader doesn't have the plugin installed, they can easily use the information to get back to the same spot.

Once an IDA instance is found that contains the relevant database, `heimdallr-client` will connect to that specific IDA instance using a gRPC server spun up by the plugin for each database that is open. This allows the client to issue a request to the `heimdallr-ida` plugin to bring the window to the foreground and jump to the requested location and view within the database. This will bring anyone back to the exact area you were looking at, with the cursor at the top of the exported area, with one click.

### Using Heimdallr in Scripts

You can also use Heimdallr in your scripts to output a markdown-based report of your results. For example:
    
    
    import heimdallr_utils.plugin as heimdallr
    import idautils, idc
    
    
    status_str = 0x0000000100236D7A
    results = []
    
    for item in idautils.DataRefsTo(status_str):
        results.append((idc.get_func_name(item), item))
    
    with open("output.md", "w") as fd:
        for func_name, ea in results:
            fd.write(f""" # {func_name} [Goto Result]({heimdallr.create_link(ea)}) """)
    

Outputs the file:
    
    
    # sub_100166808
    
    [Goto Result](ida://vim.i64?offset=0x10016688c&hash=5e8723f4e8d8f8bb30ca3b0da9d46317&view=disasm)
    
    # sub_1001A4184
    
    [Goto Result](ida://vim.i64?offset=0x1001a41f4&hash=5e8723f4e8d8f8bb30ca3b0da9d46317&view=disasm)
    

This simple technique makes it a lot easier to navigate through what your script has found while providing a convenient place to annotate your thoughts on each result.

## Under the Hood

### Resolving the URL

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/system-overview-3.png?width=757&height=546&name=system-overview-3.png) Heimdallr works by registering a system-wide URL handler which is invoked whenever an `ida://` URL is clicked. On MacOS, we can do this through an Apple Script, and (unfortunately), on other platforms we use a minimal electron application to grab the URL events. The `heimdallr-client` then pulls out the filename, hash, offset, and view from the URL, and uses it to figure out which IDA instances the request refers to, and to issue a request to that instance to jump to the offset and view.

The IDA database is found using three stages:

  1. Check which IDBs are already open – it's likely, someone will already have the database open
  2. Check IDAs recently opened files – it's likely someone will have at least opened the database recently
  3. Search a user-configurable list of paths for a matching file name – a last resort that allows people to maintain independent project structures



The IDA database is identified firstly by the name, and secondly by the MD5 hash of the input file obtained from `ida_nalt.retrieve_input_file_md()` to avoid name collisions, where the database name is the same but the input file is different. This allows colleagues to collaborate across different databases, so long as the input file was the same, without a link being misdirected to the wrong place.

## Controlling IDA over gRPC

Once we find the IDA instance with the relevant IDB (or we open one), the final step is to tell IDA what view we want to jump in, and where in it to jump to. 

I played around with manually implementing a REST server using sockets, or using a platform-specific IPC mechanism like XPC, but ultimately settled on gRPC for a few reasons:

  1. It provides a bunch of resiliency for free
  2. It's cross-platform meaning I don't need to handle those edge cases
  3. It's cross-language meaning if I wanted to extend the concept to other programs – there's a clear path for expansion in the future



The gRPC element allows `heimdallr-client` to multiplex between many open IDA instances, allowing you to switch between multiple binaries within your notes effortlessly. The server exposes a minimal subset of the IDA API to the `heimdallr-client` over a locally bound random port. This is started in a background thread when an IDA instance opens, and the connection information is stored in a directory within the IDA User directory and deleted when the instance closes. 
    
    
    % cat ~/.config/heimdallr/rpc_endpoints/62083 
    
    {"pid": 62083, "address": "127.0.0.1:51013", "file_name": "vim.i64", "file_hash": "5e8723f4e8d8f8bb30ca3b0da9d46317"}
    

This allows the `heimdallr-client` to statelessly identify the currently open IDA instances, and obtain the gRPC port for the instances the URL was referring to. The client will then form a gRPC request to jump to the desired offset and view.

IDA offers several APIs to automate the current position within the database. For example, the cursor position is easily controlled with this API.
    
    
    ida_kernwin.jumpto(addr)
    

One of my goals with this project was to enable the links to refer to specifically the disassembly or decompiler view. This was a lot more difficult to handle in a fully cross-platform manner. IDA does have APIs for opening and switching to different views and tabs:
    
    
    target_view = ida_kernwin.find_widget(name)
    ida_kernwin.activate_widget(target_view, True)
    

However, this API only works if IDA is focused in the foreground, and the experience is much better if clicking the link brings IDA to the foreground. This required abusing complicated PyQt internals to work reliably:
    
    
    # https://www.riverbankcomputing.com/static/Docs/PyQt5/
    qtwidget = ida_kernwin.PluginForm.TWidgetToPyQtWidget(ida_kernwin.get_current_viewer())
    window = qtwidget.window()
    
    # UnMinimize
    WindowMinimized =  0x00000001 # https://www.riverbankcomputing.com/static/Docs/PyQt5/api/qtcore/qt.html#WindowState
    cur_state = window.windowState()
    new_state = cur_state & (~WindowMinimized)
    window.setWindowState(new_state)
    
    # Switch desktop / give keyboard control
    window.show() 
    window.raise_() # Bring to front (MacOS)
    window.activateWindow() # Bring to front (Windows)
    

# Conclusion

While intricate, this solution gives me exactly what I wanted – the ability to get back to what I was looking at 6 months ago with one click. There are plenty of other ways to annotate this information in notes, but I found that doing so added a lot of friction to both creating my notes and using them. The "one-click" nature of this solution makes it much easier for me to record this information and ensures that my notes are specific around what they are relevant to.

Going forward, I'd love to add support for IDA Teams to remove the need to have an existing database and improve platform support. If you've got any idea on how to implement these, open a PR/Issue on our [GitHub](<https://github.com/interruptlabs/heimdallr-ida>), or reach out on [Twitter](<https://twitter.com/interruptlabs>).

Get the Heimdallr plugin: <https://github.com/interruptlabs/heimdallr-ida>

---

## 3. 

**Date:** Posted: Jun 12, 2023  
**Link:** [https://hex-rays.com/blog/ida-8-3-qt-5-15-2-sources-build-scripts](https://hex-rays.com/blog/ida-8-3-qt-5-15-2-sources-build-scripts)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.3: Qt 5.15.2 sources & build scripts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-3-qt-5-15-2-sources-build-scripts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-3-qt-5-15-2-sources-build-scripts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-3-qt-5-15-2-sources-build-scripts>)

Arnaud Diederen ✦ Posted: Jun 12, 2023

![IDA 8.3: Qt 5.15.2 sources & build scripts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

A handful of our users have already requested information regarding the Qt 5.15.2 build, that is shipped with IDA 8.3.

The Qt sources used by IDA are:

  * based on Qt 5.15.2,
  * to which [the KDE Qt5 patch collection](<https://community.kde.org/Qt5PatchCollection>) has been added,
  * plus a few custom patches/fixes



### Rebuilding Qt from source

In order to obtain compatible libs, the simplest way forward is to

  * [download the full archive](<https://hex-rays.com/products/ida/support/freefiles/qt-5.15.2-full-IDA83.tar.bz2>) – it contains the original 5.15.2 source + KDE patches + Hex-Rays patches
  * go to the `build/` directory
  * type `python3 build.py -h`



Note: on Windows, that command should be issued from a Visual Studio 2019 command prompt.

---

## 4. 

**Date:** Posted: Jun 9, 2023  
**Link:** [https://hex-rays.com/blog/ida-8-3-released](https://hex-rays.com/blog/ida-8-3-released)

[Back](<https://hex-rays.com/blog>)

[IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.3 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-3-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-3-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-3-released>)

A 

Alex Petrov ✦ Posted: Jun 9, 2023

![IDA 8.3 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We are pleased to announce the release of IDA version 8.3!

In this release, there are many new features and enhancements, including:

  * IDA64 support for (32-bit) .idb files
  * UX improvements
  * IDA Teams enhancements 
  * DWARF speedup
  * ARM64 system registers
  * IDA Educational now includes x86/x64 decompiler, and file size limit has been lifted.
  * IDA Home features IDA Python API improvements
  * Golang: added support for Go 1.20 and additional improvements
  * and more…



See full updates here: <https://hex-rays.com/products/ida/news/8_3/>

### How to request the new versions

All new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively, you can [submit your ida.key](<https://www.hex-rays.com/updida/>), and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially shortly after the release. Please be patient.

If you have not received anything within an hour or so, send us a message to [support@hex-rays.com](<mailto:support@hex-rays.com>).

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

##

---

## 5. 

**Date:** Posted: Jun 2, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-143-fixing-wrong-address-references-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-143-fixing-wrong-address-references-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #143: Fixing wrong address references in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-143-fixing-wrong-address-references-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-143-fixing-wrong-address-references-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-143-fixing-wrong-address-references-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Jun 2, 2023

![Igor’s Tip of the Week #143: Fixing wrong address references in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When decompiling code without high-level metadata (especially firmware), you may observe strange-looking address expressions which do not seem to make sense.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/wrongaddr1-3.png?width=1331&height=198&name=wrongaddr1-3.png)

What are these and how to fix/improve the pseudocode?

Because on the CPU level there is no difference between an [address and a simple number](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>), distinguishing addresses and plain numbers is a difficult task which is not solvable in general case without actually executing the code. IDA uses some heuristics to try and detect when a number looks like an address and convert such numbers to [offsets](<https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets/>), but such heuristics are not always reliable and may lead to false positives. This can be especially bad when the database has valid addresses around `0`, because then many small numbers look like addresses. The decompiler relies on IDA’s analysis and uses the information provided by it to produce the pseudocode which is supposed to faithfully represent behavior of the machine code. However, this can backfire in case the analysis made a mistake. Thankfully, IDA is _interactive_ and allows you to fix almost anything.

In situation like above, usually the simplest algorithm is as follows:

  1. position cursor on the wrong address expression
  2. press `Tab` to switch to disassembly. You should land on or close to the wrong offset expression. Note that it does not always match what you see in the pseudocode.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/wrongaddr2-2.png?width=494&height=117&name=wrongaddr2-2.png)
  3. convert it to a plain number, e.g. by pressing `Q` (hex), `H` (decimal) or `#` (default).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/wrongaddr3-3.png?width=542&height=618&name=wrongaddr3-3.png)
  4. press `Tab` to switch back to pseudocode and `F5` to refresh it. The wrong expression should be converted to plain number or another context-dependent expression.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/wrongaddr4-3.png?width=751&height=188&name=wrongaddr4-3.png)

---

## 6. 

**Date:** Posted: May 26, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-142-mapping-local-types](https://hex-rays.com/blog/igors-tip-of-the-week-142-mapping-local-types)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #142: Mapping local types

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-142-mapping-local-types>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-142-mapping-local-types>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-142-mapping-local-types>)

I 

Igor Skochinsky ✦ Posted: May 26, 2023

![Igor’s Tip of the Week #142: Mapping local types](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

When working on a binary, you often recover types used in it from many sources:

  * creating structures manually, [from data](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>), or [using decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-118-structure-creation-in-the-decompiler/>);
  * [parsing header files](<https://hex-rays.com/blog/igors-tip-of-the-week-141-parsing-c-files/>);
  * importing them from [type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>) or [debug information](<https://hex-rays.com/blog/igors-tip-of-the-week-140-loading-pdb-types/>);



However, it may happen that eventually you discover duplicates. For example, you find out that the “custom” structure you’ve been building up is actually a well-known type and you found the correct definition in debug info or header files. Or, after analyzing two different functions, you only find out later that two structures are, in fact, one and the same. Of course, you can go and replace all references to the “wrong” one manually, which is doable if you discover this early, but if you already have hundreds of functions or other types referring to it, the process can become tedious.

### Type mapping

To map a type to another, open the Local Types window (`Shift`–`F1`), and choose “Map to another type…” from the context menu on the type you want to map.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/maptype1-3.png?width=654&height=600&name=maptype1-3.png)

After choosing the type to replace it, the original type is deleted and all references to it are redirected to the new one. This is indicated by the arrow sign pointing to the new type’s definition.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/maptype2-3.png?width=1055&height=288&name=maptype2-3.png)

All uses of the old type in the function prototypes, local variable types etc. are replaced by the new type automatically.

See also:

[IDA Help: Local types window](<https://www.hex-rays.com/products/ida/support/idadoc/1259.shtml>)

---

## 7. 

**Date:** Posted: May 22, 2023  
**Link:** [https://hex-rays.com/blog/free-madame-de-maintenon-ctf-challenge](https://hex-rays.com/blog/free-madame-de-maintenon-ctf-challenge)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [CTF](<https://hex-rays.com/blog/tag/ctf>)

# Free Madame De Maintenon – CTF Challenge

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/free-madame-de-maintenon-ctf-challenge>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/free-madame-de-maintenon-ctf-challenge>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/free-madame-de-maintenon-ctf-challenge>)

A 

Alex Petrov ✦ Posted: May 22, 2023

![Free Madame De Maintenon – CTF Challenge](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/free-madame-de-maintenon-ctf-3.png?width=1024&height=768&name=free-madame-de-maintenon-ctf-3.png)

Madame de Maintenon (AKA “IDA lady”) is locked in a castle and needs help to escape. Do you think you could free her? Be careful, you might get lost or caught by vicious guardians. Traps are laid along the way, so keep your eyes open, your mind sharp, and capture the flag. 

Send us proof of your success (you will receive instructions upon solving the puzzle) **before 17:00 CEST, May 26th, 2023**. Over 90 brave reversers are already trying to solve it, but do not worry… speed is not essential! **We will randomly pick eleven winners** and award them.

Here is what awaits the winners:

  * **One IDA Pro license**
  * One Hex-Rays hoodie
  * Three T-shirts
  * One F5 cap
  * Five cup coasters



### How to participate?

If you already have IDA Pro, Free or Home, then you can download the challenge below. Don’t have IDA yet? Get IDA Free [here.](<https://hex-rays.com/ida-free/#download>)

[Download the challenge](<https://hex-rays.com/wp-content/uploads/2023/05/free-madame-de-maintenon-challenge.zip>)

### What you need to know

  * Submissions made after the deadlines will not be considered.
  * We will announce the winners on 29 May on our website and social networks.
  * The eleven winners will be contacted to provide us with their complete shipping details.
  * This challenge is completely free and does not require a purchase of a product or service; 
  * The winner of the IDA Pro license: 
    * Should accept the [HEX-RAYS SA General Terms and Conditions](<https://hex-rays.com/products/ida/t%26c.pdf>) and the [End-user license agreement;](<https://hex-rays.com/order/#EULA>)
    * May need to provide a copy of valid official identification document to obtain the license.
  * If we do not receive complete delivery information, we will consider the submission unsuccessful and select another winner;
  * The delivery is free of charge for the winner;
  * All personal data, including email addresses, names, and shipping details, will not be:
    * Given to third parties;
    * Used for further communication;
    * Stored on our servers longer than the duration of the challenge.
  * Should you have any complaints or questions, please get in touch with [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)



Good luck!

### Congratulations to the winners

#### The IDA Pro License goes to:

Harald Andreasen

#### The Hex-Rays hoodie goes to:

Aleksandra

####  The three T-shirts go to:

Matan Ziv  
Melih Kaan Yildiz  
Jorge Martins

#### The F5 Cap goes to:

Chris Issing

#### The five cup coasters go to:

Elias Bachaalany  
Hyunsoo Cha  
Mads Hansen  
Robert Yates  
Bétrisey Samuel Jérémy

---

## 8. 

**Date:** Posted: May 19, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-141-parsing-c-files](https://hex-rays.com/blog/igors-tip-of-the-week-141-parsing-c-files)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #141: Parsing C files

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-141-parsing-c-files>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-141-parsing-c-files>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-141-parsing-c-files>)

I 

Igor Skochinsky ✦ Posted: May 19, 2023

![Igor’s Tip of the Week #141: Parsing C files](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Previosuly, we’ve covered creating structures from C code [using the Local Types window](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>), however this may be not very convenient when you have complex types with many dependencies (especially of scattered over several fiels or depending on preprocessor defines). In such case it may be nore convenient to parse the original header file(s) on disk.

### Parsing header files

If you happen to have the types you need in a header file, you can try using IDA’s built-in C parser via the File > Load file > Parse C header file… (shortcut `Ctrl`+`F9`).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cheader1-3.png?width=609&height=250&name=cheader1-3.png)  
Just like a compiler, IDA will handle the preprocessor directives (`#include`,`#define`, `#ifdef` and so on), and add any types discovered to the Local Types list, from where they can be used in the decompiler (or the disassembly, after importing into the IDB).

### Setting compiler options

IDA’s built-in parser can mimic several popular compilers, including Visual C++, GCC (and compatibles), Borland C++ Builder, or Watcom. For many stuctured files the compiler is preset to a detected or guessed value, but you can also change or set it via Options > Compiler… dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cheader2-3.png?width=402&height=540&name=cheader2-3.png)

In this dialog you can also adjust settings necessary for the preprocessing step, such as the predefined preprocessor macros (`#define`s) or the include paths for the `#include` directives. They are pre-filled from the `CC_PARMS` setting in `ida.cfg`.

### Clang parser

The built-in parser is quite basic and handles mostly simple C syntax or very basic C++ (e.g templates are not supported). If you have complex files employing new, modern C or C++ features, you may have more luck using the Clang-based parser added in IDA 7.7. It can be selected in the _Source parser_ dropdown of the compiler options dialog and will be used next time you invoke the _Parse C header file_ command. For the details on using it, see the dedicated [IDAClang tutorial](<https://hex-rays.com/tutorials/idaclang/>).

See also: 

[IDA Help: Load C header](<https://www.hex-rays.com/products/ida/support/idadoc/1367.shtml>)

[IDA Help: Compiler](<https://www.hex-rays.com/products/ida/support/idadoc/1354.shtml>)

[Igor’s tip of the week #62: Creating custom type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-62-creating-custom-type-libraries/>)

[Introducing the IDAClang Tutorial](<https://hex-rays.com/blog/introducing-the-idaclang-tutorial/>)

---

## 9. 

**Date:** Posted: May 15, 2023  
**Link:** [https://hex-rays.com/blog/hex-rays-gives-away-two-tickets-to-typhooncon-2023](https://hex-rays.com/blog/hex-rays-gives-away-two-tickets-to-typhooncon-2023)

[Back](<https://hex-rays.com/blog>)

[TyphoonCon](<https://hex-rays.com/blog/tag/typhooncon>) [Giveaway](<https://hex-rays.com/blog/tag/giveaway>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-Rays gives away two tickets to TyphoonCon 2023

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-gives-away-two-tickets-to-typhooncon-2023>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-gives-away-two-tickets-to-typhooncon-2023>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-gives-away-two-tickets-to-typhooncon-2023>)

A 

Alex Petrov ✦ Posted: May 15, 2023

![Hex-Rays gives away two tickets to TyphoonCon 2023](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/giveaway_typhooncon-4-3.png)

Are you ready for an immersive experience in the world of cybersecurity and reverse engineering? Hex-Rays, a leading provider of cutting-edge software analysis tools, is excited to announce an exclusive giveaway for 2 lucky IDA users. We are offering you the opportunity to win two free tickets to the highly anticipated TyphoonCon 2023 event!

[TyphoonCon 2023 ](<https://typhooncon.com/>)is a premier cybersecurity and reverse engineering conference where industry experts, researchers, and enthusiasts gather to explore the latest trends and advancements in the field. It focuses on highly technical offensive security issues such as vulnerability discovery, advanced exploitation techniques, reverse engineering, and many more.

### How to Participate

Entering the giveaway is simple! Just follow these steps:

  * Send an email to [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)
  * In the email, include the following details: 
    * Full name attached to the license
    * License ID (48-XXX-XXX-XX)



**Please note that the deadline for submissions is 22 May 2023.** Winners will be chosen through a random draw and contacted via email. **Remember, the tickets are non-transferable. Travel, accommodation, or any other expenses are not included.**

Don’t hesitate to reach out to our team at [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>) if you have any questions or need further information.

Good luck to all participants!

---

