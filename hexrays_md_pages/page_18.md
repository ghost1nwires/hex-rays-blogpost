# Hex-Rays Blog – Page 18

**Archived:** 2026-02-20 12:15
**URL:** https://hex-rays.com/blog/page/18

---

## 1. 

**Date:** Posted: Dec 19, 2022  
**Link:** [https://hex-rays.com/blog/the-hex-rays-plugin-repository](https://hex-rays.com/blog/the-hex-rays-plugin-repository)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [plugin repository](<https://hex-rays.com/blog/tag/plugin-repository>)

# The Hex-Rays plugin repository

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/the-hex-rays-plugin-repository>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/the-hex-rays-plugin-repository>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/the-hex-rays-plugin-repository>)

A 

Alex Petrov ✦ Posted: Dec 19, 2022

![The Hex-Rays plugin repository](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We are delighted to announce the Hex-Rays plugin repository! As you know, plugins have always played a substantial role in IDA due to their ability to enrich its functionality. Most of these extensions are created by the users and resolve all sorts of practical cases. 

Until now, for the lack of a centralized “index”, finding a plugin required effort (and often luck). IDA users have been asking for some time for one single place for all these add-ons. We’ve been listening to these suggestions, and our plugin repository is just the beginning…

Currently, the repository contains over a hundred extensions and is constantly updated with new plugins. There is always something new to try out and explore. 

Additionally, the repository is open to submissions from the community. If you have developed a plugin that you think would be useful to others, you can submit it for inclusion in the repository.

We intend to develop this repository further and offer more opportunities for the plugin writers and the end users. [Take a look or upload your plugin](<https://plugins.hex-rays.com>)

![plugin repository logo](https://hex-rays.com/hubfs/Imported_Blog_Media/logo-2-3.png)

[Visit the plugin repository](<https://plugins.hex-rays.com>)

---

## 2. 

**Date:** Posted: Dec 16, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-119-force-call-type](https://hex-rays.com/blog/igors-tip-of-the-week-119-force-call-type)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #119: Force call type

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-119-force-call-type>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-119-force-call-type>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-119-force-call-type>)

I 

Igor Skochinsky ✦ Posted: Dec 16, 2022

![Igor’s Tip of the Week #119: Force call type](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

When dealing with compile binary code, the decompiler lacks information present in the source code, such as function prototypes and so must guess it or rely on the information provided by the user (where its interactive features come handy).

One especially tricky situation is indirect calls: without exact information about the destination of the call, the decompiler can only try to analyze registers or stack slots initialized before the call and try to deduce the potential function prototype this way. For example, check this snippet from a UEFI module:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/forcecall1-3.png?width=968&height=495&name=forcecall1-3.png)

For several indirect calls involving `qword_21D40`, the decompiler had to guess the arguments and add casts.

If we analyze the module from the entry point, we can find the place where the variable is initialized and figure out that it is, in fact, the standard UEFI global variable `gBS` of the type `EFI_BOOT_SERVICES *`:
    
    
    EFI_STATUS __fastcall UefiBootServicesTableLibConstructor(EFI_HANDLE ImageHandle, EFI_SYSTEM_TABLE *SystemTable)
    {
      gImageHandle = ImageHandle;
      if ( DebugAssertEnabled() && !gImageHandle )
        DebugAssert(
          "u:\\MdePkg\\Library\\UefiBootServicesTableLib\\UefiBootServicesTableLib.c",
          0x33ui64,
          "gImageHandle != ((void *) 0)");
      gST = SystemTable;
      if ( DebugAssertEnabled() && !gST )
        DebugAssert(
          "u:\\MdePkg\\Library\\UefiBootServicesTableLib\\UefiBootServicesTableLib.c",
          0x39ui64,
          "gST != ((void *) 0)");
      // gBS was qword_21D40	  
      gBS = SystemTable->BootServices;
      if ( DebugAssertEnabled() && !gBS )
        DebugAssert(
          "u:\\MdePkg\\Library\\UefiBootServicesTableLib\\UefiBootServicesTableLib.c",
          0x3Fui64,
          "gBS != ((void *) 0)");
      return 0i64;
    }

After renaming and changing the type of the global variable, the original function is slightly improved thanks to the type information from the standard UEFI type library:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/forcecall2-3.png?width=1228&height=495&name=forcecall2-3.png)

Even though the decompiler now has prototypes of function pointers such as `LocateDevicePath` (shown in the pop-up) or `FreePool`, it has to add casts because the arguments which are passed to the calls do not match the prototype. To tell the decompiler to rely on the type information instead of guessing the arguments, use the command _Force call type_ from the context menu:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/forcecall3-3.png?width=323&height=323&name=forcecall3-3.png)

When running the command on the indirect calls, the decompiler also uses the type information to update the types of the arguments (except those already set by the user), making the pseudocode much cleaner:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/forcecall4-3.png?width=979&height=498&name=forcecall4-3.png)

See also: [Hex-Rays interactive operation: Force call type](<https://www.hex-rays.com/products/decompiler/manual/cmd_force_call_type.shtml>)

---

## 3. 

**Date:** Posted: Dec 15, 2022  
**Link:** [https://hex-rays.com/blog/ida-8-2-released](https://hex-rays.com/blog/ida-8-2-released)

[Back](<https://hex-rays.com/blog>)

[IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.2 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-2-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-2-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-2-released>)

A 

Alex Petrov ✦ Posted: Dec 15, 2022

![IDA 8.2 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We are excited to announce the release of IDA version 8.2!

In this release, there are many new features and enhancements for IDA Pro, IDA Teams, and IDA Home, including:

  * 32-bit support in IDA64
  * Processor modules improvements
  * Swift
  * picture_search plugin
  * UI candy
  * and more…



See full updates here: <https://hex-rays.com/products/ida/news/8_2/>

## How to request the new versions

As usual, the new versions of IDA Pro and IDA Home are free for users with an active support plan. Please use the “Help, Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can submit your ida.key to <https://www.hex-rays.com/updida> and our servers will prepare new download links for all your licenses. Please forgive us if your request takes some time to be processed, especially today, after the release. Thanks for your patience.

In case of problems, do not hesitate to send us a message. Just wait an hour or so, and if you do not hear from us, send a message to [support@hex-rays.com](<mailto:support@hex-rays.com>)

### Has your support plan expired?

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

### Upgrading to IDA Teams

We offer the following upgrade options to corporate users. Your options depend on your licenses:

  * if you have IDA Pro floating licenses: you can convert those, free of charge, to IDA Teams licenses;
  * if you have IDA Pro non-floating licenses: it’s possible to upgrade those to IDA Teams as well, but you need to contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) for a quote. Specify the number of seats you require and what bundle you are interested in (see <https://www.hex-rays.com/ida-teams/> );
  * if you do not have licenses yet, or wish to acquire new licenses: please contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) for a quote.



Note that IDA Teams is not an add-on for the existing IDA Pro licenses: it is a new product. IDA Teams uses a subscription-based licensing model. If you have IDA Teams, you can take advantage of a Private Lumina Server at no additional costs. Learn more about the Private Lumina Server: <https://hex-rays.com/products/ida/lumina/#private-server>.

---

## 4. 

**Date:** Posted: Dec 9, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-118-structure-creation-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-118-structure-creation-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #118: Structure creation in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-118-structure-creation-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-118-structure-creation-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-118-structure-creation-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Dec 9, 2022

![Igor’s Tip of the Week #118: Structure creation in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

We’ve covered structure creation using [disassembly or Local Types](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>), but there is also a way of doing it from the decompiler, especially when dealing with unknown, custom types used by the program.

Whenever you see code dereferencing a variable with different offsets, it is likely a structure pointer and the function is accessing different fields of it.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decstruct1-3.png?width=448&height=254&name=decstruct1-3.png)

You can, of course, create the structure manually and change the variable’s type, but it is also possible to ask the decompiler to come up with a suitable layout. For this, use “Create new struct type…” from the context menu on the variable:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decstruct2-3.png?width=268&height=308&name=decstruct2-3.png)

If you don’t see the action, you may need to [reset the pointer type](<https://hex-rays.com/blog/igors-tip-of-the-week-117-reset-pointer-type/>) first. After you invoke it, the decompiler will analyze accesses to the variables and come up with a candidate structure type which matches them:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decstruct3-3.png?width=462&height=352&name=decstruct3-3.png)

You can accept the suggestion as-is, or make any suitable adjustments (for example, change the structure name, or edit some of the fields). After confirming, the structure is added to Local Types and the variable is converted to the corresponding pointer type:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decstruct4-3.png?width=460&height=257&name=decstruct4-3.png)

You can, of course, keep refining the structure as you continue with your analysis and discover how the fields are used in other functions and what they mean. Renaming fields can be done directly from the pseudocode view, while for adding or rearranging them you’ll likely need to use Local Types or Structures window.

See also:[ Hex-Rays interactive operation: Create new struct type](<https://www.hex-rays.com/products/decompiler/manual/cmd_new_struct.shtml>)

---

## 5. 

**Date:** Posted: Dec 2, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-117-reset-pointer-type](https://hex-rays.com/blog/igors-tip-of-the-week-117-reset-pointer-type)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #117: Reset pointer type

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-117-reset-pointer-type>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-117-reset-pointer-type>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-117-reset-pointer-type>)

I 

Igor Skochinsky ✦ Posted: Dec 2, 2022

![Igor’s Tip of the Week #117: Reset pointer type](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

While currently (as of version 8.1) the Hex-Rays decompiler does not try to perform full type recovery, it does try to deduce some types based on operations done on the variables, or using the type information for the API calls from [type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>).

One simple type deduction performed by the decompiler is creation of typed pointers when a variable is being dereferenced, for example:
    
    
    _QWORD *__fastcall sub_140006C94(_QWORD *a1)
    {
      a1[2] = 0i64;
      a1[1] = "bad array new length";
      *a1 = &std::bad_array_new_length::`vftable';
      return a1;
    }

Unfortunately, such conversions are not always correct, as can be seen in the example: we have a mix of integer and pointer elements in one array, so it’s more likely a structure. Also, due to C’s array indexing rules, the array indexes are multiplied by the element size (so, for example, `a1[2]` actually corresponds to the byte offset 16). If you prefer seeing “raw” offsets, you can change the variable’s type to a plain integer. This can, of course, be done by manually changing the variable’s type but there is a convenience command in the context menu which can be used to do it quickly:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/resetptr-3.png?width=405&height=385&name=resetptr-3.png)

After resetting, the variable becomes a simple integer type and all dereferences now use explicit byte offsets and casts:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/resetptr2-3.png?width=524&height=137&name=resetptr2-3.png)

Now you can, for example, create a structure corresponding to these accesses, or choose an existing one.

See also: 

[Hex-Rays Decompiler: Interactive operation](<https://www.hex-rays.com/products/decompiler/manual/interactive.shtml>)

---

## 6. 

**Date:** Posted: Nov 25, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-116-ida-startup-files](https://hex-rays.com/blog/igors-tip-of-the-week-116-ida-startup-files)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #116: IDA startup files

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-116-ida-startup-files>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-116-ida-startup-files>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-116-ida-startup-files>)

I 

Igor Skochinsky ✦ Posted: Nov 25, 2022

![Igor’s Tip of the Week #116: IDA startup files](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

IDA’s behavior and defaults can be configured using the [Options](<https://hex-rays.com/blog/igors-tip-of-the-week-25-disassembly-options/>) dialog, saved [desktop layouts](<https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts/>), or [config files](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>). However, sometimes the behavior you need depends on something in the input file and can’t be covered by a single option, or you may want IDA to do something additional after the file is loaded. Of course, there is always the possibility of making a plugin or a loader using IDA SDK or IDAPython, but it could be an overkill for simple situations. Instead, you can make use of several startup files used by IDA every time it loads a new file or even a previously saved database, and do the necessary work there.

The following files can be used for such purpose:

### ida.idc

This file in `idc` subdirectory if IDA’s install is automatically loaded on each run of IDA and can be used to perform any actions you may need. The default implementation defines a utility class for managing breakpoints and a small helper function, but you can add there any other code you need. As an example, it has a commented call to change a global setting:
    
    
    // uncomment this line to remove full paths in the debugger process options:
    // set_inf_attr(INF_LFLAGS, LFLG_DBG_NOPATH|get_inf_attr(INF_LFLAGS));

Instead of editing the file itself (which may have been installed in a read-only location), you can create a file `idauser.idc` with a function `user_main()` and put it in the [user directory](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>). If found, IDA will parse it and the main function of `ida.idc` will try to call `user_main()`. This feature allows you to keep the custom behaviour across multiple IDA installs and versions, without having to edit `ida.idc` every time.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/startup1-3.png?width=807&height=411&name=startup1-3.png)

### onload.idc

This file is similar to `ida.idc`, but is only executed for _newly loaded files_. In it you can, for example, do some additional parsing and formatting to augment the behavior of the default file loader(s). The default implementation detects when a DOS driver (EXE or COM file with `.sys` or `.drv` extension) is loaded and tries to format its header.

Similarly to `ida.idc`, instead of editing the file itself, you can create a file named `userload.idc` in the user directory and define a function `userload`.
    
    
    //      If you want to add your own processing of newly created databases,
    //      you may create a file named "userload.idc":
    //
    //      #define USERLOAD_IDC
    //      static userload(input_file,real_file,filetype) {
    //              ... your processing here ...
    //      }
    //
    
    #softinclude <userload.idc>
    
    // Input parameteres:
    //      input_file - name of loaded file
    //      real_file  - name of actual file that contains the input file.
    //                   usually this parameter is equal to input_file,
    //                   but is different if the input file is extracted from
    //                   an archive.
    //      filetype   - type of loaded file. See FT_.. definitions in idc.idc
    

### idapythonrc.py

Unlike the previous examples, this a Python file, so it is only loaded if you have IDAPython installed and working. If the file is found in the [user directory](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>), it will be loaded and executed on startup of IDAPython, so you can put there any code to perform fine-tuning of IDA, add utility functions to be called from the [CLI](<https://hex-rays.com/blog/igors-tip-of-the-week-73-output-window-and-logging/>), or run any additional scripts.

### Useful functions

Some functions which can be called from the startup files to configure IDA:

[`get_inf_attr()`/ `set_inf_attr()`/`set_flag()`](<https://www.hex-rays.com/products/ida/support/idadoc/285.shtml>): read and set various flags controlling IDA’s behavior. For example, `INF_AF` can be used to change various [analysis options](<https://hex-rays.com/blog/igors-tip-of-the-week-98-analysis-options/>).

[`process_config_directive()`](<https://www.hex-rays.com/products/ida/support/idadoc/642.shtml>): change a setting using keyword=value syntax. Most settings from `ida.cfg` can be used, as well as some processor-specific or debugger-specific ones. A few examples:

  * `process_config_directive("ABANDON_DATABASE=YES");`: do not save the database on exit. Please note that this setting has a side effect in that it disables most user actions which change the database, for example `MakeUnknown` (`U`) or `MakeCode` (`C`).
  * `process_config_directive("PACK_DATABASE=2");`: set the default database packing option to “deflate”;
  * `process_config_directive("GRAPH_OPCODE_BYTES=4");`: enable display of opcode bytes in graph mode;
  * for more examples, see `ida.cfg` (open it in any text editor).

---

## 7. 

**Date:** Posted: Nov 18, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-115-set-callee-address](https://hex-rays.com/blog/igors-tip-of-the-week-115-set-callee-address)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #115: Set callee address

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-115-set-callee-address>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-115-set-callee-address>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-115-set-callee-address>)

I 

Igor Skochinsky ✦ Posted: Nov 18, 2022

![Igor’s Tip of the Week #115: Set callee address](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

[Cross-references](<https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references/>) is one of the most useful features of IDA. For example, they allow you to see where a particular function is being called or referenced from, helping you to see how the function is used and understand its behavior better or discover potential bugs or vulnerabilities. For direct calls, IDA adds cross-references automatically, but in modern programs there are also many indirect calls which can’t always be resolved at disassembly time. In such cases, it is useful to have an option to set the target call address manually.

### Indirect call types

Most instruction sets have some kind of an indirect call instruction. The most common one uses a processor register which holds the address of the function to be called:

x86/x64 and ARM can use almost any general-purpose register:
    
    
    call edi (x86)
    call rax (x64)
    BLX R12 (ARM32)
    BLX R3
    BLR X8 (ARM64)

PowerPC is more limited and has to use dedicated `ctr` or `lr` registers:
    
    
    mtlr r12
    blrl
    
    mr r12, r9
    mtctr r9
    bctrl
    

in MIPS, in theory any register can be used, but binaries conforming to the standard PIC ABI tend to use the register t9:
    
    
    la      $t9, __cxa_finalize
    lw      $a0, (_fdata - 0x111E0)($v0)  # void *
    jalr    $t9 ; __cxa_finalize
    

In addition to simple register, some processors support more complex expressions. For example, on x86/x64 it is possible to use a register with offset, allowing to read a pointer value and jump to it in a single instruction:
    
    
    call    dword ptr [eax+0Ch] (x86)
    call    qword ptr [rax+98h] (x64)
    

### Setting callee address

In some simple situations (e.g. the register is initialized shortly before the call), IDA is able to resolve it automatically and adds a comment with the target address, like in the MIPS example above, or this one:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/callee1-3.png?width=599&height=623&name=callee1-3.png)

In more complicated situations, especially involving multiple memory dereferences or runtime calculations, it is possible to specify the target address manually. For this, use the standard plugin command available in Edit > Plugins > Change the callee address. The default shortcut is `Alt`–`F11`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/callee2-3.png?width=612&height=747&name=callee2-3.png)

The plugin will ask you to enter the target address (you can also use a function name):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/callee3-3.png?width=323&height=99&name=callee3-3.png)

The call instruction will gain a comment with the target address, as well as a cross-reference:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/callee4-3.png?width=1038&height=666&name=callee4-3.png)

Currently the plugin is implemented for x86/x64, ARM and MIPS. If you need to set or access this information programmatically, you can check how it works by consulting the source code in the SDK, under `plugins/callee`.

---

## 8. 

**Date:** Posted: Nov 11, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-114-split-offsets](https://hex-rays.com/blog/igors-tip-of-the-week-114-split-offsets)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #114: Split offsets

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-114-split-offsets>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-114-split-offsets>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-114-split-offsets>)

I 

Igor Skochinsky ✦ Posted: Nov 11, 2022

![Igor’s Tip of the Week #114: Split offsets](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Previously, we have [covered](<https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets/>) [offset](<https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base/>) [expressions](<https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets/>) which fit into a single instruction operand or data value. But this is not always the case, so let’s see how IDA can handle offsets which may be built out of multiple parts.

### 8-bit processors

Although slowly dying out, the 8-bit processors — especially the venerable 8051 — can still appear in current hardware, and of course we’ll be dealing with legacy systems for many years to come. Even though their registers can store only 8 bits af data, most of them can address 16-bit (64KiB) or more of memory which means that the addresses may need to be built by parts.

For example, consider this sequence of instructions from an 8051 firmware:
    
    
    code:CF22    mov     R3, #0xFF
    code:CF24    mov     R2, #0xF6
    code:CF26    mov     R1, #0xA6
    code:CF28    sjmp    code_CF36
    

The code for 8051 is often compiled using Keil C51 compiler, and this pattern is a typical way of initializing a [generic pointer to code memory](<https://www.keil.com/support/man/docs/c51/c51_le_genptrs.htm>). The address being referenced is `0xF6A6`, but can we make the instructions look “nice” and create cross references to it?

One possibility is to use [offset with custom base](<https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base/>) on the last move and specify the base of `0xF600`:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitoff1-3.png?width=331&height=575&name=splitoff1-3.png)

This does calculate the final address and create a cross-reference but the code is not quite “nice looking” and the other instruction remains a plain number:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitoff2-3.png?width=806&height=96&name=splitoff2-3.png)

In fact, a better option is to use the high8/low8 offsets for the two instructions. Because each instruction provides only a part of the full offset, it alone cannot be used by IDA for calculating the full address which needs to be provided by the user.

R2 provides the top 8 bits of the address, so we should use the `HIGH8` offset type for it. We also need to fill in the full address (`0xF6A6`) in the _Target address_ field. Base address should be reset to 0.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitoff3-3.png?width=331&height=575&name=splitoff3-3.png)

For R1, `LOW8` and the same target can be used:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitoff4-3.png?width=331&height=575&name=splitoff4-3.png)

After applying both offsets, IDA displays them using matching assembler operators:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitoff5-3.png?width=469&height=60&name=splitoff5-3.png)

### RISC processors

RISC processors often use fixed-width instructions and may not be able to reach the full range of the address space with the limited space for the immediate operand in the instruction. This include SPARC, MIPS, PowerPC and some others. As an example, let’s look at this PowerPC VLE snippet:
    
    
    seg001:0000C156         e_lis     r3, 1 # Load Immediate Shifted
    seg001:0000C15A         e_add16i  r3, r3, -0x1650 # 0xE9B0
    seg001:0000C15E         se_mtlr   r3
    seg001:0000C160         se_blrl
    

The code calculates an address of a function in `r3` and then calls it. IDA helpfully shows the final address in a comment, but we can also use custom offsets to represent them nicely. For the `e_add16i` instruction, we can use the `LOW16` type, as expected, but in case of `e_lis`, the processor-specific type `HIGHA16` should be used instead of `HIGH16`. This is because the low 16 bits are used here not as-is but as a sign-extened addend, with the high 16 bits of the final address becoming 0 after the addition (0x10000-0x1650=0xE9B0).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitoff6-3.png?width=339&height=598&name=splitoff6-3.png)

After converting both parts, IDA uses special assembler operators to show the final address:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitoff7-3.png?width=591&height=59&name=splitoff7-3.png)

Now we can go to the target and create a function there.

Note: specifically for PowerPC, IDA will automatically convert such sequences to offset expression if the target address exists and has instructions or data. But the manual approach can still be useful for other processors or complex situations (for example, the two instructions are too far apart).

---

## 9. 

**Date:** Posted: Nov 4, 2022  
**Link:** [https://hex-rays.com/blog/plugin-focus-hrdevhelper](https://hex-rays.com/blog/plugin-focus-hrdevhelper)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: HRDevHelper

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-hrdevhelper>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-hrdevhelper>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-hrdevhelper>)

A 

Alex Petrov ✦ Posted: Nov 4, 2022

![Plugin focus: HRDevHelper](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

**This is a guest entry written by Dennis Elser from Trenchant Advanced Research Center (formerly Azimuth Security). His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author.**

## HRDevHelper

[HRDevHelper](<https://github.com/patois/HRDevHelper>) is a decompiler plugin that takes advantage of the Interactive Disassembler’s built-in graph-rendering engine in order to visualize the [Abstract Syntax Tree](<https://en.wikipedia.org/wiki/Abstract_syntax_tree>) (i.e., `ctree`) of pseudo-code functions generated by the Hex-Rays decompiler. The plugin also comes with a context viewer that displays structural information on the current decompiled function.

**HRDevHelper** has been developed for the purpose of:

  * visualizing the Hex-Rays _ctree_ of decompiled functions
  * demystifying and learning the structure of the Hex-Rays _ctree_ and its individual elements
  * assisting in identifying and finding code patterns
  * simplifying the process of developing further Hex-Rays decompiler plugins and scripts



Other than using the decompiler’s context menu, the **HRDevHelper** viewers can be opened using keyboard shortcuts. There exist _ctree_ viewers and a context viewer, whose modes of operation are explained in this guest blog.

![The HRDevHelper's viewers can be invoked from a pop-up menu or by using keyboard shortcuts](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scrn00-3.png?width=600&height=316&name=scrn00-3.png)

_The HRDevHelper’s viewers can be invoked from a pop-up menu or by using keyboard shortcut_

## Ctree Graph Viewers

In its default configuration, pressing the key combination `Ctrl`–`.` opens up and attaches a _ctree graph view_ to the current pseudocode window’s righthand side. The pseudocode view and the graph are now _contextually linked_ to each other and allow for interactive exploration of the _ctree_ using the mouse or the keyboard. Placing the cursor on any pseudocode element visually highlights and centers the graph on corresponding _ctree_ nodes. If, instead, a static view is desired, auto-centering the graph can be disabled by pressing the `C` key, with a graph’s widget focused.

![HRDevHelper establishes context between decompiled code and the graph by highlighting nodes](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scrn01-3.png?width=1059&height=429&name=scrn01-3.png)

_HRDevHelper establishes context between decompiled code and the graph by highlighting nodes_

Likewise, the `Ctrl`–`Shift`–`.` key combination creates an isolated graph from a pseudocode fragment for a more focused overview or later reference. It could be thought of as “cutting off” a branch of a _ctree_ , as can be seen in the image below.

![Parts of the graphs can be isolated from the entire ctree for better overview and later reference](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scrn02-3.png?width=1059&height=429&name=scrn02-3.png)

_Parts of the graphs can be isolated from the entire ctree for better overview and later reference_

## Context View

Finally, there is a context view in **HRDevHelper** that can be opened by pressing the `C` key from within a decompiler’s widget. It further displays the decompiler’s internals in a textual representation. The viewer’s` .op` field, for example, reflects the current [ctree item’s code](<https://hex-rays.com/products/decompiler/manual/sdk/hexrays_8hpp.shtml#a8fff5d4d0a6974af5b5aa3feeebab2a0>), which is a Hex-Rays representation of the underlying decompiled instruction or set of instructions. It can be either a statement code `cit_...`or an expression code `cot_...`.

One example of such a statement code is `cit_if`, which stands for a C-like` if` statement, whereas `cot_call` is an example of an expression code that indicates a _function call_. A comprehensive list of _ctree_ item codes can be found online as part of the [Hex-Rays decompiler SDK reference](<https://hex-rays.com/products/decompiler/manual/sdk/index.shtml>) or in the Hex-Rays decompiler SDK that comes with a local installation of the decompiler `hexrays.hpp`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scrn04-3.png?width=222&height=134&name=scrn04-3.png)

_The`.op` field shown by the context viewer refers to the item code pointed to by the screen cursor_

_Ctree_ statement and expression codes are one of the Hex-Rays decompiler’s _abstraction layers of machine code_ that can be accessed programmatically via C++ using the Hex-Rays SDK or via Python by using[ IDAPython](<https://github.com/idapython/src>) bindings.

The **HRDevHelper** context viewer takes this concept further by _linking individual ctree items_ using logical `AND` operations and by _creating Python expressions_ from it.

As shown in the screenshot below, the result is a programmatic description of a pseudo-C code pattern that is not only i _ndependent from a processor architecture_ but also decoupled from non-structural detail.

The generated expression shown below structurally describes the `memcpy` call that can be seen on the left-hand side of the screenshot. Its first item code is a call `cot_call` to an object ( `cot_obj`, which has a field that contains the target function’s address) and whose first and second function arguments are stack or register variables `cot_var`, and its third argument is a number `cot_num`.

![The HRDevHelper context viewer generates and displays programmatic descriptions of code patterns on-the-fly](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scrn03-3.png?width=774&height=372&name=scrn03-3.png)

_The HRDevHelper context viewer generates and displays programmatic descriptions of code patterns on-the-fly_

One of the practical outcomes of having these expressions at hand is when combining them with the Hex-Rays [tree visitor class](<https://hex-rays.com/blog/hex-rays-decompiler-primer/#visitor>), which opens up the door for _finding code patterns in binaries independent from their respective CPU architecture._

Example use cases comprise:

  * finding new security vulnerabilities
  * finding variants of known security vulnerabilities
  * finding malicious code patterns
  * proving code theft / license violation



## Putting It All Together

Assuming we were interested in _finding calls to the_` memcpy` _function_ but wanted to exclude those calls whose third argument is an explicit fixed number, the IDAPython script below would help us do just that. It is based on the expression shown in the screenshot above, and has been wrapped in a Hex-Rays tree visitor class.
    
    
    import ida_name, ida_hexrays, ida_funcs
    
    class memcpy_finder_t(ida_hexrays.ctree_visitor_t):
        def __init__(self):
            ida_hexrays.ctree_visitor_t.__init__(self, ida_hexrays.CV_FAST)
    
        def _process(self, i):
            # find function calls but with the following
            # restrictions in place:
            # - the called function's symbolic name must contain 'memcpy'
            # - the call's number of arguments must be three
            # - the call's 3rd argument must not be an explicit number
            found = (i.op is ida_hexrays.cot_call and
                i.x.op is ida_hexrays.cot_obj and
                "memcpy" in ida_name.get_name(i.x.obj_ea) and
                len(i.a) == 3 and
                i.a[2].op is not ida_hexrays.cot_num)
    
            # once found, print address of current item's address
            # (whose item code happens to be cot_call)
            if found:
                print("memcpy() found at %x" % i.ea)
            return 0
    
        # process expressions
        def visit_expr(self, e):
            return self._process(e)
            
        # for the sake of completeness, also process statements
        def visit_insn(self, i):
            return self._process(i)
    
    # process all of the IDA database's functions
    for i in range(ida_funcs.get_func_qty()):
        # get 'func_t' structure
        f = ida_funcs.getn_func(i)
        if f:
            # get cfunc_t structure
            cfunc = ida_hexrays.decompile(f)
            if cfunc:
                # run the visitor class
                mf = memcpy_finder_t()
                mf.apply_to(cfunc.body, None)
    

Naturally, more complex search expressions are possible. Reading both the [Hex-Rays Decompiler Primer](<https://hex-rays.com/blog/hex-rays-decompiler-primer>) by Elias Bachaalany and the Hex-Rays SDK will surely help in automating and improving the efficiency of search endeavors!

Alternatively, the above script may also be optimized for speed by resolving and decompiling only the `memcpy` function’s callers as opposed to decompiling each and every of the target binary’s functions. It comes with a minor disadvantage, however: the script would certainly miss those `memcpy` calls that may have been inlined by the linker (e.g., using the `rep movsb` instruction on x86) but which are successfully identified as `memcpy` by the decompiler.

## Plugin Installation

**HRDevHelper** is free and open-source software written in IDAPython. Being a plugin for the Hex-Rays decompiler, it requires valid IDA and Hex-Rays decompiler licenses, along with a Python 3 interpreter that comes with the IDA installer. Other than that, the plugin has no further dependencies and can freely be downloaded or cloned from its [official github repository](<https://github.com/patois/HRDevHelper.git>).

As both its code and installation instructions may be subject to change, it is advised to follow the installation instructions found in the **HRDevHelper** repository.

## Plugin Configuration

After running the plugin for the first time, a configuration file `HRDevHelper.cfg` is created in the [IDA user directory’s](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>)` cfg` sub-folder. The file can be opened and edited with a text editor in order to customize the plugin’s default settings: keyboard shortcuts, color palettes, and the _ctree_ graph’s default docking behavior, to name a few examples.

_The author would like to thank Hex-Rays / the IDAPython project for having provided the[vds5](<https://github.com/idapython/src/blob/master/examples/hexrays/vds5.py>) sample application on which **HRDevHelper** is based on. The author would also like to thank past and future contributors to the **HRDevHelper** plugin. The IDA color scheme used in the screenshots of this blog post is based on [long_night](<https://github.com/ioncodes/long_night>)._

---

