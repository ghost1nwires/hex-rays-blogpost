# Hex-Rays Blog – Page 40

**Archived:** 2026-02-20 12:22
**URL:** https://hex-rays.com/blog/page/40

---

## 1. 

**Date:** Posted: Mar 10, 2016  
**Link:** [https://hex-rays.com/blog/ida-6-9-mac-os-x-random-crashes](https://hex-rays.com/blog/ida-6-9-mac-os-x-random-crashes)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 6.9, Mac OS X, 'random' crashes

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-6-9-mac-os-x-random-crashes>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-6-9-mac-os-x-random-crashes>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-6-9-mac-os-x-random-crashes>)

Hex-Rays ✦ Posted: Mar 10, 2016

![IDA 6.9, Mac OS X, 'random' crashes](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

## Intended audience

IDA 6.9 users on Mac OS X, who have suffered seemingly-apparent crashes while using IDA.

## The problem

The Qt 5.4.1 libraries shipped with IDA 6.9 suffer from the following bug: <https://bugreports.qt.io/browse/QTBUG-44708>, which was apparently fixed in Qt 5.5.0.

If, when IDA crashes, you ever spotted a backtrace that looks like the following:
    
    
    frame #0: 0x00000000
    frame #1: 0x00d8a50d QtGui'QT::QTextEngine::shapeText(int) const + 1187
    frame #2: 0x00d8b517 QtGui'QT::QTextEngine::shape(int) const + 1199
    frame #3: 0x00d8c977 QtGui'QT::QTextEngine::width(int, int) const + 155
    frame #4: 0x00d73571 QtGui'QT::QFontMetricsF::width(QT::QString const&) const + 163
    frame #5: 0x00041184 idaq'___lldb_unnamed_function853$$idaq + 420
    ...
    

then you’ve been a victim of this rather tiresome issue.  
(note: frame #0 doesn’t quite matter; the 2nd line, `QT::QTextEngine::shapeText(int)`, is the important one)

## The solution

We have applied the patch mentionned in the Qt bugreport & re-built the `libqcocoa.dylib` Qt platform support.  
You will have to:

  1. download & unzip the following archive: [qcocoa.zip](</wp-content/uploads/2016/03/qcocoa.zip>). 
  2. verify the shasum of the libqcocoa.dylib file: 
         
         $ shasum libqcocoa.dylib
         afcf3603f593776c6f39f41f81e98843897cf0ed  libqcocoa.dylib
         

  3. place the `libqcocoa.dylib` binary instead of the one in `/path/to/IDA_6.9/idaq.app/Contents/Plugins/`



Once that is done, those crashes shouldn’t happen anymore. 

A big, _big_ thank you to Willem Jan Hengeveld & Vladimir Putin, who have reported this!

---

## 2. 

**Date:** Posted: Feb 29, 2016  
**Link:** [https://hex-rays.com/blog/beware-ida-c-plugins-qt-5-x-qstringliteral-crash-at-exit-time](https://hex-rays.com/blog/beware-ida-c-plugins-qt-5-x-qstringliteral-crash-at-exit-time)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Beware: IDA C++ plugins, Qt 5.x, QStringLiteral: crash at exit-time

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/beware-ida-c-plugins-qt-5-x-qstringliteral-crash-at-exit-time>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/beware-ida-c-plugins-qt-5-x-qstringliteral-crash-at-exit-time>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/beware-ida-c-plugins-qt-5-x-qstringliteral-crash-at-exit-time>)

Arnaud Diederen ✦ Posted: Feb 29, 2016

![Beware: IDA C++ plugins, Qt 5.x, QStringLiteral: crash at exit-time](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

## Intended audience

IDA C++ plugin authors, who wish to link such plugins against Qt 5.x libraries.

## The problem

One of our customers, Aliaksandr Trafimchuk, recently reported that whenever IDA was run with a plugin of his that links against the Qt libraries that we ship, IDA would crash at exit-time (at least on Windows.)

Aliaksandr already did most of the work of figuring out exactly what was causing the crash, and even had a work-around (more like a kludge, as he pointed out, really) for it, but he still wanted to let us know about it so we are aware of the problem & perhaps can communicate about it.

The crash is an access violation, in an area of memory that doesn’t seem to be mapped by any stack, heap, DLL code or data.

The stack reveals that the crash happens at `QCoreApplication::~QCoreApplication()`-time (i.e., at application exit), when the `QFontCache` is freeing/releasing its entries:
    
    
      Qt5Core.dll!QT::QSettingsGroup::~QSettingsGroup()
      Qt5Gui.dll!QT::QMapNode::destroySubTree()
      Qt5Gui.dll!QT::QFontCache::clear()
      Qt5Gui.dll!QT::QFontCache::~QFontCache()
      [External Code]
      Qt5Gui.dll!QT::QThreadStorage::deleteData(void * x)
      Qt5Core.dll!QT::QThreadStorageData::set(void * p)
      Qt5Gui.dll!QT::QFont::cleanup()
      Qt5Gui.dll!QT::QGuiApplicationPrivate::~QGuiApplicationPrivate()
      [External Code]
      Qt5Core.dll!QT::QObject::~QObject()
      Qt5Core.dll!QT::QCoreApplication::~QCoreApplication()
      idaq.exe!013426c5() Unknown
      (...)
    

## Why does that happen?

Our customer’s plugin uses a UI description file, that needs to be processed by Qt’s `uic` (UI-compiler). The generated code contains lines such as these:
    
    
    label = new QLabel(TestDialog);
    label->setObjectName(QStringLiteral("label"));
    QFont font;
    font.setFamily(QStringLiteral("Comic Sans MS"));
    

Note the use of `QStringLiteral`.

## The `QStringLiteral` type

This is an optimization that came in Qt 5.x, and that causes actual `QString` instances to be laid out in the `.rodata` section of the program (together with a special refcount value that is `-1`, meaning “don’t touch this refcount”.)

Although at exit-time, this “static const” `in-.rodata-QString` instance wouldn’t be modified (because of the -1 refcount), simply reading it will cause a crash, since the section holding it has been removed from memory. 

This is a known limitation/problem, too: <https://bugreports.qt.io/browse/QTBUG-46880>

## The plugin lifecycle

This is where the problem lies: at exit-time, IDA will:

  1. unload plugins 
  2. proceed until its `QCoreApplication` goes out of scope, which will perform (among other things) the `QFontCache` cleanup. 
  3. alas, at that time, the `QFontCache` still refers to literal `QString` data, in a section that is now gone (it was discarded at #1) 



In fact, Qt expects that any binary that uses Qt libraries should remain in memory, so that some optimizations (such as the `QStringLiteral`) will continue to work. That’s why, when Qt unloads some of its **_own_** plugins, [it doesn’t really unload those from memory](<http://lists.qt-project.org/pipermail/development/2015-November/023691.html>). 

Although the Qt library maintainers consider that having such limitations on binaries that link against Qt is [acceptable](<http://lists.qt-project.org/pipermail/development/2015-November/023707.html>), I personally hope they try to keep those restrictions as minimal as possible. 

## The solution

In any case, concerning this `QStringLiteral` issue, we have a way out: at compilation-time, pass the compiler the following flag: `-DQT_NO_UNICODE_LITERAL`

This will turn the `QStringLiteral()` expression into a `QString::fromUtf8()`, which will allocate the memory on the heap and the plugin should work just fine.

* * *

## Another possible solution

Another possibility reported by an IDA user (but untested by us), is to add the following after the Qt headers #include directives:
    
    
    #undef QStringLiteral
    #define QStringLiteral(_str) _str
    

With this method, the literal C-style string will be implicitly converted to a `QString`, using the default conversion rules. 

* * *

### Footnotes

The kludge (which is Windows-specific) consists of calling `LoadLibrary(szMyPluginFilePath)`, thereby somewhat artificially incrementing the refcount of his plugin, which will cause it to remain in memory & thus the `~QFontCache` cleanup will succeed.

---

## 3. 

**Date:** Posted: Dec 30, 2015  
**Link:** [https://hex-rays.com/blog/plugins-migrating-pyside-code-to-pyqt5](https://hex-rays.com/blog/plugins-migrating-pyside-code-to-pyqt5)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDAPython: migrating PySide code to PyQt5

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugins-migrating-pyside-code-to-pyqt5>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugins-migrating-pyside-code-to-pyqt5>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugins-migrating-pyside-code-to-pyqt5>)

Arnaud Diederen ✦ Posted: Dec 30, 2015

![IDAPython: migrating PySide code to PyQt5](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

### Background

Contrary to previous versions that shipped with Qt 4.8.4, IDA 6.9 ships with Qt 5.4.1 and as we announced [some time ago](<http://www.hexblog.com/?p=906>), this will force some changes for plugin writers who were using IDAPython + PySide in order to build custom interfaces.  
What’s more, in addition to the Qt4 -> Qt5 switch, we have also chosen to drop PySide in favor of PyQt5, as it seems to be more actively developed.

### Porting

While we were porting some internal plugins & test scripts, we have come across a number of ‘issues’ that are due both to the change in major Qt version, and also to the switch to PyQt5.  
Here is a (non-exhaustive) list of such problems that needed to be dealt with.

#### Use ‘from PyQt5’ instead of ‘from PySide’
    
    
       from PySide import QtCore, QtGui
    

Becomes:
    
    
       from PyQt5 import QtCore, QtGui, QtWidgets
    

(notice the added ‘QtWidgets’. Keep reading.)

#### Most widgets are now in QtWidgets, not QtGui anymore (Qt5 architecture change)
    
    
       from PySide import QtGui
       edit = QtGui.QTextEdit()
    

Becomes:
    
    
       from PyQt5 import QtWidgets
       edit = QtWidgets.QTextEdit()
    

#### Converting TForm instances must be done to PyQt
    
    
       from PySide import QtGui, QtCore
       form = idaapi.find_tform("Output window")
       w = idaapi.PluginForm.FormToPySideWidget(form)
    

Becomes:
    
    
       from PyQt5 import QtGui, QtCore, QtWidgets
       form = idaapi.find_tform("Output window")
       w = idaapi.PluginForm.FormToPyQtWidget(form)
    

#### Public QEvent types are in QtCore.QEvent namespace
    
    
       QtCore.QEvent.Type.XXX
    

Becomes:
    
    
       QtCore.QEvent.XXX
    

#### Using QObject.connect() to connect signal with slot doesn’t work anymore. Use alternative approach.
    
    
       obj.connect(query_action, QtCore.SIGNAL("triggered()"), self.ui.queryGraph)
    

Becomes:
    
    
       query_action.triggered.connect(self.ui.queryGraph)
    

#### Misc. required changes (reported by our users)

  * QHeaderView.setResizeMode => QHeaderView.setSectionResizeMode 
  * QtCore.Qt.ItemFlag enum was moved to be accessed directly on QtCore.Qt 
  * QColorDialog.setCustomColor now takes a QColor instead of an RGB int as the second argument. 



### Final words

As I said above, this is a _non-exhaustive_ list of issues we encountered. Should you encounter other issues, please let us know about them, so we can update & improve this page to help the community!

---

## 4. 

**Date:** Posted: Dec 29, 2015  
**Link:** [https://hex-rays.com/blog/ida-6-9-qt-5-4-1-configure-options-patch](https://hex-rays.com/blog/ida-6-9-qt-5-4-1-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 6.9: Qt 5.4.1 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-6-9-qt-5-4-1-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-6-9-qt-5-4-1-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-6-9-qt-5-4-1-configure-options-patch>)

Arnaud Diederen ✦ Posted: Dec 29, 2015

![IDA 6.9: Qt 5.4.1 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

A handful of our users have already requested information regarding the Qt 5.4.1 build, that is shipped with IDA 6.9.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.4.1\configure.bat "-debug-and-release" "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-platform" "win32-msvc2015" "-opengl" "desktop" "-force-debug-info" "-prefix" "C:/Qt/5.4.1"`
    * Note that you will have to build with Visual Studio 2015, to obtain compatible libs 
  * Linux: `.../5.4.1/configure "-debug-and-release" "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-platform" "linux-g++-32" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.4.1"`
  * Mac OSX: `.../5.4.1/configure "-debug-and-release" "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-platform" "macx-g++-32" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.4.1"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2015/12/full_patch.zip>). You should therefore apply it to your own Qt 5.4.1 sources before compiling, in order to obtain similar binaries.

---

## 5. 

**Date:** Posted: Dec 23, 2015  
**Link:** [https://hex-rays.com/blog/installing-ida-6-9-on-debian-linux-derivatives-ubuntu-etc](https://hex-rays.com/blog/installing-ida-6-9-on-debian-linux-derivatives-ubuntu-etc)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Installing IDA 6.9 on Linux

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/installing-ida-6-9-on-debian-linux-derivatives-ubuntu-etc>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/installing-ida-6-9-on-debian-linux-derivatives-ubuntu-etc>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/installing-ida-6-9-on-debian-linux-derivatives-ubuntu-etc>)

Arnaud Diederen ✦ Posted: Dec 23, 2015

![Installing IDA 6.9 on Linux](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

IDA is still, as of this writing (December 23rd, 2015), a 32-bit application and both IDA & its installer(*) require certain 32-bit libraries to be present on your Linux system before they can run.  
Here is the list of commands you will have to run in order to install those dependencies, for the following systems:

  * Debian & derivative systems such as Ubuntu, Xubuntu, … 
  * Red Hat Enterprise Linux 7.2 (and likely other versions as well) 



Note: we cannot possibly install & try IDA on all flavors/versions of all Linux distributions, but we will do our best to update this post with relevant information, whenever we learn of a distribution requiring special attention.

(*) that is: if you want the installer to run a graphical interface, instead of a command-line one.

## Debian & Ubuntu

### Common dependencies

The following should allow IDA to run on most Linux systems deriving from Debian distributions:
    
    
    sudo dpkg --add-architecture i386
    sudo apt-get update
    sudo apt-get install libc6-i686:i386 libexpat1:i386 libffi6:i386 libfontconfig1:i386 libfreetype6:i386 libgcc1:i386 libglib2.0-0:i386 libice6:i386 libpcre3:i386 libpng12-0:i386 libsm6:i386 libstdc++6:i386 libuuid1:i386 libx11-6:i386 libxau6:i386 libxcb1:i386 libxdmcp6:i386 libxext6:i386 libxrender1:i386 zlib1g:i386 libx11-xcb1:i386 libdbus-1-3:i386 libxi6:i386 libsm6:i386 libcurl3:i386
    

### Xubuntu 15.10

It is necessary to also run those commands, for IDA to present a usable GUI on Xubuntu 15.10
    
    
    sudo apt-get install libgtk2.0-0:i386 gtk2-engines-murrine:i386 gtk2-engines-pixbuf:i386
    

## Red Hat Enterprise Linux 7.2

IDA will require the following packages to be installed, in order to run properly on RHEL 7.2 (and probably any other RPM-based distribution) :
    
    
    redhat-lsb-core.i686
    glib2.i686
    libXext.i686
    libXi.i686
    libSM.i686
    libICE.i686
    freetype.i686
    fontconfig.i686
    dbus-libs.i686

---

## 6. 

**Date:** Posted: Jul 17, 2015  
**Link:** [https://hex-rays.com/blog/hack-of-the-day-0-somewhat-automating-pseudocode-html-generation-with-idapython](https://hex-rays.com/blog/hack-of-the-day-0-somewhat-automating-pseudocode-html-generation-with-idapython)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hack of the day #0: Somewhat-automating pseudocode HTML generation, with IDAPython.

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hack-of-the-day-0-somewhat-automating-pseudocode-html-generation-with-idapython>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hack-of-the-day-0-somewhat-automating-pseudocode-html-generation-with-idapython>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hack-of-the-day-0-somewhat-automating-pseudocode-html-generation-with-idapython>)

Arnaud Diederen ✦ Posted: Jul 17, 2015

![Hack of the day #0: Somewhat-automating pseudocode HTML generation, with IDAPython.](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

### The problem

As you may already know1, Hex-Rays decompilers can generate HTML files from pseudocode windows.  
That feature, however, is limited to generating HTML for a single function, or a portion of a function.

Recently, one of our customers asked us whether there was a way to generate HTML files for multiple functions all at once. I was at the regret of telling him that, no, we don’t have that feature (and it doesn’t really seem to be of interest to many, since AFAICT we have received no request for it) 

However, after the [Great Actions Refactoring™](<http://www.hexblog.com/?p=886>), decompiler action are now 1st class IDA citizens (just like any other plugin actions, really.) That means we can now invoke them, just like we could already invoke any IDA core action. We just need to take a few precaution, is all. 

### A first approach

Here’s a piece of IDAPython code I hammered into shape, that will go through all the functions of the program and (if the current function is decompilable) will prompt the user for saving: 
    
    
    from idaapi import *
    from PySide import QtGui, QtCore
    # Our piece of code relies on the user having already opened a pseudocode view. That's a limitation, but oh well.
    tform = find_tform("Pseudocode-A")
    if not tform:
        raise Exception("Please open a pseudocode view")
    qwidget = PluginForm.FormToPySideWidget(tform)
    # Iterate all funcs
    for fidx in xrange(get_func_qty()):
        f = getn_func(fidx)
        try:
            if decompile(f):
                # Ok, we can jump there w/o being
                # switched to "IDA View-A"
                jumpto(f.startEA)
                # Needed. W/o that, focus might be
                # elsewhere, and the 'hx:GenHtml'
                # will refuse to work.
                qwidget.setFocus()
                process_ui_action("hx:GenHtml")
        except DecompilationFailure:
            print "Decompilation failure: %x. Skipping."  % f.startEA
            pass
    

Notes:

  * I only tried that with IDA 6.8, on a Linux box. 
  * this is *NOT* meant to be an elegant, comfortable or rock-solid approach to generating HTML files with pseudocode (hence the ‘Hack of the day’ subject of this post!) 
  * this is really just meant to illustrate how IDA APIs can be (ab)used, and perhaps give inspiration to some readers for some of their use-cases 
  * this will go through _all_ functions in the program. If you just need to print 20 functions out of 13942, it might become tedious to reject (13942 – 20 =) 13922 “Save file” dialogs. 



### Improving that code, so only the selected functions will be printed

A possibly nice improvement to this, would be to know what functions the user selected in the “Functions window”, and iterate over those only, prompting for save. 

Here’s how I would go about it, knowing that:

  * the ‘Functions window’ is what we call, in IDA-land, a ‘chooser’ 
  * alas, this is not a user-controlled chooser 
  * thus we don’t control the data being printed, nor do we get called back whenever something gets selected/deselected… 

#### Great Actions Refactoring to the rescue again!

We will:

    1. create an “Generate HTML files” action that, basically, wraps the above code, then 
    2. register a ‘popup being populated’ hook: when the popup is for the ‘Functions window’ we will attach our new action to that popup 
    3. then, when that action is invoked (i.e., user clicked it), in our action handler we will retrieve the list of selected indices and iterate over those, prompting for save 
    
    from idaapi import *
    from PySide import QtGui, QtCore
    # Register action
    class GenMultiHTML(action_handler_t):
        def __init__(self):
            action_handler_t.__init__(self)
        def activate(self, ctx):
            tform = find_tform("Pseudocode-A")
            if not tform:
                raise Exception("Please open a pseudocode view")
            qwidget = PluginForm.FormToPySideWidget(tform)
            for fidx in ctx.chooser_selection:
                f = getn_func(fidx - 1)
                try:
                    if decompile(f):
                        # ok, we can jump there w/o being
                        # switched to "IDA View-A"
                        jumpto(f.startEA)
                        qwidget.setFocus()
                        process_ui_action("hx:GenHtml")
                except DecompilationFailure:
                    print "Decompilation failure: %x. Skipping."  % f.startEA
                    pass
            return 1
        def update(self, ctx):
            return AST_ENABLE_FOR_FORM if ctx.form_title == "Functions window" else AST_DISABLE_FOR_FORM
    register_action(
        action_desc_t(
            'my:gen_multi_html',
            'Generate HTML files',
            GenMultiHTML()))
    # Listen to 'finish populating popup' hook, to add our
    # action if the popup is for the "Functions window"
    class Hooks(idaapi.UI_Hooks):
        def finish_populating_tform_popup(self, form, popup):
            if get_tform_title(form) == "Functions window":
                attach_action_to_popup(
                    form,
                    popup,
                    "my:gen_multi_html",
                    None)
    hooks = Hooks()
    hooks.hook()
    

Happy hacking! 

* * *

Footnotes

#1: And if you didn’t know: go to a pseudocode window, click on the function’s title (i.e., the first line), and open the context menu. You can also select a portion of text, and generate HTML just for that.

---

## 7. 

**Date:** Posted: Apr 9, 2015  
**Link:** [https://hex-rays.com/blog/idapython-pysidepyqt-future-plans](https://hex-rays.com/blog/idapython-pysidepyqt-future-plans)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDAPython + PySide/PyQt: future plans

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/idapython-pysidepyqt-future-plans>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/idapython-pysidepyqt-future-plans>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/idapython-pysidepyqt-future-plans>)

Arnaud Diederen ✦ Posted: Apr 9, 2015

![IDAPython + PySide/PyQt: future plans](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

# Intended audience

IDAPython plugin writers who are using the PySide Qt bindings.

# PySide: some background

For some time now it has been possible, through IDAPython, to use PySide bindings to the Qt libraries that are shipped with IDA.

Those PySide bindings were first placed on [Hex-Rays’s website](<https://www.hex-rays.com/products/ida/support/download.shtml>) and, since we noticed a considerable interest for them, we later decided to ship them with IDA (starting at IDA 6.6.)

## What about PyQt?

The choice of PySide over PyQt was essentially due to incompatibilities between the licensing model of PyQt, and that of Hex-Rays.

# The problem

PySide appears to have pretty much stopped evolving, and now remains stuck with Qt 4 (i.e., the 4.8 branch.)  
PySide isn’t available for Qt 5 and, as of today, the last update of the [PySide roadmap](<http://qt-project.org/wiki/PySide_Roadmap>) dates back to “March 26, 2013”.

There is still some activity on the PySide mailing list, but despite that, not much seems to be happening (and the latest release is from 1 year ago.)

This is very problematic for us as Qt 4 is becoming more and more irrelevant by the day, and we would love to move to Qt 5.

We could, in theory, invest in porting PySide to support Qt 5, but it would probably lead us to an undesirable situation where PySide is pretty much used and maintained by Hex-Rays only.

# Meanwhile, in PyQt land

In the meantime, the PyQt people at <http://www.riverbankcomputing.co.uk/> have changed their licensing options, and after having contacted them recently, we have found that we can now consider the following:

  * Hex-Rays can acquire PyQt licenses
  * with those, we can build PyQt, and ship it with IDA
  * IDA users can make use of PyQt themselves



In other words, we will be able to provide a replacement for PySide, that:

  * is widely used, and has momentum 
  * supports Qt 5 



# Our plan

Our plan is to roll, in a few days, IDA 6.8 that still depends on Qt 4, and still ships with PySide.

The release after that (i.e., 6.9) will then depend on Qt 5, and come with PyQt.

# What that means for developers

Thankfully, the interfaces of PySide and PyQt are highly compatible, and thus it should not be necessary to rewrite your plugins from scratch (in fact, very little should have to be modified.)

I would recommend you to stay tuned to [the Hex-Rays forums](<https://hex-rays.com/forum/>), so that you can participate in the beta program for IDA 6.9 once it is announced. That will give you the possibility of porting your tools to PyQt before IDA 6.9 is released. We will also provide guidance on how to approach that porting effort (even though, once again, it should be fairly small.)

As you may already know, we are very reluctant to any kind of API/compatibility breakage. But unfortunately, it seems we simply don’t have a choice in this particular case.

---

## 8. 

**Date:** Posted: Dec 24, 2014  
**Link:** [https://hex-rays.com/blog/augmenting-ida-ui-with-your-own-actions](https://hex-rays.com/blog/augmenting-ida-ui-with-your-own-actions)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Augmenting IDA UI with your own actions.

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/augmenting-ida-ui-with-your-own-actions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/augmenting-ida-ui-with-your-own-actions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/augmenting-ida-ui-with-your-own-actions>)

Arnaud Diederen ✦ Posted: Dec 24, 2014

![Augmenting IDA UI with your own actions.](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

# Intended audience

Plugin writers, either using the C SDK or IDAPython, who would like to add actions/commands to IDA UI in order to augment its capabilities.

# Rationale: before 6.7

## APIs galore

Depending on what type of context you were in, various APIs were available to you:

  * Want to add a main menu item?
        
        add_menu_item(const char *menupath, const char *name, const char *hotkey, int flags, menu_item_callback_t *callback, void *ud);
        del_menu_item(const char *menupath)
        

  * Want to add an action to a list view? (or, as we call them: ‘choosers’)
        
        add_chooser_command(const char *chooser_caption, const char *cmd_caption, chooser_cb_t *chooser_cb, int menu_index=-1, int icon=-1, int flags=0);
        add_chooser_command(const char *chooser_caption, const char *cmd_caption, chooser_cb_t *chooser_cb, const char *hotkey, int menu_index=-1, int icon=-1, int flags=0);
        

…or through populating callbacks:
        
        struct chooser_info_t
        {
            // ...snipped...
            // function is called every time before
            // context menu is shown to fill user_cmds
            prepare_popup_cmds_t *prepare_popup_cmds;
        };
        

  * Want to add/manage the items that show up in the context menu for a custom code viewer you have created? (that does not work for the ‘core’ viewers like ‘IDA View-A’!)
        
        set_custom_viewer_popup_menu(TCustomControl *custom_viewer, TPopupMenu *menu);
        add_custom_viewer_popup_item(TCustomControl *custom_viewer, const char *title, const char *hotkey, menu_item_callback_t *cb, void *ud);
        

  * Want to add/manage the items that show up in the context menu for a custom graph viewer you have created? (again, doesn’t work with ‘core’ viewers)
        
        viewer_add_menu_item(graph_viewer_t *gv, const char *title, menu_item_callback_t *callback, void *ud, const char *hotkey, int flags);
        

  * Want to add an item to the Output window’s context menu?
        
        add_output_popup(char *n, menu_item_callback_t *c, void *u);
        




## Drawbacks

Note that not all of those functions take the same set of parameters, or even the same type for seemingly-related ones (e.g., `chooser_cb_t *chooser_cb` VS `menu_item_callback_t *callback`)

It is also not possible to specify some attributes of the commands in some cases. E.g.,

  * `add_menu_item()` doesn’t allow to specify an icon
  * `add_output_popup()` doesn’t accept a shortcut
  * …



And it’s just plain impossible to do some things. E.g.,

  * augment IDA View’s context menu with your own actions  
**EDIT: This is incorrect, as the ‘view_popup’ notification could be used for this. However, this is still limited to IDA View-A, and cannot be used for other widgets.**
  * remove an item that was previously added to the Output window’s context menu through `add_output_popup()`.
  * in some cases, actions shortcuts would do nothing if the context menu wasn’t currently displayed (i.e., showing the context menu was necessary to enabled the hotkeys)
  * adding custom toolbar buttons was impossible.



These inconsistencies/lack of features were making writing extensions difficult, as the wealth of APIs was somewhat confusing and it was easy to hit the limitations.

In addition, there was no really good way of enabling/disabling your actions, or changing their properties after adding them.

Thus, we decided a refactoring of those APIs was in order.

# The new actions API

Borrowing some concepts from Qt itself (upon which IDA’s graphical interface relies) we have decided to redesign the actions API in a hopefully more streamlined manner.

## Key aspects

  * An action first needs to be registered. Once registered, it can be triggered using its shortcut (if one was specified), but it doesn’t appear anywhere in the UI just yet.
  * Once registered, it is possible attach/detach that action to/from things: 
    * Main menus
    * Toolbars
    * Context menus
  * The same action can be used in multiple places.
  * An action has a “handler”, which is a small structure consisting of two callbacks: 
    * an ‘activate’ callback, called when the action was triggered (i.e., user selected a menu item, or entered a shortcut, or pressed a toolbar item, …)
    * an ‘update’ callback, which is responsible for two things: 
      * Declaring whether the action is currently enabled, and when it should be queried again for that information.
      * Possibly updating the action’s state (e.g., text, icon, …) (although that’s rarely needed.)



What follows is a series of short examples, using the IDAPython bindings for actions. Note that the IDAPython API is purposely similar to the C SDK API.

## Registering an action
    
    
        # 1) Create the handler class
        class MyHandler(idaapi.action_handler_t):
            def __init__(self):
                idaapi.action_handler_t.__init__(self)
            # Say hello when invoked.
            def activate(self, ctx):
                print "Hello!"
                return 1
            # This action is always available.
            def update(self, ctx):
                return idaapi.AST_ENABLE_ALWAYS
        # 2) Describe the action
        action_desc = idaapi.action_desc_t(
            'my:action',   # The action name. This acts like an ID and must be unique
            'Say hello!',  # The action text.
            MyHandler(),   # The action handler.
            'Ctrl+H',      # Optional: the action shortcut
            'Says hello',  # Optional: the action tooltip (available in menus/toolbar)
            199)           # Optional: the action icon (shows when in menus/toolbars)
        # 3) Register the action
        idaapi.register_action(action_desc)
    

From there on, the action is created and, although it has not yet been attached to menus, toolbars or context menus, it can already be invoked by pressing Ctrl+H: 

This is slightly more code than before (and in this case, it is explicitely verbose), but regardless of the context/widget in which you want to use that action, it will always be the same API.

Note: In this example, we used icon number 199, which corresponds to a stock icon. It is perfectly possible to define your own icons by using the API function `load_custom_icon()`. See `kernwin.hpp` for more information.

## Inserting the action into the menu
    
    
        idaapi.attach_action_to_menu(
            'Edit/Other/Manual instruction...', # The relative path of where to add the action
            'my:action',                        # The action ID (see above)
            idaapi.SETMENU_APP)                 # We want to append the action after the 'Manual instruction...'
    

## Inserting the action into a toolbar
    
    
        idaapi.attach_action_to_toolbar(
            "AnalysisToolBar",  # The toolbar name
            'my:action')        # The action ID
    

## Inserting the action into a widget’s context menu

There are two ways to add actions to a widget’s context menu:

  * Permanently: the action will be there every time the context menu is shown
  * On the fly: you can attach an action as the context menu is being populated, and the action will be displayed in this invocation of the context menu, but not others (unless you attach it again, obviously.)



### Attaching an action permanently

To permanently attach an action to a widget’s context menu:
    
    
        # Create a widget, or retrieve a pointer to it.
        form = idaapi.get_current_tform()
        idaapi.attach_action_to_popup(form, None, "my:action", None)
    

Notice that:

  * We used `get_current_tform()`, to retrieve the widget that currently has the keyboard focus.
  * We passed `None` as second parameter to `attach_action_to_popup()`. That means we want to attach the action to that widget permanently.



Pros:

  * Trivial to use.
  * This is what you typically want to use when you create your own widgets



Cons:

  * Only feasible when you can get a reference (i.e., pointer) to the widget.



### Attaching an action on-the-fly

To attach an action to a context menu when it is being created, you need to listen to a UI notification, and use the same `attach_action_to_popup` function:
    
    
        class Hooks(idaapi.UI_Hooks):
            def populating_tform_popup(self, form, popup):
                # You can attach here.
                pass
            def finish_populating_tform_popup(self, form, popup):
                # Or here, after the popup is done being populated by its owner.
                # We will attach our action to the context menu
                # for the 'Functions window' widget.
                # The action will be be inserted in a submenu of
                # the context menu, named 'Others'.
                if idaapi.get_tform_type(form) == idaapi.BWN_FUNCS:
                    idaapi.attach_action_to_popup(form, popup, "my:action", "Others/")
        hooks = Hooks()
        hooks.hook()
    

Notice that:

  * This time, the 2nd parameter is _not_ ‘None’. That instructs the function to attach the action for this invocation only.
  * We know that we are currently in the “Functions window”, by checking the type of the current form against `BWN_FUNCS`. Many other `BWN_*` values are available; see `kernwin.hpp` for a list of those!
  * As an alternative, we could check the form’s title, using `get_tform_title(form)`.



Pros:

  * Finer-grained control.
  * You typically want to use this when you cannot easily retrieve a reference (i.e., pointer) to the widget.



Cons:

  * More difficult to use: requires UI hooks.



## A word about the `update` callback

As stated above, the update callback is responsible not only for possibly updating some of the actions properties (See the `update_action_*` functions in `kernwin.hpp`), but also for stating whether the action is currently available or not, and when `update` should be queried again.

Per contract, `update` should return one of the following values (again, from `kernwin.hpp`):
    
    
        AST_ENABLE_ALWAYS     // enable action and do not call action_handler_t::update() anymore
        AST_ENABLE_FOR_IDB    // enable action for the current idb. Call action_handler_t::update() when a database is opened/closed
        AST_ENABLE_FOR_FORM   // enable action for the current form. Call action_handler_t::update() when a form gets/loses focus
        AST_ENABLE            // enable action - call action_handler_t::update() when anything changes
        AST_DISABLE_ALWAYS    // disable action and do not call action_handler_t::action() anymore
        AST_DISABLE_FOR_IDB   // analog of ::AST_ENABLE_FOR_IDB
        AST_DISABLE_FOR_FORM  // analog of ::AST_ENABLE_FOR_FORM
        AST_DISABLE           // analog of ::AST_ENABLE
    

Thus, not only does `update` tell IDA whether the action is available or not, but it also tells IDA the “time span” during which the action will be (un)available.

For example, returning `AST_ENABLE_FOR_FORM` will tell IDA: “this action is available for the current widget; don’t call `update` again until the user switches to another widget.”

The `AST_*` values really represent various levels of “granularity”, where

  * `AST_ENABLE_ALWAYS` is the coarser level: the action will be available all the time, whatever happens.
  * …
  * `AST_ENABLE` is the finest level: whenever the user moves the cursor, switches to another form, triggers another action, …, `update` will be called again.



## “Dynamic” actions

In most cases, you will want your action to be registered in IDA, and be available through menus, shortcuts, etc…

But there can be “one-shot” situations where you really want to add an action to a context menu only once, and you really don’t want to go through the trouble of registering & deregistering the action.

For those “one-shot” situations, although they are rare, we have created a helper:
    
    
        class Hooks(idaapi.UI_Hooks):
            def finish_populating_tform_popup(self, form, popup):
                tft = idaapi.get_tform_type(form)
                if tft == idaapi.BWN_EXPORTS:
                    # Define a silly handler.
                    class MyHandler(idaapi.action_handler_t):
                        def activate(self, ctx):
                            print "Hello from exports"
                        def update(self, ctx):
                            return idaapi.AST_ENABLE_ALWAYS
                    # Note the 'None' as action name (1st parameter).
                    # That's because the action will be deleted immediately
                    # after the context menu is hidden anyway, so there's
                    # really no need giving it a valid ID.
                    desc = idaapi.action_desc_t(None, 'My dynamic action', MyHandler())
                    idaapi.attach_dynamic_action_to_popup(form, popup, desc, None)
        hooks = Hooks()
        hooks.hook()
    

The main difference When using `attach_dynamic_action_to_popup` is that the action will be available _only_ during the time the context menu is displayed. Once the context menu is hidden, the action will be unregistered & destroyed.

## Removing action from things

  * `detach_action_from_menu`
  * `detach_action_from_toolbar`
  * `unregister_action`



# Older APIs: Backward compatibility

All the previous API is still supported. We have fairly good test coverage of those, and therefore are quite confident the backward compatibility is working.

If your plugin suddenly stops working properly in 6.7, that’s a bug. Should that happen, please let us know about it and we’ll do our best to address it.

---

## 9. 

**Date:** Posted: Jul 11, 2014  
**Link:** [https://hex-rays.com/blog/ida-dalvik-debugger-tips-and-tricks](https://hex-rays.com/blog/ida-dalvik-debugger-tips-and-tricks)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [debugger](<https://hex-rays.com/blog/tag/debugger>) [Android](<https://hex-rays.com/blog/tag/android>) [Dalvik](<https://hex-rays.com/blog/tag/dalvik>)

# IDA Dalvik debugger: tips and tricks

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-dalvik-debugger-tips-and-tricks>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-dalvik-debugger-tips-and-tricks>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-dalvik-debugger-tips-and-tricks>)

Nikolay Logvinov ✦ Posted: Jul 11, 2014

![IDA Dalvik debugger: tips and tricks](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

One of the new features of IDA 6.6 is the Dalvik debugger, which allows us to debug Dalvik binaries on the bytecode level.

Let us see how it can help when analysing Dalvik files.

## Encoded strings

Let us consider the package with the encrypted strings:
    
    
    STRINGS:0001F143 unk_1F143:.byte 0x30 # 0  # DATA XREF: STR_IDS:off_70
    STRINGS:0001F144 aFda8sohchnidgh:
    .string **"FDA8sOhCHNidghM2hzFxMXUsivl2k7hFOhkJrW7O2ml8qLVM"** ,0
    STRINGS:0001F144                           # DATA XREF: q_b@V
    STRINGS:0001F144                           # String #277 (0x115)
    STRINGS:0001F175 unk_1F175:.byte 0x3C # <  # DATA XREF: STR_IDS:off_70
    STRINGS:0001F176 aCgv01n8li2s3ok:
    .string **"CGv01N8li2s3OKN29j6exe6-rvzgIRaCcWoOt5y30zjP1k43-f7WVOtXjbg="**
    STRINGS:0001F176                           # DATA XREF: q_b@V+C
    

There is a data reference, let us see where this string is used (e.g. using `Ctrl-X`).
    
    
    CODE:000090C0 const-string        v0, **aFda8sohchnidgh** # "FDA8sOhCH"...
    CODE:000090C4 invoke-static       {v0}, <ref **RC4.decryptBase64(ref)**
                                             RC4_decryptBase64@LL>
    CODE:000090CA move-result-object  v0
    

So, apparently the strings are encrypted with `RC4+Base64`.

Let us set a breakpoint after the `RC4.decryptBase64()` call and start the debugger.

After hitting the breakpoint, open the “Locals” debugger window. Even if the application was stripped of debug information, IDA makes the best effort to show function’s input arguments and return value.

[![dalvik_blog_pict1](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dalvik_blog_pict1-3.png?width=475&height=186&name=dalvik_blog_pict1-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/dalvik_blog_pict1-3.png>)

Note the local variable named `retval`. It is a synthetic variable created IDA to show the function return value.

This is how we managed to decode the string contents.

### How to debug Dalvik and ARM code together

Let us have a look at application that uses a native library. On a button press, the function `stringFromJNI()` implemented in the native library is called.
    
    
    package ida.debug.hellojni;
    public class MainActivity extends Activity {
        **public native String stringFromJNI();**
        static {
            **System.loadLibrary("hello-jni");**
        }
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            final TextView tv = new TextView(this);
            final Button btn = new Button(this);
            btn.setText("Press me to call the native code");
            btn.setOnClickListener(new Button.OnClickListener() {
                    public void onClick(View v) {
                            tv.setText(**stringFromJNI()**);
                    }
            });
            LinearLayout layout = new LinearLayout(this);
            layout.setOrientation(LinearLayout.VERTICAL);
            layout.setLayoutParams(new LayoutParams(
                LayoutParams.FILL_PARENT, LayoutParams.FILL_PARENT));
            layout.addView(btn);
            layout.addView(tv);
            setContentView(layout);
        }
    }
    

Native library function returns a well-known string.
    
    
    jstring Java_ida_debug_hellojni_MainActivity_stringFromJNI(
            JNIEnv* env,
            jobject thiz)
    {
      return (*env)->NewStringUTF(env, "Hello, JNI world!");
    }
    

So, we have application packaged in `hellojni.apk` file and installed in Android Emulator.

Because IDA cannot analyse or debug both Dalvik and native (ARM) code at the same time, we’ll need to use two IDA instances to perform debugging in turns.

To prepare the first IDA instance:

  1. load `hellojni.apk` into IDA, select `classes.dex` to analyze.

  2. go to the native function call and set breakpoint there.
         
         CODE:0000051C  iget-object         v0, this, MainActivity$1_val$tv
         CODE:00000520  iget-object         v1, this, MainActivity$1_this$0
         **CODE:00000524  invoke-virtual      {v1}, <ref MainActivity.stringFromJNI() imp. @ _def_MainActivity_stringFromJNI@L>**
         CODE:0000052A  move-result-object  v1
         

  3. change the default port in “Debugger/Process options” to any other value.

  4. start the Dalvik debugger and wait until breakpoint is hit.




Then prepare the second IDA instance:

  1. prepare to debug native ARM Android application (copy and start `android_server` and so on).

  2. load `hellojni.apk` into IDA, and now select `lib/armeabi-v7a/libhello-jni.so` to analyze.

  3. the name of the native function was formed by special rules and in our case it is `Java_ida_debug_hellojni_MainActivity_stringFromJNI()`, so go to to it and set a breakpoint:
         
         .text:00000BC4 EXPORT Java_ida_debug_hellojni_MainActivity_stringFromJNI
         .text:00000BC4 Java_ida_debug_hellojni_MainActivity_stringFromJNI
         .text:00000BC4
         .text:00000BC4 var_C  = -0xC
         .text:00000BC4 var_8  = -8
         .text:00000BC4
         **.text:00000BC4        STMFD   SP!, {R11,LR}**
         .text:00000BC8        ADD     R11, SP, #4
         .text:00000BCC        SUB     SP, SP, #8
         .text:00000BD0        STR     R0, [R11,#var_8]
         .text:00000BD4        STR     R1, [R11,#var_C]
         .text:00000BD8        LDR     R3, [R11,#var_8]
         .text:00000BDC        LDR     R3, [R3]
         .text:00000BE0        LDR     R2, [R3,#0x29C]
         .text:00000BE4        LDR     R0, [R11,#var_8]
         .text:00000BE8        LDR     R3, =(aHelloJniWorld - 0xBF4)
         .text:00000BEC        ADD     R3, PC, R3 ; "Hello, JNI world!"
         .text:00000BF0        MOV     R1, R3
         .text:00000BF4        BLX     R2
         .text:00000BF8        MOV     R3, R0
         .text:00000BFC        MOV     R0, R3
         .text:00000C00        SUB     SP, R11, #4
         .text:00000C04        LDMFD   SP!, {R11,PC}
         .text:00000C04 ; End of function
                          Java_ida_debug_hellojni_MainActivity_stringFromJNI
         

  4. select “Remote ARM Linux/Android debugger” and attach to the application process.

  5. press `F9` to continue.




Now switch to the first IDA session and press, for example, F8 to call native function. If we return back to the second IDA session then we can notice the breakpoint event.

[![dalvik_blog_pict2](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dalvik_blog_pict2-3.png?width=754&height=499&name=dalvik_blog_pict2-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/dalvik_blog_pict2-3.png>)

Now we can continue to debug the native code. When we finish, press `F9` and return to the first IDA session.

The full source code of the example you can download from our [site](<https://www.hex-rays.com/wp-content/uploads/2014/07/dalvik_blog_hellojni.zip>).

---

