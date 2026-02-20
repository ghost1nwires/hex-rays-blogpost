# Hex-Rays Blog – Page 11

**Archived:** 2026-02-20 12:13
**URL:** https://hex-rays.com/blog/page/11

---

## 1. 

**Date:** Posted: Sep 8, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-156-command-line-options-for-firmware-loading](https://hex-rays.com/blog/igors-tip-of-the-week-156-command-line-options-for-firmware-loading)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #156: Command-line options for firmware loading

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-156-command-line-options-for-firmware-loading>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-156-command-line-options-for-firmware-loading>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-156-command-line-options-for-firmware-loading>)

I 

Igor Skochinsky ✦ Posted: Sep 8, 2023

![Igor’s Tip of the Week #156: Command-line options for firmware loading](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Firmware binaries often use raw binary file format without any metadata so they have to be loaded manually into IDA. You can do it interactively using the [binary file loader](<https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader/>), but if you have many files to disassemble it can quickly get boring. If you already know some information about the files you’re disassembling, you can speed up at least the first steps. For example, if you have a binary for **big endian ARM** , which should be loaded at address **0xFFFF0000** , you can use the following command line:
    
    
    ida -parmb -bFFFF000 firmware.bin

The` -p` switch tells IDA which processor module to pre-select. You can see the available names for different processor types in the second column of the processor selector pane in the load dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fw_cmdline1-3.png?width=859&height=763&name=fw_cmdline1-3.png)

The `-b` switch specifies the load base to be used, however due to IDA’s origins as a DOS program, the value needs to be specified in _paragraphs_ (16-byte units), so we have to omit the last hexadecimal zero.

In case the file is recognized by IDA as some specific format, it will be used instead of the plain binary, but the processor specified will be retained if possible. For example, [since IDA 8.3](<https://hex-rays.com/products/ida/news/8_3/>) the firmware for Cortex-M processors is usually recognized as such out-of-box:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fw_cmdline2-3.png?width=651&height=512&name=fw_cmdline2-3.png)

If you prefer to have the file loaded as plain binary or another non-default format, you can force it using the `-T` switch with the unique prefix of the preferred format name:
    
    
    ida -parm -b800400 -Tbinary firmware.bin

(`-Tbin` would also work)

See also:

[IDA Help: Processor Type](<https://www.hex-rays.com/products/ida/support/idadoc/618.shtml>)

[IDA Help: Command line switches](<https://www.hex-rays.com/products/ida/support/idadoc/417.shtml>)

[Igor’s tip of the week #41: Binary file loader](<https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader/>)

---

## 2. 

**Date:** Posted: Sep 2, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-155-splitting-stack-variables-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-155-splitting-stack-variables-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #155: Splitting stack variables in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-155-splitting-stack-variables-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-155-splitting-stack-variables-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-155-splitting-stack-variables-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Sep 2, 2023

![Igor’s Tip of the Week #155: Splitting stack variables in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

We’ve covered [splitting expressions](<https://hex-rays.com/blog/igors-tip-of-the-week-69-split-expression/>) before, but there may be situations where it can’t be used.

For example, consider following situation:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitvar1-3.png?width=735&height=157&name=splitvar1-3.png)

The decompiler decided that the function returns a 64-bit integer and allocated a 64-bit stack varible for it. For example, the code may be manipulating a register pair commonly used for 64-bit variables (`eax:edx`) which triggers the heirustics for recovering 64-bit calculations. However, here it seems to be a false positive: we can see separate accesses to the low and high dword of the variable, and the third argument for the IndexFromId call also uses a pointer into the middle of the variable.

One option is to hint to the decompiler that the function returns a 32-bit integer by editing the function’s prototype (use “Set item type” or the `Y` shotrcut on the first line). 

Often this fixes the decompilation, but not here:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitvar2-3.png?width=711&height=156&name=splitvar2-3.png)

We still have a 64-bt variable on the stack at `ebp-10h`, so it’s worth inspecting the [stack frame](<https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view/>). It can be opened by pressing Ctrl-K in disassembly view or double-cliking stack variable in disassembly or pseudocode:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitvar3-3.png?width=537&height=328&name=splitvar3-3.png)

We see that there is a quadword (64-bit) variable at offset `-10`. it can be converted to 32-bit(dword) by pressing `D` three times. Another dword can be added in the same manner at offset `-C`:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitvar4-3.png?width=296&height=198&name=splitvar4-3.png)

After refreshing pseudocode, we can see improved output:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitvar5-3.png?width=622&height=138&name=splitvar5-3.png)

There’s only one small issue: `v5` became an array. This happened bcause passing an array or an address of a single integer produces the same code but there was a gap in the stack frame after `var_C`, so the decompiler decided that it’s actually an array. If you’re certain that it’s a single integer, you have the following options:

  1. Edit the stack frame again and define some variables after `var_C` so that there is no space for an array.
  2. retype v5 directly from the pseudocode (use Y and enter ‘int’).



Now the pseudocode looks correct and there is only one variable of correct size:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/splitvar6-3.png?width=594&height=143&name=splitvar6-3.png)

Note that in some cases a variable passed by address may be really an array, or a structure – in case of doubt inspect the called function to confirm how the argument is being used.

See also:

[Igor’s tip of the week #65: stack frame view](<https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view/>)

[Igor’s tip of the week #42: Renaming and retyping in the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)

---

## 3. 

**Date:** Posted: Aug 25, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-154-synchronized-views](https://hex-rays.com/blog/igors-tip-of-the-week-154-synchronized-views)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #154: Synchronized views

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-154-synchronized-views>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-154-synchronized-views>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-154-synchronized-views>)

I 

Igor Skochinsky ✦ Posted: Aug 25, 2023

![Igor’s Tip of the Week #154: Synchronized views](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

When working with a binary in IDA, most of the time you probably use one of the main views: disassembly (IDA View) or [decompilation](<https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics/>) (Pseudocode). If you need to switch between the two, you can use the `Tab` key – usually it jumps to the the same location in the other view. If you want to consult disassembly and pseudocode at the same time, [copying pseudocode to disassembly](<https://hex-rays.com/blog/igors-tip-of-the-week-153-copying-pseudocode-to-disassembly/>) is one option, however it is of rather limited usefulness. You can [dock](<https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts/>) two view side-by-side and Tab between them, but this can be rather tedious.

### Synchronizing views

To ensure that position in one view follows another automatically, select it in the “Synchronize with” context submenu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/syncviews1-3.png?width=560&height=470&name=syncviews1-3.png)

Now, if you place disassembly and pseudocode side-by-side, the cursor position will be synchronized automatically when navigating in either window. The matching lines are also helpfully highlighted. Because a single pseudocode line may be represented by several assembly instructions and vice versa, the match is not one-to-one.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/syncviews3-3.png?width=1847&height=537&name=syncviews3-3.png)

Any view which displays information tied to addresses can be synchronized to another. As of IDA 8.3 these include:

  1. Disassembly (IDA View)
  2. Decompilation (Pseudocode)
  3. [Hex View](<https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view/>)



You can even sync more than two views at the same time, although this has to be done in a specific sequence. For example:

  1. Synchronize IDA View-A and Pseudocode-A
  2. Synchronize Hex View with the other pair



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/syncviews2-3.png?width=574&height=228&name=syncviews2-3.png)

### Synchronizing to registers in debugger

During debugging, an additional feature is available: synchronizing a view to a register value. You may have noticed that during debugging the default disassembly view changes name to IDA View-EIP (IDA View-RIP for x64 or IDA View-PC for ARM). This is because cursor follows the current execution address stored in the corresponding processor register.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/syncviews4-3.png?width=674&height=506&name=syncviews4-3.png)

You can also synchronize the default Hex View to a register, or open additional views if you need to follow a specific one. For this, use “Open register window” from the context menu on the register in the registers view.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/syncviews5-3.png?width=334&height=482&name=syncviews5-3.png)  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/syncviews6-3.png?width=625&height=259&name=syncviews6-3.png)

See also:

[Igor’s tip of the week #22: IDA desktop layouts](<https://hex-rays.com/blog/igors-tip-of-the-week-22-ida-desktop-layouts/>)

[Igor’s tip of the week #38: Hex view](<https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view/>)

[Igor’s Tip of the Week #153: Copying pseudocode to disassembly](<https://hex-rays.com/blog/igors-tip-of-the-week-153-copying-pseudocode-to-disassembly/>)

---

## 4. 

**Date:** Posted: Aug 22, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-generating-signatures-for-nim-and-other-non-c-programming-languages](https://hex-rays.com/blog/plugin-focus-generating-signatures-for-nim-and-other-non-c-programming-languages)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: Generating signatures for Nim and other non-C programming languages

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-generating-signatures-for-nim-and-other-non-c-programming-languages>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-generating-signatures-for-nim-and-other-non-c-programming-languages>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-generating-signatures-for-nim-and-other-non-c-programming-languages>)

A 

Alex Petrov ✦ Posted: Aug 22, 2023

![Plugin focus: Generating signatures for Nim and other non-C programming languages](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

**This is a guest entry written by Holger Unterbrink from Cisco Talos. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author.**

Adversaries are increasingly writing malware in programming languages such as Go, Rust, or Nim, likely because these languages present challenges to investigators using reverse engineering tools designed to work best against the C family of languages. 

It’s often difficult for reverse engineers examining non-C languages to differentiate between the malware author’s code and the language’s standard library code. In the vast majority of cases, HexRay’s Interactive Disassembler (IDA) has the out-of-the-box capability to identify library functions or generate custom signatures and solve the issue. 

But for Nim, generating signatures is distinctly more difficult. The techniques described in this blog post focus on how to overcome the challenges associated with generating a signature file for the Nim programming language. However, these techniques can easily be applied to other languages or situations where malware authors use special and uncommon compiler switches to make standard library profiling more challenging for investigators. 

This blog starts from scratch in detailing the issues associated with generating signatures for Nim. If you are already familiar with FLIRT and just interested in the solution, you can skip to the “Our fully automated solution” section. All scripts of the fully automated solution can be found [here](<https://github.com/Cisco-Talos/Nim-IDA-FLIRT-Generator>).

### The problem: Too many code variations

A reverse engineer’s main goal is to understand the logic behind a certain malware sample or, in other words, what the sample is designed to accomplish. IDA makes this task much easier by resolving standard library functions like ‘memcpy’ by using the [Fast Library Identification and Recognition Technology (FLIRT)](<https://hex-rays.com/products/ida/tech/flirt/in_depth/>), which is essentially a binary signature for the bytes of a function. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-1-3.png?width=624&height=73&name=signatures-for-nim-1-3.png)

For common libraries and compilers, IDA comes with FLIRT signatures out of the box. You can see a list of the default FLIRT signatures by opening the ‘File/Load File/FLIRT Signature file…’ menu in IDA. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-2-3.png?width=572&height=352&name=signatures-for-nim-2-3.png)

The problem here is, like with any static signature, that machine code has hundreds of options to do the same thing with different instructions. The way in which machine code is produced is heavily dependent on the compiler and compiler switches. For example, if a user was to compile the OpenSSL library, a certain function in this library can have different machine code bytes depending on which compiler, version of the compiler, version of the library, and compiler flags are used. Due to this complexity, IDA will likely never provide FLIRT signatures for all libraries and all possible combinations. 

The same issue as described above occurs when investigating samples that have statically compiled libraries embedded that were compiled with uncommon compiler switches. An investigator may see thousands of functions that do not resolve automatically in this case, and it is easy to get lost in analyzing standard library functions instead of the code the malware author wrote. 

### The standard solution: How to generate FLIRT signatures

To address this issue, HexRays Fast Library Acquisition for Identification and Recognition (FLAIR) toolkit provides the ability to generate custom FLIRT signatures. A reverse engineer using this toolkit needs to find out which compiler, library version and compiler switches the adversary used. IDA, PE/Packer tools like [DIE](<https://github.com/horsicq/Detect-It-Easy>), or string extraction tools like [Flare-floss](<https://github.com/mandiant/flare-floss>) can help with that. Usually, you want to look for library version strings or functions introduced in certain releases of the library of interest. Alternatively, once you identify the library used, an investigator can compile the library with different switches and/or versions and compare the result with the malware sample by using tools like [Diaphora](<https://github.com/joxeankoret/diaphora>). Once the library version is determined, even if it is a stand-alone library like OpenSSL or SQLite3, or a programming language runtime library (RTL), you can start compiling it with the desired compiler switches and generate FLIRT signatures. 

**Standard steps for generating FLIRT signatures**

Investigators must follow a number of standard steps to generate FLIRT signatures. First, the FLAIR toolkit has to be downloaded from the [Hex-Rays download center](<https://hex-rays.com/download-center/>). This toolkit can be unpacked to a directory of choice. Then, change to the `flair <IDA Version>` directory. In this directory, you can find several helpful text files describing the different tools:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-3-3.png?width=624&height=336&name=signatures-for-nim-3-3.png)

In theory, the process of building a FLIRT signature is straightforward. From the library of interest, for example, the runtime library of the programming language, you need to generate a pattern file `(.pat)` with one of the pattern tools, like `‘pcf.exe’`. There are multiple tools for multiple file formats. You can then use the `‘sigmake.exe’` tool to generate the final signature file from one or more pattern files `(.pat)`. You can concatenate pattern files with the plus sign (+). The compiled signature file needs to be copied to the IDA signature architecture directory e.g. `C:\Program Files\IDA Pro <VERSION>\sig\pc`. The pattern file is generated based on the library’s target architecture. For example, if you are using Windows and built a static library, the pattern file will be in COFF format and the `pcf.exe` (parsecoff) tool from the FLAIR toolkit would be used. A Linux-based example can be found [here](<https://www.boozallen.com/insights/cyber/tech/ida-flirt-signatures-for-linux-binaries.html>). 

The signature file is a binary file that uses the file extension `.sig`, while the pattern file(s) is a text file that uses the extension `.pat` and can be read or edited in your text editor of choice. The format of a pattern file is described in the picture below:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-4-1-3.png?width=1784&height=719&name=signatures-for-nim-4-1-3.png)

The following will provide a theoretical example to demonstrate the steps for generating common FLIRT signatures. **This example is only for demonstration purposes and would not actually work** , as Nim doesn’t support static linking of the RTL. Even if Nim did support this, you would need to use the additional compiler switch ‘-dynlibOverrideAll’ to overwrite certain pragmas, an issue we will revisit later in this post. We are just using the command line below to make sure the RTL will compile and as an example that could be adapted to other languages or libraries.

**Build the static library:**
    
    
      # **nim c  -d:release --opt:size --app:staticlib --out:nimrtl.lib .\nimrtl.nim**
      ….
      62678 lines; 5.626s; 93.871MiB peakmem; proj:  C:\Users\hunte\.choosenim\toolchains\nim-1.6.12\lib\nimrtl.nim; out:  C:\Users\hunte\.choosenim\toolchains\nim-1.6.12\lib\nimrtl.lib [SuccessX]
      

**Generate the pattern (.pat) file:**
    
    
      # C:\tools\IDA\flair82\bin\win\**pcf.exe .\nimrtl.lib**
      C:\Users\hunte\.choosenim\toolchains\nim-1.6.12\lib\nimrtl.lib:  skipped 0, total 32
     

**Build the signature file:**
    
    
      #C:\tools\IDA\flair82\bin\win\**sigmake.exe -n"nim-rtl-1612"  nimrtl.pat nim-rtl-1612.sig**
    

Note: If there are collisions/overlaps in the pattern file(s), for example, two or more functions with the same bytes, ‘sigmake’ will generate an ‘.exc’ file and you will see something like this:
    
    
      _nim-1612.sig:  modules/leaves: 449/503, COLLISIONS: 1_
      _See the documentation to learn how to resolve collisions._
      

The `‘.exc’` file will list the colliding signatures and you can either manually resolve the issue or just delete the four or five comment lines at the beginning of the `‘.exc’` file that start with a semicolon, and run the sigmake command again. The latter automatically resolves the collisions.
    
    
      # **Copy the signature file to IDA signature directory (this needs to be done from an  elevated prompt):**
      **copy  ‘nim-rtl-1612.sig’ ‘C:\Program Files\IDA Pro 8.2\sig\pc’**
    

Now, you can find the Nim signature in the list of available library modules in IDA (File/Load File/FLIRT Signature file…):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-5-3.png?width=624&height=409&name=signatures-for-nim-5-3.png)

In IDA's output window you can see that the signature was loaded and applied:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-6-3.png?width=624&height=133&name=signatures-for-nim-6-3.png)

****

**Why these standard steps do not work with Nim**

This is the point where an investigator would usually be done and all library functions would be resolved in IDA. Below is a quick Nim test application to check if the Nim functions are getting resolved properly:
    
    
    **t1.nim:**
    _var  s1:string = "Hello "_
    _var  s2:string = "World!"_
    _var  s3:string = s1 & s2_
    _echo s3_
    

We are compiling `t1.nim` with the same compiler switches as we did for the Nim runtime library (RTL) above and stripping the symbols from the binary `(--passL:-s)` to make sure IDA doesn’t resolve the function names via symbol information in the PE file. 
    
    
    _nim c  -d:release --opt:size --passL:-s -o:"t1-release-size-strip.exe"  t1.nim_
    

When we load the executable into IDA and apply the Nim signatures, the amount of detected library functions in our test Nim program is much too low:

IDA Memory Graph before applying the signatures (See light blue ‘Library functions’)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-7-3.png?width=624&height=39&name=signatures-for-nim-7-3.png)

IDA Memory Graph after applying the signatures (See light blue ‘Library functions’)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-8-3.png?width=624&height=36&name=signatures-for-nim-8-3.png)

If we search for the echo function in IDAs functions window, there is none.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-9-3.png?width=520&height=192&name=signatures-for-nim-9-3.png)

Let’s try to find the echo function in our sample by first browsing through the Nim initialization functions to get to the function where the code we wrote is executed. The Nim compiler works by translating Nim source code into an intermediate language or JavaScript, though the latter is out of scope for this blog. The intermediate language can be C, C++ or ObjectC. In this example, we demonstrate the default C code generation. Once the compiler has generated the intermediate C source code, it uses the platform’s compiler to build the executable. On Windows, the default compiler is MinGW. You can read more about this on [Nim’s backend integration](<https://nim-lang.org/docs/backends.html>) page and [compiler user guide](<https://nim-lang.org/docs/nimc.html>). Other Nim basics are covered in a tutorial [here](<https://nim-lang.org/docs/tut1.html>). Looking into the intermediate C code shows that there are several initialization routines before the author’s actual code starts at the NimMainModule function. For most executables written in Nim, the functions are in the following order: `Main > NimMain > NimMainInner > NimMainModul`

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-10-1-3.png?width=1851&height=627&name=signatures-for-nim-10-1-3.png)

Unfortunately, in stripped Nim binaries like the one in the example, not all of the initialization routines are automatically resolved by IDA, but at least `NimMain` is usually detected by HexRay’s Lumina. We can search for this in the IDA’s function window and jump to `NimMain`. Alternatively, you can follow the path described in the picture below, starting at `tmainCRTStartup`. There are `‘ _lea rax_`, `_< NimMainInnerAddr>_ ’ ` and `‘ _call rax_ ’` instructions, that are one click from the `NimMainModule` function. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-11-1-3.png?width=2459&height=1209&name=signatures-for-nim-11-1-3.png)

Typically this path is the same for most cases, but it depends on the compiler switches and obfuscation techniques used by the malware author. Another trick to find `NimMainModule` is to check the Xrefs to `‘ _nimRegisterGlobalMarker’_`, as usually the function with most pointers to `_‘nimRegisterGlobalMarker’_`is `‘ _NimMainModule’_`. If you are dealing with real malware, it is also important to keep in mind that there is an intermediate C code. Nothing keeps the malware author away from modifying the initialization routines described above. In other words, execute malicious code before `‘ _NimMainModule’_`.

Unfortunately, once we found `_‘NimMainModule’_`we can see that the echo function from our source code was not resolved by default nor after we applied the Nim signature file from above. The function `‘ _sub_140001CE9_ ’` in the screenshot below, is the echo function from the source code. At compile time it will be translated to the `echoBinSafe` function from the `io.nim` library, so from now we will use this name. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-12-3.png?width=505&height=590&name=signatures-for-nim-12-3.png)

But why is it not resolved? If we look into the pattern file generated above, we can find the `_echoBinSafe_`function, indicating the signature is included in our signature file. The reason the function did not resolve is that, comparing the `echoBinSafe` function from the runtime library file `nimrtl.lib` to our test file’s `t1-release-size-strip.exe`, `echoBinSafe` function reveals they are similar but not byte compatible.

![nimrtl.lib  vs t1-release-size-strip.exe](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-13-1-3.png?width=2363&height=957&name=signatures-for-nim-13-1-3.png) nimrtl.lib vs t1-release-size-strip.exe 

The stack initialization is different — `nimrtl.lib` has additional functions like `_EnterCriticalSection_` and other minor differences. This means that if we have a signature based on the bytes found in the `_echoBinSafe_`function of the `nimrtl.lib`, it will not detect the version the compiler has built for the standalone executable `t1-release-size-strip.exe`. 

The library source code contains pragmas for building it as a dynamic link library (DLL), so we would need to use `‘-dynlibOverrideAll’` to disable the dynlib pragma. Unfortunately, the Nim authors do not support building it as a static library: 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-14-1-3.png?width=1958&height=721&name=signatures-for-nim-14-1-3.png)

**The Python script workaround for generating a Nim executable including all runtime library functions**

As the standard way of generating a Nim signature file doesn’t work, we designed a workaround by writing a Python script that parses all important Nim standard libraries, extracts all library functions and generates valid and compilable function calls from it. We tried a couple of other ways to convince the Nim compiler to generate the same source code, but all failed for different reasons. That was the reason why we decided to generate a large Nim source code file that includes all important Nim functions, compile it, and generate a pattern file by using the [IDB2PAT](<https://github.com/mandiant/flare-ida>) plugin. This plugin can generate pattern files from an IDB or, in other words, from any executable you can load into IDA. The signatures for all symbols IDA can resolve are written into a `.pat file`. From there you can follow the way which we described above to generate a signature file `.sig` by using `sigmake.exe`. 

As mentioned above, we endeavored to write a Python script that parses all the important Nim standard libraries, extracts all library functions and generates valid and compilable function calls from it. Given the complexity of Nim’s syntax, it took more than a week to write a parser that could generate valid and compilable source code from all the Nim libraries we wanted to include. Our work is similar to the [NimP](<https://github.com/zaphodef/NimP>) project, though this project is a bit outdated and we were not able to get it to work with the latest Nim version. To give you an idea of the complexity of the Nim syntax, below is a small subset of functions from the Nim standard libraries, all of which are valid functions:
    
    
    _func  spaces*(n: Natural): string_
    _func  endsWith*(s: string, suffix: char): bool {.inline.}_
    _func  invalidFormatString() {.noinline.}_
    _func  `%`*(formatstr: string, a: openArray[string]): string {.rtl, extern:  "nsuFormatOpenArray".}_
    _func  fromOct*[T: SomeInteger](s: string): T =_
    _func  clamp*[T](val: T, bounds: Slice[T]): T {.since: (1, 5), inline.}_
    _proc  initOptParser*(cmdline = "", shortNoVal: set[char] = {}, longNoVal:  seq[string] = @[]; allowWhitespaceAfterColon = true): OptParser =_
    _proc  next*(p: var OptParser) {.rtl, extern: "npo$1".}_
    _proc  `$`*(t: StringTableRef): string {.rtlFunc, extern: "nstDollar".}_
    _proc  hasKey*(t: StringTableRef, key: string): bool {.rtlFunc, extern:  "nst$1".}_
    _proc  enlarge(t: StringTableRef) =_
    _proc  clear*(s: StringTableRef) {.since: (1, 1).}_
    _proc  ` <%`*(a, b: Rune): bool =_
    

Though this complexity challenged our process, we created a working script that may be rewritten or cleaned up in due course. The script does not cover a few special cases, but it includes the majority of Nim standard library functions we were interested in. If a certain function is missing, it can be added to the source code the script is producing and compiled manually. 

We used most libraries included in the Nim runtime and some others often used in Nim malware:   
_`parseutils.nim`, `strutils.nim`, `parseopt.nim`, `parsecfg.nim`, `strtabs.nim`, `unicode.nim`, `ropes.nim`, `os.nim`, `osproc.nim`, `cstrutils.nim`, `math.nim`, `browsers.nim`, `io.nim`_. 

The first version of our script generated one executable file per library file. Due to overlapping code and other dependencies, it would be difficult to generate one executable that includes all functions of all libraries. This means the script parses 12 standard library files, resulting in 12 executables that must be manually loaded into IDA and for every single one, the IDB2PAT plugin has to be executed. 

In the second version of the script, we tried to automate this process by using IDA’s scripting functionality. Additional information on this and the issues we were facing with this approach can be found in Appendix B. 

### Our fully automated solution

To fully automate the signature-generating process and for educational purposes, we finally wrote three scripts: one generating the ‘fake’ source code file `_nim_rtl_builder.py_`, one COFF parser `_coffparser.py_`, and one using the COFF parser to generate signatures from all object files in the Nim cache directory `_obj2patfile.py_`. These object files are generated by the Nim compiler automatically at compile time. If you do not need signatures for special versions of the Nim runtime library, you can also download some signature files for Nim 1.6.12 [here ](<https://github.com/Cisco-Talos/Nim-IDA-FLIRT-Generator/signatures>)and skip the rest of the blog post. These signature files likely work for some other Nim versions too. 

After execution, each script will give you instructions on what to do next. Start with the `_nim_rtl_builder.py script_`. It needs to be executed in the `‘<NIM_INSTALL_DIR>\lib\pure’` directory. `_Nim_rtl_builder.py_` will parse all functions from the library files mentioned above, then build a Nim source code file per library with function calls to these functions. For example, there is the following function in the library:
    
    
    proc readAll*(file: File): string {.tags:  [ReadIOEffect], benign.}
    

The parser script will analyze the parameters of the function, which in this case, is just the ‘file’ parameter of type ‘File’. The script then generates variables for the function parameters and as well a call to the function using these parameter variables. This source code would likely not execute or even do anything useful, but that is not our goal. Rather, we want to produce compilable source code to get a binary file that includes the bytes of the functions in the same way a normal Nim executable would have, so we can use it to generate our signatures. Below is an example of what a function in the generated source code file looks like. The first line is a comment describing the original function in the parsed standard library, followed by the generated variables and the function call. If a function returns a value in Nim, you have to use this value or otherwise discard it. This is what the discard instruction below is doing. 
    
    
    #proc readAll*(file: File): string {.tags:  [ReadIOEffect], benign.}
    var fil682: File = open("test.txt",  fmReadWrite)
    discard readAll(fil682)
    

You probably now have an idea of how complex it is to parse Nim functions. You need to understand the parameter types and the return value, because some standard library functions have return values, and some don’t. To make it even more complex, Nim comes with Metaprogramming features, like [generics](<https://nim-lang.org/docs/tut2.html#generics>) and [others](<https://nim-lang.org/docs/tut3.html#introduction>). 

Before executing, you have to edit the` _nim_rtl_builder.py_` script and change some variables, of which only the first one is mandatory. You then must change to the `\lib\pure directory of your Nim installation`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-15-3.png?width=624&height=89&name=signatures-for-nim-15-3.png)
    
    
    #cd <NIM_INSTALL_DIR>\lib\pure
    

and execute 
    
    
    # ‘python  <path_to_script>\nim_rtl_builder.py’
    

These steps build the executables from our ‘fake’ source code and with that, also the object files in the Nim cache directory, which we need for the next stage. A successful run would look like this:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-16-3.png?width=482&height=330&name=signatures-for-nim-16-3.png)

Now you can parse all generated COFF object files `.o` for function symbols and bytes in the Nim compiler cache directory by executing the `obj2patfile.py` script. Again, you need to set the right paths in the header of the `obj2patfile.py` script.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-17-3.png?width=624&height=33&name=signatures-for-nim-17-3.png)

**Make sure you changed the working directory to the Nim cache directory before running the` obj2patfile.py script`.** As a reminder, the cache directory name was set in the `_nim_rtl_builder.py_` script above.
    
    
    # cd <NIM_CACHE_DIR> (e.g.  \nim-1.6.12\lib\pure\HU_nim_cache)
    # python <path_to_script>\obj2patfile.py
    

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-18-3.png?width=624&height=353&name=signatures-for-nim-18-3.png)

Now run the `_sigmake.exe_` tool from the FLAIR toolkit to generate the final signature file — just copy and paste it from the output of the `_obj2patfile.py_` script.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-19-3.png?width=624&height=80&name=signatures-for-nim-19-3.png)

There will be some collisions, which is expected. You need to delete the comments in the _nim-1612.exc_ file that was generated by `_sigmake.exe_`. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-20-3.png?width=624&height=87&name=signatures-for-nim-20-3.png)

Once the comments are deleted, run the `sigmake.exe` command again in the same way you did before. This will automatically resolve the collisions. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-21-3.png?width=624&height=61&name=signatures-for-nim-21-3.png)

Now that you have a valid IDA signature file `nim-1612.sig`, the last step is to copy this over to the IDA signature directory (e.g. C:\Program Files\IDA Pro 8.2\sig\pc). You can load these signatures when you are analyzing a Nim executable in IDA via the `_File/Load File/FLIRT Signature file…_` menu. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/signatures-for-nim-22-1-3.png?width=3076&height=1730&name=signatures-for-nim-22-1-3.png)

We hope you enjoyed our journey of generating Nim signatures. Happy reversing!

### Appendix A

Offensive Nim resources:

<https://github.com/byt3bl33d3r/OffensiveNim/>  
<https://s3cur3th1ssh1t.github.io/Playing-with-OffensiveNim/>  
<https://ajpc500.github.io/nim/Shellcode-Injection-using-Nim-and-Syscalls/>  
<https://github.com/ajpc500/NimlineWhispers>  
<https://github.com/chvancooten/NimPlant>  
<https://github.com/adamsvoboda/nim-loader>  
<https://twitter.com/ShitSecure/status/1482428360500383755>  
<https://github.com/icyguider/Nimcrypt2>  
<https://github.com/aeverj/NimShellCodeLoader>  
<https://ppn.snovvcrash.rocks/red-team/maldev/nim>  
<https://assume-breach.medium.com/home-grown-red-team-bypassing-windows-11-defenses-with-covenant-c2-and-nimcrypt2-2557a0e3dfff>  
<https://www.securityartwork.es/2022/01/12/bypassing-av-edr-with-nim/>  
<https://sec-consult.com/blog/detail/nimpostor-bringing-the-nim-language-to-mythic-c2/>  
<https://thehackernews.com/2021/03/researchers-spotted-malware-written-in.html>  
<https://www.bleepingcomputer.com/news/security/trickbots-bazarbackdoor-malware-is-now-coded-in-nim-to-evade-antivirus/>  
<https://research.checkpoint.com/2022/chinese-actor-takes-aim-armed-with-nim-language-and-bizarro-aes/>

### Appendix B

**Why does automatically starting the plugin not work in IDA scripting mode?**

The IDB2PAT plugin is missing some initialization methods and acts more like a standalone IDA Python script than a real plugin. This and some other IDA internals lead to the issue that we were only able to semi-automate the workflow. In this very special case, we did not find a way to fully automate the signature generation process by using IDA’s onboard tools, but we think it is worth including it here as this works for most other cases and it hopefully provides a good starting point for your own IDA automation tasks.

See below for our Python/IDC Script to automatically load the generated library into IDA and run the plugin without user interaction:

**Python control script:**
    
    
    _import os_
    _import sys_
    _import subprocess_
    
    _if len(sys.argv) != 2:_
               _print("[ERROR] You have to give  me a filename to load into IDA")_
      _exit(1)_
      
    _filename  = sys.argv[1]_
    
    _def  built_pat(filename):_
    
    _ida_app = 'ida64'_
    **# ‘-A’  autonomous  mode. IDA will not display dialog boxes.**
    **# Default answer is always chosen**
      _ida_opt1 = '-A'_
    **# ‘-S’ Execute a script file (RunIDB2PAT.idc) when the database is opened**
      _ida_opt2 = '-SRunIDB2PAT.idc'_
      _ida_dir = 'C:\\Program Files\\IDA  Pro 8.2\\'_
      _exe_file_path = os.getcwd() + '\\' +  filename_
      _ida_filepath  = ida_dir + ida_app_
      
      _# This would be the full command to start and run the plugin automatically, but_
      _# due to the described issue we are disabling it and just loading the file into IDA_
      _# built_cmd  = [ida_filepath, ida_opt1, ida_opt2, exe_file_path]_
      
      _# Workaround: Removed ida_opt1 and ida_opt2 to be able to manually_
      _# execute the plugin._
      _built_cmd   = [ida_filepath, exe_file_path]_
      _built_cmd_str = "  ".join(built_cmd)_
      _input(f"Should we execute: {built_cmd_str}  [Hit Enter to continue]?")_
      _return_code =  subprocess.call(built_cmd)_
      
      _if return_code == 0:_
      _print("Command executed successfully.")_
      _else:_
      _print(f"Command failed with return code {return_code}")_
      _exit(1)_
    
    _print(f"Loading  File: {filename}")_
    
    _built_pat(filename)_
    
    _print("Done.")_
    

**IDC script ‘ _RunIDB2PAT.idc’:_**
    
    
    #include <idc.idc>
    
    static main()
    {
      auto  ret;
      
      msg("Waiting for the end of the auto analysis...\n");
      auto_wait();
      msg("Auto analysis is finished, starting plugin ...\n");
      
    **// 1st try: Load and run plugin:**
    **// This would work for standard plugins, but unfortunately, the idb2pat.py plugin**
    **// is more acting like a standalone Python script and missing some plugin init routines**
    // ret  = load_and_run_plugin("C:\\Program Files\\IDA Pro 8.2\\python\\flare\\idb2pat.py",0);
    
    **// 2nd try: Another way of executing the Python code from the script via exec_python**
    **// IDC function, unfortunately this doesn’t work either, some functions seem to get**
    **// resolved after auto_wait(). This means the automation works and a .pat file is**
    **// generated, but the signatures in the .pat file are not complete.**
    ret =  exec_python("idaapi.IDAPython_ExecScript(r'C:\\Program Files\\IDA Pro  8.2\\python\\flare\\idb2pat.py', globals())");
    
    if  (ret == 0) {
      		 msg("[Success]  Return value: %d\n", ret);
      }
      else {
      	    msg("[Failed]  Error: %s\n", ret);
      }
    
    // The following line instructs IDA to quit without saving the database
     process_config_directive("ABANDON_DATABASE=YES");
     qexit(0);
    }

---

## 5. 

**Date:** Posted: Aug 18, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-153-copying-pseudocode-to-disassembly](https://hex-rays.com/blog/igors-tip-of-the-week-153-copying-pseudocode-to-disassembly)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #153: Copying pseudocode to disassembly

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-153-copying-pseudocode-to-disassembly>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-153-copying-pseudocode-to-disassembly>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-153-copying-pseudocode-to-disassembly>)

I 

Igor Skochinsky ✦ Posted: Aug 18, 2023

![Igor’s Tip of the Week #153: Copying pseudocode to disassembly](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When using the decompiler, you probably spend most of the time in the [Pseudocode view](<https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics/>). In case you need to consult the corresponding disassembly, it’s a quick `Tab` away. However, if you actually prefer the disassembly, there is another option you can try.

### Copy to assembly

This action is available in the pseudocode view’s context menu when right-clicking outside of the decompiled code:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/copyasm1-3.png?width=347&height=203&name=copyasm1-3.png)

Because the decompiler uses disassembly [comments](<https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida/>) for this feature, it warns you that the action will destroy any existing ones:

![Copying pseudocode to disassembly listing will destroy
existing anterior and posterior comments.
Do you want to continue?](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/copyasm2-3.png?width=367&height=177&name=copyasm2-3.png)

After confirmation, comments with pseudocode lines are added to the disassembly:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/copyasm3-3.png?width=716&height=433&name=copyasm3-3.png)

You can see these comments even in the [graph view](<https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view/>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/copyasm4-3.png?width=871&height=510&name=copyasm4-3.png)

In fact, you can make use of this feature even without switching to pseudocode. While in disassembly, use Edit > Comments > Copy pseudocode to disassembly, or the shortcut `/`

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/copyasm5-3.png?width=578&height=533&name=copyasm5-3.png)

Note that unlike pseudocode itself, these comments are static and do not change when you make changes in the pseudocode (e.g. rename variables). To update the comments, you need to trigger the action again.

In case you changed your mind and want to clean up the function, use “Delete pseudocode comments” from the same menu.

See also:

[Hex-Rays interactive operation: Copy to assembly](<https://www.hex-rays.com/products/decompiler/manual/cmd_copy.shtml>)

[Igor’s tip of the week #14: Comments in IDA](<https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida/>)

---

## 6. 

**Date:** Posted: Aug 11, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-152-force-creating-functions](https://hex-rays.com/blog/igors-tip-of-the-week-152-force-creating-functions)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #152: Force-creating functions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-152-force-creating-functions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-152-force-creating-functions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-152-force-creating-functions>)

I 

Igor Skochinsky ✦ Posted: Aug 11, 2023

![Igor’s Tip of the Week #152: Force-creating functions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Occasionally, especially when working with embedded firmware or obfuscated code, you may see an error message when trying to create a function (from context menu or using `P` hotkey):

![\[Output\]
ROM:C998: The function has undefined instruction/data at the specified address.
Your request has been put in the autoanalysis queue.](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/forcefunc1-3.png?width=901&height=80&name=forcefunc1-3.png)

There can be multiple reasons for it, for example:

  1. some code has been incorrectly converted to data and the execution flows into it;
  2. the function calls a [non-returning function](<https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions/>) which hasn’t been marked as such, so IDA thinks that the execution flows into the following data or undefined bytes;
  3. the function uses an unrecognized [switch pattern](<https://hex-rays.com/blog/igors-tip-of-the-week-53-manual-switch-idioms/>);
  4. the function calls some function which uses embedded data after the call, but IDA tries to decode it as instructions;
  5. code has been obfuscated and IDA’s autoanalysis went down a wrong path.



You can double-click the address indicated to jump there and to see if you can identify the issue and try to fix it, but it can take a long time to figure out.

Functions are required to use some of IDA’s basic functionality such as [graph view](<https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view/>) or the [decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics/>).

### Forcing IDA to create a function

Whatever the reason of the error, you can still create a function manually if you can determine its bounds using your best judgement. For this, the [anchor selection](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>) is the most simple and convenient way:

  1. while staying on the first instruction of the function, use Edit > Begin selection, or press `Alt`–`L`;
  2. navigate down to the function’s end (e.g. look for a return instruction or start of the next function);
  3. press `P` (Create function)



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/forcefunc2-3.png?width=2804&height=859&name=forcefunc2-3.png)

Note that the function created this way may have all kinds of issues, e.g. disconnected blocks in the graph view, `JUMPOUT` statements in pseudocode or wrong decompilation, but at least it should allow you to advance in your analysis.

---

## 7. 

**Date:** Posted: Aug 10, 2023  
**Link:** [https://hex-rays.com/blog/building-ida-python-on-windows](https://hex-rays.com/blog/building-ida-python-on-windows)

[Back](<https://hex-rays.com/blog>)

[IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Building IDA Python on Windows

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/building-ida-python-on-windows>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/building-ida-python-on-windows>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/building-ida-python-on-windows>)

A 

Alex Petrov ✦ Posted: Aug 10, 2023

![Building IDA Python on Windows](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

**This is a guest entry written by Elias Bachaalany. His views and opinions are his own and not those of Hex-Rays. Any questions with regards to the content of this blog post should be directed to the author.**

## Introduction

During the [IDA Advanced training](<https://hex-rays.com/training/#advanced>), I get asked a lot about how to set up the IDA SDK and how to buid IDAPython and extend it. In this article, we will show you how to build IDAPython on Windows.

> On Linux/macOS, the process is straightforward as per the [BUILDING.txt](<https://github.com/idapython/src/blob/master/BUILDING.txt>).

In this article:

  * Setting up VS 2019
  * Setting up Cygwin
  * Setting up Python 3.x
  * Setting up SWIG
  * Setting up IDA SDK
  * Building IDAPython



Make sure you already installed the latest IDA for Windows and grabbed yourself a copy of the [SDK](<https://hex-rays.com/download-center>).

Let's get started.

## Setting up VS 2019 Community Edition

IDA SDK's default configuration is targeted towards VS 2019. Just to keep this article simple, let's stick with that.

Grab VS2019 from here <https://visualstudio.microsoft.com/vs/older-downloads/>

When you install it, make sure you leave all the default installation location (in the `C:\Program Files (x86)`, etc.)

## Setting up Cygwin 64

Download [Cygwin 64](<https://www.cygwin.com/setup-x86_64.exe>) and install the following components:

  * `make` from the `Devel` category.
  * `unzip` from the `Archive` category.



## Setting up Python 3.x

Download Python 3.x _AMD64_ (say 3.4 to 3.11) and select custom install:

  * Install to C:\PythonXY (X = major, Y = minor)
  * Make sure you select the following options: 
    * for all users
    * pip
    * Precompile standard library
    * Optional: "Download debugging symbols"



After Python is installed, install the `six` module:
    
    
    C:\PythonXY\Scripts\pip install six
    

## Setting up SWIG

In this section, we will build [SWIG](<https://www.swig.org>) for Windows from scratch. We need a special patched version of SWIG (version 4.0.1, with support for `-py3-limited-api`) and we cannot use the pre-built binaries.

Whether you are using a Linux distro (for example Ubuntu 20) or WSL on Windows 10/11, all the steps below still apply.

Depending on your Linux setup, you may have already installed the needed packages. Just to be on the safe side, you need the following:
    
    
    sudo apt update
    sudo apt install wget build-essential mingw-w64 byacc bison automake autotools-dev patchelf -y
    

> If mingw-64 failed to install:  
>  `sudo apt-add-repository ppa:mingw-w64/ppa && sudo apt-get update && sudo apt-get install mingw-w64`

### Clone IDAPython's patched SWIG
    
    
    git clone --branch py3-stable-abi https://github.com/idapython/swig.git swig-py3-stable-abi
    

### Download PCRE library and build it

Download PCRE directly into SWIG's source directory:

`wget https://master.dl.sourceforge.net/project/pcre/pcre/8.10/pcre-8.10.tar.bz2`

Then edit the PCRE build script in SWIG to specify the host compiler:

Open `./Tools/pcre-build.sh` and change this line:

`cd pcre && ./configure --prefix=$pcre_install_dir --disable-shared $* || bail "PCRE configure failed"`

To:

`cd pcre && ./configure --host=x86_64-w64-mingw32 --prefix=$pcre_install_dir --disable-shared $* || bail "PCRE configure failed"`

(we only added the `--host` switch)

Now just run the PCRE build script again:

`./Tools/pcre-build.sh`

You are now ready to build SWIG.

### Building SWIG

First run `./autogen.sh`, then run the configure script with the `--host` switch, while also specifying static linking:
    
    
    LDFLAGS="-static -static-libgcc -static-libstdc++" \
    ./configure --host=x86_64-w64-mingw32 --prefix=/tmp/swig4-win
    

Now you can run `make` and `make install` as usual.

Alternatively, you can download the pre-build version from [here](<https://hex-rays.com/wp-content/uploads/2023/08/swig4-win.zip>).

Copy the SWIG Windows binaries from `/tmp/swig4-win` to your Windows machine. Let's put them in `C:\idasdk\swig4`.

## Setting up the IDA SDK

  * Download the IDA SDK and unzip to a folder, for example `c:\idasdk`.

  * If you have the Decompiler installed, then copy the Decompiler headers from `<ida_install>/plugins/hexrays_sdk/include/hexrays.hpp` to `c:\idasdk\include`.




### Initialize the SDK's config files

We need to configure various configurations. Open the "Developer Command Prompt for VS 2019".

If Cygwin was not in the path, then typing 'make' will cause an error. If that's the case, just add it to the PATH:
    
    
    set PATH=c:\cygwin64\bin;%PATH%
    

(Keep that command prompt open for the remainder of this article.)

From `c:\idasdk`, type the following commands:
    
    
    cd c:\idasdk
    set __NT__=1
    set __X64__=1
    

Now let's generate the various config files:

  * ida64 debug build: `cmd /c "set __EA64__=1 && make env"`
  * ida64 optimized build: `cmd /c "set __EA64__=1 && set NDEBUG=1 && make env"`
  * ida debug build: `make env`
  * ida optimized build: `cmd /c "set NDEBUG=1 && make env`



If you have done everything right, you should have these files in `c:\idasdk`:

  * vs19paths.cfg
  * x64_win_vc_32.cfg
  * x64_win_vc_32_opt.cfg
  * x64_win_vc_64.cfg
  * x64_win_vc_64_opt.cfg



To test if everything is okay, let's try building the `hello` plugin:
    
    
    cd c:\idasdk\plugins\hello
    make
    

Last but not least, let's put the IDA binaries in the correct place in accordance with the SDK make system. Assuming that IDA was installed in `C:\Tools\IDA82`:
    
    
    xcopy /s c:\Tools\IDA82 c:\idasdk\bin
    

Now, anything we build will go to `c:\idasdk\bin\[plugins|loaders|procs]`:

  * `plugins`: for plugin binaries
  * `loaders`: for loader modules
  * `procs`: for processor modules



## Building IDAPython

Okay, now we are ready to build IDAPython!

### Clone IDAPython

Navigate to `c:\idasdk\plugins` and clone IDAPython there:
    
    
    git clone https://github.com/idapython/src.git idapython
    

### Building IDAPython

Now we have all the prerequisit steps completed, let's build IDAPython:
    
    
    set PYTHON_VERSION_MAJOR=X
    set PYTHON_VERSION_MINOR=Y
    
    C:\PythonXY\python.exe build.py --with-hexrays --swig-home c:\ida\swig4 --ida-install c:\idasdk\bin --debug
    

  * Replace `X` and `Y` with the proper values
  * If you don't have the Decompiler then omit the `--with-hexrays` switch
  * Similarily, drop the `--debug` for optimized builds.
  * Run `build.py --help` for more information



Be patient, this can take between 5 and 30 minutes to complete the first time.

## That's it!

I know, this has been tedious but I hope it is helpful.

Contributions and suggestions to [IDAPython](<https://github.com/idapython/src>). We are happy to discuss and merge your changes as applicable.

---

## 8. 

**Date:** Posted: Aug 7, 2023  
**Link:** [https://hex-rays.com/blog/the-plugin-submission-initiative](https://hex-rays.com/blog/the-plugin-submission-initiative)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [plugin repository](<https://hex-rays.com/blog/tag/plugin-repository>)

# The Plugin Submission Initiative

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/the-plugin-submission-initiative>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/the-plugin-submission-initiative>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/the-plugin-submission-initiative>)

A 

Alex Petrov ✦ Posted: Aug 7, 2023

![The Plugin Submission Initiative](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/plugin-initiative-blog-3.png?width=1200&height=675&name=plugin-initiative-blog-3.png)

We are thrilled to kick off an exciting new campaign – **The Plugin Submission Initiative**. Our Plugin Repository was developed not too long ago and has already reached a milestone with 122 plugins currently available for our users! It is a great start, but we believe there’s room for growth and improvement.

The success of this reasonably new platform relies entirely on the contribution of talented developers like you. That’s why **we invite you to join the Plugin Submission Initiative** and participate in our efforts to expand and enhance the plugin experience for all users.

### Have you already developed a plugin that isn’t published yet in our Repository?

Are you a plugin author with a plugin that deserves a place in the spotlight? We want to hear from you. Throughout the campaign, **we encourage all authors to submit their IDA extensions to our repository**. Whether it is a small script that streamlines the workflow, or an ingenious tool, we welcome a broad range of submissions.

### We have already published your plugin, but you haven’t requested its ownership from us?

We populated our repository with publicly available plugins in the initial development phase. If you find your plugin in our repository, **you can claim its ownership** and make sure it is up to date. This way, you help us improve the Plugin Repository environment, and your colleagues can benefit from the newest features in your tool.

### Earn an Exclusive Cap as a Token of Gratitude!

As a gesture of appreciation for your valuable contributions, **we are awarding caps to authors** who successfully submit and have their plugins approved by the admins and those of you who successfully take over the ownership of already published plugins*. **Bear in mind that the offer is valid while stock lasts but no later than December 29th, 2023.**

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cap-blog-3.png?width=450&height=450&name=cap-blog-3.png)

### How to participate…

Participating in the Plugin Submission Initiative is simple:

  1. Check whether your plugin is already published: <https://plugins.hex-rays.com/>
  2. If not, click the “Submit a plugin” link and follow the steps.
  3. If your plugin is already published and you haven’t taken over the ownership, then contact [plugins@hex-rays.com](<mailto:plugins@hex-rays.com>), and you’ll be given instructions.
  4. When submission/ownership takeover is successful, you will be contacted to provide us with your delivery information.



If you have any questions regarding the initiative, please let us know at [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)

**Joining us in this campaign will help us make the Repository more valuable to all IDA users. It will get us closer to the goal of putting all IDA Plugins in a friendlier and safer environment. Send us your plugin today!**

_

*Please bear in mind that we reserve the right to not send the awards to plugin writers from certain countries. Regardless of the number of successful submissions an author makes, each eligible author will be entitled to receive only one gift.

_

---

## 9. 

**Date:** Posted: Aug 4, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-151-fixing-function-frame-is-wrong](https://hex-rays.com/blog/igors-tip-of-the-week-151-fixing-function-frame-is-wrong)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #151: Fixing “function frame is wrong”

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-151-fixing-function-frame-is-wrong>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-151-fixing-function-frame-is-wrong>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-151-fixing-function-frame-is-wrong>)

I 

Igor Skochinsky ✦ Posted: Aug 4, 2023

![Igor’s Tip of the Week #151: Fixing “function frame is wrong”](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

[Previously](<https://hex-rays.com/blog/igors-tip-of-the-week-148-fixing-call-analysis-failed/>), we’ve run into a function which produces a cryptic error if you try to decompile it:

![Warning
8058FF0: function frame is wrong](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/badframe1-3.png?width=530&height=195&name=badframe1-3.png)

In such situations, you need to go back to disassembly to see what could be wrong. More specifically, check the [stack frame layout](<https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view/>) by double-clicking a stack variable or pressing `Ctrl`–`K`.

On the first glance it looks normal:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/badframe2-3.png?width=767&height=495&name=badframe2-3.png)

However, if you compare with another function which decompiles fine, you may notice some notable differences:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/badframe3-3.png?width=767&height=497&name=badframe3-3.png)

This frame has two members which are mentioned in the top comment:

`Two special fields " r" and " s" represent return address and saved registers.`

They’re absent in the “bad” function, so the whole layout is probably wrong and the function can’t be decompiled reliably. On closer inspection, we can discover that the structure `sigset_t` (type of the variable `oset` in `sub_8058FF0`) is 0x80 bytes, so applying it to the frame overwrote the special members. You can also see that the variable crossed over from the local variable area (negative offsets) to the argument area (positive offsets), which normally should not happen.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/badframe4-3.png?width=1294&height=222&name=badframe4-3.png)

### Fixing a bad stack frame

Although you can try to fix the frame layout by rearranging or editing the local variables, this won’t bring back the special variables, so usually the best solution is to recreate the function (and thus its stack frame). This can be done by undefining (`U`) the first instruction, then creating the function (`P`). A quicker and less destructive way is to delete just the function (`Ctrl`–`P`, `Del`), then recreate it (`P`). Normally this should recreate the default frame then add local variables and stack arguments based on the instructions accessing the stack:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/badframe5-3.png?width=608&height=475&name=badframe5-3.png)

And now the function decompiles fine:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/badframe6-3.png?width=809&height=536&name=badframe6-3.png)

Some code is wrong because the function prototype still uses wrongly detected `sigset_t` argument. This is easy to fix – just delete the prototype (Y, Del) to let the decompiler guess the arguments:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/badframe7-3.png?width=906&height=536&name=badframe7-3.png)

See also:

[Igor’s Tip of the Week #148: Fixing “call analysis failed”](<https://hex-rays.com/blog/igors-tip-of-the-week-148-fixing-call-analysis-failed/>)

[Igor’s tip of the week #65: stack frame view](<https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view/>)

[Decompiler Manual: Failures and troubleshooting](<https://www.hex-rays.com/products/decompiler/manual/failures.shtml>)

---

