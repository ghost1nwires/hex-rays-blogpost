# Hex-Rays Blog – Page 37

**Archived:** 2026-02-20 12:21
**URL:** https://hex-rays.com/blog/page/37

---

## 1. 

**Date:** Posted: Jan 7, 2020  
**Link:** [https://hex-rays.com/blog/ida-7-4-and-python-3-8](https://hex-rays.com/blog/ida-7-4-and-python-3-8)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [idapyswitch](<https://hex-rays.com/blog/tag/idapyswitch>)

# IDA 7.4 and Python 3.8

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-4-and-python-3-8>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-4-and-python-3-8>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-4-and-python-3-8>)

I 

Igor Skochinsky ✦ Posted: Jan 7, 2020

![IDA 7.4 and Python 3.8](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

As several of our users have noticed, IDA 7.4 Windows installer refuses to use Python 3.8.0 if you installed it. You can usually observe output similar to following:
    
    
    ----------
    Checking installs from "Python Software Foundation"
    Checking "Python 3.8 (64-bit)" (3.8)
    Found: "C:\Program Files\Python38\" (version: 3.8.0 ('38'))
    Ignoring unusable Python 3.8.0
    No Python installations were found
    ----------
    

So why exactly is 3.8.0 “unusable”? Well, it so happens that the 3.8.0 Windows release [broke an API](<https://bugs.python.org/issue37633>) on which IDAPython relies so we decided to exclude it. This let us proceed but now with 3.8.1 release which fixed it two new issues have surfaced:

1\. The 3.8.1 install is misdetected as 3.8.0 and thus is ignored. This is caused by the idapyswitch tool relying only on the file path for version detection. We have implemented more correct version checking that detects 3.8.1 properly and allows it to be selected. You can download the fixed version of idapyswitch [here](<https://www.hex-rays.com/wp-content/uploads/2020/01/idapyswitch.zip>).

2\. When trying to use PyQt, IDA crashes. Unfortunately, in Python 3.8 the layout of some internal structures has changed and SIP (the API bindings library used by PyQt) tries to access wrong data. Notably, this issue also affects Linux and Mac versions of IDAPython. If you are affected by this, you can download an updated build of the sip module below.

Downloads

[sip38-ida74-linux.zip](</wp-content/uploads/2020/01/sip38-ida74-linux.zip>)

[sip38-ida74-mac.zip](</wp-content/uploads/2020/01/sip38-ida74-mac.zip>)

[sip38-ida74-win.zip](</wp-content/uploads/2020/01/sip38-ida74-win.zip>)

---

## 2. 

**Date:** Posted: Jan 7, 2020  
**Link:** [https://hex-rays.com/blog/ida-7-3-qt-5-6-3-configure-options-patch](https://hex-rays.com/blog/ida-7-3-qt-5-6-3-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 7.3: Qt 5.6.3 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-3-qt-5-6-3-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-3-qt-5-6-3-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-3-qt-5-6-3-configure-options-patch>)

I 

Igor Skochinsky ✦ Posted: Jan 7, 2020

![IDA 7.3: Qt 5.6.3 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.3 build, that is shipped with IDA 7.3.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.3\configure.bat "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.3-x64"`
    * Note that you will have to build with Visual Studio 2015 or newer, to obtain compatible libs
  * Linux: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-64" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.3-x64"`
  * Mac OSX: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-clang" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.3-x64"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2019/06/qt-5.6.3_full-IDA73.patch_.zip>). You should therefore apply it to your own Qt 5.6.3 sources before compiling, in order to obtain similar binaries (`patch -p 1 < path/to/qt-5_6_3_full-IDA73.patch`)

Note that this patch should work without any modification, against the 5.6.3 release as found [there](<https://download.qt.io/archive/qt>). You may have to fiddle with it, if your Qt 5.6.3 sources come from somewhere else.

---

## 3. 

**Date:** Posted: Nov 29, 2019  
**Link:** [https://hex-rays.com/blog/environment-variable-editor](https://hex-rays.com/blog/environment-variable-editor)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Environment variable editor

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/environment-variable-editor>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/environment-variable-editor>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/environment-variable-editor>)

Elias Bachaalany ✦ Posted: Nov 29, 2019

![Environment variable editor](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Normally, to change environment variables in a running process, one has to terminate the process, edit the environment variables and re-run the process. In this blog entry we are going to write an IDAPython script that allows us to add, edit or delete environment variables in a running process directly. To achieve this we will use [Appcall](</blog/introducing-the-appcall-featur-1>) to manage the variables and a [custom viewer](</blog/using-custom-viewers-from-idap>) that serves as the graphical interface.

![envedit.gif](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/envedit-3.gif?width=672&height=479&name=envedit-3.gif)

## Background

In MS Windows, environment variables can be managed using various API calls. For our script we are only interested in 3 calls:

  * [GetEnvironmentStrings](<http://msdn.microsoft.com/en-us/library/ms683187\(VS.85\).aspx>): Returns a block of memory pointing to all the environment variables in the process. This block is a list of zero terminated strings, the end of the list is marked with two **\0** characters (such list is also known as MULTI_SZ)
  * [FreeEnvironmentStrings](<http://msdn.microsoft.com/en-us/library/ms683151\(VS.85\).aspx>): Frees the block returned by GetEnvironmentStrings()
  * [SetEnvironmentVariable](<http://msdn.microsoft.com/en-us/library/ms686206\(VS.85\).aspx>): Creates or edits an environment variable



In order to modify the environment variables in a process, we need to issue those API calls in the context of that process. One solution is to inject a DLL into the running process and ask it to call those APIs. Another simpler solution is to write a script that uses Appcall to issue those API calls.

## Setting up Appcall

Let us use Appcall to retrieve 3 callable objects corresponding to the APIs in question:
    
    
    GetEnvironmentStrings =
        Appcall.proto("kernel32_GetEnvironmentStringsA",
                      "unsigned int __stdcall GetEnvironmentStrings();")
    SetEnvironmentVariable =
        Appcall.proto("kernel32_SetEnvironmentVariableA",
                      "int __stdcall SetEnvironmentVariable(const char *lpName, const char *lpValue);")
    FreeEnvironmentStrings =
        Appcall.proto("kernel32_FreeEnvironmentStringsA",
                      "int __stdcall FreeEnvironmentStrings(unsigned int block);")
    

Now we can use those callable objects to call the APIs in the context of the debugged process, for example:
    
    
    # Add a new environment variable
    SetEnvironmentVariable("MY_VAR", "Some value")

Please note that:

  * Both GetEnvironmentStrings() and FreeEnvironmentStrings() prototypes are re-declared to accept an **unsigned int** instead of a character pointer because Appcall does not support the MULTI_SZ type.
  * We are using the ASCII version of those APIs. Modifying the script to work with the unicode version is not very difficult to achieve.



## Retrieving all environment variables

We need to write a function to retrieve all the environment variables in a list:

  1. Call the GetEnvironmentStrings() to retrieve a pointer to the environment block in the process. Save that pointer. 
  2. Read the debuggee’s memory at that block and retrieve the variables accordingly.
  3. Free the block by calling FreeEnvironmentStrings()



To read the debuggee’s memory, we can use idc.Bytes(), idaapi.get_many_bytes() or idaapi.dbg_read_memory(). Since we don’t know how big the block is, we will read one character at a time and process it. For demonstration purposes, we will use yet another mechanism to read from the debuggee’s memory as if we were reading from a file. We will use the **loader_input_t** class followed by a call to **open_memory()** function: 
    
    
    def Variables():
        """Returns all environment blocks"""
        # Get all environment strings
        env_ptr = **GetEnvironmentStrings**()
        if env_ptr == 0:
            return None
        # Always refresh the memory after a call
        # that could change the memory configuration
        idc.**RefreshDebuggerMemory**()
        # Open process memory
        f = idaapi.**loader_input_t**()
        f.**open_memory**(env_ptr)
        # Parse all environment variables into a list of variables
        vars = []
        var = []
        while True:
            ch = f.**get_char**()
            if not ch:
                break
            if ch == '\x00':
                # double-null -> end of list
                if not var:
                    break
                vars.append(''.join(var))
                var = []
            else:
                var.append(ch)
        f.close()
        # Free the block
        **FreeEnvironmentStrings**(env_ptr)
        return vars
    

## Writing a custom viewer to manage the environment variables

Now that we have all the required supporting code, we can write a custom viewer that does the following:

  * Retrieves all the environment variables: call GetEnvironmentStrings() and parse the result 
    * Parse each environment variable into KEY/VALUE components
    * Add a colored line into the custom viewer with different colors for the KEY and VALUE
  * Add popup menu entries and handle keypresses to support three operations: 
    * Insert: Ask the user for a key and value using the **idaapi.askstr**() then call the SetEnvironmentVariable() API as shown in the previous example
    * Edit: Retrieve the cursor position, extract the key name, ask the user for a new value only (w/o asking for a key) and call the SetEnvironmentVariable() API
    * Delete: Retrieve the key name and call SetEnvironmentVariable() API with value pointing to NULL



Before running the script, make sure that the debugged process is suspended. Right-click on an environment variable to edit/insert or delete.

Please download the script from [here](</ida_pro/files/envedit.py>).

---

## 4. 

**Date:** Posted: Nov 29, 2019  
**Link:** [https://hex-rays.com/blog/extending-ida-processor-modules-for-gdb-debugging](https://hex-rays.com/blog/extending-ida-processor-modules-for-gdb-debugging)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Extending IDA processor modules for GDB debugging

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/extending-ida-processor-modules-for-gdb-debugging>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/extending-ida-processor-modules-for-gdb-debugging>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/extending-ida-processor-modules-for-gdb-debugging>)

Ramiro Polla ✦ Posted: Nov 29, 2019

![Extending IDA processor modules for GDB debugging](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

# Introduction

IDA has debugging support for multiple architectures, such as Intel x86, ARM, PowerPC, MIPS, and, since [IDA 7.4](<https://www.hex-rays.com/products/ida/7.4/index.shtml>), Motorola 68k, Infineon TriCore, and Renesas RH850.

Some of these architectures are natively supported, either locally through IDA (x86-only), or remotely through the use of debugger servers (x86 and ARM). The other architectures listed above are supported through the GDB debugger in IDA.

The GDB debugger in IDA implements the [GDB remote serial protocol](<https://sourceware.org/gdb/onlinedocs/gdb/Remote-Protocol.html>), which is itself architecture-agnostic. It could theoretically be used by IDA to support any architecture through any program that implements a GDB stub. To name a few:

  * [gdbserver](<https://www.gnu.org/software/gdb/>)
  * [QEMU](<https://www.qemu.org/>)
  * [VMware](<https://www.vmware.com/>)
  * [Wine](<https://www.winehq.org/>) (fun fact: you can use Wine+GDB+IDA to debug Windows programs natively under Linux)
  * [OpenOCD](<http://openocd.org/>)
  * [Bochs](<http://bochs.sourceforge.net/>)
  * [Lauterbach’s TRACE32](<https://www.lauterbach.com/sim.html>)
  * [Wind River’s Simics](<https://www.windriver.com/products/simics/>)
  * [MAME](<https://www.mamedev.org/>)



But what if you want to debug another architecture, which your favorite GDB stub implements, but which still does not have debug support for IDA?

For that, you can edit IDA’s GDB debugger configuration and write your own IDA plugin to extend the processor module, by providing callbacks that make IDA aware of the architecture you want to debug.

And this is exactly what we will do in this post, improving the Z80 processor module to support remote debugging through the GDB stub in the [Multiple Arcade Machine Emulator](<https://www.mamedev.org/>), which will allow us to debug `pacman`.

# Adding an architecture to GDB debugger

The first step is to update the configuration file for IDA’s GDB debugger (`dbg_gdb.cfg`) and add a configuration for the new architecture.
    
    
    --- cfg/dbg_gdb.cfg
    +++ cfg/dbg_gdb.cfg
    @@ -49,6 +49,7 @@ ARM_UPDATE_CPSR = 1
     
     // copied from idp.hpp
     #define PLFM_386         0        ///< Intel 80x86 (and x86_64/AMD64)
    +#define PLFM_Z80         1        ///< 8085, Z80
     #define PLFM_68K         7        ///< Motorola 680x0
     #define PLFM_MIPS       12        ///< MIPS
     #define PLFM_ARM        13        ///< ARM (also includes AArch64)
    @@ -95,6 +96,7 @@ CONFIGURATIONS =
       "MIPS64 Little-endian":    [ PLFM_MIPS,      0,        8,       0,         "mipsl",        "mips",             "mips64-linux.xml",      "0D000500", 0 ],
       "Motorola 68k":            [ PLFM_68K,       1,        4,       0,         "68k",          "m68k",             "m68k.xml",              "4E4F",     1 ],
       "Infineon TriCore":        [ PLFM_TRICORE,   0,        4,       0,         "tricore",      "tricore",          "tricore.xml",           "00A0",     1 ],
    +  "Zilog Z80":               [ PLFM_Z80,       0,        4,       0,         "z80",          "z80",              "",                      "",         1 ],
       "Renesas RH850":           [ PLFM_NEC_V850X, 0,        4,       0,         "rh850",        "rh850",            "rh850.xml",             "40F8",     1 ]
     }
     
    @@ -552,6 +554,7 @@ ARCH_MAP =
       "mips":             [ PLFM_MIPS,      -1,   -1,   -1 ],
       "m68k":             [ PLFM_68K,        0,    1,   -1 ],
       "tricore":          [ PLFM_TRICORE,    0,    0,   -1 ],
    +  "z80":              [ PLFM_Z80,        0,    0,   -1 ],
       "rh850":            [ PLFM_NEC_V850X,  0,    0,   -1 ]
     }
     
    

The define for `PLFM_Z80` is copied from `idd.hpp`.

The definition in `CONFIGURATIONS` does not provide an XML file with the register layout at the moment and does not provide the breakpoint instruction, since it does not exist for Z80.

The definition in `ARCH_MAP` specifies that the processor is not 64-bit, and that it is not big-endian.

This is the bare minimum you need to do to add a new architecture for debugging using GDB in IDA. It provides limited debugging capabilities, and may not work at all if the remote GDB stub does not provide register information or does not support single stepping.

In this specific case though it kind of works.

Start MAME with:
    
    
        $ mame64 pacman -debugger gdbstub -debug
    

It will start and wait for a debugger to attach.

Then start IDA, and attach to MAME by going `Debugger` > `Attach` > `Remote GDB debugger`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/debugger_attach_gdb-3.png?width=460&height=316&name=debugger_attach_gdb-3.png) Debugger > Attach > Remote GDB debugger 

Set the hostname to `localhost`, click `Debug options`, `Set specific options`, and under the `Configuration` dropdown menu, select the `Zilog Z80` configuration that we just added. Click `OK` (four times) to finally attach.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/gdb_setup_z80_config-3.png?width=1024&height=736&name=gdb_setup_z80_config-3.png) Z80 Configuration 

At this point you can single step (since this GDB stub does support single-stepping), continue, break, examine memory contents, use breakpoints, and a few more basic debugging commands.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pacman_game_over-3.png?width=1057&height=600&name=pacman_game_over-3.png) pacman 

But we are missing many features, such as viewing individual flags in the `F` register, stepping over instructions, using register values in IDC/IDAPython scripts, and viewing hints in IDA to quickly navigate the disassembly.

# Describing registers

If the remote GDB stub does not provide the register layout, IDA needs to have a default layout provided in form of XML files, which hopefully matches the layout from the remote GDB stub. For example, create the following file in `cfg/z80.xml`:
    
    
    <?xml version="1.0"?>
    <!DOCTYPE target SYSTEM "gdb-target.dtd">
    <target version="1.0">
    <architecture>z80</architecture>
      <feature name="mame.z80">
        <reg name="af" bitsize="16" type="int"/>
        <reg name="bc" bitsize="16" type="int"/>
        <reg name="de" bitsize="16" type="int"/>
        <reg name="hl" bitsize="16" type="int"/>
        <reg name="af'" bitsize="16" type="int"/>
        <reg name="bc'" bitsize="16" type="int"/>
        <reg name="de'" bitsize="16" type="int"/>
        <reg name="hl'" bitsize="16" type="int"/>
        <reg name="ix" bitsize="16" type="int"/>
        <reg name="iy" bitsize="16" type="int"/>
        <reg name="sp" bitsize="16" type="data_ptr"/>
        <reg name="pc" bitsize="16" type="code_ptr"/>
      </feature>
    </target>
    

Note: The XML file above was provided by MAME itself over the serial protocol. This layout must match the one being used in the GDB stub. You might need to delve into the GDB stub implementation to get the proper register layout.

Now add the XML file to `dbg_gdb.cfg`, and also provide a better description of the Z80 registers (along with the bitfields for the flags register).
    
    
    --- cfg/dbg_gdb.cfg
    +++ cfg/dbg_gdb.cfg
    @@ -96,7 +96,7 @@ CONFIGURATIONS =
       "MIPS64 Little-endian":    [ PLFM_MIPS,      0,        8,       0,         "mipsl",        "mips",             "mips64-linux.xml",      "0D000500", 0 ],
       "Motorola 68k":            [ PLFM_68K,       1,        4,       0,         "68k",          "m68k",             "m68k.xml",              "4E4F",     1 ],
       "Infineon TriCore":        [ PLFM_TRICORE,   0,        4,       0,         "tricore",      "tricore",          "tricore.xml",           "00A0",     1 ],
    -  "Zilog Z80":               [ PLFM_Z80,       0,        4,       0,         "z80",          "z80",              "",                      "",         1 ],
    +  "Zilog Z80":               [ PLFM_Z80,       0,        4,       0,         "z80",          "z80",              "z80.xml",               "",         1 ],
       "Renesas RH850":           [ PLFM_NEC_V850X, 0,        4,       0,         "rh850",        "rh850",            "rh850.xml",             "40F8",     1 ]
     }
     
    @@ -480,6 +480,32 @@ IDA_FEATURES =
         }
       },
     
    +  "z80":
    +  {
    +    "mame.z80":
    +    {
    +      "title": "General registers",
    +      "code_ptr": "pc",
    +      "stack_ptr": "sp",
    +      "data_ptr":
    +      [
    +        "af",  "bc",  "de",  "hl",
    +        "af'", "bc'", "de'", "hl'",
    +        "ix",  "iy",  "sp",  "pc"
    +      ],
    +      "bitfields":
    +      {
    +        "af":
    +        {
    +          "C":   [0, 0],
    +          "P/V": [2, 2],
    +          "Z":   [6, 6],
    +          "S":   [7, 7]
    +        }
    +      }
    +    }
    +  },
    +
       "rh850":
       {
         "rh850.core":
    

We can now see the flag bits in the General Registers window.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/flags-3.png?width=645&height=228&name=flags-3.png) Z80 flags 

But to solve all other missing debugger features, we will have to create a plugin that extends the processor module for Z80.

# Processor module extension skeleton

Using the `IDA SDK`, we will create a skeleton processor module extension. You may use the `procext` plugin as an example.

Inside the SDK, create the `plugins/z80dbg/` directory.

Inside this new directory, create a `makefile` with these contents:
    
    
    PROC=z80dbg
    
    include ../plugin.mak
    

And a file named `z80dbg.cpp` with these contents:
    
    
    #include <ida.hpp>
    #include <idd.hpp>
    #include <idp.hpp>
    
    #include <loader.hpp>
    
    //-------------------------------------------------------------------------
    static ssize_t idaapi z80_debug_callback(void * /*user_data*/, int event_id, va_list va)
    {
      // TODO process debug callbacks
      switch ( event_id )
      {
        case processor_t::ev_get_reg_info:
          break;
        case processor_t::ev_get_idd_opinfo:
          break;
        case processor_t::ev_next_exec_insn:
          break;
        case processor_t::ev_calc_step_over:
          break;
      }
      return 0;                     // event is not processed
    }
    
    //-------------------------------------------------------------------------
    static int idaapi init(void)
    {
      if ( ph.id != PLFM_Z80 )
        return PLUGIN_SKIP;
      hook_to_notification_point(HT_IDP, z80_debug_callback);
      return PLUGIN_KEEP;
    }
    
    //-------------------------------------------------------------------------
    static void idaapi term(void)
    {
      unhook_from_notification_point(HT_IDP, z80_debug_callback);
    }
    
    //-------------------------------------------------------------------------
    static bool idaapi run(size_t)
    {
      return true;
    }
    
    //-------------------------------------------------------------------------
    static const char comment[] = "Z80 debugger processor extension";
    static const char help[] =
      "Z80 debugger module\n"
      "\n"
      "This plugin extends the Z80 processor module to support debugging.\n";
    static const char wanted_name[] = "Z80 debugger processor extension";
    static const char wanted_hotkey[] = "";
    plugin_t PLUGIN =
    {
      IDP_INTERFACE_VERSION,
      PLUGIN_PROC,          // Load plugin when a processor module is loaded
      init,                 // Initialize plugin
      term,                 // Terminate plugin
      run,                  // Invoke plugin
      comment,              // Long comment about the plugin
      help,                 // Multiline help about the plugin
      wanted_name,          // The preferred short name of the plugin
      wanted_hotkey         // The preferred hotkey to run the plugin
    };
    

This is the minimum amount of code we need to start creating our plugin. It adds a hook (`z80_debug_callback()`) which will be loaded whenever the `Z80` processor module is loaded, and will handle processor module events (`HT_IDP`).

Now we build it in the command line with:
    
    
        $ cd plugins/z80dbg
        $ make
    

The plugin will be created in `../../bin/plugins/z80dbg.so`. We copy that file over to IDA’s installation, under the `plugins/` directory.
    
    
        $ cp ../../bin/plugins/z80dbg.so ~/idapro-7.4/plugins/
    

Restart IDA and start debugging MAME again, and you should be able to see a new entry in `Edit` > `Plugins` > `Z80 debugger processor extension`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/plugin1-3.png?width=863&height=641&name=plugin1-3.png) Plugin skeleton 

Congratulations! The plugin still does nothing, but it is already being run by IDA.

# Adding register name information

The `processor_t::ev_get_reg_info` event requests more information about the register from the processor module, such as the width of the register, and whether it is made of a subset of a bigger register.

This is the case with Z80, where the GDB stub provides the `AF` register, but we want to access the `A` and the `F` registers individually.

First we implement the function that checks for substrings in the register names and returns bitfield information:
    
    
    //-------------------------------------------------------------------------
    static bool z80_get_reg_info(
            const char **main_regname,
            bitrange_t *bitrange,
            const char *regname)
    {
      // Sanity checks.
      if ( regname == NULL || regname[0] == '\0' )
        return false;
    
      static const char *const subregs[][3] =
      {
        { "af",  "a",  "f"  },
        { "bc",  "b",  "c"  },
        { "de",  "d",  "e"  },
        { "hl",  "h",  "l"  },
        { "af'", "a'", "f'" },
        { "bc'", "b'", "c'" },
        { "de'", "d'", "e'" },
        { "hl'", "h'", "l'" },
        { "ix",  NULL, NULL },
        { "iy",  NULL, NULL },
        { "sp",  NULL, NULL },
        { "pc",  NULL, NULL },
      };
    
      // Check if we are dealing with paired or single registers and return
      // the appropriate information.
      for ( size_t i = 0; i < qnumber(subregs); i++ )
      {
        for ( size_t j = 0; j < 3; j++ )
        {
          if ( subregs[i][j] == NULL )
            break;
          if ( strieq(regname, subregs[i][j]) )
          {
            if ( main_regname != NULL )
              *main_regname = subregs[i][0];
            if ( bitrange != NULL )
            {
              switch ( j )
              {
                case 0: *bitrange = bitrange_t(0, 16); break;
                case 1: *bitrange = bitrange_t(8,  8); break;
                case 2: *bitrange = bitrange_t(0,  8); break;
              }
            }
            return true;
          }
        }
      }
    
      return false;
    }
    

And then we implement a wrapper that parses the arguments for the event inside `z80_debug_callback()`, and call that function:
    
    
        case processor_t::ev_get_reg_info:
          {
            const char **main_regname = va_arg(va, const char **);
            bitrange_t *bitrange      = va_arg(va, bitrange_t *);
            const char *regname       = va_arg(va, const char *);
            return z80_get_reg_info(main_regname, bitrange, regname) ? 1 : -1;
          }
    

This function tells IDA that, for example, the `F` register is a subset of the `AF` register, and that it starts at offset 8, and is 8 bits long.

If we run the debugger again, we see that we can now access the register values from IDC/IDAPython scripts. We can even access the individual register values from the paired registers:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idc-3.png?width=742&height=369&name=idc-3.png) IDC registers 

# Getting more information on operands

The `processor_t::ev_get_idd_opinfo` event requests more information about an operand in the listing. It is very helpful to quickly show values of registers or memory by hovering the mouse over the operand, and to quickly jump to a location pointed by an operand by double-clicking on it.

First implement the function:
    
    
    //-------------------------------------------------------------------------
    typedef const regval_t &idaapi getreg_t(const char *name, const regval_t *regvalues);
    
    //-------------------------------------------------------------------------
    static sval_t named_regval(
            const char *regname,
            getreg_t *getreg,
            const regval_t *rv)
    {
      // Get register info.
      const char *main_regname;
      bitrange_t bitrange;
      if ( !z80_get_reg_info(&main_regname, &bitrange, regname) )
        return 0;
    
      // Get main register value and apply bitrange.
      sval_t ret = getreg(main_regname, rv).ival;
      ret >>= bitrange.bitoff();
      ret &= (1ULL << bitrange.bitsize()) - 1;
      return ret;
    }
    
    //-------------------------------------------------------------------------
    static sval_t regval(
            const op_t &op,
            getreg_t *getreg,
            const regval_t *rv)
    {
      // Check for bad register number.
      if ( op.reg > ph.regs_num )
        return 0;
      return named_regval(ph.reg_names[op.reg], getreg, rv);
    }
    
    //-------------------------------------------------------------------------
    static bool z80_get_operand_info(
            idd_opinfo_t *opinf,
            ea_t ea,
            int n,
            getreg_t *getreg,
            const regval_t *regvalues)
    {
      // No Z80 instruction has operand number greater than 2.
      if ( n < 0 || n > 2 )
        return false;
    
      // Decode instruction at ea.
      insn_t insn;
      if ( decode_insn(&insn, ea) < 1 )
        return false;
    
      // Check the instruction features to see if the operand is modified.
      opinf->modified = has_cf_chg(insn.get_canon_feature(), n);
    
      // Get operand value (possibly an ea).
      uint64 v = 0;
      const op_t &op = insn.ops[n];
      switch ( op.type )
      {
        case o_reg:
          // We use the getreg function (along with regvalues) to retrieve
          // the value of the register specified in op.reg.
          v = regval(op, getreg, regvalues);
          break;
        case o_mem:
        case o_near:
          // Memory addresses are stored in op.addr.
          opinf->ea = op.addr;
          break;
        case o_phrase:
          // Memory references using register value.
          opinf->ea = regval(op, getreg, regvalues);
          break;
        case o_displ:
          // Memory references using register and address value.
          opinf->ea = regval(op, getreg, regvalues) + op.addr;
          break;
        case o_imm:
          // Immediates are stored in op.value.
          v = op.value;
          break;
        default:
          return false;
      }
      opinf->value._set_int(v);
      opinf->value_size = get_dtype_size(op.dtype);
    
      return true;
    }
    

And then implement the wrapper for `z80_debug_callback()`:
    
    
        case processor_t::ev_get_idd_opinfo:
          {
            idd_opinfo_t *opinf       = va_arg(va, idd_opinfo_t *);
            ea_t ea                   = va_arg(va, ea_t);
            int n                     = va_arg(va, int);
            int thread_id             = va_arg(va, int);
            getreg_t *getreg          = va_arg(va, getreg_t *);
            const regval_t *regvalues = va_arg(va, const regval_t *);
            qnotused(thread_id);
            return z80_get_operand_info(opinf, ea, n, getreg, regvalues) ? 1 : 0;
          }
    

What the function does is decode the instruction and get the current value of the specified operand number. This might require reading registers or memory values.

This function is tricky since it depends a lot on how the processor module represents each operand. Normally you will deal with default operand types such as `o_reg`, `o_mem`, `o_near`, `o_phrase`, `o_displ`, and `o_imm`, but you might have to deal with custom operand types (`o_idpspec0` and such). In this case, it is better to have the source code of the processor module to verify how each operand was assigned.

When you don’t know how the operands are represented, it helps to break every time the function is passed an unknown operand. It is particularly helpful to check the value of `ph.instruc[insn.itype]`, so you know which instruction you are dealing with and which features it has (see `struct instruc_t` from `idp.hpp`).

We can now hover the mouse on the operand and quickly get hints with the value of the memory at the address pointed to by `(hl)`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hints-3.png?width=636&height=245&name=hints-3.png) Hints 

Also note that not only `hl`, but also the `h` and `l` registers are highlighted, due to the previous `processor_t::ev_get_reg_info` event.

# Single-stepping support

Some GDB stubs support a command to single-step. But for stubs that do not implement single-stepping, IDA must know where the program counter will end up after one instruction so it may place a breakpoint there.

A conditional jump instruction, for example, may either jump to the specified address or continue execution at the next instruction.

We must inform IDA of all the instructions that may change the control flow, and figure out at run-time what is the next instruction that will be executed. This might also require reading registers or memory values.

The control-flow instructions for the Z80 are rather simple:
    
    
    //-------------------------------------------------------------------------
    static ea_t z80_next_exec_insn(
            ea_t ea,
            getreg_t *getreg,
            const regval_t *regvalues)
    {
      // Decode instruction at ea.
      insn_t insn;
      if ( decode_insn(&insn, ea) < 1 )
        return BADADDR;
    
      // Get next address to be executed.
      ea_t target = BADADDR;
      switch ( insn.itype )
      {
        case Z80_jp:
        case Z80_jr:
        case Z80_call:
          if ( z80_check_cond(insn.Op1.reg, getreg, regvalues) )
          {
            if ( insn.Op2.type == o_near )
              target = insn.Op2.addr;
            else if ( insn.Op2.type == o_phrase )
              target = regval(insn.Op2, getreg, regvalues);
          }
          break;
    
        case Z80_djnz:
          {
            uint8_t B = named_regval("B", getreg, regvalues);
            if ( (B-1) != 0 )
              target = insn.Op1.addr;
          }
          break;
    
        case Z80_ret:
          if ( !z80_check_cond(insn.Op1.reg, getreg, regvalues) )
            break;
          // fallthrough
        case Z80_reti:
        case Z80_retn:
          {
            uint16_t SP = named_regval("SP", getreg, regvalues);
            target = get_word(SP);
          }
          break;
      }
    
      return target;
    }
    

And then implement the wrapper for `z80_debug_callback()`:
    
    
        case processor_t::ev_next_exec_insn:
          {
            ea_t *target              = va_arg(va, ea_t *);
            ea_t ea                   = va_arg(va, ea_t);
            int tid                   = va_arg(va, int);
            getreg_t *getreg          = va_arg(va, getreg_t *);
            const regval_t *regvalues = va_arg(va, const regval_t *);
            qnotused(tid);
            *target = z80_next_exec_insn(ea, getreg, regvalues);
            return 1;
          }
    

IDA also uses this information to provide visual cues of the control flow of the debuggee. In the listing window, we get a green arrow pointing to the next instruction to be executed.

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png) Single stepping 

# Step-over support

The last event we need to implement for debugging support is `processor_t::ev_calc_step_over`. It is used to tell IDA which instructions should be stepped over. This usually involves the instructions that call functions and some specific loop instructions (such as `djnz` in the Z80).
    
    
    //-------------------------------------------------------------------------
    static ea_t z80_calc_step_over(ea_t ip)
    {
      insn_t insn;
      if ( ip == BADADDR || decode_insn(&insn, ip) < 1 )
        return BADADDR;
    
      // Allow stepping over call instructions and djnz.
      bool step_over = is_call_insn(insn)
                    || insn.itype == Z80_djnz;
      if ( step_over )
        return insn.ea + insn.size;
    
      return BADADDR;
    }
    

And then implement the wrapper for `z80_debug_callback()`:
    
    
        case processor_t::ev_calc_step_over:
          {
            ea_t *target = va_arg(va, ea_t *);
            ea_t ip      = va_arg(va, ea_t);
            *target = z80_calc_step_over(ip);
            return 1;
          }
    

Now we can easily break out of this loop (that would take another six iterations) by stepping over the `djnz` instruction.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/step_over-3.png?width=607&height=229&name=step_over-3.png) Step over 

# Conclusion

We have successfully extended the Z80 processor module with debugging support.

For a quick example, let’s access the [pacman level 256 glitch](<https://en.wikipedia.org/wiki/Pac-Man#Level_256>) using MAME over GDB (more information about the technical aspects of the glitch can be found at [[1]](<http://www.donhodges.com/how_high_can_you_get2.htm>) [[2]](<http://cubeman.org/arcade-source/pacman.asm>))

1\. Attach to `pacman`  
2\. Go to address `0x2BF0`, create an instruction (press `C`), and add a breakpoint (press `F2`)  
3\. Resume execution (press `F9`)  
4\. Insert a coin and start the game  
5\. When the breakpoint hits, patch the memory with this IDC command:  
`patch_byte(0x4E13, 0xFF)`  
6\. Remove the breakpoint at address `0x2BF0`  
7\. Resume execution  
8\. Enjoy pacman at level 256!

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pacman_level_256-1024x536-3.png?width=660&height=345&name=pacman_level_256-1024x536-3.png) pacman level 256 

The full source code for the plugin can be found at /wp-content/uploads/2019/11/z80dbg.zip

---

## 5. 

**Date:** Posted: Nov 4, 2019  
**Link:** [https://hex-rays.com/blog/ida-7-4-idapython-and-python-3](https://hex-rays.com/blog/ida-7-4-idapython-and-python-3)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.4: IDAPython and Python 3

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-4-idapython-and-python-3>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-4-idapython-and-python-3>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-4-idapython-and-python-3>)

Arnaud Diederen ✦ Posted: Nov 4, 2019

![IDA 7.4: IDAPython and Python 3](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

IDA 7.4 will still ship with IDAPython for Python 2.7 by default, but users will now have the opportunity to [pick IDAPython for Python 3.x at installation-time!](</products/ida/support/ida74_idapython_python3>)

---

## 6. 

**Date:** Posted: Nov 4, 2019  
**Link:** [https://hex-rays.com/blog/ida-7-4-qt-5-6-3-configure-options-patch](https://hex-rays.com/blog/ida-7-4-qt-5-6-3-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 7.4: Qt 5.6.3 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-4-qt-5-6-3-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-4-qt-5-6-3-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-4-qt-5-6-3-configure-options-patch>)

Arnaud Diederen ✦ Posted: Nov 4, 2019

![IDA 7.4: Qt 5.6.3 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.3 build, that is shipped with IDA 7.4.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.3\configure.bat "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.3-x64"`
    * Note that you will have to build with Visual Studio 2015 to obtain compatible libs
  * Linux: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-64" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.3-x64"`
  * Mac OSX: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-clang" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.3-x64"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2019/11/qt-5.6.3_full-IDA74.patch_.zip>). You should therefore apply it to your own Qt 5.6.3 sources before compiling, in order to obtain similar binaries (`patch -p 1 < path/to/qt-5_6_3_full-IDA74.patch`)

Note that this patch should work without any modification, against the 5.6.3 release as found [there](<https://download.qt.io/archive/qt>). You may have to fiddle with it, if your Qt 5.6.3 sources come from somewhere else.

---

## 7. 

**Date:** Posted: Oct 8, 2019  
**Link:** [https://hex-rays.com/blog/lumina-certificate-expiration-on-october-10th-2010-workaround](https://hex-rays.com/blog/lumina-certificate-expiration-on-october-10th-2010-workaround)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Lumina certificate expiration on October 10th, 2010: workaround

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/lumina-certificate-expiration-on-october-10th-2010-workaround>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/lumina-certificate-expiration-on-october-10th-2010-workaround>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/lumina-certificate-expiration-on-october-10th-2010-workaround>)

Arnaud Diederen ✦ Posted: Oct 8, 2019

![Lumina certificate expiration on October 10th, 2010: workaround](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

We invite our **Lumina** users to read [this short announcement](</products/ida/lumina/lumina-cert-20191010/>)

---

## 8. 

**Date:** Posted: Aug 1, 2019  
**Link:** [https://hex-rays.com/blog/ida-7-4-turning-off-ida-6-x-compatibility-in-idapython-by-default](https://hex-rays.com/blog/ida-7-4-turning-off-ida-6-x-compatibility-in-idapython-by-default)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.4: Turning off IDA 6.x compatibility in IDAPython by default

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-4-turning-off-ida-6-x-compatibility-in-idapython-by-default>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-4-turning-off-ida-6-x-compatibility-in-idapython-by-default>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-4-turning-off-ida-6-x-compatibility-in-idapython-by-default>)

Arnaud Diederen ✦ Posted: Aug 1, 2019

![IDA 7.4: Turning off IDA 6.x compatibility in IDAPython by default](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

IDA 7.4 will ship with the IDAPython “IDA 6.x” compatibility layer off by default. Please see [this article](</products/ida/support/ida74_idapython_no_bc695>) for more information!

---

## 9. 

**Date:** Posted: Jun 19, 2019  
**Link:** [https://hex-rays.com/blog/ida-7-3-css-styling](https://hex-rays.com/blog/ida-7-3-css-styling)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.3: CSS styling

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-3-css-styling>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-3-css-styling>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-3-css-styling>)

Arnaud Diederen ✦ Posted: Jun 19, 2019

![IDA 7.3: CSS styling](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Since version 7.3, IDA is styled using CSS. Please see [this article](<https://www.hex-rays.com/products/ida/support/tutorials/themes.shtml>) to see what can be done, and how!

---

