# Hex-Rays Blog – Page 39

**Archived:** 2026-02-20 12:22
**URL:** https://hex-rays.com/blog/page/39

---

## 1. 

**Date:** Posted: Mar 30, 2018  
**Link:** [https://hex-rays.com/blog/ida-on-non-os-x-retina-hi-dpi-displays](https://hex-rays.com/blog/ida-on-non-os-x-retina-hi-dpi-displays)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA on non-OS X/Retina Hi-DPI displays

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-on-non-os-x-retina-hi-dpi-displays>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-on-non-os-x-retina-hi-dpi-displays>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-on-non-os-x-retina-hi-dpi-displays>)

Arnaud Diederen ✦ Posted: Mar 30, 2018

![IDA on non-OS X/Retina Hi-DPI displays](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

# The problem

Some users running IDA on Windows & Linux X11 platforms with Hi-DPI displays, have reported that IDA looks rather odd: the navigator bar is too narrow, the text under it gets truncated, and there is overall feeling of packing & clumsiness:

  * Windows: [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/windows-without-sf-300x169-4.png?width=300&height=169&name=windows-without-sf-300x169-4.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/windows-without-sf-4.png>)
  * Linux X11: [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/linux-without-sf-300x171-4.png?width=300&height=171&name=linux-without-sf-300x171-4.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/linux-without-sf-4.png>)



Looking closely, one can notice the following issues (probably not an exhaustive list)

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/linux-without-sf-annotated-300x171-4.png?width=300&height=171&name=linux-without-sf-annotated-300x171-4.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/linux-without-sf-annotated-4.png>)

Note: this should not happen and shouldn’t apply for OS X users running IDA on Retina displays. Nor should it happen (but we didn’t get a chance to test this) on non-X11 Linux display managers, such as Wayland.

## Fix / mitigation:

On Linux X11 & Windows, if you are using Hi-DPI monitors and IDA looks somewhat like it does in the above screenshots, please try setting the environment variable `QT_AUTO_SCREEN_SCALE_FACTOR` to `1`:

E.g., on Linux/X11:  
`  
~# export QT_AUTO_SCREEN_SCALE_FACTOR=1  
~# path/to/ida my.idb`

` `

``

IDA should now look more pleasant:

  * Windows: [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/windows-with-sf-300x169-4.png?width=300&height=169&name=windows-with-sf-300x169-4.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/windows-with-sf-4.png>)
  * Linux X11: [![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/linux-with-sf-300x171-4.png?width=300&height=171&name=linux-with-sf-300x171-4.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/linux-with-sf-4.png>)



Some things are still not perfect (e.g., checkboxes might remain small), but IDA definitely looks better.

Please give it a try!

# Gory details

When one applies scaling/zooming, either in Windows and Linux X11, that will have the effect of causing the OS to return scaled values for font metrics when queried using point sizes (which is almost always the case.)

For example, when the font metrics for a font of size `12pt` are requested by a Qt application, instead of returning `14 pixels` as it would on a non-scaled system, the operating system will instead return `28 pixels` on a 200% scaled one (in other words, this is essentially a font database-related feature).

That will, in turn, have the net effect of causing Qt to compute layout of the surrounding widgets according to those scaled font metrics, which explains why the text is (for the most part) not truncated.

However, what applying scaling does **_not_** do, is tell Qt that it should scale all other pixel measurements by that scale factor.

Consequently, paddings, margins, scrollbars, etc… all have uncomfortably small dimensions, especially when compared to text.

The `QT_AUTO_SCREEN_SCALE_FACTOR` environment variable is an opt-in that program users can define, in order to control how the program should look. It will in essence instruct Qt to perform automatic scaling of (non-font-related) graphical operations according to the pixel density of the screen(s).

More information can be found [on Qt’s website](<http://doc.qt.io/qt-5/highdpi.html#high-dpi-support-in-qt>).

## Why is this not needed under OS X + Retina?

This is not needed under OS X + Retina, because Qt does not need to perform any kind of scaling there: the scaling is performed by the drawing primitives of the OS _itself_ , and is entirely transparent to the application.

(In fact, an OSX application doesn’t even work with the real screen geometry, but rather with an “abstract” coordinate system, normalizing pixel sizes across screen densities.)

---

## 2. 

**Date:** Posted: Mar 5, 2018  
**Link:** [https://hex-rays.com/blog/ida-7-1-qt-5-6-3-configure-options-patch](https://hex-rays.com/blog/ida-7-1-qt-5-6-3-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 7.1: Qt 5.6.3 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-1-qt-5-6-3-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-1-qt-5-6-3-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-1-qt-5-6-3-configure-options-patch>)

Arnaud Diederen ✦ Posted: Mar 5, 2018

![IDA 7.1: Qt 5.6.3 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.3 build, that is shipped with IDA 7.1.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.3\configure.bat "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.3-x64"`
    * Note that you will have to build with Visual Studio 2015, to obtain compatible libs 
  * Linux: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-64" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.3-x64"`
  * Mac OSX: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-g++" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.3-x64"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2018/03/qt-5_6_3_full-IDA71.zip>). You should therefore apply it to your own Qt 5.6.3 sources before compiling, in order to obtain similar binaries (`patch -p 1 < path/to/qt-5_6_3_full-IDA71.patch`)  
Note that this patch should work without any modification, against the 5.6.3 release. You may have to fiddle with it, if your Qt 5.6.3 sources come from somewhere else.

---

## 3. 

**Date:** Posted: Dec 28, 2017  
**Link:** [https://hex-rays.com/blog/idapython-namespacing-for-plugins-loaders-processor-modules](https://hex-rays.com/blog/idapython-namespacing-for-plugins-loaders-processor-modules)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDAPython: namespacing for plugins, loaders & processor modules.

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/idapython-namespacing-for-plugins-loaders-processor-modules>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/idapython-namespacing-for-plugins-loaders-processor-modules>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/idapython-namespacing-for-plugins-loaders-processor-modules>)

Arnaud Diederen ✦ Posted: Dec 28, 2017

![IDAPython: namespacing for plugins, loaders & processor modules.](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

# Intended audience

`IDAPython` plugins, loaders or processor modules developers.

# The problem

Until now, `IDAPython` would load all loaders, processor modules & plugins in the `'__main__'` module.

This causes namespace pollution, which can sometimes leads to _very_ obscure errors.

# The solution

Starting with version `7.1`, IDA will import plugins, loaders & processor modules in their own, separate Python modules.

The names of those Python modules is derived from the plugin, loader or processor module’s file name.

E.g.,

  * a plugin named `myplg.py` will now be imported into its own `'__plugins__myplg'` Python module. 
  * a loader named `myldr.py` will now be imported into its own `'__loaders__myldr'` Python module. 
  * a processor module named `myprc.py` will now be imported into its own `'__procs__myprc'` Python module. 



# My plugin/loader/processor module complains about unexisting variables/functions!

It very likely means that the code in question was, in effect, relying on some other code being loaded before it, and that was “polluting” the `'__main__'` module in a way that was very fortunate from its point-of-view (a happy coincidence, if you will.)

The solution is of course to fix the code in question so that it imports everything it needs, before making use of it.

However…

# I’m in a hurry

As a temporary workaround, you can set `NAMESPACE_AWARE` to `NO` in `cfg/python.cfg`.

This will cause IDA to revert to the old behavior, where the `'__main__'` module is shared across all plugins, loaders, processor modules & user scripts.

However, please note that support for `NAMESPACE_AWARE` will be removed sometimes in the future.

---

## 4. 

**Date:** Posted: Oct 30, 2017  
**Link:** [https://hex-rays.com/blog/ida-and-common-python-issues](https://hex-rays.com/blog/ida-and-common-python-issues)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA and common Python issues

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-and-common-python-issues>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-and-common-python-issues>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-and-common-python-issues>)

I 

Igor Skochinsky ✦ Posted: Oct 30, 2017

![IDA and common Python issues](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

With IDA 7.0 switching fully to native x64 architecture, we also switched to the x64 Python which brought some  
new issues but also exposed some we’ve seen before. This post tries to summarize the most common issues we’ve seen our users encounter as  
well as suggestions about how to fix them or at least diagnose where things went wrong before you have to contact support.

## Common errors

**The specified module could not be found**

You may see such messages in IDA’s Output window:
    
    
    LoadLibrary(C:\Program Files\IDA\plugins\python.dll) error: The specified module could not be found.
    C:\Program Files\IDA\plugins\python.dll: can't load file

…while the file is obviously there. This message (it comes from the OS) is a little misleading:  
it actually means that one of the modules that  
`python.dll` (IDAPython plugin) links _to_ could not be found.

The most common one is `python27.dll` (python runtime) which is _usually_ installed in `%windir%\system32` but  
may be in a different place for whatever reason (e.g. you’re using an alternative Python distribution or user-specific Python  
installation). You may need to check your `PATH` environment variable or possibly reinstall Python (x64 version).

**%1 is not a valid Win32 application**

Typical output:
    
    
    LoadLibrary(C:\Program Files\IDA\plugins\python.dll) error: %1 is not a valid Win32 application.
    C:\Program Files\IDA\plugins\python.dll: can't load file

or:
    
    
    ImportError: DLL load failed: %1 is not a valid Win32 application

This error can happen if IDA tries to load a DLL with wrong architecture (e.g. x86 DLL into x64 IDA or vice versa). Common causes:

  * wrong variant of `python27.dll` present in PATH
  * wrong versions of native modules (`.pyd`) are being used (e.g. you have `PYTHONPATH` or `PYTHONHOME`  
environment variable set pointing to the x86 install)
  * installing x86 and x64 Python into the same directory (this will never work).



**IDAPython: importing “site” failed**
    
    
    DAPython: importing "site" failed

This issue is usually caused by presence of non-standard `python27.dll` in the `PATH` which  
uses its own set of modules (you should edit `PATH` in this case). However, it may happen  
if your Python installation is broken in some way. Reinstalling Python manually may fix it.

## Diagnosis checklist

  1. Check that you have one and only one `python27.dll` in `PATH`. you can check it by executing  
“`where python27.dll`” in a command prompt. Expected output:
         
         c:\>where python27.dll
         C:\Windows\System32\python27.dll

  2. Check where Python is looking for modules. Check this registry key:
         
         HKEY_LOCAL_MACHINE\SOFTWARE\Python\PythonCore\2.7\InstallPath

Its expected value is `C:\Python27-x64\`

  3. Check your environment (type “`set`” in command prompt) for any variables starting with  
`PYTHON`, especially `PYTHONHOME` or `PYTHONPATH` and fix or remove them. If you need  
these variables for other software, we suggest making a `.bat` file to clear  
these variables and then start IDA.

  4. if IDAPython loads but you get import errors when running scripts, dump sys.path and check for any unexpected/wrong entries.

Example good output:
         
         Python>import sys
         Python>sys.path
         ['C:\\Windows\\system32\\python27.zip', 'C:\\Python27-x64\\Lib', 'C:\\Python27-x64\\DLLs', 'C:\\Python27-x64\\Lib\\lib-tk', 'C:\\Program Files\\IDA 7.0\\python', 'C:\\Python27-x64', 'C:\\Python27-x64\\lib\\site-packages', 'C:\\Program
         Files\\IDA 7.0\\python\\lib\\python2.7\\lib-dynload\\ida_32', 'C:\\Program Files\\IDA 7.0\\python']

  5. Trace from which paths IDAPython is loading modules. You can do it by setting environment  
variable `PYTHONVERBOSE=1` before running IDA. Paths will be printed into Output Window  
(you can also save it to file by adding `-L<logfile>` to IDA’s  
command line).




## Reinstalling Python/IDA

In case you decide to reinstall Python and/or IDA, _do not_ just remove their directories  
as this may leave remains of installation in registry or elsewhere and mess up future installs.  
Use the corresponding uninstallers. Note that IDA’s uninstaller does not uninstall Python  
so that needs to be done separately if required.

Normally IDA 7.0 installer installs x64 Python if it’s not already installed but you can also  
download a recent 2.7.x installer  
[from python.org](<https://www.python.org/downloads/release/python-2714/>)  
(pick the “Windows x86-64 MSI installer”). You can install it into any location of your choice as long as IDA can find it  
(`python27.dll` should be in `PATH`), however we recommend using `C:\Python27-x64`  
(“All Users” option) so that it does not conflict with the 32-bit install.

Before uninstalling IDA, check that you still have the original installer. If necessary, you can  
[request a new download](</updida/>)  
(only possible with active support).

---

## 5. 

**Date:** Posted: Sep 19, 2017  
**Link:** [https://hex-rays.com/blog/ida-7-0-qt-5-6-0-configure-options-patch](https://hex-rays.com/blog/ida-7-0-qt-5-6-0-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 7.0: Qt 5.6.0 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-0-qt-5-6-0-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-0-qt-5-6-0-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-0-qt-5-6-0-configure-options-patch>)

Arnaud Diederen ✦ Posted: Sep 19, 2017

![IDA 7.0: Qt 5.6.0 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.0 build, that is shipped with IDA 7.0.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.0\configure.bat "-nomake" "tests" "-qtnamespace" "QT"  
"-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.0-x64"`

    * Note that you will have to build with Visual Studio 2015, to obtain compatible libs 
  * Linux: `.../5.6.0/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-64" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.0-x64"`
  * Mac OSX: `.../5.6.0/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-g++" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.0-x64"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2017/08/qt-5_6_0_full-IDA70.zip>). You should therefore apply it to your own Qt 5.6.0 sources before compiling, in order to obtain similar binaries.  
Note that this patch should work without any modification, against the 5.6.0 release as found [there](<https://download.qt.io/archive/qt>). You may have to fiddle with it, if your Qt 5.6.0 sources come from somewhere else.

---

## 6. 

**Date:** Posted: Jun 14, 2017  
**Link:** [https://hex-rays.com/blog/news-about-the-x64-edition](https://hex-rays.com/blog/news-about-the-x64-edition)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# News about the x64 edition

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/news-about-the-x64-edition>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/news-about-the-x64-edition>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/news-about-the-x64-edition>)

Ilfak Guilfanov ✦ Posted: Jun 14, 2017

![News about the x64 edition](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Sorry for the long silence since IDA v6.95, we all were incredibly busy with the transition to the 64-bit version. We are happy to say now that we are close to the finish line and will announce the beta test soon.  
Transition to x64 itself was not that hard. We have been compiling IDA in x64 mode since many years, so making it actually work was a piece of cake.  
It was much more time consuming to clean up the API: make it more logical, easier to use, and even remove some obsolete stuff that we have been carrying around since ages. Switching to the x64 is the unique opportunity for this cleanup because there are no existing x64 plugins yet and we won’t be breaking any working plugins. We hope that you will like the new API much more.  
Just to give you an idea: we made more than 8000 commits to our source code repository and just the commit descriptions are more than 1MB. We reached 1.6M lines of source code without taking into account any third party or auto-generated files. I haven’t counted how many lines had changed since v6.95, but it can easily be that every second line got modified during the transition. In short, it was a huge undertaking.  
While it was huge, we did not manage to make everything ideal (is it possible at all?…) We will continue to work on the API in the future.  
Naturally, we were also busy fixing our past bugs (309 bugs since the public release of v6.95, to be exact; fortunately almost all of them reveal themselves in very specific circumstances).  
We were also working on new features. Just to give you an idea of the new stuff, see very short descriptions below.  
First of all, IDA fully switched to UTF-8. It will be possible to use Unicode strings everywhere, and specify any specific encoding for a string in the input file. The databases will be kept in UTF-8 as well, which will allow us to get rid of inconsistencies between Windows and Unix versions of IDA. In fact it is deeper that a simple support of UTF-8, we have a new system for international character support but it deserves a separate blog entry. We will talk about it in more detail after the release.  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/utf8-4.png)  
We will release the PowerPC 64-bit decompiler along with IDA v7. This assembler code (which did not fit into the screenshot):  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/ppc64_asm-4.png)  
Gets converted into one line:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/ppc64_c-4.png)  
We added support for exception handling. IDA recognizes try/except blocks and neatly comments them in the listing:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/try-4.png)  
The iOS debugger improves support for debugging dylibs from dyld_shared_cache and adds support for source code level debugging.  
We have tons of other improvements: better GDBServer support, updated FLAIR signatures, improved decompiler heuristics, updated built-in functions for IDAPython and IDC, new switch table patterns, etc.

Stay tuned, we will announce the beta test soon!

---

## 7. 

**Date:** Posted: Aug 9, 2016  
**Link:** [https://hex-rays.com/blog/installing-ida-6-95-on-linux](https://hex-rays.com/blog/installing-ida-6-95-on-linux)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Installing IDA 6.95 on Linux

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/installing-ida-6-95-on-linux>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/installing-ida-6-95-on-linux>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/installing-ida-6-95-on-linux>)

Arnaud Diederen ✦ Posted: Aug 9, 2016

![Installing IDA 6.95 on Linux](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

IDA is still, as of this writing (August 9th, 2015), a 32-bit application and both IDA & its installer(*) require certain 32-bit libraries to be present on your Linux system before they can run.  
Here is the list of commands you will have to run in order to install those dependencies, for the following systems:

  * Debian & derivative systems such as Ubuntu, Xubuntu, … 
  * Red Hat Enterprise Linux 7.2 (and likely other versions as well) 



Note: we cannot possibly install & try IDA on all flavors/versions of all Linux distributions, but we will do our best to update this post with relevant information, whenever we learn of a distribution requiring special attention.

## Debian & Ubuntu

### Common dependencies

The following should allow IDA to run on most Linux systems deriving from Debian distributions:
    
    
    sudo dpkg --add-architecture i386
    sudo apt-get update
    sudo apt-get install libc6-i686:i386 libexpat1:i386 libffi6:i386 libfontconfig1:i386 libfreetype6:i386 libgcc1:i386 libglib2.0-0:i386 libice6:i386 libpcre3:i386 libpng12-0:i386 libsm6:i386 libstdc++6:i386 libuuid1:i386 libx11-6:i386 libxau6:i386 libxcb1:i386 libxdmcp6:i386 libxext6:i386 libxrender1:i386 zlib1g:i386 libx11-xcb1:i386 libdbus-1-3:i386 libxi6:i386 libsm6:i386 libcurl3:i386
    # In addition, it is also necessary to install the
    # following packages, for IDA to present a usable
    # & well-integrated GUI on many Debian & Ubuntu
    # desktops. If the set of dependencies above are
    # not enough to obtain a slick UI, please
    # install the following:
    sudo apt-get install libgtk2.0-0:i386 gtk2-engines-murrine:i386 gtk2-engines-pixbuf:i386 libpango1.0-0:i386
    

Note: one of our users kindly warned us that the package “libpng12-0:i386” has been replaced by “libpng16-16:i386” in Debian Stretch repositories. Consequently, you may have to change the before-last command above to:
    
    
    sudo apt-get install libc6-i686:i386 libexpat1:i386 libffi6:i386 libfontconfig1:i386 libfreetype6:i386 libgcc1:i386 libglib2.0-0:i386 libice6:i386 libpcre3:i386 **libpng16-16:i386** libsm6:i386 libstdc++6:i386 libuuid1:i386 libx11-6:i386 libxau6:i386 libxcb1:i386 libxdmcp6:i386 libxext6:i386 libxrender1:i386 zlib1g:i386 libx11-xcb1:i386 libdbus-1-3:i386 libxi6:i386 libsm6:i386 libcurl3:i386
    

## Red Hat Enterprise Linux 7.2

IDA will require the following packages to be installed, in order to run properly on RHEL 7.2 (and probably any other RPM-based distribution):
    
    
    redhat-lsb-core.i686
    glib2.i686
    libXext.i686
    libXi.i686
    libSM.i686
    libICE.i686
    freetype.i686
    fontconfig.i686
    dbus-libs.i686
    libgnomeui:i686
    

## Arch Linux

While the author of this post is not quite familiar with Arch Linux, one of our users reported that adding the required dependencies can be performed through the AUR package that can be found at: <https://aur.archlinux.org/packages/ida-pro-6.4/>.  
(And then, installing a 32-bit system-wide interpreter (i.e., if one favors that option over the default that consists of using the Python runtime shipped with IDA), should be performed by installing the package ‘lib32-python2’.)

---

## 8. 

**Date:** Posted: Aug 9, 2016  
**Link:** [https://hex-rays.com/blog/ida-6-95-qt-5-6-0-configure-options-patch](https://hex-rays.com/blog/ida-6-95-qt-5-6-0-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 6.95: Qt 5.6.0 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-6-95-qt-5-6-0-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-6-95-qt-5-6-0-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-6-95-qt-5-6-0-configure-options-patch>)

Arnaud Diederen ✦ Posted: Aug 9, 2016

![IDA 6.95: Qt 5.6.0 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.0 build, that is shipped with IDA 6.95.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.0\configure.bat "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.0"`
    * Note that you will have to build with Visual Studio 2015, to obtain compatible libs 
  * Linux: `.../5.6.0/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-32" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.0"`
  * Mac OSX: `.../5.6.0/build/../configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-g++-32" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.0"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2016/08/qt-5_6_0_full.zip>). You should therefore apply it to your own Qt 5.6.0 sources before compiling, in order to obtain similar binaries.  
Note that this patch should work without any modification, against the 5.6.0 release. You may have to fiddle with it, if your Qt 5.6.0 sources come from somewhere else.

---

## 9. 

**Date:** Posted: Mar 10, 2016  
**Link:** [https://hex-rays.com/blog/ida-windows-system-python-2-7-11](https://hex-rays.com/blog/ida-windows-system-python-2-7-11)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA + Windows + system python 2.7.11

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-windows-system-python-2-7-11>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-windows-system-python-2-7-11>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-windows-system-python-2-7-11>)

Arnaud Diederen ✦ Posted: Mar 10, 2016

![IDA + Windows + system python 2.7.11](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In case you:

  * are running IDA on Windows 
  * are using the system’s Python (as opposed to the bundled Python distribution, that one can opt for at installation-time) 
  * have installed the recently released [Python 2.7.11](<https://www.python.org/downloads/release/python-2711/>)



…you will have noticed that IDAPython fails to load the ‘site’ module and, consequently, IDAPython is not available.

As far as I understand, this is a Python installer issue, which I reported ( <https://bugs.python.org/issue25824> ).  
While waiting for feedback from the Python authors, we can already offer the two following solutions:

  1. remove Python 2.7.11 and pick an earlier version (2.7.10 is known to work) 
  2. open regedit.exe, and rename the key:  
HKLM\Software\Wow6432Node\Python\PythonCore\2.7\PythonPath into  
HKLM\Software\Wow6432Node\Python\PythonCore\2.7-32\PythonPath 



This post will be updated whenever more information becomes available on the Python side.  
HTH!  
(Thanks to Tamir Bahar, who reported this issue very early on!)

---

