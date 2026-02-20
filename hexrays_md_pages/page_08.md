# Hex-Rays Blog – Page 8

**Archived:** 2026-02-20 12:11
**URL:** https://hex-rays.com/blog/page/8

---

## 1. 

**Date:** Posted: Feb 7, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-174-ida-database-idbdetails](https://hex-rays.com/blog/igors-tip-of-the-week-174-ida-database-idbdetails)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #174: IDA database (IDB) details

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-174-ida-database-idbdetails>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-174-ida-database-idbdetails>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-174-ida-database-idbdetails>)

I 

Igor Skochinsky ✦ Posted: Feb 7, 2024

![Igor’s Tip of the Week #174: IDA database \(IDB\) details](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When you work in IDA, it saves the results of your analysis in the _IDA Database_ , so that you can pause and continue at a later time. You can recognize the database files by their file extension `.idb` (for legacy, 32-bit IDA) or `.i64` (for 64-bit IDA or IDA64). Thus they’re also often called just **IDB**. But what do they contain?

You can get a hint by looking at the working directory when the IDB is open in IDA:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idb1-3.png?width=610&height=154&name=idb1-3.png)

So, IDB is a container which contains several sub-files:

  1. `filename.id0` is the actual database (implemented using B-tree), which contains all the metadata extracted from the input file and/or added by the user (names, comments, function boundaries and much more);
  2. `filename.id1` stores the _virtual array_ , containing a copy of all data loaded from the input file plus internal flags needed by IDA. Due to that it is usually 4-5 times as big as the original file but may grow or shrink if you [add](<https://hex-rays.com/blog/igors-tip-of-the-week-96-loading-additional-files/>) or remove data from the database;
  3. `filename.id2`(if present) stores the data for _sparse_ memory areas (e.g. mostly zero-filled segments) used in some situations;
  4. `filename.nam` is a special cache for names used in the database;
  5. `filename.til` is the type library containing [Local Types](<https://hex-rays.com//products/ida/support/idadoc/1259.shtml>) for the database.



When you close the database, IDA gives you a choice what to do with these files:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idb2-3.png?width=257&height=286&name=idb2-3.png)

  * _Don’t pack database_ leaves the individual sub-files on disk as-is. It is the fastest option but also can be dangerous because there are no integrity checks so any corruption may go undetected until much later;
  * _Pack database (Store)_ simply combines the sub-files into an `.idb` or `.i64` container, adding checksums so that file corruption can be detected. Because no compression is used, the IDB size is roughly equal to the total size of the sub-files;
  * _Pack database (Deflate)_ compresses sub-files using zlib compression which can significantly decrease the disk space compared to the Store option at the cost of more time spent saving and unpacking the IDB.



See also:

[IDA Help: Exit IDA](<https://hex-rays.com//products/ida/support/idadoc/450.shtml>)

[Igor’s tip of the week #58: Keyboard modifiers](<https://hex-rays.com/blog/igors-tip-of-the-week-58-keyboard-modifiers/>)

---

## 2. 

**Date:** Posted: Feb 2, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-173-navigating-to-types-from-pseudocode](https://hex-rays.com/blog/igors-tip-of-the-week-173-navigating-to-types-from-pseudocode)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #173: Navigating to types from pseudocode

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-173-navigating-to-types-from-pseudocode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-173-navigating-to-types-from-pseudocode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-173-navigating-to-types-from-pseudocode>)

I 

Igor Skochinsky ✦ Posted: Feb 2, 2024

![Igor’s Tip of the Week #173: Navigating to types from pseudocode](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

[Previously](<https://hex-rays.com/blog/igors-tip-of-the-week-172-type-editing-from-pseudocode/>) we’ve seen how to do small edits to types directly from the pseudocode view. While this is enough for minor edits, sometimes you still need to use the full editor.

Of course, it is always possible to open Structures, Enums, or Local Types and look for your type there, but what if you have thousands of them? Fortunately, there are quicker options:

  * If the current variable has a type defined in Local Types, you can use “Jump to local type…” from the context menu  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/typenav1-3.png?width=703&height=345&name=typenav1-3.png)
  * If the cursor is on a structure field, you can use “Jump to structure definition..” (or `Z`) to open the structure and jump directly to the field in question.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/typenav2-e1706804039550-3.png?width=599&height=426&name=typenav2-e1706804039550-3.png)



Once you’re in either view, you can perform the necessary changes to the type before switching back to the decompiler.

NB: to avoid slowdowns, the decompiler does not track changes performed outside of the pseudocode view, so you may need to press `F5` to refresh it with new changes.

See also:

[Igor’s Tip of the Week #172: Type editing from pseudocode ](<https://hex-rays.com/blog/igors-tip-of-the-week-172-type-editing-from-pseudocode/>)

[Igor’s tip of the week #11: Quickly creating structures](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>)

---

## 3. 

**Date:** Posted: Jan 23, 2024  
**Link:** [https://hex-rays.com/blog/plugin-focus-q3vm](https://hex-rays.com/blog/plugin-focus-q3vm)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: q3vm

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-q3vm>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-q3vm>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-q3vm>)

A 

Alex Petrov ✦ Posted: Jan 23, 2024

![Plugin focus: q3vm](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

_This is a guest entry written by David Catalán from Outpost24. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author._

Software reverse engineering involves working with a wide variety of processor architectures, both real and virtual. Thus, having the capability to extend existing tooling to face new challenges is crucial.

Due to its feature-rich kernel, convenient interface, and wide number of existing plugins, IDA Pro provides a perfect environment to develop new tooling. This plugin is yet another example of how IDA Pro can be extended to support different types of binaries.

The Q3VM plugin includes the loader and processor modules to help analyze binaries built with the [Quake III virtual machine obfuscator](<https://github.com/jnz/q3vm>). Despite being initially developed for the Quake III Arena videogame, this project is independent of the game and has been used to obfuscate code fragments within the Rhadamanthys malware.

# Dealing with virtual machine obfuscators

To prevent code from being disassembled by the state-of-the-art tools used by reverse engineers, virtual machine obfuscators implement code interpreters that translate a new custom set of instructions to native assembly. As a result, a disassembler is only able to process the interpreter's code, leaving the bytes belonging to the protected code intact.

![](https://hex-rays.com/hubfs/Imported_Blog_Media/obfuscated-Jun-18-2024-09-10-02-8314-AM.png) How Q3VM obfuscated code looks on a hexadecimal editor 

The solution to the problem VM obfuscators present is neither trivial nor new for reverse engineers. Every time we need to work with a new architecture, we study it and adapt our existing tooling to support it.

IDA's SDK is a very convenient platform for these tasks as it allows the user to focus strictly on understanding the target architecture, forgetting about things like programming a disassembly algorithm, creating a proper user interface to work on, etc.

# The Q3VM loader and processor modules

Installing them is fairly easy: build both modules following the instructions provided by the SDK and place them on the corresponding directories (%IDADIR%/loaders and %IDADIR%/procs).

QVM binaries can be standalone or embedded into other binaries. To spot and extract QVM files, we must start looking for the QVM's header magic constant, defined in [vm.h](<https://github.com/jnz/q3vm/blob/b25f51a2cb15373a6be54a913372500c2578cf57/src/vm/vm.h#L32>). Once the start of the file has been located, it is possible to calculate the total size of the file using the fields of the header, which follows [this structure](<https://github.com/jnz/q3vm/blob/b25f51a2cb15373a6be54a913372500c2578cf57/src/vm/vm.h#L95>).

If the headers have been manipulated, some extra creativity is needed. Luckily, the QVM bytecode contains plenty of repetitive patterns like a CONST instruction followed by a STORE4, which corresponds to a value being assigned to a local variable. This kind of repetitive pattern can be very useful to locate the code section of a QVM binary.

To disassemble a previously obtained QVM binary, simply open it with IDA Pro. The loading screen should automatically detect which processor to use.

![Loading a new QVM file.](https://hex-rays.com/hubfs/Imported_Blog_Media/loader-3.png) Loading a new QVM file. 

The processor module supports stack variables renaming and cross-referencing calls.

![QVM binary disassembled](https://hex-rays.com/hubfs/Imported_Blog_Media/processor-3.png) QVM binary disassembled. 

![Cross-references from CALL instruction](https://hex-rays.com/hubfs/Imported_Blog_Media/x-ref-3.png) Cross-references from CALL instruction. 

# Closing

As previously mentioned, the Q3VM plugins enable reverse engineers to analyze QVM binaries easily within IDA Pro, benefiting from most of its features.

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/plugin-repo-Jun-18-2024-09-10-03-5475-AM.png?width=530&height=324&name=plugin-repo-Jun-18-2024-09-10-03-5475-AM.png)](<https://plugins.hex-rays.com/q3vm>)

---

## 4. 

**Date:** Posted: Jan 16, 2024  
**Link:** [https://hex-rays.com/blog/participate-in-our-ida-plugin-community-survey](https://hex-rays.com/blog/participate-in-our-ida-plugin-community-survey)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [plugin repository](<https://hex-rays.com/blog/tag/plugin-repository>) [Plugins](<https://hex-rays.com/blog/tag/plugins>) [Survey](<https://hex-rays.com/blog/tag/survey>)

# Participate in our IDA Plugin Community Survey

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/participate-in-our-ida-plugin-community-survey>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/participate-in-our-ida-plugin-community-survey>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/participate-in-our-ida-plugin-community-survey>)

A 

Alex Petrov ✦ Posted: Jan 16, 2024

![Participate in our IDA Plugin Community Survey](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/plugin-survey-3.png)

One reason for our success is the strong community that has emerged around our products. We are always excited and surprised to see what plugins and tools you’ve been building on top of IDA all those years. This year, we want to engage with all of you better. It starts with asking you a few questions we’ve compiled in this short survey.

Do not hesitate to take part. Whether you are a seasoned plugin developer, a user, or just considering dipping your toes into the world of IDA plugins – we want to hear from you. This survey is your opportunity to share how you use plugins, what drives you to create them, and the challenges you face along the way. We are highly interested to learn more about your go-to resources for support and guidance. Your contribution will play a tremendous role in shaping the future development of the IDA Plugin environment and support, ensuring we align our efforts with your needs and expectations. 

**It is all about your experience!**

Your feedback is more than just responses to us. It is the starting point for providing a more tailored and effective user experience. By completing the questionnaire, you’re contributing to a community of like-minded colleagues and influencing the tools and resources we develop to support your creative and technical endeavors. We expect you to be honest and tell us – what works, what doesn’t, and what you wish to see improved.

The time is now. This is your chance to have your say and help us build a more supportive, innovative, and user-friendly environment. This survey will only take a few minutes of your time and is anonymous. You can be assured that we highly appreciate your time and efforts, and therefore, **five of you** who answered all questions and provided detailed feedback will have the chance to win an **IDA Mug**! The survey will conclude on 31 January 2024 and on 6 February 2024 we will pick and contact the winners.

Thank you for your continued support and for helping us craft a better IDA plugin community! 

[Take the Survey](<https://forms.gle/gyoBMRW1osEQz4PX9>)

---

## 5. 

**Date:** Posted: Jan 15, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-172-type-editing-from-pseudocode](https://hex-rays.com/blog/igors-tip-of-the-week-172-type-editing-from-pseudocode)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [UI](<https://hex-rays.com/blog/tag/ui>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #172: Type editing from pseudocode

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-172-type-editing-from-pseudocode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-172-type-editing-from-pseudocode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-172-type-editing-from-pseudocode>)

I 

Igor Skochinsky ✦ Posted: Jan 15, 2024

![Igor’s Tip of the Week #172: Type editing from pseudocode](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

We already know that user-defined types such as [structures](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>) and [enums](<https://hex-rays.com/blog/igors-tip-of-the-week-99-enums/>) can be created and edited through the corresponding views, or the [Local Types](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>) list.

However, some small edits can be performed directly in the pseudocode view:

  1. structure fields can be renamed using the “Rename” action (shortcut `N`):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hredit1-3.png?width=381&height=174&name=hredit1-3.png)
  2. you can also quickly retype them using the “Set type” action (`Y`):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hredit2-3.png?width=402&height=181&name=hredit2-3.png)  
NB: the neighboring fields are automatically overwritten if the size of the field increases so use Undo to revert if necessary.



These actions also work on stack variables because they behave like fields of the stack frame structure.

If you prefer to use the free-form C syntax editor to see and edit the whole structure, you can quickly navigate to the type using the “Jump to local type” action from the context menu and then use “Edit” (`Ctrl`–`E`) on the type’s entry:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hredit3-3.png?width=787&height=358&name=hredit3-3.png)

See also:

[Igor’s tip of the week #99: Enums](<https://hex-rays.com/blog/igors-tip-of-the-week-99-enums/>)

[Igor’s tip of the week #42: Renaming and retyping in the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)

[Igor’s tip of the week #11: Quickly creating structures](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>)

---

## 6. 

**Date:** Posted: Jan 5, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-171-enums-as-structure-members](https://hex-rays.com/blog/igors-tip-of-the-week-171-enums-as-structure-members)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #171: Enums as structure members

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-171-enums-as-structure-members>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-171-enums-as-structure-members>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-171-enums-as-structure-members>)

I 

Igor Skochinsky ✦ Posted: Jan 5, 2024

![Igor’s Tip of the Week #171: Enums as structure members](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

We’ve seen how custom structures can be used to [format data tables](<https://hex-rays.com/blog/igors-tip-of-the-week-170-instantiating-structures/>) nicely, but sometimes you can improve your understanding even further with small adjustments. For example, in the structure we created, the first member (`nMessage`) is printed as a simple integer:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strenum1-3.png?width=698&height=375&name=strenum1-3.png)

If you know Win32 API well, you may recognize that these numbers correspond to [window messages](<https://learn.microsoft.com/en-us/windows/win32/winmsg/window-messages>), but it would be nice to see the symbolic names instead of numbers without having to check MSDN or Windows headers every time. In fact, IDA already has this mapping in the standard [type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>), so we just need to use it for our structure member. It can be done pretty easily using the following steps:

  1. Open the Enums window (`Shift`+`F10`).
  2. Press `Ins` or use “Add Enum…” from the context menu.
  3. Either click _Add standard enum by symbol name_ and pick one of the known messages (e.g. `WM_COMMAND`), or just type `MACRO_WM` in the Name field directly, then click OK.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strenum2-229x300-3.png?width=229&height=300&name=strenum2-229x300-3.png)
  4. now go to `AFX_MSGMAP_ENTRY` in the Structures window 
  5. on the first field, use Field type > Enum member… from the context menu, or the shortcut `M`.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strenum3-3.png?width=519&height=149&name=strenum3-3.png)
  6. select `MACRO_WM`from the list. An automatic comment is added for the field:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strenum4-3.png?width=672&height=91&name=strenum4-3.png)  
  

  7. back in the listing, the numbers are replaced by the symbolic constants:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strenum5-3.png?width=747&height=372&name=strenum5-3.png)



See also:

[Igor’s tip of the week #99: Enums](<https://hex-rays.com/blog/igors-tip-of-the-week-99-enums/>)

[Igor’s Tip of the Week #125: Structure field representation](<https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation/>)

[Igor’s tip of the week #60: Type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>)

---

## 7. 

**Date:** Posted: Dec 29, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-170-instantiating-structures](https://hex-rays.com/blog/igors-tip-of-the-week-170-instantiating-structures)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #170: Instantiating structures

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-170-instantiating-structures>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-170-instantiating-structures>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-170-instantiating-structures>)

I 

Igor Skochinsky ✦ Posted: Dec 29, 2023

![Igor’s Tip of the Week #170: Instantiating structures](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

[Creating user-defined structures](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>) can be quite useful both in disassembly and [pseudocode](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>) when dealing with code using custom types. However, they can be useful not only in code but also data areas.

### MFC message maps

As an example, let’s consider an MFC program which uses [message maps](<https://learn.microsoft.com/en-us/cpp/mfc/tn006-message-maps?view=msvc-140>). These maps are present in the constant data area of the program and are initially represented by IDA as a mix of numbers and offsets:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst1-3.png?width=1151&height=586&name=structinst1-3.png)

To make sense of it, we can consult the `AFX_MSGMAP_ENTRY` structure defined in `afxwin.h`:
    
    
    struct AFX_MSGMAP_ENTRY
    {
    	UINT nMessage; // windows message
    	UINT nCode; // control code or WM_NOTIFY code
    	UINT nID; // control ID (or 0 for windows messages)
    	UINT nLastID; // used for entries specifying a range of control id's
    	UINT_PTR nSig; // signature type (action) or pointer to message #
    	AFX_PMSG pfn; // routine to call (or special value)
    };

To quickly add the structure to the database, we can use the [Local Types](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>) window after replacing the MFC-specific `AFX_PMGS` type with a void pointer:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst2-3.png?width=1067&height=389&name=structinst2-3.png)

### Applying structure to data

Once the structure has been sycnchronized to IDB, it can be used in the disassembly listing. In cases where the candidate area is undefined and the list of available structures is small, you can use the context menu:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst3-3.png?width=631&height=323&name=structinst3-3.png)

If there are too many candidates, or the data is already defined (e.g. converted to an array by autoanalysis), you can directly use the Edit > Struct var… menu item, or the shortcut `Alt`–`Q`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst5-3.png?width=483&height=454&name=structinst5-3.png)

In either case, IDA will use the structure layout to show the data as corresponding fields:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst4-e1703868566457-3.png?width=1130&height=84&name=structinst4-e1703868566457-3.png)

Note that the [dummy name](<https://hex-rays.com/blog/igors-tip-of-the-week-34-dummy-names/>) of the location changes to reflect the fact that it’s a structure instance.

Once a structure instance is defined, you can:

  1. create an [array of structures](<https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays/>) (e.g. using the `*` shortcut):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst6-3.png?width=1131&height=268&name=structinst6-3.png)
  2. switch between the [terse and full](<https://hex-rays.com/blog/igors-tip-of-the-week-31-hiding-and-collapsing/>) structure representation:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst7-3.png?width=863&height=187&name=structinst7-3.png)



### Applying structures by retyping

In addition to the “Struct var…” action or the context menu, you can also quickly apply structure to data by specifying its name in the “Set type…” command (`Y` shortcut). 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/structinst8-3.png?width=888&height=165&name=structinst8-3.png)

This approach also works for structures which have not yet been imported to IDB or are present only in the loaded [type libraries](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>).

See also:

[IDA Help: Declare a structure variable](<https://hex-rays.com//products/ida/support/idadoc/496.shtml>)

[Igor’s tip of the week #11: Quickly creating structures](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>)

[Igor’s tip of the week #12: Creating structures with known size](<https://hex-rays.com/blog/igor-tip-of-the-week-12-creating-structures-with-known-size/>)

[Igor’s tip of the week #94: Variable-sized structures](<https://hex-rays.com/blog/igors-tip-of-the-week-94-variable-sized-structures/>)

---

## 8. 

**Date:** Posted: Dec 22, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-169-jumping-to-a-file-offset](https://hex-rays.com/blog/igors-tip-of-the-week-169-jumping-to-a-file-offset)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #169: Jumping to a file offset

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-169-jumping-to-a-file-offset>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-169-jumping-to-a-file-offset>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-169-jumping-to-a-file-offset>)

I 

Igor Skochinsky ✦ Posted: Dec 22, 2023

![Igor’s Tip of the Week #169: Jumping to a file offset](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Even though most manipulations with binaries can be done directly in IDA, you may occasionally need to use other tools. For example, [Binwalk](<https://github.com/ReFirmLabs/binwalk>) for basic firmware analysis, or a hex editor/viewer to find interesting patterns in the file manually.

Let’s say you found an interesting text or byte pattern at some offset in the file and want to look at it in IDA. In case of raw binary (e.g. a firmware) loaded at 0, the solution is simple: you can use [“Jump to address”](<https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places/>) action since addresses are equivalent to file offsets. But in case of a structured file like PE, ELF, or Mach-O, this can get [quite complicated](<https://stackoverflow.com/questions/4524837/how-can-we-map-rva-relative-virtual-address-of-a-location-to-pe-file-offset>).

Luckily, IDA keeps a mapping of file offsets to addresses when it loads the file, so in such cases, you can use Jump > Jump to file offset… action.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fileoffset1-215x300-3.png?width=215&height=300&name=fileoffset1-215x300-3.png)

You can confirm that you ended up at the correct place by checking the first field of IDA View’s [status bar](<https://hex-rays.com/blog/igors-tip-of-the-week-61-status-bars/>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fileoffset2-300x246-3.png?width=300&height=246&name=fileoffset2-300x246-3.png)

NB: in some cases the action might fail because IDA does not always load all parts of the file. For example, the PE header may not be loaded by default. Also, extra data which is not present in memory at runtime (such as file’s overlay/trailing data, debug info, or other metadata) is usually not loaded into the database. However, in some cases you can load it using [manual load](<https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load/>) option.

The action may also fail if there is no 1-to-1 mapping between the file and loaded data (e.g. data on disk was compressed).

See also:

[Igor’s tip of the week #20: Going places](<https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places/>)

[Igor’s tip of the week #61: Status bars](<https://hex-rays.com/blog/igors-tip-of-the-week-61-status-bars/>)

---

## 9. 

**Date:** Posted: Dec 15, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-168-rebasing](https://hex-rays.com/blog/igors-tip-of-the-week-168-rebasing)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #168: Rebasing

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-168-rebasing>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-168-rebasing>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-168-rebasing>)

I 

Igor Skochinsky ✦ Posted: Dec 15, 2023

![Igor’s Tip of the Week #168: Rebasing](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

When you load a file into IDA, whether a standard executable format (e.g. PE, ELF, Macho-O), or a [raw binary](<https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader/>), IDA assigns a particular address range to the data loaded from it, either from the file’s metadata or user’s input (in case of binary file). The lowest address from those occupied by the file is commonly called _imagebase_ and you can usually see it in the file comment at the start of the disassembly listing:

![Format      : ELF64 for ARM64 \(Shared object\)
Imagebase   : 2000000](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rebase1-3.png?width=906&height=81&name=rebase1-3.png)

There may be situations where you may need to move the loaded data to another address. The most common case is probably debugging on a live system: due to ASLR (Address space layout randomization), or simple memory usage patterns, the addresses occupied by the executable/library at runtime may not match the defaults used by IDA. If you use IDA’s own debugger, it should adjust addresses automatically, but in other situations you can do it manually via the _rebasing_ action.

### Rebasing

To move the currently loaded file to another address, you can use Edit > Segments > Rebase program…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rebase2-3.png?width=743&height=146&name=rebase2-3.png)

You can then specify the new address or a shift value (positive or negative):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rebase3-3.png?width=317&height=372&name=rebase3-3.png)

The “Fix up relocations” option will adjust values which depend on the load address (if the input file has relocation info and it was parsed by IDA), simulating the process performed by the OS loader/dynamic linker.

“Rebase the whole image” uses an algorithm optimized for moving the whole file at once (otherwise each segment is moved separately which may fail if there is an overlap between old and new addresses).

See also:

[Igor’s tip of the week #41: Binary file loader](<https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader/>)

[Igor’s Tip of the Week #122: Manual load](<https://hex-rays.com/blog/igors-tip-of-the-week-122-manual-load/>)

---

