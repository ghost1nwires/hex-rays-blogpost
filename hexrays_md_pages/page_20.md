# Hex-Rays Blog – Page 20

**Archived:** 2026-02-20 12:16
**URL:** https://hex-rays.com/blog/page/20

---

## 1. 

**Date:** Posted: Sep 30, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-108-raw-memory-accesses-in-pseudocode](https://hex-rays.com/blog/igors-tip-of-the-week-108-raw-memory-accesses-in-pseudocode)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #108: Raw memory accesses in pseudocode

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-108-raw-memory-accesses-in-pseudocode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-108-raw-memory-accesses-in-pseudocode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-108-raw-memory-accesses-in-pseudocode>)

I 

Igor Skochinsky ✦ Posted: Sep 30, 2022

![Igor’s tip of the week #108: Raw memory accesses in pseudocode](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Sometimes in pseudocode you may encounter strange-looking code:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rawmem1-3.png?width=299&height=228&name=rawmem1-3.png)

The code seems to dereference an array called`MEMORY` and is highlighted in red. However, this variable is not defined anywhere. What is it?

Such notation is used by the decompiler when the code accesses memory addresses not present in the database. In most cases it indicates an error in the original source code. If we look at the disassembly for the example above, we’ll see this:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rawmem2-3.png?width=743&height=307&name=rawmem2-3.png)

The variable `pfont` is loaded into register `edx` which is then compared against zero using `test edx, edx/jz` sequence. The jump to `loc_4060D3` can only occur if `edx` is zero, which means that the `mov eax, [edx+10h]` instruction will try to dereference the address `0x10`. Because the database does not contain the address `0x10`, it can’t be represented as a normal or a dummy variable so the decompiler represents it as a pseudo-variable `MEMORY` and uses the address as the index. The dereference is shown in red to bring attention to the potential error in the code. For example, judging by the assembly, in this binary the programmer tried reading a structure pointer even if it is NULL. A more modern compiler would probably even remove such code as dereferencing NULL pointer is undefined behavior.

In cases where such access is **not** an error (for example, the code directly accesses memory-mapped hardware registers), creating a new segment for the accessed address range is usually the correct approach.

---

## 2. 

**Date:** Posted: Sep 23, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-107-multiple-return-values](https://hex-rays.com/blog/igors-tip-of-the-week-107-multiple-return-values)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #107: Multiple return values

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-107-multiple-return-values>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-107-multiple-return-values>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-107-multiple-return-values>)

I 

Igor Skochinsky ✦ Posted: Sep 23, 2022

![Igor’s tip of the week #107: Multiple return values](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

The Hex-Rays decompiler was initially created to decompile C code, so its pseudocode output uses (mostly) C syntax. However, the input binaries may be compiled using other languages: C++, Pascal, Basic, ADA, and many others. While the code of most of them can be represented in C without real issues, some have peculiarities which require [language extensions](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>) or have to be handled with [user input](<https://hex-rays.com/blog/igors-tip-of-the-week-71-decompile-as-call/>). Still, some languages use approaches so different from standard compiled С code that special handling for that is necessary. For example, [Go](<https://go.dev/>) uses a [calling convention](<https://go.dev/src/cmd/compile/abi-internal>) (stack-based or register-based) so different from standard C calling conventions, that custom support for it had to be [added to IDA](<https://hex-rays.com/products/ida/news/7_6/>). 

### Multiple return values

Even with custom calling conventions, one fundamental limitation of IDA’s type system remains (as of IDA 8.0): a function may return only a single value. However, even in otherwise C-style programs you may encounter functions which return more than one value. One example is compiler helpers like `idivmod`/`uidivmod`. They return simultaneously the quotient and remainder of a division operation. The decompiler knows about the standard ones (e.g. `__aeabi_idivmod`for ARM EABI) but you may encounter a non-standard implementation, or an unrelated function using a similar approach (e.g. a function written manually in assembly).

Because the decompiler does not expect that function returns more than one value, you may need to inspect the disassembly or look at the place of the call to recognize such functions. For example, here’s a fragment of decompiled ARM32 code which seems to use an undefined register value:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mretval0-3.png?width=458&height=170&name=mretval0-3.png)

The function seems to modify the `R1` register, although normally the return values (for 32-bit types) are placed in R0. Possibly this is an equivalent of divmod function which returns quotient in `R0` and remainder in `R1`?

To handle this, we can use an artificial structure and a custom calling convention specifying the registers and/or stack locations where it should be placed. For example, add such struct to Local Types:
    
    
    struct divmod_t
    {
      int quot;
      int rem;
    };

and set the function prototype: `divmod_t __usercall my_divmod@<R1:R0>(int@<R0>, int@<R1>);`

The decompiler then interprets the register values after the call as if they were structure fields:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mretval1-3.png?width=408&height=172&name=mretval1-3.png)

A similar approach may be used for languages with native support for functions with multiple return values: Go, Swift, Rust etc.

See also:

[Igor’s tip of the week #51: Custom calling conventions](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>)

---

## 3. 

**Date:** Posted: Sep 16, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-106-outlined-functions](https://hex-rays.com/blog/igors-tip-of-the-week-106-outlined-functions)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #106: Outlined functions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-106-outlined-functions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-106-outlined-functions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-106-outlined-functions>)

I 

Igor Skochinsky ✦ Posted: Sep 16, 2022

![Igor’s tip of the week #106: Outlined functions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

The release notes for [IDA 8.0](<https://hex-rays.com/products/ida/news/8_0/>) mention _outlined functions_. What are those and how to deal with them in IDA?

Function outlining is an optimization that saves code size by identifying recurring sequences of machine code and replacing each instance of the sequence with a call to a new function that contains the identified sequence of operations. It can be considered an extension of the [shared function tail](<https://hex-rays.com/blog/igors-tip-of-the-week-87-function-chunks-and-the-decompiler/>) optimization by sharing not only tails but arbitrary common parts of functions.

### Function outlining example

For example, here’s a function from iOS’s `debugserver` with some calls to outlined fragments:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outlined1-3.png?width=812&height=509&name=outlined1-3.png)

The first fragment contains only two instructions besides the return instruction so it may not sound like we’re saving much, but by looking at the cross-references you’ll see that it is used in many places:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outlined2-3.png?width=875&height=405&name=outlined2-3.png)

So the savings accumulated across the whole program can be quite substantial.

### Handling outlined functions in decompiler

If we decompile the function, the calls to outlined fragments are shown as is, and the registers used or set by them show up as potentially undefined (orange color):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outlined3-3.png?width=510&height=307&name=outlined3-3.png)

To tell the decompiler that the calls should be inlined into the function’s body, all the `OUTLINED_FUNCTION_NN` should be marked as outlined code. This can be done manually, via the Edit Function (`Alt`–`P`) dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outlined4-3.png?width=490&height=329&name=outlined4-3.png)

The added attribute is also displayed in the listing:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outlined5-3.png?width=578&height=226&name=outlined5-3.png)

Once all outlined functions are marked up, the decompiler inlines them and there are no more possibly undefined variables:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outlined6-3.png?width=710&height=255&name=outlined6-3.png)

### Automating outlined function processing

If you have a big binary with hundreds or thousands of functions, it may become pretty tedious to mark up outlined functions manually. In such case, making a small script may speed things up. For example, if you have symbols and outlined functions have a known naming pattern, the following Python snippet should work:
    
    
    import idautils
    import ida_name
    import ida_funcs
    for f in idautils.Functions():
        nm = ida_name.get_name(f)
        if nm.startswith("_OUTLINED_FUNCTION") or nm.find(".cold.") != -1:
            print ("%08X: %s"% (f, nm))
            pfn = ida_funcs.get_func(f) 
            pfn.flags |= idaapi.FUNC_OUTLINE 
            ida_funcs.update_func(pfn)
    

It can be executed using File > Script command… (`Shift`+`F2`)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/outlined7-3.png?width=680&height=320&name=outlined7-3.png)

See also:

[IDA Help: Edit Function ](<https://www.hex-rays.com/products/ida/support/idadoc/485.shtml>)

[IDA Help: Function flags](<https://www.hex-rays.com/products/ida/support/idadoc/1729.shtml>)

---

## 4. 

**Date:** Posted: Sep 12, 2022  
**Link:** [https://hex-rays.com/blog/hex-rays-launches-a-beta-program](https://hex-rays.com/blog/hex-rays-launches-a-beta-program)

[Back](<https://hex-rays.com/blog>)

[beta tester](<https://hex-rays.com/blog/tag/beta-tester>) [beta program](<https://hex-rays.com/blog/tag/beta-program>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [beta testing](<https://hex-rays.com/blog/tag/beta-testing>) [beta](<https://hex-rays.com/blog/tag/beta>)

# Hex-Rays launches a Beta Program!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-launches-a-beta-program>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-launches-a-beta-program>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-launches-a-beta-program>)

A 

Alex Petrov ✦ Posted: Sep 12, 2022

![Hex-Rays launches a Beta Program!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

In the past, we have worked with various beta testers that helped us shape our products by trying out our pre-release versions. Today, we are launching our **Beta Program** initiative with the idea of building a community of enthusiasts who would regularly take an active part in the evolution of IDA. 

If you have an active IDA license, are keen on test-driving our new releases before they are available to the public, and would take the challenge seriously, we want to hear from you!

[ **Read more about the program and subscribe**](<https://hex-rays.com/beta-program/>)

---

## 5. 

**Date:** Posted: Sep 9, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base](https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #105: Offsets with custom base

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base>)

I 

Igor Skochinsky ✦ Posted: Sep 9, 2022

![Igor’s tip of the week #105: Offsets with custom base](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

We’ve already covered [simple offsets](<https://hex-rays.com/blog/igors-tip-of-the-week-95-offsets/>), where an operand value or a data value matches an address in the program and so can be directly converted to an offset. However, programs may also employ more complex, or indirect ways of referring to a location. One common approach is using a small offset from some predefined base address.

### Offset (displacement) from a register

Many processors support instructions with addressing modes called “register with displacement”, “register with offset” or similar. Operands in such mode may use syntax similar to following:

  1. reg(offset)
  2. offset(reg)
  3. reg[offset]
  4. [reg, offset]
  5. [reg+offset]
  6. etc.



The basic logic is the same in all cases: offset is added to the value of the register and then used as a number or (more commonly) as an address. In the latter case it may be useful to have IDA calculate the final address for you and add the cross-reference to it. If you know the value of the register at the time this instruction is executed (e.g. it is set in the preceding instructions), it is very simple to do:

  1. With the cursor on the operand, Invoke Edit > Operand type > Offset > Offset (user-defined), or press `Ctrl`–`R`;  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offbase1-3.png?width=1032&height=470&name=offbase1-3.png)
  2. Enter the register value in the _Base address_ field;  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offbase2-3.png?width=1033&height=600&name=offbase2-3.png)
  3. Click OK;
  4. IDA will calculate the final address, replace the offset value by an equivalent expression, and add a cross-reference to destination:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/offbase3-3.png?width=805&height=99&name=offbase3-3.png)



Now it is obvious that the location being referenced is `dword_E01FC0C4`.  
  


See also:   
[IDA Help: Convert operand to offset (user-defined base)](<https://www.hex-rays.com/products/ida/support/idadoc/470.shtml>)  
[IDA Help: Complex Offset Expression](<https://www.hex-rays.com/products/ida/support/idadoc/471.shtml>)

---

## 6. 

**Date:** Posted: Sep 2, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-season-02](https://hex-rays.com/blog/igors-tip-of-the-week-season-02)

[Back](<https://hex-rays.com/blog>)

[idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [“Igor’s tip of the week.”](<https://hex-rays.com/blog/tag/igors-tip-of-the-week->)

# Igor’s tip of the week: Season 02

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-season-02>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-season-02>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-season-02>)

A 

Alex Petrov ✦ Posted: Sep 2, 2022

![Igor’s tip of the week: Season 02](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In August 2020, we started blog series called **[“Igor’s tip of the week.”](<https://hex-rays.com/blog/tag/idatips/>) ** These blog posts aim to help you better work with IDA & Decompilers. Over the years, his pieces of advice have become very popular, and our natural response was to make them even more accessible and easy to read. That’s how we came up with **[Igor’s tip of the week – Season 1](<https://hex-rays.com/blog/igors-tip-of-the-week-season-01/>)** , which had 52 of his valuable hints.

We are pleased that our colleague Igor continued his sterling job, and Season 2 is now a fact! You can expect more of his knowledgeable tips, categorized under **six main topics** :

  * **Usage:** basic and advanced usage of IDA features
  * **Types:** working with types
  * **Hidden:** hidden gems, not widely known but useful functionality
  * **Decompiler:** related to the Hex-Rays decompiler
  * **Automation:** automating repetitive tasks
  * **Customization:** customizing IDA UI to better suit your workflow.



Have you missed Season 1? **[Read it now](<https://hex-rays.com/blog/igors-tip-of-the-week-season-01/>)**

Are you ready to take your IDA skills to a higher level? **[Register for one of our upcoming training sessions!](<https://hex-rays.com/training/>)**

[![Igor-tip-book-S02](https://hex-rays.com/hubfs/Imported_Blog_Media/Igor-Poster-Season-2_v5-3.png)](<https://hex-rays.com/hubfs/freefile/igor-tip-of-the-week-S02.pdf>)

[ Download Season 2](</hubfs/freefile/igor-tip-of-the-week-S02.pdf>)

---

## 7. 

**Date:** Posted: Aug 29, 2022  
**Link:** [https://hex-rays.com/blog/ida-8-0-service-pack-1-released](https://hex-rays.com/blog/ida-8-0-service-pack-1-released)

[Back](<https://hex-rays.com/blog>)

[IDA](<https://hex-rays.com/blog/tag/ida>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.0 Service Pack 1 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-0-service-pack-1-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-0-service-pack-1-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-0-service-pack-1-released>)

Geoffrey Czokow ✦ Posted: Aug 29, 2022

![IDA 8.0 Service Pack 1 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

IDA Service Pack 1 (SP1) for IDA 8.0 is now available.

This new release fixes a few issues that might have affected some users.

### How to request the new versions

All new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively, you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to [support@hex-rays.com](<mailto:support@hex-rays.com>).

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist](</products/ida/news/8_0sp1/>) [Get a quote](</cgi-bin/quote.cgi>)

---

## 8. 

**Date:** Posted: Aug 26, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-104-immediate-search](https://hex-rays.com/blog/igors-tip-of-the-week-104-immediate-search)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #104: Immediate search

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-104-immediate-search>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-104-immediate-search>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-104-immediate-search>)

I 

Igor Skochinsky ✦ Posted: Aug 26, 2022

![Igor’s tip of the week #104: Immediate search](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Immediate search is one of three main [search types](<https://hex-rays.com/blog/igors-tip-of-the-week-48-searching-in-ida/>) available in IDA. While not that known, it can be very useful in some situations. Here are some examples.

### Unique (magic) constants

If you know some unique constants used by the program, looking for them can let you narrow down the range of code you have to analyze. For example, if a program reports a numerical error code, you could look for it to find the possible locations which may be returning this error.

### Undiscovered cross-references in RISC processors

Many RISC processors use fixed-width instructions which does not leave enough space for encoding full address values in the instruction. Thus they have to resort to building address values out of small pieces. For, example, in SPARC, loading of a 32-bit value has to be done as a [pair of instructions](<https://arcb.csc.ncsu.edu/~mueller/codeopt/codeopt00/notes/sparc.html>):
    
    
    sethi %hi(Prompt),%o1
    or    %o1,%lo(Prompt),%o1

Where `%hi` returns top 22 bits of the value and `%lo` returns the low 10 bits. Because such instructions may be not immediately next to each other, IDA may fail to “connect” them and recover the full 32-bit value, leading to missing cross references. So if you have, for example, a string constant at address `N`, which you think should be referenced from somewhere, doing an immediate search for `N&0x3FF` should produce a list of potential candidates for instructions referring to that address.

## Structure field references

Sometimes you may have a structure with a field at a specific offset which is pretty unique (not a small or round value) and want to find where it is used in the program. For example, let’s look at a recent Windows kernel and the structure `_KPRCB`. At offset 63Eh, it has a field `CoresPerPhysicalProcessor:`

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/immsearch1-3.png?width=632&height=542&name=immsearch1-3.png)

How to find where it is used? Searching for the value 0x63e gives a list of instructions using that value.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/immsearch2-3.png?width=257&height=271&name=immsearch2-3.png)   
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/immsearch3-3.png?width=726&height=429&name=immsearch3-3.png)

You can then inspect these instructions and see if they indeed reference the `_KPRCB` field and not something else.

This is probably one of the best uses for immediate search but it does not replace manual analysis. For example:

  1. it may miss references which do not use the value directly but calculate it one way or another;
  2. false positives may happen, especially for common or small values
  3. the field may be referenced indirectly via a bigger containing structure (e.g. `_KPCR` includes `_KPRCB` as a member, so references from `_KPCR` will have an additional offset).



See also:

[IDA Help: Search for next instruction/data with the specified operand](<https://www.hex-rays.com/products/ida/support/idadoc/574.shtml>)

---

## 9. 

**Date:** Posted: Aug 19, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-103-sharing-plugins-between-ida-installs](https://hex-rays.com/blog/igors-tip-of-the-week-103-sharing-plugins-between-ida-installs)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [plugin](<https://hex-rays.com/blog/tag/plugin>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #103: Sharing plugins between IDA installs

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-103-sharing-plugins-between-ida-installs>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-103-sharing-plugins-between-ida-installs>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-103-sharing-plugins-between-ida-installs>)

I 

Igor Skochinsky ✦ Posted: Aug 19, 2022

![Igor’s tip of the week #103: Sharing plugins between IDA installs](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

As of the time of writing, IDA does not have a built-in plugin manager, so third-party plugins have to be installed manually.

### Installing into IDA directory

The standard location for IDA plugins is the `plugins` directory in IDA’s installation (for example, `C:\Program Files\IDA Pro 8.0\plugins` on Windows). So this is the most common way of installing them — just copy the plugin file(s) there and they’ll be loaded on next start of IDA. However, this only makes them available for this specific IDA install. If you install a new version of IDA (which by default uses a version-specific directory name), you’ll need to re-copy plugins to the new location.

### Installing into user directory

In addition to IDA’s own directory, IDA also checks for plugins in the [user directory](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>). So you can put them in:

  * `%APPDATA%\Hex-Rays\IDA Pro\plugins` on Windows
  * `$HOME/.idapro/plugins` on Linux/Mac



You can find out the exact path for your system by executing `idaapi.get_ida_subdirs("plugins")` in IDA.

Such plugins will be loaded by any IDA, so there may be issues if they use functionality which is not available or changed between versions, but the advantage is that there’s no need to reinstall them when upgrading IDA (or using multiple versions).

See also:

[Igor’s tip of the week #33: IDA’s user directory (IDAUSR)](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>)  
[IDA Help: Environment variables](<https://www.hex-rays.com/products/ida/support/idadoc/1375.shtml>)

---

