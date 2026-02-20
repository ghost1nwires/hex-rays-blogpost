# Hex-Rays Blog – Page 28

**Archived:** 2026-02-20 12:18
**URL:** https://hex-rays.com/blog/page/28

---

## 1. 

**Date:** Posted: Sep 24, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-58-keyboard-modifiers](https://hex-rays.com/blog/igors-tip-of-the-week-58-keyboard-modifiers)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #58: Keyboard modifiers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-58-keyboard-modifiers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-58-keyboard-modifiers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-58-keyboard-modifiers>)

I 

Igor Skochinsky ✦ Posted: Sep 24, 2021

![Igor’s tip of the week #58: Keyboard modifiers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Today we’ll cover how keyboard modifiers (`Ctr`, `Alt`, `Shift`) can be used with some IDA actions to modify their behavior or provide additional functionality.

### Modifiers in shortcuts

Obviously, some shortcuts already include modifiers as part of their key sequence, but some commonalities may be not immediately obvious. For example, the Search menu commands tend to use `Alt`-letter to start search and corresponding `Ctrl`-letter to continue the search:

search type | start search | next (continue) search  
---|---|---  
Binary (bytes) search | `Alt`–`B` | `Ctrl`–`B`  
Text search | `Alt`–`T` | `Ctrl`–`T`  
Immediate search | `Alt`–`I` | `Ctrl`–`I`  
  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/modif_search-3.png?width=255&height=389&name=modif_search-3.png)

A somewhat similar situation exists with data formatting shortcuts: same as `D` defines byte/word/dword items and `Alt`–`D` is used for extra item types and configuration, `A` creates a default string literal type while `Alt`–`A` handles additional ones and configuration.

### Modifiers and the mouse

In some situations modifiers also change how mouse operations are interpreted:

  * In the text IDA View, holding `Ctrl` while double-clicking a label or address opens the target in a new tab.
  * Holding `Ctrl` or `Shift` while using the mouse wheel scrolls text views by page (like `PgDn`/`PgUp`). This also resizes [hint popups](<https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida/>) faster.
  * In the [graph view](<https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view/>), `Ctrl`+wheel zooms the graph, while `Alt`+wheel scrolls horizontally (you can also use two-finger panning on trackpads).
  * `Ctrl`+wheel also zooms in the [navigation band](<https://hex-rays.com/blog/igors-tip-of-the-week-49-navigation-band/>).



### Miscellaneous

  * if one of the recent file entries in the File menu is selected while `Shift` key is held down, the file is opened in a new IDA instance.
  * Windows only: if `Shift` key is held down while clicking the close window (X) corner button, IDA closes without confirmation with default exit options (save the database). If `Ctrl` is also held down, IDA exits without saving.

---

## 2. 

**Date:** Posted: Sep 17, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-57-shifted-pointers-2](https://hex-rays.com/blog/igors-tip-of-the-week-57-shifted-pointers-2)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #57: Shifted pointers 2

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-57-shifted-pointers-2>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-57-shifted-pointers-2>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-57-shifted-pointers-2>)

I 

Igor Skochinsky ✦ Posted: Sep 17, 2021

![Igor’s tip of the week #57: Shifted pointers 2](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

This week we’ll cover another situation where [shifted pointers](<https://hex-rays.com/blog/igors-tip-of-the-week-54-shifted-pointers/>) can be useful.

### Intrusive linked lists

This approach is used in many linked list implementations. Let’s consider the one used in the Linux kernel. `list.h` defines the linked list structure:
    
    
    struct list_head { 
      struct list_head *next, *prev; 
      };

As an example of its use, consider the struct `module` from `module.h`:
    
    
    struct module {
    	enum module_state state;
    
    	/* Member of list of modules */
    	struct list_head list;
    
    	/* Unique handle for this module */
    	char name[MODULE_NAME_LEN];
    	
    [..skipped..]
    } ____cacheline_aligned __randomize_layout;
    

Where `struct list_head list;`is used to link the instances of `struct module`together. Because the `next` and `prev` pointers do not point to the start of `struct module`, some pointer math is required to access its fields. For this, various macros in list.h are used:
    
    
    /**
     * list_entry - get the struct for this entry
     * @ptr:	the &struct list_head pointer.
     * @type:	the type of the struct this is embedded in.
     * @member:	the name of the list_head within the struct.
     */
    #define [list_entry](<https://elixir.bootlin.com/linux/latest/C/ident/list_entry>)(ptr, type, [member](<https://elixir.bootlin.com/linux/latest/C/ident/member>)) \
    	[container_of](<https://elixir.bootlin.com/linux/latest/C/ident/container_of>)(ptr, type, [member](<https://elixir.bootlin.com/linux/latest/C/ident/member>))
    
    
    /**
     * list_first_entry - get the first element from a list
     * @ptr:	the list head to take the element from.
     * @type:	the type of the struct this is embedded in.
     * @member:	the name of the list_head within the struct.
     *
     * Note, that list is expected to be not empty.
     */
    #define [list_first_entry](<https://elixir.bootlin.com/linux/latest/C/ident/list_first_entry>)(ptr, type, [member](<https://elixir.bootlin.com/linux/latest/C/ident/member>)) \
    	[list_entry](<https://elixir.bootlin.com/linux/latest/C/ident/list_entry>)((ptr)->next, type, [member](<https://elixir.bootlin.com/linux/latest/C/ident/member>))
    /**
     * list_last_entry - get the last element from a list
     * @ptr:	the list head to take the element from.
     * @type:	the type of the struct this is embedded in.
     * @member:	the name of the list_head within the struct.
     *
     * Note, that list is expected to be not empty.
     */
    #define [list_last_entry](<https://elixir.bootlin.com/linux/latest/C/ident/list_last_entry>)(ptr, type, [member](<https://elixir.bootlin.com/linux/latest/C/ident/member>)) \
    	[list_entry](<https://elixir.bootlin.com/linux/latest/C/ident/list_entry>)((ptr)->[prev](<https://elixir.bootlin.com/linux/latest/C/ident/prev>), type, [member](<https://elixir.bootlin.com/linux/latest/C/ident/member>))
    
    

Let’s look at some functions from `module.c`. For example, `m_show()`:
    
    
    static int m_show(struct seq_file *m, void *p)
    {
    	struct module *mod = list_entry(p, struct module, list);
    	char buf[MODULE_FLAGS_BUF_SIZE];
    	void *value;
    
    	/* We always ignore unformed modules. */
    	if (mod->state == MODULE_STATE_UNFORMED)
    		return 0;
    
    	seq_printf(m, "%s %u",
    		   mod->name, mod->init_layout.size + mod->core_layout.size);
    	print_unload_info(m, mod);
    
    	/* Informative for users. */
    	seq_printf(m, " %s",
    		   mod->state == MODULE_STATE_GOING ? "Unloading" :
    		   mod->state == MODULE_STATE_COMING ? "Loading" :
    		   "Live");
    	/* Used by oprofile and other similar tools. */
    	value = m->private ? NULL : mod->core_layout.base;
    	seq_printf(m, " 0x%px", value);
    
    	/* Taints info */
    	if (mod->taints)
    		seq_printf(m, " %s", module_flags(mod, buf));
    
    	seq_puts(m, "\n");
    	return 0;
    }
    

Although the function accepts a `void * p`, from the code we can see that it actually points to the list entry for the module at offset 8.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/shiftted_module0-3.png?width=388&height=229&name=shiftted_module0-3.png)

The initial decompilation looks like follows:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/shifted_module1-3.png?width=739&height=456&name=shifted_module1-3.png)

Not very readable, is it? But since we know that `p` actually points to `list` inside `struct module`, we can use a shifted pointer instead:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/shifted_module2-3.png?width=678&height=456&name=shifted_module2-3.png)

This is already much better. The ugly expression with the `next` variable is caused by the fact that `source_list` actually stores instances of `struct module_use` so by changing the variable’s type we can improve the output again:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/shifted_module3-3.png?width=535&height=229&name=shifted_module3-3.png)

### Creating shifted pointers for structures

Although shifted pointers are not limited to structure members, it is the most common use case, and thus we implemented a UI feature to make their creation easier.

In the decompiler, untyped variables and void pointers have a context menu item “Convert to struct *…”. When invoked, the dialog shows a list of structures (and unions) available in the local type library so you can easily create a pointer to it without typing manually. But in addition to simple struct pointers, you can create a shifted pointer by entering a non-zero delta value in the “Pointer shift value” field.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/shiftted_strucptr1-3.png?width=555&height=303&name=shiftted_strucptr1-3.png)  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/shiftted_strucptr2-1024x665-3.png?width=1024&height=665&name=shiftted_strucptr2-1024x665-3.png)![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/shiftted_strucptr3-3.png?width=634&height=178&name=shiftted_strucptr3-3.png)   
Because the original pointer had type `void *`, the shifted pointer retained it, so you may need to change the final type to get proper decompilation (in our example, `struct list_head *__shifted(module,8) p`).

If you want to practice this, here’s the 7.6 IDB with the function described: [vmlinux_trimmed.elf.i64](<https://hex-rays.com/wp-content/uploads/2021/09/vmlinux_trimmed.elf_.i64.zip>). To save space, it’s been trimmed to only include the function in question and its direct dependencies. To get the full kernel with symbols, see the post on [DWARF info loading](<https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols/>).

---

## 3. 

**Date:** Posted: Sep 10, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-56-string-literals-in-pseudocode](https://hex-rays.com/blog/igors-tip-of-the-week-56-string-literals-in-pseudocode)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #56: String literals in pseudocode

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-56-string-literals-in-pseudocode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-56-string-literals-in-pseudocode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-56-string-literals-in-pseudocode>)

I 

Igor Skochinsky ✦ Posted: Sep 10, 2021

![Igor’s tip of the week #56: String literals in pseudocode](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Strings in binaries are very useful for the reverse engineer: they often contain messages shown to the user, or sometimes even internal debugging information (function or variable names) and so having them displayed in the decompiled code is very helpful.

However, sometimes you may see named variables in pseudocode even though the disassembly shows the string nicely. Why does this happen and how to fix it?

### Memory access permissions

When deciding whether to display a string literal inline, the main criteria are attributes of the memory area it resides in. If the memory is writable, it means that the string is not really constant but may change, so displaying a variable name is more correct. For example, here’s the default pseudocode of a function from a decompressed Linux kernel:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_strlit1-1024x379-3.png?width=1024&height=379&name=hr_strlit1-1024x379-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/hr_strlit1-3.png>)

We can see a string literal is displayed as a variable name (`aApicIcrReadRet`) even though it is a nice-looking string in the disassembly. The mystery can be cleared up if we jump to its definition (e.g. by double-clicking) and inspect the segment properties (Edit > Segment > Edit Segment…, or `Alt`–`S`). We can see that the segment is marked as writable:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_strlit2-3.png?width=512&height=374&name=hr_strlit2-3.png)

Why does `.rodata` (“read-only data”) have write permissions? We can’t say for sure, but the section does include this flag in the ELF headers:

(`readelf` output)
    
    
    Section Headers:
      [Nr] Name              Type             Address           Offset
           Size              EntSize          Flags  Link  Info  Align
      [ 0]                   NULL             0000000000000000  00000940
           0000000000000000  0000000000000000           0     0     0
      [ 1] .text             PROGBITS         ffffffff81000000  00001000
           0000000000628281  0000000000000000  AX       0     0     4096
      [ 2] .notes            NOTE             ffffffff81628284  00629284
           0000000000000204  0000000000000000  AX       0     0     4
      [ 3] __ex_table        PROGBITS         ffffffff81628490  00629488
           0000000000002cdc  0000000000000000   A       0     0     4
      [ 4] .rodata           PROGBITS         ffffffff81800000  0062d000
           0000000000275332  0000000000000000  WA       0     0     4096
    
    <...skipped...>
    
    Key to Flags:
      W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
      L (link order), O (extra OS processing required), G (group), T (TLS),
      C (compressed), x (unknown), o (OS specific), E (exclude),
      l (large), p (processor specific)
    

One possibility is that it is made actually read-only later in the boot process.

So one solution for our problem is to make sure that the segment has only Read (and possibly Execute) permissions but not Write. If you do that, the string literals from that segment will be displayed inline:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_strlit3-3.png?width=693&height=495&name=hr_strlit3-3.png)

### Override access permissions

While changing segment attributes works, it may not be suitable for all cases. For example, some compilers can put string constants in the same section as other writable data, so if you change the segment permissions to read-only, the decompiler could produce wrong output for functions using the writable variables. You may also have an opposite situation: a string constant is not actually constant but simply has a default value, so it needs to be marked as variable. In such cases, you can override the attributes of each string variable using `const` or `volatile` type attributes. For example, instead of changing the whole segment’s permission, you could edit the type of the `aApicIcrReadRet` variable by pressing `Y` (change type) and changing its type to `const char aApicIcrReadRet[]`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_strlit4-3.png?width=694&height=99&name=hr_strlit4-3.png)

With this option, only the edited strings literals will be shown inline and others remain as variables.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_strlit5-3.png?width=619&height=218&name=hr_strlit5-3.png)

### Show all string literals

Yet another possibility is to rely on IDA’s analysis of disassembly and show all strings marked as string literals on the disassembly level. This can be done in the decompiler options ( Edit > Plugins > Hex-Rays Decompiler, Options, Analysis Options 1) by turning off “Print only constant string literals” option.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_strlit6-3.png?width=280&height=461&name=hr_strlit6-3.png)

To change this option for all future databases, see the `HO_CONST_STRINGS` option in `hexrays.cfg`.

For more info see the decompiler manual:

  * [Tips and tricks: Constant memory](<https://hex-rays.com/products/decompiler/manual/tricks.shtml#02>)
  * [Configuration](<https://hex-rays.com/products/decompiler/manual/config.shtml>)

---

## 4. 

**Date:** Posted: Sep 3, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols](https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #55: Using debug symbols

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols>)

I 

Igor Skochinsky ✦ Posted: Sep 3, 2021

![Igor’s tip of the week #55: Using debug symbols](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

IDA supports many file formats, among them the main ones used on the three major operating systems:

  * PE (Portable Executable) on Windows;
  * ELF (Executable and Linkable Format) on Linux;
  * Mach-O (Mach object) on macOS.



### Symbols and debugging information

_Symbols_ associate locations inside the file (e.g. addresses of functions or variables) with textual names (usually the names used in the original source code). The part of the file storing this association is commonly called _symbol table_. Symbols can be stored in the file itself or separately.

Traditionally, the PE files do not contain any symbols besides those that are required for imports or exports for inter-module linking. ELF and Mach-O commonly do keep names for global functions, however most of this information can be removed, or _stripped_ , without affecting execution of the file. Because such information is very valuable for possible debugging later, it can be stored in a separate _debug information file_. 

For PE files, a common debug format is **PDB** (Program Database), although other formats were used in the past, for example **TDS** (Turbo Debugger Symbols) was used by Borland compilers, and **DBG** in legacy versions of Visual Studio. Both ELF and Mach-O use [DWARF](<https://en.wikipedia.org/wiki/DWARF>). All of the above can contain not only plain symbols but also **types** (structures, enums, typedefs), function **prototypes** , information on **local variables** as well as mapping of binary code to **source files**(filenames and line numbers).

Although originally intended to improve debugging experience, all this information obviously makes the reverse engineering process much easier, so IDA supports these formats out of box, using standard plugins shipped with IDA:

  * pdb for PDB;
  * tds for TDS;
  * dbg for DBG;
  * dwarf for DWARF.



### Automatic debug info loading

Standard file loaders detect when the file has been built with debug information and invoke the corresponding debug info loader. If debug info is found in the input file, next to it, or in another well-known location, the user is prompted whether to load it.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdb_auto-3.png?width=351&height=212&name=pdb_auto-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dwarf_auto-3.png?width=232&height=217&name=dwarf_auto-3.png)

### Manual debug info loading

If the separate debug info file is not present in standard location or discovered later, after you’ve already loaded the file, it can be loaded manually. Currently only PDB and DWARF can be loaded using this option. 

  * For PDB, use File > Load file > PDB File…
  * For DWARF, Edit > Plugins > Load DWARF File



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dbgi_pdb-3.png?width=542&height=228&name=dbgi_pdb-3.png)

For the PDB loader, you can specify a DLL or EXE file instead of the PDB; in that case IDA will try to find and load a matching PDB for it, including downloading it from symbol servers if necessary. By using the “Types only” option, you can import types from an arbitrary PDB and not necessarily PDB for the current file. For example, PDB for the Windows kernel (ntoskrnl.exe) contains various structures used in kernel-mode code (drivers etc.) so this feature can be useful when reverse-engineering files without available debug info.

### Example: Linux kernel debug info

Linux kernels are usually stripped during build, however many distros provide separate [debug info repositories](<http://ddebs.ubuntu.com/pool/main/l/linux/>), or you can [recompile the kernel with debug info](<https://wiki.ubuntu.com/Kernel/Systemtap#How_do_I_build_a_debuginfo_kernel_if_one_isn.27t_available.3F>). How to load it into IDA?

For self-built kernel it’s pretty simple — the `vmlinux`file is a normal ELF which can be simply loaded into IDA. However, the pre-built kernels are usually distributed as `vmlinuz` which is a PE file (so that it can be booted directly by the UEFI firmware), with the actual kernel code stored as compressed payload inside it. The unpacked kernel can be extracted [manually](<https://stackoverflow.com/questions/12002315/extract-vmlinux-from-vmlinuz-or-bzimage>) or using the [vmlinux-to-elf](<https://github.com/marin-m/vmlinux-to-elf>) project, loaded into IDA, and the external debuginfo file can then be loaded via Edit > Plugins > Load DWARF File, producing a nice database with all kernel types and proper function prototypes.

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dwarf_linux-1024x484-3.png?width=1024&height=484&name=dwarf_linux-1024x484-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/dwarf_linux-3.png>)

---

## 5. 

**Date:** Posted: Aug 27, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-54-shifted-pointers](https://hex-rays.com/blog/igors-tip-of-the-week-54-shifted-pointers)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #54: Shifted pointers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-54-shifted-pointers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-54-shifted-pointers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-54-shifted-pointers>)

I 

Igor Skochinsky ✦ Posted: Aug 27, 2021

![Igor’s tip of the week #54: Shifted pointers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

[Previously](<https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes/>) we briefly mentioned _shifted pointers_ but without details. What are they?

Shifted pointers is another custom extension to the C syntax. They are used by IDA and decompiler to represent a pointer to an object with some offset or _adjustment_ (positive or negative). Let’s see how they work and several situations where they can be useful.

### Shifted pointer description and syntax

A shifted pointer is a regular pointer with additional information about the name of the parent structure and the offset from its beginning. For example, consider this structure:
    
    
    struct mystruct
    {
     char buf[16];
     int dummy;
     int value;            // <- myptr points here
     double fval;
    };
    

And this pointer declaration:
    
    
     int *__shifted(mystruct,20) myptr;
    

It means that `myptr` is a pointer to `int` and if we decrement it by **20** bytes, we end up with `mystruct*`. 

In fact, the offset value is not limited to the containing structure and can even be negative. Also, the “parent” type does not have to be a structure but can be any type except `void`. This can be useful in some situations.

Whenever a shifted pointer is used with an adjustment, it will be displayed using the `ADJ` helper, a pseudo-operator which returns the pointer to the parent type (in our case `mystruct`). For example, if the pointer is dereferenced after adding 4 bytes, it can be represented like this:
    
    
            ADJ(myptr)->fval
    

### Optimized loop on array of structures

When compiling code which is processing an array of structures, a compiler may optimize the loop so that the “current item” pointer points into a middle of the structure instead of the beginning. This is especially common when only a small subset of fields are being accessed. Consider this example:
    
    
    struct mydata
    {
      int a, b, c;
      void *pad[2];
      int d, e, f;
      char path[260];
    };
    
    int sum_c_d(struct mydata *arr, int count)
    {
        int sum=0;
        for (int i=0; i< count; i++)
        {
            sum+=arr[i].d+arr[i].c*43;
        }
        return sum;
    }

When compiled with Microsoft Visual C++ x86, it can produce the following code:
    
    
    ?sum_c_d@@YAHPAUmydata@@H@Z proc near
    
    arg_0 = dword ptr  4
    arg_4 = dword ptr  8
    
          mov     edx, [esp+arg_4]
          push    esi
          xor     esi, esi
          test    edx, edx
          jle     short loc_25
          mov     eax, [esp+4+arg_0]
          add     eax, 14h
    
    loc_12:                                 ; CODE XREF: sum_c_d(mydata *,int)+23↓j
          imul    ecx, [eax-0Ch], 2Bh ; '+'
          add     ecx, [eax]
          lea     eax, [eax+124h]
          add     esi, ecx
          sub     edx, 1
          jnz     short loc_12
    
    loc_25:                                 ; CODE XREF: sum_c_d(mydata *,int)+9↑j
          mov     eax, esi
          pop     esi
          retn
    

And initial decompilation looks quite strange even after adding and specifying the correct types:
    
    
    int __cdecl sum_c_d(struct mydata *arr, int count)
    {
      int v2; // edx
      int v3; // esi
      int *p_d; // eax
      int v5; // ecx
    
      v2 = count;
      v3 = 0;
      if ( count <= 0 )
        return v3;
      p_d = &arr->d;
      do
      {
        v5 = *p_d + 43 * *(p_d - 3);
        p_d += 73;
        v3 += v5;
        --v2;
      }
      while ( v2 );
      return v3;
    }

Apparently the compiler decided to use the pointer to the `d` field and accesses `c` relative to it. How can we make this look nicer?

We can find out the offset at which `d` is situated in the structure via manual calculation, by inspecting disassembly, or by hovering the mouse over it in pseudocode.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hint_offset-3.png?width=284&height=116&name=hint_offset-3.png)

Thus, we can change the type of `p_d` to `int * __shifted(mydata, 0x14)` to get improved pseudocode:
    
    
    int __cdecl sum_c_d(struct mydata *arr, int count)
    {
      int v2; // edx
      int v3; // esi
      int *__shifted(mydata,0x14) p_d; // eax
      int v5; // ecx
    
      v2 = count;
      v3 = 0;
      if ( count <= 0 )
        return v3;
      p_d = &arr->d;
      do
      {
        v5 = ADJ(p_d)->d + 43 * ADJ(p_d)->c;
        p_d += 73;
        v3 += v5;
        --v2;
      }
      while ( v2 );
      return v3;
    }

### Prepended metadata

This technique is used in situations where a raw block of memory needs to have some management info attached to it, i.e. heap allocators, managed strings and so on.

As a specific example, let’s consider the classic MFC 4.x CString class. It uses a structure placed before the actual character array:
    
    
    struct CStringData
    {
        long  nRefs;    // reference count
        int   nDataLength;    // length of data (including terminator)
        int   nAllocLength;   // length of allocation
        // TCHAR data[nAllocLength]
    
        TCHAR* data()         // TCHAR* to managed data
        {
            return (TCHAR*)(this+1);
        }
    };
    

The `CString`class itself has just one data member:
    
    
    class CString
    {
    public:
    // Constructors
    [...skipped]
    private:
        LPTSTR   m_pchData;        // pointer to ref counted string data
    
        // implementation helpers
        CStringData* GetData() const;
    [...skipped]
    };
    inline
    CStringData*
    CString::GetData(
        ) const
    {
        ASSERT(m_pchData != NULL);
        return ((CStringData*)m_pchData)-1;
    }
    

Here’s how it looks in memory:
    
    
                   ┌───────────────┐
                   │   nRefs       │
                   ├───────────────┤
     CStringData   │ nDataLength   │
                   ├───────────────┤
                   │ nAllocLength  │
                   ├───────────────┴─────┐
               ┌──►│'H','e','l','l','o',0│
               │   └─────────────────────┘
               │
               │
             ┌─┴────────┐
    CString  │m_pchData │
             └──────────┘
    

Here’s how the CString’s destructor looks like in initial decompilation:
    
    
    void __thiscall CString::~CString(CString *this)
    {
      if ( *(_DWORD *)this - (_DWORD)off_4635E0 != 12 && InterlockedDecrement((volatile LONG *)(*(_DWORD *)this - 12)) <= 0 )
        operator delete((void *)(*(_DWORD *)this - 12));
    }

Even after creating a CString structure with a single member`char *m_pszData`it’s still somewhat confusing:
    
    
    void __thiscall CString::~CString(CString *this)
    {
      if ( this->m_pszData - (char *)off_4635E0 != 12 && InterlockedDecrement((volatile LONG *)this->m_pszData - 3) <= 0 )
        operator delete(this->m_pszData - 12);
    }

Finally, if we create the`CStringData`struct as described above and change the type of the CString member to: `char *__shifted(CStringData,0xC) m_pszData`:
    
    
    void __thiscall CString::~CString(CString *this)
    {
      if ( ADJ(this->m_pszData)->data - (char *)off_4635E0 != 12 && InterlockedDecrement(&ADJ(this->m_pszData)->nRefs) <= 0 )
        operator delete(ADJ(this->m_pszData));
    }

Now the code is more understandable: if the decremented reference count becomes zero, the `CStringData`instance is deleted.

More info: [IDA Help: Shifted pointers](<https://hex-rays.com/products/ida/support/idadoc/1695.shtml>)

---

## 6. 

**Date:** Posted: Aug 20, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-53-manual-switch-idioms](https://hex-rays.com/blog/igors-tip-of-the-week-53-manual-switch-idioms)

[Back](<https://hex-rays.com/blog>)

[idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #53: Manual switch idioms

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-53-manual-switch-idioms>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-53-manual-switch-idioms>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-53-manual-switch-idioms>)

I 

Igor Skochinsky ✦ Posted: Aug 20, 2021

![Igor’s tip of the week #53: Manual switch idioms](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

IDA supports most of the switch patterns produced by major compilers out-of-box and usually you don’t need to worry about them. However, occasionally you may encounter a code which has been produced by an unusual or a very recent compiler version, or some peculiarity of the code prevented IDA from recognizing the pattern, so it may become necessary to help IDA and tell it about the switch so a proper function graph can be presented and decompiler can produce nice pseudocode.

### Switch pattern components

The common switch pattern is assumed to have the following components:

  1. indirect jump  
This is an instruction which actually performs the jump to the destination block handling the switch case; usually involves some register holding the address value;
  2. jump table  
A table of values, containing either direct addresses of the destination blocks, or some other values allowing to calculate those addresses (e.g. offsets from some _base_ address). It has to be of a specific fixed _size_(number of elements) and the values may be scaled with a _shift_ value. Some switches may use two tables, first containing indexes into the second one with addresses.
  3. input register  
register containing the initial value which is being used to determine the destination block. Most commonly it is used to index the jump table.



### Switch formula

The standard switches are assumed to use the following calculation for the destination address:
    
    
    target = base +/- (table_element << shift)

`base` and `shift` can be set to zero if not used.

### Example

Here’s a snippet from an ARM64 firmware.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/switch_arm64-3.png?width=732&height=325&name=switch_arm64-3.png)

The indirect jump is highlighted with the red rectangle. Here’s the same code in text format:
    
    
    __text:FFFFFF8000039F88 STP X22, X21, [SP,#-0x10+var_20]!
    __text:FFFFFF8000039F8C STP X20, X19, [SP,#0x20+var_10]
    __text:FFFFFF8000039F90 STP X29, X30, [SP,#0x20+var_s0]
    __text:FFFFFF8000039F94 ADD X29, SP, #0x20
    __text:FFFFFF8000039F98 MOV X19, X0
    __text:FFFFFF8000039F9C BL sub_FFFFFF80000415E4
    __text:FFFFFF8000039FA0 B.HI loc_FFFFFF800003A01C
    __text:FFFFFF8000039FA4 MOV W20, #0
    __text:FFFFFF8000039FA8 ADR X9, byte_FFFFFF8000048593
    __text:FFFFFF8000039FAC NOP
    __text:FFFFFF8000039FB0 ADR X10, loc_FFFFFF8000039FC0
    __text:FFFFFF8000039FB4 LDRB W11, [X9,X8]
    __text:FFFFFF8000039FB8 ADD X10, X10, X11,LSL#2
    __text:FFFFFF8000039FBC BR X10

We can see that the register used in the indirect branch (`X10`) is a result of some calculation so it is probably a switch pattern. However, because the code was compiled with size optimization (the range check is moved into a separate function used from several places), IDA was not able to match the pattern in the automatic fashion. Let’s see if we can find out components of the standard switch described above.

The formula matches the instruction `ADD X10, X10, X11,LSL#2`(in C syntax: `X10 = X10+(X11<<2)`). We can see that the table element (`X11`) is shifted by 2 before being added to the base (`X10`). The value of `X11` comes from the previous load of `W11` using `LDRB` (load _byte_) from the table at `X9` and index `X8`. Thus:

  1. Indirect jump: yes, the `BR X10` instruction at `FFFFFF8000039FBC`.
  2. jump table: yes, at `byte_FFFFFF8000048593`. Additionally, we have a _base_ at `loc_FFFFFF8000039FC0` and _shift_ value of **2**. It contains **eight** elements (this can be checked visually or deduced from the range check which uses **7** as the maximum allowed value).
  3. input register: yes, `X8` is used to index the table (we can also use **W8** which is the 32-bit part of `X8` and is used by the range check function.



Now that we have everything, we can specify the pattern by putting the cursor on the indirect branch and invoking Edit > Other > Specify switch idiom…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/switch_dlg-3.png?width=425&height=445&name=switch_dlg-3.png)

The values can be specified in C syntax (0x…) or as labels thanks to the [expression evaluation](<https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida/>) feature. Once the dialog is confirmed, we can observe the switch nicely labeled and function graph updated to include newly reachable nodes.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/switch_graph-3.png?width=745&height=386&name=switch_graph-3.png)

We can also use “List cross-references from…” (`Ctrl`–`J`) to see the list of targets from the indirect jump.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/switch_xrefs-3.png?width=875&height=302&name=switch_xrefs-3.png)

#### Additional options

Our example was pretty straightforward but in some cases you can make use of the additional options in the dialog.

  1. separate value table is present: when a two-level table is used, i.e.:

`table_element = jump_table[value_table[input_register]];` instead of the default `table_element = jump_table[input_register];`

  2. signed jump table elements: when table elements are loaded using a sign-extension instruction, for example `LDRSB` or `LDRSW` on ARM or `movsx` on x86.
  3. Subtract table elements: if the values are _subtracted_ from the base instead of being added (minus sign is used in the formula).
  4. Table element is insn: the “jump table” contains instructions instead of data values. This is used in some architectures which can perform relative jumps using a delta value from the instruction pointer. For example, the legacy ARM jumps using direct `PC` manipulation:  

         
         CMP             R3, #7 ; SWITCH ; switch 8 cases             
                         ADDLS           PC, PC, R3,LSL#2 ; switch jump               
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_6684                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               def_6680 ; jumptable 00006680 default case, c
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_6688                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               loc_66A8 ; jumptable 00006680 case 0         
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_668C                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               loc_6854 ; jumptable 00006680 case 1         
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_6690                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               loc_68CC ; jumptable 00006680 case 2         
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_6694                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               loc_695C ; jumptable 00006680 case 3         
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_6698                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               loc_6990 ; jumptable 00006680 case 4         
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_669C                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               loc_69FC ; jumptable 00006680 case 5         
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_66A0                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               def_6680 ; jumptable 00006680 default case, c
         ; ---------------------------------------------------------------------------
                                                                                      
         loc_66A4                                ; CODE XREF: __pthread_manager+1BC↑j 
                         B               loc_699C ; jumptable 00006680 case 7         
         ; ---------------------------------------------------------------------------
         

Usually in such situation the table “elements” are fixed-size branches to the actual destinations.




#### Optional values

Some values can be omitted by default but you may also fill them for a more complete mapping to the original code:

  1. Input register of switch: can be omitted if you only need cross-references for the proper function flow graph but it has to be specified if you want decompiler to properly parse and represent the switch.
  2. First(lowest) input value: the value of the input register corresponding to the entry 0 of the jump table. In the example above we can see that the range check calculates `W8 = W1 - 1`, so we could specify lowest value of 1 (this would also update the comments at the destination addresses to be 1 to 8 instead of 0 to 7).
  3. default jump address: the address executed when the input range check fails (in our example – destination of the `B.HI` instruction). Can make the listing and/or decompilation a little more neat but is not strictly required otherwise.



For even more detailed info about supported switch patterns, see the [`switch_info_t` structure](<https://hex-rays.com/products/ida/support/sdkdoc/structswitch__info__t.html>) and the `uiswitch` plugin source code in the SDK. If you encounter a switch which cannot be handled by the standard formula, you can also look into writing a [custom jump table handler](<https://hex-rays.com/blog/jump-tables/>).

---

## 7. 

**Date:** Posted: Aug 13, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes](https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes)

[Back](<https://hex-rays.com/blog>)

[idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #52: Special type attributes

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes>)

I 

Igor Skochinsky ✦ Posted: Aug 13, 2021

![Igor’s tip of the week #52: Special type attributes](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

IDA uses mostly standard C (and basic C++) syntax, but it also supports some extensions, in particular to represent low-level details which are not necessary for “standard” C code but are helpful for real-life binary code analysis. We’ve already covered custom [types](<https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types/>) and [calling conventions](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>), but there are more extensions you may use or encounter.

### Function attributes

The following attributes may be used in function prototypes:

  * `__pure` : a pure function (always returns the same result for same inputs and does not  
affect memory in a visible way);
  * `__noreturn`: function does not return to the caller;
  * `__usercall` or `__userpurge`: user-defined calling convention (see [previous post](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>));
  * `__spoils`: explicit spoiled registers specification (see [previous post](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>));
  * `__attribute__((format(printf,n1,n2)))`: variadic function with a printf-style format string in argument at position n1 and variadic argument list at position n2.



### Argument attributes

These attributes can often appear when IDA _lowers_ a user-provided prototype to represent the actual low-level details of argument passing.

  * `__hidden`: the argument was not present in source code (for example the implicit `this` pointer in C++ class methods).
  * `__return_ptr`: hidden argument used for the return value (implies `__hidden`);
  * `__struct_ptr`: argument was originally a structure value;
  * `__array_ptr`: argument was originally an array (arrays ;
  * `__unused`: unused function argument.



For example, if `s1` is a structure of 16 bytes, then the following prototype:
    
    
    struct s1 func();

will be lowered by IDA to:
    
    
    struct s1 *__cdecl func(struct s1 *__return_ptr __struct_ptr retstr);

### Other attributes

  * `__cppobj`: used for structures representing C++ objects; some layout details change if this attribute is used (e.g. treatment of empty structs or reuse of end-of-struct padding in inheritance);
  * `__ptr32`, `__ptr64`: explicitly-sized pointers;
  * `__shifted`: a pointer which points not at the start of an object but some location inside or before it.



See also:[ Set function/item type](<https://hex-rays.com/products/ida/support/idadoc/1361.shtml>) in IDA Help.

---

## 8. 

**Date:** Posted: Aug 6, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions](https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #51: Custom calling conventions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions>)

I 

Igor Skochinsky ✦ Posted: Aug 6, 2021

![Igor’s tip of the week #51: Custom calling conventions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

The Hex-Rays decompiler was originally created to deal with code produced by standard C compilers. In that world, everything is (mostly) nice and orderly: the [calling conventions](<https://docs.microsoft.com/en-us/cpp/cpp/calling-conventions>) are known and standardized and the arguments are passed to function according to the [ABI](<https://en.wikipedia.org/wiki/Application_binary_interface>).

However, the real life is not that simple: even in code coming from standard compilers there may be helper functions accepting arguments in non-standard locations, code written in assembly, or [whole program optimization](<https://docs.microsoft.com/en-us/cpp/build/reference/gl-whole-program-optimization>) causing compiler to use custom calling conventions for often-used functions. And code created with non-C/C++ compilers may use completely different calling conventions (a notable example is Go).

Thus a need arose to specify custom calling conventions so that the decompiler can provide readable output when they’re used. For this, ability to specify custom calling conventions has been added to IDA and decompiler.

### Usercall

The most commonly used custom calling convention is specified using the keyword `__usercall`. The basic syntax is as follows:
    
    
    {return type} __usercall funcname@<return argloc>({type} arg1, {type} arg2@<argloc>, ...);

where `argloc`is one of the following:

  * a processor register name, e.g. `eax`, `ebx`, `esi` etc. In some cases flag registers (`zf`, `sf`, `cf` etc.) may be accepted too.
  * a register pair delimited with a colon, e.g. `<edx:eax>`.



The register size should match the argument or return type (if the function returns `void`, return argloc must be omitted). Arguments without location specifiers are assumed to be passed on stack according to usual rules.

### Scattered argument locations

In complicated situations a large argument (such as a structure instance) may be passed in multiple registers and/or stack slots. In such case the following descriptors can be used:

  * a partial register location: `argoff:register^regoff.size`.
  * a partial stack location: `argoff:^stkoff.size`.
  * a list of partial register and/or stack locations covering the whole argument delimited with a comma.



Where:

  * `argoff` – offset within the argument
  * `stkoff` – offset in the stack frame (the first stack argument is at offset 0)
  * `register` – register name used to pass part of the argument
  * `regoff` – offset within the register
  * `size` – number of bytes for this portion of the argument



` regoff` and `size` can be omitted if there is no ambiguity (i.e. whole register is used).

For example, a 12-byte structure passed in `RDI` and `RSI` could be specified like this:
    
    
    void __usercall myfunc(struc_1 s@<0:rdi, 8:rsi.4>);

### Userpurge

The `__userpurge` calling convention is equivalent to `__usercall` except it is assumed that the callee adjusts the stack to account for arguments passed on stack (this is similar to how `__cdecl` differs from `__stdcall` on x86).

### Spoiled registers

The compiler or OS ABI also usually specifies which registers are caller-saved, i.e. may be _spoiled_ (or _clobbered_) by a function call. In general, any register which can be used for argument passing or return value is considered potentially spoiled because the called function could in turn call other functions. For example, on x86, `EAX`, `ECX`, and `EDX` are by default considered spoiled and their values after the call are considered undefined by the decompiler. If this is not the case, you can help the decompiler by using the `__spoils<{reglist}>` specifier. For example, if the function does not clobber any registers, you can use the following prototype:

`void __spoils<> func();`  
  
If a custom memcpy implementation uses `esi` and `edi` without saving and restoring them, you can add them to the spoiled list:
    
    
    void* __spoils<esi, edi> memcpy(void*, void*, int);

The `__spoils` attribute can also be combined with `__usercall`:
    
    
    int __usercall __spoils<> g@<esi>();

See also:[ Set function/item type](<https://hex-rays.com/products/ida/support/idadoc/1361.shtml>) and [Scattered argument locations](<https://hex-rays.com/products/ida/support/idadoc/1492.shtml>) in IDA Help.

---

## 9. 

**Date:** Posted: Jul 30, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-50-execution-flow-arrows](https://hex-rays.com/blog/igors-tip-of-the-week-50-execution-flow-arrows)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #50: Execution flow arrows

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-50-execution-flow-arrows>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-50-execution-flow-arrows>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-50-execution-flow-arrows>)

I 

Igor Skochinsky ✦ Posted: Jul 30, 2021

![Igor’s tip of the week #50: Execution flow arrows](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Although nowadays most IDA users probably use the [graph view](<https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view/>), the text view can still be useful in certain situations. In case you haven’t noticed, it has a UI element which can help you visualize code flow even without the full graph and even outside of functions (the graph view is available only for functions). This element is shown on the left of the disassembly listing:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrows-3.png?width=588&height=229&name=arrows-3.png)

The arrows represent code flow (cross-references) and the following types may be present:

  * Solid lines represent unconditional jumps/branches, dashed lines – conditional ones;
  * Thick arrows are used for jumps back to lower addresses (they indicate potential loops);
  * The current arrow is highlighted in black;
  * Red arrows are used when target and/or destination lies outside of the function boundaries 



In addition to arrows, the blue dots indicate potential breakpoint location, so the breakpoint can be added by clicking on the dot, which will highlight the whole line red to indicate an active breakpoint.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dot_breakpoint-3.png?width=558&height=62&name=dot_breakpoint-3.png)

---

