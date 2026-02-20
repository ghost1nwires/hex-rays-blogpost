# Hex-Rays Blog – Page 24

**Archived:** 2026-02-20 12:17
**URL:** https://hex-rays.com/blog/page/24

---

## 1. 

**Date:** Posted: Mar 25, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-82-decompiler-options-pseudocode-formatting](https://hex-rays.com/blog/igors-tip-of-the-week-82-decompiler-options-pseudocode-formatting)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #82: Decompiler options: pseudocode formatting

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-82-decompiler-options-pseudocode-formatting>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-82-decompiler-options-pseudocode-formatting>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-82-decompiler-options-pseudocode-formatting>)

I 

Igor Skochinsky ✦ Posted: Mar 25, 2022

![Igor’s tip of the week #82: Decompiler options: pseudocode formatting](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

The default output of the Hex-Rays decompiler tries to strike a balance between conciseness and readability. However, everyone has different preferences so it offers a few options to control the layout and formatting of the pseudocode.

### Accessing the options

Because of its origins as a third-party plugin for IDA, the decompiler options are accessible not through IDA’s Options menu, but via Edit > Plugins > Hex-Rays Decompiler, Options button

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options2-Jun-18-2024-08-40-01-9177-AM.png?width=729&height=681&name=hr_options2-Jun-18-2024-08-40-01-9177-AM.png)

### Pseudocode formatting options

Formatting options are available on the main page of the options dialog.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options3-3.png?width=497&height=357&name=hr_options3-3.png)

  * **Comment indent** : starting position for regular (end-of-line) comments. Obviously, for longer lines the comment will be shifted further to the right. [Block comments](<https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output/>) are aligned to the statement they’re attached to so this setting does not apply to them.![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options_comments-3.png?width=915&height=387&name=hr_options_comments-3.png)
  * **Block indent** : indentation for nested statements, e.g. inside `if` statements or `for`/`do`/`while` loop bodies.
  * **Right margin** : the decompiler tries to keep the pseudocode line length under the specified length. For example, it will try to split a function call with many arguments by putting them on separate lines:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options_margin-3.png?width=557&height=685&name=hr_options_margin-3.png)  
Note that in some cases you may still see lines longer than the specified margin because it’s not always possible to break a long line so that it remains valid C.
  * **Max strlit len** : maximum length of a string constant displayed directly (inline) in the pseudocode. A constant longer than this value will be replaced by a name referring to it.![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_longstr1-3.png?width=1020&height=266&name=hr_longstr1-3.png)![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_longstr2-3.png?width=781&height=490&name=hr_longstr2-3.png)
  * **Max commas** : how many [comma operators](<https://en.cppreference.com/w/c/language/operator_other#Comma_operator>) the decompiler can use in one expression to make the code more compact. By reducing this value, you should see simpler expressions at the cost of more lines of code: more/deeper nested `if` statements, extra variables for intermediate results, or even additional`goto` statements.  
For example, here’s a fragment of pseudocode with a comma statement inside the `if` condition:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_commas1-3.png?width=584&height=137&name=hr_commas1-3.png)  
After changing max commas to 1, the comma disappears at the cost of additional `if` and `goto` statements:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_commas2-3.png?width=585&height=211&name=hr_commas2-3.png)  
  




### Changing the defaults

When changing the settings from inside IDA using the UI described above, they apply only to the current database. To change the defaults for all new databases, either edit `cfg/hexrays.cfg` in IDA’s install directory, or create one in the [user directory](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>) with the options you want to override.

#### Related options

Extra empty lines can be added to the pseudocode to improve readability. This feature was described in the tip #43 ([Annotating the decompiler output](<https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output/>)).

More info: [Configuration (Hex-Rays Decompiler User Manual)](<https://www.hex-rays.com/products/decompiler/manual/config.shtml>)

---

## 2. 

**Date:** Posted: Mar 18, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-81-database-notepad](https://hex-rays.com/blog/igors-tip-of-the-week-81-database-notepad)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #81: Database notepad

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-81-database-notepad>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-81-database-notepad>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-81-database-notepad>)

I 

Igor Skochinsky ✦ Posted: Mar 18, 2022

![Igor’s tip of the week #81: Database notepad](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

There are multiple ways of annotating IDA databases: renaming, [commenting](<https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida/>), or [adding bookmarks](<https://hex-rays.com/blog/igors-tip-of-the-week-80-bookmarks/>). However, sometimes there is a need for general notes for the database as a whole, not tied to specific locations.

### Notepad window

The database notepad is a text input box which can store arbitrary text within the database, so you can add your notes and thoughts there instead of using separate software.   
It can be opened using the menu View > Open subview > Notepad. ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/notepad_win-3.png?width=916&height=218&name=notepad_win-3.png)

There is no built-in shortcut for quick access, but you can [add one](<https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>), or open the [Quick view](<https://hex-rays.com/blog/igors-tip-of-the-week-30-quick-views/>) (`Ctrl`–`1`), type “no” to select the entry and `Enter` to activate it.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/notepad_qv-3.png?width=506&height=630&name=notepad_qv-3.png)

---

## 3. 

**Date:** Posted: Mar 11, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-80-bookmarks](https://hex-rays.com/blog/igors-tip-of-the-week-80-bookmarks)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #80: Bookmarks

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-80-bookmarks>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-80-bookmarks>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-80-bookmarks>)

I 

Igor Skochinsky ✦ Posted: Mar 11, 2022

![Igor’s tip of the week #80: Bookmarks](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

In addition to [comments](<https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida/>), IDA offers a few more features for annotating and quickly navigating in the database. Today we’ll cover bookmarks.

### Adding bookmarks

Bookmarks can be added at most locations in the address-based views (disassembly listing, Hex View, Pseudocode), as well as Structures and Enums. This can be done via the Jump > Mark position… menu, or the hotkey `Alt`–`M`. You can enter a short text to describe the bookmark which will then be displayed in the bookmark list.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bookmarks_add1-3.png?width=641&height=99&name=bookmarks_add1-3.png)

In the disassembly listing, bookmarks can be quickly added while in the text view by clicking to the left of the breakpoint circles in the [execution flow arrows panel](<https://hex-rays.com/blog/igors-tip-of-the-week-50-execution-flow-arrows/>). In that case, the bookmark description will contain only the address and the label, if any. Active bookmarks are marked with the an icon which can be clicked again to remove them. Hover the mouse over the icon to see the bookmark’s description.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bookmark_add2-3.png?width=810&height=349&name=bookmark_add2-3.png)

### Managing and navigating bookmarks

To see the list of bookmarks and quickly jump to any of them, use Jump > Jump to &marked position… menu, or the `Ctrl`–`M` hotkey. This dialog can also be used to delete or edit bookmarks via the context menu or hotkeys (`Del` and `Ctrl`–`E`, respectively). 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bookmarks_jump-3.png?width=814&height=490&name=bookmarks_jump-3.png)

However, if you add many bookmarks, it can get difficult to find the one you need, so [in IDA 7.6](<https://hex-rays.com/products/ida/news/7_6/>) we’ve added a dedicated bookmarks view as well as the possibility to group bookmarks into folders. The new view is available via View > Open subviews > Bookmarks, or `Ctrl`–`Shift`–`M` shortcut. The window is non-modal and can be docked.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bookmarks_view-3.png?width=1049&height=578&name=bookmarks_view-3.png)

---

## 4. 

**Date:** Posted: Mar 4, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-79-handling-variable-reuse](https://hex-rays.com/blog/igors-tip-of-the-week-79-handling-variable-reuse)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #79: Handling variable reuse

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-79-handling-variable-reuse>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-79-handling-variable-reuse>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-79-handling-variable-reuse>)

I 

Igor Skochinsky ✦ Posted: Mar 4, 2022

![Igor’s tip of the week #79: Handling variable reuse](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Previously we’ve discussed how to reduce the number of variables used in pseudocode by [mapping](<https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables/>) copies of a variable to one. However, sometimes you may run into an opposite problem: a single variable can be used for different purposes.

### Reused stack slots

One common situation is when the compiler reuses a stack location of either a local variable or even an incoming stack argument for a different purpose. For example, in a snippet like this:
    
    
    vtbl = DiaSymbol->vtbl;
    vtbl->get_symTag(DiaSymbol, (int *)&DiaSymbol);
    Symbol->Tag = (int)DiaSymbol;

The second argument of the call is clearly an output argument and has a different meaning and type from `DiaSymbol` before the call. In such case, you can use the “Force new variable” command (shortcut `Shift`–`F`). Due to implementation details, sometimes the option is not displayed if you right-click on the variable itself; in that case try right-clicking on the start of the pseudocode line.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/newvar1-3.png?width=790&height=513&name=newvar1-3.png)

The decompiler creates a new variable at the same stack location, initially with the same type:
    
    
    IDiaSymbol *v14; // [esp+30h] [ebp+8h] FORCED BYREF
    
    vtbl = DiaSymbol->vtbl;
    vtbl->get_symTag(DiaSymbol, (int *)&v14);
    Symbol->Tag = (int)v14;
    

Naturally, you can change its type and name to a better one:
    
    
    int tag; // [esp+30h] [ebp+8h] FORCED BYREF
    
    vtbl = DiaSymbol->vtbl;
    vtbl->get_symTag(DiaSymbol, &tag);
    Symbol->Tag = tag;

### Using a union to represent a polymorphic variable

Unfortunately, “Force new variable” is not available for register variables (as of IDA 7.7). In such case, using a union may work. For example, consider this snippet from `ntdll.dll`‘s `LdrRelocateImage` function:
    
    
      int v6; // esi
      int v7; // eax
      int v8; // edi
      int v9; // eax
      
      v6 = 0;
      v20 = 0;
      v7 = RtlImageNtHeader(a1);
      v8 = v7;
      if ( !v7 )
        return -1073741701;
      v9 = *(unsigned __int16 *)(v7 + 24);
      if ( v9 == IMAGE_NT_OPTIONAL_HDR32_MAGIC )
      {
        v18 = *(_DWORD *)(v8 + 52);
        v16 = 0;
      }
      else
      {
        if ( v9 != IMAGE_NT_OPTIONAL_HDR64_MAGIC )
          return -1073741701;
        v18 = *(_DWORD *)(v8 + 48);
        v16 = *(_DWORD *)(v8 + 52);
      }
    

The function `RtlImageNtHeader` returns a pointer to the `IMAGE_NT_HEADERS` structure of the PE image at the given address. After changing its prototype and types of the variables, the code becomes a little more readable:
    
    
      int v6; // esi
      PIMAGE_NT_HEADERS v7; // eax
      PIMAGE_NT_HEADERS v8; // edi
      int Magic; // eax
      int v10; // edx
      int v11; // eax
      unsigned int v12; // ecx
      int v13; // ecx
      int v15; // [esp+Ch] [ebp-10h]
      unsigned int v16; // [esp+10h] [ebp-Ch]
      int v17; // [esp+10h] [ebp-Ch]
      unsigned int v18; // [esp+14h] [ebp-8h]
      char *v19; // [esp+14h] [ebp-8h]
      int v20; // [esp+18h] [ebp-4h] BYREF
    
      v6 = 0;
      v20 = 0;
      v7 = RtlImageNtHeader(a1);
      v8 = v7;
      if ( !v7 )
        return -1073741701;
      Magic = v7->OptionalHeader.Magic;
      if ( Magic == IMAGE_NT_OPTIONAL_HDR32_MAGIC )
      {
        v18 = v8->OptionalHeader.ImageBase;
        v16 = 0;
      }
      else
      {
        if ( Magic != IMAGE_NT_OPTIONAL_HDR64_MAGIC )
          return -1073741701;
        v18 = v8->OptionalHeader.BaseOfData;
        v16 = v8->OptionalHeader.ImageBase;
      }
    

However, there is a small problem. Judging by the checks of the magic value, the code can handle both 32-bit and 64-bit images, however the current `PIMAGE_NT_HEADERS` type is 32-bit (`PIMAGE_NT_HEADERS32`) so the code in the `else` clause is likely incorrect. If we change `v8` to `PIMAGE_NT_HEADERS64`, then the `if` clause becomes incorrect:
    
    
      if ( Magic == IMAGE_NT_OPTIONAL_HDR32_MAGIC )
      {
        ImageBase = HIDWORD(v8->OptionalHeader.ImageBase);
        v16 = 0;
      }
      else
      {
        if ( Magic != IMAGE_NT_OPTIONAL_HDR64_MAGIC )
          return -1073741701;
        ImageBase = v8->OptionalHeader.ImageBase;
        v16 = HIDWORD(v8->OptionalHeader.ImageBase);
      }
    

We can’t force a new variable because `v8` is allocated in a register and not on stack. Can we still use both types at once?

The answer is yes: we can use a union which combines both types. Here’s how it can be done in this example:

  1. Open Local Types (Shift-F1);
  2. Add a new type (Ins);
  3. Enter this definition: 
         
         union nt_headers
         {
          PIMAGE_NT_HEADERS32 hdr32;
          PIMAGE_NT_HEADERS64 hdr64;
         };

  4. change type of `v8` to `nt_headers` and use “[Select Union Field](<https://hex-rays.com/blog/igors-tip-of-the-week-75-working-with-unions/>)” to pick the correct field in each branch of the if:


    
    
      Magic = v7->OptionalHeader.Magic;
      if ( Magic == IMAGE_NT_OPTIONAL_HDR32_MAGIC )
      {
        ImageBase = v8.hdr32->OptionalHeader.ImageBase;
        ImageBaseHigh = 0;
      }
      else
      {
        if ( Magic != IMAGE_NT_OPTIONAL_HDR64_MAGIC )
          return -1073741701;
        ImageBase = v8.hdr64->OptionalHeader.ImageBase;
        ImageBaseHigh = HIDWORD(v8.hdr64->OptionalHeader.ImageBase);
      }
    

In this specific example the difference is minor and you could probably get by with some comments, but there may be situations where it makes a real difference. Note that this approach can be used for stack variables too.

See also: [Hex-Rays interactive operation: Force new variable](<https://www.hex-rays.com/products/decompiler/manual/cmd_force_lvar.shtml>)

---

## 5. 

**Date:** Posted: Feb 25, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-78-auto-hidden-messages](https://hex-rays.com/blog/igors-tip-of-the-week-78-auto-hidden-messages)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #78: Auto-hidden messages

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-78-auto-hidden-messages>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-78-auto-hidden-messages>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-78-auto-hidden-messages>)

I 

Igor Skochinsky ✦ Posted: Feb 25, 2022

![Igor’s tip of the week #78: Auto-hidden messages](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

During the work with binaries, IDA sometimes shows warnings to inform the user about unusual or potentially dangerous behavior or asks questions:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/msg3-3.png?width=351&height=212&name=msg3-3.png) ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/msg2-3.png?width=300&height=141&name=msg2-3.png) ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/msg1-3.png?width=300&height=210&name=msg1-3.png)

### Hiding messages

For some of such messages there is a checkbox “Don’t Display this message again”. If you enable it before answering or confirming the message (hint: you can press ‘D’ to [toggle it without the mouse](<https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida/>)), IDA will remember your answer and use it the next time automatically. This can be observed in the log of the [Output window](<https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output/>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/msg4-3.png?width=646&height=408&name=msg4-3.png)

### Changing the automatic answer

Sometimes you may change your mind and want to pick a different answer. For example, you’ve answered “No” to the PDB symbols questions but later you do need to load PDB symbols for a file at load time (Note: it is still [possible to do it](<https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols/>) after the fact using the File menu). Currently, there is no per message option but you can reset automatic answers for all of them using the menu Windows > Reset hidden messages…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/msg5-3.png?width=412&height=473&name=msg5-3.png)

After this, IDA will revert to the default settings and once again show all prompts and warnings, giving you a chance to answer differently.

[IDA Help: Reset Hidden Messages](<https://www.hex-rays.com/products/ida/support/idadoc/1464.shtml>)

---

## 6. 

**Date:** Posted: Feb 18, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables](https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #77: Mapped variables

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-77-mapped-variables>)

I 

Igor Skochinsky ✦ Posted: Feb 18, 2022

![Igor’s tip of the week #77: Mapped variables](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

[Quick rename](<https://hex-rays.com/blog/igors-tip-of-the-week-76-quick-rename/>) can be useful when you have code which copies data around so the variable names stay the same or similar. However, sometimes there is a way to get rid of duplicate variables altogether.

### Reasons for duplicate variables

Even if in the source code a specific variable may appear only once, on the machine code level it is not always possible. For example, most arithmetic operations use machine registers, so the values have to be moved from memory to registers to perform them. Conversely, sometimes a value has to be moved to memory from a register, for example:

  * taking a reference/address of a variable requires that it resides in memory;
  * when there are too few available registers, some variables have to be [spilled](<https://en.wikipedia.org/wiki/Register_allocation#Components_of_register_allocation>) to the stack;
  * when a calling convention uses stack for passing arguments;
  * recursive calls or closures are usually implemented by storing the current variables on the stack;
  * some other situations.



All this means that the same variable may be present in different locations during the lifetime of the function. Although the decompiler tries its best to merge these different locations into a single variable, it is not always possible, so extra variables may appear in the pseudocode.

### Example

For a simple example, we can go back to `DriverEntry` in `kprocesshacker.sys` from the [last post](<https://hex-rays.com/blog/igors-tip-of-the-week-76-quick-rename/>). The initial output looks like this:
    
    
    NTSTATUS __stdcall DriverEntry(_DRIVER_OBJECT *DriverObject, PUNICODE_STRING RegistryPath)
    {
      NTSTATUS result; // eax
      NTSTATUS v5; // r11d
      PDEVICE_OBJECT v6; // rax
      struct _UNICODE_STRING DestinationString; // [rsp+40h] [rbp-18h] BYREF
      PDEVICE_OBJECT DeviceObject; // [rsp+60h] [rbp+8h] BYREF
    
      qword_132C0 = (__int64)DriverObject;
      VersionInformation.dwOSVersionInfoSize = 284;
      result = RtlGetVersion(&VersionInformation);
      if ( result >= 0 )
      {
        result = sub_15100(RegistryPath);
        if ( result >= 0 )
        {
          RtlInitUnicodeString(&DestinationString, L"\\Device\\KProcessHacker3");
          result = IoCreateDevice(DriverObject, 0, &DestinationString, 0x22u, 0x100u, 0, &DeviceObject);
          v5 = result;
          if ( result >= 0 )
          {
            v6 = DeviceObject;
            DriverObject->MajorFunction[0] = (PDRIVER_DISPATCH)&sub_11008;
            qword_132D0 = (__int64)v6;
            DriverObject->MajorFunction[2] = (PDRIVER_DISPATCH)&sub_1114C;
            DriverObject->MajorFunction[14] = (PDRIVER_DISPATCH)&sub_11198;
            DriverObject->DriverUnload = (PDRIVER_UNLOAD)sub_150EC;
            v6->Flags &= ~0x80u;
            return v5;
          }
        }
      }
      return result;
    }

We can see that there are two variables which look redundant: `v5` and `v6`. `v5` is a copy of `result` which resides in `r11d` and `v6` is a copy of `DeviceObject` which resides in `rax`. It seems they were introduced for related reasons:

  1. The compiler had to move `DeviceObject` from the stack to a register to initialize the global variable qword_132D0 and also modify the `Flags` member. It picked the register `rax` for that;
  2. Because `rax` already contained the `result` variable (in the lower part of it: `eax`), it had to be saved elsewhere in the meantime (and moved back to `eax` at the end of manipulations with `DeviceObject`);
  3. The decompiler could not automatically merge `DeviceObject`with `v6` because they use different storage types (stack vs register) and because in theory the writes to `DriverObject->MajorFunction` could have changed the stack variable, so the values would not be the same anymore.



### Mapping variables

After looking at the code closely, it seems that `v5` and `v6` can be replaced correspondingly by `result` and `DeviceObject` in all cases. To ask the decompiler do it, we can use “[Map to another variable](<https://hex-rays.com/products/decompiler/manual/cmd_map_lvar.shtml>)” action from the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mapvar1-3.png?width=366&height=234&name=mapvar1-3.png)

When you use it for the first time, the following warning appears:

![Information: The 'map variable' command allows you to replace all occurrences of a variable by another variable of the same type. The decompiler does not perform any checks. An wrong mapping may render the decompiler output incorrect. Use this command cautiously!](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mapvar2-Jun-18-2024-08-57-23-5924-AM.png?width=439&height=197&name=mapvar2-Jun-18-2024-08-57-23-5924-AM.png)

Alternatively, you can use the hotkey `=` (equals sign); it’s best to use it on the initial assignment such as `v6 = DeviceObject` because then the best match (the other side of assignment) will be preselected in the list of replacement candidates. In our case we get only one candidate, but in big functions you may have several variables of the same type, so triggering the action on an assignment helps ensure that you pick the correct one.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mapvar3-Jun-18-2024-08-57-22-5621-AM.png?width=438&height=174&name=mapvar3-Jun-18-2024-08-57-22-5621-AM.png)

After mapping both variables, the output no longer mentions them:
    
    
    NTSTATUS __stdcall DriverEntry(_DRIVER_OBJECT *DriverObject, PUNICODE_STRING RegistryPath)
    {
      NTSTATUS result; // eax MAPDST
      struct _UNICODE_STRING DestinationString; // [rsp+40h] [rbp-18h] BYREF
      PDEVICE_OBJECT DeviceObject; // [rsp+60h] [rbp+8h] MAPDST BYREF
    
      qword_132C0 = (__int64)DriverObject;
      VersionInformation.dwOSVersionInfoSize = 284;
      result = RtlGetVersion(&VersionInformation);
      if ( result >= 0 )
      {
        result = sub_15100(RegistryPath);
        if ( result >= 0 )
        {
          RtlInitUnicodeString(&DestinationString, L"\\Device\\KProcessHacker3");
          result = IoCreateDevice(DriverObject, 0, &DestinationString, 0x22u, 0x100u, 0, &DeviceObject);
          if ( result >= 0 )
          {
            DriverObject->MajorFunction[0] = (PDRIVER_DISPATCH)&sub_11008;
            qword_132D0 = (__int64)DeviceObject;
            DriverObject->MajorFunction[2] = (PDRIVER_DISPATCH)&sub_1114C;
            DriverObject->MajorFunction[14] = (PDRIVER_DISPATCH)&sub_11198;
            DriverObject->DriverUnload = (PDRIVER_UNLOAD)sub_150EC;
            DeviceObject->Flags &= ~0x80u;
          }
        }
      }
      return result;
    }

You can see that `result` and `DeviceObject` variables now have a new [annotation](<https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations/>): `MAPDST`. This means that some other variable(s) have been mapped to them. 

### Unmapping variables

If you’ve changed your mind and want to see how the original pseudocode looked like, or observe something suspicious in the output involving mapped variables, you can remove the mapping by right-clicking a mapped variable (marked with `MAPDST`) and choosing “Unmap variable(s)”.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mapvar4-3.png?width=437&height=198&name=mapvar4-3.png)

More info: [Hex-Rays interactive operation: Map to another variable](<https://hex-rays.com/products/decompiler/manual/cmd_map_lvar.shtml>)

---

## 7. 

**Date:** Posted: Feb 11, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-76-quick-rename](https://hex-rays.com/blog/igors-tip-of-the-week-76-quick-rename)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #76: Quick rename

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-76-quick-rename>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-76-quick-rename>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-76-quick-rename>)

I 

Igor Skochinsky ✦ Posted: Feb 11, 2022

![Igor’s tip of the week #76: Quick rename](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

One of the features added in [IDA 7.6](<https://hex-rays.com/products/ida/news/7_6/>) was automatic renaming of variables in the decompiler. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename1-3.png?width=530&height=509&name=rename1-3.png)

Unlike [PIT](<https://hex-rays.com/blog/igors-tip-of-the-week-74-parameter-identification-and-tracking-pit/>), it is not limited to stack variables but also handles variables stored in registers and not just calls but also assignments and some other expressions. It also tries to interpret function names which include a verb (get, make, fetch, query etc.) and rename the assigned result accordingly.

### Triggering renaming manually

To cover situations where automatic renaming fails or insufficient, the decompiler also supports a manual action called “Quick Rename” with the default hotkey `Shift`–`N`. It can be used to propagate names across assignments and other expressions. Usually it only renames dummy variables which were not explicitly named by the user (`v1`, `v2`, etc.). Here is an incomplete list of rules used by the action:

  * by name of the opposite variable in assignments: `v1 = myvar`: rename **v1** -> **myvar1**
  * by name of the opposite variable in comparisons: `offset < v1`: rename **v1** -> **offset1**
  * as pointer to a well-named variable: `v1 = &Value`: rename **v1** -> **p_Value**
  * by structure field in expressions: `v1 = x.Next`: rename **v1** -> **Next**
  * as pointer to a structure field: `v1 = &x.left`: rename **v1** -> **p_left**
  * by name of formal argument in a call: `close(v1)`: rename **v1** -> **fd**
  * by name of a called function: `v1=create_table()`: rename **v1** -> **table**
  * by return type of called function: `v1 = strchr(s, '1')`: rename **v1** -> **str**
  * by a string constant: `v1 = fopen("/etc/fstab", "r")`: rename **v1** -> **etc_fstab**
  * by variable type: **error_t v1** : rename **v1** -> **error**
  * standard name for the result variable: `return v1`: rename **v1** -> **ok** if current function returns bool



### Example: Windows driver

We’ll inspect the driver used by [Process Hacker](<https://processhacker.sourceforge.io/>) to perform actions requiring kernel mode access. On opening `kprocesshacker.sys`, IDA automatically applies well-known function prototype to the `DriverEntry` entrypoint and loads kernel mode type libraries, so the default decompilation is already decent:
    
    
    NTSTATUS __stdcall DriverEntry(_DRIVER_OBJECT *DriverObject, PUNICODE_STRING RegistryPath)
    {
      NTSTATUS result; // eax
      NTSTATUS v5; // r11d
      PDEVICE_OBJECT v6; // rax
      struct _UNICODE_STRING DestinationString; // [rsp+40h] [rbp-18h] BYREF
      PDEVICE_OBJECT DeviceObject; // [rsp+60h] [rbp+8h] BYREF
    
      qword_132C0 = (__int64)DriverObject;
      VersionInformation.dwOSVersionInfoSize = 284;
      result = RtlGetVersion(&VersionInformation);
      if ( result >= 0 )
      {
        result = sub_15100(RegistryPath);
        if ( result >= 0 )
        {
          RtlInitUnicodeString(&DestinationString, L"\\Device\\KProcessHacker3");
          result = IoCreateDevice(DriverObject, 0, &DestinationString, 0x22u, 0x100u, 0, &DeviceObject);
          v5 = result;
          if ( result >= 0 )
          {
            v6 = DeviceObject;
            DriverObject->MajorFunction[0] = (PDRIVER_DISPATCH)&sub_11008;
            qword_132D0 = (__int64)v6;
            DriverObject->MajorFunction[2] = (PDRIVER_DISPATCH)&sub_1114C;
            DriverObject->MajorFunction[14] = (PDRIVER_DISPATCH)&sub_11198;
            DriverObject->DriverUnload = (PDRIVER_UNLOAD)sub_150EC;
            v6->Flags &= ~0x80u;
            return v5;
          }
        }
      }
      return result;
    }

However, to make sense of it we need to make some changes. The indexes into the `MajorFunction` array are so-called [IRP Major Function Codes](<https://docs.microsoft.com/en-us/windows-hardware/drivers/kernel/irp-major-function-codes>) which have symbolic names starting with `IRP_MJ_`. So we can apply the Enum action (`M` hotkey) to convert numbers to the corresponding symbolic constants available in the type library.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename2_enum-3.png?width=630&height=533&name=rename2_enum-3.png)

Afterwards we can rename the corresponding routines and make the pseudocode look very similar to the [standard DriverEntry](<https://docs.microsoft.com/en-us/windows-hardware/drivers/kernel/driverentry-s-required-responsibilities>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename3-3.png?width=895&height=208&name=rename3-3.png)

To get rid of the casts, set the proper prototypes to the [dispatch routines](<https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch>) using the “[Set item type](<https://hex-rays.com/products/decompiler/manual/cmd_settype.shtml>)” action (`Y` hotkey). We can use the same prototype string for all three routines:  
`NTSTATUS Dispatch(PDEVICE_OBJECT Device, PIRP Irp)`

This works because function name is not considered to be a part of function prototype and is ignored by IDA. For the unload function, the prototype is different:  
`void Unload(PDRIVER_OBJECT Driver)`

After setting the prototypes, no more casts:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename4-3.png?width=921&height=508&name=rename4-3.png)

Now we can go into `KhDispatchDeviceControl` to investigate how it works. Thanks to the preset prototype, the initial pseudocode looks plausible at the first glance:
    
    
    NTSTATUS __stdcall KhDispatchDeviceControl(PDEVICE_OBJECT Device, PIRP Irp)
    {
      // [COLLAPSED LOCAL DECLARATIONS. PRESS KEYPAD CTRL-"+" TO EXPAND]
    
      v13 = Irp;
      CurrentStackLocation = Irp->Tail.Overlay.CurrentStackLocation;
      FsContext = CurrentStackLocation->FileObject->FsContext;
      Parameters = CurrentStackLocation->Parameters.CreatePipe.Parameters;
      Options = CurrentStackLocation->Parameters.Create.Options;
      LowPart = CurrentStackLocation->Parameters.Read.ByteOffset.LowPart;
      AccessMode = Irp->RequestorMode;
      if ( !FsContext )
      {
        v9 = -1073741595;
        goto LABEL_105;
      }
      if ( LowPart != -1718018045
        && LowPart != -1718018041
        && (dword_132CC == 2 || dword_132CC == 3)
        && (*FsContext & 2) == 0 )
    

But on closer inspection, some oddities become apparent. The `Parameters` member of the [`_IO_STACK_LOCATION`](<https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/ns-wdm-_io_stack_location>) structure is a union which contains the request-specific parameters. With insufficient information, the decompiler picked the first matching members, but they do not make sense for the request we’re dealing with. For `IRP_MJ_DEVICE_CONTROL`, the `DeviceIoControl` struct should be used. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename5-3.png?width=784&height=503&name=rename5-3.png)

Thus, we can use the “[Select union field](<https://hex-rays.com/products/decompiler/manual/cmd_select_union_field.shtml>)” action (`Alt`–`Y` hotkey) to choose `DeviceIoControl` on the three references to `CurrentStackLocation->Parameters` to see which parameters are actually being used.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename6-3.png?width=825&height=641&name=rename6-3.png)

The references have been changed, but the variable names and types remain. In such situation, we can update the names by using Quick rename (`Shift`–`N`) on the assignments.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename7-3.png?width=1064&height=60&name=rename7-3.png)

To get rid of the cast, we can either change the `Type3InputBuffer` variable type to` void*` manually, or simply refresh the decompilation (F5). This causes the decompiler to rerun the type derivation algorithm and update types of automatically typed variables.

Now the pseudocode more closely reflects what is going on. In particular, we can see that the first comparisons are checking the `IoControlCode` against some expected values, which makes more sense than the original `LowPart`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rename8-3.png?width=734&height=306&name=rename8-3.png)

### Other uses

Quick rename can be useful when automatic renaming fails due to a name conflict. For example, if we go back to `DriverEntry`, we can see that `DeviceObject` is copied to a temporary variable `v6`:
    
    
            v6 = DeviceObject;
            DriverObject->MajorFunction[IRP_MJ_CREATE] = KhDispatchCreate;
            qword_132D0 = (__int64)v6;
            DriverObject->MajorFunction[IRP_MJ_CLOSE] = KhDispatchClose;
            DriverObject->MajorFunction[IRP_MJ_DEVICE_CONTROL] = KhDispatchDeviceControl;
            DriverObject->DriverUnload = KhUnload;
            v6->Flags &= ~0x80u;
    

We can rename `v6` manually, or simply press `Shift`–`N` on the assignment and the decompiler will reuse the name with a numerical suffix to resolve the conflict:
    
    
            DeviceObject1 = DeviceObject;
            DriverObject->MajorFunction[IRP_MJ_CREATE] = KhDispatchCreate;
            qword_132D0 = (__int64)DeviceObject1;
            DriverObject->MajorFunction[IRP_MJ_CLOSE] = KhDispatchClose;
            DriverObject->MajorFunction[IRP_MJ_DEVICE_CONTROL] = KhDispatchDeviceControl;
            DriverObject->DriverUnload = KhUnload;
            DeviceObject1->Flags &= ~0x80u;

---

## 8. 

**Date:** Posted: Feb 10, 2022  
**Link:** [https://hex-rays.com/blog/introducing-the-patching-plugin](https://hex-rays.com/blog/introducing-the-patching-plugin)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Introducing the ‘Patching’ plugin

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-the-patching-plugin>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-the-patching-plugin>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-the-patching-plugin>)

Arnaud Diederen ✦ Posted: Feb 10, 2022

![Introducing the ‘Patching’ plugin](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

**This is a guest entry written by Markus Gaasedelen from[RET2 SYSTEMS](<https://ret2.io/>). His views and opinions are his own, and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to him, through the github.com repository.**

# Refreshing IDA’s Binary Patching Workflow

Patching assembly code to change the behavior of an existing program is not uncommon in malware analysis, software reverse engineering, and broader domains of security research. It’s an important practice that can be used to fix bugs, skip problematic (even dangerous) code, or augment software to make it easier to study.

As a long-time user of IDA Pro, I always felt teased by the fact that there wasn’t an easy and intuitive way to modify the assembly code it presents. This became the driving motivation behind my desire to build a plugin that could breathe new life into IDA’s patching experience.

## An Interactive Patching Plugin for IDA Pro

Today, I am releasing a new interactive [patching plugin](<https://github.com/gaasedelen/patching>) for IDA Pro (7.6 and up) which is designed to accommodate fast-paced workflows that demand rapid iteration.

The plugin embeds several new contextual patching actions into IDA’s right click disassembly menus:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/context-3.png?width=717&height=537&name=context-3.png)

It also includes a dedicated dialog for editing or assembling new instructions:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patching-3.png?width=631&height=483&name=patching-3.png)

As an initial release, the plugin only supports Intel x86/x64 and ARM/ARM64. In the next release, it will be extended to support some of the other popular architectures including PPC and MIPS.

## Additional Information

A more complete description of the plugin and its features are available on [GitHub](<https://github.com/gaasedelen/patching>). Going forward, the latest distributions of the plugin will be made available on the [releases](<https://github.com/gaasedelen/patching/releases>) page. Please report any bugs or issues with the plugin directly to the GitHub repo.

Happy patching!

_Thanks to Hex-Rays for supporting the development of this plugin!_

---

## 9. 

**Date:** Posted: Feb 8, 2022  
**Link:** [https://hex-rays.com/blog/introducing-the-idaclang-tutorial](https://hex-rays.com/blog/introducing-the-idaclang-tutorial)

[Back](<https://hex-rays.com/blog>)

[IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [IDAClang](<https://hex-rays.com/blog/tag/idaclang>)

# Introducing the IDAClang Tutorial

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-the-idaclang-tutorial>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-the-idaclang-tutorial>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-the-idaclang-tutorial>)

Troy Bowman ✦ Posted: Feb 8, 2022

![Introducing the IDAClang Tutorial](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

As part of the [7.7](<https://hex-rays.com/products/ida/news/7_7/>) release, IDA bundles a new C++ parser based on the libclang library from the LLVM project.

In addition to that, we wrote [a new command-line utility](<https://hex-rays.com/products/ida/support/ida/idaclang77.zip>), which allows you to build custom type libraries from C/C++ codebases. (Note: this link is protected by the download area password, included in your latest download e-mail)

Given how powerful those new features are (both the bundled IDA plugin, and the command-line utility), we also created a brand [new tutorial](<https://hex-rays.com/tutorials/idaclang/idaclang_tutorial.pdf>), demonstrating how they can be used – from basic usage, to building type libraries for large projects such as the Linux kernel & the iOS SDK.

We hope you find the new tutorial informative!

Links:  
* [PDF tutorial](<https://docs.hex-rays.com/user-guide/types/type-libraries/idaclang_tutorial>)  
* [HTML tutorial](<https://docs.hex-rays.com/user-guide/types/type-libraries/idaclang_tutorial>)  
* [IDAClang command line utility](<https://my.hex-rays.com/dashboard/download-center>)

---

