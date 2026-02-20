# Hex-Rays Blog – Page 31

**Archived:** 2026-02-20 12:19
**URL:** https://hex-rays.com/blog/page/31

---

## 1. 

**Date:** Posted: Apr 23, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida](https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #36: Working with list views in IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida>)

I 

Igor Skochinsky ✦ Posted: Apr 23, 2021

![Igor’s tip of the week #36: Working with list views in IDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

List views (also called _choosers_ or table views) are used in many places in IDA to show lists of different kind of information. For example, the [Function list](<https://www.hex-rays.com/blog/igors-tip-of-the-week-28-functions-list/>) we’ve covered previously is an example of a list view. Many windows opened via the View > Open subviews menu are list views:

  * Exports
  * Imports
  * Names
  * Strings
  * Segments
  * Segment registers
  * Selectors
  * Signatures
  * Type libraries
  * Local types
  * Problems
  * Patched bytes



Many modal dialogs from the Jump menu (such as those for listing [Cross references](<https://www.hex-rays.com/blog/igor-tip-of-the-week-16-cross-references/>)) are also examples of list views. Because they are often used to select or choose one entry among many, they may also be called _choosers_.

List view can also be part of another dialog or widget, for example the shortcut list in the [Shortcut editor](<https://www.hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>). These are called “embedded choosers” in the IDA SDK.

All list views share common features which we discuss below.

### Searching

#### Text search

You can search for arbitrary text in the contents of the list view by using `Alt`–`T` to specify the search string and `Ctrl`–`T` to find the next occurrence.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chooser_search-Jun-18-2024-09-10-53-8398-AM.png?width=543&height=241&name=chooser_search-Jun-18-2024-09-10-53-8398-AM.png)

#### Incremental search

Simply start typing to navigate to the closest item which starts with the typed text. The text will appear in the status bar. Use `Backspace` to erase incorrectly typed letters and `Ctrl`–`Enter` to jump to the next occurrence of the same prefix (if any).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chooser_incr-Jun-18-2024-09-10-53-0363-AM.png?width=540&height=238&name=chooser_incr-Jun-18-2024-09-10-53-0363-AM.png)

### Columns

Each list view has column headers at the top. In most (not all) of them, you can hide specific columns by using “Hide column” or “Columns…” from the context menu.

Similarly to the standard list views in most OSes, you can resize columns by dragging the delimiters between them or auto-size the column to fit the longest string in it by double-clicking the right delimiter.

### Sorting

The list view can be sorted by clicking on a column’s header. The sorting indicator shows the direction of sorting (click it again to switch the direction). Because IDA needs to fetch the whole list of items to sort them, this can be slow in big lists so a reminder with the text “Caching <window>…” is printed in the Output window each time the list is updated and re-sorted. To improve the performance, you can disable sorting by using “Unsort” from the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chooser_sort-Jun-18-2024-09-10-57-3079-AM.png?width=489&height=296&name=chooser_sort-Jun-18-2024-09-10-57-3079-AM.png)

### Filtering

A quick filter box can be opened by pressing `Ctrl`–`F`. Type some text in it to only show items which include the typed substring. By default it performs case-insensitive match on all columns, however you can modify some options from the context menu, such as:

  * enable case-sensitive matching
  * match only whole words instead of any substring
  * enable fuzzy matching
  * interpret the entered string as a regular expression
  * pick a column on which to perform the matching



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chooser_quickfilter-Jun-18-2024-09-10-56-2846-AM.png?width=715&height=375&name=chooser_quickfilter-Jun-18-2024-09-10-56-2846-AM.png)

Instead of a quick filter, you can also use more complicated filtering (“Modify Filters” from context menu, or `Ctrl`–`Shift`–`F`). In this dialog you can not only include matching items, but also exclude or simply highlight them with a custom color.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/chooser_fullfilter-Jun-18-2024-09-10-54-9037-AM.png?width=619&height=274&name=chooser_fullfilter-Jun-18-2024-09-10-54-9037-AM.png)

Similarly to sorting, filtering requires fetching of the whole list which can slow down IDA, especially during autoanalysis. To remove any filters, choose “Reset filters” from the context menu.

See also: [How To Use List Viewers in IDA](<https://www.hex-rays.com/products/ida/support/idadoc/427.shtml>)

---

## 2. 

**Date:** Posted: Apr 16, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-35-demangled-names](https://hex-rays.com/blog/igors-tip-of-the-week-35-demangled-names)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #35: Demangled names

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-35-demangled-names>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-35-demangled-names>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-35-demangled-names>)

I 

Igor Skochinsky ✦ Posted: Apr 16, 2021

![Igor’s tip of the week #35: Demangled names](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

**Name mangling** (also called **name decoration**) is a technique used by compilers to implement some of the features required by the language. For example, in C++ it is used to distinguish functions with the same name but different arguments (_function overloading_), as well as to support namespaces _,_ templates, and other purposes.

Mangled names often end up in the final binary and, depending on the compiler, may be non-trivial to understand for a human (a simple example: “operator new” could be encoded as `??2@YAPAXI@Z` or `_Znwm`). While these cryptic strings can be decoded by a compiler-provided utility such as `undname` (MSVC) or `c++filt` (GCC/Clang), it’s much better if the disassembler does it for you (especially if you don’t have the compiler installed). This process of decoding back to a human-readable form is called **demangling**. IDA has out-of-box support for demangling names for the following compilers and languages:

  * Microsoft (Visual C++)
  * Borland (C++, Pascal, C++ Builder, Delphi)
  * Watcom (C++)
  * Visual Age (C++)
  * DMD (D language)
  * GNU mangling (GCC, Clang, some commercial compilers)
  * Swift



You do not need to pick the compiler manually; IDA will detect it from the name format and apply the corresponding demangler automatically.

### Demangled name options

By default, IDA uses a comment to show the result of demangling, meaning that every time a mangled name is used, IDA will print a comment with the result of demangling. For example, `?FromHandle@CGdiObject@@SGPAV1@PAX@Z` demangles to `CGdiObject::FromHandle(void *)`, which is printed as a comment:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/demangled_comment-Jun-18-2024-08-49-33-4406-AM.png?width=597&height=265&name=demangled_comment-Jun-18-2024-08-49-33-4406-AM.png)

If you prefer, you can show the demangled result in place of the mangled name instead of just a comment. This can be done in the Options > Demangled names… dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/demangled_names-Jun-18-2024-08-49-36-6089-AM.png?width=257&height=329&name=demangled_names-Jun-18-2024-08-49-36-6089-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/demangled_name-300x249-Jun-18-2024-08-49-34-6479-AM.png?width=300&height=249&name=demangled_name-300x249-Jun-18-2024-08-49-34-6479-AM.png)

### Short and long names

The buttons “Setup short names” and “Setup long names” allow you to modify the behavior of the built-in demangler in two common situations. The “short” names are used in contexts where space is at premium: references in disassembly, lists of functions and so on. “Long” names are used in other situations, for example when printing a comment at the start of the function. By using the additional options dialog, you can select what parts of the demangled name to show, hide, or shorten to make it either more compact or more verbose.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/demangled_short-Jun-18-2024-08-49-35-6792-AM.png?width=346&height=553&name=demangled_short-Jun-18-2024-08-49-35-6792-AM.png)

### Name simplification

Some deceptively simple-looking names may end up very complicated after compilation, especially when templates are involved. For example, a simple `[std::string](<https://en.cppreference.com/w/cpp/string>)` from STL actually expands to 
    
    
    std::basic_string<char,std::char_traits<char>,std::allocator<char>>

To ensure interoperability, the compiler has to preserve these details in the mangled name, so they reappear on demangling; however, such implementation details are usually not interesting to a human reader who would prefer to see a simple `std::string` again. This is why IDA implements name simplification as a post-processing step. Using the rules in the file `cfg/goodname.cfg`, IDA applies them to transform a name like 
    
    
    std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > & __thiscall std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >::erase(unsigned int,unsigned int)

into 
    
    
    std::string & std::string::erase(unsigned int,unsigned int)

which is much easier to read and understand.

IDA ships with rules for most standard STL classes but you can add custom ones too. Read the comments inside `goodname.cfg` for the description of how to do it.

More info: [Demangled names](<https://www.hex-rays.com/products/ida/support/idadoc/611.shtml>) in IDA Help.

---

## 3. 

**Date:** Posted: Apr 13, 2021  
**Link:** [https://hex-rays.com/blog/ida-7-6-empty-qtreeview-qtreewidget](https://hex-rays.com/blog/ida-7-6-empty-qtreeview-qtreewidget)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.6: empty QTreeView/QTreeWidget

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-6-empty-qtreeview-qtreewidget>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-6-empty-qtreeview-qtreewidget>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-6-empty-qtreeview-qtreewidget>)

Arnaud Diederen ✦ Posted: Apr 13, 2021

![IDA 7.6: empty QTreeView/QTreeWidget](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

A rather nasty issue evaded our testing and found its way into IDA 7.6: using the `PyQt5` modules that are shipped with IDA, `QTreeView` (or `QTreeWidget`) instances will always fail to display contents.

E.g., the following script
    
    
    from PyQt5 import QtWidgets
    
    tree = QtWidgets.QTreeWidget()
    item = QtWidgets.QTreeWidgetItem()
    item.setText(0, "Test col#0")
    tree.addTopLevelItem(item)
    tree.show()
    

used to render like so:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/tw75-300x256-Jun-18-2024-09-10-46-9161-AM.png?width=300&height=256&name=tw75-300x256-Jun-18-2024-09-10-46-9161-AM.png)

but now looks like this:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/tw76-300x227-Jun-18-2024-09-10-47-5078-AM.png?width=300&height=227&name=tw76-300x227-Jun-18-2024-09-10-47-5078-AM.png)

## The fix

In order to solve this, please download the fix corresponding to your IDA installation, and replace the original `QtWidgets` DLL with the one contained in the .zip file

  * Windows: [pyqt5_qtwidgets_win](<https://www.hex-rays.com/wp-content/uploads/2021/04/pyqt5_qtwidgets_win.zip>)
  * Linux: [pyqt5_qtwidgets_linux](<https://www.hex-rays.com/wp-content/uploads/2021/04/pyqt5_qtwidgets_linux.zip>)
  * MacOS (Intel): [pyqt5_qtwidgets_mac_x64](<https://www.hex-rays.com/wp-content/uploads/2021/04/pyqt5_qtwidgets_mac_x64.zip>)
  * MacOS (AppleSilicon): [pyqt5_qtwidgets_mac_arm](<https://www.hex-rays.com/wp-content/uploads/2021/04/pyqt5_qtwidgets_mac_arm.zip>)

---

## 4. 

**Date:** Posted: Apr 9, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-34-dummy-names](https://hex-rays.com/blog/igors-tip-of-the-week-34-dummy-names)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s tip of the week #34: Dummy names

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-34-dummy-names>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-34-dummy-names>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-34-dummy-names>)

I 

Igor Skochinsky ✦ Posted: Apr 9, 2021

![Igor’s tip of the week #34: Dummy names](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In IDA’s disassembly, you may have often observed names that may look strange and cryptic on first sight: `sub_73906D75`, `loc_40721B`, `off_40A27C` and more. In IDA’s terminology, they’re called _dummy_ names. They are used when a name is required by the assembly syntax but there is nothing suitable available, for example the input file has no debug information (i.e. it has been stripped), or when referring to a location not present in the debug info. These names are not actually stored in the database but are generated by IDA on the fly, when printing the listing.

### Dummy name prefixes

The dummy name consists of a type-dependent _prefix_ and a unique suffix which is usually address-dependent. The following prefixes are used in IDA:

  * `sub_` instruction, subroutine(function) start
  * `locret_` a return instruction
  * `loc_` other kind of instruction
  * `off_` data, contains an offset(pointer) value
  * `seg_` data, contains a segment address value
  * `asc_` data, start of a string literal
  * `byte_` data, byte
  * `word_` data, 16-bit
  * `dword_` data, 32-bit
  * `qword_` data, 64-bit
  * `byte3_` data, 3-byte
  * `xmmword_` data, 128-bit
  * `ymmword_` data, 256-bit
  * `packreal_` data, packed real
  * `flt_` floating point data, 32-bit
  * `dbl_` floating point data, 64-bit
  * `tbyte_` floating point data, 80-bit
  * `stru_` structure
  * `custdata_` custom data type
  * `algn_` alignment directive
  * `unk_` unexplored (undefined, unknown) byte



Because the prefixes are treated in a special way by IDA, they’re reserved and cannot be used in user-defined names. If you try to use such a name, you’ll get an error from IDA:

![Warning 328: can't rename byte as 'sub_x' because the name has a reserved prefix.](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dummy_error-Jun-18-2024-09-03-31-6844-AM.png?width=459&height=128&name=dummy_error-Jun-18-2024-09-03-31-6844-AM.png)

Warning: can’t rename byte because the name has a reserved prefix

A possible workaround is to add an underscore at the start so the prefix is different. But if you want to get rid of an existing name and have IDA use a dummy name again, just delete it (rename to an empty string).

### Name suffixes

The default suffix is the linear (aka effective) address of the item to which the dummy name is attached. However, this is not the only possibility. By using the Options > Name representation… dialog, you can choose something different.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dummy_names-Jun-18-2024-09-03-32-0009-AM.png?width=671&height=615&name=dummy_names-Jun-18-2024-09-03-32-0009-AM.png)

Dummy name representation dialog

The options from the first half can be especially useful when dealing with segmented programs such as 16-bit DOS software; instead of a global linear address you can see the segment and the offset inside it so, for example, it is evident when the destination is in another segment.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dummy_dos-Jun-18-2024-09-03-30-7681-AM.png?width=566&height=490&name=dummy_dos-Jun-18-2024-09-03-30-7681-AM.png)

DOS program when using “segment name & offset from the segment base” representation

### Other prefixes

In addition to dummy names, there are two other kinds of _autogenerated_ names that are used in IDA:

  1. Stack variables (`var_`) and arguments (`arg_`). 
  2. String literal names generated from their text (e.g. `aException` for “exception”)



The stack prefixes are hardcoded and not configurable but the latter can be configured in Options > General…, Strings tab.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/dummy_strlit-Jun-18-2024-09-03-33-6765-AM.png?width=584&height=488&name=dummy_strlit-Jun-18-2024-09-03-33-6765-AM.png)

Strings options

Unlike the dummy names, these names are stored in the database marked as autogenerated so their prefixes are not considered reserved and you can use them in custom names.

---

## 5. 

**Date:** Posted: Apr 2, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr](https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr)

Error fetching post

---

## 6. 

**Date:** Posted: Apr 1, 2021  
**Link:** [https://hex-rays.com/blog/ida-7-6-qt-5-6-3-configure-options-patch](https://hex-rays.com/blog/ida-7-6-qt-5-6-3-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.6: Qt 5.6.3 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-6-qt-5-6-3-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-6-qt-5-6-3-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-6-qt-5-6-3-configure-options-patch>)

Arnaud Diederen ✦ Posted: Apr 1, 2021

![IDA 7.6: Qt 5.6.3 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.3 build, that is shipped with IDA 7.6.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.3\configure.bat "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.3-x64"`
    * Note that you will have to build with Visual Studio 2015 (or later) to obtain compatible libs
  * Linux: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-64" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.3-x64"`
  * Mac OSX: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-clang" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.3-x64"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](<https://www.hex-rays.com/wp-content/uploads/2021/04/qt-5.6.3_full-IDA76.patch_.zip>). You should therefore apply it to your own Qt 5.6.3 sources before compiling, in order to obtain similar binaries (`patch -p 1 < path/to/qt-5_6_3_full-IDA76.patch`)

Note that this patch should work without any modification, against the 5.6.3 release as found [there](<https://download.qt.io/archive/qt>). You may have to fiddle with it, if your Qt 5.6.3 sources come from somewhere else.

Downloads

[qt-5.6.3_full-IDA76.patch_.zip](</wp-content/uploads/2021/04/qt-5.6.3_full-IDA76.patch_.zip>)

---

## 7. 

**Date:** Posted: Mar 29, 2021  
**Link:** [https://hex-rays.com/blog/2021-ida-training-course-registration-is-now-open](https://hex-rays.com/blog/2021-ida-training-course-registration-is-now-open)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Programming](<https://hex-rays.com/blog/tag/programming>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [idatraining](<https://hex-rays.com/blog/tag/idatraining>)

# 2021 IDA Training Course: Registration is now open!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/2021-ida-training-course-registration-is-now-open>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/2021-ida-training-course-registration-is-now-open>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/2021-ida-training-course-registration-is-now-open>)

Tra Tran ✦ Posted: Mar 29, 2021

![2021 IDA Training Course: Registration is now open!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

The [2021 IDA training course](</products/ida/training/>) will take place online from 10–14 and 17-19 May 2021, CEST time.

Due to the ongoing COVID-19 situation, the world-class IDA Training course is taking place online for the second time from 10-14 and 17-19 May 2021 (CEST time). The course is devised to help professional reverse engineers master IDA Pro, the top notch software disassembly tool and take their reversing skills to the next level. Provided by the experts behind IDA, the training offers its participants unique opportunity to dissect the famous disassembler, understand its internals and upgarde their analysis with IDA’s limitless capabilities.

The first five days (10-14 May) will be devoted to the **[standard IDA training](</products/ida/training/#standard>)**. The following three days (17-19 May) to a course on **[advanced training (Programming for IDA)](</products/ida/training/#programming>)**.

Training will be both theoretical and practical. After each theoretical section hands-on exercises will be carried out so as to master thorough understanding of concepts and methods. Training material is always updated to include the latest additions to IDA.

Detailed information including [course programs, cost and registration forms](</products/ida/training/>) can be found in our dedicated training page. If needed, additional information may be requested by emailing our [sales](<mailto:sales@hex-rays.com>) team.

[button type=”info” size=”lg” link=”/products/ida/training/”]Learn more about the course __[/button]

---

## 8. 

**Date:** Posted: Mar 26, 2021  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-32-running-scripts](https://hex-rays.com/blog/igors-tip-of-the-week-32-running-scripts)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [IDC](<https://hex-rays.com/blog/tag/idc>)

# Igor’s tip of the week #32: Running scripts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-32-running-scripts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-32-running-scripts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-32-running-scripts>)

I 

Igor Skochinsky ✦ Posted: Mar 26, 2021

![Igor’s tip of the week #32: Running scripts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Scripting allows you to automate tasks in IDA which can be repetitive or take a long time to do manually. We [previously covered](<https://www.hex-rays.com/blog/igor-tip-of-the-week-08-batch-mode-under-the-hood/>) how to run them in batch (headless) mode, but how can they be used interactively?

### Script snippets

File > Script Command… (`Shift`+`F2`)

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/script_snippet-3.png?width=681&height=493&name=script_snippet-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/script_snippet-3.png>)

Although this dialog is mainly intended for quick prototyping and database-specific snippets, you can save and load scripts from external files via the “Export” and “Import” buttons. There is some basic syntax highlighting but it’s not a replacement for a full-blown IDE. Another useful feature is that the currently selected snippet can be executed using the `Ctrl`+`Shift`+`X` shortcut (“SnippetsRunCurrent” action) even when the focus is in another widget.

### Command Line Interface (CLI)

The input line at the bottom of IDA’s screen can be used for executing small one-line expressions in IDC or Python (the interpreter can be switched by clicking on the button).

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/script_cli-3.png?width=714&height=333&name=script_cli-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/script_cli-3.png>)

While somewhat awkward to use for bigger tasks, it has a couple of unique features:

  * the result of entered expression is printed in the Output Window (unless inhibited with a semicolon). In case of IDC, values are printed in multiple numeric bases and objects are pretty-printed recursively.
  * It supports limited [Tab completion](<https://www.hex-rays.com/blog/implementing-command-completion-for-idapython/>).



### Running script files

If you already have a stand-alone script file and simply want to run it, File > Script file.. (`Alt`+`F7`) is probably the best and quickest solution. It supports both IDC and Python scripts.

### Recent scripts

The scripts which were executed through the “Script file…” command are remembered by IDA and can be executed again via the Recent Scripts list (View > Recent scripts, or `Alt`+`F9`). You can also invoke an external editor (configured in Options > General…, Misc tab) to edit the script before running.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/script_recent-Jun-18-2024-08-56-03-6582-AM.png?width=673&height=303&name=script_recent-Jun-18-2024-08-56-03-6582-AM.png)

### Examples

IDA ships with some example scripts which can be found in “idc” directory for IDC and “python/examples” for IDAPython. There are also some user-contributed scripts in the [download area](</products/ida/support/download/>).

---

## 9. 

**Date:** Posted: Mar 22, 2021  
**Link:** [https://hex-rays.com/blog/ida-7-6-released](https://hex-rays.com/blog/ida-7-6-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 7.6 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-6-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-6-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-6-released>)

Tra Tran ✦ Posted: Mar 22, 2021

![IDA 7.6 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Today, Hex-Rays team is thrilled to announce the release of IDA version 7.6!

Our top-notch binary analysis tool IDA Pro’s latest version delivers new features and various enhancements. With significantly-improved performance, version 7.6 is expected to certainly accelerate its users’ reverse-engineering experience.

**Here are the highlight features and changes introduced in IDA 7.6:**

  * Apple Silicon’s full support: The much-anticipated feature is finally arrived!
  * Golang analysis: improved to properly support its peculiarities
  * Thorough improvements for Hex-Rays Decompiler on renaming variables, recognizing stack arrays or making it more readable
  * Two new processor modules added to the selection: RISC-V and RL78
  * Further supports for compressed macOS and iOS kernelcache and for Python 3.9



**And many more!**

### How to request the new versions

As usual, the new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to support@hex-rays.com.

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist __](</products/ida/news/7_6/>)[Get a quote __](</cgi-bin/quote.cgi>)

  
  


**You might also be interested in:**   
[IDA 7.6 Service Pack 1 released](</blog/ida-7-6-service-pack-1-released/>)

---

