# Hex-Rays Blog – Page 27

**Archived:** 2026-02-20 12:18
**URL:** https://hex-rays.com/blog/page/27

---

## 1. 

**Date:** Posted: Nov 19, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view](https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #65: stack frame view

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view>)

I 

Igor Skochinsky ✦ Posted: Nov 19, 2021

![Igor’s tip of the week #65: stack frame view](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

The s _tack frame_ is part of the stack which is managed by the current function and contains the data used by it.

### Background

The stack frame usually contains data such as:

  * local and temporary variables;
  * incoming arguments (for calling conventions which use stack for passing arguments);
  * saved volatile registers;
  * other bookkeeping information (e.g. the return address on x86).



Because the stack may change unpredictably during execution, the stack frame and its parts do not have a fixed address. Thus, IDA uses a pseudo _structure_ to represent its layout. This structure is very similar to other structures in the Structures view, with a few differences:

  1. The frame structure has no name and is not included in the global Structures list; it can only be reached from the corresponding function
  2. Instead of offsets from the structure start, offsets from the _frame pointer_ are shown (both positive and negative);
  3. It may contain special members to represent the saved return address and/or saved register area.



### Stack frame view

To open the stack frame view:

  * Edit > Functions > Stack variables… or press `Ctrl`–`K` while positioned in a function in disassembly (IDA View);
  * Double-click or press `Enter` on a stack variable in the disassembly or pseudocode.



In this view, you can perform most of the same operations as in the Structures view: 

  1. Define new or change existing stack variables (`D`);
  2. Rename variables (`N`)
  3. Create [arrays](<https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays/>) (`*`) or structure instances (`Alt`–`Q`)



### Example

Consider this vulnerable program:
    
    
    #include <stdio.h>
    int main () {
        char username[8];
        int allow = 0;
        printf external link("Enter your username, please: ");
        gets(username); // user inputs "malicious"
        if (grantAccess(username)) {
            allow = 1;
        }
        if (allow != 0) { // has been overwritten by the overflow of the username.
            privilegedAction();
        }
        return 0;
    }

Source: [CERN Computer Security](<https://security.web.cern.ch/recommendations/en/codetools/c.shtml>)

When compiled by an old GCC version, it might produce the following assembly:
    
    
    .text:0000000000400580 main proc near                          ; DATA XREF: _start+1D↑o
    .text:0000000000400580
    .text:0000000000400580 var_10= byte ptr -10h
    .text:0000000000400580 var_4= dword ptr -4
    .text:0000000000400580
    .text:0000000000400580 ; __unwind {
    .text:0000000000400580     push    rbp
    .text:0000000000400581     mov     rbp, rsp
    .text:0000000000400584     sub     rsp, 10h
    .text:0000000000400588     mov     [rbp+var_4], 0
    .text:000000000040058F     mov     edi, offset format          ; "Enter your username, please: "
    .text:0000000000400594     mov     eax, 0
    .text:0000000000400599     call    _printf
    .text:000000000040059E     lea     rax, [rbp+var_10]
    .text:00000000004005A2     mov     rdi, rax
    .text:00000000004005A5     call    _gets
    .text:00000000004005AA     lea     rax, [rbp+var_10]
    .text:00000000004005AE     mov     rdi, rax
    .text:00000000004005B1     call    grantAccess
    .text:00000000004005B6     test    eax, eax
    .text:00000000004005B8     jz      short loc_4005C1
    .text:00000000004005BA     mov     [rbp+var_4], 1
    .text:00000000004005C1
    .text:00000000004005C1 loc_4005C1:                             ; CODE XREF: main+38↑j
    .text:00000000004005C1     cmp     [rbp+var_4], 0
    .text:00000000004005C5     jz      short loc_4005D1
    .text:00000000004005C7     mov     eax, 0
    .text:00000000004005CC     call    privilegedAction
    .text:00000000004005D1
    .text:00000000004005D1 loc_4005D1:                             ; CODE XREF: main+45↑j
    .text:00000000004005D1     mov     eax, 0
    .text:00000000004005D6     leave
    .text:00000000004005D7     retn
    .text:00000000004005D7 ; } // starts at 400580
    .text:00000000004005D7 main endp
    

On opening the stack frame we can see the following picture:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stackframe1-3.png?width=555&height=556&name=stackframe1-3.png)

By comparing the source code and disassembly, we can infer that `var_10` is `username` and `var_4` is `allow`. Because the code only takes the address of start of the buffer, IDA could not detect its full size and created a single byte variable. To improve it, press `*` on `var_10` and convert it into an array of 8 bytes. We can also rename the variables to their proper names.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stackframe2-3.png?width=555&height=556&name=stackframe2-3.png)

Because IDA shows the stack frame layout in the natural memory order (addresses increase towards the bottom), we can immediately see the problem demonstrated by the vulnerable code: the `gets` function has no bounds checking, so entering a long string can overflow the `username` buffer and overwrite the `allow` variable. Since the code is only checking for a non-zero value, this will bypass the check and result in the execution of the `privilegedAction` function. 

### Frame offsets and stack variables

As mentioned above, in the stack frame view structure offsets are shown relative to the _frame pointer_. In some cases, like in the example above, it is an actual processor register (`RBP`). For example, the variable `allow` is placed at offset `-4` from the frame pointer and this value is used by IDA in the disassembly listing for the symbolic name instead of raw numerical offset:
    
    
    .text:0000000000400580 allow= dword ptr -4
    [...]
    .text:0000000000400588 mov [rbp+allow], 0
    [...]

By pressing `#` or `K` on the instruction, you can ask IDA to show you the instruction’s original form:
    
    
    .text:0000000000400588 mov dword ptr [rbp-4], 0

Press `K` again to get back to the stack variable representation.

In other situations the frame pointer can be just an arbitrary location used for convenience (usually a fixed offset from the stack pointer value at function entry). This is common in binaries compiled with frame pointer omission, a common optimization technique. In such situation, IDA may use an extra delta to compensate for the stack pointer changes in different parts of function. For example, consider this function:
    
    
    .text:10001030 sub_10001030 proc near                  ; DATA XREF: sub_100010B0:loc_100010E7↓o
    .text:10001030
    .text:10001030 LCData= byte ptr -0Ch
    .text:10001030 var_4= dword ptr -4
    .text:10001030
    .text:10001030     sub     esp, 0Ch
    .text:10001033     mov     eax, dword_100B2960
    .text:10001038     push    esi
    .text:10001039     mov     [esp+10h+var_4], eax
    .text:1000103D     xor     esi, esi
    .text:1000103F     call    ds:GetThreadLocale
    .text:10001045     push    7                           ; cchData
    .text:10001047     lea     ecx, [esp+14h+LCData]
    .text:1000104B     push    ecx                         ; lpLCData
    .text:1000104C     push    1004h                       ; LCType
    .text:10001051     push    eax                         ; Locale
    .text:10001052     call    ds:GetLocaleInfoA
    .text:10001058     test    eax, eax
    .text:1000105A     jz      short loc_1000107D
    .text:1000105C     mov     al, [esp+10h+LCData]
    .text:10001060     test    al, al
    .text:10001062     lea     ecx, [esp+10h+LCData]
    .text:10001066     jz      short loc_1000107D
    

Here, the explicit frame pointer (ebp) is not used, and IDA arranges the stack frame so that the return address is placed offset 0:
    
    
    -00000010 ; Frame size: 10; Saved regs: 0; Purge: 0
    -00000010 ;
    -00000010
    -00000010     db ? ; undefined
    -0000000F     db ? ; undefined
    -0000000E     db ? ; undefined
    -0000000D     db ? ; undefined
    -0000000C LCData db ?
    -0000000B     db ? ; undefined
    -0000000A     db ? ; undefined
    -00000009     db ? ; undefined
    -00000008     db ? ; undefined
    -00000007     db ? ; undefined
    -00000006     db ? ; undefined
    -00000005     db ? ; undefined
    -00000004 var_4 dd ?
    +00000000  r  db 4 dup(?)
    +00000004
    +00000004 ; end of stack variables

To compensate for the changes of the stack pointer (`sub esp, 0Ch` and the `push` instructions), values `10h` or `14h` have to be added in the stack variable operands. Thanks to this, we can easily see that instructions at `10001047` and `1000105C` refer to the same variable, even though in raw form they use different offsets (⁠`[esp+8]` and `[esp+4]`).

Extra information: [IDA Help: Stack Variables Window](<https://www.hex-rays.com/products/ida/support/idadoc/488.shtml>)

---

## 2. 

**Date:** Posted: Nov 12, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-64-full-screen-mode](https://hex-rays.com/blog/igors-tip-of-the-week-64-full-screen-mode)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #64: Full-screen mode

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-64-full-screen-mode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-64-full-screen-mode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-64-full-screen-mode>)

I 

Igor Skochinsky ✦ Posted: Nov 12, 2021

![Igor’s tip of the week #64: Full-screen mode](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

While not commonly used, full-screen mode can be useful on complex IDA layouts when working with a single monitor or on a laptop, for example when you need to read a long listing line but are tired of scrolling around.

The feature is somewhat hidden, but the action _is_ present in the View menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fullscreen_menu-3.png?width=344&height=396&name=fullscreen_menu-3.png)

By pressing `F11`, the current view is temporarily expanded to full screen, and all other views and UI elements (toolbars, menus) are hidden to remove all distractions.

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fullscreen_std-3.png?width=640&height=350&name=fullscreen_std-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/fullscreen_std-3.png>)

⬇

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fullscreen_exp-3.png?width=640&height=360&name=fullscreen_exp-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/fullscreen_exp-3.png>)

To go back to the standard layout, just press `F11` again.

---

## 3. 

**Date:** Posted: Nov 10, 2021  
**Link:** [https://hex-rays.com/blog/ida-training-sessions-december-2021](https://hex-rays.com/blog/ida-training-sessions-december-2021)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Training](<https://hex-rays.com/blog/tag/training>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [idatraining](<https://hex-rays.com/blog/tag/idatraining>)

# IDA Training Sessions – December 2021

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-training-sessions-december-2021>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-training-sessions-december-2021>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-training-sessions-december-2021>)

Holly Walsh ✦ Posted: Nov 10, 2021

![IDA Training Sessions – December 2021](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

IDA is the Swiss army knife of reverse-engineering and has countless applications that can’t be summarized with a catchy one-liner. Security experts, malware analysts, and software engineers use IDA daily to solve a critical problem in their workflow. Improving your knowledge of IDA through one of our training sessions can help you to unlock the full potential of IDA.

Here are some examples of IDA’s capabilities:

  * **Digital Forensics** : analyzing binary code is an art and attending our training will enrich your set of tools/methods.
  * **Penetration Testing** : Software developers regularly attack their own software for the purpose of hardening security, becoming an expert in IDA will help to improve effectiveness.
  * **Intellectual Property** : For software IP, the task of finding and proving violations represents a challenge. Learn how to use IDA as a foundation to develop an automatic system to tackle this.
  * **Dynamic Analysis and Debugging** : IDA Pro can debug applications on all major desktop platforms, mobile platforms and emulators.
  * **Interoperability** : To deal with exotic file formats, IDA Pro can be easily extended with custom-crafted “loaders” and make the data available from within the user Interface.
  * **Software Assessment** : IDA supports all major architectures used in desktop, mobile, and embedded devices. It can be used to disassemble binaries with or without debug info. In our training sessions we take you through the ins and outs of the disassembly process.
  * And many more… Check out our[ solutions page](<https://hex-rays.com/solutions/>) to read more and explore various case studies on real world IDA usages!



We run training courses for beginner to expert IDA users. We have designed these sessions with our users in mind, think [Igor’s tip of the week](<https://hex-rays.com/blog/igors-tip-of-the-week-season-01/>) but as a training session. Learn the various tips and tricks while getting firsthand experience with guidance from IDA experts to ensure you have all the tools necessary to make the most of IDA in your organization and beyond.   
Our next training sessions are scheduled for December; find out more <https://hex-rays.com/training/>

**Have a look at what past attendees have said about their training experience:**

___I feel that the training was well worth the money, and I plan on taking it again any chance I get._ __

___I have used IDA compiler more than 4 years. However, lots of detail hide in devil, and this course helped me know more about reversing and design principal of IDA Pro._ __

___That this training should be mandatory before using IDA to use all capabilities and enhance/improve performance during analysis_ __

___Great job for first time online course. I have benefited from not traveling and saving money._ __

___Great training! Thank you for providing videos!_ __

___I am looking forward to improving enough to be able to move on to your advanced course._ __

___It gives you deep understanding for internal mechanism how IDA process files and to interacting them_ __

___I was very pleased with the course. The instructor and the material are very good. I liked the way that the instructor taught by doing. He came across humble, honest and knowledgeable._ __

___Both instructors were really awesome. Hope to see more of this stuff in the future. Thank you for this time._ __

_…_

---

## 4. 

**Date:** Posted: Nov 5, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-63-ida-installer-command-line-options](https://hex-rays.com/blog/igors-tip-of-the-week-63-ida-installer-command-line-options)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #63: IDA installer command-line options

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-63-ida-installer-command-line-options>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-63-ida-installer-command-line-options>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-63-ida-installer-command-line-options>)

I 

Igor Skochinsky ✦ Posted: Nov 5, 2021

![Igor’s tip of the week #63: IDA installer command-line options](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Most users probably run IDA installers in standard, interactive mode. However, they also can be run in unattended mode (e.g. for automatic, non-interactive installation).

### Available options

To get the list of available options, run the installer with the `--help` argument. For example, here’s the list on Linux:
    
    
    igor@/home/igor$ ./idapronl[...].run --help
    IDA Pro and Hex-Rays Decompilers (x86, x64, ARM, ARM64, PPC, PPC64, MIPS) 7.6 7.6
    Usage:
    
     --help                                      Display the list of valid options
    
     --version                                   Display product information
    
     --unattendedmodeui <unattendedmodeui>	Unattended Mode UI
                                                 Default: none
                                                 Allowed: none minimal minimalWithDialogs
    
     --optionfile <optionfile>                  Installation option file
                                                 Default:
    
     --debuglevel <debuglevel>                   Debug information level of verbosity
                                                 Default: 2
                                                 Allowed: 0 1 2 3 4
    
     --mode <mode>                               Installation mode
                                                 Default: gtk
                                                 Allowed: gtk xwindow text unattended
    
     --debugtrace <debugtrace>                   Debug filename
                                                 Default:
    
     --installer-language <installer-language>   Language selection
                                                 Default: en
                                                 Allowed: sq ar es_AR az eu pt_BR bg ca hr cs da nl en et fi fr de el he hu id it ja kk ko lv lt no fa pl pt ro ru sr zh_CN sk sl es sv th zh_TW tr tk uk va vi cy
    
     --prefix <prefix>                           Installation Directory
                                                 Default: /home/igor/idapro-7.6
    
     --python_version <python_version>           IDAPython Version
                                                 Default: 3
                                                 Allowed: 2 3
    
     --installpassword <installpassword>         Installation password
                                                 Default:
    

For example, to run the installer in text (console) instead of GUI mode, specify `--mode text`.

On Windows, the set of options is slightly different (and is shown in a GUI dialog instead of console):
    
    
    IDA Pro and Hex-Rays Decompilers (x86, x64, ARM, ARM64, PPC, PPC64, MIPS) 7.6 7.6
    Usage:
    
     --help                                      Display the list of valid options
    
     --version                                   Display product information
    
     --unattendedmodeui <unattendedmodeui>       Unattended Mode UI
                                                 Default: none
                                                 Allowed: none minimal minimalWithDialogs
    
     --optionfile <optionfile>                   Installation option file
                                                 Default: 
    
     --debuglevel <debuglevel>                   Debug information level of verbosity
                                                 Default: 2
                                                 Allowed: 0 1 2 3 4
    
     --mode <mode>                               Installation mode
                                                 Default: win32
                                                 Allowed: win32 unattended
    
     --debugtrace <debugtrace>                   Debug filename
                                                 Default: 
    
     --installer-language <installer-language>   Language selection
                                                 Default: en
                                                 Allowed: sq ar es_AR az eu pt_BR bg ca hr cs da nl en et fi fr de el he hu id it ja kk ko lv lt no fa pl pt ro ru sr zh_CN sk sl es sv th zh_TW tr tk uk va vi cy
    
     --prefix <prefix>                           Installation Directory
                                                 Default: C:\Program Files/idapro-7.6
    
     --python_version <python_version>           IDAPython Version
                                                 Default: 3
                                                 Allowed: 2 3
    
     --installpassword <installpassword>         Installation password
                                                 Default: 
    
     --install_python <install_python>           Install Python 3
                                                 Default: 
    

In partcular, `--install_python` option allows to enable installation of the bundled Python 3 (useful for machines without Python preinstalled). On Linux and Mac, the system-wide Python is presumed to be available

### Using the option file

Especially for unattended mode, you may need to specify multiple options (install path, Python version, installation password etc.). Instead of passing them all on the command line, you can use the _option file:_

  1. Create a simple text file with a list of `option=value` lines. The option names are those from the usage screen without the leading `--`.
  2. Pass the filename to the installer using the `--optionfile <filename>` switch.



For example, here’s a file for unattended install for Python 3:
    
    
    installpassword=mypassword
    prefix=/home/igor/ida-7.6
    mode=unattended
    python_version=3

### Running Mac installer from command line

Because the Mac installer is not a single binary but an app bundle, you need to pass arguments to the executable inside the bundle, for example:
    
    
    ida[...].app/Contents/MacOS/installbuilder.sh --mode text

### Uninstaller options

The uninstaller can also be run with commandline opions:

`~/idapro-7.6/uninstall --mode unattended`

---

## 5. 

**Date:** Posted: Oct 29, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-62-creating-custom-type-libraries](https://hex-rays.com/blog/igors-tip-of-the-week-62-creating-custom-type-libraries)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #62: Creating custom type libraries

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-62-creating-custom-type-libraries>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-62-creating-custom-type-libraries>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-62-creating-custom-type-libraries>)

I 

Igor Skochinsky ✦ Posted: Oct 29, 2021

![Igor’s tip of the week #62: Creating custom type libraries](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Previously we’ve talked about [using type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>) shipped with IDA, but what can be done when dealing with uncommon or custom APIs or SDKs not covered by them? 

In such situation it is possible to use the `tilib` utility available for IDA Pro users from our [download center](<https://hex-rays.com/download-center/>).

### Creating type libraries

`tilib` is a powerful command-line utility and the full list of options may look somewhat scary.
    
    
    Type Information Library Utility v1.227 Copyright (c) 2000-2021 Hex-Rays
    usage: tilib [-sw] til-file
      -c     create til-file              -t...  set til-file title
      -h...  parse .h file                -P     C++ mode (not ready yet)
      -D...  define a symbol              -I...  list of include dirs
      -M...  create macro defs file       -x     external display types
      -i     internal display types       -z     debug .h file parsing (use it!)
      -B...  dump bad macro defs          -q     internal check: unpack types
      -C...  compiler info(-C? help)      -G...  mangling format (n=org.name)
      -m...  parse macro defs file        -S     strip macro table
      -dt... delete type definition       -rtX:Y rename type X as Y
      -ds... delete symbol definition     -rsX:Y rename symbol X as Y
      -b...  use base til                 -o...  directory with til files
      -l[1csxf] show til-file contents; 1-with decorated names, c-as c code
               s-dump struct layout, x-exercise udt serialization, f-dump funcarg locations
      -v     verbose                      -e     ignore errors
      -R     allow redeclarations         -n     ignore til macro table
      -u+    uncompress til-file          -u-    compress til-tile
      -U     set 'universal til' bit      -em    suppress macro creation errors
      -#     enable ordinal types         -#-    disable ordinal types
      -p...  load types from PDB (Win32)  -TL    lower existing type
      -TAL   assume low level types       -TH    keep high types
      -g[nb]X:Y move macro X (regex) to group Y; n-name, b-body
       @...  response file with switches
    example: tilib -c -Cc1 -hstdio.h stdio.til
    

However, as mentioned at the botttom, the basic usage can be quite simple:

`tilib -c -Cc1 -hstdio.h stdio.til`

This creates a type library `stdio.til` by parsing the header file `stdio.h` as a Visual C++ compiler.

### Advanced options

The sample commandline might work in simple cases (e.g. a single, self-contained header) but with real life SDKs you will likely run into problems quickly. To handle them, additional options may be necessary:

  1. Include directories for headers from `#include` directives: `-I<directory>` (can be specified multiple times);
  2. preprocessor defines: `-Dname[=value]`;



Instead of using `-D` on command line, you can also create a new header with `#define` statements and include other headers from it.

### Response files

To avoid specifying the same options again and again, you can use _response files_. These files contain one command line option per line and can be passed to `tilib` using the `@` option:

`tilib @vc32.cfg -c -hinput.h output.til`

There are sample response files shipped with the tilib package for Visual C++ (32- and 64-bit), GCC and Borland C++.

### Examining type libraries

You can dump the contents of a til file using the `-l` switch:

`tilib -l mylib.til`

### Using created type libraries in IDA

To make the custom type library available in IDA, copy it in the `til/<processor>` subdirectory of IDA. For example, libraries for x86/x64 files should go under `til/pc`. After this, the new library should appear in the list shown when you invoke the “Load type library” command.

### Advanced example

One of our users made a very nice write-up on generating a type library for Apache modules. Please find it here: <https://github.com/trou/apache-module-ida-til>.

See also readme.txt in the tilib package for advanced usage such as creating enums from groups of preprocessor macro definitions.

---

## 6. 

**Date:** Posted: Oct 22, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-61-status-bars](https://hex-rays.com/blog/igors-tip-of-the-week-61-status-bars)

[Back](<https://hex-rays.com/blog>)

[UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #61: Status bars

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-61-status-bars>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-61-status-bars>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-61-status-bars>)

I 

Igor Skochinsky ✦ Posted: Oct 22, 2021

![Igor’s tip of the week #61: Status bars](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Many of IDA’s windows have status bars and they contain useful information and functionality which may not be always obvious.

### Main window status bar

The status bar at the bottom of IDA’s main window contains:

  1. Autoanalysis progress indicator. See [IDA Help: Analysis options](<https://hex-rays.com/products/ida/support/idadoc/620.shtml>) for possible values you may see there.
  2. Search direction indicator for [“Next search” commands](<https://hex-rays.com/blog/igors-tip-of-the-week-58-keyboard-modifiers/>) (`Ctrl`+Letter);
  3. Free disk space



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/status1-3.png?width=436&height=84&name=status1-3.png)

It also has a context menu offering quick access to analysis and processor-specific options (if supported by current processor module).

### Disassembly view status bar

Each disassembly (IDA View) windows has a separate status bar too. In the text mode it contains:

  1. offset in the input file (for addresses which can be mapped directly to the input file);
  2. address on the current cursor position 
  3. symbolic location (if available). for locations inside functions, a function name and offset from its start is printed;
  4. synchronization status.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/status2-3.png?width=534&height=50&name=status2-3.png)

Same status bar style is also used for Hex View and Pseudocode windows.

In the [Graph mode](<https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view/>), additional graph-related information is displayed (zoom level, mouse position etc.).

### Chooser (list view) status bar

[List views’](<https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list/>) status bars by default display the current index and total number of items in the list

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/status3-3.png?width=544&height=264&name=status3-3.png)

However, when using incremental search (type the first letters of the item to jump to the matching item), the typed letters replace it.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/status3b-3.png?width=544&height=264&name=status3b-3.png)

---

## 7. 

**Date:** Posted: Oct 15, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries](https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #60: Type libraries

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries>)

I 

Igor Skochinsky ✦ Posted: Oct 15, 2021

![Igor’s tip of the week #60: Type libraries](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Type libraries are collections of high-level type information for selected platforms and compilers which can be used by IDA and the decompiler.

A type library may contain:

  1.      1. function prototypes, e.g.:  

            
            void *__cdecl memcpy(void *, const void *Src, size_t Size);
            BOOL __stdcall EnumWindows(WNDENUMPROC lpEnumFunc, LPARAM lParam);

     2. typedefs, e.g.:  

            
            typedef unsigned long DWORD;
            BOOL (__stdcall *WNDENUMPROC)(HWND, LPARAM);
            

     3. standard structure and enum definitions, e.g.:  

            
            struct tagPOINT
            {
             LONG x;
             LONG y;
            };
            enum tagSCRIPTGCTYPE
            {
              SCRIPTGCTYPE_NORMAL = 0x0,
              SCRIPTGCTYPE_EXHAUSTIVE = 0x1,
            };
            

     4. Synthetic enums created from groups of preprocessor definitions (macros):  

            
            enum MACRO_WM
            {
              WM_NULL = 0x0,
              WM_CREATE = 0x1,
              WM_DESTROY = 0x2,
              WM_MOVE = 0x3,
              WM_SIZEWAIT = 0x4,
              WM_SIZE = 0x5,
              WM_ACTIVATE = 0x6,
              WM_SETFOCUS = 0x7,
              WM_KILLFOCUS = 0x8,
              WM_SETVISIBLE = 0x9,
              [...]
             };




### Manipulating type libraries

The list of currently loaded type libraries is available in the Type Libraries view (View > Open subiews > Type Libraries, or `Shift`–`F11`).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/til_list-3.png?width=667&height=295&name=til_list-3.png)

Additional libraries can be loaded using “Load type library…” context menu item or the `Ins` hotkey.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/til_add-3.png?width=491&height=550&name=til_add-3.png)

Once loaded, definitions from the type library can be used in IDA and the decompiler: you can use them in function prototypes and global variable types (`Y` hotkey), as well as when adding new definitions in [Local Types](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>).

### Importing types into IDB

While the decompiler can use types from loaded type libraries without extra work, to use them in the disassembly some additional action may be necessary. For example, to use a standard structure or enum, it has to be added to the list in the corresponding view first:

  1. Open the Structures (`Shift`–`F9`) or Enums (`Shift`–`F10`) window;
  2. Select “Add struct type..” or “Add enum” from the context menu, or use the hotkey (`Ins`);
  3. If you know the struct/enum name, enter it in the name field and click OK;  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/til_addstruct-3.png?width=900&height=272&name=til_addstruct-3.png)
  4. If you don’t know or remember the exact name, click “Add standard structure” (“Add standard enum”) and select the struct or enum from the list of all corresponding types in the loaded type libraries. As with all [choosers](<https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida/>), you can use incremental search or filtering (`Ctrl`–`F`).  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/til_choosestruct-3.png?width=748&height=482&name=til_choosestruct-3.png)



After importing, the structure or enum can be used in the disassembly view.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/til_useestruct-3.png?width=698&height=232&name=til_useestruct-3.png)

### Function prototypes

When a type library is loaded, functions with name matching the prototypes present in the library will have their prototypes applied in the database. Alternatively, you can rename functions after loading the library, like we described [last week](<https://hex-rays.com/blog/igors-tip-of-the-week-59-automatic-function-arguments-comments/>).

---

## 8. 

**Date:** Posted: Oct 8, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-59-automatic-function-arguments-comments](https://hex-rays.com/blog/igors-tip-of-the-week-59-automatic-function-arguments-comments)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #59: Automatic function arguments comments

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-59-automatic-function-arguments-comments>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-59-automatic-function-arguments-comments>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-59-automatic-function-arguments-comments>)

I 

Igor Skochinsky ✦ Posted: Oct 8, 2021

![Igor’s tip of the week #59: Automatic function arguments comments](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

You may have observed that IDA knows about standard APIs or library functions and adds automatic function comments for the arguments passed to them.

For example, here’s a fragment of disassembly with commented arguments to Win32 APIs `CreateFileW` and `ReadFile`:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pit_args1-3.png?width=441&height=535&name=pit_args1-3.png)

This works well when functions are imported in a standard way and are known at load time. However, there may be cases when the actual function is only known after analysis (e.g. imported dynamically using `GetProcAddress` or using a name hash). In that case, there may be only a call to some dummy name and no commented arguments:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pit_unmarked-3.png?width=419&height=499&name=pit_unmarked-3.png)

You can of course add a comment that `dword_4031A5` is `CreateFileA`, and comment arguments manually, but this can be quite tedious. Is there a way to do it automatically? 

In fact, it is sufficient to simply rename the pointer variable to the corresponding API name for IDA to pick up the prototype and comment the arguments:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pit_marked-3.png?width=610&height=500&name=pit_marked-3.png)

A few notes about this feature:

  1. The function prototype must be present in one of the loaded type libraries;
  2. The comments are added only for code inside a function, so you may need to create one around the call (e.g. in case of decrypted or decompressed code);
  3. if the function is called in many places, it may take a few seconds for IDA to analyze and comment all call sites.

---

## 9. 

**Date:** Posted: Oct 1, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-season-01](https://hex-rays.com/blog/igors-tip-of-the-week-season-01)

[Back](<https://hex-rays.com/blog>)

[idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week: Season 01

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-season-01>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-season-01>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-season-01>)

Geoffrey Czokow ✦ Posted: Oct 1, 2021

![Igor’s tip of the week: Season 01](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

For over a year now, our colleague Igor has been disseminating his deep knowledge of IDA & decompilers through [“Igor’s tip of the week”](<https://hex-rays.com/blog/tag/idatips/>) blog posts. Those have received critical acclaim, and we’ve received a lot of very positive feedback during that time. This motivated us to find a way to improve the accessibility to this resource.

Today, Igor is taking a well-deserved holiday, so we took that opportunity to compile a first “season” of his precious tips (**52 of them!**), categorized by high-level topics.

  * **Usage:** basic and advanced usage of IDA features
  * **Navigation:** moving around the database
  * **Types:** working with types
  * **Hidden:** hidden gems, not widely known but useful functionality
  * **Decompiler:** related to the Hex-Rays decompiler
  * **Automation:** automating repetitive tasks
  * **Customization:** customizing IDA UI to better suit your workflow



We’re sure this valuable resource (now available online and offline) will be very useful to many!

[![Igor-tip-book-S01](https://hex-rays.com/hubfs/Imported_Blog_Media/igor-poster-3.png)](<https://hex-rays.com/hubfs/freefile/igor-tip-of-the-week-S01.pdf>)

[Download now!](</hubfs/freefile/igor-tip-of-the-week-S01.pdf>)

You can also take a look at [ **Igor’s tip of the Week: Season 02** ](<https://hex-rays.com/blog/igors-tip-of-the-week-season-02/>)

Would you like to use IDA effectively? Attend our [training](<https://hex-rays.com/training/>) and learn even more tips and tricks like these!

---

