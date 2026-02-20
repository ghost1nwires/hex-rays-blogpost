# Hex-Rays Blog – Page 41

**Archived:** 2026-02-20 12:22
**URL:** https://hex-rays.com/blog/page/41

---

## 1. 

**Date:** Posted: Apr 15, 2014  
**Link:** [https://hex-rays.com/blog/extending-idapython-in-ida-6-5-be-careful-about-the-gil](https://hex-rays.com/blog/extending-idapython-in-ida-6-5-be-careful-about-the-gil)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Extending IDAPython in IDA 6.5: Be careful about the GIL

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/extending-idapython-in-ida-6-5-be-careful-about-the-gil>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/extending-idapython-in-ida-6-5-be-careful-about-the-gil>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/extending-idapython-in-ida-6-5-be-careful-about-the-gil>)

Arnaud Diederen ✦ Posted: Apr 15, 2014

![Extending IDAPython in IDA 6.5: Be careful about the GIL](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

# Target audience

You may want to read this if you have been writing an IDA C++ plugin, that itself uses the CPython runtime.

### Prior art

In 2010, Elias Bachaalany wrote a blog post about extending IDAPython: <http://www.hexblog.com/?p=126>  
Note that this is not about writing your own plugins in Python. Rather, that blog post instruct on how you may want to write a plugin that, itself, links against libpython27, so that it interacts nicely with IDAPython (which links against the same libpython27 library).

# Before 6.5

The following plugin body was enough to have a working C++ plugin, that will execute a simple Python statement:  
`  
void idaapi run(int)  
{  
PyRun_SimpleString("print \"Hello from plugin\"");  
}  
`

### Notes about the previous example

It is worth pointing out that, whenever one wants to execute _any_ CPython code (such as the ‘PyRun_SimpleString’ call above), one must have acquired what is called in CPython-land as the Global Interpreter Lock, or **GIL** : <https://wiki.python.org/moin/GlobalInterpreterLock>.  
It may seem strange, therefore, that the code above even works, since there’s nothing that even remotely looks like a “GIL-acquiring” sequence of instructions.  
The reason the code above used to work is because IDAPython was not releasing the GIL: it was acquiring it at startup-time, and never releasing it. Ever.

# IDA 6.5 & later versions

In IDA 6.5, many efforts have been made to improve the use of multithreading in IDAPython: The IDAPython plugin acquires the GIL just in time to execute CPython code and then releases it, as it should. This will permit a reliable use of threading in IDAPython (in some circumstances, when using multithreading, IDA < 6.5 could freeze because of the GIL not being released.)  
Starting with 6.5, such a plugin would require to be written in the following manner:  
`  
void idaapi run(int)  
{  
PyGILState_STATE state = PyGILState_Ensure(); // Acquire the GIL  
PyRun_SimpleString("print \"Hello from plugin\"");  
PyGILState_Release(state); // Release the GIL  
}  
`  
Note the use of the CPython-defined PyGILState_* helpers.

### Existing plugins

Unfortunately, we couldn’t do anything in this case to ensure backwards-compatibility: existing plugins will have to be adapted & recompiled. Sorry about this, but we just could not find a reasonable solution to maintain bw-compat in this case; hence this blog post.  
So far, we have received a grand total of 1 report for this issue, so this doesn’t appear to impact many of our users. But in case you are running into problems with your previously-compiled CPython-aware plugin, hopefully this will have explained why.

---

## 2. 

**Date:** Posted: Apr 1, 2014  
**Link:** [https://hex-rays.com/blog/x64-decompiler-not-far-away](https://hex-rays.com/blog/x64-decompiler-not-far-away)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# x64 decompiler not far away

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/x64-decompiler-not-far-away>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/x64-decompiler-not-far-away>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/x64-decompiler-not-far-away>)

Ilfak Guilfanov ✦ Posted: Apr 1, 2014

![x64 decompiler not far away](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Just a short post to show you the current state of the x64 decompiler. In fact, it already mostly works but we still have to solve some minor problems. Let us consider this source code:
    
    
    struct color_t
    {
      short red;
      short green;
      short blue;
      short alpha;
    };
    extern color_t lighten(color_t c);
    color_t func(int red, int green, int blue, int alpha)
    {
      color_t c;
      c.red = red;
      c.green = green;
      c.blue = blue;
      c.alpha = alpha;
      return lighten(c);
    }

After compilation we get the following binary code:
    
    
    .text:0000000000000000 ?func@@YA?AUcolor_t@@HHHH@Z proc near
    .text:0000000000000000
    .text:0000000000000000 c = color_t ptr -18h
    .text:0000000000000000 var_10 = qword ptr -10h
    .text:0000000000000000 arg_0 = dword ptr 8
    .text:0000000000000000 arg_8 = dword ptr 10h
    .text:0000000000000000 arg_10 = dword ptr 18h
    .text:0000000000000000 arg_18 = dword ptr 20h
    .text:0000000000000000
    .text:0000000000000000 mov [rsp+arg_18], r9d ; $LN3
    .text:0000000000000005 mov [rsp+arg_10], r8d
    .text:000000000000000A mov [rsp+arg_8], edx
    .text:000000000000000E mov [rsp+arg_0], ecx
    .text:0000000000000012 sub rsp, 38h 
    .text:0000000000000016 movzx eax, word ptr [rsp+38h+arg_0]
    .text:000000000000001B mov [rsp+38h+c.red], ax
    .text:0000000000000020 movzx eax, word ptr [rsp+38h+arg_8]
    .text:0000000000000025 mov [rsp+38h+c.green], ax
    .text:000000000000002A movzx eax, word ptr [rsp+38h+arg_10]
    .text:000000000000002F mov [rsp+38h+c.blue], ax
    .text:0000000000000034 movzx eax, word ptr [rsp+38h+arg_18]
    .text:0000000000000039 mov [rsp+38h+c.alpha], ax
    .text:000000000000003E mov rcx, qword ptr [rsp+38h+c.red] ; c
    .text:0000000000000043 call ?lighten@@YA?AUcolor_t@@U1@@Z ; lighten(color_t)
    .text:0000000000000048 mov [rsp+38h+var_10], rax
    .text:000000000000004D mov rax, [rsp+38h+var_10]
    .text:0000000000000052 add rsp, 38h 
    .text:0000000000000056 retn
    .text:0000000000000056 ?func@@YA?AUcolor_t@@HHHH@Z endp
    

Please note that the **c,** which is a structure, is passed by value in 2 registers: **rcx** and **rdx**. We had to rework quite many things in the decompiler to support such variables (we call them _scattered variables_). However, the output was worth it:  

    
    
    color_t __fastcall func(__int16 cx0, __int16 dx0, __int16 r8_0, __int16 r9_0)
    {
      color_t c;
      c.red = cx0;  c.green = dx0;  c.blue = r8_0;  c.alpha = r9_0; return lighten(c); } 

There is still some work to be done, but it seems we solved most problematic issues. Stay tuned, there will be more decompiler news soon!

---

## 3. 

**Date:** Posted: Dec 19, 2013  
**Link:** [https://hex-rays.com/blog/interacting-with-ida-through-ipc-channels](https://hex-rays.com/blog/interacting-with-ida-through-ipc-channels)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Interacting with IDA through IPC channels

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/interacting-with-ida-through-ipc-channels>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/interacting-with-ida-through-ipc-channels>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/interacting-with-ida-through-ipc-channels>)

Ilfak Guilfanov ✦ Posted: Dec 19, 2013

![Interacting with IDA through IPC channels](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

I’m happy to present you a guest post by David Zimmer <dzzie@yahoo.com>. The approach he describes can be used to develop plugins more conveniently (but not limited to that):

* * *

In this article we are going to discuss a mechanism that can be used to interact with IDA through external applications.  
The reason this technique was developed was to provide a convenient way for utility applications to query information from IDA databases, and automate its interface.  
Over the years, several methods have been tried such as pipes, and sockets. In the end, the easiest Inter-Process Communication (IPC) technique I have found is the Windows specific SendMessage API along with the WM_COPYDATA message. This technique was chosen for its simplicity, reliability, and its inherently synchronous nature.  
With this technique, the IDA server plugin creates a window and watches for command messages to come in.  


  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/ipc_server_window-3.png) (abbreviated C source to CreateWindow and receive messages)   
The plugin contains its own text based API which can be used to automate actions and return queried data back to the calling process. Example commands and data transfer routines are shown below:   
  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/ipc_commands-3.png) (hwnd arguments specify which window handle receives data callback)   
![](https://hex-rays.com/hubfs/Imported_Blog_Media/ipc_sendMessage-3.png)   
There are several advantages to this technique. Traditional plugin development usually requires compiling your code and launching the plugin from the host application.   
Debugging often entails wading through runtime debug logs, inferring problems through observed behavior, and chasing crashs in a debugger.   
This technique allows you to debug your executable in the development IDE as normal, harnessing all of its powerful capabilities such as edit and continue, call stack viewing, variable watches, breakpoints etc.   
Complex projects can be run directly while you iron out interface behaviors and are not dependant upon the host. Datasets can even be loaded from test files so that many routines can be debugged independently of IDA.   
One project where this technique was put to good use was an experiment to see if a Javascript IDE could be created for IDA automation tasks. The desire was for a scriptable interface that supported intellisense, function prototype tool tips, and syntax highlighting.   
  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/ipc_ida_jscript_screen_shot-3.png)   
With a project such as this, coding for the IDE behaviors is a major part of the development task. To develop such an application strictly as a plugin would be a daunting endeavor. Developing it as a standalone application was a great advantage where it could be run directly from the Visual Studio IDE without any intermediate steps.   
One other advantage to this technique is that it opens up plugin development to include higher level languages that do not have native support for the IDA API*. Languages such as C#, Delphi, and VB6 can easily interact with IDA through this mechanism. These languages have excellent rapid GUI development capabilities and a wealth of complex components already available to them. This technique is even open to Java developers through the JNI.   
Once the intermediate API and the client access library is written, creating utilities to integrate with IDA becomes a pretty quick process. Another example project that has come in handy is a Wingraph32 replacement that was coded in about a day.   
The interface shown below automatically syncs the IDA disassembly when graph nodes are clicked and can perform several renaming operations.   
  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/ipc_gleeGraph-3.png)   
There are some limitations to this technique as well. The particular implementation detailed in this article is Windows specific. It also requires both applications to be running on the same machine.   
Another consideration is that the data is crossing process boundaries which can be a performance hit depending on what is being transferred and how often it is being invoked. For example, extracting or patching large blocks of data would best be optimized by reserving the use of the SendMessage API as the command channel, and utilizing files or shared memory for the data transfer. This will likely be an optimization included in future revisions.   
The example IDASRVR project linked to below currently supports 36 API and uses a simple text based command protocol. Sample code is provided in C and C#. Client libraries are also available for C# and VB6 in the associated projects below. 

Sample Projects:  
[IDA_Jscript](<http://sandsprite.com//blogs/index.php?uid=7&pid=361>) – Javascript IDE for IDA (VB6)  
* You can create [C# and VB6 in process plugins](<http://sandsprite.com/CodeStuff/VB_Plugin_for_Olly.html>) for IDA through COM.  
This was my first approach, and was used in my  
[IDACompare plugin](<http://sandsprite.com/iDef/IDACompare/>).

---

## 4. 

**Date:** Posted: May 17, 2013  
**Link:** [https://hex-rays.com/blog/vulnerability-fix-for-btree-engine](https://hex-rays.com/blog/vulnerability-fix-for-btree-engine)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Vulnerability fix for bTree engine

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/vulnerability-fix-for-btree-engine>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/vulnerability-fix-for-btree-engine>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/vulnerability-fix-for-btree-engine>)

Ilfak Guilfanov ✦ Posted: May 17, 2013

![Vulnerability fix for bTree engine](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Just a quick note for all IDA users.  
We published a fix for potential vulnerability in IDA. Please check out <https://www.hex-rays.com/vulnfix.shtml>. It does not seem to be exploitable but we prefer to be on the safe side. Feel free to download and copy it to your plugins subdirectory. The plugin will validate all opened databases and protect you from malformed idbs.

---

## 5. 

**Date:** Posted: May 6, 2013  
**Link:** [https://hex-rays.com/blog/loading-your-own-modules-from-your-idapython-scripts-with-idaapi-require](https://hex-rays.com/blog/loading-your-own-modules-from-your-idapython-scripts-with-idaapi-require)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Loading your own modules from your IDAPython scripts with idaapi.require()

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/loading-your-own-modules-from-your-idapython-scripts-with-idaapi-require>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/loading-your-own-modules-from-your-idapython-scripts-with-idaapi-require>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/loading-your-own-modules-from-your-idapython-scripts-with-idaapi-require>)

Arnaud Diederen ✦ Posted: May 6, 2013

![Loading your own modules from your IDAPython scripts with idaapi.require\(\)](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

# TL;DR

If you were using `import` to import your own “currently-in-development” modules from your IDAPython scripts, you may want to use `idaapi.require()`, starting with IDA 6.5.

# Rationale

When using IDAPython scripts, users were sometimes facing [the following issue](<https://code.google.com/p/idapython/issues/detail?id=42>)

Specifically:

  * User loads script
  * Script `import`s user’s module `mymodule`
  * Script ends
  * User modifies code of `mymodule` (Note: the module is modified, not the script)
  * User reloads script
  * Modifications to `mymodule` aren’t taken into consideration.



While that’s perfectly understandable (the python runtime doesn’t have to reload `mymodule` if it has been compiled & loaded already), this is somewhat of an annoyance for users that were importing modules that were often modified.

# IDA <= 6.4: Ensuring a user-specified module gets reloaded, by destroying it.

Up until IDA 6.4, the IDAPython plugin [would do some magic after you have run your user script](<https://code.google.com/p/idapython/source/detail?r=273>).  
(click “expand all” to reveal the diff)

The sequence becomes:

  * User loads script
  * Script `import`s user’s module `mymodule`
  * Script ends
  * [module `mymodule` is deleted]
  * User modifies code of `mymodule`
  * User reloads script
  * Modifications to `mymodule` are taken into consideration, since module was deleted.



Unfortunately we have to stop doing this because:

  * That prevents us from using python-based hooks to be used after the script is finished (see below).
  * That goes against the rest of the python philosophy (i.e., modifications to objects are not reverted), and is therefore unexpected.



### Issues with hooks.

Imagine you have the following script, `dbghooks.py`:
    
    
    from idaapi import *
    import mydbghelpers
    class MyHooks(DBG_Hooks):
      def __init__(self):
        ...
      def dbg_bpt(self, tid, ea):
        mydbghelpers.do_something()
        return 0
      def dbg_step_into(self):
        ...
    hooks = MyHooks()
    hooks.hook()
    

  * User loads script
  * Scripts `import`s `mydbghelpers`
  * Script creates instance of `MyHooks`, and hooks it into IDA’s debugger APIs
  * Script ends
  * [module `mydbghelpers` is deleted]
  * User runs debugger, and a breakpoint is hit. Two things can happen: 
    * The hook fails executing
    * IDA crashes (that can happen if the form `from mydbghelpers import *` was used)



# IDA > 6.4: Introducing `idaapi.require()`

Everywhere else in python, when you modify a runtime object, those changes will remain visible.

We decided it would be better to not go against that standard behaviour anymore, and provide a helper to achieve the same results as what was achieved before with the deletion of user modules.

You can now import & re-import of a module with: `idaapi.require(name)`

Here is its definition:
    
    
    def require(modulename):
        if modulename in sys.modules.keys():
            reload(sys.modules[modulename])
        else:
            import importlib
            import inspect
            m = importlib.import_module(modulename)
            frame_obj, filename, line_number, function_name, lines, index = inspect.stack()[1]
            importer_module = inspect.getmodule(frame_obj)
            if importer_module is None: # No importer module; called from command line
                importer_module = sys.modules['__main__']
            setattr(importer_module, modulename, m)
            sys.modules[modulename] = m
    

**EDIT (September 16th, 2016):** After one of our users has reported an issue with this, we have updated the definition to:
    
    
    def require(modulename, package=None):
        import inspect
        frame_obj, filename, line_number, function_name, lines, index = inspect.stack()[1]
        importer_module = inspect.getmodule(frame_obj)
        if importer_module is None: # No importer module; called from command line
            importer_module = sys.modules['__main__']
        if modulename in sys.modules.keys():
            reload(sys.modules[modulename])
            m = sys.modules[modulename]
        else:
            import importlib
            m = importlib.import_module(modulename, package)
            sys.modules[modulename] = m
        setattr(importer_module, modulename, m)
    

The problem with the previous definition, is that if module `A` loads module `C`, and then module `A` loads module `B` which, itself, also loads module `C`, then module `A` will have the module `C` properly bound to its `globals()`, but module `B` won’t (since the module was already present in `sys.modules`, we were only reloading it, not setting the attribute in module `B`) 

### Example

The example debugger hooks script above becomes:
    
    
    from idaapi import *
    idaapi.require("mydbghelpers")
    class MyHooks(DBG_Hooks):
      def __init__(self):
        ...
      def dbg_bpt(self, tid, ea):
        mydbghelpers.do_something()
        return 0
      def dbg_step_into(self):
        ...
    hooks = MyHooks()
    hooks.hook()
    

I.e., only the second line changes.

---

## 6. 

**Date:** Posted: Apr 30, 2013  
**Link:** [https://hex-rays.com/blog/installing-pip-packages-and-using-them-from-ida-on-a-64-bit-machine](https://hex-rays.com/blog/installing-pip-packages-and-using-them-from-ida-on-a-64-bit-machine)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Installing PIP packages, and using them from IDA on a 64-bit machine

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/installing-pip-packages-and-using-them-from-ida-on-a-64-bit-machine>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/installing-pip-packages-and-using-them-from-ida-on-a-64-bit-machine>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/installing-pip-packages-and-using-them-from-ida-on-a-64-bit-machine>)

Arnaud Diederen ✦ Posted: Apr 30, 2013

![Installing PIP packages, and using them from IDA on a 64-bit machine](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Recently, one of our customers came to us asking how he should proceed to be able to install python packages, using PIP, and use those from IDA.  
The issue he was facing is that his system is a 64-bit Ubuntu 12.04 VM.  
Therefore using the Ubuntu-bundled PIP will just result in installing the desired package (let’s say `ssdeep`) for the system Python runtime, which is a 64-bit runtime and therefore not compatible with IDA.  
The best (as in: cleanest) solution I have found is to:

  * build a 32-bits python on the system.
  * `pip`-install packages in that 32-bits python’s sub-directories.
  * export `PYTHONPATH` to point to the 32-bits python’s sub-directories.



We figured we’d write it down here just in case it might help others.

# Prerequisites

  * Install autoconf
  * Install ia32-libs



# Building & installing a 32-bits python

  * `..$ export LD_LIBRARY_PATH=/lib/i386-linux-gnu/:/usr/lib32:$LD_LIBRARY_PATH`
  * Download Python2.7.4 
    * **Note:** You should make sure that the MD5 checksum and the size of the file you downloaded match those that are advertised on the page. That would prevent a man-in-the-middle attacker from providing you with a malicious Python bundle. 
  * Build it. Note that you’ll probably have to sudo-create a few symlinks. I had to do this, on the Ubuntu 12.04 64-bit VM I tested this on: 
    * `/lib/i386-linux-gnu/libssl.so` ⇒ `/lib/i386-linux-gnu/libssl.so.1.0.0`
    * `/lib/i386-linux-gnu/libcrypto.so` ⇒ `/lib/i386-linux-gnu/libcrypto.so.1.0.0`
    * `/lib/i386-linux-gnu/libz.so` ⇒ `/lib/i386-linux-gnu/libz.so.1`
  * For the sake of completeness, here are my build commands (don’t forget the flags, of course): 
    * ..$ CFLAGS=-m32 LDFLAGS=-m32 ./configure --prefix=/opt/Python2.7.4-32bits

    * ..$ CFLAGS=-m32 LDFLAGS=-m32 make -j 8




### Once the build completes

Here’s what I have as last lines of the build:
    
    
    INFO: Can't locate Tcl/Tk libs and/or headers
    Python build finished, but the necessary bits to build these modules were not found:
    _bsddb             _curses            _curses_panel
    _sqlite3           _tkinter           bsddb185
    bz2                dbm                gdbm
    readline           sunaudiodev
    To find the necessary bits, look in setup.py in detect_modules() for the module's name.

If you see, below that, that it failed to build, say `'binascii'`, then something went wrong.  
Make sure you run `make -j 1` to check out what went wrong (i.e., what library it claims not being able to find)  
Once you have succesfully built your 32-bits Python, it’s time to install it: `sudo make install`

### Trying your freshly-built python
    
    
    ..$ /opt/Python2.7.4-32bits/bin/python2.7
    Python 2.7.4 (default, Apr 26 2013, 16:03:38)
    [GCC 4.6.3] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import binascii
    >>>

No complaint so far. Good.

### Checking that `pkg_resources` is available.

Try importing `pkg_resources`. If it fails, you’ll probably have to do the following:
    
    
    ..$ cd /tmp
    ..$ curl -O http://python-distribute.org/distribute_setup.py
    ..$ less distribute_setup.py  # (*)
    ..$ sudo /opt/Python2.7.4-32bits/bin/python2.7 distribute_setup.py

That will print out quite a fair amount of info, and should succeed.  
**(*) Note:** A careful reader has pointed out that it would be fairly easy to intercept (man-in-the-middle) such an HTTP request, and serve malicious content that would then be piped (as root) to Python.  
That’s why I think it’s important to mention, as a third step (i.e., `less ...`), that the code that was downloaded should ideally be checked. Hopefully, http://python-distribute.org will soon provide HTTPS support, which will limit such MITM attack risks.

### Trying your freshly-built python, again

We want to make sure `pkg_resources` can be imported.
    
    
    ..$ /opt/Python2.7.4-32bits/bin/python2.7
    Python 2.7.4 (default, Apr 26 2013, 16:03:38)
    [GCC 4.6.3] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import pkg_resources
    >>>

Still no complaint. Good.  
If yours complains, you’ll have to first make sure you fix whatever is causing it to fail, because the next will not work without that.

# Installing PIP for your new Python build

Since using your system’s PIP will probably not work (as it would build & install things in a 64-bits python sub-directory), you’ll have to install a PIP package specifically for your freshly-built Python.  
Here’s how I proceeded:
    
    
    ..$ cd /tmp;
    ..$ curl -O https://raw.github.com/pypa/pip/master/contrib/get-pip.py;
    ..$ sudo /opt/Python2.7.4-32bits/bin/python2.7 get-pip.py

PIP is now installed.

# PIP-installing a package (i.e., `ssdeep`)

To download/build/install the `ssdeep` package I ran, as root (either that, or you’ll have to give your user the rights to write in /opt/Python2.7.4-32bits):
    
    
    ..$ su
    Password:
    root ..$ export CFLAGS=-m32
    root ..$ export LDFLAGS=-m32
    root ..$ export LD_LIBRARY_PATH=/lib/i386-linux-gnu/:/usr/lib32:$LD_LIBRARY_PATH
    root ..$ /opt/Python2.7.4-32bits/bin/python2.7 /opt/Python2.7.4-32bits/bin/pip install ssdeep

Notice how I use my freshly-built python, with my fresly-installed PIP (and not the system one.)  
Note: Don’t forget the `export` lines, or PIP will partially build stuff for x64, and partially for x86. That, as you can guess, won’t quite work.  
If you forgot the `export` lines and started building anyway (and the build failed because of the mixed architecture issue I just wrote about), make sure you delete whatever is in `/tmp/pip-build-*`, so that there won’t be stale object files of inappropriate architecture in there.

# Check out the PIP-installed package works
    
    
    ..$ /opt/Python2.7.4-32bits/bin/python2.7
    Python 2.7.4 (default, Apr 26 2013, 16:03:38)
    [GCC 4.6.3] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import ssdeep
    >>> ssdeep
    <module 'ssdeep' from '/opt/Python2.7.4-32bits/lib/python2.7/site-packages/ssdeep.so'>
    >>> dir(ssdeep)
    ['Error', '__all__', '__builtins__', '__doc__', '__file__', '__name__', '__package__', '__test__', '__version__', 'compare', 'hash', 'hash_from_file', 'sys']
    >>>

So far so good.

# Testing the PIP-installed package in IDA

Since that’s still the goal (though you might have forgotten by now, given the amount of directions above.. 😉 ),  
we’ll now try and make use of that PIP-installed package in IDA.

  * `..$ export PYTHONPATH=/opt/Python2.7.4-32bits/lib/python2.7/site-packages:/opt/Python2.7.4-32bits/:$PYTHONPATH`
  * `..$ idaq`



If all went well, typing `import ssdeep` in the Python input line should properly, silently, nicely import the package.

---

## 7. 

**Date:** Posted: Jun 22, 2012  
**Link:** [https://hex-rays.com/blog/recon-2012-compiler-internals](https://hex-rays.com/blog/recon-2012-compiler-internals)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Recon 2012: Compiler Internals

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/recon-2012-compiler-internals>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/recon-2012-compiler-internals>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/recon-2012-compiler-internals>)

I 

Igor Skochinsky ✦ Posted: Jun 22, 2012

![Recon 2012: Compiler Internals](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

This year I again was lucky to present at [Recon](<http://recon.cx/>) in Montreal. There were many great talks as usual. I combined the topic of my last year’s talk on C++ reversing and my OpenRCE article on Visual C++ internals. New material was implementation of exceptions and RTTI in MSVC x64 and GCC (including Apple’s iOS).  
The videos are not up yet but here are the slides of my presentation and a few demo scripts I made for it to parse GCC’s RTTI structures and exception tables. I also added my old scripts from OpenRCE which I amended slightly for the current IDA versions (mostly changed hotkeys).  
[Slides](<https://www.hex-rays.com/wp-content/uploads/2012/06/Recon-2012-Skochinsky-Compiler-Internals.pdf>)  
[Scripts](<https://www.hex-rays.com/wp-content/uploads/2012/06/recon-2012-skochinsky-scripts.zip>)

---

## 8. 

**Date:** Posted: Apr 5, 2012  
**Link:** [https://hex-rays.com/blog/calling-ida-apis-from-idapython-with-ctypes](https://hex-rays.com/blog/calling-ida-apis-from-idapython-with-ctypes)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Calling IDA APIs from IDAPython with ctypes

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/calling-ida-apis-from-idapython-with-ctypes>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/calling-ida-apis-from-idapython-with-ctypes>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/calling-ida-apis-from-idapython-with-ctypes>)

I 

Igor Skochinsky ✦ Posted: Apr 5, 2012

![Calling IDA APIs from IDAPython with ctypes](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

IDAPython provides wrappers for a big chunk of IDA SDK. Still, there are some APIs that are not wrapped because of SWIG limitations or just because we didn’t get to them yet. Recently, I needed to test the get_loader_name() API which is not available in IDAPython but I didn’t want to write a full plugin just for one call. For such cases it’s often possible to use the [ctypes module](<http://docs.python.org/library/ctypes.html>) to call the function manually.  
The IDA APIs are provided by the kernel dynamic library. In Windows, it’s called **ida.wll** (or **ida64.wll**), in Linux **libida[64].so** and on OS X **libida[64].dylib**. **ctypes** provides a nice feature that dynamically creates a callable wrapper for a DLL export by treating it as an attribute of a special class instance. Here’s how to get that instance under the three platforms supported by IDA:  
``

`
    
    
    import ctypes
    idaname = "ida64" if __EA64__ else "ida"
    if sys.platform == "win32":
        dll = ctypes.windll[idaname + ".wll"]
    elif sys.platform == "linux2":
        dll = ctypes.cdll["lib" + idaname + ".so"]
    elif sys.platform == "darwin":
        dll = ctypes.cdll["lib" + idaname + ".dylib"]
    

`

``  
We use “**windll** ” because IDA APIs use **stdcall** calling convention on Windows (check the definition of **idaapi** in pro.h).  
Now we just need to call our function just as if it was an attribute of the “dll” object. But first we need to prepare the arguments. Here’s the declaration from loader.hpp:  
`idaman ssize_t ida_export get_loader_name(char *buf, size_t bufsize);`  
**ctypes** provides a convenience functions for creating character buffers:  
`buf = ctypes.create_string_buffer(256)`  
And now we can call the function:  
`dll.get_loader_name(buf, 256)`  
To retrieve the contents of the buffer as a Python byte string, just use its .raw attribute. The complete script now looks like this:  
``

`
    
    
    import ctypes
    idaname = "ida64" if __EA64__ else "ida"
    if sys.platform == "win32":
        dll = ctypes.windll[idaname + ".wll"]
    elif sys.platform == "linux2":
        dll = ctypes.cdll["lib" + idaname + ".so"]
    elif sys.platform == "darwin":
        dll = ctypes.cdll["lib" + idaname + ".dylib"]
    buf = ctypes.create_string_buffer(256)
    dll.get_loader_name(buf, 256)
    print "loader:", buf.raw

`

``  
**ctypes** offers many means to interface with C code, so you can use it to call almost any IDA API.

---

## 9. 

**Date:** Posted: Jan 24, 2012  
**Link:** [https://hex-rays.com/blog/the-trace-replayer](https://hex-rays.com/blog/the-trace-replayer)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# The trace replayer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/the-trace-replayer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/the-trace-replayer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/the-trace-replayer>)

Joxean Koret ✦ Posted: Jan 24, 2012

![The trace replayer](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

One of the new features that will be available in the next version of IDA is a trace re-player. This pseudo-debugger allows to re-play execution traces of programs debugged in IDA. The replayer debugger allows replaying traces recorded with any of the currently supported debuggers, ranging from local Linux or win32 debuggers to remote GDB targets. Currently supported targets include x86, x86_64, ARM, MIPS and PPC.  
When we are re-playing a recorded trace, we can step forward and backward, set breakpoints, inspect register values, change the instruction pointer to any recorded IP, etc…  
Also, trace management capabilities have been added to IDA in order to allow saving and loading recorded execution traces. Let’s see an example.  
****  
**A vulnerable sample program**  
****For this blog post, I will show you how this plugin can be used to analyze a bug in a toy executable program. This sample application receives 2 arguments: a message to display and the size of it. The program checks if the size of the given buffer (calling strlen) is longer than the size specified, printing out an error message and exiting. If not, memory of the given size is reserved for a local variable, the contents of the buffer copied to it and a message based on this string printed out to stdout. After this, the memory reserved is freed and the application simply exits.  
In this application there is a little integer overflow bug that can be triggered giving to the size argument a negative value. Let’s record a trace of the program crashing and replay it in IDA to understand why the program is crashing:

  * Set a breakpoint in the entry point.
  * Set the program arguments to “ _whatever_ -1” in Debugger → Process options.
  * Run the application with the correspondent debugger (in my case, the “Local Linux” debugger).
  * When the breakpoint is reached, enable instruction tracing (via the menu item “Debugger → Tracing → Instruction tracing”).
  * Let the application continue (press F9).



At some point it will crash with a message similar to this:  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic1-300x63-3.png?width=300&height=63&name=pic1-300x63-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic1-3.png>)

When the application crashes, stop the debugger, go to the trace window (Debugger → Tracing → Trace window) and save the trace to a file (right click on the window and, from the pop-up menu, select the option Other options → Save binary trace file to disk). Specify the file name and a description for this trace and click OK:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic2-300x93-3.png?width=300&height=93&name=pic2-300x93-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic2-3.png>)

Next, switch to the “Trace replayer debugger” (from the menu Debugger → Switch to debugger, and then select “Trace replayer”). After this, go to the trace window to see where is it crashing and set a breakpoint in the function call that segfaults. In our example, it’s crashing in a call to strcpy in function “foo” as we may see here:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic3-300x91-3.png?width=300&height=91&name=pic3-300x91-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic3-3.png>)

We will set a breakpoint in the call to strcpy in function “foo” and press F9 to replay the trace. When the breakpoint is reached we have all the register values as they were when the program was really executed (click to enlarge):

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic4-300x190-3.png?width=300&height=190&name=pic4-300x190-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic4-3.png>)

There is a check at 0x08048502 for the size of the given buffer and the size given in the command line. As -1 is lower than strlen(“AAAA”) the developer didn’t expected the program to reach the basic block where the malloc and strcpy calls are made. There is another bug here: before the call to strcpy there is a call to malloc to reserve memory with the size we gave in the command line, but the developer didn’t performed any check to see if the memory was correctly reserved or not. Let’s step back to this position by selecting from the menu Debugger → Step back. Click this menu item various times until EIP points to 0x804851D. Alternatively, at the IDC prompt in the bottom part of IDA, we may enter the command “StepBack()” and press enter. No matter how we moved to the instruction after the malloc call, we will see the following in IDA:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic5-300x168-3.png?width=300&height=168&name=pic5-300x168-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic5-3.png>)

The program failed to reserve memory as the returned pointer is NULL. Step back until the “call _malloc” instruction to see the size passed to malloc:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic6-300x144-3.png?width=300&height=144&name=pic6-300x144-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic6-3.png>)

The program is trying to reserve 0xFFFFFFFF bytes (4 GB) and it fails to do so. Let’s analyse how the program reached the point where the memory is reserved, the strcpy call is performed, etc… when it wasn’t supposed to do so. Click on the 1st instruction of the function “foo”, right click and select from the pop-up menu “Set IP” (this way, we are telling the re-player to change IP to the nearest event with this IP). The instruction pointer and all the other register values changes:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic7-300x94-3.png?width=300&height=94&name=pic7-300x94-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic7-3.png>)

Now, step over the instructions until we reach the JBE one (alternatively, we could simply right click in the JBE instruction and select from the pop-up menu “Set IP”). Once moved to this instruction, we will see the following register values:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pic8-300x119-3.png?width=300&height=119&name=pic8-300x119-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pic8-3.png>)

The result of the call to strlen passing the given buffer (unsigned) is compared against the size value we gave in the command line (which is calculated as atoi(argv[2]), which returns a signed integer). Then, an unsigned comparison (JBE) is performed and the check “0x4 <= 0xFFFFFFFF” passes. Now you can document this bug and continue searching for more or fix it and recompile your application.

We hope you like this new IDA feature!

---

