# Hex-Rays Blog – Page 19

**Archived:** 2026-02-20 12:15
**URL:** https://hex-rays.com/blog/page/19

---

## 1. 

**Date:** Posted: Nov 4, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-113-image-relative-offsets-rva](https://hex-rays.com/blog/igors-tip-of-the-week-113-image-relative-offsets-rva)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #113: Image-relative Offsets (RVA)

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-113-image-relative-offsets-rva>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-113-image-relative-offsets-rva>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-113-image-relative-offsets-rva>)

I 

Igor Skochinsky ✦ Posted: Nov 4, 2022

![Igor’s Tip of the Week #113: Image-relative Offsets \(RVA\)](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Image-relative offsets are values that represent an offset from the _image base_ of the current module (image) in memory. This means that they can be used to refer to other locations in the same module regardless of its real, final load address, and thus can be used to make the code position-independent (PIC), similarly to the [self-relative offsets](<https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets/>). The alternative name RVA means “Relative virtual address” and is often used in the context of the PE file format.

However, PIC is not the only advantage of RVAs. For example, on x64-bit platforms RVA values usually use 32 bits instead of 64 like a full pointer. While this makes their range more limited (4GiB from imagebase), the savings from pointer-type values can be substantial when accumulated over the whole binary.

For known RVA values, such as those in the PE headers or EH structures, IDA can usually convert them to an assembler-specific expression automatically:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rva1-3.png?width=611&height=165&name=rva1-3.png)

However, sometimes there may be a need to do it manually, for example, when dealing with another update of the file format not yet handled by IDA, or a custom format/structure which uses RVAs for addressing. In that case, you can use yet another variation of the [User-defined offset](<https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base/>). The option to turn on is _Use image base as offset base_. When it’s enabled, IDA will ignore the entered offset base and will always use the imagebase.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rva2-3.png?width=331&height=575&name=rva2-3.png)

However, even if you use this approach in a 64-bit program, you may fail to reach the desired effect: the value will be displayed in red to indicate an error and not show a nice expression with the final address, as expected.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rva3-3.png?width=140&height=62&name=rva3-3.png)

This happens because the command defaults to OFF32 for 32-bit values, but the final address does not fit into 32 bits. The fix is simple: select OFF64 instead of OFF32.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rva4-3.png?width=331&height=575&name=rva4-3.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rva5-3.png?width=488&height=361&name=rva5-3.png)

NOTE: for ARM binaries, the `imagerel` keyword is used instead of `rva`.

See also:

[Igor’s tip of the week #105: Offsets with custom base](<https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base/>)

[Igor’s tip of the week #110: Self-relative offsets](<https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets/>)

---

## 2. 

**Date:** Posted: Oct 28, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-112-matching-braces](https://hex-rays.com/blog/igors-tip-of-the-week-112-matching-braces)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #112: Matching braces

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-112-matching-braces>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-112-matching-braces>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-112-matching-braces>)

I 

Igor Skochinsky ✦ Posted: Oct 28, 2022

![Igor’s tip of the week #112: Matching braces](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

When working with big functions in the decompiler, it may be difficult to find what you need if the listing is long. While you can use [cross-references](<https://hex-rays.com/blog/igors-tip-of-the-week-18-decompiler-and-global-cross-references/>) to jump between uses of a variable or [collapse](<https://hex-rays.com/blog/igors-tip-of-the-week-100-collapsing-pseudocode-parts/>) parts of pseudocode to make it more compact, there is one simple shortcut which can make your life easier.

The shortcut is not currently (IDA 8.1) shown in the context menu, but it was mentioned in the [release notes for IDA 7.4](<https://hex-rays.com/products/ida/news/7_4/>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/braces1-3.png?width=1032&height=173&name=braces1-3.png)

You can also discover it by opening the Options > Shortcuts… dialog while the cursor is positioned on a brace or parenthesis:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/braces2-3.png?width=1032&height=543&name=braces2-3.png)

This dialog can also be used to modify the shortcut to something you may find more convenient, for example `Ctrl`–`]`

See also:

[Igor’s tip of the week #06: IDA Release notes – Hex Rays](<https://hex-rays.com/blog/igor-tip-of-the-week-06-release-notes/>)

[Igor’s tip of the week #02: IDA UI actions and where to find them – Hex Rays](<https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>)

---

## 3. 

**Date:** Posted: Oct 27, 2022  
**Link:** [https://hex-rays.com/blog/the-ida-patfind-plugin](https://hex-rays.com/blog/the-ida-patfind-plugin)

[Back](<https://hex-rays.com/blog>)

[IDA 8.0](<https://hex-rays.com/blog/tag/ida-8-0>) [plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA](<https://hex-rays.com/blog/tag/ida>) [firmware](<https://hex-rays.com/blog/tag/firmware>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [patfind](<https://hex-rays.com/blog/tag/patfind>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# The IDA patfind plugin

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/the-ida-patfind-plugin>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/the-ida-patfind-plugin>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/the-ida-patfind-plugin>)

A 

Alex Petrov ✦ Posted: Oct 27, 2022

![The IDA patfind plugin](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

**The IDA patfind plugin**

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patfind_without-3.png?width=700&height=611&name=patfind_without-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/patfind_without-3.png>)

_Just raw binary data at address 0x00000AC_

While IDA excels at extracting useful information from all sorts of binary files, it may happen that some unstructured binary files (e.g., firmwares, raw memory dumps, …) throw it off the rails, and the user needs to kickstart autoanalysis by figuring out some sort of entry point. To help with that, for decades now, IDA has had a feature called “code startup sequences,” useful to spot some binary patterns indicating the start of functions.

As it turns out, those lack flexibility and do not systematically produce the best results. That is what prompted one of the 2021 plugin contestants to come up with a plugin that would improve on that idea by providing stronger, richer, more powerful startup sequences. He called his plugin [**IDAPatternSearch**](<https://www.hex-rays.com/contests_details/contest2021/#IDAPatternSearch>) His idea was so good that it, in turn, inspired us at Hex-Rays to build upon it and improve it even further: this eventually became the **patfind** plugin. Thanks to **patfind** , that same binary file now looks like this (out of the box):

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/patfind_with-3.png?width=800&height=748&name=patfind_with-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/patfind_with-3.png>)

_Pattern found at address 0x00000AC and function created_

**patfind** was released as part of IDA 8.0 (<https://hex-rays.com/products/ida/news/8_0/#better-firmware-analysis-thanks-to-the-function-finder-plugin-patfind>)

**Configuration**

The **patfind** plugin will run automatically every time an unstructured binary file is loaded. There is also an option to run it manually from the menu ‘ _Edit- >Plugins->Find Functions_.’

The IDA **patfind** plugin can be configured with the `patfind.cfg`. There is not much to configure, really, just the place where to look for pattern files and whether the plugin should run automatically or not.

**In-depth**

Binary and binary-like files are difficult to analyze if you don’t know what part (if any) of the binary data contains code. The **patfind** plugin searches for places in the binary file where functions start and tell IDA to analyze the data as a function at the found addresses. That, in combination with the capabilities of IDA to check cross-references and follow function calls, may result in a fully analyzed file. Still, a lot depends on the quality of the provided patterns.

Every compiler has its own architecture-specific idioms when it comes to code generation. Corresponding patterns are defined in XML files. A very strict pattern may result in a perfect match, but if there’s a slight variation in the generated code, it will fail to match. On the other hand, a pattern that’s too loose will result in many results, but many of those will be false positives. The trick then is to find the right balance, using wild cards in patterns and different combinations of instructions or by checking the binary data just before where there might be a function to see if it matches the ending of a previous function, for example.

It is possible to add new architectures by simply adding a new XML file, just like the other XML files. It’s also possible to add, remove or change existing patterns for better matching.

The XML files used by **patfind** initially came from a Ghidra distribution and were augmented with a set of attributes that make them even more useful (e.g., automatic support for endianness).

---

## 4. 

**Date:** Posted: Oct 21, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-111-ida-keyboard-shortcuts-cheat-sheet](https://hex-rays.com/blog/igors-tip-of-the-week-111-ida-keyboard-shortcuts-cheat-sheet)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [keyboard shortcuts](<https://hex-rays.com/blog/tag/keyboard-shortcuts>)

# Igor’s tip of the week #111: IDA Keyboard Shortcuts cheat sheet

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-111-ida-keyboard-shortcuts-cheat-sheet>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-111-ida-keyboard-shortcuts-cheat-sheet>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-111-ida-keyboard-shortcuts-cheat-sheet>)

I 

Igor Skochinsky ✦ Posted: Oct 21, 2022

![Igor’s tip of the week #111: IDA Keyboard Shortcuts cheat sheet](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Many [keyboard shortcuts](<https://hex-rays.com/blog/tag/shortcuts/>) have been described on this blog, but they may be difficult to retain, especially if you don’t use them every day. To remedy that, we have been publishing a cheat sheet with the most common ones.

You can find it linked from our [documentation page](<https://hex-rays.com/documentation/>) in [HTML](<https://hex-rays.com/products/ida/support/idapro_cheatsheet.html>) or [PDF](<https://hex-rays.com/products/ida/support/freefiles/IDA_Pro_Shortcuts.pdf>) format.

NOTE: the shortcuts described are for the default configuration; you can [modify them](<https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>) to your liking.

See also:

[Igor’s tip of the week #01: Lesser-known keyboard shortcuts in IDA](<https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida/>)

[Igor’s tip of the week #02: IDA UI actions and where to find them](<https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>)

---

## 5. 

**Date:** Posted: Oct 20, 2022  
**Link:** [https://hex-rays.com/blog/halloween-challenge](https://hex-rays.com/blog/halloween-challenge)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Halloween Challenge](<https://hex-rays.com/blog/tag/halloween-challenge>) [Challenge](<https://hex-rays.com/blog/tag/challenge>) [Halloween](<https://hex-rays.com/blog/tag/halloween>) [Puzzle](<https://hex-rays.com/blog/tag/puzzle>)

# Halloween challenge

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/halloween-challenge>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/halloween-challenge>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/halloween-challenge>)

Geoffrey Czokow ✦ Posted: Oct 20, 2022

![Halloween challenge](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

## The Hex-Rays Halloween Challenge

It is almost Halloween, and this year we have decided to celebrate it with a challenge! Solve the challenge correctly, and if you’re quick enough to be within the first five, you will get an exclusive Halloween-themed IDA T-shirt.

**Before we continue, we would like to make you aware of some essential rules:**

  * This challenge is completely free and does not require a purchase of a product or service;
  * Everyone is welcome to participate;
  * The gifts are 5 (five) and will be given to the first 5 (five) participants who successfully solved the challenge;
  * Each participant can send more than one submission, but only one gift will be given even if the challenge is solved more than once. To be awarded, the submission with the correctly solved challenge should also adhere to p.3 above;
  * The deadline for participation is**31 October 23:59 (CET timezone)**. Submissions received after that date would not be considered;
  * The winners will be contacted directly by 2 November;
  * The gifts will be sent within a week after the announcement. The delivery is free of charge for the awardee. All winners should provide us with correct and precise shipping details. Should we not receive complete delivery information, we will consider the submission unsuccessful and will award the next participant in the list;
  * All personal data, including email addresses, names, and shipping details, will not be: 
    * Given to third parties;
    * Used for further communication;
    * Stored on our servers longer than the duration of the challenge.
  * Should you have any complaints or questions, please get in touch with [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)



**Let’s begin with some instructions…**

  * The first step is to read our Twitter post. There is a hidden instruction in the image that will reveal just a part of the solution;
  * You would also need to read this blog post carefully. To solve the challenge, you must look beyond what’s obvious to the eyes… If you do so, you’ll have the second set of instructions;
  * Have you found all the missing parts? They will help you to find the hidden page.



Got questions? Let us know at [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)

**Good luck to everyone, and we are looking forward to receiving your submissions!**

---

## 6. 

**Date:** Posted: Oct 20, 2022  
**Link:** [https://hex-rays.com/blog/hex-rays-acquisition](https://hex-rays.com/blog/hex-rays-acquisition)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# A consortium of investors acquires Hex-Rays

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-acquisition>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-acquisition>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-acquisition>)

A 

Alex Petrov ✦ Posted: Oct 20, 2022

![A consortium of investors acquires Hex-Rays](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Hex-Rays has been acquired by a consortium of investors led by **Smartfin** , a leading European venture capital and private equity investor, and including co-investors **SFPIM** and **SRIW**. Ilfak Guilfanov, the founder of Hex-Rays, also reinvests a substantial amount in the new structure. Over the past 10 years, Hex-Rays’ revenues have significantly increased. With this transaction, Hex-Rays aims to expand and professionalize whilst enhancing the further development of its products and services.

“ _Smartfin is excited to leverage its prior experience by actively contributing to the further growth and international expansion of Hex-Rays, together with Ilfak, SFPIM and SRIW._ ” said Bart Luyten, Founding Partner at Smartfin. “ _Hex-Rays has built a unique market-leading technology in a highly successful manner and is well positioned to further build on that strong basis in order to capture the ample growth opportunities that the core software reverse engineering and adjacent markets will undoubtedly offer over the next few years._ ”

“ _This partnership is an important milestone and opens new horizons for Hex-Rays._ ” added Ilfak Guilfanov, Founder of Hex-Rays. “ _I look forward to this newly established alliance, which will allow Hex-Rays to further professionalize its organization and accelerate its product innovation efforts as a means to jointly unlock the numerous international growth paths we have identified so far._ ”

“ _SRIW and SFPIM were happy to invest alongside Smartfin and Hex-Rays’ founder in expanding the organization of this world-class tech company_ ” said Leon Cappaert, Investment Manager at SFPIM. “ _This transaction is a fine illustration of a public/private partnership and will contribute to SRIW’s objective of strengthening the Walloon region’s technological ecosystem_ ” added Nicolas Dhaene, Investment Manager at SRIW.

**About Smartfin**

Smartfin is a European private equity and venture capital investor, managing ca. €380m in assets and investing in mid to later-stage technology companies with a focus on B2B technologies.   
The company has an open-ended investment philosophy and invests throughout Europe and the United Kingdom. The team combines a successful venture capital and private equity track record with extensive operational experience in building and managing leading international technology companies, which differentiates it as an experienced hands-on value-added partner to its portfolio companies.

**About the SFPIM: The Federal Holding and Investment Company is the Belgian Sovereign Wealth Fund.**

SFPIM acts as a trusted partner in helping Belgian companies, SMEs as well as scale-ups to become a reference in their industry by providing smart capital solutions. SFPIM also plays a major role in safeguarding the long-term stability of the Belgian economy by contributing to the anchoring of strategic assets through smart capital solutions in both promising and established companies or ecosystems.

**About SRIW**

SRIW contributes to the economic development of the Walloon region through partial financing of Walloon companies or development projects located in Wallonia. It invests in growth alongside private investors through loans and equity.

---

## 7. 

**Date:** Posted: Oct 14, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets](https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #110: Self-relative offsets

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-110-self-relative-offsets>)

I 

Igor Skochinsky ✦ Posted: Oct 14, 2022

![Igor’s tip of the week #110: Self-relative offsets](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We’ve covered [offsets with base](<https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base/>) previously. There is a variation of such offsets commonly used in position-independent code which can be handled easily with a little trick.

Let’s consider this ARM function from an ARM32 firmware:
    
    
    ROM:00000058 ; int sub_58()
    ROM:00000058 sub_58                                  ; CODE XREF: sub_10A4:loc_50↑j
    ROM:00000058                                         ; DATA XREF: sub_8D40+20↓r ...
    ROM:00000058                 ADR             R0, off_88 ; R0 = 0x88
    ROM:0000005C                 LDM             R0, {R10,R11} ; R10 = 0x3ADC0, R11 = 0x3AE00
    ROM:00000060                 ADD             R10, R10, R0 ; R10 = 0x3ADC0+0x88
    ROM:00000064                 SUB             R7, R10, #1
    ROM:00000068                 ADD             R11, R11, R0 ; R11 = 0x3AE00+0x88
    ROM:0000006C
    ROM:0000006C loc_6C                                  ; DATA XREF: sub_58+20↓o
    ROM:0000006C                 CMP             R10, R11
    ROM:00000070                 BEQ             sub_D50
    ROM:00000074                 LDM             R10!, {R0-R3}
    ROM:00000078                 ADR             LR, loc_6C
    ROM:0000007C                 TST             R3, #1
    ROM:00000080                 SUBNE           PC, R7, R3
    ROM:00000084                 BX              R3
    ROM:00000084 ; End of function sub_58
    ROM:00000084
    ROM:00000084 ; ---------------------------------------------------------------------------
    ROM:00000088 off_88          DCD dword_3ADC0         ; DATA XREF: sub_58↑o
    ROM:00000088                                         ; sub_58+4↑o
    ROM:0000008C                 DCD off_3AE00
    

IDA has converted the values at addresses 88 and 8C to offsets because they happen to be valid addresses, but if you look at what the code does (I’ve added comments describing what happens), we’ll see that both values are added to the address from which they’re loaded (0x88), i.e. they’re relative to their own position (or self-relative). 

To get the final value they refer to, we can use the action Edit > Operand type > Offset >Offset (user-defined) (shortcut `Ctrl`–`R`), and enter as the base either the address value (0x88), or, for the case of the value at `00000088`, the IDC keyword `here`, which expands to the address under the cursor.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/selfrel1-3.png?width=827&height=651&name=selfrel1-3.png)

IDA calculates the final address and replaces the value with an expression which uses a special symbol `.`, which denotes the current address on ARM:
    
    
    ROM:00000088 off_88          DCD off_3AE48 - .       ; DATA XREF: sub_58↑o

For the value at `0000008C`, `here` will not work since it expands to 0x8c while the addend is 0x88. There are several options we can use:

  1. use the actual value `0x88` as the base
  2. use the expression `here-4`which resolves to 0x88.
  3. use `here`, but specify 4 in the _Target delta_ field. 



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/selfrel2-3.png?width=802&height=597&name=selfrel2-3.png)

IDA will use the delta as an additional adjustment for the expression:
    
    
    ROM:0000008C                 DCD byte_3AE88+4 - .

Now we can see what addresses the function is actually using and analyze it further.

See also:

[Igor’s tip of the week #105: Offsets with custom base](<https://hex-rays.com/blog/igors-tip-of-the-week-105-offsets-with-custom-base/>)

[Igor’s tip of the week #21: Calculator and expression evaluation feature in IDA](<https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida/>)

---

## 8. 

**Date:** Posted: Oct 7, 2022  
**Link:** [https://hex-rays.com/blog/ida-8-1-released](https://hex-rays.com/blog/ida-8-1-released)

[Back](<https://hex-rays.com/blog>)

[IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.1 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-1-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-1-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-1-released>)

Geoffrey Czokow ✦ Posted: Oct 7, 2022

![IDA 8.1 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Hex-Rays team is thrilled to announce the release of IDA version 8.1!

As with every release, IDA Pro and IDA Home gained many new features and enhancements, including:

  * Private Lumina server
  * New icons
  * Golang regabi support
  * Sunsetting IDA for 32-bit binaries (IDA32)
  * and more



See full updates here: <https://hex-rays.com/products/ida/news/8_1/>

## How to request the new versions

As usual, the new versions of IDA Pro and IDA Home are free for users with an active support plan. Please use the “Help, Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can submit your ida.key to <https://www.hex-rays.com/updida> and our servers will prepare new download links for all your licenses. Please forgive us if your request will take some time to be processed, especially today, after the release. Thanks for your patience.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to [support@hex-rays.com](<mailto:support@hex-rays.com>)

Please consider that only IDA Pro & IDA Home (i.e., not IDA Teams) can be renewed through this usual channel at the moment. Contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) if you are interested in receiving a quote for IDA Teams!

### Has your support plan expired?

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

### Upgrading to IDA Teams

Since IDA Teams was introduced not long ago, our offer to upgrade to IDA Teams still holds. We offer the following upgrade options to corporate users. Your options depend on your licenses at the release date:

  * if you have IDA Pro floating licenses: you can convert those, free of charge, to IDA Teams licenses. This offer is valid until 2022-10-31.
  * if you have IDA Pro non-floating licenses: it’s possible to upgrade those to IDA Teams as well, but the upgrade is not 100% free. Please contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) for a quote. Specify the number of seats you require, and what bundle you are interested in (see <https://www.hex-rays.com/ida-teams/>.) We offer a significant discount until 2022-10-31.
  * if you do not have licenses yet, or wish to acquire new licenses: please contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) for a quote.



Note that IDA Teams is not an add-on for the existing IDA Pro licenses: it is a new product. IDA Teams uses a subscription-based licensing model.

---

## 9. 

**Date:** Posted: Oct 7, 2022  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-109-hex-view-text-encoding](https://hex-rays.com/blog/igors-tip-of-the-week-109-hex-view-text-encoding)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #109: Hex view text encoding

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-109-hex-view-text-encoding>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-109-hex-view-text-encoding>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-109-hex-view-text-encoding>)

I 

Igor Skochinsky ✦ Posted: Oct 7, 2022

![Igor’s tip of the week #109: Hex view text encoding](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

The Hex view is used to display the contents of the database as a hex dump. It is also used during debugging to display memory contents.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hextext1-3.png?width=611&height=168&name=hextext1-3.png)

By default it has a part on the right with the textual representation of the data. Usually the text part shows Latin letters or dots for unprintable characters but you may also encounter something unusual:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hextext2-3.png?width=623&height=96&name=hextext2-3.png)

Why is there Chinese among English? Is it a hidden message and the binary actually comes from China?

In fact, the mystery has a very simple explanation: the encoding used for showing text data in hex view uses the [database default](<https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings/>) which is usually UTF-8, so a valid UTF-8 byte sequence may decode to Chinese, Japanese, Russian, Korean, or even emoji. If you prefer to see only the plain ASCII text, you can change the encoding using these simple steps:

  1. From the hex view’s context menu, invoke Text > Add encoding…  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hextext3-3.png?width=358&height=182&name=hextext3-3.png)
  2. Enter “ascii”;
  3. the new encoding will be added to the list and made default, so any bytes not falling into the ASCII range will be shown as unprintable:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hextext4-3.png?width=511&height=230&name=hextext4-3.png)



Instead of “ascii” you can use another encoding which matches the type of binary you’re analyzing. For example, if you work with legacy Japanese software, encodings like “Shift-JIS”, “cp932” or “EUC-JP” may help you discover otherwise hidden text.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hextext5-3.png?width=533&height=323&name=hextext5-3.png)

See also:

[Igor’s tip of the week #13: String literals and custom encodings](<https://hex-rays.com/blog/igor-tip-of-the-week-13-string-literals-and-custom-encodings/>)

---

