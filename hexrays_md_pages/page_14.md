# Hex-Rays Blog – Page 14

**Archived:** 2026-02-20 12:13
**URL:** https://hex-rays.com/blog/page/14

---

## 1. 

**Date:** Posted: May 12, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-140-loading-pdb-types](https://hex-rays.com/blog/igors-tip-of-the-week-140-loading-pdb-types)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #140: Loading PDB types

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-140-loading-pdb-types>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-140-loading-pdb-types>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-140-loading-pdb-types>)

I 

Igor Skochinsky ✦ Posted: May 12, 2023

![Igor’s Tip of the Week #140: Loading PDB types](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

While IDA comes with a rich set of [type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>) for Windows API, they don’t cover the whole set of types used in Windows. Our libraries are based on the official Windows SDK/DDK headers, which tend to only include public, stable information which is common to multiple Windows versions. A new Windows build may introduce new types or use some of the previously reserved fields. Because some of these structures are critical for proper debugging, Microsoft usually publishes a subset of actual, up-to-date types in the PDBs for the core Windows modules (`kernel32.dll` and `ntdll.dll` for user mode, `ntoskrnl.exe`for kernel mode). Thus, usually you can use these files to get types matching the Windows version you’re analyzing.

### Loading types from PDB

To load an additional PDB file, use File > Load file > PDB File…

Here, you can specify either an already downloaded PDB, or a path to .exe or .dll. In the latter case, IDA will try to fetch the matching PDB from the symbol servers. Because we’re loading the PDB which does not actually match the currently loaded file, check “Types only” so that the global symbols from it are not applied unnecessarily.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdbtypes2-3.png?width=753&height=265&name=pdbtypes2-3.png)

After downloading and processing the PDB, the new types can be consulted in the Local Types view.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdbtypes3-3.png?width=955&height=855&name=pdbtypes3-3.png)

See also:

[Igor’s tip of the week #55: Using debug symbols](<https://hex-rays.com/blog/igors-tip-of-the-week-55-using-debug-symbols/>)

---

## 2. 

**Date:** Posted: May 11, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-ntrays](https://hex-rays.com/blog/plugin-focus-ntrays)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: NtRays

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-ntrays>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-ntrays>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-ntrays>)

A 

Alex Petrov ✦ Posted: May 11, 2023

![Plugin focus: NtRays](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

**This is a guest entry written by Can Bölük. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author.**

## NtRays: Reversing Windows kernel, simplified

Windows kernel has changed a lot in the past few years, with the addition of Hypervisor enhancements, security mitigations, scheduler hints, and general performance optimizations, it has become much snappier and more secure. However, combined with inlining, this also means that it has become increasingly more complicated to understand with each passing week, seemingly with no end.

NtRays is an open-source IDA plugin using Hex-Ray's powerful microcode hooks to help you read through the inlined boilerplate code with a simplified pseudo-code output reminiscent of the Windows XP era, with a few extra features to help in any kind of kernel mode reverse engineering.

### Using NtRays

You can install NtRays through a single drag and drop into the plugins folder, either by using the pre-compiled release from the [Github repository](<https://github.com/can1357/NtRays>) or by compiling a dll from source.

Following the installation, the next time you launch IDA Pro, you will have its entry under `Edit > Plugins > NtRays`, from which you can simply toggle it on or off.

![Plugins entry](https://hex-rays.com/hubfs/Imported_Blog_Media/WeRxV-3.png)

## Features

### Scheduler assist & Perf instrumentations

Take the function `KeReleaseInterruptSpinLock` as an example, I mean, it does sound like a simple one, doesn't it? Here's a comparison between Windows 7 and the latest Windows 11 kernels.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/tzfbN-3.png)

Now imagine if this was inlined into a much more complex subroutine with multiple calls, you'd basically be combing through the boilerplate to follow the actual logic, which is, unfortunately, what looking at NT kernel feels like these days.

By utilizing Hex-Ray's microcode optimizations, NtRays turns the Windows 11 version into a measly 7 lines.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/DcVSM-3.png)

### Memory manager: Dynamic relocations

NT kernel has two very special constants, namely the PTE base and the PFN database. Nowadays, with KASLR (Kernel-Mode Address Space Layout Randomization), they aren't constants at all, but for performance reasons, the MS compiler keeps them as constants (propagating any arithmetic as well) and instead puts the relocation information in the PE header for the bootloader to patch during startup.

Naturally, this becomes a nightmare when you try to understand how the memory manager works, consider `MmGetVirtualForPhysical`, for instance, which does nothing more than looking up a field in the PFN database and subtracting it from the PTE base.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/3dBcf-3.png)

This time, NtRays modifies the C-level tree to give you type information as well as clear names for the constants.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/wdyo6-3.png)

### KUSER_SHARED_DATA

On the topic of constants, another one is the address of `KUSER_SHARED_DATA`, which is a structure with two constant addresses, one for user-mode and one for kernel-mode, holding global system information such as the time.

This is lifted as a global variable with the proper type applied instead of another annoying constant.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/UBKQJ-3.png)

### Mitigations

Following the Spectre/Meltdown vulnerability, every OS vendor targeting chips with speculative execution features (Hint: all of them) had to implement certain mitigations. Naturally, this ended up in the NT kernel as well.

#### KPTI: Shadow page tables

To secure kernel-mode memory, NT started keeping two sets of page tables per process, this means every page table access is now followed by a very long branch doing the exact same operation. A comparison of `MiInitializePfn` with and without NtRays demonstrates how distracting it can be.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/XCXwU-3.png)

#### RSB flushes

Return stack buffer was another concern, so the kernel started inserting RSB flush gadgets to the start of every mode-switch, such as the one below.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/7pgKR-3.png)

This used to prevent decompilation entirely. NtRays instead lifts this sequence into an imaginary intrinsic.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/KAerP-3.png)

### Interrupt Service Routines

Interrupt service routines are the bread and butter of any operating system. However, they are usually written in assembly with an irregular calling convention, and a C decompilation may not represent what's going on very clearly.

To help clear things up, once again, we use the Hex-Rays microcode coupled with the powerful adjusted pointer primitive in the IDA type system.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/Gl1Zq-3.png)

In this example, you can also see the additional non-standard intrinsics NtRays implements for OS-level code such as the `__swapgs`.

Although the IDA inline assembly representation is often adequate, from time to time, it does become problematic due to variables being forced into register-names, instructions like `int 2c` not being marked `noreturn`, etc.

This functionality is extended to many system instructions, such as:

  * **__assert_fail:** int 2C
  * **__cpuid:** cpuid
  * **__xgetbv:** xgetbv
  * **__xsetbv:** xsetbv
  * **__clac:** clac
  * **__stac:** stac
  * **__swapgs:** swapgs
  * **__saveprevssp:** saveprevssp
  * **__setssbsy:** setssbsy
  * **__endbr64:** endbr64
  * **__endbr32:** endbr32
  * **__incsspq:** incsspq
  * **__incsspd:** incsspd
  * **__rstorssp:** rstorssp
  * **__wrssd:** wrssd
  * **__wrssq:** wrssq
  * **__wrussd:** wrussd
  * **__wrussq:** wrussq
  * **__clrssbsy:** clrssbsy
  * **_mm_clflushopt:** clflushopt
  * **_mm_clwb:** clwb
  * **__vmclear:** vmclear
  * **__vmlaunch:** vmlaunch
  * **__vmptrld:** vmptrld
  * **__vmptrst:** vmptrst
  * **__vmwrite:** vmwrite
  * **__vmxoff:** vmxoff
  * **__vmxon:** vmxon
  * **_invept:** invept
  * **_invvpid:** invvpid
  * **_invpcid:** invpcid
  * **_invlpga:** invlpga
  * **_xsaves:** xsaves
  * **_xrstors:** xrstors
  * **_mm_prefetcht0:** prefetcht0
  * **_mm_prefetcht1:** prefetcht1
  * **_mm_prefetcht2:** prefetcht2
  * **_mm_prefetchnta:** prefetchnta
  * **_rdrand:** rdrand
  * **_rdseed:** rdseed
  * **__rdsspd:** rdsspd
  * **__rdsspq:** rdsspq
  * **__vmread:** vmread
  * **__iretq:** iretq
  * **__sysretq:** sysretq



### Closing

NtRays can be very useful for Windows reverse engineering both in kernel-mode and user-mode. However, it's important to note that some optimizations may discard important information depending on the area of focus in your research.

If you find yourself trying to understand how the shadow table implementation works and end up confused due to all the missing code, it might be a good idea to hit the global toggle to see the real implementation!

* * *

NtRays source code comes with a very easy-to-use wrapper around the IDA SDK, [HexSuite](<https://github.com/can1357/HexSuite>), and an average optimizer for scenarios demonstrated above ends up somewhere between 20 to 30 lines.

If you find yourself dealing with more boilerplate code that could be lifted in a similar fashion, I highly encourage modifying the code base and, if you are willing, sending a pull request, which is always very welcome!

[Get the NtRays plugin](<https://github.com/can1357/NtRays>)

---

## 3. 

**Date:** Posted: May 5, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-139-license-borrowing](https://hex-rays.com/blog/igors-tip-of-the-week-139-license-borrowing)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #139: License borrowing

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-139-license-borrowing>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-139-license-borrowing>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-139-license-borrowing>)

I 

Igor Skochinsky ✦ Posted: May 5, 2023

![Igor’s Tip of the Week #139: License borrowing](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Floating licenses allow additional flexibility for companies with many IDA users: IDA can be installed on as many computers as required, but only a limited number of copies can run simultaneously. 

This flexibility its downsides: IDA needs to have permanent connection to your organization’s license server which may make things problematic in some situations (e.g. working on an isolated network or in the field/while traveling). While the first issue can be handled by placing the license server inside that network, accessing the company network during travel may be problematic or impossible. In such situations, you can use license borrowing.

Borrowing allows the user to check out the license for a fixed period and work without connection to the server during that time.

### Borrowing licenses

To borrow a license, in a floating-license IDA go to Help > Floating licenses > Borrow licenses…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/borrow1-3.png?width=524&height=286&name=borrow1-3.png)

You will get a dialog like the following:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/borrow2-3.png?width=764&height=368&name=borrow2-3.png)

Here you can pick which licenses you want to borrow and the borrow period end date. By default, IDA offers one week but you can make it shorter or longer (by default we limit the maximum borrow period to 6 months but it can be limited further by the license server administrator). 

If you click “Borrow”, you should see this confirmation:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/borrow3-3.png?width=372&height=195&name=borrow3-3.png)

and the details in the Output window:
    
    
    Successfully borrowed licenses:
    IDAPROFW (IDA Pro) [currently borrowed until 2023-05-12 23:59]
    

After this, you can disconnect from the network and IDA will continue working until the specified date.

NB: once borrowed, the license(s) remain checked out (“In Use”) on the license server and will not become available for others until the end of the borrow period or early return.

### Returning licenses

If you need to return borrowed licenses early (before the end of the borrow period):

  1. Reconnect to the network with the server from which you borrowed the license
  2. Go to Help > Floating licenses > Return licenses…  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/borrow4-3.png?width=579&height=317&name=borrow4-3.png)
  3. select the license(s) to return and click “Return and Exit”.
  4. IDA will exit since it has returned the license, but you can start it again to use the license server in online mode or borrow again for another period.



### Borrowing and returning licenses from command line

If you prefer using command line, check the [corresponding section on our support page](<https://hex-rays.com/products/ida/support/flexlm/#borrow>).

See also:

[Floating Licenses](<https://hex-rays.com/products/ida/support/flexlm/>)

---

## 4. 

**Date:** Posted: Apr 28, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-138-pointer-math-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-138-pointer-math-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #138: Pointer math in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-138-pointer-math-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-138-pointer-math-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-138-pointer-math-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Apr 28, 2023

![Igor’s Tip of the Week #138: Pointer math in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

While working with decompiled code and retyping variables (or sometimes when they get typed by the decompiler automatically), you might be puzzled by the discrepancies between pseudocode and disassembly.

Consider the following example:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ptrmath1-3.png?width=1041&height=333&name=ptrmath1-3.png)

We see that `X22` is accessed with offset 0x10 (16) in the disassembly but 2 in the pseudocode. Is there a bug in the decompiler?

In fact, there is no bug. The difference is explained by the C/C++pointer/array referencing rules: the array indexing or integer addition operation advances the pointer value by the value of index _multiplied by the element size_. In this case, the type of `v4` is `_QWORD*`, which means that elements are `_QWORD`s (64-bit or 8-byte integers). Thus, 2*8=16(0x10), which matches the assembly code.

To confirm what’s really going on, you can do “Reset pointer type” on the variable so that it reverts to the generic integer variable and the decompiler is forced to use raw byte offsets:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ptrmath2-3.png?width=1048&height=331&name=ptrmath2-3.png)

See also:

[Igor’s Tip of the Week #117: Reset pointer type](<https://hex-rays.com/blog/igors-tip-of-the-week-117-reset-pointer-type/>)

[Igor’s tip of the week #42: Renaming and retyping in the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)

[Igor’s Tip of the Week #118: Structure creation in the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-118-structure-creation-in-the-decompiler/>)

---

## 5. 

**Date:** Posted: Apr 25, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-ttddbg](https://hex-rays.com/blog/plugin-focus-ttddbg)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: ttddbg

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-ttddbg>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-ttddbg>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-ttddbg>)

A 

Alex Petrov ✦ Posted: Apr 25, 2023

![Plugin focus: ttddbg](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

**This is a guest entry written by Simon Garrelou and Sylvain Peyrefitte from the Airbus CERT Team. Their views and opinions are their own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the authors.**

# Power up your debugging with time travel: the ttddbg plugin

[Time Travel Debugging](<https://blogs.windows.com/windowsdeveloper/2017/09/27/time-travel-debugging-now-available-windbg-preview/>) is an innovative way of debugging programs by recording their state at every step of the execution, allowing analysts to move freely around the execution timeline. Several time-travelling debuggers already exist for a variety of programming languages, but most of them require access to the source code of the program being analyzed.

WinDbg's TTD feature is different: it works at the lowest possible level, recording the execution of a program opcode by opcode. This means that it can be used on programs for which we do not have access to the source code, such as malware samples.

[ttddbg](<https://github.com/airbus-cert/ttddbg>) brings the power of Microsoft WinDbg's Time-Travel Debugging to IDA. Using the native TTD API, [reverse engineered](<https://github.com/commial/ttd-bindings>) by [commial](<https://ajax.re/>), we were able to interface directly with WinDbg's TTD libraries and load trace files. This was then used to create a brand new debugger plugin, integrating it in IDA directly. Using ttddbg, you can navigate WinDbg TTD trace files in IDA, integrating all your reverse engineering work such as recovered structs, function and variable names, etc.

## Using ttddbg

ttddbg relies on the TTD API exposed by WinDbg Preview, which is contained in two Dynamic Link Libraries (DLL). For legal reasons, we cannot directly distribute these files. However, we have automated the process of getting a copy of these DLLs as much as possible: you only need to install [WinDbg Preview](<https://www.microsoft.com/store/apps/9pgjgd53tn86>) from the Microsoft Store yourself. Installing ttddbg is made easy by using our [automated installer](<https://github.com/airbus-cert/ttddbg/releases>). Simply click Next until it's done! The installer will automatically copy the required DLLs from WinDbg's installation folder.

![Screenshot of IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/ttddbg-1-3.png) Screenshot of IDA 

The next time you launch IDA, you should see a new debugger appearing in the usual place called "ttddbg". Starting a debugging session with this debugger requires a _trace file_ , which can be [generated from WinDbg](<https://github.com/airbus-cert/ttddbg/blob/main/HOWTO_TIME_TRAVEL.md>). Change the path to the "Application" to your trace file, and carry on with the debugging.

From there, you can debug like you usually do: step through the execution, set breakpoints, observe the memory…but you can also use some new features from the debugging toolbar:

![Screenshot of icons](https://hex-rays.com/hubfs/Imported_Blog_Media/ttddbg-2-3.png) Screenshot of icons 

### Backwards steps

The first and third icons are similar to the well-known "Continue" and "Step into" icons, albeit reversed. They will allow you to rewind execution backwards until you hit a breakpoint, or to go back in the execution flow instruction by instruction.

The second icon allows you to perform a full run of the application, from start to finish.

Combined with the regular stepping features, this allows you to move through the execution trace using well-known tools. And when you encounter an interesting part, you can save its context by using the Timeline feature.

### Timeline

The third icon opens up the "Timeline" window, which records points in time that you might find interesting. By default, it is populated with every module load and thread creation event which happened during the process life.

![Screenshot of timeline](https://hex-rays.com/hubfs/Imported_Blog_Media/ttddbg-3-3.png) Screenshot of timeline 

You can freely edit this list with the "Delete" and "Insert" keys. It is also saved into the IDB: if you share it with someone else (or if you use [IDA Teams](<https://hex-rays.com/ida-teams/>)), your colleagues can see the results of your analysis right away.

Double-clicking on a timeline entry will jump straight to this time point in the debugger.

### Function tracing

The last two icons are part of our latest feature: function tracing. You can mark functions to be traced by the TTD engine by right-clicking on a function in the dedicated window (Shift-F3). All currently traced functions can be seen in the "Function tracing" window.

Any time the engine encounters a "call" instruction pointing towards a traced function, it will be recorded by ttddbg. We also automatically extract the arguments to the function, and use the type information from IDA to pretty-print the result if possible.

When a "ret" instruction happens in a traced function, it is also recorded, and its return value as well.

All trace events can be seen using the "Tracing events" window. From there, you can also copy the address to every function call argument, which allows you to go directly to it in memory if you wish.

![Screenshot of tracing events](https://hex-rays.com/hubfs/Imported_Blog_Media/ttddbg-4-3.png) Screenshot of tracing events 

## Keep in mind

While it can be very useful, ttddbg has a few limitations to keep in mind:

  * There is no guarantee on Microsoft's side that the TTD API will remain stable. An update to WinDbg might totally break the interoperability one day.
  * Even when inside the debugger, you are not actually executing any of the code. While this is a good thing from a security point of view, it also means that you cannot change the state of the program like you could with a real debugger (i.e. changing conditional jumps, patching bytes, etc.)

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/plugin-repo-Jun-18-2024-08-47-48-4432-AM.png?width=530&height=324&name=plugin-repo-Jun-18-2024-08-47-48-4432-AM.png)](<https://plugins.hex-rays.com>)

---

## 6. 

**Date:** Posted: Apr 21, 2023  
**Link:** [https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-5](https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-5)

[Back](<https://hex-rays.com/blog>)

[Trivia](<https://hex-rays.com/blog/tag/trivia>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Quiz](<https://hex-rays.com/blog/tag/quiz>)

# IDA Trivia – Test your IDA knowledge! DAY 5

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-5>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-5>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-5>)

A 

Alex Petrov ✦ Posted: Apr 21, 2023

![IDA Trivia – Test your IDA knowledge! DAY 5](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

The **#IDATrivia game! – Day 5**

All questions are related to IDA. Read them carefully, check our website, take a look at Igor’s tips, and send your guesses to [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>).  
**Attention!** Submissions made after the deadlines will not be considered. The deadlines will be published with each round of questions. If all your answers are correct, you will participate in a raffle, and on the following day, we will pick and announce a winner. Each day we will give a different prize. Wondering what the awards are? That’s a mystery… and we will keep it secret. If you win, you will find out what your prize is at the time of the delivery.

If you think you are an IDA whiz, this should be a walk in the park for you. So let’s find that out…

###  **Round 5**

**Date: 21 April**

**Questions:**

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/trivia_blog_5_2-3.png?width=689&height=324&name=trivia_blog_5_2-3.png)

9\. What would happen if you enter a “+100” prefix in the Jump to address dialog?  
10\. What command should you use if you want to see the raw offsets from a pointer in the decompiler?

**Deadline for submissions:[22 April, 1 pm CEST](<https://www.timeanddate.com/worldclock/converter.html?iso=20230422T110000&p1=137&p2=179&p3=136&p4=48&p5=49&p6=tz_jst&p7=240>)**

### **General Terms & Conditions:**

[See the initial post](<https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge/>)

Should you have any complaints or questions, please get in touch with [marketing@hex-rays.com ](<mailto:marketing@hex-rays.com>)

---

## 7. 

**Date:** Posted: Apr 21, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-137-processor-modes-and-segment-registers](https://hex-rays.com/blog/igors-tip-of-the-week-137-processor-modes-and-segment-registers)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #137: Processor modes and segment registers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-137-processor-modes-and-segment-registers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-137-processor-modes-and-segment-registers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-137-processor-modes-and-segment-registers>)

I 

Igor Skochinsky ✦ Posted: Apr 21, 2023

![Igor’s Tip of the Week #137: Processor modes and segment registers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Some of the processors supported by IDA support different ISA variants, in particular:

  * ARM processor module supports the classic 32-bit ARM instructions (A32), 16-bit Thumb or mixed 16/32-bit Thumb32 (T32) , as well as 64-bit A64 instructions (A64)
  * PPC processor module supports the standard 32-bit PowerPC instructions and mixed 16/32-bit Variable Length Environment (VLE)
  * MIPS module supports the classic 32-bit instructions as well as the compressed variants MIPS16 and microMIPS



Because sometimes these instructions sets may be present in the same binary, IDA needs a way to determine which subset to use. For this, it repurposes _segment registers_ , originally used on 16-bit x86 processors to extend the 16-bit addressing. For example, if you load an ARM firmware binary, you will see the following informational box:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/procmode1-3.png?width=659&height=420&name=procmode1-3.png)

In many cases, IDA is able to determine the correct processor mode by analyzing the code and determining mode switch sequences (e.g. BX/BLX instructions), but you can also force its decision by using the described shortcut `Alt`–`G` (if you prefer menus, you can find it in Edit > Segments > Change segment register value…).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/procmode2-3.png?width=702&height=249&name=procmode2-3.png)

In the dialog, select the **T** register and specify `0` for ARM mode or `1` for Thumb (includes Thumb32 aka Thumb-2).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/procmode3-3.png?width=384&height=252&name=procmode3-3.png)

You can observe mode switches in the disassembly listing by the `CODE32`/`CODE16` directives (usually text view only):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/procmode5-3.png?width=839&height=449&name=procmode5-3.png)

If you need a global overview, use the View> Open subviews > Segment registers…. (`Shift`–`F8`) view or its modal version Jump > Jump to segment (`Ctrl`–`G`):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/procmode4-3.png?width=660&height=810&name=procmode4-3.png)

The _Tag_ column gives a hint on how the specific changepoint was created: **a** denotes a changepoint added by IDA during autoanalysis while **u** is used for those specified by the user (or, sometimes a plugin).

If necessary, wrong changepoints can be deleted from the list (even many at a time, using the selection). When a change point is deleted, IDA uses the value of a preceding one (or the default for the current segment).

For MIPS, the **mips16** pseudoregister is used to switch between standard MIPS and MIPS16 or microMIPS, and for PPC, **vle** is used to enable decoding of VLE instructions.

See also:

[IDA Help: Segment Register Change Points](<https://www.hex-rays.com/products/ida/support/idadoc/524.shtml>)

[IDA Help: Jump to the specified segment register change point](<https://www.hex-rays.com/products/ida/support/idadoc/547.shtml>)

---

## 8. 

**Date:** Posted: Apr 20, 2023  
**Link:** [https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-4](https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-4)

[Back](<https://hex-rays.com/blog>)

[Trivia](<https://hex-rays.com/blog/tag/trivia>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Quiz](<https://hex-rays.com/blog/tag/quiz>)

# IDA Trivia – Test your IDA knowledge! DAY 4

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-4>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-4>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-4>)

A 

Alex Petrov ✦ Posted: Apr 20, 2023

![IDA Trivia – Test your IDA knowledge! DAY 4](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

The **#IDATrivia game! – Day 4**

All questions are related to IDA. Read them carefully, check our website, take a look at Igor’s tips, and send your guesses to [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>).  
**Attention!** Submissions made after the deadlines will not be considered. The deadlines will be published with each round of questions. If all your answers are correct, you will participate in a raffle, and on the following day, we will pick and announce a winner. Each day we will give a different prize. Wondering what the awards are? That’s a mystery… and we will keep it secret. If you win, you will find out what your prize is at the time of the delivery.

If you think you are an IDA whiz, this should be a walk in the park for you. So let’s find that out…

###  **Round 4**

**Date: 20 April**

**Questions:**

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/trivia_blog_4-3.png?width=689&height=324&name=trivia_blog_4-3.png)

7\. Which kind of items can be renamed directly in the pseudocode view? Name them all!  
8\. If the default radix option is set to “0”, what would that mean?

**Deadline for submissions:[21 April, 1 pm CEST](<https://www.timeanddate.com/worldclock/converter.html?iso=20230421T110000&p1=137&p2=179&p3=136&p4=48&p5=49&p6=tz_jst&p7=240>)**

### **General Terms & Conditions:**

[See the initial post](<https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge/>)

Should you have any complaints or questions, please get in touch with [marketing@hex-rays.com ](<mailto:marketing@hex-rays.com>)

---

## 9. 

**Date:** Posted: Apr 19, 2023  
**Link:** [https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-3](https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-3)

[Back](<https://hex-rays.com/blog>)

[Trivia](<https://hex-rays.com/blog/tag/trivia>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Quiz](<https://hex-rays.com/blog/tag/quiz>)

# IDA Trivia – Test your IDA knowledge! DAY 3

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-3>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-3>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge-day-3>)

A 

Alex Petrov ✦ Posted: Apr 19, 2023

![IDA Trivia – Test your IDA knowledge! DAY 3](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

The **#IDATrivia game! – Day 3**

All questions are be related to IDA. Read them carefully, check our website, take a look at Igor’s tips, and send your guesses to [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>).  
**Attention!** Submissions made after the deadlines will not be considered. The deadlines will be published with each round of questions. If all your answers are correct, you will participate in a raffle, and on the following day, we will pick and announce a winner. Each day we will give a different prize. Wondering what the awards are? That’s a mystery… and we will keep it secret. If you win, you will find out what your prize is at the time of the delivery.

If you think you are an IDA whiz, this should be a walk in the park for you. So let’s find that out…

###  **Round 3**

**Date: 19 April**

**Questions:**

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/trivia_blog_3-3.png?width=689&height=324&name=trivia_blog_3-3.png)

5\. You see the following message in the decompiler “positive sp value has been detected”. What disassembly setting could you use to detect the source of the problem?  
6\. What does the locret_ prefix mean in IDA?

**Deadline for submissions:[20 April, 1 pm CEST](<https://www.timeanddate.com/worldclock/converter.html?iso=20230420T110000&p1=137&p2=179&p3=136&p4=48&p5=49&p6=tz_jst&p7=240>)**

### **General Terms & Conditions:**

[See the initial post](<https://hex-rays.com/blog/ida-trivia-test-your-ida-knowledge/>)

Should you have any complaints or questions, please get in touch with [marketing@hex-rays.com ](<mailto:marketing@hex-rays.com>)

---

