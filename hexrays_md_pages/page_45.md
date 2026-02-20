# Hex-Rays Blog – Page 45

**Archived:** 2026-02-20 12:24
**URL:** https://hex-rays.com/blog/page/45

---

## 1. 

**Date:** Posted: Mar 10, 2010  
**Link:** [https://hex-rays.com/blog/preview-of-the-new-cross-platform-ida-pro-gui](https://hex-rays.com/blog/preview-of-the-new-cross-platform-ida-pro-gui)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Preview of the new cross-platform IDA Pro GUI

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/preview-of-the-new-cross-platform-ida-pro-gui>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/preview-of-the-new-cross-platform-ida-pro-gui>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/preview-of-the-new-cross-platform-ida-pro-gui>)

Daniel Pistelli ✦ Posted: Mar 10, 2010

![Preview of the new cross-platform IDA Pro GUI](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

In order to provide our customers with the best user experience and in order to target many different platforms, the IDA Pro graphical user interface is currently being rewritten using the Qt technology.  
Qt (pronounced “cute”) is a cross-platform application and UI framework and the Win32 VCL-based IDA Pro interface is being ported to it. The goal is to provide all the features available in the current GUI while maintaining the maximum compatibility with plugins and other external modules.  
Here is a screenshot of the current build of **idaqt** running on Ubuntu:  
[![idaqt_preview_100310_thumb_1.jpg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idaqt_preview_100310_thumb_1-3.jpg?width=680&height=405&name=idaqt_preview_100310_thumb_1-3.jpg)](</ida_pro/pix/idaqt_preview_100310_1.html>)  
You can click on the images to enlarge them.  
  
From the first version **idaqt** will include a fully functional graphing, which is, as it is possible to notice from the screenshot, already implemented. The same is true for hints, navigation band and all other advanced IDA Pro features.  
[![idaqt_preview_100310_thumb_2.jpg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idaqt_preview_100310_thumb_2-3.jpg?width=680&height=397&name=idaqt_preview_100310_thumb_2-3.jpg)](</ida_pro/pix/idaqt_preview_100310_2.html>)  
This is **idaqt** on Windows 7. The text view looks exactly the same and all other features like choosers and forms will be available with no exception on all supported platforms.  
[![idaqt_preview_100310_thumb_3.jpg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idaqt_preview_100310_thumb_3-3.jpg?width=680&height=397&name=idaqt_preview_100310_thumb_3-3.jpg)](</ida_pro/pix/idaqt_preview_100310_3.html>)  
The full range of options and customizations which the Win32 interface provides will be available as well.  
As you can see, apart from the still to be implemented docking, the new interface looks pretty much the same as the Win32 one.  
Not only will it be possible to deploy the same native graphical interface to Windows, OS X, Linux and other platforms which in the future may become popular, but the quality of the user experience and the further development capabilities will be hugely increased thanks to an advanced framework such as Qt.  
Although **idaqt** is going to replace the current GUI completely, for some time they will be deployed together in order to fix any incompatibility issues and to give third party developers the necessary time to thoroughly test their products against the new interface.

---

## 2. 

**Date:** Posted: Feb 25, 2010  
**Link:** [https://hex-rays.com/blog/custom-data-types-and-formats](https://hex-rays.com/blog/custom-data-types-and-formats)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Custom data types and formats

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/custom-data-types-and-formats>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/custom-data-types-and-formats>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/custom-data-types-and-formats>)

Elias Bachaalany ✦ Posted: Feb 25, 2010

![Custom data types and formats](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Another new feature that will be available in the upcoming version of IDA Pro is the ability to create and render custom data types and formats.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_cover-3.gif)  
(Embedded instructions disassembled and rendered along side with x86 code)  
  


## What are custom types and formats

  * Custom data type: A custom type is basically just a way to tag some bytes for later display with custom format, when the built-in IDA types (dt_byte, dt_word, etc) are not enough.  
For example: an XMM vector, a Pascal string, a half-precision (16 bits) floating-point number, a 16:32 far pointer (fword), uleb128 number and so on.  
To define a custom type, you need to provide its name, size (fixed or dynamically calculated), keyword for disassembly and a few other attributes.

  * Custom data format:  
The custom data format allows you do display a custom or built-in data type in any way you like. You can register several formats for each type and switch the representation.  
For example, you might want to switch the display of the same 16-byte XMM vector between four floats or two doubles.  
A format definition includes callback for printing (to display) and scanning (used during debugging to change the register values). 



For example, here is a custom MAKE_DWORD format applied to the built-in dword type:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_mkdword-3.gif)

Its implementation is very simple:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_mkdword_code-3.gif)

Next we illustrate some possible usages of custom types and formats. Other uses are also possible too, it is up to your imagination.

## Decoding embedded bytecodes

Imagine you are debugging an x86 program that implements its own VM and embeddes them in the program.  
The classical solution for this problem can be:

  * Write a dedicated processor module and then load the extracted bytecodes separately 
  * Or define the bytecodes as bytes and then use comments to describe the real meaning of those bytecodes. 



With this new addition, one can just write a custom data type to handle the situation:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_vm_data-3.gif)

And if you happen to have a situation where the bytecodes are operands to instructions (as means of obfuscation), you can still apply the custom format on those operands:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_vm_opr-3.gif)

The [previous](</blog/scriptable-processor-modules>) blog entry showed how to write processor modules using Python. What if one simply uses the “import” statement to import a full-blown processor module script and use it in the custom data types/formats? 😉

## Displaying resource strings

When reversing MS Windows applications, one can encounter string IDs, but then how to easily and nicely go fetch the data and display it in the disassembly listing?  
Normally, one would have to use a resource editor to extract the string value corresponding to the string id, then to create an enum in IDA for each string ID with a repeatable comment:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_rsrc_enum-3.gif)

That works, but what about writing your own custom format instead:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_rsrc_menu-3.gif)

And then applying it directly without having to use a resource editor to extract the string value, have the custom format do that programmatically for you :

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_rsrc-3.gif)

This is how a resource string custom format handler can look like:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/custdata_rsrc_code-3.gif)

To take a closer look at it, you can [download](</ida_pro/files/custdata_files.zip>) the custom data type handler script along with the source code of the simplevm assembler/disassembler and the C program that was used in this article.

---

## 3. 

**Date:** Posted: Feb 16, 2010  
**Link:** [https://hex-rays.com/blog/scriptable-processor-modules](https://hex-rays.com/blog/scriptable-processor-modules)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Scriptable Processor modules

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/scriptable-processor-modules>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/scriptable-processor-modules>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/scriptable-processor-modules>)

Elias Bachaalany ✦ Posted: Feb 16, 2010

![Scriptable Processor modules](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

One of the new features we are preparing for the next version of IDA is the ability to write processor modules using your favorite scripting language.  
After realizing how handy it is to write [file loaders](</blog/pdf-file-loader-to-extract-and-1>) using scripting languages, we set out to making the same thing for processor modules. As an exercise for this new feature, we implemented a processor module for the [EFI bytecode](<http://en.wikipedia.org/wiki/Extensible_Firmware_Interface>).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/scriptproc_idagraph-3.gif?width=688&height=470&name=scriptproc_idagraph-3.gif)  


## Background

In IDA Pro, a processor module implementation is usually split into four parts:

  1. Processor, assembler, instructions and registers definitions (ins.cpp/.hpp, reg.cpp) 
  2. Decoder (ana.cpp): decodes an instruction into an insn_t structure (the ‘cmd’ global variable) 
  3. Emulation (emu.cpp): emulates instructions, creates appropriate cross references, traces the stack, recognizes code patterns, etc… 
  4. Output (out.cpp): outputs the result to the screen 



The processor module is described using the **processor_t** structure. It holds pointers to registers, instructions, processor module name and other callbacks (ana, emu, out, notify, …).   
The assembler is described using the **asm_t** structure. It holds pointers to the assembler syntax and other callbacks.

## Writing a processor module in Python

To write a processor module in Python, we follow similar logic.

  1. Write the **get_idp_desc()** function. It simply tells IDA what processors the module can handle. 
         
         def get_idp_desc():
             return "EFI Byte code:ebc"
         

The return value means that this processor is named “EFI Byte code” and its shortname is “ebc”. Thus a subsequent call to **set_processor_type(‘ebc’)** from the part of a file loader will succeed.

In case of the **pc** processor module, which can handle many variations of x86 architecture, the string looks like this:
         
         Intel 80x86 processors:8086:80286r:80286p:80386r:80386p:...

  2. Define the registers and instructions: 
         
         # Registers definition
           **proc_Registers** = [
               # General purpose registers
               "R0",
               "R1",
               ...,
               "R10",
               ...
           ]
           # Instructions definition
           **proc_Instructions** = [
               {'name': 'INSN1', 'feature': CF_USE1},
               {'name': 'INSN2', 'feature': CF_USE1 | CF_CHG1}
               ...
           ]
         

  3. Write the **get_idp_def()** function. It should return a dictionary similar to the **processor_t** structure with the processor, assembler, instructions and registers definitions. 
         
         # This function returns the processor module definition
         def get_idp_def():
             return {
                 'version': IDP_INTERFACE_VERSION,
                 # IDP id
                 'id' : 0x8000 + 1,
                 # Processor features
                 'flag' : PR_USE32 | PRN_HEX | PR_RNAMESOK,
                 # short processor names
                 # Each name should be shorter than 9 characters
                 '**psnames** ': ['ebc'],
                 # long processor names
                 # No restriction on name lengthes.
                 '**plnames** ': ['EFI Byte code'],
                 # number of registers
                 'regsNum': len(proc_Registers),
                 # register names
                 'regNames': **proc_Registers** ,
                 # Array of instructions
                 'instruc': **proc_Instructions** ,
                 ....
                 '**assembler** ': \
                 {
                         # flag
                         'flag' : ASH_HEXF3 | AS_UNEQU | AS_COLON | ASB_BINF4 | AS_N2CHR,
                         # Assembler name (displayed in menus)
                         'name': "EFI bytecode assembler",
                         ...
                         # byte directive
                         'a_byte': "db",
                         # word directive
                         'a_word': "dw",
                         # remove if not allowed
                         'a_dword': "dd",
                         ...
                 } # Assembler
             }
         




Now that we finished all the declarations, we can implement the decoder (or analyzer), emulator and the output callbacks.

  * The analyzer looks like this: 
        
        def **ph_ana**():
            """
            Decodes an instruction into the global variable 'cmd'
            Current address is pre-filled in cmd.ea
            """
            cmd = idaapi.cmd
            # take opcode byte
            b = ua_next_byte()
            # decode and fill cmd.Operands etc...
            # ...
            # Return decoded instruction size or zero
            return cmd.size
        

And decoding one instruction/filling the ‘cmd’ variable may look like this:
        
        def decode_JMP8(opbyte, cmd):
            conditional   = (opbyte & 0x80) != 0
            cs            = (opbyte & 0x40) != 0
            cmd.Op1.type  = o_near
            cmd.Op1.dtyp  = dt_byte
            addr          = ua_next_byte()
            cmd.Op1.addr  = (as_signed(addr, 8) * 2) + cmd.size + cmd.ea
            if conditional:
                cmd.auxpref = FL_CS if cs else FL_NCS
            return True
        

  * The emulator: 
        
        # Emulate instruction, create cross-references, plan to analyze
        # subsequent instructions, modify flags etc. Upon entrance to this function
        # all information about the instruction is in 'cmd' structure.
        # If zero is returned, the kernel will delete the instruction.
        def **ph_emu**():
            aux = cmd.auxpref
            Feature = cmd.get_canon_feature()
            if Feature & CF_USE1:
                handle_operand(cmd.Op1, 1)
            if Feature & CF_CHG1:
                handle_operand(cmd.Op1, 0)
            if Feature & CF_USE2:
                handle_operand(cmd.Op2, 1)
            if Feature & CF_CHG2:
                handle_operand(cmd.Op2, 0)
            if Feature & CF_JUMP:
                QueueMark(Q_jumps, cmd.ea)
            # add flow xref
            if Feature & CF_STOP == 0:
                ua_add_cref(0, cmd.ea + cmd.size, fl_F)
            return 1
        

  * The output callback: 
        
        # Generate text representation of an instruction in 'cmd' structure.
        # This function shouldn't change the database, flags or anything else.
        # All these actions should be performed only by ph_emu() function.
        def **ph_out**():
            cmd = idaapi.cmd
            # Init output buffer
            buf = idaapi.init_output_buffer(1024)
            # First, output the instruction mnemonic
            OutMnem()
            # Output the first operand if present (this invokes the ph_outop callback)
            out_one_operand( 0 )
            # Output the rest of the operands
            for i in xrange(1, 3):
                op = cmd[i]
                if op.type == o_void:
                    break
                out_symbol(',')
                OutChar(' ')
                out_one_operand(i)
            # Terminate the output buffer
            term_output_buffer()
            # Emit the line
            cvar.gl_comm = 1
            MakeLine(buf)
        

Note that the previous callbacks are very similar to their C language counterparts. 




Although this feature will not work with the current version of IDA Pro, you can download the [EBC script](</ida_pro/files/scriptproc_ebc.py>) sample for a preview of how a module would look.

If you like this feature, make sure to apply for the beta testing of next version when we announce it!

---

## 4. 

**Date:** Posted: Feb 5, 2010  
**Link:** [https://hex-rays.com/blog/new-idc-improvement-in-ida-pro-5-6](https://hex-rays.com/blog/new-idc-improvement-in-ida-pro-5-6)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# New IDC improvement in IDA Pro 5.6

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/new-idc-improvement-in-ida-pro-5-6>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/new-idc-improvement-in-ida-pro-5-6>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/new-idc-improvement-in-ida-pro-5-6>)

Elias Bachaalany ✦ Posted: Feb 5, 2010

![New IDC improvement in IDA Pro 5.6](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Scripting with IDA Pro has always been a very handy feature, not only when used in scripts but also in expressions, breakpoint conditions, form fields, etc…  
In IDA Pro 5.6 we improved the IDC language and made it more convenient to use by adding objects, exceptions, support for strings with embedded zeroes, string slicing and references.  


## General language improvements

Local variables can now be declared and initialized anywhere within a function:
    
    
    static func1()
    {
      Message("Hello world\n");
      auto s = AskStr("Enter new name", "noname00");
      // ...
      auto i = 0;
      // ....
    }

Global variables can be declared (in a function or in the global scope) with the **extern** keyword:
    
    
    // Global scope
    extern g_count; // Global variables cannot be initialized during declaration
    static main()
    {
      extern g_another_var;
      g_another_var = 123;
      g_count = 1;
    }
    

Functions can be passed around and used as callbacks:
    
    
    static my_func(a,b)
    {
      Message("a=%d, b=%d\n", a, b);
    }
    static main()
    {
      auto f = my_func;
      f(1, 2);
    }
    

Strings can now contain the zero character thus allowing you to use IDC strings like buffers. This is extremely useful when used with [Appcall](</blog/introducing-the-appcall-featur-1>) to call functions that expect buffers:
    
    
    auto s = "\x83\xF9\x00\x74\x10";
    Message("len=%d\n", strlen(s));
    // Construct a buffer with strfill()
    s = strfill('!', 100);
    Message("len=%d\n", strlen(s));
    

Strings can be easily manipulated with slices (Python style):
    
    
    #define QASSERT(x) if (!(x)) { Warning("ASSERT: " #x); }
    auto x = "abcdefgh";
    // get string slice
    QASSERT(x[1] == "b");
    QASSERT(x[2:] == "cdefgh");
    QASSERT(x[:3] == "abc");
    QASSERT(x[4:6] == "ef");
    // set string slice
    x[0]   = "A";           QASSERT(x == "Abcdefgh");
    x[1:3] = "BC";          QASSERT(x == "ABCdefgh");
    // delete part of a string
    x[4:5] = "";            QASSERT(x == "ABCdfgh");
    // patch part of the string with numbers
    x[0:4] = 0x11223344;
    

Strings and numbers are always passed by value in IDC, but now it is possible to pass variables by reference (using the ampersand operator):
    
    
    static incr(a)
    {
      a++;
    }
    static main()
    {
      auto i = 1;
      incr(**&** i);
      Message("i=%d\n", i);
    }
    

Note that objects (described below) are always passed by reference.  


IDC classes

  
Classes can now be declared in IDC. All classes derive from the built-in base class **object** :
    
    
    auto o = object();
    o.ea = here;
    o.flag = 0;

User objects can be defined with the **class** keyword:
    
    
    class testclass
    {
      testclass(name)
      {
        Message("constructing: %s\n", name);
        this.name = name;
      }
      ~testclass()
      {
        Message("destructing: %s\n", this.name);
      }
      set_name(n)
      {
        Message("testclass.set_name -> old=%s new=%s\n", this.name, n);
        this.name = n;
      }
      get_name()
      {
        return this.name;
      }
    }
    static f1(n)
    {
      auto o1 = testclass("object in f1()");
      o1.set_name(n);
    }
    static main()
    {
      auto o2 = testclass("object2 in main()");
      Message("calling f1()\n");
      f1("new object1 name");
      Message("returned from f1()\n");
    }

Which outputs the following when executed:
    
    
    constructing: object2 in main()
    calling f1()
    constructing: object in f1()
    testclass.set_name -> old=object in f1() new=new object1 name
    destructing: new object1 name
    returned from f1()
    destructing: object2 in main()
    

To enumerate all the attributes in an object:
    
    
    auto attr_name;
    auto o = object();
    o.attr1 = "value1";
    o.attr2 = "value2";
    for ( attr_name=firstattr(o); attr_name != 0; attr_name=nextattr(o, attr_name) )
      Message("->%s: %s\n", attr_name, getattr(o, attr_name));
    

If object attribute names are numbers then they can be accessed with the subscript operator:
    
    
    class list
    {
      list()
      {
        this.__count = 0;
      }
      size()
      {
        return this.__count;
      }
      add(e)
      {
        this[this.__count++] = e;
      }
    }
    static main()
    {
      auto a = list();
      a.add("hello");
      a.add("world");
      a.add(5);
      auto i;
      for (i=a.size()-1;i>=0;i--)
        print(a[i]);
    }

IDC classes also support inheritance:
    
    
    class testclass_extender: testclass
    {
      testclass_extender(id): testclass('asdf')
      {
        this.id = id;
      }
      // Override a method and then call the base version
      set_name(n)
      {
        Message("testclass_extender-> %s\n", n);
        testclass::set_name(this, n);
      }
    }
    

They also support getattr/setattr hooking like in Python:
    
    
    class attr_hook
    {
      attr_hook()
      {
        this.id = 1;
      }
      // setattr will trigger for every attribute assignment
      __setattr__(attr, value)
      {
        Message("setattr: %s->", attr);
        print(value);
        setattr(this, attr, value);
      }
      // getattr will only trigger for non-existing attributes
      __getattr__(attr)
      {
        Message("getattr: '%s'\n", attr);
        if ( attr == "magic" )
          return 0x5f8103;
        // Ofcourse this will cause an exception since
        // we try to fetch a non-existing attribute
        return getattr(this, attr);
      }
    }
    

## Exceptions

Normally when a runtime error occurs, the script will abort and the interpret will display the runtime error message. With the use of exception handling, one can catch runtime errors:
    
    
    static test_exceptions()
    {
      // variable to hold the exception information
      auto e;
      try
      {
        auto a = object();
        // Try to read an invalid attribute:
        Message("a.name=%s\n", a.name);
      }
      catch ( e )
      {
        Message("Exception occured. Exception dump follows:\n");
        print(e);
      }
    }
    

Resulting in the following output:
    
    
    Executing function 'main'...
    Exception occured. Exception dump follows:
    object
    description: "No such attribute: object.name"
    file: "C:\\Temp\\ida56.idc"
    func: "test_exceptions"
    line:          91.       5Bh
    pc:          31.       1Fh
    qerrno:        1538.      602h
    

## IDC debugging tips

Last but not least, we would like to mention two useful IDC debugging tips.

The first (we used it previously) involves the print() function:
    
    
    // Print variables in the message window
    // This function print text representation of all its arguments to the output window.
    // This function can be used to debug IDC scripts
    void    print           (...);
    

This function can be very handy when used to print a variable of any type especially objects and all their nested attributes.  


And the second tip involves the use of the command window to evaluate commands. The trick is to type an IDC statement without a terminating semicolon.  
To illustrate, we will first use the DecodeInstruction() with a semicolon:

![idc56_semi.gif](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idc56_semi-3.gif?width=534&height=204&name=idc56_semi-3.gif)

And now the same thing, repeated, without a semicolon would automatically invoke the print() against the returned result, thus:

![idc56_nosemi.gif](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idc56_nosemi-3.gif?width=695&height=390&name=idc56_nosemi-3.gif)

Although we said two debugging tips, but here’s the third: you can use the peroid key (“.”) to jump from an IDA View to the command window and the escape key to return to the IDA View.  
The script snippets used in this blog entry can be downloaded from [here](</ida_pro/files/idc56.idc>).

---

## 5. 

**Date:** Posted: Jan 20, 2010  
**Link:** [https://hex-rays.com/blog/hex-rays-against-aurora](https://hex-rays.com/blog/hex-rays-against-aurora)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-Rays against Aurora

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-against-aurora>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-against-aurora>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-against-aurora>)

Ilfak Guilfanov ✦ Posted: Jan 20, 2010

![Hex-Rays against Aurora](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

As everyone knows, Google and some other companies were under a targeted attack a few days ago. A vulnerability in the Internet Explorer was used to penetrate the computers.  
An IDA user very kindly sent us the following link  
[http://www.avertlabs.com/research/blog/index.php/2010/01/18/an-insight-into-the-aurora-communication-protocol/ ](<http://www.avertlabs.com/research/blog/index.php/2010/01/18/an-insight-into-the-aurora-communication-protocol/>)  
  
As it is visible from the screenshots, the code is somewhat nasty to analysis, because it consists of very short blocks like this:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/zd_asmflat-3.gif)  
Even displayed in the graph mode, the output is still lengthy and messy:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/zd_asm2-3.gif)  
We were pleasantly surprised to see how the decompiler handles this code:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/zd_decomp2-3.gif)  
I renamed some variables and specified their types, but even without this, the output was very readable.  
Just one more example. Virtually all functions are obfuscated with this quite simple technique:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/zd_asm1-3.gif)  
Yet the decompiler output is pleasing to the eye:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/zd_decomp1-3.gif)  
I’m very impressed by the results 🙂  
We are currently completing support for intrinsic functions in the decompiler (it turned out that there are literally hundreds and hundreds of them). Also, SEE based scalar floating point computations will be mapped to high level constructs. It will probably take a few more weeks before the code stabilizes, it won’t be long. Thanks for being patient 🙂

---

## 6. 

**Date:** Posted: Jan 16, 2010  
**Link:** [https://hex-rays.com/blog/practical-appcall-examples](https://hex-rays.com/blog/practical-appcall-examples)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Practical Appcall examples

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/practical-appcall-examples>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/practical-appcall-examples>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/practical-appcall-examples>)

Elias Bachaalany ✦ Posted: Jan 16, 2010

![Practical Appcall examples](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Last week we introduced the new [Appcall feature](</blog/introducing-the-appcall-featur-1>) in IDA Pro 5.6. Today we will talk a little about how it’s implemented and describe some of the uses of Appcall in various scenarios.

## How Appcall works

Given a function with a correct prototype, the Appcall mechanism works like this:

  1. Save the current thread context 
  2. Serialize the parameters (we do not allocate memory for the parameters, we use the debuggee’s stack) 
  3. Modify the input registers in question 
  4. Set the instruction pointer to the beginning of the function to be called 
  5. Adjust the return address so it points to a special area where we have a breakpoint (we refer to it as _control breakpoint_) 
  6. Resume the program and wait until we get an exception or the control breakpoint (inserted in the previous step) 
  7. Deserialize back the input (only for parameters passed by reference) and save the return value 



In the case of a manual Appcall, the debugger module will do all but the last two steps, thus giving you a chance to debug interactively the function in question.  
When you encounter the control breakpoint:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_manual_control-Jun-18-2024-09-07-27-0144-AM.gif)  
you can issue the **CleanupAppcall()** IDC command to restore the previously saved thread context and resume your debugging session.  


## Using the debuggee functions

Sometimes it is useful to call certain functions from inside your debuggee’s context:

  * Functions that you identified as cryptographic functions: encrypt/decrypt/hashing functions 
  * Explicitly call not-so-popular functions: instead of waiting the program to call a certain function, simply call it directly 
  * Change the program logic: by calling certain debuggee functions it is possible to change the logic and the internal state of the program 
  * Extend your program: since Appcall can be used inside the condition expression of a conditional breakpoint, it is possible to extend applications that way 
  * Fuzzing applications: easily fuzz your program on a function level 
  * … 



Let’s take a program that contains a decryption routine that we want to use:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/appcall_xdecrypt-3.gif?width=592&height=138&name=appcall_xdecrypt-3.gif)  
In IDC, you can do something like:
    
    
    auto s_in = "SomeEncryptedBuffer", s_out = strfill(_SizeOfBuffer_);
    decrypt_buffer(**&** s_in, **&** s_out, _SizeOfBuffer_);

Or in Python:
    
    
    # Explicitly create the buffer as a byref object
    s_in = Appcall.byref("SomeEncryptedBuffer")
    # Buffers are always returned byref
    s_out = Appcall.buffer(" ", SizeOfBuffer)
    # Call the debuggee
    Appcall.decrypt_buffer(s_in, s_out, SizeOfBuffer)
    # Print the result
    print "decrypted=", s_out.value
    

## Function level fuzzing

Instead of generating input strings and passing them to the application as command line arguments, input files, etc…it is also possible to test the application on a function level using Appcall.  
It is sufficient to find the functions we want to test, give them appropriate prototypes and Appcall each one of these functions with the desired set of (malformed) input.
    
    
    def fuzz_func1():
      _"""
      Finds functions with one parameter that take a string buffer and tries to see if one
      of these functions will crash if a malformed input was passed
      """_
      # prepare functions search criteria
      tps  = ['LPCWSTR', 'LPCSTR', 'char *', 'const char *', 'wchar_t *']
      tpsf = [1    , 0     , 0     , 0       , 1]
      pat  = r'\((%s)\s*\w*\)' % "|".join(tps).replace('*', r'\*')
      # set Appcall options
      **old_opt = Appcall.set_appcall_options(Appcall.APPCALL_DEBEV)**
      # Enumerate all functions
      for x in Functions():
        # Get the type string
        t = GetType(x)
        if not t:
          continue
        # Try to parse its declaration
        t = re.search(pat, t)
        if not t:
          continue
        # Check if the parameter is a unicode string or not
        is_unicode = tpsf[tps.index(t.group(1))]
        **# Form the input string: here we can generate mutated input
        # and keep on looping until our input pool for this function is exhausted.
        # For demonstration purposes only one string is passed to the Appcalled functions
        s = "A" * 1000**
        # Do the Appcall but protect it with try/catch to receive the exceptions
        try:
          # Create the buffer appropriately
          if is_unicode:
            buf = Appcall.unicode(s)
          else:
            buf = Appcall.buffer(s)
          print "%x: calling. unicode=%d" % (x, is_unicode)
          # Call the function in question
          **r = Appcall[x](buf)**
        except _OSError, e_ :
          **exc_code = idaapi.as_uint32(e.args[0].code)**
          print "%X: Exception %X occurred @ %X. Info: <%s>\n" % (x,
            exc_code, e.args[0].ea, e.args[0].info)
          # stop the test
          break
        except Exception, e:
          print "%x: Appcall failed!" % x
          break
      # Restore Appcall options
      Appcall.set_appcall_options(old_opt)

It is important to enable the APPCALL_DEBEV Appcall option in order to retrieve the last exception that occurred during the Appcall.

### Injecting Libraries in the Debuggee

To inject libraries in the debuggee simply Appcall LoadLibrary():
    
    
    loadlib = Appcall.proto("kernel32_LoadLibraryA", "int __stdcall loadlib(const char *fn);")
    hmod = loadlib("dll_to_inject.dll")

### Set/Get the last error

To retrieve the last error value we can either parse it manually from the TIB or Appcall the GetLastError() API:
    
    
    getlasterror = Appcall.proto("kernel32_GetLastError", "DWORD __stdcall GetLastError();")
    print "lasterror=", getlasterror()
    

Similarly we can do the same to set the last error code value:
    
    
    setlasterror = Appcall.proto("kernel32_SetLastError", "void __stdcall SetLastError(int dwErrCode);")
    setlasterror(5)

### Retrieving the command line value

To retrieve the command line of your program we can either parse it from the PEB or Appcall the GetCommandLineA() API:
    
    
    getcmdline = Appcall.proto("kernel32_GetCommandLineA", "const char *__stdcall getcmdline();")
    print "command line:", getcmdline()
    

### Setting/Resetting events

Sometimes the debugged program may deadlock while waiting on a semaphore or an event. You can manually release the semaphore or signal the event.  
Killing a thread is possible too:
    
    
    releasesem = Appcall.proto("kernel32_ReleaseSemaphore",
      "BOOL __stdcall ReleaseSemaphore(HANDLE hSemaphore, LONG lReleaseCount, LPLONG lpPreviousCount);")
    resetevent = Appcall.proto("kernel32_SetEvent",
      "BOOL __stdcall SetEvent(HANDLE hEvent);")
    termthread = Appcall.proto("kernel32_TerminateThread",
      "BOOL __stdcall TerminateThread(HANDLE hThread, DWORD dwExitCode);")
    

### Change the debuggee’s virtual memory configuration

It is possible to change a memory page’s protection. In the following example we will change the PE header page protection to execute/read/write (normally it is read-only):
    
    
    virtprot = Appcall.proto("kernel32_VirtualProtect",
      "BOOL __stdcall VirtualProtect(LPVOID addr, DWORD sz, DWORD newprot, PDWORD oldprot);")
    r = virtprot(0x400000, 0x1000, Appcall.Consts.PAGE_EXECUTE_READWRITE, Appcall.byref(0));
    print "VirtualProtect returned:", r
    RefreshDebuggerMemory()
    

And if you need to allocate a new memory page:
    
    
    virtalloc = Appcall.proto("kernel32_VirtualAlloc",
      "int __stdcall VirtualAlloc(int addr, SIZE_T sz, DWORD alloctype, DWORD protect);")
    m = virtualalloc(0, Appcall.Consts.MEM_COMMIT, 0x1000, Appcall.Consts.PAGE_EXECUTE_READWRITE)
    RefreshDebuggerMemory()
    

### Load a library and call an exported function

With Appcall it is also possible to load a library, resolve a function address and call it. Let us illustrate with an example:
    
    
    def get_appdata():
        hshell32 = loadlib("shell32.dll")
        if hshell32 == 0:
            print "failed to load shell32.dll"
            return False
        print "%x: shell32 loaded" % hshell32
        # make sure to refresh the debugger memory after loading a new library
        RefreshDebuggerMemory()
        # resolve the function address
        p = getprocaddr(hshell32, "SHGetSpecialFolderPathA")
        if p == 0:
            print "shell32.SHGetSpecialFolderPathA() not found!"
            return False
        # create a prototype
        shgetspecialfolder = Appcall.proto(p,
          "BOOL SHGetSpecialFolderPath(HWND hwndOwner, LPSTR lpszPath, int nFolder, BOOL fCreate);")
        print "%x: SHGetSpecialFolderPath() resolved..."
        # create a buffer
        buf = Appcall.buffer("\x00" * 260)
        # CSIDL_APPDATA  = 0x1A
        if not shgetspecialfolder(0, buf, 0x1A, 0):
            print "SHGetSpecialFolderPath() failed!"
        else:
            print "AppData Path: >%s<" % Appcall.cstr(buf.value)
        return True
    

## Closing words

Appcall has a variety of applications, hopefully it will be handy while solving your day to day reversing problems.  
For your convenience, please download [this](</ida_pro/files/appcall_prototypes.py>) script containing the prototypes of the API functions used in this blog entry.

Please send your suggestions/questions to support@hex-rays.com

---

## 7. 

**Date:** Posted: Jan 12, 2010  
**Link:** [https://hex-rays.com/blog/introducing-the-appcall-feature-in-ida-pro-5-6](https://hex-rays.com/blog/introducing-the-appcall-feature-in-ida-pro-5-6)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Introducing the Appcall feature in IDA Pro 5.6

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-the-appcall-feature-in-ida-pro-5-6>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-the-appcall-feature-in-ida-pro-5-6>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-the-appcall-feature-in-ida-pro-5-6>)

Elias Bachaalany ✦ Posted: Jan 12, 2010

![Introducing the Appcall feature in IDA Pro 5.6](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

In this blog entry we are going to talk about the new Appcall feature that was introduced in IDA Pro 5.6.  
Briefly, Appcall is a mechanism used to call functions inside the debugged program from the debugger or your script as if it were a built-in function. If you’ve used GDB (call command), VS (Immediate window), or Borland C++ Builder then you’re already familiar with such functionality.  
  
[![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_intro-3.jpg>)  
(Screenshot showing how we called three functions (printf, MessageBoxA, GetDesktopWindow) using IDC syntax)

Before diving in, please keep in mind that this blog entry is a short version of the full Appcall reference found [here](<//hex-rays.com/wp-content/uploads/2019/12/debugging_appcall.pdf>). 

  


## Quick start

To start with, we explain the basic concepts of Appcall using the IDC command line:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_printf-3.gif)

It can be called by simply typing:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_idc_printf-3.gif)  
As you notice, we invoked an Appcall by simply treating _printf as if it were a built-in IDC function.  
If you have a function with a mangled name or containing characters that cannot be used as an identifier name in the IDC language:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_imsgbox-3.gif)

then issue the Appcall with this syntax:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_idc_msgbox-3.gif)  
We use the **LocByName** function to get the address of the function given its name, then using the address (which is callable) we issue the Appcall. In two steps this can be achieved with:
    
    
    auto myfunc = LocByName("_my_func@8");
    myfunc("hello", "world");
    

Please note that Appcalls take place in the context of the current thread. If you want to execute in a different thread then switch to the desired thread first.  


## Appcall and IDC

The Appcall mechanism can be used from IDC through the following function:
    
    
    // Call application function
    //      ea - address to call
    //      type - type of the function to call. can be specified as:
    //              - declaration string. example: "int func(void);"
    //              - typeinfo object. example: GetTinfo(ea)
    //              - zero: the type will be retrieved from the idb
    //      ... - arguments of the function to call
    // Returns: the result of the function call
    // If the call fails because of an access violation or other exception,
    // a runtime error will be generated (it can be caught with try/catch)
    // In fact there is rarely any need to call this function explicitly.
    // IDC tries to resolve any unknown function name using the application labels
    // and in the case of success, will call the function. For example:
    //      _printf("hello\n")
    // will call the application function _printf provided that there is
    // no IDC function with the same name.
    anyvalue Appcall(ea, type, ...);
    

The Appcall IDC function requires you to pass a function address, function type information and the parameters (if any):
    
    
    auto p = LocByName("_printf");
    auto ret = Appcall(p, GetTinfo(p), "Hello %s\n", "world");
    

We’ve seen so far how to call a function if it already has type information, now suppose we have a function that does not:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_ifindwindowa-3.gif)

Before calling this function with Appcall() we need first to get the type information (stored in a typeinfo object) by calling ParseType() and then pass the function ea and type to Appcall():
    
    
    auto p = ParseType("long __stdcall FindWindow(const char *cls, const char *wndname)", 0);
    Appcall(LocByName("user32_FindWindowA"), p, 0, "Untitled - Notepad");
    

Note that we used **ParseType()** function to construct a typeinfo object that we can pass to Appcall(), however it is possible to permanently set the prototype of a function, thus:
    
    
    SetType(LocByName("user32_FindWindowA"),
    "long __stdcall FindWindow(const char *cls, const char *wndname)");

## Passing arguments by reference

To pass function arguments by reference, it suffices to use the **& amp** symbol as in the C language.

  * For example to call this function:


    
    
    void ref1(int *a)
    {
    if (a == NULL)
    return;
    int o = *a;
    int n = o + 1;
    *a = n;
    printf("called with %d and returning %d\n", o, n);
    }
    

We can use this code from IDC:
    
    
    auto a = 5;
    Message("a=%d", a);
    ref1(**&** a);
    Message(", after the call=%d\n", a);

  * To call a C function that takes a string buffer and modifies it:


    
    
    /* C code */
    int ref2(char *buf)
    {
    if (buf == NULL)
    return -1;
    printf("called with: %s\n", buf);
    char *p = buf + strlen(buf);
    *p++ = '.';
    *p = '\0';
    printf("returned with: %s\n", buf);
    int n=0;
    for (;p!=buf;p--)
    n += *p;
    return n;
    }
    

We need to create a buffer and pass it, thus:
    
    
    auto s = strfill('\x00', 20); // create a buffer of 20 characters
    s[0:5] = "hello"; // initialize the buffer
    ref2(&s); // call the function and pass the string by reference
    if (s[5] != ".")
    Message("not dot\n");
    else
    Message("dot\n");
    

## __usercall calling convention

It is possible to Appcall functions with non standard calling conventions, such as routines written in assembler that expect parameters in various registers and so on.  
One way is to describe your function with the **__usercall** calling convention.

Consider this function:
    
    
    /* C code */
    // eax = esi - edi
    int __declspec(naked) asm1()
    {
    __asm
    {
    mov eax, esi
    sub eax, edi
    ret
    }
    }
    

And from IDC:
    
    
    auto p = ParseType("int __usercall asm1<eax>(int a<esi>, int b<edi>);", 0);
    auto r = Appcall(LocByName("_asm1"), p, 5, 2);
    Message("The result is: %d\n", r);
    

## Variable argument functions

In C:
    
    
    int va_altsum(int n1, ...)
    {
    va_list va;
    va_start(va, n1);
    int r = n1;
    int alt = 1;
    while ( (n1 = va_arg(va, int)) != 0 )
    {
    r += n1*alt;
    alt *= -1;
    }
    va_end(va);
    return r;
    }

And in IDC:
    
    
    auto result = va_altsum(5, 4, 2, 1, 6, 9, 0);

## Calling functions that can cause exceptions

Exceptions may occur during an Appcall. To capture them, you can use the try/catch in IDC:
    
    
    auto e;
    try
    {
    AppCall(some_func_addr, func_type, arg1, arg2);
    // Or equally:
    // some_func_name(arg1, arg2);
    }
    catch (e)
    {
    // Exception occured .....
    }
    

The exception object “e” will be populated with the following fields:

  * description: description text generated by the debugger module while it was executing the Appcall 
  * func: The IDC function name where the exception happened. 
  * line: The line number in the script 
  * qerrno: The internal code of last error occured 



For example, you could get something like this:
    
    
      description: "Appcall: The instruction at 0x401F93 referenced memory at 0x5.
    The memory could not be read"
    file: "<internal>"
    func: "___idc0"
    line: 4
    qerrno: 92
    

In some cases the exception object will contain more information.  


## Specifying Appcall options

Appcall can be configured with SetAppcallOptions(), by passing the following option(s):

  * APPCALL_MANUAL: Only set up the appcall, do not run it (you should call CleanupAppcall() when finished). Please Refer to Manual Appcall section for more information. 
  * APPCALL_DEBEV: If this bit is set, exceptions during appcall will generate IDC exceptions with full information about the exception. Please refer to Capturing exception debug events section for more information. 



It is possible to retrieve the Appcall options, change them and then restore them back. To retrieve the options use the **GetAppcallOptions()**.  
Please note that Appcall option is saved in the database so if you set it once it will retain its value as you save and load the database.  


## Manual Appcall

So far we’ve seen how to issue an Appcall and capture the result from the script, but what if we only want to setup the environment and manually step through a function?  
This can be achieved with manual Appcall. The manual Appcall mechanism can be used to save the current execution context, execute another function in another context and then pop back the previous context and continue debugging from that point. Let us directly illustrate manual Appcall with a real life scenario:

  1. You are debugging your application 
  2. You discover a buggy function (foo()) that misbehaves when called with certain arguments: foo(0xdeadbeef) 
  3. Instead of waiting until the application calls foo() with the desired arguments that can cause foo() to misbehave, you can manually call foo() with the desired arguments, trace the function 
  4. Finally, one calls CleanupAppcall() to restore the execution context 



To illustrate, let us take the ref1 function and call it with an invalid pointer:

  1. SetAppcallOptions(APPCALL_MANUAL); // Set manual Appcall mode 
  2. ref1(6); // call the function with an invalid pointer 



Directly after doing that, IDA will switch to the function and from that point on we can debug:  
![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

When we reach the end of the function:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_manual_ref1_end-3.gif)  
and trace beyond the return instruction, we expect to see something like this:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_manual_control-Jun-18-2024-08-55-59-7491-AM.gif)  
This is the control code that we use to determine the end of an Appcall. It is at this point that one should call CleanupAppcall() to return to the previous execution context:  
  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_manual_somectx-3.gif)

## Capturing exception debug events

We previously illustrated that we can capture exceptions that occur during an Appcall, but that is not enough if we want to learn more about the nature of the exception from the operating system point of view.  
  
It would be better if we could somehow get the last **debug_event_t** that occured inside the debugger module. This is possible if we use the APPCALL_DEBEV option. Let us repeat the previous example but with the APPCALL_DEBEV option enabled:
    
    
    auto e;
    try
    {
    SetAppcallOptions(APPCALL_DEBEV); // Enable debug event capturing
    ref1(6);
    }
    catch (e)
    {
    // Exception occured. This time "e" is populated with debug_event_t fields (check idd.hpp)
    }
    

And in this case, if we dump the exception object’s contents, we get these attributes:
    
    
    can_cont: 1
    code:  C0000005h
    ea:    401F93h
    eid:    40h _(from idd.hpp: EXCEPTION = 0x00000040 Exception)_
    file: ""
    func: "___idc0"
    handled: 1
    info: "The instruction at 0x401F93 referenced memory at 0x6. The memory could not be read"
    line: 4h
    pid:  123Ch
    ref:  6h
    tid:  1164h

## Appcall and Python

The Appcall concept remains the same between IDC and Python, nonetheless Appcall/Python has a different syntax (using references, unicode strings, etc, etc…)

The Appcall mechanism is provided by idaapi module through the Appcall variable. To issue an Appcall:
    
    
    Appcall.printf("Hello world!\n");

One can take a reference to an Appcall:
    
    
    printf = Appcall.printf
    # ...later...
    printf("Hello world!\n");
    

  * If you have a function with a mangled name or with characters that cannot be used as an identifier name in the Python language:


    
    
    findclose     = Appcall["__imp__FindClose@4"]
    getlasterror  = Appcall["__imp__GetLastError@0"]
    setcurdir     = Appcall["__imp__SetCurrentDirectoryA@4"]
    

  * In case you want to redefine the prototype of a given function, then use the Appcall.proto(func_name or func_ea, prototype_string):


    
    
    # pass an address name and Appcall.proto() will resolve it
    loadlib = Appcall.proto("__imp__LoadLibraryA@4",
    "int (__stdcall *LoadLibraryA)(const char *lpLibFileName);")
    # Pass an EA instead of a name
    freelib = Appcall.proto( LocByName("__imp__FreeLibrary@4"),
    "int (__stdcall *FreeLibrary)(int hLibModule);")
    

  * To pass unicode strings you need to use the Appcall.unicode() function:


    
    
        getmodulehandlew    = Appcall.proto("__imp__GetModuleHandleW@4",
    "int (__stdcall *GetModuleHandleW)(LPCWSTR lpModuleName);")
    hmod = getmodulehandlew(Appcall.**unicode**("kernel32.dll"))
    

  * To define a prototype and then later assign an address so you can issue an Appcall:


    
    
    # Create a typed object (no address is associated yet)
    virtualalloc = Appcall.**typedobj**(
    "int __stdcall VirtualAlloc(int lpAddress, SIZE_T dwSize, DWORD flAllocationType, DWORD flProtect);")
    # Later we have an address, so we pass it:
    virtualalloc.**ea** = LocByName("kernel32_VirtualAlloc")
    # Now we can Appcall:
    ptr = virtualalloc(0, Appcall.Consts.MEM_COMMIT, 0x1000, Appcall.Consts.PAGE_EXECUTE_READWRITE)
    

Before we conclude (if you read so far;)), here’s a small [script](</ida_pro/files/appcall_man.idc>) that can be used to initiate and terminate Appcalls using hotkeys. If you want to have this script load everytime you start IDA then put its contents in idc\ida.idc file.

Here’s a simple scenario where manual Appcalls can be handy:

  * You’re debugging a program and then you require to debug another function then continue debugging the current function  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_deb_1-3.gif)

  * You press Ctrl-Alt-F9 to initiate a manual Appcall and you type the desired function name and arguments  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_deb_init-3.gif)

  * The debugger will switch to the new function and you start tracing the new function  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/appcall_deb_2-3.gif)

  * Once you’re done to return to your previous function you terminate the Appcall by pressing Ctrl-Alt-F10 



If you want to temporary start tracing from the current cursor location then use Ctrl-Alt-F4 to start a manual Appcall. Use then Ctrl-Alt-F10 to return to previous execution context. 

Remember, Appcall can do more than what is illustrated in this blog entry, make sure you refer to the [Appcall manual](<//hex-rays.com/wp-content/uploads/2019/12/debugging_appcall.pdf>) for other advanced topics.

---

## 8. 

**Date:** Posted: Jan 8, 2010  
**Link:** [https://hex-rays.com/blog/debugging-arm-code-snippets-in-ida-pro-5-6-using-qemu-emulator](https://hex-rays.com/blog/debugging-arm-code-snippets-in-ida-pro-5-6-using-qemu-emulator)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Debugging ARM code snippets in IDA Pro 5.6 using QEMU emulator

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/debugging-arm-code-snippets-in-ida-pro-5-6-using-qemu-emulator>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/debugging-arm-code-snippets-in-ida-pro-5-6-using-qemu-emulator>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/debugging-arm-code-snippets-in-ida-pro-5-6-using-qemu-emulator>)

I 

Igor Skochinsky ✦ Posted: Jan 8, 2010

![Debugging ARM code snippets in IDA Pro 5.6 using QEMU emulator](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

## Introduction

IDA Pro 5.6 has a new feature: automatic running of the QEMU emulator. It can be used to debug small code snippets directly from the database.  
In this tutorial we will show how to dynamically run code that can be difficult to analyze statically.

## Target

As an example we will use shellcode from the article [“Alphanumeric RISC ARM Shellcode”](<http://www.phrack.com/issues.html?issue=66&id=12>) in Phrack 66.  
It is self-modifying and because of alphanumeric limitation can be quite hard to undestand. So we will use the debugging feature to decode it.

The sample code is at the bottom of the article but here it is repeated:
    
    
    80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR80AR
    80AR80AR80AR80AR80AR80AR80AR80AR80AR00OB00OR00SU00SE9PSB9PSR0pMB80SBcACP
    daDPqAGYyPDReaOPeaFPeaFPeaFPeaFPeaFPeaFPd0FU803R9pCRPP7R0P5BcPFE6PCBePFE
    BP3BlP5RYPFUVP3RAP5RWPFUXpFUx0GRcaFPaP7RAP5BIPFE8p4B0PMRGA5X9pWRAAAO8P4B
    gaOP000QxFd0i8QCa129ATQC61BTQC0119OBQCA169OCQCa02800271execme22727

Copy this text to a new text file, **remove all line breaks** (i.e. make it a single long line) and save. Then load it into IDA.

## Loading binary files into IDA

IDA displays the following dialog when it doesn’t recognize the file format (as in this case):

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_load-3.gif)

Since we know that the code is for ARM processor, choose ARM in the “Processor type” dropdown and click Set. Then click OK. The following dialog appears:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_ram-3.gif)

When you analyze a real firmware dumped from address 0, these settings are good.  
However, since our shellcode is not address-dependent, we can choose any address. For example, enter 0x10000 in “ROM start address” and “Loading address” fields.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_disass1-3.gif)

IDA doesn’t know anything about this file so it didn’t create any code. Press C to start disassembly.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_disass2-3.gif)

## Configuring QEMU

Before starting debug session, we need to set up automatic running of QEMU.

  1. Download a recent version of QEMU with ARM support (e.g. from http://homepage3.nifty.com/takeda-toshiya/qemu/index.html).  
If qemu-system-arm.exe is in a subdirectory, move it next to qemu.exe and all DLLs.  
**Note** : if you’re running Windows 7 or Vista, it’s recommended to use QEMU 0.11 or 0.10.50 (“Snapshot” on Takeda Toshiya’s page),  
as the older versions listen for GDB connections only over IPv6 and IDA can’t connect to it.
  2. Edit cfg/gdb_arch.cfg and change “set QEMUPATH” line to point to the directory where you unpacked QEMU. Change “set QEMUFLAGS” if you’re using an older version.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_cfg-3.gif)

  3. In IDA, go to Debug-Debugger options…, Set specific options.
  4. Enable “Run a program before starting debugging”.
  5. Click “Choose a configuration”. Choose Versatile or Integrator board. The command line and Initial SP fields will be filled in.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_options-3.gif)

  6. Memory map will be filled from the config file too. You can edit it by clicking the “Memory map” button, or from the Debugger-Manual memory regions menu item.



Now on every start of debugging session QEMU will be started automatically.

## Executing the code

By default, initial execution point is the entry point of the database. If you want to execute some other part of it, there are two ways:

  1. Select the code range that you want to execute, or
  2. Rename starting point **ENTRY** and ending point **EXIT** (convention similar to Bochs debugger)



In our case we do want to start at the entry point so we don’t need to do anything. If you press F9 now, IDA will write the database contents to an ELF file (**database.elfimg**) and start QEMU, passing the ELF file name as the “kernel” parameter.  
QEMU will load it, and stop at the initial point.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_start-3.gif)

Now you can step through the code and inspect what it does. Most of the instructions “just work”, however, there is a syscall at 0x0010118:

  
ROM:00010118 SVCMI 0x414141  


Since the QEMU configuration we use is “bare metal”, without any operating system, this syscall won’t be handled. So we need to skip it.

  1. Navigate to 010118 and press F4 (Run to cursor). Notice that the code was changed (patched by preceding instructions):

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_syscall-3.gif)  
(Incidentally, 0x9F0002 is sys_cacheflush for ARM Linux.)

  2. Right-click next line (0001011C) and choose Set IP.
  3. Press F7 three times. Once you’re on BXPL R6 line, IDA will detect the mode switch and add a change point to Thumb code:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_bx-3.gif)  
However, the following, previously existing code will (incorrectly) stay in ARM mode. We need to fix that.

  4. Go to 01012C and press U (Undefine).
  5. Press Alt-G (Change Segment Register Value) and set value of T to 1. The erroneous CODE32 will disappear.
  6. Go back to 00010128 and press C (Make code). Nice Thumb code will appear:

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_thumb-3.gif)

  7. In Thumb code, there is another syscall at 00010152. If you trace or run until it, you can see that R7 becomes 0xB (sys_execve) and R0 points to 00010156.

If you undefine code at 00010156 and make it a string (‘A’ key), it will look like following:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_thumb2-3.gif)  
Thus we can conclude that the shellcode tries to execute a file at the path “/execme”. 




**Hint** : if the code you’re investigating has many syscalls and you don’t want to handle them one by one, put a breakpoint at the address 0000000C (ARM’s vector for syscalls). Return address will be in LR.

## Saving results to database

If you want to keep the modified code or data for later analysis, you’ll need to copy it to the database. For that:

  1. Edit segment attributes (Alt-S) and make sure that segments with the data you need have the “Loader segment” attribute set.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/qemu_segm-3.gif)

  2. Choose Debugger-Take memory snapshot and answer “Loader segments”.

**Note** : if you answer “All segments”, IDA will try to read the whole RAM segment (usually 128M) which can take a VERY long time.

  3. Now you can stop the debugging and inspect the new data.  
**Note** : this will update your database with the new data and discard the old. _Repeated execution probably will not be correct._
__

__




This concludes our short tutorial.

Happy debugging!   
Please send any comments or questions to [support@hex-rays.com](<mailto:support@hex-rays.com>)

---

## 9. 

**Date:** Posted: Jan 6, 2010  
**Link:** [https://hex-rays.com/blog/pdf-file-loader-to-extract-and-analyse-shellcode](https://hex-rays.com/blog/pdf-file-loader-to-extract-and-analyse-shellcode)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# PDF file loader to extract and analyse shellcode

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/pdf-file-loader-to-extract-and-analyse-shellcode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/pdf-file-loader-to-extract-and-analyse-shellcode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/pdf-file-loader-to-extract-and-analyse-shellcode>)

Elias Bachaalany ✦ Posted: Jan 6, 2010

![PDF file loader to extract and analyse shellcode](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

One of the new features in [IDA Pro 5.6](<http://www.hex-rays.com/idapro/56/index.htm>) is the possibility to write file loaders using scripts such as IDC or Python.  
To illustrate this new feature, we are going to explain how to write a file loader using IDC and then we will write a file loader (in Python) that can extract shell code from malicious PDF files.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_loader-4.gif?width=497&height=462&name=pdf_loader-4.gif)  


## Writing a loader script for BIOS images

Before writing file loaders we need to understand the file format in question. For demonstration purposes we chose to write a loader for BIOS image files statisfying these conditions:

  * Should be no more than 64kb in size 
  * Contain the far jump instruction at 0xFFF0 
  * Contain a date string at 0xFFF5 



Each file loader should define at least the two functions: accept_file() and load_file(). The former decides whether the file format is supported and the latter loads the previously accepted file and populates the database.
    
    
    // Verify the input file format
    //      li - loader_input_t object. it is positioned at the file start
    //      n  - invocation number. if the loader can handle only one format,
    //           it should return failure on n != 0
    // Returns: if the input file is not recognized
    //              return 0
    //          else
    //              return object with 2 attributes:
    //                 format: description of the file format
    //                 options:1 or ACCEPT_FIRST. it is ok not to set this attribute.
    static accept_file(li, n)
    {
      if ( n )
        return 0; // this loader supports only one format
      // we support max 64K images
      if ( li.size() > 0x10000 )
        return 0;
      li.seek(-16, SEEK_END);
      if ( li.getc() != 0xEA ) // jmp?
        return 0;
      li.seek(-2, SEEK_END);
      // reasonable computer type?
      if ( (li.getc() & 0xF0) != 0xF0 )
        return 0;
      auto buf;
      li.seek(-11, SEEK_END);
      li.read(&buf, 9);
      // 06/03/08
      if ( buf[2] != "/" || buf[5] != "/" || buf[8] != "\x00" )
        return 0;
      // accept the file
      return "BIOS Image"; // description of the file format
    }
    

The accept_file() will be called many times by IDA kernel starting with _n_ =0, n=1, n=2, … until it returns zero. This allows you to handle multiple formats present in the same input file.  
For example, PE files can be loaded as MS-DOS MZ EXE files or as PE files. The PE file loader plugin does something like this:
    
    
    if (n == 0)
      return "MZ executable";
    else if (n == 1)
    {
      // check if it is a PE file
      // ....
      return "PE executable";
    }
    else
      return 0;
    

The _li_ parameter is an instance of _loader_input_t_ described in idc.idc (for IDC) and idaapi.py (for IDAPython). This class allows you to seek and read from the input file. 

The load_file() will receive a _loader_input_t_ instance, the format name previously returned by the accept_file() and the loading flags in _neflags_. This flag can be tested against the NEF_MAN constant to detect whether the user checked the “Manual Load” option while loading the new file.  
These are the main responsibilities of load_file():

  * Set the processor corresponding to the input file 
  * Create segments 
  * Add entry points 
  * Add fixups 
  * Create import/export segments 
  * etc… 


    
    
    // Load the file into the database
    //      li      - loader_input_t object. it is positioned at the file start
    //      neflags - combination of NEF_... bits describing how to load the file
    //                probably NEF_MAN is the most interesting flag that can
    //                be used to select manual loading
    //      format  - description of the file format
    // Returns: 1 - means success, 0 - failure
    static load_file(li, neflags, format)
    {
      auto base = 0xF000;
      auto start = base << 4;
      auto size = li.size();
      SetProcessorType("metapc", SETPROC_ALL);
      // copy bytes to the database
      loadfile(li, 0, base<<4, size);
      // create a segment
      AddSeg(start, start+size, base, 0, saRelPara, scPub);
      // set the entry registers
      SetLongPrm(INF_START_IP, size-16);
      SetLongPrm(INF_START_CS, base);
      return 1;
    }
    

This script (bios_image.idc) is installed with IDA Pro 5.6 in the loaders directory.

Now that we know how to write a simple file loader using a scripting language, let us write a real life file loader that assists us in extracting shellcode from malicious PDF files.

## PDF shellcode extractor

The purpose of this article is not to explain how PDF exploits work, however we will explain the general idea as we write the file loader. If you need more information please check [Didier Steven’s](<http://blog.didierstevens.com>) site and this blog [entry](<http://blog.didierstevens.com/2008/10/20/analyzing-a-malicious-pdf-file/>), also check [Jon Paterson and Dennis Elser](<http://www.avertlabs.com/research/blog/index.php/2009/10/13/latest-pdf-zero-day-leads-to-exploit-egg-hunt/>) blog entry showing how they extracted the shellcode manually and loaded it into IDA for analysis.

In this section we are going to write a very basic shellcode extractor that handles a couple of simple cases.

The first case is when the PDF document contains an embedded JavaScript:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_expl1-4.gif?width=567&height=485&name=pdf_expl1-4.gif)  
And the second case when an object refers to another object containing the compressed script:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_expl2-4.gif?width=652&height=331&name=pdf_expl2-4.gif)  
Object 31 refers to object 32 (compressed with DEFLATE algorithm) and contains the actual script that exploits a given vulnerability in the PDF reader.  
After taking everything between stream/endstream inside object 32 and passing it to gzip.decompress() we get:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_expl2_js-4.gif?width=512&height=474&name=pdf_expl2_js-4.gif)  
In both cases the shellcode is passed to the unescape() and we can use that as a very basic mechanism to extract the shellcode.  
Before writing the code let us summarize what we need to do:

  1. Find potential JavaScript: 
     * Scan the PDF document for objects that reference compressed JS streams: 
       1. Find the referencing object 
       2. Find the referred object 
       3. Take the stream and decompress it 
     * Or scan the PDF document for objects that contains embedded JS and take the JS as-is 
  2. Find all calls to unescape() and extract its parameters. These parameters could be potential shellcode 
  3. Decode the unescape parameter into a byte string 
  4. Create a segment and load the shellcode into the segment 



## Extracting JS scripts from the PDF

To look for embedded JS scripts we call **find_embedded_js()** that employs a regular expression:
    
    
    def find_embedded_js(str):
        js = re.finditer('**\/S\s*\/JavaScript\s*\/JS \((.+?) >>**', str, re.MULTILINE | re.DOTALL)
    

Once we have a match we remember it without further processing.

To look for compressed JavaScript objects we first call **find_js_ref_streams()** that also employs a regular expression to locate all objects that refer to another JavaScript object:
    
    
    def find_js_ref_streams(str):
        js_ref_streams = re.finditer('**\/S\s*\/JavaScript\/JS (\d+) (\d+) R** ', str)
    

We then use the **find_obj()** to find the body of the refered object (that contains the compressed JavaScript):
    
    
    def find_obj(str, id, ver):
        stream = re.search('%d %d obj(.*?)endobj' % (id, ver), str, re.MULTILINE | re.DOTALL)
        if not stream:
            return None
        return str[stream.start(1):stream.end(1)]
    

And finally we call **decompress_stream()** to decompress the referred stream:
    
    
    def decompress_stream(str):
        if str.find('Filter[/FlateDecode]') == -1:
            return None
        m = re.search('stream\s*(.+?)\s*endstream', str, re.DOTALL | re.MULTILINE)
        if not m:
            return None
        # Decompress and return
        return zlib.decompress(m.group(1))
    

## Extracting potential shellcode in the JS scripts

Since this article is for demonstration purposes only, we will assume that the shellcode is always enclosed in the unescape() call. For this we simply convert back the %uXXYY or %XX format strings back to the corresponding byte characters:
    
    
    def extract_shellcode(lines):
        p = 0
        shellcode = [] # accumulate shellcode
        while True:
            p = lines.find('unescape("', p)
            if p == -1:
                break
            e = lines.find(')', p)
            if e == -1:
                break
            expr = lines[p+9:e]
            data = []
            for i in xrange(0, len(expr)):
                if expr[i:i+2] == "%u":
                    i += 2
                    data.extend([chr(int(expr[i+2:i+4], 16)), chr(int(expr[i:i+2], 16))])
                    i += 4
                elif expr[i] == "%":
                    i += 1
                    data.append(int(expr[i:i+2], 16))
                    i += 2
            # advance the match pos
            p += 8
            shellcode.append("".join(data))
        # That's it
        return shellcode
    

Now we can glue all those helper functions to create one function that returns the shellcode:
    
    
    def extract_pdf_shellcode(buf):
        ret = []
        # find all JS stream references
        r = find_js_ref_streams(buf)
        for id, ver in r:
            # extract the JS stream object
            obj = find_obj(buf, id, ver)
            # decode the stream
            stream = decompress_stream(obj)
            # extract shell code
            scs = extract_shellcode(stream)
            i = 0
            for sc in scs:
                i += 1
                ret.append([id, ver, i, sc])
        # find all embedded JS
        r = find_embedded_js(buf)
        if r:
            ret.extend(r)
        return ret
    

## Writing the file loader

Now that we have all the needed functions to open a PDF and extract all shellcode, let us write a file loader so that we can use IDA to open a malicious PDF file. First we start with the accept_file():
    
    
    def accept_file(li, n):
        # we support only one format per file
        if n > 0:
            return 0
        li.seek(0)
        if li.read(5) != '%PDF-':
            return 0
        buf = read_whole_file(li)
        r = extract_pdf_shellcode(buf)
        if not r:
            return 0
        return 'PDF with shellcode'
    

As you can see, there is nothing special about this function: (1) check PDF file signature (2) check if we found at least one shellcode

And the load_file() will populate all the extracted shellcode into the database:
    
    
    def load_file(li, neflags, format):
        # Select the PC processor module
        idaapi.set_processor_type("metapc", SETPROC_ALL|SETPROC_FATAL)
        buf = read_whole_file(li)
        r = extract_pdf_shellcode(buf)
        if not r:
            return 0
        # Load all shellcode into different segments
        start = 0x10000
        seg = idaapi.segment_t()
        for id, ver, n, sc in r:
            size = len(sc)
            end  = start + size
            # Create the segment
            seg.startEA = start
            seg.endEA   = end
            seg.bitness = 1 # 32-bit
            idaapi.add_segm_ex(seg, "obj_%d_%d_%d" % (id, ver, n), "CODE", 0)
            # Copy the bytes
            idaapi.mem2base(sc, start, end)
            # Mark for analysis
            AutoMark(start, AU_CODE)
            # Compute next loading address
            start = ((end / 0x1000) + 1) * 0x1000
        # Select the bochs debugger
        LoadDebugger("bochs", 0)
        return 1
    

## Testing the script

Let us copy the PDF loader [script](</ida_pro/files/pdf-ldr.py>) to IDA / loaders directory and open a malicious PDF file:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_loader2-4.gif?width=389&height=461&name=pdf_loader2-4.gif)  
After the file is loaded we can directly see the shellcode:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_sc2-4.gif?width=464&height=254&name=pdf_sc2-4.gif)  
And for the other malware sample, after we load it with IDA:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_sc1-4.gif?width=548&height=439&name=pdf_sc1-4.gif)  
We notice that it contains a decoder that decodes the rest of the shellcode:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_sc2_enc-4.gif?width=612&height=210&name=pdf_sc2_enc-4.gif)  
To uncover the code we can use the [Bochs debugger](</blog/bochs-plugin-goes-alpha>) in the IDB operation mode by selecting the range of code we want to emulate and pressing F9:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_sc2_bochs-4.gif?width=385&height=466&name=pdf_sc2_bochs-4.gif)  
After the decoding is finished we can take a memory snapshot to save the decoded shellcode.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pdf_sc2_dec-4.gif?width=714&height=358&name=pdf_sc2_dec-4.gif)

Please download the code from [here](</ida_pro/files/pdf-ldr.py>)

Special thanks to [Didier Stevens](<http://blog.didierstevens.com/>) for his [free PDF tools](<http://blog.didierstevens.com/programs/pdf-tools/>) and for providing some samples.

---

