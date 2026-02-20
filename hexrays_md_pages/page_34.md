# Hex-Rays Blog – Page 34

**Archived:** 2026-02-20 12:20
**URL:** https://hex-rays.com/blog/page/34

---

## 1. 

**Date:** Posted: Nov 13, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-15-comments-in-structures-and-enums](https://hex-rays.com/blog/igor-tip-of-the-week-15-comments-in-structures-and-enums)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #15: Comments in structures and enums

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-15-comments-in-structures-and-enums>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-15-comments-in-structures-and-enums>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-15-comments-in-structures-and-enums>)

I 

Igor Skochinsky ✦ Posted: Nov 13, 2020

![Igor’s tip of the week #15: Comments in structures and enums](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Last week we’ve discussed [various kinds of comments](</blog/igor-tip-of-the-week-14-comments-in-ida/>) in IDA’s disassembly and pseudocode views.

In fact, the comments are also available for Structures and Enums. You can add them both for the struct/enum as a whole and for individual members. Similar to the disassembly, regular and repeatable comments are supported.

Repeatable comments are duplicated in the listing when the enum or structure member is used.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_enum1-Jun-18-2024-08-59-03-8765-AM.png?width=528&height=416&name=comm_enum1-Jun-18-2024-08-59-03-8765-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_enum2-Jun-18-2024-08-59-05-4880-AM.png?width=478&height=169&name=comm_enum2-Jun-18-2024-08-59-05-4880-AM.png)

One interesting use of this is for C++ class vtables (or any struct with pointers): if you add the comment with the method’s address in the vtable structure, it will be printed in disassembly and you can double-click it to jump to the implementation or hover over it to see a hint window with disassembly.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_struct1-Jun-18-2024-08-59-04-8033-AM.png?width=432&height=60&name=comm_struct1-Jun-18-2024-08-59-04-8033-AM.png)  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_struct2-Jun-18-2024-08-59-03-1970-AM.png?width=526&height=293&name=comm_struct2-Jun-18-2024-08-59-03-1970-AM.png)

---

## 2. 

**Date:** Posted: Nov 6, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida](https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #14: Comments in IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida>)

I 

Igor Skochinsky ✦ Posted: Nov 6, 2020

![Igor’s tip of the week #14: Comments in IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

The “I” in IDA stands for _interactive_ , and one of the most common interactive actions you can perform is adding comments to the disassembly listing (or decompiler pseudocode). There are different types of comments you can add or see in IDA.

### Regular comments

These comments are placed at the end of the disassembly line, delimited by an assembler-specific comment character (semicolon, hash, at-sign etc.). A multi-line comment shifts the following listing lines down and is printed aligned with the first line which is why they can also be called _indented comments_.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_regular-Jun-18-2024-08-55-13-9659-AM.png?width=576&height=107&name=comm_regular-Jun-18-2024-08-55-13-9659-AM.png)

Shortcut: `:` (colon) 

### Repeatable comments

Basically equivalent to regular comments with one small distinction: they are _repeated_ in any location which refers to the original comment location. For example, if you add a repeatable comment to a global variable, it will be printed at any place the variable is referenced.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_repeatable-Jun-18-2024-08-55-07-8406-AM.png?width=572&height=104&name=comm_repeatable-Jun-18-2024-08-55-07-8406-AM.png)

Shortcut: `;` (semicolon)

### Function comments

A repeatable comment added at the first instruction of a function is considered a _function comment._ It is printed before the function header and — since it’s a repeatable comment — at any place the function is called from. They’re good for describing what the function does in more detail than can be inferred from the function’s name.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_function-Jun-18-2024-08-55-11-2819-AM.png?width=549&height=332&name=comm_function-Jun-18-2024-08-55-11-2819-AM.png)

Shortcut: `;` (semicolon)

### Anterior and posterior comments

These are printed before (_anterior_) or after (_posterior_) the current address as separate lines of text, shifting all other listing lines. They are suitable for extended explanations, ASCII art and other freestanding text. Unlike regular comments, no assembler comment characters are added automatically.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_anterior-Jun-18-2024-08-55-07-1372-AM.png?width=757&height=284&name=comm_anterior-Jun-18-2024-08-55-07-1372-AM.png)

Shortcuts: `Ins`, `Shift`–`Ins` (`I` and `Shift`–`I` on Mac)

Trivia: the comment with file details that is usually added at the beginning of the listing is an anterior comment so you can use `Ins` to edit it.

### Pseudocode comments

In the decompiler pseudocode you can also add [_indented_](<https://www.hex-rays.com/products/decompiler/manual/cmd_comments.shtml>) comments using the shortcut `/` (slash) and [_block_](<https://www.hex-rays.com/products/decompiler/manual/cmd_block_cmts.shtml>) comments using `Ins` (`I` on Mac). They are stored separately from the disassembly comments, however _function comments_ are shared with those in disassembly.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_pseudo-Jun-18-2024-08-55-13-2867-AM.png?width=555&height=183&name=comm_pseudo-Jun-18-2024-08-55-13-2867-AM.png)

### Automatic comments

In some situations IDA itself can add comments to disassembly. A few examples:

“Auto comments” in Option > General.., Disassembly tab enables **instruction comments**.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_auto1-Jun-18-2024-08-55-14-6795-AM.png?width=584&height=488&name=comm_auto1-Jun-18-2024-08-55-14-6795-AM.png)  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_auto2-Jun-18-2024-08-55-08-7960-AM.png?width=943&height=373&name=comm_auto2-Jun-18-2024-08-55-08-7960-AM.png)

**Demangled names** are shown as auto comments by default. Use the Options > Demangled names… dialog if you prefer to replace the mangled symbol directly in the listing.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_auto4-1-Jun-18-2024-08-55-11-6220-AM.png?width=596&height=308&name=comm_auto4-1-Jun-18-2024-08-55-11-6220-AM.png)

**String literals** work similarly to repeatable comments: the string contents shows up as a comment in the place it’s referenced from.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comm_auto4-Jun-18-2024-08-55-13-6591-AM.png?width=446&height=104&name=comm_auto4-Jun-18-2024-08-55-13-6591-AM.png)

---

## 3. 

**Date:** Posted: Nov 2, 2020  
**Link:** [https://hex-rays.com/blog/updates-to-xnu-debugging-tutorial](https://hex-rays.com/blog/updates-to-xnu-debugging-tutorial)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Mac](<https://hex-rays.com/blog/tag/mac>) [debugger](<https://hex-rays.com/blog/tag/debugger>) [xnu](<https://hex-rays.com/blog/tag/xnu>)

# Updates to XNU debugging tutorial

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/updates-to-xnu-debugging-tutorial>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/updates-to-xnu-debugging-tutorial>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/updates-to-xnu-debugging-tutorial>)

Troy Bowman ✦ Posted: Nov 2, 2020

![Updates to XNU debugging tutorial](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Along with the release of [ Service Pack 3](<https://www.hex-rays.com/blog/ida-pro-7-5-sp3-released/>) for IDA 7.5, we have updated our [XNU Debugging Tutorial](<https://www.hex-rays.com/wp-content/static/tutorials/xnu_debugger_primer/xnu_debugger_primer.html>) with a new section about [macOS11 kernel debugging](<https://www.hex-rays.com/wp-content/static/tutorials/xnu_debugger_primer/xnu_debugger_primer.html#macos11-kernel-debugging>).

It has some analysis and debugging tips for the new kernelcache format in macOS11 Big Sur. We hope you will find it useful!

Downloads

[XNU Debugging Tutorial __](</wp-content/static/tutorials/xnu_debugger_primer/xnu_debugger_primer.pdf>)

---

## 4. 

**Date:** Posted: Oct 30, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings](https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #13: String literals and custom encodings

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings>)

I 

Igor Skochinsky ✦ Posted: Oct 30, 2020

![Igor’s tip of the week #13: String literals and custom encodings](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Most of IDA users probably analyze software that uses English or another Latin-based alphabet. Thus the defaults used for string literals – the OS system encoding on Windows and UTF-8 on Linux or macOS – are usually good enough. However, occasionally you may encounter a program which does use another language.

### Unicode strings

In case the program uses _wide strings_ , it is usually enough to use the corresponding “Unicode C-style” option when creating a string literal:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_wide1-Jun-18-2024-09-06-31-6641-AM.png?width=525&height=343&name=strlit_wide1-Jun-18-2024-09-06-31-6641-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_wide2-Jun-18-2024-09-06-23-5834-AM.png?width=486&height=30&name=strlit_wide2-Jun-18-2024-09-06-23-5834-AM.png)

In general, Windows programs tend to use 16-bit wide strings (`wchar_t` is 16-bit) while Linux and Mac use 32-bit ones (`wchar_t` is 32-bit). That said, exceptions happen and you can use either one depending on a specific binary you’re analyzing.

Hint: you can use accelerators to quickly create specific string types, for example `Alt`–`A`, `U` for Unicode 16-bits.

### Custom encodings

There may be situations when the binary being analyzed uses an encoding different from the one picked by IDA, or even multiple mutually incompatible encodings in the same file. In that case you can set the encoding separately for individual string literals, or globally for all new strings.

##### Add a new encoding

To add a custom encoding to the default list (usually UTF-8, UTF-16LE and UTF-32LE):

  1. Options > String literals… (`Alt`–`A`);
  2. Click the button next to “Currently:”;
  3. In context menu, “Insert…” (Ins);
  4. Specify the encoding name.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_enc1-Jun-18-2024-09-06-31-0082-AM.png?width=295&height=321&name=strlit_enc1-Jun-18-2024-09-06-31-0082-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_enc2-Jun-18-2024-09-06-27-9086-AM.png?width=606&height=221&name=strlit_enc2-Jun-18-2024-09-06-27-9086-AM.png)

For the encoding name you can use: 

  * Windows codepages (e.g. 866, CP932, windows-1251)
  * Well-known charset names (e.g. Shift-JIS, UTF-8, Big5)



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_enc3-Jun-18-2024-09-06-32-6623-AM.png?width=430&height=283&name=strlit_enc3-Jun-18-2024-09-06-32-6623-AM.png)

On Linux or macOS, run `iconv -l` to see the available encodings.

Note: some encodings are not supported on all systems so your IDB may become system-specific.

##### Use the encoding for a specific string literal

  1. Invoke Options > String literals… (`Alt`–`A`);
  2. Click the button next to “Currently:”;
  3. Select the encoding to use;
  4. Click the specific string button (e.g. C-Style) if creating a new literal or just OK if modifying an existing one.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_enc4-Jun-18-2024-09-06-26-2391-AM.png?width=440&height=342&name=strlit_enc4-Jun-18-2024-09-06-26-2391-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_enc5-Jun-18-2024-09-06-25-3404-AM.png?width=710&height=77&name=strlit_enc5-Jun-18-2024-09-06-25-3404-AM.png)

##### Set an encoding as default for all new string literals

  1. Invoke Options > String literals… (`Alt`–`A`);
  2. Click “Manage defaults”;
  3. Click the button next to “Default 8-bit” and select the encoding to use.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_def1-Jun-18-2024-09-06-25-6333-AM.png?width=295&height=321&name=strlit_def1-Jun-18-2024-09-06-25-6333-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_def2-Jun-18-2024-09-06-26-6465-AM.png?width=584&height=488&name=strlit_def2-Jun-18-2024-09-06-26-6465-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strlit_def3-Jun-18-2024-09-06-32-3259-AM.png?width=584&height=488&name=strlit_def3-Jun-18-2024-09-06-32-3259-AM.png)

From now on, the `A` shortcut will create string literals with the new default encoding, but you can still override it on a case-by-case basis, as described above.

---

## 5. 

**Date:** Posted: Oct 27, 2020  
**Link:** [https://hex-rays.com/blog/ida-pro-7-5-sp3-released](https://hex-rays.com/blog/ida-pro-7-5-sp3-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 7.5 SP3 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-7-5-sp3-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-7-5-sp3-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-7-5-sp3-released>)

Tra Tran ✦ Posted: Oct 27, 2020

![IDA Pro 7.5 SP3 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Hex-Rays announces the release of Service Pack 3 (SP3) for IDA Pro 7.5.

It is glad to announce the release of the Service Pack 3 today. The release introduces a handful of new and interesting features specific to the soon-to-be-released macOS 11 (Big Sur) and provides fixes for numerous errors in IDA.

**We improved:**

  * macOS11 kernel debugging with VMware Fusion 12;
  * symbolication of MH_FILESET kernelcaches.



**Fixes**

  * this update fixed various minor issues related to several potential crash situations as well as minor errors in IDA 7.5.



### How to request the new versions

As usual, the new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to support@hex-rays.com.

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist __](</products/ida/news/7_5sp3/>)[Get a quote __](</cgi-bin/quote.cgi>)

---

## 6. 

**Date:** Posted: Oct 23, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-12-creating-structures-with-known-size](https://hex-rays.com/blog/igor-tip-of-the-week-12-creating-structures-with-known-size)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #12: Creating structures with known size

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-12-creating-structures-with-known-size>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-12-creating-structures-with-known-size>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-12-creating-structures-with-known-size>)

I 

Igor Skochinsky ✦ Posted: Oct 23, 2020

![Igor’s tip of the week #12: Creating structures with known size](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Sometimes you know the structure size but not the actual layout yet. For example, when the size of memory being allocated for the structure is fixed:

### ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize1-Jun-18-2024-08-39-04-4576-AM.png?width=836&height=484&name=strsize1-Jun-18-2024-08-39-04-4576-AM.png)

In such cases, you can quickly make a dummy structure and then modify it as you analyze code which works with it. There are several approaches which can be used here.

### Fixed-size structure 1: single array

This is the fastest option but makes struct modification a little awkward.

  1. create the struct (go to Structures view, press `Ins` and specify a name);
  2. create the array (position cursor at the start of the struct, press `*` and enter the size (decimal or hex)



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize2-e1603441294648-Jun-18-2024-08-39-13-5935-AM.png?width=561&height=470&name=strsize2-e1603441294648-Jun-18-2024-08-39-13-5935-AM.png) ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize3-Jun-18-2024-08-39-14-3209-AM.png?width=581&height=43&name=strsize3-Jun-18-2024-08-39-14-3209-AM.png)

When you need to create a field in the middle, press `*` to resize the array so it ends before the field, create the field, then create another array after it to pad the struct to the full size again.

### Fixed-size structure 2: big gap in the middle

  1. create the struct (go to Structures view, press `Ins` and specify a name);
  2. create a byte field (press `D`);
  3. add a gap (`Ctrl`–`E` or “Expand struct type..” in context menu) and enter the size minus 1;
  4. (optional but recommended) On `field_0` which is now at the end of the struct, press `N`, `Del`, `Enter`. This will reset the name to match the actual offset and will not hinder creation of another `field_0` at offset 0 if needed.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize4-Jun-18-2024-08-39-07-4120-AM.png?width=569&height=157&name=strsize4-Jun-18-2024-08-39-07-4120-AM.png)![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize5-Jun-18-2024-08-39-14-9454-AM.png?width=570&height=241&name=strsize5-Jun-18-2024-08-39-14-9454-AM.png)

To create fields in the middle of the gap, go to the specific offset in the struct (`G` can be used for big structs).

### Fixed-size structure 3: fill with dummy fields

  1. create the struct (go to Structures view, press `Ins` and specify a name);
  2. create one dummy field (e.g. a dword);
  3. press `*` and enter the size (divided by the field size if different from byte). Uncheck “Create as array” and click OK.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize6-Jun-18-2024-08-39-06-5754-AM.png?width=562&height=462&name=strsize6-Jun-18-2024-08-39-06-5754-AM.png) ![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize7-e1603443371279-Jun-18-2024-08-39-08-5373-AM.png?width=564&height=193&name=strsize7-e1603443371279-Jun-18-2024-08-39-08-5373-AM.png)

### Automatically create fields from code

Using a structure with a gap in the middle (option 2 above) is especially useful when analyzing functions that work with it using a fixed register base. For example, this function uses `rbx` as the base for the structure:
    
    
    ATI6000Controller::initializeProjectDependentResources(void) proc near
      push    rbp
      mov     rbp, rsp
      push    rbx
      sub     rsp, 8
      mov     rbx, rdi
      lea     rax, `vtable for'NI40SharedController
      mov     rdi, rbx        ; this
      call    qword ptr [rax+0C30h]
      test    eax, eax
      jnz     loc_25CD
      mov     rax, [rbx+168h]
      mov     [rbx+4B8h], rax
      mov     rax, [rbx+178h]
      mov     [rbx+4C0h], rax
      mov     rax, [rbx+150h]
      mov     [rbx+4C8h], rax
      mov     [rbx+4B0h], rbx
      mov     rax, [rbx+448h]
      mov     [rbx+4D0h], rax
      mov     rcx, [rbx+170h]
      mov     [rbx+4D8h], rcx
      mov     rcx, [rax]
      mov     [rbx+4E0h], rcx
      mov     eax, [rax+8]
      mov     [rbx+4E8h], rax
      call    NI40PowerPlayManager::createPowerPlayManager(void)
      mov     [rbx+450h], rax
      test    rax, rax
      jnz     short loc_2585
      mov     eax, 0E00002BDh
      jmp     short loc_25CD
    ---------------------------------------------------------------
    
    loc_2585:
      mov     rcx, [rax]
      lea     rsi, [rbx+4B0h]
      ...
    

To automatically create fields for all rbx-based accesses:

  1. select all instructions using `rbx`;
  2. from context menu, choose “Structure offset” (or press `T`);
  3. in the dialog, make sure Register is set to `rbx`, select the created struct (a red cross simply means that it has no fields at the matching offsets _currently_);
  4. from the right pane’s context menu, choose “Add missing fields”.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize8-Jun-18-2024-08-39-11-5310-AM.png?width=784&height=739&name=strsize8-Jun-18-2024-08-39-11-5310-AM.png)![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strsize9-Jun-18-2024-08-39-04-1637-AM.png?width=778&height=205&name=strsize9-Jun-18-2024-08-39-04-1637-AM.png)

You can then repeat this for all other functions working with the structure to create other missing fields.

---

## 7. 

**Date:** Posted: Oct 16, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures](https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #11: Quickly creating structures

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures>)

I 

Igor Skochinsky ✦ Posted: Oct 16, 2020

![Igor’s tip of the week #11: Quickly creating structures](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

When reverse engineering a big program, you often run into information stored in structures. The standard way of doing it involves using the Structures window and adding fields one by one, similar to the way you format data items in disassembly. But are there other options? Let’s look at some of them.

### Using already formatted data

This was mentioned briefly in the[ post on selection](</blog/igor-tip-of-the-week-03-selection-in-ida/>) but is worth repeating. If you happen to have some formatted data in your disassembly and want to group it into a structure, just select it and choose “Create struct from selection” in the context menu.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/sel_struct1-Jun-18-2024-09-09-36-8748-AM.png)

### Using Local Types

The Local Types view shows the _high level_ or _C level_ types used in the database such as structs, enums and typedefs. It is most useful with the decompiler but can still be used for the _a_ _ssembler level_ types such as Structures and Enums. For example, open the Local Types (`Shift`–`F1` or View > Open subviews > Local Types), then press `Ins` (or pick Insert.. from the context menu). In the new dialog enter a C syntax structure definition and click OK.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ltype_add-Jun-18-2024-09-09-36-4577-AM.png?width=462&height=352&name=ltype_add-Jun-18-2024-09-09-36-4577-AM.png)

The structure appears in the list but cannot yet be used in disassembly. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ltype_list-Jun-18-2024-09-09-41-0600-AM.png?width=713&height=153&name=ltype_list-Jun-18-2024-09-09-41-0600-AM.png)

To make it available, double-click it and answer “Yes”.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ltype_sync-Jun-18-2024-09-09-39-5466-AM.png?width=468&height=124&name=ltype_sync-Jun-18-2024-09-09-39-5466-AM.png)

Now that a corresponding assembler level type has been created in the Structures view, it can be used in the disassembly.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structs1-Jun-18-2024-09-09-35-8256-AM.png?width=711&height=166&name=structs1-Jun-18-2024-09-09-35-8256-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structs2-Jun-18-2024-09-09-40-1186-AM.png?width=453&height=261&name=structs2-Jun-18-2024-09-09-40-1186-AM.png)

For more info about using Local Types and two kinds of types check [this IDA Help topic](<https://www.hex-rays.com/products/ida/support/idadoc/1042.shtml>).

---

## 8. 

**Date:** Posted: Oct 9, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays](https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #10: Working with arrays

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays>)

I 

Igor Skochinsky ✦ Posted: Oct 9, 2020

![Igor’s tip of the week #10: Working with arrays](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Arrays are used in IDA to represent a sequence of multiple items of the same type: basic types (byte, word, dword etc.) or complex ones (e.g. structures).

### Creating an array

To create an array:

  1. Create the first item;
  2. Choose “Array…” from the context menu , or press `*`;
  3. Fill in at least the _Array size_ field and click OK.



Step 1 is optional; if no data item exists at the current location, a byte array will be created.

Hint: if you select a range before pressing `*`, _Array size_ will be pre-filled with the number of items which fits into the selected range.

### Array parameters

Array parameters affect how the array is displayed in the listing and can be set at the time the array is first created or any time later by pressing `*`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/array_params-Jun-18-2024-08-59-44-0060-AM.png?width=395&height=442&name=array_params-Jun-18-2024-08-59-44-0060-AM.png)

  * Array size: total number of elements in the array;
  * Items on a line: how many items (at most) to print on one line. 0 means to print the maximum number which fits into the disassembly line;
  * Element print width: how many characters to use for each element. Together with the previous parameter can be used for formatting arrays into nice-looking tables. For example:  
8 items per line, print width **-1** :   

        
        db 1, 2, 3, 4, 5, 6, 7, 8
        db 9, 10, 11, 12, 13, 14, 15, 16
        db 17, 18, 19, 20, 21, 22, 23, 24
        db 25, 255, 255, 255, 255, 255, 255, 26
        db 27, 28, 29, 30, 31, 32, 33, 34
        db 35, 36, 37, 38, 39, 40, 41, 42
        

print width **0** :  

        
        db   1,  2,  3,  4,  5,  6,  7,  8
        db   9, 10, 11, 12, 13, 14, 15, 16
        db  17, 18, 19, 20, 21, 22, 23, 24
        db  25,255,255,255,255,255,255, 26
        db  27, 28, 29, 30, 31, 32, 33, 34
        db  35, 36, 37, 38, 39, 40, 41, 42
        

print width **5** :  

        
        db     1,    2,    3,    4,    5,    6,    7,    8
        db     9,   10,   11,   12,   13,   14,   15,   16
        db    17,   18,   19,   20,   21,   22,   23,   24
        db    25,  255,  255,  255,  255,  255,  255,   26
        db    27,   28,   29,   30,   31,   32,   33,   34
        db    35,   36,   37,   38,   39,   40,   41,   42
        

  * Use “dup” construct: for assemblers that support it, repeated items with the same value will be collapsed into a dup expression instead of printing each item separately;  
dup off: `db 0FFh, 0FFh, 0FFh, 0FFh, 0FFh, 0FFh`  
dup on: `db 6 dup(0FFh)`
  * Signed elements: integer items will be treated as signed numbers;
  * Display indexes: for each line, first item’s array index will be printed in a comment. 
  * Create as array: if unchecked, IDA will convert the array into separate items.



### Creating multiple string literals

The last option in array parameters dialog can be useful when dealing with multiple string literals packed together. For example, if we have a string table like this:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strarray1-e1602241530916-Jun-18-2024-08-59-44-3760-AM.png?width=739&height=360&name=strarray1-e1602241530916-Jun-18-2024-08-59-44-3760-AM.png)

First, create one string.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strarray2-e1602241558294-Jun-18-2024-08-59-38-0772-AM.png?width=739&height=360&name=strarray2-e1602241558294-Jun-18-2024-08-59-38-0772-AM.png)

Then, select it and all the following strings using one of the methods [described before](<https://www.hex-rays.com/blog/igor-tip-of-the-week-04-more-selection/>).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strarray3-e1602241577448-Jun-18-2024-08-59-41-0642-AM.png?width=739&height=360&name=strarray3-e1602241577448-Jun-18-2024-08-59-41-0642-AM.png)

Invoke Edit > Array… or press `*`. The array size will be set to the total length of the selection. In the dialog, **uncheck** “Create as array”. Click OK.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strarray4-268x300-3.png?width=268&height=300&name=strarray4-268x300-3.png)

We get a nicely formatted string table!

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strarray5-e1602241689700-Jun-18-2024-08-59-40-3677-AM.png?width=739&height=360&name=strarray5-e1602241689700-Jun-18-2024-08-59-40-3677-AM.png)

This approach works also with Unicode (UTF-16) strings.

---

## 9. 

**Date:** Posted: Oct 2, 2020  
**Link:** [https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis](https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #09: Reanalysis

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis>)

I 

Igor Skochinsky ✦ Posted: Oct 2, 2020

![Igor’s tip of the week #09: Reanalysis](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

While working in IDA, sometimes you may need to reanalyze some parts of your database, for example:

  * after changing a prototype of an external function (especially calling convention, number of purged bytes, or “Does not return” flag);
  * after fixing up incorrectly detected ARM/Thumb or MIPS32/MIPS16 regions;
  * after changing global processor options (_e.g._ setting `$gp` value in MIPS or TOC in PPC);
  * other situations (analyzing switches, etc.)



### Reanalyzing individual instructions

To reanalyze an instruction, position the cursor in it and press `C` (convert to code). Even if the instruction is already code, this action is not a no-op: it asks the IDA kernel to:

  1. delete cross-references from the current address;
  2. have the processor module reanalyze the instruction; normally this should result in (re-)creation of cross-references, including the flow cross-reference to the following instruction (unless the current instruction stops the code flow).



### Reanalyzing a function

All of the function’s instructions are reanalyzed when any of the function’s parameters are changed (_e.g._. in case stack variables need to be recreated). So, the following key sequence causes the whole function to be reanalyzed: `Alt-P` (Edit function), `Enter` (confirm dialog).

### Reanalyzing a bigger range of instructions

For this we can use the trick covered in the [post on selection](</blog/igor-tip-of-the-week-04-more-selection/>).

  1. go to start of the range;
  2. press `Alt-L` (start selection);
  3. go to the end of selection;
  4. press `C` (convert to code). Pick “Analyze” in the first prompt and “No” in the second. 

![Analyze 1](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/analyze1-3.png?width=470&height=124&name=analyze1-3.png)

![Analyze 2](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/analyze2-3.png?width=369&height=137&name=analyze2-3.png)




### Reanalyzing whole database

If you need to reanalyze everything but don’t want to go through the hassle of selecting all the code, there is a dedicated command which can be invoked in two ways:

  1. Menu Options > General…, Analysis Tab, Reanalyze program button; 

![manu reanalyze](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/reanalyze1-3.png?width=584&height=488&name=reanalyze1-3.png)

  2. Right-click the status bar at the bottom of IDA’s window, Reanalyze program 

![Status bar reanalyze](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/reanalyze2-1-3.png?width=221&height=86&name=reanalyze2-1-3.png)

---

