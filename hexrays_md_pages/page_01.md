# Hex-Rays Blog – Page 1

**Archived:** 2026-02-20 12:09
**URL:** https://hex-rays.com/blog/page/1

---

## 1. IDA 9.3 Release: Expanded Architecture Support, Faster UI and More

**Date:** Posted: Feb 13, 2026  
**Link:** [https://hex-rays.com/blog/ida-9.3-release-expanded-architecture-support-faster-ui-and-more](https://hex-rays.com/blog/ida-9.3-release-expanded-architecture-support-faster-ui-and-more)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [decompiler](<https://hex-rays.com/blog/tag/decompiler>) [Android](<https://hex-rays.com/blog/tag/android>) [IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>)

# IDA 9.3 Release: Expanded Architecture Support, Faster UI and More

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-release-expanded-architecture-support-faster-ui-and-more>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-release-expanded-architecture-support-faster-ui-and-more>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-release-expanded-architecture-support-faster-ui-and-more>)

Hex-Rays ✦ Posted: Feb 13, 2026

![IDA 9.3 Release: Expanded Architecture Support, Faster UI and More](https://hex-rays.com/hubfs/blog.png)

After months of feedback and refinement from beta testers, we’re excited to introduce **IDA 9.3**!

Building on the foundation laid in 9.2, this update brings deeper architecture support, smarter decompilation, improved performance, and quality-of-life enhancements that make everyday reverse engineering smoother and more powerful.

Below is an overview of the key features and improvements in IDA 9.3. For complete technical details, be sure to check out the official [release notes](<https://docs.hex-rays.com/release-notes/9_3>).

## **Enhanced Support Across Architectures and Platforms**

IDA 9.3 continues to expand processor coverage and improve instruction decoding accuracy across modern and embedded targets.

  * ARM64 support has been significantly extended, including decoding and intrinsic handling for newer architectural extensions such as SVE, SME, and Memory Tagging Extension (MTE). These additions are especially important for analysts working with modern operating systems and hardened binaries.

  * Support for additional embedded architectures has also grown. A new processor module introduces support for the Andes AndeStar V3 core (NDS32), commonly found in embedded and IoT devices. Improvements have also been made to RISC-V, TriCore, and ARC processors, with better cross-reference detection and overall analysis reliability.

  * On more traditional platforms, x86/x64 and PowerPC benefit from improved instruction semantics and enhanced handling of indirect jumps, contributing to more accurate control-flow reconstruction.

  * This broader hardware coverage ensures IDA remains a strong choice for firmware, automotive, mobile, and platform-specific reverse engineering workflows.




## **Decompiler Improvements and Workflow Enhancements**

The decompiler in IDA 9.3 receives several meaningful upgrades focused on clarity, control, and productivity.

  * One of the most notable additions is decompiler support for the Renesas V850 (RH850) architecture, strengthening IDA’s position in automotive and embedded analysis domains.

  * The microcode viewer has also been enhanced, giving users more control over low-level representations. Analysts can experiment more freely with microcode, including modifying instructions and adjusting variable values, making it easier to explore alternative interpretations and refine complex analyses.

  * Improvements to value range propagation and conditional analysis allow the decompiler to eliminate unreachable branches more effectively and produce clearer, more accurate pseudocode. These refinements translate directly into easier reading and faster comprehension of complex logic.

  * In addition, the “Decompile All” command is now available in more product editions, expanding access to bulk decompilation workflows and improving automation capabilities for a wider audience.




## **User Interface and Responsiveness**

IDA 9.3 places a strong emphasis on performance and user experience.

  * Tabular views such as Functions, Names, Imports, Bookmarks, and Breakpoints are now noticeably more responsive, especially when working with large databases. Navigation across large codebases feels smoother, and view updates are faster and more consistent.

  * Numerous interface refinements and consistency fixes further improve the overall experience. While many of these changes are incremental, together they significantly enhance day-to-day usability and make long analysis sessions more fluid.




## **Debugger Advancements**

Debugger improvements in IDA 9.3 help bridge the gap between static and dynamic analysis.

  * Support for modern Android environments has been extended, including improved compatibility with recent platform versions and enhanced handling of security mechanisms such as pointer authentication (PAC).

  * The stack view has also been improved, offering more intuitive dereferencing and better visualization of stack contents during debugging sessions. These enhancements make runtime inspection clearer and more efficient.




# **Keep us in the loop!**

  * If you see something, say something. You can report bugs here: [https://support.hex-rays.com](<https://get.support.hex-rays.com/servicedesk/customer/portals>)
  * Have ideas on how we can improve IDA? We want to know! Send us a note with your feedback directly to [product@hex-rays.com](<mailto:product@hex-rays.com>), or post it on [Discourse](<https://community.hex-rays.com/>).



## Getting the IDA 9.3

**IDA 9 Customers**  
If you have an active IDA 9 license, you will see IDA 9.3 in the **Download Center** of the My Hex-Rays [ customer portal](<https://my.hex-rays.com/login>).  
  
**Support plan expired? No problem.** You can purchase an updated license online [here](<https://hex-rays.com/pricing>). You’ll see we’ve updated our product packages—if your previous plan doesn’t align with our current offerings, we’re here to help you find the right fit (and price). Just email us at **sales@hex-rays.com**.

❤️ Special Valentine's Day Promo ❤️

We're offering new and returning customers 40% off any IDA Pro license.

Use promo code **LOVE40** by visiting our webshop, or email **sales@hex-rays.com** and mention this promo.

**Offer expires February 28, 2026.** Check out the details and full terms [here](</license-love>).

---

## 2. Collaborate Where You Analyze: Teams Moves Inside IDA

**Date:** Posted: Feb 11, 2026  
**Link:** [https://hex-rays.com/blog/ida-9.3-teams-moves-inside-ida](https://hex-rays.com/blog/ida-9.3-teams-moves-inside-ida)

[Back](<https://hex-rays.com/blog>)

[IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [Collaboration](<https://hex-rays.com/blog/tag/collaboration>) [IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>)

# Collaborate Where You Analyze: Teams Moves Inside IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-teams-moves-inside-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-teams-moves-inside-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-teams-moves-inside-ida>)

Hex-Rays ✦ Posted: Feb 11, 2026

![Collaborate Where You Analyze: Teams Moves Inside IDA](https://hex-rays.com/hubfs/9.3%20Teams%20Blog.png)

Starting with IDA 9.3, [Teams](</teams>) is fully integrated into IDA itself — helping analysts who collaborate on shared reverse engineering projects via the [Hex-Rays Vault Server](<https://docs.hex-rays.com/admin-guide/teams-server>). Previously, collaboration required switching between IDA and the standalone [HVUI](<https://docs.hex-rays.com/user-guide/teams/hvui_user_manual>) application. Now all workflows happen directly inside IDA: connecting to the Vault Server, browsing remote files, diffing, and committing changes.

The HVUI application is completely retired and no longer shipped with 9.3. This is a major architectural change affecting all Teams users.

# New Teams Menu

A new top-level Teams menu provides access to all collaboration features previously available in HVUI. Teams → Connect opens the server connection dialog, Teams → Connect → Users (for admins) manages user access, and all HVUI widgets are now standard IDA views. Widget names and commands remain familiar to existing users.

![9.3 teams - 1](https://hex-rays.com/hs-fs/hubfs/9.3%20teams%20-%201.png?width=320&height=452&name=9.3%20teams%20-%201.png) ![9.3 teams - 2](https://hex-rays.com/hs-fs/hubfs/9.3%20teams%20-%202.png?width=310&height=445&name=9.3%20teams%20-%202.png)

# Quick Start Integration

The Quick Start dialog now includes a Remote option for browsing remote repositories. You can open files from the Vault Server directly without first working locally.

![9.3 teams - 3](https://hex-rays.com/hs-fs/hubfs/9.3%20teams%20-%203.png?width=326&height=284&name=9.3%20teams%20-%203.png)

# In-IDA Diffing

Database diffs can now be performed inside IDA. No need to switch to an external tool to compare revisions.

![9.3 teams - 4](https://hex-rays.com/hs-fs/hubfs/9.3%20teams%20-%204.png?width=512&height=297&name=9.3%20teams%20-%204.png)

# User Impact

Users can now work on collaboration tasks without switching applications — one tool to learn and maintain instead of two. The workflow remains the same (fetch → edit → commit), and existing Teams users will find familiar commands and widgets. No relearning required.

# Implementation Notes

HVUI had its own lighter implementations of some core features. Bringing Teams into IDA meant replacing those with IDA's full-featured equivalents — more capable, but also more complex to integrate with.

# How to Access Teams

The main entry point is Teams → Connect in the menu bar. The Quick Start dialog includes a Remote option for accessing remote repositories. Connection status appears in the bottom-right corner of the IDA window.

Available with IDA Pro and the Teams add-on license. Bundled with the product release.

# Final Notes

For existing Teams users, the transition should be straightforward — same workflows, fewer windows. Feedback on edge cases or missing functionality is welcome.

# Future developments

In the next releases, we’re rolling out several new features for the Collaboration Suite. This broader initiative is designed to make it easier for people to work together when reverse engineering, and it significantly improves the team experience in IDA. You’ll be able to plug in your own Git-based version control system to store IDA databases and handle diffing and merging just like you would with code. We’re also adding support for deep links, so you can share a link straight from IDA and your teammates can jump right to the exact spot you’re looking at. And finally, live collaboration will let multiple people work together in the same IDA session at the same time — no more passing databases around.

---

## 3. Disassemblers Aplenty: ARM64 Extensions (SVE, MTE, CSSC), AndeStar™ (V3 and V5), ARC, TriCore & More

**Date:** Posted: Jan 30, 2026  
**Link:** [https://hex-rays.com/blog/ida-9.3-disassemblers-aplenty](https://hex-rays.com/blog/ida-9.3-disassemblers-aplenty)

[Back](<https://hex-rays.com/blog>)

[ARM](<https://hex-rays.com/blog/tag/arm>) [RISC-V](<https://hex-rays.com/blog/tag/risc-v>) [TriCore](<https://hex-rays.com/blog/tag/tricore>) [IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>) [ARC](<https://hex-rays.com/blog/tag/arc>) [NDS32](<https://hex-rays.com/blog/tag/nds32>)

# Disassemblers Aplenty: ARM64 Extensions (SVE, MTE, CSSC), AndeStar™ (V3 and V5), ARC, TriCore & More

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-disassemblers-aplenty>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-disassemblers-aplenty>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-disassemblers-aplenty>)

Alexandru Petenchea ✦ Posted: Jan 30, 2026

![Disassemblers Aplenty: ARM64 Extensions \(SVE, MTE, CSSC\), AndeStar™ \(V3 and V5\), ARC, TriCore & More](https://hex-rays.com/hubfs/9.3%20Disassembler%20Blog.png)

These upcoming updates primarily help users working close to the metal, on kernels, firmware, and embedded targets, where stack behavior and control flow need to be trustworthy.

By improving instruction decoding, idiom recognition, and cross-reference generation, these changes turn previously noisy or ambiguous regions of code into something that can be analyzed directly.

# **ARM64 (Apple extensions and analysis improvements)**

## **SVE Decoding (Apple kernels)**

When released, IDA 9.3 improves decoding of Scalable Vector Extension (SVE) and Scalable Matrix Extension (SME) instructions as used in recent Apple kernels.

STR Z0, [X2]

STR Z1, [X2,#1,MUL VL]

STR Z2, [X2,#2,MUL VL]

STR Z3, [X2,#3,MUL VL]

UZP1 V0.4S, V0.4S, V1.4S

ZIP1 V2.4S, V2.4S, V2.4S

  
Concretely:

  * **Z and P registers.** These are SVE’s vector (Z0, Z1, …) and predicate (P0, P1, …) registers.
  * **ZA arrays and ZT0 tables (SME-specific).** SME introduces new register-like storage (e.g. ZA) that behaves more like a tiled array than a normal register. IDA now prints accesses like: ZA[Wv,#offset], which tells you _which tile_ , _which index_ , and _which offset_ is being accessed.
  * **MUL VL scaling.** SVE uses the vector length (VL), which is not fixed at compile time.



## **Memory Tagging Extension (MTE) intrinsics**

The ARM64 decompiler now emits explicit intrinsics for Memory Tagging Extension instructions instead of inline assembly blocks. Instructions such as IRG, ADDG, GMI, LDG, STG, and SUBP are translated into corresponding __arm_mte_* intrinsics in the decompiler output.

Before:
```asm
__int64 __fastcall create_tag(__int64 _X0, unsigned int a2)

{

__int64 result; // x0

_X8 = a2;

__asm { IRG X0, X0, X8 }

return result;

}
```

After:
```
void *__fastcall create_tag(void *a1, unsigned int a2)

{

return __arm_mte_create_random_tag(a1, a2);

}
```

See the release notes for a full list of supported MTE intrinsics: <https://docs.hex-rays.com/release-notes/9_3beta#memory-tagging-extension-mte-intrinsics>

## **Common Short Sequence Compression (CSSC) decoding & intrinsics**

We added decoding and decompiler support for ARMv8 CSSC instructions, including SMAX, SMIN, UMAX, UMIN, ABS, CNT, and CTZ. For scalar general-purpose register variants, the decompiler also emits corresponding intrinsics, improving readability and reducing noise in arithmetic-heavy code paths.

```assembly
ABS W0, W0

CNT W21, W10

CTZ W23, W13

SMAX WZR, WZR, #0xFFFFFFFF

SMIN X0, X0, #0

UMAX XZR, XZR, #0xFFFFFFFFFFFFFFFF

UMIN X23, X13, X8
```

## **Improved address construction from MOV/MOVK/MOVW/MOVT sequences**

Address construction using MOV/MOVK (AArch64) and MOVW/MOVT (ARM/Thumb) sequences is now handled more robustly, including cases where instructions are not strictly adjacent.
```assembly
Before

MOV X8, #0

MOV W9, #0x64AE

MOVK X8, #0,LSL#16

MOVK W9, #0x299E,LSL#16

MOVK X8, #0,LSL#32

MOVK X8, #0,LSL#48
```

After
```
MOV X8, #(WORD0(unk_16120))

MOV W9, #(WORD0(0x299E64AE))

MOVK X8, #(WORD1(unk_16120)),LSL#16

MOVK W9, #(WORD1(0x299E64AE)),LSL#16

MOVK X8, #(WORD2(unk_16120)),LSL#32

MOVK X8, #(WORD3(unk_16120)),LSL#48
```

At the same time we added support for AArch64 MOVW_UABS_G[0-3] relocations, allowing ELF binaries that build addresses via MOVZ/MOVK sequences to produce correct references for each 16-bit chunk and, where applicable, merged full-width addresses.

On Apple platforms, recognition of __auth_stubs sections has improved, and BTI-enabled PLT stubs in ELF binaries are now correctly detected, named, and marked as thunks.

# **NDS32 (Andes AndeStar™ V3)**

We’re introducing a new processor module for the Andes AndeStar™ V3 NDS32 architecture, along with ELF loader integration. NDS32 ELF binaries are now recognized automatically, with IDA selecting the correct processor and displaying architecture and ABI details directly in the load banner.

It supports NDS32’s mixed 16-bit and 32-bit instruction encodings, covering the base ISA along with commonly used extensions such as DSP, SIMD, ZOL, FPU, and coprocessor instructions. For better readability, by default IDA hides the encoding suffixes of instruction mnemonics (i.e. movi rather than movi55, add rather than add333, etc.) and the dollar prefixes of registers. If needed, these details can be enabled again in the processor module options.

![disassembler 9.3 - 1 - v2](https://hex-rays.com/hs-fs/hubfs/disassembler%209.3%20-%201%20-%20v2.png?width=1193&height=717&name=disassembler%209.3%20-%201%20-%20v2.png)

Concretely:

  * **Disassembly and instruction coverage.**  
Mixed 16-bit and 32-bit instruction encodings, covering the base ISA along with commonly used extensions such as DSP, SIMD, FPU, and coprocessor instructions.


  * **Aggressive resolution of indirections.** Memory accesses via gp-relative addressing and direct calls assembled from constants at runtime both are tracked, with resolved targets displayed as comments next to the corresponding instructions.


  * **ex9.it instruction resolution.** The procmod support for resolving ex9.it instructions, which are table-indexed 16-bit opcodes that depend on the ITB (Instruction Table Base). When the ITB value is known (either from symbols or register initialization) IDA can resolve these instructions into their corresponding 32-bit forms. This behavior is optional and controlled via a processor setting.  
  




![disassembler 9.3 - 2 - v2](https://hex-rays.com/hs-fs/hubfs/disassembler%209.3%20-%202%20-%20v2.png?width=1193&height=717&name=disassembler%209.3%20-%202%20-%20v2.png)

# **ARC (Push/Pop idiom recognition)**

# **Clearer prologues and epilogues**

IDA 9.3 improves ARC disassembly by recognizing common stack save and restore idioms and presenting them as explicit push and pop instructions. Store and load instructions that update the stack pointer by one word are now rewritten when they semantically represent a push or pop.

Before

st.a r16, [sp,0xC+var_10]

st.a r17, [sp,0x10+var_14]

st.a r18, [sp,0x14+var_18]

st.a r19, [sp,0x18+var_1C]

st.a r20, [sp,0x1C+var_20]

st.a r21, [sp,0x20+var_24]

st.a fp, [sp,0x24+var_28]

After

push r16

push r17

push r18

push r19

push r20

push r21

push fp

## **Improved stack analysis**

Because pushes and pops are now explicit, IDA’s stack tracking logic can reason about ARC functions more reliably. Stack pointer deltas are derived directly from push/pop instructions, which leads to more consistent stack point placement and frame sizing.

This also preserves compatibility with existing ARC analysis features. In particular, ARCompact millicode patterns that rely on recognizing chains of push instructions continue to be detected correctly, even with instruction simplification enabled.

These changes are also reflected in the decompiler.

# **Andes RISC-V extensions (configurable decoding)**

Both the RISC-V disassembler and the decompiler now support the **Andes XAndesPerf** extension (29 new instructions). We’ve also added processor specific options for vendor extensions (currently Andes XAndesPerf and T-Head XThead). You can toggle those extensions in the RISC-V specific options dialog.

![disassembler 9.3 - 3](https://hex-rays.com/hs-fs/hubfs/disassembler%209.3%20-%203.png?width=403&height=427&name=disassembler%209.3%20-%203.png)

When only Andes decoding is enabled, those words disassemble as nds.* instructions; with only T-Head enabled, they disassemble as th.*. If you’d rather not have these prefixes, you can enable the “Hide extension prefixes” option.

If both vendor extensions are enabled and an encoding collides, the procmod doesn’t silently choose one interpretation: it keeps one as the primary decode and emits the other as an **auto-comment** (for example: nds.lbgp a0, 11BC5h ; alt XThead: th.lrb). That makes ambiguity explicit during analysis, which is especially helpful when you’re dealing with incomplete core documentation or binaries built for slightly different configurations.

![disassembler 9.3 - 4](https://hex-rays.com/hs-fs/hubfs/disassembler%209.3%20-%204.png?width=512&height=308&name=disassembler%209.3%20-%204.png)

# **TriCore**

IDA 9.3 includes a set of focused analysis improvements for TriCore that mainly affect how values are tracked and how control flow is reconstructed. You can now use the regfinder API programmatically:

import ida_idp

import ida_regfinder

reg = ida_idp.str2reg(reg)

res = ida_regfinder.find_reg_value(ea, reg)

Switch statement detection has also been refined, allowing more jump tables to be recognized and rendered as structured control flow instead of opaque indirect branches.

## Wrapping it Up

Taken together, these changes make disassembly more trustworthy and analysis workflows more predictable. As always, feedback from real binaries is invaluable, particularly for edge cases that still don’t decode or analyze as expected.

---

## 4. 

**Date:** Posted: Jan 22, 2026  
**Link:** [https://hex-rays.com/blog/ida-9.3-jump-anywhere](https://hex-rays.com/blog/ida-9.3-jump-anywhere)

[Back](<https://hex-rays.com/blog>)

[IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>) [Jump Anywhere](<https://hex-rays.com/blog/tag/jump-anywhere>)

# Jump Anywhere: Unified Navigation Gets an Upgrade in IDA 9.3

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-jump-anywhere>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-jump-anywhere>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-jump-anywhere>)

Hex-Rays ✦ Posted: Jan 22, 2026

![Jump Anywhere: Unified Navigation Gets an Upgrade in IDA 9.3](https://hex-rays.com/hubfs/9.3%20Jump%20Anywhere%20Blog.png)

An IDA database stores many different kinds of information: functions, named global variables, types, and more. **Jump Anywhere** , introduced in IDA 9.2, is a unified “quick navigation” dialog that lets you search across those database items from a single place. It also supports resolving simple expressions that the user could have entered into the “Jump to address” dialog.

With the upcoming IDA 9.3, Jump Anywhere becomes substantially more practical for everyday use: it’s faster on large databases, supports searching _demangled_ symbol names, and remains responsive thanks to asynchronous searching. Additional improvements are planned for future IDA releases.

![jump anywhere 1](https://hex-rays.com/hs-fs/hubfs/jump%20anywhere%201.png?width=687&height=457&name=jump%20anywhere%201.png)_Jump Anywhere dialog showing the results list and a preview type listing for the currently highlighted match._

# Using Jump Anywhere in Practice

Jump Anywhere is a feature that aims to help you navigate faster between functions, local types and other database items. It is designed to be a single, multi-purpose quick navigation feature, allowing search in data normally scattered across multiple IDA subwindows or dialogs.

With Jump Anywhere you can:

  * Search for addresses (accepting the same forms of input as the previous “Go to address” dialog);
  * Search for functions, names, local types, and segments, with results sorted by a similarity score.



__

If the input can be parsed as an address or expression, that interpreted address is prioritized over the rest of the results. Other results are ordered primarily by similarity score.

__

As of the time of writing, the score IDA uses is based on the length of the displayed result text, with further improvements planned in future IDA versions. When multiple results have the same score, IDA currently applies tie-breakers: first the result type is prioritized (functions, then names, then local types, then segments), and then the address. This behavior may change in future versions of IDA.

__

The dialog also provides a preview for the destination where the user will jump if the current entry is selected.

![jump anywhere 2](https://hex-rays.com/hs-fs/hubfs/jump%20anywhere%202.png?width=682&height=441&name=jump%20anywhere%202.png)

_The dialog with a hex address as input. A result with the parsed expression is added in addition to the list of functions matching the query._

In IDA 9.3 the feature was improved in several ways:

  * The demangled versions of functions and names are searchable as well.
  * Searches are now performed asynchronously, which means long-running searches can be cancelled and the UI remains responsive.
  * Demangled symbols are now indexed, matching the default behavior of the Functions and Names windows.
  * The dialog supports jumping to a field offset in the Local Types view, acting as a full replacement for the “Jump to type offset” dialog.



![jump anywhere 3](https://hex-rays.com/hs-fs/hubfs/jump%20anywhere%203.png?width=682&height=450&name=jump%20anywhere%203.png)

_An example search using part of a demangled name as the query, with a local type result selected. The mangled name is not shown unless the demangled name does not match._

![jump anywhere 4](https://hex-rays.com/hs-fs/hubfs/jump%20anywhere%204.png?width=678&height=442&name=jump%20anywhere%204.png)

_An example search using a mangled name as the query. The foo::Container destructor from the previous screenshot is now displayed in mangled form._

![jump anywhere 5](https://hex-rays.com/hs-fs/hubfs/jump%20anywhere%205.png?width=694&height=500&name=jump%20anywhere%205.png)

_The dialog when used in Local Types. Field offsets (absolute or relative) and field names can be used to jump to the corresponding field, complete with preview._

# How Search is Implemented

To enable fast searches, IDA indexes the functions and names in the database.

In IDA 9.3, search performance has been significantly optimized, making Jump Anywhere much more usable on large IDBs. The data structure holding indexed function and name data has been rewritten so IDA can safely perform parallel searches.

The new structure stores all names in lowercase in a continuous data buffer. This allows IDA to perform a simple sequential memory scan, which in our testing proved substantially faster than the old approach of scanning more scattered data.

Each thread scans a portion of this buffer. Because we prioritize ongoing analysis, any modification to this structure cancels all pending searches.

IDA indexes both mangled and demangled names for all functions and names listed in the Names window, preferring the demangled variant. If a match is found only in the mangled form, the result will be displayed in its mangled form.

# Where You’ll Find It (Once Released)

You can open the Jump Anywhere dialog by pressing Ctrl-Alt-G (CMD-Alt-G on macOS). Alternatively you can make this feature open by pressing the G key (replacing the default “Jump to address” dialog) by enabling an option in the Feature Flags dialog (Options → Feature Flags...).

![jump anywhere 6](https://hex-rays.com/hs-fs/hubfs/jump%20anywhere%206.png?width=679&height=173&name=jump%20anywhere%206.png)

Additionally, there are some advanced settings that can be changed in idagui.cfg:

  * JUMP_ANYWHERE_MAX_RESULTS \- IDA will truncate the list of results to this value before displaying them to the user
  * JUMP_ANYWHERE_MAX_HISTORY \- the number of queries that IDA should save in the history (not yet available in IDA 9.3)



Jump Anywhere requires IDA’s indexer subsystem. It is configured using the ENABLE_INDEXER option in ida.cfg and is enabled by default.

# Looking Ahead

IDA 9.3 is only the starting point for Jump Anywhere: features like history, improved scoring, fuzzy search, and API access are still planned for future versions. In the meantime, we hope the current feature set is useful in real workflows and provides a solid foundation for feedback.

---

## 5. 

**Date:** Posted: Jan 16, 2026  
**Link:** [https://hex-rays.com/blog/ida-9.3-more-responsive-tabular-views](https://hex-rays.com/blog/ida-9.3-more-responsive-tabular-views)

[Back](<https://hex-rays.com/blog>)

[UI](<https://hex-rays.com/blog/tag/ui>) [IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>) [Tabular Views](<https://hex-rays.com/blog/tag/tabular-views>)

# Faster, More Responsive Tabular Views in IDA 9.3

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-more-responsive-tabular-views>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-more-responsive-tabular-views>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-more-responsive-tabular-views>)

Arnaud Diederen ✦ Posted: Jan 16, 2026

![Faster, More Responsive Tabular Views in IDA 9.3](https://hex-rays.com/hubfs/9.3%20Tabular%20View%20Blog.png)

We have started a long-term effort to improve IDA’s performance across the board.

IDA 9.3 comes with a first pass at eliminating some of the most prominent bottlenecks in the core widgets that make up the IDA UI. These bottlenecks could lead to (sometimes serious) performance penalties when working with large binary files.

Perhaps the most noticeable improvements will be seen when working with large datasets in tabular views (e.g., **Functions** , **Names** , **Local Types** in the left-hand pane, etc.).

# **What’s new, and why does it matter?**

Perhaps the most striking impact of this first pass of refactoring is the removal of (hopefully all) significant slowdowns that could occur when manipulating large selections of items in big datasets:

![tabular views screenshot](https://hex-rays.com/hs-fs/hubfs/tabular%20views%20screenshot.png?width=512&height=641&name=tabular%20views%20screenshot.png)

For example, if you were working on an IDB with, say, ~1 million functions in IDA 9.2, attempting to select a large subset of them (e.g., ~500,000) and move them into a new folder would result in an unpleasant user experience.

IDA 9.3 eliminates this class of slowdown. While working with very large datasets (i.e., large IDBs) will never be as fast as working with small ones, the overall user experience should now be significantly more pleasant. That’s the goal, and if we missed something and you still encounter performance issues, please be sure to let us know ([support@hex-rays.com](<mailto:support@hex-rays.com>)).

# **Behind the scenes**

For the curious, here are a few technical nuggets…

Since its earliest days, IDA has featured _tabular views_ (think **Functions** , **Imports** , etc.). Until around 2020, those views were strictly _flat_.

With IDA 7.5, we introduced a new (and very well-received) feature that allowed users to _classify_ items into groups (i.e., “folders”):  
[https://docs.hex-rays.com/release-notes/7_5#folder-view](<https://docs.hex-rays.com/release-notes/7_5#folder-view>)

In a nutshell, our approach was: _“Let’s try to complement our traditionally flat view backend (the venerable_ _chooser_[multi_]t_ _) with a companion data structure that can also be represented as a tree.”_

This is why users could toggle folders on and off in certain widgets, such as **Functions**.

It worked! …but it came with strings attached.

At the time, we thought we could live with those tradeoffs, but we were eventually proven otherwise. We couldn’t find a way to make those widgets perform as fast as they should, and as binary files grew larger and larger, the issue became increasingly visible. There was also a more insidious side effect: the _complexity_ of the code manipulating those large datasets increased significantly as well.

Conceptually, the solution is very simple: move away from those complex widgets and use simpler ones instead. We did exactly that for many widgets in IDA 9.3, including **Functions** , **Names** , **Local Types** , **Bookmarks** , **Imports** , and **Breakpoints**.

…however, this is much easier said than done. Taking the **Functions** widget as an example, there are a myriad of operations that must be supported: manipulating breakpoints, synchronizing with the listing, querying through the API, and many others. Of course, all that functionality is intact in the upcoming release.

# **Closing**

For this blog, we mainly focused on tabular views because we believe that was one area that had a lot of room for improvement. The tabular views and other updates which you will find in IDA 9.3 are aimed at improving the overall experience when working with large files.  
  
There are more UI and UX performance gains to be made and we’ll continue looking for them.

Many, _many_ thanks to the IDA users who reported issues — your feedback is invaluable!  
…and BIG kudos to our beta testers as well: one regression slipped into IDA 9.3b1, but it didn’t make it past [「redacted」 ](<https://redacted.im/>)’s testing!

---

## 6. 

**Date:** Posted: Jan 8, 2026  
**Link:** [https://hex-rays.com/blog/ida-9.3-expands-decompiler-lineup](https://hex-rays.com/blog/ida-9.3-expands-decompiler-lineup)

[Back](<https://hex-rays.com/blog>)

[decompiler](<https://hex-rays.com/blog/tag/decompiler>) [Microcode Viewer](<https://hex-rays.com/blog/tag/microcode-viewer>) [IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>) [RH850](<https://hex-rays.com/blog/tag/rh850>)

# IDA 9.3 Expands and Improves Its Decompiler Lineup

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-expands-decompiler-lineup>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-expands-decompiler-lineup>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-expands-decompiler-lineup>)

Hex-Rays ✦ Posted: Jan 8, 2026

![IDA 9.3 Expands and Improves Its Decompiler Lineup](https://hex-rays.com/hubfs/9.3%20Decompiler%20Blog.png)

We know you’re always looking for broader platform coverage from the Hex-Rays decompiler, which is why we’re adding another one to the lineup: the RH850 decompiler. And of course, we haven’t stopped improving what’s already there. In this upcoming release, we’ve enhanced the analysis of Golang programs, fine-tuned value range optimization, made the new microcode viewer easier to use, and more.

# RH850 Decompiler

While there's still room for further polishing, the RH850 decompiler can already handle real-world applications. For example, this assembler sequence:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-40-25-3277-PM.png?width=608&height=356&name=undefined-Jan-07-2026-09-40-25-3277-PM.png)

Gets converted into this short code snippet:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-40-47-5234-PM.png?width=521&height=134&name=undefined-Jan-07-2026-09-40-47-5234-PM.png)

Or this quite long function:  
![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-41-07-2298-PM.png?width=623&height=331&name=undefined-Jan-07-2026-09-41-07-2298-PM.png)

Gets decompiled into just one line like this:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-41-06-9663-PM.png?width=226&height=64&name=undefined-Jan-07-2026-09-41-06-9663-PM.png)

We’re certain our users will appreciate the new decompiler.

# Improvements to Golang Support

And if you were expecting more exciting news, you’re absolutely right—we have more to share. Let’s take a look at the upcoming improvements to Golang support. First, assembly-level analysis of Golang functions is now clearer, with improved recognition of standard structures:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-44-27-6893-PM.png?width=593&height=250&name=undefined-Jan-07-2026-09-44-27-6893-PM.png)

Including the standard “string” type:  
![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-44-44-1729-PM.png?width=628&height=179&name=undefined-Jan-07-2026-09-44-44-1729-PM.png)

Now we do a better job of detecting and collapsing duplicate types. 

Before:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-45-19-0988-PM.png?width=324&height=120&name=undefined-Jan-07-2026-09-45-19-0988-PM.png)

After:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-45-19-3830-PM.png?width=314&height=91&name=undefined-Jan-07-2026-09-45-19-3830-PM.png)

Types are stored in subfolders. 

Before:  
![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-45-45-5698-PM.png?width=283&height=426&name=undefined-Jan-07-2026-09-45-45-5698-PM.png)   


After:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-46-13-2056-PM.png?width=275&height=413&name=undefined-Jan-07-2026-09-46-13-2056-PM.png)

# Microcode Viewer Updates

The microcode viewer has more interactivity: now it is possible to delete undesirable instructions during decompilation or add new instructions. For example, the instruction 1.9 was deleted by the user right clicking on an instruction and selecting “Delete instruction”:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-47-16-0149-PM.png?width=481&height=167&name=undefined-Jan-07-2026-09-47-16-0149-PM.png)

This means that the rest of decompilation will be handled without it. This feature is handy for deleting junk code. The user can also add new instructions by right clicking and selecting “Insert assertion”:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-50-14-0924-PM.png?width=408&height=194&name=undefined-Jan-07-2026-09-50-14-0924-PM.png)

Such an instruction can be used to notify the decompiler about the outcome of a complex sequence of instructions (for example, the outcome of an obfuscated code). This is a step towards making our decompilers more interactive.

We keep improving our decompilation engine, to produce shorter and more readable code. There are many improvements, but let us show you just one of them.

Before:  
![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-51-03-9368-PM.png?width=398&height=30&name=undefined-Jan-07-2026-09-51-03-9368-PM.png)  
After:   
![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-51-04-5232-PM.png?width=195&height=26&name=undefined-Jan-07-2026-09-51-04-5232-PM.png)

The decompiler proved that the second condition is not necessary and removed it. Behind the scenes, this shortening was possible because of the value range optimization.

Another new feature is “Forbid assignment propagation”. It makes the output more readable. For example, we have a repeated expression here:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-51-04-2091-PM.png?width=431&height=122&name=undefined-Jan-07-2026-09-51-04-2091-PM.png)

The user can ask not to propagate v19:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-51-04-8147-PM.png?width=347&height=223&name=undefined-Jan-07-2026-09-51-04-8147-PM.png)

And the result will be:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-07-2026-09-53-09-5522-PM.png?width=507&height=102&name=undefined-Jan-07-2026-09-53-09-5522-PM.png)

As you can see, the repeated expression disappeared.

We’ve highlighted the most exciting decompiler updates here, but this is only part of the story. There’s more waiting for you once IDA 9.3 is released . Stay tuned!

---

## 7. 

**Date:** Posted: Jan 6, 2026  
**Link:** [https://hex-rays.com/blog/ida-9.3-type-system-improvements](https://hex-rays.com/blog/ida-9.3-type-system-improvements)

[Back](<https://hex-rays.com/blog>)

[IDAPython](<https://hex-rays.com/blog/tag/idapython>) [IDAClang](<https://hex-rays.com/blog/tag/idaclang>) [IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>) [Type Systems](<https://hex-rays.com/blog/tag/type-systems>) [tilib](<https://hex-rays.com/blog/tag/tilib>)

# IDA 9.3: Practical Improvements to the Type System

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-type-system-improvements>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-type-system-improvements>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-type-system-improvements>)

Alexandru Petenchea ✦ Posted: Jan 6, 2026

![IDA 9.3: Practical Improvements to the Type System](https://hex-rays.com/hubfs/9.3%20Blog%20-%20Type%20Systems.png)

The soon to be released, IDA 9.3 tightens up the type system in a few practical ways: Objective-C headers can now be parsed directly, exported headers are easier to read, and type information can make a full round trip between headers and the database without losing structure.  


**Objective-C parser (Clang-based)**

IDA 9.3 adds a Clang-based Objective-C parser that understands @interface declarations, methods, categories, protocols, and @property definitions, and stores them as real types in the database. Objective-C interfaces are represented as __objc_interface __cppobj types with proper inheritance, so ivars and superclass layouts are preserved instead of being approximated or flattened.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-06-12-4822-PM.png?width=1084&height=1194&name=undefined-Jan-06-2026-07-06-12-4822-PM.png)

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-07-44-2516-PM.png?width=1600&height=755&name=undefined-Jan-06-2026-07-07-44-2516-PM.png)

Method signatures are emitted as named types using the familiar -[Class selector:] and +[Class selector:] forms, including automatically generated property accessors. This makes selectors readable, searchable, and reusable as actual type information rather than opaque strings.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-08-35-6879-PM.png?width=1600&height=641&name=undefined-Jan-06-2026-07-08-35-6879-PM.png)

### **Fixed structs round-trip cleanly**

This release introduces the __fixed(size) and __at(offset[, bit]) annotations to represent structures with explicit layouts. These annotations preserve exact sizes, offsets, and bitfields when exporting headers, and can be parsed back into IDA without losing layout information.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-09-26-0303-PM.png?width=1076&height=448&name=undefined-Jan-06-2026-07-09-26-0303-PM.png)

This makes it possible to round-trip types such as stack frames, ABI-defined records, or packed structures without manual fixes after re-import.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-09-52-6573-PM.png?width=1174&height=508&name=undefined-Jan-06-2026-07-09-52-6573-PM.png)

### **Cleaner header exports**

Header exports are easier to work with thanks to the new “Ignore IDA anonymous types” option. When enabled, anonymous structs and unions are inlined directly into their parent types instead of being emitted as hash-named intermediates.

The result is headers that are more readable, less IDA-specific, and better suited for reuse in external tools or for re-importing later without unnecessary noise.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-10-24-3381-PM.png?width=1140&height=514&name=undefined-Jan-06-2026-07-10-24-3381-PM.png)

### **tilib comes equipped with C++ and Objective-C**

tilib can now parse C++ and Objective-C headers directly using the built-in Clang parser, so you don’t need to run idaclang just to build a TIL. For new workflows, using tilib directly is recommended. It uses the same parser as the type editor, and will continue to receive new language and type-system features going forward.

### **Why it matters and What's happening behind the scenes**

In practice, this means Objective-C headers can live in Local Types, fixed-layout structs no longer break when round-tripped through C headers, and exported types don’t need manual cleanup before being reused elsewhere.  


Objective-C parsing has existed in IDA for a long time through [IDAClang](<https://docs.hex-rays.com/user-guide/types/type-libraries/idaclang_tutorial>), primarily as an import mechanism. In IDA 9.3, the new Clang-based type parser reaches feature parity with IDAClang while making the same functionality available directly in the type editor.

One of the main challenges has been integrating Clang’s richer type information without breaking compatibility with existing IDA parsers and type syntax. The new parser is designed to coexist with those systems, and it will continue to be extended as more language features are brought over.

### **Where you'll find it in 9.3**

### **Clang parser (GUI)**

  1. Options -> Compiler
  2. Set “Source parser” to clang
  3. Parser specific options: check Objective-C or Objective-C++
  4. Add SDK include paths in the “Include directories”.
  5. Local Types -> Add type… (Ins)
  6. Paste your Objective-C code in the type editor



![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-31-26-9767-PM.png?width=1600&height=976&name=undefined-Jan-06-2026-07-31-26-9767-PM.png)

### **Clang parser (IDAPython)**

The same workflow can be driven programmatically using IDAPython:

import ida_srclang

import idaapi

ida_srclang.set_parser_argv(

"clang",

"-x objective-c -target arm64-apple-darwin -framework Foundation -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk -ILibrary/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include"

)

idaapi.parse_decls(

None,

"/path/to/header.h",

None,

idaapi.HTI_FIL | idaapi.HTI_NWR | idaapi.HTI_LOWER | idaapi.HTI_NDC

)

The main requirement for successful Objective-C parsing is having the correct headers available to Clang. On macOS, this usually just means pointing the parser at an installed Xcode SDK, since the Objective-C runtime and Foundation headers are already present on the system. In most cases, adding the SDK’s usr/include path is sufficient.

On Linux, the situation is different: Objective-C headers are typically not installed by default. You’ll need to install a runtime and headers yourself. For example, projects based on [GNUstep](<https://github.com/gnustep/libobjc>) can provide compatible headers (libobjc, libs-base), which can then be added to the Clang include paths used by IDA.

As long as Clang can see a coherent set of Objective-C runtime and framework headers, IDA’s type parser will accept and import the declarations.

### **Fixed-struct round-trip (create / export / import)**

Fixed-layout structures can be created or edited directly in **Local Types** using the C-syntax editor and the new layout annotations:

struct __fixed(0x48) x64_prologue_frame

{

__at(0x0000) void *return_addr;

__at(0x0008) void *saved_rbp;

__at(0x0010) void *saved_rbx;

__at(0x0038) int local_var1;

__at(0x003C) int local_var2;

};

  * **Export:** Use **File → Produce file → Create C header file…**. The generated header will preserve __fixed() and __at() annotations so that layout information is not lost.
  * **Import / round-trip:** You can give the exported header to a colleague and re-import it in another IDB via **File → Load file → Parse C header file…** , or programmatically:



idaapi.parse_decls(None, "/path/to/exported.h", None, idaapi.HTI_FIL)

You will find your types imported to **Local Types,** exactly as they were in the original IDB.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-39-43-3910-PM.png?width=1600&height=981&name=undefined-Jan-06-2026-07-39-43-3910-PM.png)

### **Ignore anonymous types (clean header export)**

Consider the following type:

struct with_inline_struct_t

{

struct

{

int a;

} b;

};

Parsing it produces hash-named intermediate types:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Jan-06-2026-07-40-04-2999-PM.png?width=1600&height=345&name=undefined-Jan-06-2026-07-40-04-2999-PM.png)

To avoid such types in exported headers:

**GUI**

  * Open **File → Produce file → Create C header file…**
  * Enable **Ignore IDA anonymous types**



The exported header file will contain the original type, which is fully re-parsable:

/* 35 */

struct with_inline_struct_t

{

struct

{

int a;

} b;

};

**IDAPython**

import ida_typeinf

class def_sink(ida_typeinf.text_sink_t):

def __init__(self):

ida_typeinf.text_sink_t.__init__(self)

def _print(self, defstr):

print(defstr)

return 0

ida_typeinf.print_decls(def_sink(), None, [], ida_typeinf.PDF_NO_ANON_NAME)

This inlines anonymous structs and unions directly into their parent types, producing headers that are easier to read and more suitable for reuse in other tools.

### **tilib**

Existing idaclang-based workflows will continue to work, but tilib provides a more direct path that matches how types are parsed and represented inside IDA itself. For consistency and long-term maintainability, new scripts and automation are expected to use tilib.

**Examples**

**C++ headers**  


tilib -ch/path/to/header.hpp -TC -P -I/path/to/includes out.til

  * -TC enables the Clang parser
  * -P enables C++ mode
  * -I adds include directories



**Objective-C headers**

tilib -ch/path/to/header.h -TC -CT-xobjective-c -I/path/to/SDK/usr/include out.til

  * -CT-xobjective-c passes the language to Clang



**Objective-C++ headers**

tilib -ch/path/to/header.mm -TC -CT-xobjective-c++ -I/path/to/SDK/usr/include out.til

Wrapping it Up

Together, these changes make the type system in IDA 9.3 more reliable in your daily workflows, particularly for Objective-C and fixed-layout types. If you have real-world headers that still don’t round-trip cleanly, we’d love to hear about them.

---

## 8. 

**Date:** Posted: Jan 2, 2026  
**Link:** [https://hex-rays.com/blog/incentive-programs-overview](https://hex-rays.com/blog/incentive-programs-overview)

[Back](<https://hex-rays.com/blog>)

[IDA Pro Plugin](<https://hex-rays.com/blog/tag/ida-pro-plugin>) [IDA Classroom](<https://hex-rays.com/blog/tag/ida-classroom>) [Content Collaboration](<https://hex-rays.com/blog/tag/content-collaboration>) [Plugin Contest](<https://hex-rays.com/blog/tag/plugin-contest>) [Contributor Program](<https://hex-rays.com/blog/tag/contributor-program>) [Bug Bounty](<https://hex-rays.com/blog/tag/bug-bounty>)

# Incentive Programs: Engage, Contribute, and Get Rewarded

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/incentive-programs-overview>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/incentive-programs-overview>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/incentive-programs-overview>)

Justine Benjamin ✦ Posted: Jan 2, 2026

![Incentive Programs: Engage, Contribute, and Get Rewarded](https://hex-rays.com/hubfs/Incentive%20Programs-1.png)

Our tools thrive because of the people who use them. From plugin developers to educators to security researchers, the Hex-Rays community pushes IDA and its ecosystem forward every day.

We've recently expanded and refined the programs that support you: Contributor Program, IDA Plugin Contest, Bug Bounty Program, and IDA Classroom. 

So whether you’re coding plugins, competing in CTFs, hardening security, or shaping the next wave of talent, each program provides recognition, resources, and a path to build your reputation while advancing the state of reverse engineering.

* * *

### Contributor Program 

**What It Is**  
A new initiative created to recognize and support developers who enhance the IDA ecosystem. Until now, it’s only been shared by email with existing plugin authors, and this is the first public introduction.

Learn more: <https://hex-rays.com/contributor-program>

**Why It Matters**  
If you’re actively maintaining or planning a plugin, or you’ve built something that helps IDA users, we’d love to hear from you. Participants can expect dedicated support, early technical information, and recognition from Hex-Rays.

**What You Get**

  * Access to Hex-Rays technical support for contributors

  * Early looks at API changes and feature roadmaps

  * Recognition on our website, blog, and social channels

  * Free or discounted licenses for qualifying projects




* * *

### IDA Plugin Contest 

**What It Is**  
Our flagship competition for plugin developers. We’ve beefed up the prizes and streamlined the entry process to make it easier than ever to participate. We currently run this on an annual basis, but may increase the frequency in the future.

Learn more: <https://hex-rays.com/plugin-contest>

**Why It Matters**  
This contest celebrates the creativity and skill of developers building on top of IDA, and helps surface innovative tools that benefit the entire community.

**What You Get**

  * Cash, free licenses, and exclusive conference passes for 1st place

  * Easier, faster submission process

  * Promotion of your plugin to the Hex-Rays audience

  * A spotlight opportunity to show off your work




📅 Submission Deadline is January 15, 2026!

Keep and eye on your inbox or our social channels for this rounds' winners and the next submission window.

* * *

### Content Collaborator

**What It Is**

This program is designed to recognize and support community members who create meaningful content around IDA. Historically, this has been an invite-only initiative, shared directly with select IDA power users producing high-quality blog posts or video content. This is the first time we’re introducing it publicly.

Contributions can take many forms, such as (but not limited to!) case studies, CTF write-ups, workflow breakdowns, or videos demonstrating how IDA is used in real-world analysis. 

➥ Some examples of our content collaborations can be found [here](<https://x.com/bellis1000>), [here](</blog/reversing-hanwha-security-cameras-a-deep-dive-by-matt-brown>) and [here](</blog/streamlining-vulnerability-research-idalib-rust-bindings>).

To inquire about a potential collab, contact: [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)

**Why It Matters**

IDA’s ecosystem is strengthened by people who share how they solve real problems. If you’re already building workflows, researching vulnerabilities, or exploring binaries in depth, your experience can help others learn faster and more effectively.

This collaboration program creates a direct line between content creators and Hex-Rays, offering support, visibility, and early insight to help you continue (or start) sharing your work with the community.

**What You Get**

  * A free IDA license for individuals creating qualifying content

  * Access to Hex-Rays technical support

  * Early visibility into upcoming features (via Beta Program)

  * Publication on our website, inclusion in our newsletters and recognition on our social channels




* * *

### Bug Bounty Program 

**What It Is**  
An official channel for the community to share potential vulnerabilities in Hex-Rays products. Submit your findings, help improve our software, and get rewarded for your contribution.

Learn more: <https://hex-rays.com/bug-bounty>

**Why It Matters**  
We’ve clarified our reporting process and response timelines, making it easier to submit potential security issues. We’ve also recently rewarded two researchers for responsibly disclosing vulnerabilities.

And, you could be next.

**What You Get**

  * Up to 3,000 USD bounty for validated security bugs
  * Transparent, faster response times for reports
  * Public acknowledgment of your contributions (with permission)

  * A direct line of communication with the Hex-Rays security team



* * *

### IDA Classroom

**What It Is**  
A global education initiative that supports universities, research institutes, and training organizations by providing free IDA licenses for teaching reverse engineering and related disciplines.

Learn more: <https://hex-rays.com/classroom>

**Why It Matters**  
This initiative enables the next generation of reverse engineers to gain hands-on experience with industry-standard tools, while providing educators with the resources they need to succeed.

**What You Get**

  * Free IDA Classroom licenses or heavily discounted IDA Pro licenses

  * Access to teaching resources and technical support

  * The ability to integrate a real-world reverse engineering tool into your curriculum

  * Recognition as an IDA Classroom Partner on our site




Questions? Email [classroom@hex-rays.com](<mailto:classroom@hex-rays.com>)

* * *

### How to get involved

Whether you’re building plugins, entering our annual contest, reporting security findings, or teaching the next generation, these programs are our way of saying thank you — and making it easier to collaborate with Hex-Rays. Our programs offer tangible benefits designed to support your work and showcase your contributions.

We can’t wait to see what you build, discover, and teach next.

---

## 9. 

**Date:** Posted: Dec 31, 2025  
**Link:** [https://hex-rays.com/blog/ida-online-training-2026-dates-available](https://hex-rays.com/blog/ida-online-training-2026-dates-available)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA Pro Training](<https://hex-rays.com/blog/tag/ida-pro-training>)

# IDA Online Training: 2026 Dates Available

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-online-training-2026-dates-available>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-online-training-2026-dates-available>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-online-training-2026-dates-available>)

Justine Benjamin ✦ Posted: Dec 31, 2025

![IDA Online Training: 2026 Dates Available](https://hex-rays.com/hubfs/Maintenon%20-%20grey%20waves%20-%202x.png)

Our 2026 IDA Online Training Schedule is live!

Before we dive into what’s ahead, we’d like to extend a sincere thank you to everyone who joined one of our IDA training courses last year. With hundreds of participants across our programs, your questions, feedback, and real-world use cases continue to shape how we design and deliver our training.

Based on that input, we’ve refined several aspects of the training experience for 2026 — from format and logistics to course materials and exercises. Below is an overview of what’s new, along with details on how to register.

Our training program is designed to support IDA users at every stage. Whether you’re new to the tool, building confidence through regular use, or pushing into advanced workflows, there’s a course tailored to your experience level.

>>> You can view the 2026 schedule by scrolling down or visiting our [Training Page](</training>). All registrations are handled directly through the Training Page. <<<

# **⭐** Starter Training: Moving to On-Demand ⭐

Beginning in 2026, our **Starter IDA training** , _Learning the Fundamentals_ , will be offered **exclusively as an on-demand course**. We’ve made the decision to retire live, virtual Starter sessions in favor of a self-paced format that gives new users more flexibility and a smoother learning experience.

The on-demand Starter course will be available in **Spring 2026** and will cover the same foundational IDA workflows, concepts, and best practices with the added benefit of learning at your own pace, revisiting sections as needed, and fitting training into your schedule.

This change also allows our live instruction to focus more heavily on Intermediate and Advanced users, where interactive discussion and hands-on guidance provide the greatest value.

## In-Person Training at RE Conferences

While we won’t be hosting standalone in-person classes this year, you can still find us teaching **in person at select reverse engineering and security conferences throughout the year**.

These conference-based trainings allow us to deliver focused, high-impact sessions alongside industry events many of you already attend. They also give participants the opportunity to learn in a highly technical environment while connecting with peers from the broader RE community.

Details on upcoming conference trainings—including locations, formats, and registration links—will be shared as they are finalized. If there’s a specific event you’d like us to participate in, feel free to reach out and let us know.

# Course Materials & Exercises

All course materials are now hosted on dedicated, private webpages specific to each course type. This provides a centralized location for schedules, slides, exercises, and supporting resources. This makes access simpler while maintaining attendee privacy.

For Intermediate and Advanced courses, we’re continuing to expand hands-on content, including more interactive exercises such as CTF-style challenges to reinforce real-world analysis workflows.

# Private Training Options

Looking to train a team? We offer private classes delivered online or in person. For private sessions, we recommend a minimum of:

  * **5 participants** for online training

  * **10 participants** for in-person training




That said, we’re happy to discuss customized arrangements based on your organization’s needs. To start the conversation, email **training@hex-rays.com.**

# Discounts

  * **Early Bird Registration** is now open. Use code **EARLY2026** to receive **15% off** eligible courses.

    * For courses held **February–June 2026** , the offer expires **21 February 2026**

    * For courses held **September–December 2026** , the offer expires **3 September 2026**

  * Planning to attend more than one course? Use code **MULTI2026** to receive **20% off** with our multi-course discount.

  * Attended a course in 2025? You may qualify for a returning-student discount. Check your inbox for details — and if you didn’t receive an email, just reach out and we’ll help you out.




If you have any questions, please don't hesitate to reach out to [training@hex-rays.com.](<mailto:training@hex-rays.com>)

### **2026 IDA Online Training Schedule**

Time Zone |  Online Course |  Start Date |  End Date |  Times & Zones  
---|---|---|---|---  
Paris |  Intermediate |  7-Apr-2026 |  9-Apr-2026 |  10am - 6pm CET  
Paris |  Advanced Malware |  14-Apr-2026 |  - |  10am - 6pm CET  
Paris |  Advanced Decompiler |  12-May-2026 |  13-May-2026 |  10am - 4pm CET  
Paris |  Advanced Programming |  26-May-2026 |  27-May-2026 |  10am - 4pm CET  
New York |  Intermediate |  12-May-2026 |  14-May-2026 |  10am - 6pm ET  
New York |  Advanced Malware |  12-May-2026 |  - |  10am - 6pm ET  
New York |  Advanced Decompiler |  2-Jun-2026 |  3-Jun-2026 |  10am - 6pm ET  
New York |  Advanced Programming |  16-Jun-2025 |  17-Jun-2025 |  10am - 6pm ET  
|  |  |  |   
Paris |  Intermediate |  29-Sep-2026 |  1-Oct-2026 |  10am - 6pm CET  
Paris |  Advanced Malware |  27-Oct-2026 |  - |  10am - 6pm CET  
Paris |  Advanced Decompiler |  12-Nov-2026 |  13-Nov-2026 |  10am - 6pm CET  
Paris |  Advanced Programming |  24-Nov-2026 |  25-Nov-2026 |  10am - 6pm CET  
New York |  Intermediate |  13-Oct-2026 |  15-Oct-2026 |  10am - 6pm ET  
New York |  Advanced Malware |  17-Nov-2026 |  - |  10am - 6pm ET  
New York |  Advanced Decompiler |  1-Dec-2026 |  2-Dec-2026 |  10am - 6pm ET  
New York |  Advanced Programming |  15-Dec-2026 |  16-Dec-2026 |  10am - 6pm ET

---

