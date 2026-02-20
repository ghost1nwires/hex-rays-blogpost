# Hex-Rays Blog – Page 47

**Archived:** 2026-02-20 12:24
**URL:** https://hex-rays.com/blog/page/47

---

## 1. 

**Date:** Posted: Jun 15, 2009  
**Link:** [https://hex-rays.com/blog/ida-pro-5-5-and-hex-rays-1-1-have-been-released](https://hex-rays.com/blog/ida-pro-5-5-and-hex-rays-1-1-have-been-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 5.5 and Hex-Rays 1.1 have been released!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-5-5-and-hex-rays-1-1-have-been-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-5-5-and-hex-rays-1-1-have-been-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-5-5-and-hex-rays-1-1-have-been-released>)

Ilfak Guilfanov ✦ Posted: Jun 15, 2009

![IDA Pro 5.5 and Hex-Rays 1.1 have been released!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

### IDA Pro 5.5

We are happy to announce a new version of IDA Pro! The major news is the  
new docking user interface. There are many other improvements: processor modules,  
file formats, analysis tweaks, well, the usual stuff. There is a new MS Windows  
Crash Dump Loader and improved Bochs debugger. The complete list of new  
features and bug fixes is available here

<http://www.hex-rays.com/idapro/55/index.htm>

### Hex-Rays 1.1

We also release a new version of our decompiler: now with the floating point  
support. It was a technically challenging task and required lots of testing, but  
we are very happy with the end result. It can really handle floating point  
computations and generates reliable output. All subtle nuances, like conversion  
rules, fpu stack state, predefined compiler helper functions, are all taken care of.

The decompiler uses debug information if it is available: in this case, even local  
variable names and types will be restored. If there is no debug information, the  
decompiler will still generate correct and precise output. In fact, it is designed  
to work without debug information, which means that virtually any  
compiler-generated executable can be analyzed and turned into C output.

### New pricing and support plans

With this release, we update the pricing of IDA Pro and Hex-Rays Decompiler.  
While the initial purchase prices are increased, upgrade prices go down.  
In order to streamline the upgrade process, we will use the same rules for  
all our products: now a support plan is renewable any time while it is active  
and also three months after its expiration. The new support period is counted from  
the expiration date of the previous support period.

If you upgraded your IDA/Hex-Rays copy the last month with older prices,  
do not worry. For you, we will add a month of support for the IDA license,  
and three months of support for Hex-Rays Decompiler.

We will continue to accept old-style upgrade orders until 12 October 2009.

### How to request the new versions

As usual, the new versions are free for users whose licenses are within active  
support plan. Submit your ida.key to

[https://www.hex-rays.com/updida.shtm](<https://www.hex-rays.com/updida/>)l

and expect a message from us within 5-10 minutes. Sometimes we do not have your  
email in the database, so please specify it (otherwise we will have no means of  
communicating with you).

To request the new version of the decompiler, please use Edit, Plugins, Hex-Rays,  
Check for updates in IDA.

### Is your key too old?

If your key is too old for a free update, you might still be  
eligible for a discounted upgrade. Until 12 October 2009 we offer the upgrade  
prices for all purchases made two years ago or less. The order forms can be  
found here:

<http://www.hex-rays.com/idapro/idaorder.htm>

We will arrange an electronic delivery to existing customers.

That’s all folks! Enjoy the release.

---

## 2. 

**Date:** Posted: Jun 2, 2009  
**Link:** [https://hex-rays.com/blog/ida-pro-5-5-goes-alpha](https://hex-rays.com/blog/ida-pro-5-5-goes-alpha)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 5.5 goes alpha

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-5-5-goes-alpha>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-5-5-goes-alpha>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-5-5-goes-alpha>)

Elias Bachaalany ✦ Posted: Jun 2, 2009

![IDA Pro 5.5 goes alpha](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

After many months of work, IDA Pro 5.5 is now in alpha stage and this week the beta will be out for testing.  
  
In this version many small and big improvements took place, here is a partial change list:

  * PC module improvements 
  * PE loader improvements 
  * ARM processor module improvements 
  * Improved Hex-View: 
    * Edit support 
    * Data display format: words, dwords, doubles, … 
    * Unicode or custom codepages 
  * Bochsrc file loader: load a bochsrc file and start debugging the disk image 
  * Windows Crash dump support: IDA now accepts MS Windows Crash dump files. Load the crash dump file and IDA Pro will create a database with the memory contents of the crash (if they were included). You can also run the Windbg debugger module and issue commands to the debugger engine to investigate more about the crash 
  * Docking interface: all windows are now dockable, allowing you to make optimal use of the desktop space. 



This is for example how a desktop configuration could look like:

[![main interface example](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/main55-thumb-3.gif?width=480&height=285&name=main55-thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/main55-3.gif>)

And this is another desktop configuration for the debugger:

[![debugger interface example](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dbg55-thumb-3.gif?width=480&height=285&name=dbg55-thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/dbg55-3.gif>)

---

## 3. 

**Date:** Posted: Apr 17, 2009  
**Link:** [https://hex-rays.com/blog/ida-v5-4-demo](https://hex-rays.com/blog/ida-v5-4-demo)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA v5.4 demo

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-v5-4-demo>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-v5-4-demo>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-v5-4-demo>)

Ilfak Guilfanov ✦ Posted: Apr 17, 2009

![IDA v5.4 demo](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Just a quick note for interested parties: we prepared the new demo version of IDA Pro. The new demo includes the bochs debugger. The debugger is fully functional with just one limitation: it will become inactive after a number of commands. I prefer to tell you this in advance rather than this limitation to be discovered in the middle of a heavy debugging session.

---

## 4. 

**Date:** Posted: Feb 19, 2009  
**Link:** [https://hex-rays.com/blog/advanced-windows-kernel-debugging-with-vmware-and-idas-gdb-debugger](https://hex-rays.com/blog/advanced-windows-kernel-debugging-with-vmware-and-idas-gdb-debugger)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Advanced Windows Kernel Debugging with VMWare and IDA’s GDB debugger

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/advanced-windows-kernel-debugging-with-vmware-and-idas-gdb-debugger>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/advanced-windows-kernel-debugging-with-vmware-and-idas-gdb-debugger>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/advanced-windows-kernel-debugging-with-vmware-and-idas-gdb-debugger>)

Hex-Rays ✦ Posted: Feb 19, 2009

![Advanced Windows Kernel Debugging with VMWare and IDA’s GDB debugger](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

We have already published short tutorial on Windows kernel debugging with IDA and VMWare on our site, but the debugging experience can still be improved.

VMWare’s GDB stub is very basic, it doesn’t know anything about processes or threads (for Windows guests), so for anything high-level we’ll need to do some extra work. We will show how to get the loaded module list and load symbols for all them using IDAPython.

#### Preparing VM for debugging

Let’s assume that you already have a VM with Windows (32-bit) installed. Before starting the debugging, copy files for which you want to see symbols to the host. If you’re not sure, copy nt*.exe and hal.dll from System32, and the whole System32\drivers directory.

Edit the VM’s .vmx file to enable GDB debugger stub:

![graphics28](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_91f1c9c-4.png)

Add these lines to the file:
    
    
    debugStub.listen.guest32 = "TRUE"
    debugStub.hideBreakpoints= "TRUE"

Save the file.

In VMWare, click "Power on this virtual machine" or click the green Play button on the toolbar.

![graphics7](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_100c5d47-4.png)

Wait until the VM boots.

#### Debugging in IDA

Start IDA.

![graphics8](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_m20f96fb6-4.gif)

If you get the welcome dialog, choose "Go".

![graphics32](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_519c4c2f-4.gif)

Choose Debugger | Attach | Remote GDB debugger.

![graphics33](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_6b9e4a85-4.gif)

Enter "localhost" for hostname and 8832 for the port number.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_m104fe357-4.gif)

Choose <attach to the process started on target> and click OK.

![graphics9](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_m15202098-4.png)

The execution should stop somewhere in the kernel (address above 0x80000000). You can step through the code, but it’s not very convenient without any names. Let’s try to gather some more information.

#### Getting the module list

The list of kernel modules is stored in the list pointed to by the `PsLoadedModuleList`symbol in the kernel. To find its address, we will use the so-called"KPCR trick". KPCR stands for Kernel Processor Control Region. It is used by the kernel to store various information about each processor. It is placed at the base of the segment pointed to by the `fs`register (similar to TEB in user mode). One of the fields in it is`KdVersionBlock`which points to a structure used by the kernel debugger. It, in turn, has various pointers to kernel structures, including`PsLoadedModuleList`.

Definition of the KPCR structure can be found in many places, including IDA’s ntddk.til. Right now we just need to know that `KdVersionBlock`field is situated at offset 0x34 from the start of KPCR. It points to`DBGKD_GET_VERSION64`, which has `PsLoadedModuleList`pointer at offset 0x18.

Let’s write a small Python function to find the value of that pointer. To retrieve the base of the segment pointed to by fs, we can use the VMWare’s debug monitor "r" command. GDB debugger plugin registers an IDC function `SendGDBMonitor()`to send commands to the monitor, and we can use IDAPython’s `Eval()`function to call it:
    
    
    fs_str = Eval('SendGDBMonitor("r fs")')

Returned string has the following format:
    
    
    fs 0x30 base 0x82744a00 limit 0x00002008 type 0x3 s 1 dpl 0 p 1 db 1

We need the address specified after"base":
    
    
    kpcr = int(fs_str[13:23], 16) #extract and convert as base 16 (hexadecimal) number

Then get the value of `KdVersionBlock`:
    
    
    kdversionblock = Dword(kpcr+0x34)

And finally `PsLoadedModuleList`:
    
    
    PsLoadedModuleList = Dword(kdversionblock+0x18)

#### Walking the module list

`PsLoadedModuleList`is declared as `PLIST_ENTRY`. `LIST_ENTRY` is a structure which represents a member of a double-linked list:
    
    
    typedef struct _LIST_ENTRY
    {
         PLIST_ENTRY Flink;
         PLIST_ENTRY Blink;
    } LIST_ENTRY, *PLIST_ENTRY;

So, we just need to follow the `Flink` pointer until we come back to where we started. A single entry of the list has the following structure:
    
    
    struct LDR_MODULE
    {
      LIST_ENTRY InLoadOrderModuleList;
      LIST_ENTRY InMemoryOrderModuleList;
      LIST_ENTRY InInitializationOrderModuleList;
      PVOID BaseAddress;
      PVOID EntryPoint;
      ULONG SizeOfImage;
      UNICODE_STRING FullDllName;
      UNICODE_STRING BaseDllName;
      ULONG Flags;
      SHORT LoadCount;
      SHORT TlsIndex;
      LIST_ENTRY HashTableEntry;
      ULONG TimeDateStamp;
    };

Now we can write a small function to walk this list and create a segment for each module:
    
    
    #get the first module
    cur_mod = Dword(PsLoadedModuleList)
    while cur_mod != PsLoadedModuleList and cur_mod != BADADDR:
      BaseAddress  = Dword(cur_mod+0x18)
      SizeOfImage  = Dword(cur_mod+0x20)
      FullDllName  = get_unistr(cur_mod+0x24)
      BaseDllName  = get_unistr(cur_mod+0x2C)
      #create a segment for the module
      SegCreate(BaseAddress, BaseAddress+SizeOfImage, 0, 1, saRelByte, scPriv)
      #set its name
      SegRename(BaseAddress, BaseDllName)
      #get next entry
      cur_mod = Dword(cur_mod)

#### Loading symbols

Having the module list is nice, but not very useful without symbols. We can load the symbols manually for each module using File | Load File | PDB file… command, but it would be better to automate it.

For that we can use the PDB plugin. From looking at its sources (available in the SDK), we can see that it supports three "call codes":
    
    
    //call_code==0: user invoked 'load pdb' command, load pdb for the input file
    //call_code==1: ida decided to call the plugin itself
    //call_code==2: load pdb for an additional exe/dll
    //              load_addr: netnode("$ pdb").altval(0)
    //              dll_name:  netnode("$ pdb").supstr(0)

Call code 2 looks just like what we need. However, current IDAPython includes a rather basic implementation of netnode class and it is not possible to set supvals from Python. However, if we look at handling of the other call codes, we can see that the plugin retrieves module base from `"$ PE header"` netnode and module path using `get_input_file_path()` function. IDAPython’s`netnode.altset()` function does work, and we can use set_root_filename() to set the input file path. Also, if we pass a call code 3, we will avoid the "Do you want to load the symbols?" prompt.
    
    
    #new netnode instance
    penode = idaapi.netnode()
    #create netnode the in database if necessary
    penode.create("$PE header")
    #set the imagebase (-2 == 0xFFFFFFFE)
    penode.altset(0xFFFFFFFE, BaseAddress)
    #set the module filename
    idaapi.set_root_filename(filename)
    #run the plugin
    RunPlugin("pdb",3)

However, we need to replace the kernel-mode path by the local path beforehand:
    
    
    #path to the local copy of System32 directory
    local_sys32 = r"D:\VmWareShared\w7\System32"
    if FullDllName.lower().startswith(r"\systemroot\system32"):
    #translate into local filename
    filename = local_sys32 + FullDllName[20:]

Now we can gather all pieces into a single script.

After running it, you should have a nice memory map:

![graphics1](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_m17b305f6-4.gif)

…and name list:

![graphics2](https://hex-rays.com/hubfs/Imported_Blog_Media/gdb-vmware-winkernel2_htm_65b4d822-4.gif)

Looks much better now. Happy debugging!

---

## 5. 

**Date:** Posted: Feb 5, 2009  
**Link:** [https://hex-rays.com/blog/ida-pro-has-9-debugger-modules](https://hex-rays.com/blog/ida-pro-has-9-debugger-modules)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro has 9 debugger modules

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-has-9-debugger-modules>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-has-9-debugger-modules>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-has-9-debugger-modules>)

Ilfak Guilfanov ✦ Posted: Feb 5, 2009

![IDA Pro has 9 debugger modules](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Since the number of debugger modules in IDA surpassed [the magical number seven plus or minus two](<http://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two>), we created a small table describing what is available and what is not:  
<http://www.hex-rays.com/idapro/debugger/index.htm>  
Direct links to tutorials are available here:  
<http://www.hex-rays.com/idapro/idasupport.htm>  
I know, I know – we need to add 64-bit support for all platforms, port the Bochs debugger module to Linux, and… any other suggestions? I personally would love to have source level debugging, yet it requires some substantial changes to the kernel. We probably will move in this direction, sooner or later…

---

## 6. 

**Date:** Posted: Jan 30, 2009  
**Link:** [https://hex-rays.com/blog/kernel-debugging-with-ida](https://hex-rays.com/blog/kernel-debugging-with-ida)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Kernel debugging with IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/kernel-debugging-with-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/kernel-debugging-with-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/kernel-debugging-with-ida>)

Elias Bachaalany ✦ Posted: Jan 30, 2009

![Kernel debugging with IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When IDA introduced debugging facilities years ago, the task of analyzing hostile code became more enriched:  
no more looking at static code and figuring out what it does, instead just run the malware in a virtual  
machine and debug it remotely, even debug just a small code snippet from the database (Bochs based debugger plugin).

With IDA 5.4 release, in addition to the Bochs and GDB plugins, we also introduced a debugger plugin based  
on Microsoft’s Debugger Engine  
(the same engine used by Windbg, cdb and kd). With this addition to IDA you can now debug live kernel targets as well.

For user mode debugging the Windbg debugger plugin beats the win32 debugger plugin, by providing you access to a  
wide range of extensions that ship with the debugging tools from Microsoft.

For kernel debugging, you can use Bochs/Disk Image loader or GDB plugin to debug the whole operating system from Bios code and on.

However when Windbg plugin is used, you get the raw power of the debugging engine (extensions / built-in commands, symbols, …).

We prepared a video showing how to debug kernel mode and user mode at the same time with full symbolic information (provided from the PDB files).  
The video also demonstrates how to set breakpoints on user mode APIs and see them get triggered when any application in the system uses those APIs.  
Before viewing the video, for those willing to experiment with the Windbg debugger plugin to debug kernel mode and user mode at the same time,  
here is how to prepare a database:

  1. If you never used the Windbg debugger plugin before please visit the [Windbg plugin tutorial](<http://www.hex-rays.com/wp-content/uploads/2019/12/debugging_windbg.pdf>) page
  2. Setup a process server inside the VM and attach to it from IDA to debug just any user mode application
  3. Once attached, go to desired segments (kernel32, user32, advapi32, gdi32, etc…) and convert them to loader segments
  4. If symbol retrieval mechanism was properly configured then most system DLLs will have symbol information, otherwise only exported names will available
  5. Now we have a database with all user mode components we wish to inspect from the live kernel debugging session
  6. Using the same database, change the connection string so that it connects to the same VM for the purpose of live kernel debugging this time
  7. Once attached to the kernel, IDA will present loaded drivers and kernel mode modules in the debugger / modules list
  8. It is possible to convert to loader segments the kernel mode components of interest
  9. That’s it! The database is now suited for kernel debugging, yet contains names and addresses of user mode components



This video will put everything into perspective!

Your browser does not support the video element. Kindly update it to latest version.

---

## 7. 

**Date:** Posted: Jan 20, 2009  
**Link:** [https://hex-rays.com/blog/ida-v5-4-release-is-not-that-far-away](https://hex-rays.com/blog/ida-v5-4-release-is-not-that-far-away)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA v5.4 release is not that far away

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-v5-4-release-is-not-that-far-away>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-v5-4-release-is-not-that-far-away>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-v5-4-release-is-not-that-far-away>)

Ilfak Guilfanov ✦ Posted: Jan 20, 2009

![IDA v5.4 release is not that far away](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

I’m happy to inform you that we are entering the beta stage of IDA v5.4!  
In addition to numerous small and not that small improvements, the new version will have three debugger modules: **bochs, gdb, and windbg** , selectable on the fly (the active debugger session will be closed, though ;))

  * With the bochs debugger, we offer three different worlds:**run-any-code-snippet** facility, **windows-like-environment** for PE files, and **any-bochs-image** bare-bone machine emulation mode. You can read more about this module in our blog: [/blog/bochs-plugin-goes-alpha](</blog/bochs-plugin-goes-alpha>)
  * With gdb, **x86** and **arm** targets are supported. Among other things, it is possible to connect IDA to **QEMU** or debug a virtual machine inside **VMWare**. We tried it **iPhone** as well. However, while it works in some curcimstances, there were some problems on the gdbserver side. 
  * With windbg, **user** and **kernel** mode debugging is available. The debugger engine from Microsoft, which is currently the only choice for driver and kernel mode debugging, can be used from IDA. It can automatically load required **PDB** files and populate the listing with meaningful names, types, etc. Speaking of PDB files, IDA imports more information from them: local function variables and types are retrieved too, c++ base classes are handled, etc. 



The gdb and windbg debugger modules support local and remote debugging. We tried to make the debugger modules as open as possible: target-specific commands can be sent to all backend engines in a very easy and user-friendly way.  
As usual, better analysis and many minor changes have been made. If you spend plenty of time analyzing gcc generated binaries, you’ll certainly appreciate that IDA handles its weird way of preparing outgoing function arguments. Now it can trace and find arguments copies to the stack with **mov** statements.  
The new IDA will support **Python** out of box, thanks to Gergely Erdelyi, who kindly agreed the Python plugin to be included in the official distribution. In fact, the main IDA window will have a command line to enter any python (or other language) expressions and immediately get a result in the message window.  
We will prepare the detailed list of improvements later this week.

---

## 8. 

**Date:** Posted: Nov 21, 2008  
**Link:** [https://hex-rays.com/blog/ida-and-mips](https://hex-rays.com/blog/ida-and-mips)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA and MIPS

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-and-mips>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-and-mips>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-and-mips>)

Ilfak Guilfanov ✦ Posted: Nov 21, 2008

![IDA and MIPS](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

If you analyze MIPS binaries, you may find useful the following addition to IDA:  
http://www.binary-art.net/?p=1002  
This is MIPS emulator for Linux. It can generate an IDC script after emulation, which then can be applied to the database and make it more readable.

---

## 9. 

**Date:** Posted: Nov 7, 2008  
**Link:** [https://hex-rays.com/blog/bochs-plugin-goes-alpha](https://hex-rays.com/blog/bochs-plugin-goes-alpha)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Bochs plugin goes alpha

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/bochs-plugin-goes-alpha>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/bochs-plugin-goes-alpha>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/bochs-plugin-goes-alpha>)

Elias Bachaalany ✦ Posted: Nov 7, 2008

![Bochs plugin goes alpha](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Bochs debugger plugin is in alpha stage now, all of the 3 loaders mentioned in the  
[previous blog entry](</blog/bochs-emulator-and-ida#more>), are now complete.

Since we demonstrated briefly the IDB loader last time, we will demonstrate the PE loader this time,  
which will allow you to debug PE executables. For this we continue using the same malware however  
using the PE loader instead. Throughout the video you will see numerous tricks used by this malware  
and you will see how the debugger works smoothly with them.

Here are some of the features supported by it:

* SEH support: we try to mimic Windows as much as possible, for example the ICEBP instruction is  
a privileged instruction, but Windows report back a single step exception. Similarly Windows  
does not distinguish between 0xCC and 0xCD 0x03, so when an exception occurs, it reports that  
the exception address is always one byte before the trap. So if it was an INT 0x3 (CD03) then  
exception address will point to the 0x03 (in the middle of the instruction). We behave the same as Windows.
* TLS callbacks: TLS callbacks are normally parsed by IDA and presented as entry points (ctrl+E),  
so you can put breakpoints there and run your program.
* Extendible API emulation: you can provide implementation of a given API using scripting facilities,  
for example, in the video you see a call to GlobalAlloc, here is how it is actually implemented (from api_kernel32.idc):
    
    
    ///func=GlobalAlloc entry=k32_GlobalAlloc purge=8
    static k32_GlobalAlloc()
    {
    eax = BochsVirtAlloc(0, BX_GETPARAM(2), 1);
    return 0;
    }
    ///func=GlobalFree entry=k32_GlobalFree purge=4
    static k32_GlobalFree()
    {
    eax = BochsVirtFree(BX_GETPARAM(1), 0);
    return 0;
    }

A simple MessageBoxA replacement can be (from api_user32.idc):
    
    
    ///func=MessageBoxA entry=messagebox purge=0x10
    static messagebox()
    {
    auto param2;
    param2 = BX_GETPARAM(2);
    Message("I am messagebox function; %s\n", GetString(param2, -1, ASCSTR_C));
    eax = 1;
    // continue execution
    return 0;
    }

Use your own code: You can also write your own DLL and map it into the process’ space. You can then redirect  
existing APIs to your own functionality, for example:
    
    
    ///func=GetProcAddress entry=bochsys.BxGetProcAddress purge=8
    ///func=ExitProcess entry=bochsys.BxExitProcess purge=4
    ///func=GetModuleFileNameA entry=bochsys.BxGetModuleFileNameA purge=12
    ///func=GetModuleHandleA entry=bochsys.BxGetModuleHandleA purge=4

We redirect some functions to bochsys.dll which will do the job inside the process’ space.

Less demanding PE loader: by that you can for example load any PE file, including system drivers or dlls.  
Given that you emulate the API calls, you can theoretically trace such targets too.

Dependency resolution: You don’t have to emulate all APIs to use this plugin, by default if an API  
is not present, then a stub will be generated for it. For example, you can define a stub that will  
always return 0 for CreateFileA call, as:
    
    
    ///func=CreateFileA retval=0

Since CreateFileA is recognized by IDA’s IDS files, no need to specify the “purge” value, otherwise a full definition would look like:
    
    
    ///func=FuncName purge=N_bytes retval=VALUE

NT structures emulation: Some malware don’t use GetProcAddress or GetModuleHandle() for example, instead they try to  
parse the system structures and deduce these values.  
For this we also provide and build the basic structure of PEB and PEB_LDR_DATA, LDR_MODULE(s) and RTL_USER_PROCESS_PARAMETERS.  
If you need to inspect PEB structure of Win32 programs, then remember to grab  
the [ldrmodule.idc](<http://www.hex-rays.com/idapro/freefiles/ldrmodules.idc>) script from IDA’s download area.

Here’s the video:

Your browser does not support the video element. Kindly update it to latest version.

---

