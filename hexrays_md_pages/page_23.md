# Hex-Rays Blog – Page 23

**Archived:** 2026-02-20 12:17
**URL:** https://hex-rays.com/blog/page/23

---

## 1. 

**Date:** Posted: May 20, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-90-suspicious-operand-limits](https://hex-rays.com/blog/igors-tip-of-the-week-90-suspicious-operand-limits)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #90: Suspicious operand limits

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-90-suspicious-operand-limits>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-90-suspicious-operand-limits>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-90-suspicious-operand-limits>)

I 

Igor Skochinsky ✦ Posted: May 20, 2022

![Igor’s tip of the week #90: Suspicious operand limits](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Although in general case the problem of correct disassembly is unsolvable, in practice it can get pretty close. IDA uses various heuristics to improve the disassembly and make it more readable, such as converting numerical values to offsets when it “looks plausible”. However, this is not always reliable or successful and it may miss some. To help you improve things manually, in some cases IDA can give you a hint.

### Suspiciousness Limits

In IDA’s Options dialog on the Disassembly tab, there are two fields: _Low suspiciousness limit_ and _High suspiciousness limit_. What do they mean?

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/suspicious1-3.png?width=584&height=511&name=suspicious1-3.png)

Whenever IDA outputs an instruction operand with the numerical value in that range, and it does not yet have an explicitly set type (i.e. it has the default AKA _void_ type), it will use a special color (orange in the default color scheme):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/suspicious2-3.png?width=406&height=148&name=suspicious2-3.png)

In such situation, you could, for example, [hover your mouse](<https://hex-rays.com/blog/igors-tip-of-the-week-47-hints-in-ida/>) over the value to see if the target looks like a valid destination, and convert it to an offset either using a hotkey (O) or via the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/suspicious3-3.png?width=401&height=190&name=suspicious3-3.png)

### Changing the Suspiciousness Limits

Initial values of the limits are taken from the input file’s loaded address range. If the valid address range changes (for example, if you rebase the database or create additional segments), it may make sense to update the ranges so you can see more of potential addresses. Conversely, you can also change the values to exclude some ranges which are unlikely to be valid addresses to reduce the false positives.

See also: [IDA Help: Low & High Suspicious Operand Limits](<https://www.hex-rays.com/products/ida/support/idadoc/606.shtml>)

---

## 2. 

**Date:** Posted: May 13, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-89-en-masse-operations](https://hex-rays.com/blog/igors-tip-of-the-week-89-en-masse-operations)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #89: En masse operations

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-89-en-masse-operations>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-89-en-masse-operations>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-89-en-masse-operations>)

I 

Igor Skochinsky ✦ Posted: May 13, 2022

![Igor’s tip of the week #89: En masse operations](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Last time we used operand types to make a function more readable and understand its behavior better. Converting operands one by one is fine if you need to do it a few times, but can quickly get tedious if you need to do it for a long piece of code.

### En masse operation

To convert operands of several instruction at once, [select them](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>) before triggering the operation (either using the corresponding hotkey (e.g. `R`), or from the Edit > Operand type menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/optype_menu-3.png?width=634&height=160&name=optype_menu-3.png)

If you have a selection when triggering one of these actions, it won’t be performed immediately but another dialog will pop up first:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enmasse_dlg-3.png?width=257&height=399&name=enmasse_dlg-3.png)

Here, you can tell IDA which operands you want to actually convert. The following options are available:

  * All operands: all operands of selected instructions will be converted to the selected type (or back to the default/number type if they already had the chosen type);
  * Operand value range: only operands with values between _Lower value_ and _Upper value_ below will be converted. For example, you could enter ‘0x20’ and ‘0x7F’ to have IDA only consider single ASCII characters like the last example from the [previous post](<https://hex-rays.com/blog/igors-tip-of-the-week-88-character-operand-type-and-stack-strings/>);
  * <type> operands: only convert operands which already have the selected type (they will be converted back to the default/number type);
  * Not <type> operands: only convert operands not already having the selected type. Both untyped and having another type (e.g. decimal/enum/offset) operands will be converted to the desired type;
  * Not typed operands: only convert operands not assigned a specific type (default/number). All operands already having an assigned type will be left as is.



P.S. you can use this feature not only with instructions but also data. For example, for converting several separate integers in the data section to decimal or octal. In such case, the ‘operands’ will be the data items.

See also: [IDA Help: Perform en masse operation](<https://hex-rays.com/products/ida/support/idadoc/459.shtml>)

---

## 3. 

**Date:** Posted: May 10, 2022  
**Link:** [https://hex-rays.com/blog/ida-teams-beta-release](https://hex-rays.com/blog/ida-teams-beta-release)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Teams beta release

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-teams-beta-release>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-teams-beta-release>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-teams-beta-release>)

Geoffrey Czokow ✦ Posted: May 10, 2022

![IDA Teams beta release](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

As software systems becomes more complex, we at Hex-Rays have witnessed a growing desire for more **collaboration** in the field of reverse-engineering: applying multiple sets of eyes and diversified skill sets, will be a boon for speeding up that process.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ida_teams_logo-Jun-18-2024-08-52-15-4045-AM.png?width=200&name=ida_teams_logo-Jun-18-2024-08-52-15-4045-AM.png)

Over the last months/years, we have been busy cooking an appropriate response to that demand, and we’re approaching the time to go public about it.

Today, the Hex-Rays team is proud to announce the release of the [**beta version of IDA Teams**](<https://hex-rays.com/ida-teams>): a collaborative tool for teams of reverse-engineers.

Inspired by processes (and tools) that are now ubiquitous in everyday traditional software engineering, **IDA Teams** brings the same power and control to the process of software reverse engineering.

Let teams collaborate, keep a history of changes made to projects, view their nature, merge conflicts seamlessly, … and this is only the beginning! (we have a few more ideas)

If you are interested or want more information, visit <https://hex-rays.com/ida-teams>

---

## 4. 

**Date:** Posted: May 6, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-88-character-operand-type-and-stack-strings](https://hex-rays.com/blog/igors-tip-of-the-week-88-character-operand-type-and-stack-strings)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #88: Character operand type and stack strings

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-88-character-operand-type-and-stack-strings>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-88-character-operand-type-and-stack-strings>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-88-character-operand-type-and-stack-strings>)

I 

Igor Skochinsky ✦ Posted: May 6, 2022

![Igor’s tip of the week #88: Character operand type and stack strings](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

We’ve mentioned [operand representation](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>) before but today we’ll use a specific one to find the Easter egg hidden in the [post #85](<https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging/>).

More specifically, it was this screenshot:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/srcdbg1-Jun-18-2024-09-07-47-2120-AM.png)

The function `surprise` calls `printf`, but the arguments being passed to it seem to all be numbers. Doesn’t `printf()` usually work with strings? What’s going on?

### Numbers and characters

As you probably know, computers do not actually distinguish numbers from characters – to them they’re all just a set of bits. So it’s all a matter of _interpretation_ or _representation_. For example, all of the following are represented by the same bit pattern:

  1. `65` (decimal number)
  2. `0x41`, `41h`, `H'41` (hexadecimal number)
  3. `0101` or `101o` (octal number)
  4. `1000001b` or `0b1000001` (binary number)
  5. `'A'` (ASCII character)
  6. `WM_COMPACTING` (Win32 API constant)
  7. (and many other variations)



### Character operand representation

In fact, listing in the screenshot has been modified from the defaults to make the Easter egg less obvious. Here’s the original version as text:
    
    
    .text:00401010 ; int surprise(...)
    .text:00401010 _surprise proc near                     ; CODE XREF: _main↑p
    .text:00401010
    .text:00401010 var_24= dword ptr -24h
    .text:00401010 var_20= dword ptr -20h
    .text:00401010 _Format= byte ptr -1Ch
    .text:00401010 var_18= dword ptr -18h
    .text:00401010 var_14= dword ptr -14h
    .text:00401010 var_10= dword ptr -10h
    .text:00401010 var_C= dword ptr -0Ch
    .text:00401010 var_8= dword ptr -8
    .text:00401010 var_4= dword ptr -4
    .text:00401010
    .text:00401010 sub     esp, 24h
    .text:00401013 mov     eax, ___security_cookie
    .text:00401018 xor     eax, esp
    .text:0040101A mov     [esp+24h+var_4], eax
    .text:0040101E lea     eax, [esp+24h+var_24]
    .text:00401021 mov     dword ptr [esp+24h+_Format], 70747468h
    .text:00401029 push    eax
    .text:0040102A lea     eax, [esp+28h+_Format]
    .text:0040102E mov     [esp+28h+var_18], 2F2F3A73h
    .text:00401036 push    eax                             ; _Format
    .text:00401037 mov     [esp+2Ch+var_14], 2D786568h
    .text:0040103F mov     [esp+2Ch+var_10], 73796172h
    .text:00401047 mov     [esp+2Ch+var_C], 6D6F632Eh
    .text:0040104F mov     [esp+2Ch+var_8], 73252Fh
    .text:00401057 mov     [esp+2Ch+var_24], 74736165h
    .text:0040105F mov     [esp+2Ch+var_20], 7265h
    .text:00401067 call    _printf
    .text:0040106C mov     ecx, [esp+2Ch+var_4]
    .text:00401070 add     esp, 8
    .text:00401073 xor     ecx, esp                        ; StackCookie
    .text:00401075 xor     eax, eax
    .text:00401077 call    @__security_check_cookie@4      ; __security_check_cookie(x)
    .text:0040107C add     esp, 24h
    .text:0040107F retn
    .text:0040107F _surprise endp
    

In hexadecimal it’s almost immediately obvious: the “numbers” are actually short fragments of ASCII text. The code is building strings on the stack piece by piece. This can be made more explicit by converting numbers to the _character_ operand type (shortcut `R`).

To help you decide whether such operand type makes sense, IDA shows a preview in the context menu:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/charop1-3.png?width=428&height=405&name=charop1-3.png)

This way it’s pretty clear that the “number” is actually a text fragment. After converting all “numbers” to character constant, a pattern begins to emerge:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/charop2-3.png?width=481&height=375&name=charop2-3.png)

Due to the little-endian memory organization of the x86 processor family, the individual fragments have to be read backwards (i.e. character literal `'ptth'` corresponds to the string fragment `"http"`).

### Decompiler and optimized string operations

Now it’s almost obvious what the result is supposed to be but there’s in fact an even easier way to discover it.

Because the approach of processing short strings in register-sized chunks is often used by compilers to implement common C runtime functions inline instead of calling the library function, the decompiler uses heuristics to detect such code patterns and show them as equivalent function calls again. If we decompile this function, the decompiler reassembles the strings and shows them as if they were like that in the pseudocode:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/charop3-3.png?width=354&height=174&name=charop3-3.png)

### Stack strings

Malware often uses a similar approach of building strings by small pieces (most often character by character) on the stack because this way the complete string does not appear in the binary and can’t be discovered by simply searching for it. Thanks to the automatic comments shown by IDA for operands not having explicitly assigned type, they are usually obvious in the disassembly:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/charop4-3.png?width=463&height=599&name=charop4-3.png)

And the decompiler easily recovers the complete string:
    
    
    void __noreturn start()
    {
      char v0[36]; // [esp+0h] [ebp-28h] BYREF  
      qmemcpy(v0, "FLAG{STACK-STRINGS-ARE-BEST-STRINGS}", sizeof(v0));
      [...]
     }
    

P.S. If you want to play with the Easter egg binary and reproduce the results in this post, download it here:[easter2022.zip](<https://hex-rays.com/wp-content/uploads/2022/05/easter2022.zip>)

---

## 5. 

**Date:** Posted: Apr 29, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-87-function-chunks-and-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-87-function-chunks-and-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #87: Function chunks and the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-87-function-chunks-and-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-87-function-chunks-and-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-87-function-chunks-and-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Apr 29, 2022

![Igor’s tip of the week #87: Function chunks and the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We’ve covered function chunks [last week](<https://hex-rays.com/blog/igors-tip-of-the-week-86-function-chunks/>) and today we’ll show an example of how to use them in practice to handle a common compiler optimization.

### Shared function tail optimization

When working with some ARM firmware, you may sometimes run into the following situation:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_decompiler1-3.png?width=1231&height=577&name=chunk_decompiler1-3.png)

We have decompilation of `sub_8098C` which ends with a strange `JUMPOUT` statement and if we look at the disassembly, we can see that it corresponds to a branch to a `POP.W` instruction in **another function** (`sub_8092C`). What happened here?

This is an example of a code size optimization. The `POP.W` instruction is 4 bytes long, while the `B` branch is only two, so by reusing it the compiler saves two bytes. It may not sound like much, but such savings can accumulate to something substantial over all functions of the binary. Also, sometimes longer sequences of several instructions may be reused, leading to bigger savings.

Can we fix the database to get clean decompilation and get rid of `JUMPOUT`? Of course, the answer is yes, but the specific steps may be not too obvious, so let’s describe some approaches.

### Creating a chunk for the shared tail instructions

First we need to create a chunk for the shared instructions (in our example, the `POP.W` instruction). A chunk can be created only from instructions which do not yet belong to any function, thus the easiest way is to delete the function so that instructions become “free”. This can be done either from the Functions window, via Edit > Functions > Delete function menu entry, or from the modal “jump to function” list (`Ctrl`–`P`, `Del`).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_decompiler2-3.png?width=624&height=155&name=chunk_decompiler2-3.png)

Once deleted, the shared tail instructions can be added as a chunk to the other function. This can be done manually:

  1. select the instruction(s),
  2. invoke Edit > Functions > Append function tail…
  3. pick the referencing function (in our case, `sub_8098C`). Normally IDA should suggest it automatically.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_decompiler3-3.png?width=918&height=173&name=chunk_decompiler3-3.png)

Or (semi)automatically:

  1. jump to the referencing branch (e.g. by double-clicking the `CODE XREF: sub_8098C+3E↓j` comment)
  2. [reanalyze](<https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis/>) the branch (press `C`). IDA will detect that execution continues outside the current function bounds and automatically create and add the chunk for the shared tail instructions.



Either solution will create the chunk and mark it as belonging to the referencing function.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_decompiler4-3.png?width=620&height=74&name=chunk_decompiler4-3.png)

We can check that it is contained in the function graph:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_decompiler5-3.png?width=552&height=278&name=chunk_decompiler5-3.png)

And the pseudocode no longer has a JUMPOUT:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_decompiler6-3.png?width=521&height=365&name=chunk_decompiler6-3.png)

### Attaching the chunk to the original function

We “solved” the problem for one function, but in the process we’ve destroyed the function which contained the shared tail. If we need to decompile it too, we can try to recreate it:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_recreate1-3.png?width=645&height=389&name=chunk_recreate1-3.png)

However, IDA ends it before the chunk, because it’s now a part of another function:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_recreate2-3.png?width=654&height=421&name=chunk_recreate2-3.png)

And if we decompile it, we get the same `JUMPOUT` issue:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_recreate3-3.png?width=456&height=217&name=chunk_recreate3-3.png)

The solution is simple: as mentioned in the previous post, a chunk may belong to multiple functions, so we just need to attach the chunk to this function too:

  1. Select the instructions of the tail;
  2. invoke Edit > Functions > Append function tail…
  3. select the recreated function (in our example, `sub_8092C`).



The chunk gains one more owner, appears in the function graph, and the decompilation is fixed:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_recreate4-3.png?width=975&height=548&name=chunk_recreate4-3.png)

### Complex situations

The above example had a tail shared by two functions, but of course this is not the limit. Consider this example:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chunk_decompiler7-3.png?width=700&height=453&name=chunk_decompiler7-3.png)

Here, the `POP.W` instruction is shared by seven functions, and two of them also reuse the `ADD SP, SP, #0x10` instruction preceding it. There is also a chunk which belongs only to one function but it had to be separated because the function was no longer contiguous. Still, IDA’s approach to fragmented functions was flexible enough to handle it with some manual help and all involved functions have proper control flow graphs and nice decompilation.

To summarize, the suggested algorithm of handling shared tail optimization is as follows:

  1. Delete the function containing the shared tail instructions. 
  2. Attach the shared tail instructions to the other function(s) (manually or by reanalyzing the branches to the tail)
  3. Recreate the deleted function and attach the shared tail(s) to it too.

---

## 6. 

**Date:** Posted: Apr 22, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-86-function-chunks](https://hex-rays.com/blog/igors-tip-of-the-week-86-function-chunks)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #86: Function chunks

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-86-function-chunks>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-86-function-chunks>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-86-function-chunks>)

I 

Igor Skochinsky ✦ Posted: Apr 22, 2022

![Igor’s tip of the week #86: Function chunks](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In IDA, _function_ is a sequence of instructions grouped together. Usually it corresponds to a high-level function or [_subroutine_](<https://en.wikipedia.org/wiki/Subroutine>):

  1. it can be _called_ from other places in the program, usually using a dedicated processor instruction;
  2. it has an _entry_ and one or more _exits_ (instruction(s) which return to the caller);
  3. it can accept _arguments_ (in registers or on the stack) and optionally return values;
  4. it can use local (stack) variables



However, IDA’s functions can group any arbitrary sequence of instructions, even those not matching the above criteria. The only hard requirement is that the function must start with a valid instruction.

### Creating functions

IDA usually creates functions automatically, based on the call instructions or debug information, but they can also be created manually using the Create Function action (under Edit > Functions or from context menu), or `P` shortcut. This can be done only for instructions not already belonging to functions. By default IDA follows the cross-references and tries to determine the function boundaries automatically, but you can also [select](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>) a range beforehand to force creation of a function, for example, if there are some invalid instructions or embedded data.

### Function range

In the most common case, a function occupies a contiguous address range, from the entry to the last return instruction. This is the start and end address specified in function properties available via the Edit Function dialog (`Alt`–`P`).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/func_range-3.png?width=490&height=332&name=func_range-3.png)

### Chunked functions

A single-range function is not the only option supported by IDA. In real-life programs, a function may be split into several disjoint ranges. For example, this may happen as a result of [profile-guided optimization](<https://devblogs.microsoft.com/cppblog/profile-guided-optimization-pgo-under-the-hood/>), which can put cold (rarely executed) basic blocks into a separate part of binary from hot (often executed) ones. In IDA, such functions are considered to consist of multiple _chunks_ (each chunk being a single contiguous range of instructions). The chunk containing the function entry is known as _entry chunk_ , while the others are called _tail chunks_ or simply _tails_.

In disassembly view, the functions which have additional chunks have additional annotations near the function’s entry, listing the tail chunks which belong to the function.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/func_chunk1-3.png?width=622&height=257&name=func_chunk1-3.png)

The tail chunks themselves are marked with “START OF FUNCTION CHUNK” and “END OF FUNCTION CHUNK” annotations, mentioning which function they belong to. This is mostly useful in text view, as in the graph view they are displayed as part of the overall function graph.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/func_chunk2-3.png?width=1001&height=293&name=func_chunk2-3.png)

Sometimes a tail chunk may be shared by multiple functions. In that case, one of them is designated _tail owner_ and others are considered _additional parents_. Such chunk will appear in the function graph for every function it belongs to.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/func_chunk3-3.png?width=687&height=159&name=func_chunk3-3.png)

### Managing chunks manually

Usually IDA handles chunked functions automatically, either detecting them during autoanalysis or by making use of other function range metadata (such as `.pdata` function descriptors in x64 PE files, or debug information). However, there may be situations where you need to add or remove chunks manually, for example to fix a false positive or handle an unusual compiler optimization.

To remove (detach) a tail chunk, position cursor inside it and invoke Edit > Functions > Remove function tail. If the tail has only one owner, it will be removed immediately and converted to standalone instructions (not belonging to any function). If it has multiple owners, IDA will offer you to choose from which function(s) it should be detached.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/func_chunk4-3.png?width=742&height=231&name=func_chunk4-3.png)

To add a range of instructions as a tail to a function, select the range and invoke Edit > Functions > Append function tail, then select a function to which it should be added. This can be done multiple times to attach a tail to several functions (whole tail must be selected again in such case).

More info:

[IDA Help: Append Function Tail](<https://www.hex-rays.com/products/ida/support/idadoc/687.shtml>)

[IDA Help: Remove Function Tail](<https://www.hex-rays.com/products/ida/support/idadoc/688.shtml>)

---

## 7. 

**Date:** Posted: Apr 15, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging](https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [debugger](<https://hex-rays.com/blog/tag/debugger>)

# Igor’s tip of the week #85: Source-level debugging

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-85-source-level-debugging>)

I 

Igor Skochinsky ✦ Posted: Apr 15, 2022

![Igor’s tip of the week #85: Source-level debugging](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Although IDA has been created first and foremost to analyze binaries in “black box” mode, i.e. without any symbols or debug information, it does have the ability to [consume such information when available](<https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols/>).

The debugger functionality was also initially optimized to debug binaries on the assembly level, but nowadays can work with source code too.

### Source-level debugging

Source-level debugging is enabled by default but can be turned off or on manually via Debugger > Use source-level debugging menu item, or the button on the Debug toolbar.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_toolbar-3.png?width=429&height=58&name=srcdbg_toolbar-3.png)

If the input file has debugging info in the format supported by IDA (e.g. PDB or DWARF), it will be automatically used when the debugging starts and the code being executed is covered by the debug info.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_log-3.png?width=768&height=172&name=srcdbg_log-3.png)

### Source code files

If source files are present in the original locations, IDA will open them in separate source view windows and highlight the currently executing line. The assembly instructions are still shown in the IDA View, and you can continue to use it for analysis independently of source code view. Note that IDA may automatically switch to disassembly when stepping through instructions which do not have a correspondence in the source code (for example, compiler helper functions, or auxiliary code such as prolog or epilog instructions).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg1-Jun-18-2024-08-59-52-6707-AM.png?width=1283&height=485&name=srcdbg1-Jun-18-2024-08-59-52-6707-AM.png)

When, after stepping through disassembly, the execution returns to the area covered by the source code, you can ask IDA to show the corresponding source code again via the action Debugger > Switch to source (also available in context menu and toolbar). This action can be used even for code away from the current execution point.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg2-3.png?width=1167&height=445&name=srcdbg2-3.png)

### Source path mappings

Sometimes the source code corresponding to the binary may be available but in a different location from what is recorded in the debug info (e.g. you may be debugging the binary on a different machine or even remotely from a different OS). The files which were not found in expected location are printed in Output window:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_notfound-3.png?width=842&height=79&name=srcdbg_notfound-3.png)

Using Options > Source paths…, you can set up mappings for IDA to find the source files in new locations.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_paths-3.png?width=995&height=591&name=srcdbg_paths-3.png)

### Locals 

When the debug info includes information about local variables, IDA can use it to show their values. A short, one-line version is shown when you hover mouse over the variables in source code view.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_hint-3.png?width=578&height=60&name=srcdbg_hint-3.png)

For more complex objects it may be more convenient to open the dedicated view where you can expand and inspect fields and sub-objects. This view is available via the menu Debugger > Debugger windows > Locals.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_locals-3.png?width=1341&height=424&name=srcdbg_locals-3.png)

### Watches

In addition to locals, you can also watch only specific variables you need instead of all locals, or values of global variables. This view can be opened via Debugger > Debugger windows > Watch view. Variables can be added using `Ins` or context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_watches-3.png?width=670&height=302&name=srcdbg_watches-3.png)

### Debugging pseudocode

Even if you don’t have debug info for the code you’re debugging but do have a decompiler, it is possible to debug pseudocode as if it were a source file. IDA will automatically use pseudocode if source-level debugging is enabled but there is no debug info for the specific code fragment you’re stepping through. You can also always switch to pseudocode during debugging using the usual `Tab` hotkey. Locals, Watches, and source-level breakpoints are available when debugging pseudocode in the same way as with “real” source code.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/srcdbg_pseudocode-3.png?width=1339&height=460&name=srcdbg_pseudocode-3.png)

P.S. attentive reader may discover an additional surprise in this post. Happy Easter! 😉

---

## 8. 

**Date:** Posted: Apr 8, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-84-array-indexes](https://hex-rays.com/blog/igors-tip-of-the-week-84-array-indexes)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #84: Array indexes

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-84-array-indexes>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-84-array-indexes>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-84-array-indexes>)

I 

Igor Skochinsky ✦ Posted: Apr 8, 2022

![Igor’s tip of the week #84: Array indexes](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

We’ve covered arrays [previously](<https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays/>), but one feature briefly mentioned there is worth a separate highlight.

Complex programs may use arrays of data, either of items such as integers or floats, or of complex items such as structures. When the arrays are small, it’s not too difficult to make sense of them, but what to do if your task requires, for example, to find the value of the item #567 in a 3000-item array?

You can of course try to count the items manually or copy the array into a text editor ([Export Data](<https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data/>) can help here) and import into a spreadsheet but there are ways to do it inside IDA without too much trouble.

### Resizing the array

Let’s say we have an array of 88 items:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrayidx1-3.png?width=818&height=107&name=arrayidx1-3.png)

and we need the item #25. Manual counting is possible but tedious, especially because we need to account for the repeated items in the `dup` expressions. There is a different approach to solve this. Because the items are counted from 0 and we have 88 of them, the last one has index 87. To make it so that the last item is number 25, we can resize the array to 26(25+1) items. For this, press `*` to open the array parameters dialog and change the _Array size_ field:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrayidx2-3.png?width=396&height=442&name=arrayidx2-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrayidx3-3.png?width=811&height=44&name=arrayidx3-3.png)

Now the array contains 26 items from #0 to #25 so we can see that the item we needed has the value `35h`.

### Array index display

Alternatively, we can enable the _Display indexes_ option.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrayidx4-3.png?width=396&height=442&name=arrayidx4-3.png)

With the option on, index of the first element is displayed as a comment for each line:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrayidx5-3.png?width=787&height=108&name=arrayidx5-3.png)

While still not very obvious, it is a little easier to find the necessary element by counting from the start of a line. You can also set the _Items on a line_ value to 1 or another small value so that each line contains fewer elements and it’s easier to find the necessary one.

### Indexes and arrays of structures

When you have an array of structures and they can be displayed in [terse form](<https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing/>), the indexes are printed for each line similarly to the array of simple values.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrayidx6-3.png?width=442&height=96&name=arrayidx6-3.png)

However, if you unhide/uncollapse the array to show the structs in verbose form, each field gets a comment with an array notation:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/arrayidx7-3.png?width=466&height=272&name=arrayidx7-3.png)

See also: [IDA Help: Convert to array](<https://www.hex-rays.com/products/ida/support/idadoc/455.shtml>)

---

## 9. 

**Date:** Posted: Apr 1, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-83-decompiler-options-default-radix](https://hex-rays.com/blog/igors-tip-of-the-week-83-decompiler-options-default-radix)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #83: Decompiler options: default radix

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-83-decompiler-options-default-radix>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-83-decompiler-options-default-radix>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-83-decompiler-options-default-radix>)

I 

Igor Skochinsky ✦ Posted: Apr 1, 2022

![Igor’s tip of the week #83: Decompiler options: default radix](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We’ve covered the major pseudocode formatting options previously but there is one more option which can influence the output. It is the _radix_ used for printing numbers in the pseudocode.

> In a positional numeral system, the **radix** or **base** is the number of unique digits, including the digit zero, used to represent numbers. For example, for the decimal/denary system (the most common system in use today) the radix (base number) is ten, because it uses the ten digits from 0 through 9. 

(from [Wikipedia](<https://en.wikipedia.org/wiki/Radix>))

### Automatic radix

The default radix setting is 0, which means “automatic”.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_radix0-3.png?width=497&height=357&name=hr_radix0-3.png)

With this setting, the decompiler uses hexadecimal radix for values detected as unsigned, and decimal otherwise. For example, in the below screenshot, arguments to `CContextMenuManager::AddMenu()` are shown in hex because the function prototype specifies the last argument type as “unsigned int”, while those for `LoadStringA()` are in decimal because the decompiler used a guessed prototype with the type [`_DWORD`](<https://hex-rays.com/blog/igors-tip-of-the-week-45-decompiler-types/>) which behaves like a signed type.![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_radix1-3.png?width=771&height=321&name=hr_radix1-3.png)

### “Nice” numbers

In some cases, the decompiler may use hex even for signed numbers if it makes the number look “nice”. Currently (as of IDA 7.7), the following rules are used:

  1. values matching 2n and 2n-1 (typical bitmasks) are printed as hexadecimal.
  2. 64-bit values which have not all-zero or all-one high 32 bits are printed as hexadecimal unless they end with more than 3 zeroes in decimal representation.
  3. -1 is printed as decimal.



### Changing the radix manually

You can always change the representation for a specific number in pseudocode from the context menu or via a hotkey.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_radix2-3.png?width=483&height=279&name=hr_radix2-3.png)

To toggle between decimal and hex, use the `H` hotkey. Octal is available only via the context menu by default, but it’s possible to add a [custom hotkey](<https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>) for the action name `hx:Oct`.

### Setting preferred radix

By changing _Default radix_ in the decompiler options, you can have decompiler always use decimal (10) or hexadecimal(16) for all numbers without an explicitly set radix. Note that in this case the “nice” number detection will be disabled.

To change the default for all new databases, set the value **DEFAULT_RADIX** in `hexrays.cfg` as described in the [previous post](<https://hex-rays.com/blog/igors-tip-of-the-week-82-decompiler-options-pseudocode-formatting/>).

More info: [Configuration (Hex-Rays Decompiler User Manual)](<https://www.hex-rays.com/products/decompiler/manual/config.shtml>)

---

