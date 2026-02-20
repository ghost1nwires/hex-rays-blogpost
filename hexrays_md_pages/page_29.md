# Hex-Rays Blog – Page 29

**Archived:** 2026-02-20 12:19
**URL:** https://hex-rays.com/blog/page/29

---

## 1. 

**Date:** Posted: Jul 23, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-49-navigation-band](https://hex-rays.com/blog/igors-tip-of-the-week-49-navigation-band)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #49: Navigation band

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-49-navigation-band>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-49-navigation-band>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-49-navigation-band>)

I 

Igor Skochinsky ✦ Posted: Jul 23, 2021

![Igor’s tip of the week #49: Navigation band](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Navigation band, also sometimes called the navigator, or navbar, is the UI element shown by default at the top of IDA’s window, in the toolbar area.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/navbar_default-3.png?width=851&height=150&name=navbar_default-3.png)

It shows the global overview of the program being analyzed and allows to see at a quick glance how well has the program been analyzed and what areas may need attention.

### Colors

The colors are explained in the legend; the default color scheme uses the following colors:

  1. __Cyan/turquose: Library functions, i.e. functions which have been recognized by a[FLIRT signature](<https://hex-rays.com/products/ida/tech/flirt/in_depth/>). Usually such functions cone from the compiler or third party libraries and not the code written by the programmer, so they can often be ignored as a known quantity;
  2. __Blue: Regular functions, i.e. functions not recognized by FLIRT or Lumina. These could contain the custom functionality, specific to the program;
  3. __Maroon/brown: instructions(code) not belonging to any functions. These could appear when IDA did not detect or misdetected function boundaries, or hint at code obfuscation being employed which could prevent proper function creation. It could also be data incorrectly being treated as code.
  4. __Gray: data. This color is used for all defined data items (string literals, arrays, individual variables).
  5. __Olive: unexplored bytes, i.e. areas not yet converted to either code or data.
  6. __Magenta: used to mark functions or data imported from other modules (including wrapper thunks for imported functions).
  7. __Lime green: functions recognized by Lumina. They could be either library functions, or custom functions seen previously in other binaries and uploaded by users to the public Lumina server.



Colors can be changed when changing the color scheme, or individually in Options > Colors… , Navigation band.

### Indicators

In addition to the colors, there may be additional indicators on the navigation band. The yellow arrow is the current cursor position in the disassembly (IDA View), while the small orange triangle on the opposite side shows the current autoanalysis location (it is only visible while autoanalysis is in progress).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/navbar_ind-3.png?width=253&height=68&name=navbar_ind-3.png)

### Additional display

The combobox (dropdown) at the right of the navigation band allows you to add some additional markers to it. For example, you can show:

  * Entry points (exported functions);
  * Binary or text pattern [search results](<https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida/>);
  * [immediate search](<https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida/>) results;
  * [cross references](<https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references/>) to a specific address;
  * bookmarked positions;
  * etc.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/navbar_add-3.png?width=166&height=157&name=navbar_add-3.png)

The markers show up as red circles and can be clicked to navigate.

### Configuration

The control can be hidden or shown via View > Toolbars > Navigator, or the same item in the toolbar’s context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/navbar_menu-3.png?width=200&height=133&name=navbar_menu-3.png)

It can be placed at any of the four sides of IDA’s window by using the drag handle.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/navbar_vertical-3.png?width=206&height=679&name=navbar_vertical-3.png)

In the horizontal position, you can show or hide the legend and the additional display combobox from the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/navbar_options1-3.png?width=591&height=174&name=navbar_options1-3.png)

### Navigation and zooming

By default, the navigation band shows the complete program, however you can zoom in to see a more detailed view of a specific part. Zooming can be done by `Ctrl` \+ mouse wheel, or from the context menu. The numerical options specify how many bytes of the program are represented by one pixel on the band.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/navbar_zoom-3.png?width=340&height=223&name=navbar_zoom-3.png)

Once zoomed in, the visible part can be scrolled with the mouse wheel or by clicking the arrow buttons at either end of the band. You can click into any part of the band to navigate there in the disassembly view.

---

## 2. 

**Date:** Posted: Jul 16, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida](https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #48: Searching in IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida>)

I 

Igor Skochinsky ✦ Posted: Jul 16, 2021

![Igor’s tip of the week #48: Searching in IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

We covered how to search for things in [choosers (list views)](<https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida/>), but what if you need to look for something elsewhere in IDA?

## Text search

When searching for textual content, the same shortcut pair (`Alt`–`T` to start, `Ctrl`–`T` to continue) works almost anywhere IDA shows text:

  * Disassembly (IDA View)
  * Hex View
  * Decompiler output (Pseudocode)
  * Output window
  * Structures and Enums windows
  * Choosers (list views)



This search matches text anywhere in the current view, for example both the instructions and comments, if present.

For the main windows, the action is also accessible via the Search > Text… menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_text-3.png?width=388&height=234&name=search_text-3.png)

The notice “(slow!)” refers to the fact that for text searching, IDA has to render **all** text lines in the range being searched, which can get quite slow, especially for big binaries. However, if you need the features like regexp matching, or searching for text in comments, the wait could be worth it.

## Binary search

Available as the shortcut pair `Alt`–`B`/`Ctrl`–`B`, or Search > Sequence of bytes…, this feature allows searching for byte sequences (including string literals) and patterns in the database (including process memory during debugging). 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_binary-300x212-3.png?width=300&height=212&name=search_binary-300x212-3.png)

The input line accepts the following inputs:

  1. byte sequence (space-delimited): `01 02 03 04`
  2. byte sequence with wildcard bytes represented by question marks: `68 ? ? ? 0` will match both `68 C4 1A 48 00` and `68 D8 1A 48 00`.
  3. one or more numbers in the selected radix (hexadecimal, decimal or octal). The number will be converted to the minimal necessary number of bytes according to the current processor endianness. For example, `04469E0` will be converted to `E0 69 44` on x86 (a little-endian processor). This feature is useful for finding values in data areas or embedded in instructions (immediates).
  4. Quoted string literals, for example `"Error"`. The string will be converted to bytes using the encoding specified in the encoding selector. If “All Encodings” is selected, search will be performed using [all configured encodings](<https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings/>).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_binarystr-3.png?width=449&height=343&name=search_binarystr-3.png)
  5. Wide-character string constant (e.g. `L"test"`). Only UTF-16 is used convert such strings to raw bytes.



## Immediate search

As mentioned previously, the same instruction operand can be [represented in different ways](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>) in IDA. For example, an instruction like

`test dword ptr [eax], 10000h`

can be also displayed as

`test dword ptr [eax], 65536`

or even

`test dword ptr [eax], AW_HIDE`

So if you do the text search for `10000h`, IDA will find the first variation but not the other two. On x86, you can use binary search for `10000` hex (will be converted to byte sequence `00 00 01`), but this will not work for processors which use instruction encodings on non-byte boundary, or may give many false positives if unrelated instructions happen to match the byte sequence. So here’s why the immediate search is preferable:

  1. it only checks instructions with numerical operands or data items, improving search speed and reducing false positives;
  2. it compares the **numerical value** of the operand, so any change in representation does not prevent the match, meaning it will find any of the three variations above



Available as the shortcut pair `Alt`–`I`/`Ctrl`–`I`, or Search > Immediate value…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_imm-3.png?width=257&height=271&name=search_imm-3.png)

The value can be entered in any numerical base using the C syntax (decimal, hex, octal).

## Search direction

By default, all searches are performed “down” from the current position, i.e. toward increasing addresses. You can change it by checking “Search Up” in the individual search dialogs or beforehand via Search > Search direction. The currently set value is displayed in the menu item as well as IDA’s status bar.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_direction-3.png?width=241&height=68&name=search_direction-3.png)

The “search next” commands and shortcuts (`Ctrl`–`T`, `Ctrl`–`B`, `Ctrl`–`I`) also use this setting.

### Find all occurrences

## ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_findalld-3.png?width=348&height=246&name=search_findalld-3.png)

This checkbox allows you to get results of the search over whole database or view in a list which you can then inspect at your leisure instead of looking at every search hit one by one.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/search_findall-3.png?width=466&height=411&name=search_findall-3.png)

## Picking the search type

This is not a definitive guide but here are some suggestions:

  1. text (e.g. prompt or error message) displayed by the program: binary search for the quoted substring (NB: this will not work if the string is not hardcoded but is in an external file or resource stream not loaded by IDA).
  2. magic constant or error code: immediate search (in some cases binary search for the value can work too).
  3. an address to which there are no apparent cross references: binary search for the address value (will only succeed if the reference actually uses the value directly without calculating it in some way).
  4. specific instruction opcode pattern: binary search for byte sequence (possibly with wildcard bytes).
  5. instruction not having a fixed encoding: text search for mnemonic and/or operands (possibly as regexp).



More info: [Search submenu](<https://hex-rays.com/products/ida/support/idadoc/568.shtml>)

---

## 3. 

**Date:** Posted: Jul 9, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida](https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [debugger](<https://hex-rays.com/blog/tag/debugger>)

# Igor’s tip of the week #47: Hints in IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida>)

I 

Igor Skochinsky ✦ Posted: Jul 9, 2021

![Igor’s tip of the week #47: Hints in IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Hints (aka tooltips) are popup windows with text which appear when you hover the mouse cursor over a particular item in IDA. They are available in many situations.  


## Disassembly hints

In the disassembly view, hints can be shown in the following cases:

  1. When hovering over names or addresses, a fragment of disassembly at the destination is shown.   
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_addr-3.png?width=840&height=155&name=hint_addr-3.png)
  2. When hovering over stack variables, a fragment of the stack frame layout is shown  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_stkvar-3.png?width=635&height=230&name=hint_stkvar-3.png)
  3. When hovering over structure offset operands, the fragment of the struct definition.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_strmem-3.png?width=588&height=289&name=hint_strmem-3.png)
  4. For enum operands – the enum with the definition.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_enum-3.png?width=584&height=171&name=hint_enum-3.png)
  5. For [renamed registers](<https://hex-rays.com/blog/igors-tip-of-the-week-24-renaming-registers/>), the hint shows the original register name  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_reg-3.png?width=309&height=60&name=hint_reg-3.png)



All these hints except the last one can be expanded or shrunk using the **mouse wheel**.

## Decompiler hints

In the pseudocode, the hints are shown for:

  1. Local variables and current function arguments: type and location (register or stack).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_hrvar-3.png?width=436&height=135&name=hint_hrvar-3.png)
  2. global variables: type.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_hrgvar-3.png?width=416&height=96&name=hint_hrgvar-3.png)
  3. structure or union members: member type and offset.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_hrstrmem-3.png?width=522&height=80&name=hint_hrstrmem-3.png)
  4. function calls: prototype and information about arguments and return value.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_hrfunc-3.png?width=502&height=119&name=hint_hrfunc-3.png)
  5. other expressions and operators: type, signedness, etc.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_hrop-3.png?width=438&height=77&name=hint_hrop-3.png)



## Debugger hints

During debugging, the hints behave mostly in the same way but with addition of dynamic information:

  1. In the disassembly view, hovering on instruction operands shows a hint with their values and, if the value resolves to a valid address, a fragment of memory at that address.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_dbgasm-3.png?width=498&height=114&name=hint_dbgasm-3.png)
  2. In pseudocode, values of variables are shown in hints.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_dbgpseudo-3.png?width=546&height=77&name=hint_dbgpseudo-3.png)



## Configuring hints

The way hints work can be configured via Options > General…, Browser tab. You can set how many lines are displayed by default and the delay before the hint is shown. The hints can be disabled completely by setting the number of lines to 0, or only disabled during the debugging (showing the hint during debugging may lead to memory reads which can be slow in some situations).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_options-3.png?width=584&height=202&name=hint_options-3.png)

See also: [Browser options](<https://hex-rays.com/products/ida/support/idadoc/1304.shtml>)

---

## 4. 

**Date:** Posted: Jul 2, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation](https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #46: Disassembly operand representation

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation>)

I 

Igor Skochinsky ✦ Posted: Jul 2, 2021

![Igor’s tip of the week #46: Disassembly operand representation](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

As we’ve mentioned before, the I in IDA stands for _interactive_ , and we already covered some of the disassembly view’s interactive features like [renaming](<https://hex-rays.com/blog/igors-tip-of-the-week-24-renaming-registers/>) or [commenting](<https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida/>). However, other changes are possible too. For example, you can change the _operand representation_ (sometimes called _operand type_ in documentation). What is it about?

Most assemblers (and disassemblers) represent machine instructions using a _mnemonic_ (which denotes the basic function of the instruction) and _operands_ on which it acts (commonly delimited by commas). As an example, let’s consider the most common x86 instruction `mov`, which copies data between two of its operands. A few examples:

`mov rsp, r11` – copy the value of `r11` to `rsp`

`mov rcx, [rbx+8]` – copy a 64-bit value from the address equal to value of the register `rbx` plus 8 to `rcx` (C-like equivalent: `rcx = *(int64*)(rbx+8);`)

`mov [rbp+390h+var_380], 2000000h` – copy the value `2000000h` (0x2000000 in C notation) to the stack variable `var_380`

The first example uses two registers as operands, the second a register and an indirect memory operand with _base register_ and _displacement_ , the third — another memory operand as well as an _immediate_ (a constant value encoded directly in the instruction’s opcode).

The last two examples are interesting because they involve numbers (displacements and immediates), and the same number can be _represented_ in multiple ways. For example, consider the following instructions:
    
    
    mov eax, 64h
    mov eax, 100
    mov eax, 144o
    mov eax, 1100100b
    mov eax, 'd'
    mov eax, offset byte_64
    mov eax, mystruct.field_64

All of them have exactly the same byte sequence (machine code) on the binary level: `B8 64 00 00 00`. So, while picking another operand representation may change the visual aspect, the underlying value and the program behavior **does not change**. This allows you to choose the best variant which represents the intent behind the code without having to add a long explanation in comments.

The following representations are available in IDA for numerical operands (some of them may only make sense in specific situations):

  1. Default number representation (aka **void**): used when there is no specific override applied on the operand (either by the user or IDA’s autoanalyzer or the processor module). The actually used representation depends on the processor module but the most common fallback is hexadecimal. Uses **orange color** in the default color scheme. For values which match a printable character in the current encoding, a comment with the character could be displayed (depends on the processor module).  
Hotkey: `#` (hash sign).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/operands_void-3.png?width=207&height=73&name=operands_void-3.png)
  2. Decimal: shows the operand as a decimal number. Hotkey is `H`.
  3. Hexadecimal: explicitly show the operand as hexadecimal. Hotkey is `Q`.
  4. Binary: shows the operand as a binary number. Hotkey is `B`.
  5. Octal: shows the operand as an octal number. No default hotkey but can be picked from the context menu or the “Operand type” toolbar.
  6. Character: shows the operand as a character constant if possible. Hotkey: `R`.
  7. Structure offset: replaces the numerical operand with a reference to a structure member with a matching offset. Hotkey: `T`.
  8. Enumeration (symbolic constant): the number is replaced by a symbolic constant with the same value. Hotkey: `M`.
  9. Stack variable: the number is replaced by a symbolic reference into the current function’s stack frame. Usually only makes sense for instructions involving stack pointer or frame pointer. Hotkey: `K`.
  10. Floating-point constant: only works in some cases and for some processors. For example, `3F000000h`(`0x3F000000`) is actually an IEEE-754 encoding of the number `0.5`. There is no default hotkey but the conversion can be performed via the toolbar or main menu.
  11. Offset operand: replace the number by an expression involving one or more addresses in the program. Hotkeys: `O`, `Ctrl`–`O` or `Ctrl`–`R` (for complex offsets).



All hotkeys revert to the default representation if applied twice.

In addition to the hotkeys, the most common conversions can be done via the context menu:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/operands_ctx-3.png?width=318&height=185&name=operands_ctx-3.png)

The full list is available in the main menu (Edit > Operand Type):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/operands_menu-3.png?width=409&height=193&name=operands_menu-3.png)

as well as the “Operand Type” toolbar:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/operands_toolbar-3.png?width=524&height=396&name=operands_toolbar-3.png)

Two more transformations can be applied to an operand on top of changing its numerical base:

  1. Negation. Hotkey `_` (underscore). Can be used, for example, to show `-8` instead of `0FFFFFFF8h` (two representations of the same binary value).
  2. Bitwise negation (aka inversion or binary NOT). Hotkey: `~` (tilde). For example, `0FFFFFFF8h` is considered to be the same as `not 7`.



Finally, if you want to see something completely custom which is not covered by the existing conversions, you can use a _manual operand_. This allows you to replace the operand by an arbitrary text; it is not checked by IDA so it’s up to you to ensure that the new representation matches the original value. Hotkey: `Alt`–`F1`.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/operands_manual-3.png?width=353&height=150&name=operands_manual-3.png)

---

## 5. 

**Date:** Posted: Jul 2, 2021  
**Link:** [https://hex-rays.com/blog/new-ida-training-session-registration-is-now-open](https://hex-rays.com/blog/new-ida-training-session-registration-is-now-open)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Programming](<https://hex-rays.com/blog/tag/programming>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [idatraining](<https://hex-rays.com/blog/tag/idatraining>)

# New IDA Training session: Registration is now open!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/new-ida-training-session-registration-is-now-open>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/new-ida-training-session-registration-is-now-open>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/new-ida-training-session-registration-is-now-open>)

Geoffrey Czokow ✦ Posted: Jul 2, 2021

![New IDA Training session: Registration is now open!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

A second [2021 IDA training session](</products/ida/training/>) will take place online on 13-17 and 20-22 September 2021, EDT time.

The session is devised to help professional reverse engineers master IDA Pro, the top notch software disassembly tool and take their reversing skills to the next level. Provided by the experts behind IDA, the training offers its participants unique opportunity to dissect the famous disassembler, understand its internals and upgrade their analysis with IDA’s limitless capabilities.

The first five days (13-17 September) will be devoted to the **[standard IDA training](</training/#standard>)**. The following three days (20-22 September) to a course on **[advanced training (Programming for IDA)](</training/#advanced>)**.

Training will be both theoretical and practical. After each theoretical section hands-on exercises will be carried out so as to master thorough understanding of concepts and methods. Training material is always updated to include the latest additions to IDA.

Detailed information including [course programs, cost and registration forms](</training/>) can be found in our dedicated training page. If needed, additional information may be requested by emailing our [sales](<mailto:sales@hex-rays.com>) team.

[Learn more about the course](</training/>) [Book a seat](<https://hex-rays.com/wp-content/uploads/2021/07/hexrays_training_2021-09.docx>)

---

## 6. 

**Date:** Posted: Jun 25, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types](https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #45: Decompiler types

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types>)

I 

Igor Skochinsky ✦ Posted: Jun 25, 2021

![Igor’s tip of the week #45: Decompiler types](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

In one of the previous posts, we’ve discussed how to [edit types of functions and variables](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>) used in the pseudocode. In most cases, you can use the standard C types: `char`, `int`, `long` and so on. However, there may be situations where you need a more specific type. Decompiler may also generate such types itself so recognizing them is useful. The following custom types may appear in the pseudocode or used in variable and function types:

#### Explicitly-sized integer types

  * `__int8` – 1-byte integer (8 bits)
  * `__int16` – 2-byte integer (16 bits
  * `__int32` – 4-byte integer (32 bits)
  * `__int64` – 8-byte integer (64 bits)
  * `__int128` – 16-byte integer (128 bits)



#### Explicitly-sized boolean types

  * `_BOOL1` – boolean type with explicit size specification (1 byte)
  * `_BOOL2` – boolean type with explicit size specification (2 bytes)
  * `_BOOL4` – boolean type with explicit size specification (4 bytes)



Regardless of size, values of these types are treated in the same way: 0 is considered `false` and all other values `true`.

#### Unknown types

  * `_BYTE` – unknown type; the only known info is its size: 1 byte
  * `_WORD` – unknown type; the only known info is its size: 2 bytes
  * `_DWORD` – unknown type; the only known info is its size: 4 bytes
  * `_QWORD` – unknown type; the only known info is its size: 8 bytes
  * `_OWORD` – unknown type; the only known info is its size: 16 bytes
  * `_TBYTE` – 10-byte floating point (x87 extended precision 80-bit value)
  * `_UNKNOWN` – no info is available about type or size (usually only appears in pointers)



Please note that these types are **not** equivalent to the similarly-looking [Windows data types](<https://docs.microsoft.com/en-us/windows/win32/winprog/windows-data-types>) and may appear in non-Windows programs.

More info:[_Set function/item type_](<https://hex-rays.com/products/ida/support/idadoc/1361.shtml>) in IDA Help.

---

## 7. 

**Date:** Posted: Jun 18, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-44-hex-dump-loader](https://hex-rays.com/blog/igors-tip-of-the-week-44-hex-dump-loader)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #44: Hex dump loader

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-44-hex-dump-loader>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-44-hex-dump-loader>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-44-hex-dump-loader>)

I 

Igor Skochinsky ✦ Posted: Jun 18, 2021

![Igor’s tip of the week #44: Hex dump loader](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

IDA has a file loader named ‘hex’ which mainly supports loading of text-based file formats such as [Intel Hex](<https://en.wikipedia.org/wiki/Intel_HEX>) or [Motorola S-Record](<https://en.wikipedia.org/wiki/SREC_\(file_format\)>). These formats contain records with addresses and data in hexadecimal encoding.

For example, here’s a fragment of an Intel Hex file:
    
    
    :18000000008F9603008FD801008FDC01008FE001008FE401008FE80190
    :20004000008FEC01008FF001008FF401008FF801008FFC01008F0002008F0402008F08024D
    :20006000008F0C02008F1002008F1402008F1802008F1C02008F2002008F2402008F280228
    :14008000008F2C02008F3002008F3402008F3802008F3C0293
    :1000A000008F4002008F4402008F4802008F4C02F4
    :20010000008F5002008F5402008F5802008F5C02008F6002008F6402008F680243204C694C
    :20012000627261727920436F707972696768742028432920313939352048492D5445434818

or an S-Record
    
    
    S0030000FC
    S1230100810F0016490F0016816F8A0A0F00000098300016B2310016BC3300168E0D0016A7
    S1230108280F00169A2900168A00F001866000080400000018230016792200160C00000032
    S12301109800E00182A09E0B8000C2012A38001608000000EA3100163A380016FA310016CA
    S1230118FF250016BE21001600000000182200169A0100169C330016F9C010010D000000D7

However, you may also have a simple unformatted hex dump, with or without addresses:
    
    
    0020: 59 69 74 54 55 B6 3E F7 D6 B9 C9 B9 45 E6 A4 52
    1000: 12 23 34 56 78
    0100: 31 C7 1D AF 32 04 1E 32 05 1E 3C 32 07 1E 21 D9
    12 23 34 56 78

Such files are recognized and handled by another loader called ‘dump’. Since, like raw binaries, they do not carry information about the processor used, it has to be selected by the user.

For example, a hex dump of some MIPS code:
    
    
    007C5DBC 27 BD FF D0
    007C5DC0 FF B0 00 20
    007C5DC4 FF BF 00 28
    007C5DC8 0C 1F 17 64
    007C5DCC 00 80 80 2D
    007C5DD0 96 03 00 3E
    007C5DD4 DF BF 00 28
    007C5DD8 DF B0 00 20
    007C5DDC 00 62 18 26
    007C5DE0 2C 62 00 01
    007C5DE4 03 E0 00 08
    007C5DE8 27 BD 00 30

can be loaded into IDA without having to convert it to binary or a structured format like ELF.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexldr-3.png?width=723&height=452&name=hexldr-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexldr_ida-3.png?width=585&height=231&name=hexldr_ida-3.png)

This feature could be useful when working with shellcode or exchanging data with other software. As we described before, IDA also supports [exporting data from database](<https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data/>) as hexadecimal dump.

---

## 8. 

**Date:** Posted: Jun 11, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output](https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #43: Annotating the decompiler output

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-43-annotating-the-decompiler-output>)

I 

Igor Skochinsky ✦ Posted: Jun 11, 2021

![Igor’s tip of the week #43: Annotating the decompiler output](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

[Last week](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>) we started improving decompilation of a simple function. While you can go quite far with renaming and retyping, some things need more explanation than a simple renamng could provide.

### Comments

When you can’t come up with a good name for a variable or a function, you can add a comment with an explanation or a theory about what’s going on. The following comment types are available in the pseudocode:

  1. Regular end-of-line comments. Use `/` to add or edit them (easy to remember because in C++ `//` is used for comments).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-comment1-3.png?width=513&height=74&name=hr-comment1-3.png)
  2. Block comments. Similarly to [anterior comments](<https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida/>) in the disassembly view, the `Ins` shortcut is used (`I` on Mac). The comment is added before the current statement (not necessarily the current line).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-comment2-3.png?width=571&height=73&name=hr-comment2-3.png)
  3. Function comment is added when you use `/` on the first line of the function.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-comment3-3.png?width=550&height=152&name=hr-comment3-3.png)



Due to limitations of the [implementation](<https://hex-rays.com/blog/coordinate-system-for-hex-rays/>), the first two types can move around or even end up as orphan comments when the pseudocode changes. The function comment is attached to the function itself and is visible also in the disassembly view.

Using the comments, we can annotate the function[ from the previous post](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>) to clarify what is going on. On the screenshot below, regular comments are highlighted in blue while block comments are outlined in orange.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-comment4-3.png?width=841&height=329&name=hr-comment4-3.png)

In the end, the function seems to be copying bytes from a2 to a1, stopping at the first zero byte. If you know libc, you’ll quickly realize that it’s actually a trivial implementation of [`strcpy`](<https://en.cppreference.com/w/c/string/byte/strcpy>). We can now rename the function and arguments to the canonical names and add a function comment explaining the purpose of the function.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-comment5-3.png?width=841&height=384&name=hr-comment5-3.png)

Alas, the existing comments are not updated automatically, so references to `a1` and `a2` would have to be fixed manually.

### Empty lines

To improve the readability of pseudocode even further, you can add empty lines either manually or automatically. For manual lines, press `Enter` after or before a statement. For example, here’s the same function with extra empty lines added:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-comment6-3.png?width=828&height=359&name=hr-comment6-3.png)

To remove the manual empty lines, edit the anterior comment (`Ins` or `I` on Mac) and remove the empty lines from the comment.

To add automatic empty lines, set `GENERATE_EMPTY_LINES = YES` in `hexrays.cfg`. This will cause the decompiler to add empty lines between compound statements as well as before labels. This improves readability of long or complex functions. For example, here’s a decompilation of the same function with both settings. You can see that the second one reads easier thanks to extra spacing.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-empty1-3.png?width=697&height=534&name=hr-empty1-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-empty2-3.png?width=703&height=545&name=hr-empty2-3.png)

---

## 9. 

**Date:** Posted: Jun 4, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #42: Renaming and retyping in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Jun 4, 2021

![Igor’s tip of the week #42: Renaming and retyping in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Previously we’ve covered how to [start using the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics/>), but unmodified decompiler output is not always easy to read, especially if the binary doesn’t have symbols or debug information. However, with just a few small amendments you can improve the results substantially. Let’s look at some basic interactive operations available in the pseudocode view.

### Renaming

Although it sounds trivial, renaming can dramatically improve readability. Even something simple like renaming of `v3` to `counter` can bring immediate clarity to what’s going on in a function. Coupled with the auto-renaming feature [added in IDA 7.6](<https://hex-rays.com/products/ida/news/7_6/>), this can help you propagate nice names through pseudocode as you analyze it. The following items can be renamed directly in the pseudocode view:

  * local variables
  * function arguments
  * function names
  * global variables (data items)
  * structure members



Renaming is very simple: put the cursor on the item to rename and press `N` – the same shortcut as the one used in the disassembly listing. Of course, the command is also available in the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-rename-3.png?width=274&height=107&name=hr-rename-3.png)

You can also choose to do your renaming in the disassembly view instead of pseudocode. This can be useful if you plan to rename many items in a big function and don’t want to wait for decompilation to finish every time. Once you finished renaming, press `F5` to refresh the pseudocode and see all the new names. Note that register-allocated local variables cannot be renamed in the disassembly; they can only be managed in the pseudocode view.

### Retyping

Type recovery is one of the hardest problems in decompilation. Once the code is converted to machine instructions, there are no more types but just bits which are being shuffled around. There are some guesses the decompiler can make nevertheless, such as a size of the data being processed, and in some cases whether it’s being treated as a signed value or not, but in general the high-level type recovery remains a challenge in which a human brain can be of great help.

For example, consider this small ARM function:
    
    
    sub_4FF203A8
      SUB R2, R0, #1
    loc_4FF203AC
      LDRB R3, [R1],#1
      CMP R3, #0
      STRB R3, [R2,#1]!
      BNE loc_4FF203AC
      BX LR

Its initial decompilation looks like this:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-types1-3.png?width=419&height=211&name=hr-types1-3.png)

We see that the decompiler could guess the type of the second argument (`a2`, passed in `R1`) because it is used in the `LDRB` instruction (load byte). However, `v2` remains a simple `int` because the first operation done on it is a simple arithmetic `SUB` (subtraction). Now, after some thinking it is pretty obvious that both `v2` and `result` are also byte pointers and the subtraction is simply pointer math (since pointers are just numbers on the CPU level).

We can fix things by changing the type of both variables to the same `unsigned __int8 *` (or the equivalent `unsigned char *`). To do this, put cursor on the variable and press `Y`, or use “Set lvar type” from the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-types2-300x241-3.png?width=300&height=241&name=hr-types2-300x241-3.png)

Alternatively, instead of fixing the local variable and then the argument, you can directly edit the function prototype by using the shortcut on the function’s name in the first line.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-types3-3.png?width=478&height=219&name=hr-types3-3.png)

In that case, first argument’s type will be automatically propagated into the local variable and you won’t need to change it manually (user-provided types have priority over guessed ones).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr-types4-3.png?width=595&height=211&name=hr-types4-3.png)

In the final version there are no more casts and it’s clearer what’s happening. We’ll solve the mystery of the function’s purpose next week, stay tuned!

---

