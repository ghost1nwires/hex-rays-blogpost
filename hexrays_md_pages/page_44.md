# Hex-Rays Blog – Page 44

**Archived:** 2026-02-20 12:23
**URL:** https://hex-rays.com/blog/page/44

---

## 1. 

**Date:** Posted: Jul 8, 2010  
**Link:** [https://hex-rays.com/blog/running-scripts-from-the-command-line-with-idascript](https://hex-rays.com/blog/running-scripts-from-the-command-line-with-idascript)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Running scripts from the command line with idascript

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/running-scripts-from-the-command-line-with-idascript>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/running-scripts-from-the-command-line-with-idascript>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/running-scripts-from-the-command-line-with-idascript>)

Elias Bachaalany ✦ Posted: Jul 8, 2010

![Running scripts from the command line with idascript](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

In this blog post we are going to demonstrate how the ‘-S’ and ‘-t’ switches (that were introduced in [IDA Pro 5.7](<http://www.hex-rays.com/idapro/57/index.htm>)) can be used to run IDC, Python or other supported scripts from the command line as if they were standlone scripts and how to use the **idascript** utility

![](https://hex-rays.com/hubfs/Imported_Blog_Media/idascript_intro-3.gif)  


## Background

In order to run a script from the command line, IDA Pro needs to know which script to launch. We can specify the script and its argument via the “-S” switch:
    
    
    idag **-Sfirst.idc** mydatabase.idb

Or:
    
    
    idag **-S"first.idc _arg1_ _arg2_ "** mydatabase.idb

In case the script does not require a database (for example, it works with the debugger and attaches to existing processes), then IDA Pro will be satisfied with the "-t" (create a temporary database) switch:
    
    
    idag -S"first.idc _arg1_ _arg2_ " **-t**

Where **first.idc** :
    
    
    #include <idc.idc>
    static main()
    {
      Message("Hello world from IDC!\n");
      return 0;
    }

If we run IDA Pro with the following command:
    
    
    idag -Sfirst.idc -t

We notice two things:

  1. Nothing is printed in the console window: This is because the message will show in the output window instead:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/idascript_first-3.gif)

(It is possible to save all the text in the output window by using the **IDALOG** environment variable.)

  2. IDA Pro remains open and does not close: To exit IDA Pro when the script finishes, use Exit() IDC function. 



In the following section, we will address those two problems with _idascript.idc_ and _idascript.py_ helper scripts and the _idascript_ utility.

## Running scripts from the command line

In order to print to the console window, we will not use IDC / Message() instead we will write to a file and when IDA Pro exits we will display the contents of that file.

Our second attempt with **second.idc** :
    
    
    extern g_idcutil_logfile;
    static LogInit()
    {
      g_idcutil_logfile = fopen("**idaout.txt** ", "w");
      if (g_idcutil_logfile == 0)
        return 0;
      return 1;
    }
    static LogWrite(str)
    {
      if (g_idcutil_logfile != 0)
        return fprintf(g_idcutil_logfile, "%s", str);
      return -1;
    }
    static LogTerm()
    {
      if (g_idcutil_logfile == 0)
        return;
      fclose(g_idcutil_logfile);
      g_idcutil_logfile = 0;
    }
    static main()
    {
      LogInit(); // Open log file
      LogWrite("Hello world from IDC!\n"); // Write to log file
      LogTerm(); // Close log file
      Exit(0); // Exit IDA Pro
    }

Now let us run IDA Pro:
    
    
    idag -Ssecond.idc -t
    

and type afterwards:
    
    
    type idaout.txt

to get the following output:
    
    
    Hello world from IDC!

To simplify this whole process, we wrote a small win32 command line utility called **idascript:**
    
    
    IDAScript 1.0 (c) Hex-Rays - A tool to run IDA Pro scripts from the command line
    It can be used in two modes:
    a) With a database:
    idascript database.idb script.(idc|py|...) [arg1 [arg2 [arg3 [...]]]]
    b) With a temporary database:
    idascript script.(idc|py|...) [arg1 [arg2 [arg3 [...]]]]
    

Since we will be using LogInit(), LogTerm(), LogWrite() and other helper functions over and over, we moved those common functions to **idascript.idc**.

The script **first.idc** can now be rewritten like this:
    
    
    #include <idc.idc>
    #include "idascript.idc"
    static main()
    {
      InitUtils(); // calls LogInit()
      Print(("Hello world from IDC!\n")); // Macro that calls LogWrite()
      Quit(0); // calls LogTerm() following by Exit()
    }
    

As for IDAPython, we wrote a small class to redirect all output to **idaout.txt** :
    
    
    import sys
    class ToFileStdOut(object):
        def __init__(self):
            self.outfile = open("**idaout.txt** ", "w")
        def write(self, text):
            self.outfile.write(text)
        def flush(self):
            self.outfile.flush()
        def isatty(self):
            return False
        def __del__(self):
            self.outfile.close()
    sys.stdout = sys.stderr = ToFileStdOut()
    

Thus, **hello.py** can be written like this:
    
    
    import idc
    **import idascript**
    print "Hello world from IDAPython\n"
    for i in xrange(1, len(**idc.ARGV**)):
        print "ARGV[%d]=%s" % (i, idc.ARGV[i])
    idc.Exit(0)

## Sample scripts

### Process list

The sample script **listprocs.idc** will enumerate all processes and display their ID and name:
    
    
    #include <idc.idc>
    #include <idascript.idc>
    static main()
    {
      InitUtils();
      **LoadDebugger**("win32", 0);
      auto q = **GetProcessQty**(), i;
      for (i=0;i<q;i++)
        Print(("[%08X] %s\n", **GetProcessPid**(i), **GetProcessName**(i)));
      Quit(0);
    }
    

### Kill process

The **killproc.idc** script illustrates how to find processes by name and terminate them one by one:
    
    
    #include <idc.idc>
    #include "idascript.idc"
    #include "procutil.idc"
    static main()
    {
      InitUtils();
      // Load the debugger
      LoadDebugger("win32", 0);
      // Get parameters
      if (**ARGV**.count < 1)
        QuitMsg(0, "Usage: killproc.idc ProcessName\n");
      auto procs = **FindProcessByName**(ARGV[1]), i;
      if (procs.count == 0)
        QuitMsg(-1, "No process(es) with name " + ARGV[1]);
      for (i=procs.count-1;i>=0;i--)
      {
        auto pid = procs[i];
        Print(("killing pid: %X\n", pid));
        **KillProcess**(pid);
      }
      Quit(0);
    }

To test the script, let us suppose we have a few instances of notepad.exe we want to kill:
    
    
    D:\idascript>idascript killproc.idc notepad.exe
    killing pid: 878
    killing pid: 14C8
    D:\idascript>

We used here the “ARGV” variable that contains all the parameters passed to IDA Pro via the -S switch, FindProcessByName() utility function and KillProcess() (check procutil.idc)

The trick behind terminating a process is to attach and call StopDebugger(). The following is an excerpt from **procutil.idc** utility script:
    
    
    static KillProcess(pid)
    {
      if (!AttachToProcess(pid))
        return 0;
      StopDebugger(); // Terminate the current process
      // Normally, we should get a PROCESS_EXIT event
      GetDebuggerEvent(WFNE_SUSP, -1);
    }
    

### Process information

The **procinfo.idc** script will display thread count, register information and the command line arguments of the process in question:
    
    
    #include "idascript.idc"
    #include "procutil.idc"
    static **DumpProcessInfo**()
    {
      // Retrieve command line via Appcall
      Print(("Command line: %s\n", **GetProcessCommandLine**()));
      // Enum modules
      Print(("Module list:\n------------\n"));
      auto x;
      for (x = **GetFirstModule**();x!=BADADDR;x=**GetNextModule**(x))
        Print(("Module [%08X] [%08X] %s\n", x, **GetModuleSize**(x), **GetModuleName**(x)));
      Print(("\nThread list:\n------------\n"));
      for (x=GetThreadQty()-1;x>=0;x--)
      {
        auto tid = GetThreadId(x);
        Print(("Thread [%x]\n", tid));
        SelectThread(tid);
        Print(("  EIP=%08X ESP=%08X EBP=%08X\n", **Eip** , **Esp** , **Ebp**));
      }
    }
    static main()
    {
      InitUtils();
      // Load the debugger
      LoadDebugger("win32", 0);
      // Get parameters
      if (ARGV.count < 2)
        QuitMsg(0, "Usage: killproc.idc ProcessName\n");
      auto procs = FindProcessByName(ARGV[1]), i;
      for (i=procs.count-1;i>=0;i--)
      {
        auto pid = procs[i];
        if (!AttachToProcess(pid))
        {
          Print(("Could not attach to pid=%x\n", pid));
          continue;
        }
        DumpProcessInfo();
        DetachFromProcess();
      }
      Quit(0);
    }

The function **GetProcessCommandLine** is implemented (using [Appcall](</blog/practical-appcall-examples-1>)) like this:
    
    
    static GetProcessCommandLine()
    {
      // Get address of the GetCommandLine API
      auto e, **GetCmdLn** = LocByName("kernel32_GetCommandLineA");
      if (GetCmdLn == BADADDR)
        return 0;
      // Set its prototype for Appcall
      SetType(GetCmdLn, "char * __stdcall x();");
      try
      {
        // Retrieve the command line using Appcall
        return **GetCmdLn()** ;
      }
      catch (e)
      {
        return 0;
      }
    }
    

### Extracting function body

So far we did not really need a specific database to work with. In the following example (**funcextract.idc**) we will demonstrate how to extract the body of a function from a given database:
    
    
    #include <idc.idc>
    #include "idascript.idc"
    static main()
    {
      InitUtils();
      if (**ARGV.count** < 2)
        QuitMsg(0, "Usage: funcextract.idc FuncName OutFile");
      // Resolve name
      auto ea = **LocByName**(ARGV[1]);
      if (ea == BADADDR)
        QuitMsg(0, sprintf("Function '%s' not found!", ARGV[1]));
      // Get function start
      ea = **GetFunctionAttr**(ea, **FUNCATTR_START**);
      if (ea == BADADDR)
        QuitMsg(0, "Could not determine function start!\n");
      // size = end - start
      auto sz = **GetFunctionAttr**(ea, **FUNCATTR_END**) - ea;
      auto fp = fopen(ARGV[2], "wb");
      if (fp == 0)
        QuitMsg(-1, "Failed to create output file\n");
      **savefile**(fp, 0, ea, sz);
      fclose(fp);
      Print(("Successfully extracted %d byte(s) from '%s'", sz, ARGV[1]));
      Quit(0);
    }
    

To test the script, we use idascript utility and pass a database name:
    
    
    D:\idascript>**idascript** _ar.idb_ funcextract.idc start start.bin
    Successfully extracted 89 byte(s) from 'start'
    D:\idascript>

### Other ideas

There are other ideas that can be implemented to create useful command line tools:

  * Process memory read/write: Check the **rwproc.idc** script that allows you to read from the process memory to a file or the other way round. 
  * Associate .IDC with idascript.exe: This allows you to double-click on IDC scripts to run them from the Windows Explorer 
  * Scriptable debugger: Write scripts to debug a certain process and extract needed information 
  * … 



## Installing the idascript utility

Please download idascript and the needed scripts from [here](</ida_pro/files/idascript_files.zip>) and follow these steps:

  1. Copy idascript.exe to the installation directory of IDA Pro (say %IDA%) 
  2. Add IDA Pro directory to the PATH environment variable 
  3. Copy idascript.idc and procutil.idc to %IDA%\idc 
  4. Copy idascript.py to %IDA%\python 
  5. Optional: Associate *.idc files with idascript.exe 



Comments and suggestions are welcome!

---

## 2. 

**Date:** Posted: Jul 2, 2010  
**Link:** [https://hex-rays.com/blog/ida-pro-5-7-highlights](https://hex-rays.com/blog/ida-pro-5-7-highlights)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 5.7 highlights

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-5-7-highlights>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-5-7-highlights>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-5-7-highlights>)

Elias Bachaalany ✦ Posted: Jul 2, 2010

![IDA Pro 5.7 highlights](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We have released a IDA Pro 5.7 few days ago. The complete whatsnew can be found [here](<http://www.hex-rays.com/idapro/57/index.htm>).  
In this blog post we will highlight some of the major changes and additions of this release.  
  


## Debuggers

Among the various changes and additions to the debugger kernel and modules, we:

  * added support for MMX/XMM registers: 

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_dbg_xmm-4.gif)

  * added more actions to the modules window: 

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_modcmenu-4.gif)

    * Load debug symbols: Load additional PDB symbols 
    * Jump to module base: Jumps to the module base in the current view 
    * Analyze module: Converts the module segments to non-debugger segments and analyzes the module. Handy when analyzing crashdump files 

  * added Bochs 2.4.2 support.  
Bochs 2.4.2 introduced range read/write physical watchpoints. If a watchpoint was added from the Bochs command line interface IDA Pro will suspend the execution when the [watchpoint](<http://bochs.sourceforge.net/doc/docbook/user/internal-debugger.html#AEN3071>) triggers. 



### Bochs Linux debugger plugin

If you found Bochs debugger plugin useful in the past (e.g. for [low level programming](</blog/develop-your-master-boot-recor>), [malware](</blog/bochs-plugin-goes-alpha>) and code snippet emulation), then you may take advantage of the same functionality under Linux / MacOS.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_bochs_select-4.gif)  
(Debugger selection)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_bochs_pemode-4.gif)  
(Debugger configuration)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_bochs_running-4.gif)  
(Debugger running Under Ubuntu 9 x86)

Please refer to the [tutorial](<http://www.hex-rays.com/idapro/debugger/bochs_linux_primer.pdf>) to learn more how to configure and use the plugin.  


### WinDbg debugger

Apart from bug fixes and minor speed improvements, we added [non-invasive](<http://msdn.microsoft.com/en-us/library/ff552274\(VS.85\).aspx>) debugging support. This ability to attach to processes that are already being debugged comes handy when you want to create crashdumps or inspect handles and other kernel objects.

Make sure you enable this option from the Debugger/Debugger Options/Specific debugger options dialog:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_windbg_noninvasive-4.gif)

If you are debugging 64-bit applications using idag64, the Windbg plugin will offer to run the debugger server for you automatically:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_windbg_serverlaunch-4.gif)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_windbg_serverlaunched-4.gif)

When the debugger server is no longer needed make sure to terminate it.

## Scripting

### Processor modules and Plugins

It is now possible to write scriptable [loaders](</blog/pdf-file-loader-to-extract-and-1>), [processor modules](</blog/scriptable-processor-modules>) and [plugins](</blog/scriptable-plugins>). If you always wanted your scripts to automatically execute when a database is loaded and unload/deinitialize when the database is closed, then turn your script into a plugin script with just a few additional lines of code.

If we get enough requests about writing debugger modules using scripts, we may add this facility in the future.

### IDAPython improvements

We refactored and improved the [IDAPython](<http://code.google.com/p/idapython/>) (now version 1.4.0) plugin (and the _extlang_t_ interface by adding new facilities to call object methods, query properties and so on).  
This has lead to significant speed gains as demonstrated by [Ero Carrera’s](<http://blog.zynamics.com/2010/06/29/ida2sql-exporting-ida-databases-to-mysql/>) blog post.

We also [documented](<http://www.hex-rays.com/idapro/idapython_docs/>) all the manually wrapped functions and utility classes which were poorly documented with the example scripts.

Please refer to the documentation of the pseudo module pywraps for more information.

## The graphical user interface

We did some last minute changes to the GUI and some of the features described [before](</blog/ui-and-scripting-improvements>) were changed:

  * The recent scripts window can be configured to be a dockable window or a modal dialog (check idagui.cfg / RECENT_SCRIPTS_MODAL) 
  * No need to hold the Alt key in order to jump to identifiers, instead simply double click on it 
  * Output window is now searchable: use Alt-T to start the search and Ctrl-T to search for the next match 



## Kernel and processor modules

### ARM module

We have added support for almost all ARMv7 instructions, including NEON (aka Advanced SIMD). NEON instructions can be found in the code made for Cortex-A8 processors, such as the one in iPhone 3GS and iPad.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_arm_neon-4.gif)  
Because ARM uses new, unified syntax for NEON and VFP (Vector Floating Point) instructions in ARMv7, we use the new syntax if NEON is enabled.  
Otherwise we still display old mnemonics for VFP instructions, as they’re what most people are used to.  
The only instructions still missing from ARMv7 are ThumbEE instructions which are supposed to be used for JIT compilation of bytecode-based languages. We have not yet encountered any real-life code using it.

You can choose which architecture version to use when disassembling ARM code. This can be done interactively in the “Processor-specific options dialog” :

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_arm_archopts-4.gif)  
via the command-line:
    
    
    idag -parm:ARMv6T2 firmware.bin

or by editing IDA.CFG:
    
    
    ARM_DEFAULT_ARCHITECTURE = "ARMv6";

For ARM Mach-O files or ELF files that include EABI attributes, the architecture version is set automatically from the flags in the file.  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_arm_flags_elf-4.gif)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_arm_flags_macho-4.gif)

### MIPS module

We have improved the register tracing and now almost all indirect code and data references are recognized. Here’s one of the many samples:  
Before:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_mips_before-4.gif)  
After:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_mips_after-4.gif)

We have also added decoding of the MIPS16e instructions jrc, jalrc, save, restore etc.).  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_mips_mips16e-4.gif)

### PC module

One small but important new feature is the improvement in the parsing of SEH (Structured Exception Handling) in Win32 files.  
It is especially useful when disassembling drivers which use SEH extensively.  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_pc_seh-4.gif)  
Notice that the finally handler is not converted into a separate function as before (because of the call), but is correctly added to the main function.

### Python processor modules

We added two new processor module scripts written entirely in Python. They can be used as a template when developing your own.

  * **ebc.py** : EFI Byte code processor module: 

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_ebc-4.gif)

  * **msp430.py** : MSP430 is a simple 27-instructions 16-bit RISC processor from TI. 

![](https://hex-rays.com/hubfs/Imported_Blog_Media/rel57_msp430-4.gif)




## Closing words

We hope that the new features make your reversing job more easier. Please feel free to send us comments, suggestions and feature requests.

Last but not least, we expect to start the beta testing of the new [IDA Qt](</blog/preview-of-the-next-generation>) interface soon. If you are interested and have an active IDA Pro license do not hesitate to [contact](<mailto:support@hex-rays.com?subject=IDA%20Qt%20-%20Beta%20%20request>) us.

---

## 3. 

**Date:** Posted: Jun 23, 2010  
**Link:** [https://hex-rays.com/blog/extending-idc-and-idapython](https://hex-rays.com/blog/extending-idc-and-idapython)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Extending IDC and IDAPython

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/extending-idc-and-idapython>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/extending-idc-and-idapython>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/extending-idc-and-idapython>)

Elias Bachaalany ✦ Posted: Jun 23, 2010

![Extending IDC and IDAPython](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Scripting with IDA Pro is very useful to automate tasks, write scripts or do batch analysis, nonetheless one problem is commonly faced by script writers: the lack of a certain function from the scripting language. In the blog post going to demonstrate how to extend both IDC and IDAPython to add new functions. 

## Extending IDC

Every IDC variable is represented by an **idc_value_t** object in the SDK. The IDC variable type is stored in the **idc_value- >vtype** field and have one of the following values:

  * VT_LONG: Used to store 32bit signed numbers (or 64bit numbers if __EA64__ is set). Use **idc_value- >set_long()** to set a value and **idc_value- >num** to read it.
  * VT_STR2: Used to store arbitrary strings. Use **idc_value- >set_string()** to set a value, **idc_value- >c_str()** to get a C string or **idc_value- >qstr()** to get a qstring instance.
  * VT_INT64: Used to store 64bit signed numbers. Use **idc_value- >i64** to read the value and idc_value->set_int64() to set a value
  * VT_OBJ: Used to represent objects. Such idc variable is created via the **VarObject()** and attributes are managed by **VarSetAttr()** / **VarGetAttr()**
  * VT_PVOID: Use to store arbitrary untyped values. Use idc_value->pvoid to read this value and idc_value->set_pvoid() to set it



Now that idc_value_t is covered, let us explain how to add a new IDC function in two steps:

  1. Writing the IDC function callback
  2. Registering the function



### Writing the callback

The callback is defined as:
    
    
    typedef error_t idaapi idc_func_t(idc_value_t *argv,idc_value_t *r);

The **argv** array is initialized by the IDC interpreter and contains the passed arguments and the **r** argument is set by the callback to return values to the IDC interpreter.

### Registering an IDC function

The IDC function callback can be registered with the IDC interpreter using this function:
    
    
    bool set_idc_func(const char *name, idc_func_t *fp, const char *args);

Where:

  * name: designates the IDC function name to be added
  * fp: IDC function callback (written in the previous step)
  * args: A zero terminated character array containing the expected arguments types (VT_xxx). For example, an IDC function that expects two numbers and one string as arguments have the following args value: _{VT_LONG, VT_LONG, VT_STR2, 0}_



When the plugin is about to unload (in its term() callback), it should unregister the IDC function by calling **set_idc_func(name, NULL, NULL)**

### Example

For the sake of demonstration, let us add getenv() to IDC:
    
    
    sstatic const char idc_getenv_args[] = { VT_STR2, 0 };
    static const char idc_getenv_name[] = "getenv";
    static error_t idaapi idc_getenv(idc_value_t *argv, idc_value_t *res)
    {
      char *env = getenv(argv[0].c_str());
      res->set_string(env == NULL ? "" : env);
      return eOk;
    }
    int idaapi init()
    {
      set_idc_func(idc_getenv_name, idc_getenv, idc_getenv_args );
      return PLUGIN_KEEP;
    }
    void idaapi term(void)
    {
      // Unregister
      set_idc_func(idc_getenv_name, NULL, NULL);
    }
    

## Extending IDAPython

It is possible to extend IDAPython in two ways:

  * Modifying the [source code](<http://code.google.com/p/idapython/>) of IDAPython
  * Writing a plugin to extend Python scripting



While the former method requires basic knowledge of [SWIG](<http://www.swig.org/>), both methods require some understanding of the [Python C API](<http://docs.python.org/c-api/>). In this article, we will use the second method because it is more practical and does not require modification to IDAPython itself. This process involes three steps:

  1. Initializing the Python C API
  2. Writing the callback(s)
  3. Registering the function(s)



If you’re new to the Python C API, you could still follow and understand the code used in this blog post without refering to this [tutorial](<http://docs.python.org/extending/>).

### Writing the callback

Let us wrap the following function:
    
    
    // ints.hpp
    idaman ssize_t ida_export get_predef_insn_cmt(
            const insn_t &cmd,
            char *buf,
            size_t bufsize);
    

which is used to retrieve the predifined comment of a given instruction. We want to expose it as the following Python function:
    
    
    def get_predef_insn_cmt(ea):
        """Return instruction comments
        @param ea: Instruction ea
        @return: None on failure or a string containing the instruction comment
        """
        pass
    

Here is the callback:
    
    
    static PyObject *py_getpredef_insn_cmt(PyObject * /*self*/, PyObject *args)
    {
      do
      {
        // Parse arguments
        unsigned long ea;
        if (**!PyArg_ParseTuple**(args, "k", &ea))
          break;
        // Decode instruction
        if (decode_insn(get_screen_ea()) == 0)
          break;
        // Get comments
        char buf[MAXSTR], *p = buf;
        p += qsnprintf(buf, sizeof(buf), "%s: ", ph.instruc[cmd.itype].name);
        if (get_predef_insn_cmt(cmd, p, sizeof(buf) - (p - buf)) == -1 || *p == '\0')
          break;
        // Return comment as a string
        return **PyString_FromString**(buf);
      } while (false);
      Py_RETURN_NONE;
    }
    

  * PyArg_ParseTuple() is used to parse the arguments. This function can be likened to sscanf(). For a description about the format string please refer to the [Py_BuildValue()](<http://docs.python.org/c-api/arg.html#Py_BuildValue>) documentation
  * get_predef_insn_cmt() is used to fetch the comment and PyString_FromString() is used to return a string Python object
  * In case of failures we use the Py_RETURN_NONE macro to return the Py_NONE value (None in Python)



### Registering the callback(s)

Unlike IDC callback registration, Python C API allows you to register more than a function at once. This is done by describing all the callbacks in a **PyMethodDef** array:
    
    
    static PyMethodDef py_methods[] =
    {
      {"**getpredef_insn_cmt** ",  py_getpredef_insn_cmt, METH_VARARGS, ""},
      {NULL, NULL, 0, NULL}
    };
    

and then calling the registration function:
    
    
    Py_InitModule("idapyext", py_methods);

Finally, to use this function, make sure you “ _import idapyext_ ” before calling _getpredef_insn_cmt_ The source code used in the blog post can be downloaded from [here](</ida_pro/files/idcpyext.zip>)  
UPD 2014-04-24: please read <http://www.hexblog.com/?p=788> for important changes in IDA v6.5

---

## 4. 

**Date:** Posted: May 26, 2010  
**Link:** [https://hex-rays.com/blog/ui-and-scripting-improvements](https://hex-rays.com/blog/ui-and-scripting-improvements)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# UI and scripting improvements

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ui-and-scripting-improvements>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ui-and-scripting-improvements>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ui-and-scripting-improvements>)

Elias Bachaalany ✦ Posted: May 26, 2010

![UI and scripting improvements](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

In addition to the [previously](</blog/scriptable-plugins>)  
[covered](</blog/scriptable-processor-modules>) features  
we’ve already added, we took the opportunity to get to the bottom of it and add even more scripting facilities where possible along with minor but convenient UI enhancements.  
In this blog entry, we will introduce some of the new features in the coming version of IDA Pro.

## Output window

We received many requests from our clients to make the output window a text control so users select portions of text instead of a whole line.

The output window is now a rich text control instead of a listbox, allowing the users to:

  * Select or delete portions of text 
  * Jump to any address from the output by Alt-clicking on it 



![](https://hex-rays.com/hubfs/Imported_Blog_Media/uiadd_select_out-3.gif)  
The Alt-click will work not just for numerical addresses but also for identifiers which are location names in the database or even registers during debugging.

## Unified script facilities in the UI

We have replaced the “File / IDC file” menu with “File / Script file”, allowing the user a unified way to run any supported script file.  
IDA Pro will match the file’s extension with a registered extlang (or the built-in IDC engine or Python plugin) and run the file with it.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/uiadd_script_menu-3.gif)

## Added “Recent scripts” window

We have dropped the “Recent IDC Scripts” toolbar and replaced it with a script list (similar to IDAPython’s **Alt-7**).  
This dialog is common to the text and the graphical interfaces of IDA Pro, and will remember any script (not just IDC). It is accessible via the Windows menu or the **Alt-F7** hotkey:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/uiadd_rscript_menu-3.gif)

Recent scripts will be displayed in a chooser:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/uiadd_rscript_win-3.gif)  
(Note that the history has both Python and IDC scripts)

The standard chooser hotkeys can be used: **Ctrl-E** to edit the script, **Ins** to browse for a new script, **Del** to remove it and **Alt-T** to search.

## Added new ‘-t’ command line switch

This switch will start IDA Pro with a temporary database. This is useful if you want to run scripts that do not neccesarily need a specific input file.  
For example, a script can use the debugger to attach to a process, gather some information and detach.

In the future blog entries we will show how to use this in real life scenarios.

## Improved the ‘-S’ command line switch

The -S switch can now run not just IDC but any supported script file (again, based on the script file extension).  
It now supports passing arguments to the script:
    
    
    idag -t -S"hello.idc arg1 arg2 \"arg 3\" arg4"

With **hello.idc** :
    
    
    #include <idc.idc>
    static main()
    {
      auto i;
      for (i=0;i<ARGV.count;i++)
        Message("ARG[%d]=%s\n", i, ARGV[i]);
    }
    

After IDA Pro runs, such an output will be displayed:
    
    
    ARG[0]=hello.idc
    ARG[1]=arg1
    ARG[2]=arg2
    ARG[3]=arg 3
    ARG[4]=arg4

In case you’re wondering, the same can be accomplished with IDAPython:
    
    
    import idc
    for i in xrange(0, len(idc.ARGV)):
    print "ARG[%d]=%s" % (i, idc.ARGV[i])
    

The ARGV array is stored in the **idc** module.

## Auto-indent in the script input box

A small but often irritating pecularity of IDA’s script boxes (**Shift-F2** for IDC and **Alt-8** for Python) is that it they don’t auto-indent.  
This is particularly annoying for Python which requires proper indentation.  
In the new version we have implemented auto-indent, so writing Python script snippets is not an excercise in frustration anymore.

We hope that these new features, while minor, will make your work more productive.

---

## 5. 

**Date:** Posted: May 12, 2010  
**Link:** [https://hex-rays.com/blog/arm-decompiler-beta-is-coming](https://hex-rays.com/blog/arm-decompiler-beta-is-coming)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# ARM decompiler beta is coming

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/arm-decompiler-beta-is-coming>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/arm-decompiler-beta-is-coming>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/arm-decompiler-beta-is-coming>)

Ilfak Guilfanov ✦ Posted: May 12, 2010

![ARM decompiler beta is coming](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

We have the beta version of the ARM decompiler almost ready! Below is a short demo of how it works now:

Your browser does not support the video element. Kindly update it to latest version. 

If you are interested in participating in the beta testing and you have an active x86 decompiler license, please send us a message. Thanks!

---

## 6. 

**Date:** Posted: Apr 30, 2010  
**Link:** [https://hex-rays.com/blog/kernel-debugging-with-ida-pro-windbg-plugin-and-virtualkd](https://hex-rays.com/blog/kernel-debugging-with-ida-pro-windbg-plugin-and-virtualkd)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Kernel debugging with IDA Pro / Windbg plugin and VirtualKd

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/kernel-debugging-with-ida-pro-windbg-plugin-and-virtualkd>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/kernel-debugging-with-ida-pro-windbg-plugin-and-virtualkd>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/kernel-debugging-with-ida-pro-windbg-plugin-and-virtualkd>)

Elias Bachaalany ✦ Posted: Apr 30, 2010

![Kernel debugging with IDA Pro / Windbg plugin and VirtualKd](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

The other day we received an email support question asking if IDA Pro / Windbg debugger plugin works with [VirtualKd](<http://virtualkd.sysprogs.org/>), a tool _that allows speeding up (up to 45x) Windows kernel module debugging using VMWare and VirtualBox virtual machines_. After we installed and experimented with VirtualKd, our answer was “yes, certainly”. This blog entry aims at illustrating how to configure VirtualKd to be used with IDA Pro / Windbg plugin and VMWare.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_cover-3.gif)  


## Installing VirtualKd

First download the VirtualKd [package](<http://virtualkd.sysprogs.org/#installation>), unzip its contents and copy the contents of the ‘target’ folder to the VMWare guest OS and run ‘vminstall.exe’. After this step, reboot the guest and run the ‘vmmon.exe’ (or ‘vmmon64.exe’) inside the host. If everything is successful, you should see something like this:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_vmon-3.png)

Please take note of the ‘Pipe name’ value as shown in the screenshot. We will use this value when configuring the Windbg plugin.

## Configuring IDA Pro / Windbg plugin

First, run IDA Pro without an input database and select Debugger / Attach / Windbg plugin:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_attach-3.png)

Next, we get this screen:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_connstr-3.png)

Here we enter a connection string (designating that the com port is a pipe). The pipe name is the same value we read from the ‘vmmon’ tool from the above steps.

Before pressing OK, we need to make sure that the Windbg plugin is properly configured. Let us verify that by pressing on the ‘Debug options’ button:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_dbgsetup-3.png)

This dialog is used to configure the debugger in general, it is common to all debugger modules. Since we don’t need to change any of those settings now, let us instead select ‘Set specific options’:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_specopt-3.png)

Make sure that these two options are properly configured:

  1. Kernel debugging is selected 
  2. Debugging tools path is correctly entered. Please note that even if you’re debugging an x64 kernel you still need to point to the x86 version of the debugging tools. 



After we configured everything, simply press OK all the way until the attach dialog shows up:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_attaching-3.png)

Pressing OK one last time will start the debugging session:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/kd_dbg-3.gif)

After debugging for a while, we were really amazed at the speed improvement achieved with the help of VirtualKd (nice work VirtualKd team). Kernel debugging speed using the Windbg plugin is almost comparable to the local win32 debugger plugin.

For a more elaborate tutorial on configuring and using the Windbg plugin please check our [support](<http://www.hex-rays.com/idapro/idasupport.htm>) page.

---

## 7. 

**Date:** Posted: Apr 28, 2010  
**Link:** [https://hex-rays.com/blog/book-review-the-art-of-assembly-language-2nd-edition](https://hex-rays.com/blog/book-review-the-art-of-assembly-language-2nd-edition)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Book Review: The Art of Assembly Language, 2nd Edition

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/book-review-the-art-of-assembly-language-2nd-edition>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/book-review-the-art-of-assembly-language-2nd-edition>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/book-review-the-art-of-assembly-language-2nd-edition>)

Elias Bachaalany ✦ Posted: Apr 28, 2010

![Book Review: The Art of Assembly Language, 2nd Edition](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Have you ever tried to teach x86 assembly language programming to someone coming from high level language programming background and discovered that it was hard?

Before being able to write a simple “Hello World” program one needs to know a fair deal about the x86 architecture, the assembler language and the operating system. Obviously this is not the case with high level languages such as C for example.

I was reading [The Art of Asssembly Language, 2nd edition](<http://nostarch.com/assembly2.htm>) book by Randall Hyde the other day and really enjoyed his approach to teaching the assembly language programming.  
  
In his book, Randall introduces the reader to the HLA (High Level Assembler) compiler which will be used as a tool to learn the x86 assembly language.

The syntax of HLA can be thought of as a hybrid of Pascal and assembly language. Here’s a sample “Hello world” HLA program:
    
    
    program helloWorld;
    #include( "stdlib.hhf" );
    begin helloWorld;
        stdout.put( "Hello, World of Assembly Language", nl );
    end helloWorld;
    

You may argue that this was not an assembly language program, but what about this:
    
    
    program h;
    #include("stdlib.hhf");
    static
        s: string := "Hello World!";
    procedure chksum_str; @returns( "eax" );
    begin chksum_str;
        mov(s, edi);
        xor(ebx, ebx);
        xor(eax, eax);
        while( mov( [edi], al ) <> #0 ) do
            inc(edi); // advance str ptr
            add(eax, ebx);
        endwhile;
        mov(ebx, eax);
    end chksum_str;
    begin h;
      stdout.put("The checksum of '", s, " is: 0x");
      chksum_str();
      stdout.put(eax);
      if ( eax == 1234 ) then
        stdout.put("Special chksum!", nl);
      endif;
    end h;
    

Although this is also not pure assembler syntax, the newcomer will enjoy learning about the x86 architecture and instruction set with the help of the features provided by HLA:

  * Ability to create user types 
  * Exception handling 
  * Control and repetition structures (and other constructs found in high level languages) 
  * Classes and objects 
  * Various libaries: stl, file i/o, os, array manipulation, math, etc… 
  * etc… 



Throughout the book, before a programming concept is introduced, Randall talks about the necessary background information (architecture and instruction set) and then explains how to put the concept into practive using HLA.

For example, in Chapter 5 (Procedures and Units), he explains in detail how the stack is set up during a procedure call, how local variables are allocated and how to access the arguments, followed by explanation on how to use HLA to write procedures. Similarly in Chapter 6, when talking about FPU arithmetics, the author carefully explains about the FPU data and controls registers, related instruction set and finally how to use HLA to do FPU arithmetics.

If your aim is to learn assembly language in order to start reverse engineering, this book is probably not for you, however I highly recommend this book for programmers:

  * That always wanted to learn the x86 assembly language but found it difficult to start with. This book is well organized and easy to read 
  * That want to teach the basics of the x86 architecture and assembly language 
  * That want to put together an x86 assembly program quickly. HLA compiler and the built-in libraries give you the convenience of a high-level programming language and the power of a low level language. If you use the standard libraries provided by HLA then your program is portable

---

## 8. 

**Date:** Posted: Mar 29, 2010  
**Link:** [https://hex-rays.com/blog/scriptable-plugins](https://hex-rays.com/blog/scriptable-plugins)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Scriptable plugins

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/scriptable-plugins>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/scriptable-plugins>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/scriptable-plugins>)

Elias Bachaalany ✦ Posted: Mar 29, 2010

![Scriptable plugins](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In IDA Pro 5.6 we added support for [loader scripts](</blog/pdf-file-loader-to-extract-and-analyse-shellcode/>), last month we added [processor module scripts](</blog/scriptable-processor-modules>) support, and now by adding support for scriptable plugins (for the next version of IDA) it will be possible to write all sort of IDA Pro extensions using scripting languages.  


![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/plgscr_idc-3.gif?width=525&height=511&name=plgscr_idc-3.gif)  
(A plugin script written using IDC)

## Background

Apart from writing scripts, writing plugin modules using the SDK is the second easiest extension to code for IDA Pro. In short, plugins are DLLs that export the **PLUGIN** symbol which is an instance of **plugin_t** (check loader.hpp):

  * init/run/term callbacks: These callbacks are called when IDA initializes, invokes and terminates the plugin. 
  * flags: The plugin flags describe the kind of the plugin. A plugin could have no flags (flags = 0) which means that this is an ordinary plugin. Or the plugin could act as a processor module extension, thus the **PLUGIN_PROC** flag. Another type of plugins are the debugger plugins which use the **PLUGIN_DBG** flag. 
  * Description and hotkey: The remaining _plugin_t_ entries are used to designate a name, help text, comment, and a hotkey for the plugin. 



The **init** callback is called when the plugin is loaded by IDA. It decides whether to load the plugin or skip it by returning one of the following constants:

  * PLUGIN_OK: Plugin agrees to work with the current database but will be loaded only when it is about to be invoked. 
  * PLUGIN_SKIP: Plugin does not want to work with the current database. For example a plugin may choose to work with certain input file types. 
  * PLUGIN_KEEP: Plugin agrees to work with the current database and will remain loaded until the database is closed. It is important to return this value if you hook to notification events or register any sort of callbacks. 



A plugin can be invoked using its hotkey or programmatically using the RunPlugin() (in IDC) or run_plugin() (in the SDK / loader.hpp). Upon invocation, the **run** callback:
    
    
    int idaapi run(int arg)

is executed. Before explaining the use of the _arg_ argument, it is important to know that plugins can also be configured in the **plugins.cfg** file, which has the following format:
    
    
    ; plugin_name     filename     hotkey  arg  [flags]
    Extract_File      extract      0       1
    Extract_Block     extract      0       2
    Extract_Item      extract      Alt-F8  3
    

(In this example, the ‘extract’ plugin provide at least 3 different functionalities depending on the passed argument value)

If a plugin is not present in plugins.cfg and is invoked using the “Edit/Plugins” menu or with the hotkey described in its _PLUGIN_ entry then _run()_ will be invoked with _arg=0_. Configured plugins, when invoked, will pass to the _run()_ the argument value specified in the configuration file.

## Scriptable plugins

Scriptable plugins behave exactly like a native plugin (written using the SDK). To write a plugin using a scripting language, declare a function called **PLUGIN_ENTRY** that returns a _plugin_t_ instance (or an object containing all attributes of a plugin_t object). This is an example script written in IDAPython:
    
    
    import idaapi
    class myplugin_t(**idaapi.plugin_t**):
        flags = idaapi.**PLUGIN_UNL**
        comment = "This is a comment"
        help = "This is help"
        wanted_name = "My Python plugin"
        wanted_hotkey = "Alt-F8"
        def **init**(self):
            idaapi.msg("init() called!\n")
            return idaapi.PLUGIN_OK
        def **run**(self, arg):
            idaapi.msg("run() called with %d!\n" % arg)
        def **term**(self):
            idaapi.msg("term() called!\n")
    def **PLUGIN_ENTRY**():
        return myplugin_t()
    

In this example, the plugin flag _PLUGIN_UNL_ instructs IDA to directly unload the plugin after it has been invoked. This flag is very useful if your plugin script does a specific task and finishes. On the other hand, if your script works with forms (choosers, custom viewers, etc.), or registers callbacks (hotkeys, event notifications, etc.) then this flag should not be used and _init()_ should return _PLUGIN_KEEP_ (as discussed earlier) instead of _PLUGIN_OK_.

Scriptable plugins are so easy to write, and deploy (a simply copy/paste in the plugins folder) and very useful in case you want your plugin to load and unload automatically like native plugins.

---

## 9. 

**Date:** Posted: Mar 25, 2010  
**Link:** [https://hex-rays.com/blog/using-custom-viewers-from-idapython](https://hex-rays.com/blog/using-custom-viewers-from-idapython)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Using custom viewers from IDAPython

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/using-custom-viewers-from-idapython>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/using-custom-viewers-from-idapython>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/using-custom-viewers-from-idapython>)

Elias Bachaalany ✦ Posted: Mar 25, 2010

![Using custom viewers from IDAPython](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Custom viewers can be used to display arbitrary textual information and can be used in any IDA [plugin](</blog/very-simple-custom-viewer>).They are used in IDA-View, Hex-View, Enum and struct views and the Hex-Rays decompiler.  


In this blog entry we are going to write an ASM file viewer in order to demonstrate how to create a custom viewer and populate it with colored lines.  
![asmview.gif](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/asmview-3.gif?width=575&height=525&name=asmview-3.gif)  


## Writing a custom viewer

The simplest custom viewer which does not handle any events (like key presses, mouse or cursor position movements, displaying hints, etc) can be created like this:
    
    
    v = idaapi.simplecustviewer_t()
    if v.Create("Simple custom viewer"):
        for i in xrange(1, 11):
            v.AddLine("Line %d" % i)
        v.Show()
    else:
        print "Failed to create viewer"
    

If handling events is required then one has to derive from **idaapi.simplecustviewer_t** class and implement the required callbacks:
    
    
    class mycv_t(**simplecustviewer_t**):
        def Create(self, sn=None):
            # Form the title
            title = "Simple custom view test"
            if sn:
                title += " %d" % sn
            # Create the customview
            if not simplecustviewer_t.Create(self, title):
                return False
            self.menu_hello = self.AddPopupMenu("Hello")
            self.menu_world = self.AddPopupMenu("World")
            for i in xrange(0, 100):
                self.AddLine("Line %d" % i)
            return True
        def OnKeydown(self, vkey, shift):
            # ESCAPE?
            if vkey == 27:
                self.Close()
            # Goto?
            elif vkey == ord('G'):
                n = self.GetLineNo()
                if n is not None:
                    v = idc.AskLong(self.GetLineNo(), "Where to go?")
                    if v:
                        self.Jump(v, 0, 5)
            elif vkey == ord('R'):
                print "refreshing...."
                self.Refresh()
            else:
                return False
            return True
        def OnPopupMenu(self, menu_id):
            if menu_id == self.menu_hello:
                print "Hello"
            elif menu_id == self.menu_world:
                print "World"
            else:
                # Unhandled
                return False
            return True
    

Or many custom viewers:
    
    
    view = mycv_t()
    if view.Create(1):
        view.Show()
    

Or many custom viewers:
    
    
    def make_many(n):
        L = []
        for i in xrange(1, n+1):
            v = mycv_t()
            if not v.Create(i):
                break
            v.Show()
            L.append(v)
        return L
    # Create 20 views
    V = make_many(20)
    

Please note that no two views should have the same title. To check if a window with a given title exists and then to close it, you can use:
    
    
    f = idaapi.find_tform("Simple custom view test 2")
    if f:
        idaapi.close_tform(f, 0)

For a more comprehensive example on custom viewers, please check the ex_custview.py example.

## Using colored lines

To use colored lines, we have to embed color tags to them. All the available foreground colors are defined in **lines.hpp** header file. The color codes are related to various item kinds in IDA, for example here are some colors:

Color name | Description  
---|---  
SCOLOR_REGCMT | Regular comment  
SCOLOR_RPTCMT | Repeatable comment  
SCOLOR_INSN | Instruction  
SCOLOR_KEYWORD | Keywords  
SCOLOR_STRING | String constant in instruction  
  
There are also special color tags treated as escape sequence codes (the concept is similar to [ANSI escape codes](<http://en.wikipedia.org/wiki/ANSI_escape_code>)). They are used to determine how a line is rendered, to mark the beginning/end of a certain color, to insert address marks, or to mark UTF8 string beginnings/endings:

Color name | Description  
---|---  
SCOLOR_ON | Escape character (ON)  
SCOLOR_OFF | Escape character (OFF)  
SCOLOR_INV | Escape character (Inverse colors)  
SCOLOR_UTF8 | Following text is UTF-8 encoded  
SCOLOR_STRING | String constant in instruction  
  
In the IDA SDK, a colored line is explained to have the following structure:
    
    
    //      A typical color sequence looks like this:
    //
    //      COLOR_ON COLOR_xxx text COLOR_OFF COLOR_xxx
    

Luckily, we don’t have to form the colored lines manually, instead we can use helper functions:
    
    
    colored_line = idaapi.COLSTR("Hello", idaapi.SCOLOR_REG) + " " + idaapi.COLSTR("World", idaapi.SCOLOR_STRING)

If we look at colored_line contents we can see the following:
    
    
    '\x01!Hello\x02! \x01\x0bWorld\x02\x0b'

Which is interpreted as:
    
    
    COLOR_ON SCOLOR_REG=0x21 Hello COLOR_OFF COLOR=0x21 SPACE COLOR_ON SCOLOR_STRING=0x0B World COLOR_OFF COLOR=0x0B

In order to strip back color tags from a colored line, use **tag_remove()** :
    
    
    line = idaapi.tag_remove(colored_line)

## Writing an ASM file viewer

Now that we covered all the needed information, let us write a very basic assembly file viewer. To accomplish the task, we need two things:

  1. ASM tokenizer: It should be able to recognize comments, strings and identifiers. For the identifiers, we will take into consideration only register names, instruction names and directives. 
     * Instruction names: To get all the instruction names we use **idautils.GetInstructionList()** which returns all the instruction names from the processor module (the ph.instruc array) 
     * Register names: Similarly we can use **idautils.GetRegisterList()**
  2. Custom viewer to render the text: We derive from **simplecustviewer_t** to handle key presses and popup menu actions 



The tokenizer (**asm_colorizer_t** class) will go over the text and when it identifies a token it will call one of the following functions: as_string(), as_comment(), as_num(), and as_id(). Those functions will use **idaapi.COLSTR()** to colorize the token appropriately. At the end of each line, the tokenizer will call the add_line() method to add the line (after it has been colored).

The custom viewer (implemented by the **asmview_t** class) will inherit from both asm_colorizer_t and simplecustviewer_t:
    
    
    class asmview_t(**idaapi.simplecustview_t** , **asm_colorizer_t**):
        def Create(self, fn):
            # Create the customview
            if not idaapi.simplecustview_t.Create(self, "ASM View - %s" % os.path.basename(fn)):
                return False
            self.instruction_list = idautils.**GetInstructionList**()
            self.instruction_list.extend(["ret"])
            self.register_list    = idautils.**GetRegisterList**()
            self.register_list.extend(["eax", "ebx", "ecx", "edx", "edi", "esi", "ebp", "esp"])
            self.fn = fn
            if not self.reload_file():
                return False
            self.id_refresh = self.AddPopupMenu("Refresh")
            self.id_close   = self.AddPopupMenu("Close")
            return True
        def reload_file(self):
            if not self.colorize_file(self.fn):
                self.Close()
                return False
            return True
        def colorize_file(self, fn):
            try:
                f = open(fn, "r")
                lines = f.readlines()
                f.close()
                self.ClearLines()
                self.colorize(lines)
                return True
            except:
                return False
        def **add_line**(self, s=None):
            if not s:
                s = ""
            self.AddLine(s)
        def **as_comment**(self, s):
            return idaapi.COLSTR(s, idaapi.SCOLOR_RPTCMT)
        def **as_id**(self, s):
            t = s.lower()
            if t in self.**register_list:**
                return idaapi.COLSTR(s, idaapi.SCOLOR_REG)
            elif t in self.**instruction_list:**
                return idaapi.COLSTR(s, idaapi.SCOLOR_INSN)
            else:
                return s
        def **as_string**(self, s):
            return idaapi.COLSTR(s, idaapi.SCOLOR_STRING)
        def **as_num**(self, s):
            return idaapi.COLSTR(s, idaapi.SCOLOR_NUMBER)
        def **as_directive**(self, s):
            return idaapi.COLSTR(s, idaapi.SCOLOR_KEYWORD)
        def **OnPopupMenu**(self, menu_id):
            if self.id_refresh == menu_id:
                return self.reload_file()
            elif self.id_close == menu_id:
                self.Close()
                return True
            return False
        def **OnKeydown**(self, vkey, shift):
            # ESCAPE
            if vkey == 27:
                self.Close()
                return True
            return False
    

This blog entry inspired you to write a new plugin? Feel free to participate in our [plugin contest](<https://www.hex-rays.com/contests/>)! 

The ASM viewer script can be downloaded from [here](</ida_pro/files/asmview.py>) (note: it requires IDAPython [r289](<http://code.google.com/p/idapython/source/detail?r=289>)).

---

