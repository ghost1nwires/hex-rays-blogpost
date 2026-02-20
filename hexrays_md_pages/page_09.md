# Hex-Rays Blog – Page 9

**Archived:** 2026-02-20 12:12
**URL:** https://hex-rays.com/blog/page/9

---

## 1. 

**Date:** Posted: Dec 13, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-msdocviewer](https://hex-rays.com/blog/plugin-focus-msdocviewer)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: msdocviewer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-msdocviewer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-msdocviewer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-msdocviewer>)

A 

Alex Petrov ✦ Posted: Dec 13, 2023

![Plugin focus: msdocviewer](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

_This is a guest entry written by Alexander Hanel from CrowdStrike. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author._

## Msdocviewer: A simple tool for viewing Microsoft’s technical specifications

An invaluable resource when reverse engineering Portable Executable (PE) binaries is Microsoft’s Windows Application Programming Interfaces (API) technical documentation. Microsoft’s documentation (commonly referred to as MSDN) describes how an API can interact with the Windows operating system. By piecing together how individual APIs interact with Windows, an analyst can infer functionality or a subset of the functionality within a binary. Having the documentation within IDA helps speed up the reverse engineering process because it can be easily accessed without switching to a browser or other third-party document viewer. The following image is an example of the msdocviewer IDA plugin. The highlighted API `GetProcessHeap` can be seen on the left in graph view with the corresponding API documentation on the right.

![example](https://hex-rays.com/hubfs/Imported_Blog_Media/msdocviewer-img-1-3.png)

## Description

The msdocviewer is a simple tool that parses Microsoft’s [Win32 API](<https://github.com/MicrosoftDocs/sdk-api/>) and [driver](<https://github.com/MicrosoftDocs/windows-driver-docs-ddi>) documentation, so they are viewable in IDA. The tools consist of three parts: the first is two git repositories, the second is the parser, and the third is an IDA plugin. 

The first part uses two repositories from [Microsoft Doc](<https://github.com/MicrosoftDocs>)’s. In an effort to allow public contribution to their API documentation, Microsoft posts their technical specifications on GitHub in the Markdown format under Microsoft Docs. The first repository is the Win32 API documentation (with a directory name of `sdk-api`), and the second is the Windows Driver DDI reference documentation (with a directory name of `windows-driver-docs-ddi`). 

The second part is a Python script that parses the documentation repositories, finds all documents related to function APIs, once found it copies the document to a directory, rearranges some of the text, and then renames the document to its corresponding API name. For example, the file name `nf-fileapi-createfilea.md` is renamed to `CreateFileA.md`. 

The third part is an IDA plugin written in IDAPython that leverages PyQt’s Markdown viewer to display the API’s documentation. 

## Installing Msdocviewer

The first step in installing msdocviewer is to clone the repository from Github using the below command.
    
    
    git clone https://github.com/alexander-hanel/msdocsviewer.git
    

Once the repository has been cloned, the repository hosting the documentation needs to be downloaded. The repositories are stored as submodules and, therefore, can be downloaded by executing the following commands. 
    
    
    cd msdocviewer
    git submodule update --init --recursive
    

The submodules repositories are over 2GB in size and, therefore, can take a while to download. If the git submodule command throws an exception, re-executing the second command should take care of the exception. Once downloaded, the documentation repositories need to be parsed by executing python `run_me_first.p`y. Note: This Python script requires `pyyaml`, which can be installed by executing `pip install -r requirements.txt`. The following is an example output of `run_me_first.py`:
    
    
    C:\Users\Admin\Documents\repo\msdocsviewer>python run_me_first.py
    INFO - creating apis_md directory at C:\Users\Admin\Documents\repo\msdocsviewer\apis_md
    INFO - starting the parsing, this can take a few minutes
    INFO - parsing C:\Users\Admin\Documents\repo\msdocsviewer\sdk-api\sdk-api-src\content
    INFO - parsing C:\Users\Admin\Documents\repo\msdocsviewer\sdk-api\sdk-api-src\content completed
    INFO - parsing C:\Users\Admin\Documents\repo\msdocsviewer\windows-driver-docs-ddi\wdk-ddi-src\content
    INFO - parsing C:\Users\Admin\Documents\repo\msdocsviewer\windows-driver-docs-ddi\wdk-ddi-src\content completed
    INFO - finished parsing, if using IDA add path C:\Users\Admin\Documents\repo\msdocsviewer\apis_md to API_MD variable in idaplugin/msdocviewida.py
    

During the parsing process, some function names are invalid files and, therefore, not created. To see what files and functions are skipped, an optional command line argument of `–log` or `-l` can be added. It stores the log to a file named `debug-parser.log`. There is also a command line option of `–overwrite` or `-o` to overwrite the current documentation. Overwriting is recommended if an old version of msdocviewer is present or if Microsoft updates their docs repos.

The plugin can be activated either through the Edit, Plugins, msdocviewer, or using the hotkey `ctrl-shift-z`. 

## Writing Your Own Documentation Tool

Overall, the code used to make msdocviewer is simple. Without the logging functionality, the parser and the IDA plugin each contain less than 100 lines of Python code. While msdocviewer is useful for reverse engineering Windows binaries, it has little value for Linux or other platforms. What makes msdocviewer useful is the documentation. So, to write a tool of value for your platform, all that is needed is documentation. Once the documentation is found and converted to a supported format (PyQt supports formats of Markdown, HTML, or Plaintext), it is easy to use IDAPython to make your own custom documentation tool. If you don’t want to start from scratch, feel free to use the msdocviewer IDA plugin as a skeleton. The functions `get_selected_api_name` and `load_markdown` contain the core logic of the plugin. The first function extracts the selected string using [ida_kernwin.get_highlight](<https://www.hex-rays.com/products/ida/support/idapython_docs/ida_kernwin.html#ida_kernwin.get_highlight>) and the second uses the selected string to determine what file name should be opened and displayed. 

Since msdocviewer is looking up file names on disk, it is easy to add your own documentation. For example, I always forget the enum values for `SYSTEM_INFORMATION_CLASS`. A fix for this is to create a file named `SYSTEM_INFORMATION_CLASS.md` within the apis_md directory with all the relevant documentation within the Markdown. Now, I only need to highlight the text `SYSTEM_INFORMATION_CLASS` and open the plugin (I prefer the hotkey; it is super convenient). 

## Closing

As previously mentioned, msdocviewer is a simple plugin because of how easy IDAPython makes selected data available and how straightforward it is to display Microsoft's documentation using PyQt. If your focus isn’t on reverse engineering Window binaries, I hope this blog post gives you motivation to make your own documentation viewer for other platforms. 

The msdocviewer plugin is available on GitHub <https://github.com/alexander-hanel/msdocsviewer>

---

## 2. 

**Date:** Posted: Dec 2, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-167-adding-and-splitting-segments](https://hex-rays.com/blog/igors-tip-of-the-week-167-adding-and-splitting-segments)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #167: Adding and splitting segments

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-167-adding-and-splitting-segments>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-167-adding-and-splitting-segments>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-167-adding-and-splitting-segments>)

I 

Igor Skochinsky ✦ Posted: Dec 2, 2023

![Igor’s Tip of the Week #167: Adding and splitting segments](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

When analyzing firmware binaries, a proper memory layout is quite important. When loading a [raw binary](<https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader/>), IDA usually creates a code segment for the whole binary. This is good enough when that code is all you need to analyze, but it is not always the case. For example, the code can refer to external hardware as MMIO (memory-mapped I/O), or use extra memory which is not part of the binary image. How to handle such situations?

### Creating segments

To make extra addresses present in the database, use Edit > Segments > Create segment… action.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/segments1-3.png?width=658&height=503&name=segments1-3.png)

Enter the segment name, start/end addresses and optional class. The class is usually just informative but may affect [decompiler’s behavior](<https://www.hex-rays.com/products/decompiler/manual/tricks.shtml>). The “Use sparse storage option” is useful for segments which are mostly empty and have relatively few data items (e.g. BSS or MMIO). When enabled, IDA will use storage optimized for such use case so the IDB won’t grow much even if the new segment is very large.

NB: the end address of the segment is **exclusive** , i.e. last byte of the segment will have address `end_ea-1`.

Once the segment is created, it may be a good idea to[ reanalyze the database](<https://hex-rays.com/blog/igor-tip-of-the-week-09-reanalysis/>) so that reference to the newly available addresses are discovered.

### Splitting segments

If you specify an address range which partially intersects with an existing segment, IDA will automatically truncate it to make room for the new one. For example, assume you have a firmware loaded as ROM segment from 0 to 0x80000 but then discover that the code area seems to end at 0x60000. To to split off the last part as read-only data, create a new segment (e.g. named `.rodata`) with boundaries 0x60000 to 0x80000 and the ROM segment will be automatically truncated to end at 0x60000.

### Moving the segment boundary

Let’s say that after analyzing the binary further, you realize that `.rodata` should actually start at 0x70000. You can move the split point quickly using the following steps:

  1. navigate to the new split point (e.g. 0x70000);
  2. Invoke Edit > Segments > Edit segment (or use shortcut `Alt`–`S`);
  3. In the Start address (or End address, depending on the direction of the move), enter `here`.
  4. Make sure “Move adjacent segments” is enabled and click OK.



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/segments2-3.png?width=592&height=466&name=segments2-3.png)

Because most numerical input fields in IDA [accept IDC expressions](<https://hex-rays.com/blog/igors-tip-of-the-week-21-calculator-and-expression-evaluation-feature-in-ida/>), `here` will be converted to the current address(0x70000), so .rodata segment boundaries will be adjusted to 0x70000-0x80000, and the adjacent ROM segment extended to 0-0x70000.

See also:

[Igor’s tip of the week #41: Binary file loader](<https://hex-rays.com/blog/igors-tip-of-the-week-41-binary-file-loader/>)

[IDA Help: Create a new segment](<https://www.hex-rays.com/products/ida/support/idadoc/507.shtml>)

[IDA Help: Change segment attributes](<https://www.hex-rays.com/products/ida/support/idadoc/514.shtml>)

---

## 3. 

**Date:** Posted: Nov 24, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-166-dealing-with-too-big-function](https://hex-rays.com/blog/igors-tip-of-the-week-166-dealing-with-too-big-function)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #166: Dealing with “too big function”

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-166-dealing-with-too-big-function>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-166-dealing-with-too-big-function>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-166-dealing-with-too-big-function>)

I 

Igor Skochinsky ✦ Posted: Nov 24, 2023

![Igor’s Tip of the Week #166: Dealing with “too big function”](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Occasionally you may run into the following error message:

![\[Warning\]
Decompilation failure:
4612E0: too big function

Please refer to the manual to find appropriate actions](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bigfunc1-3.png?width=355&height=158&name=bigfunc1-3.png)

To ensure that the decompilation speed remains acceptable and does not block IDA, especially when using batch decompilation, by default the decompiler refuses to decompile the functions over 64 kilobytes (0x10000 bytes). But here we have function which is 3x as large:

![sub_4612E0	.text	004612E0	000346E6	00007960](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bigfunc2-3.png?width=904&height=94&name=bigfunc2-3.png)

In such case you can manually increase the size to force the decompiler try decompile the function anyway. The limit can be increased temporarily or permanently.

To change the limit for current database only, open the decompiler options (Edit > Plugins > Hex-Rays Decompiler, Options):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_options2-Jun-18-2024-08-58-33-1668-AM.png?width=729&height=681&name=hr_options2-Jun-18-2024-08-58-33-1668-AM.png)

Then change the setting in Analysis Options 3:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bigfunc3-3.png?width=496&height=387&name=bigfunc3-3.png)

To change the default for all new databases, edit the parameter `MAX_FUNCSIZE` in `hexrays.cfg`.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bigfunc4-3.png?width=634&height=84&name=bigfunc4-3.png)

Note that settings in the config file are only applied to _new databases_ ; for existing databases use the first approach.

Hint: instead of editing `hexrays.cfg` in IDA’s installation, create a new file with only the changed settings in the [user directory](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>). This way the settings will persist if you upgrade your IDA version.

See also:

[Failures and troubleshooting (Hex-Rays Decompiler User Manual)](<https://www.hex-rays.com/products/decompiler/manual/failures.shtml>)

[Igor’s tip of the week #82: Decompiler options: pseudocode formatting](<https://hex-rays.com/blog/igors-tip-of-the-week-82-decompiler-options-pseudocode-formatting/>)

[Igor’s tip of the week #83: Decompiler options: default radix](<https://hex-rays.com/blog/igors-tip-of-the-week-83-decompiler-options-default-radix/>)

[Configuration (Hex-Rays Decompiler User Manual)](<https://www.hex-rays.com/products/decompiler/manual/config.shtml>)

---

## 4. 

**Date:** Posted: Nov 22, 2023  
**Link:** [https://hex-rays.com/blog/black-friday-deals-2023](https://hex-rays.com/blog/black-friday-deals-2023)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Black Friday](<https://hex-rays.com/blog/tag/black-friday>)

# Black Friday Deals – 2023

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/black-friday-deals-2023>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/black-friday-deals-2023>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/black-friday-deals-2023>)

A 

Alex Petrov ✦ Posted: Nov 22, 2023

![Black Friday Deals – 2023](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/black-friday_2023_blog-v2-3.png?width=650&height=255&name=black-friday_2023_blog-v2-3.png)

This year, our Black Friday deals have come early and include incredible opportunities to save money on Training and IDA Home! Here is what is on offer…

## 50% Off All December IDA Pro Online Training Sessions

Are you ready to take your IDA Pro expertise to the next level? Hex-Rays offers a mind-blowing 50% discount on all December IDA Pro Online Training Sessions. Here is how you can claim the offer:

**Eligible users:** Private users holding active IDA Pro license.

**Redeeming the offer:** Be one of the first 10 to register for a Standard or Advanced training session on our [Training Page](<https://hex-rays.com/training/>)

**Remember:** The courses will be conducted in the EST timezone, so mark your calendars accordingly!

## 30%off IDA Home Licenses

Are you a reverse engineering hobbyist or student, or is IDA Free no longer enough? We’ve got you covered! Enjoy a 30% discount on an IDA Home license for a year, and take your reverse engineering to the next level…

**Eligible users:** Private users.

**Redeeming the offer:** Make your purchase directly through the Hex-Rays [Web Shop](<https://hex-rays.com/cgi-bin/quote.cgi/login>)

## Terms and Conditions:

To ensure a smooth and secure experience, please review the following conditions:

  * This offer is valid from 4 pm (CET) Wednesday, 22nd November, **until 3 pm (CET) December 1st, 2023**.

  * Each person is entitled to 30% off IDA Home Named License for one year.

  * **The next ten registrants for any December training session** are eligible for a 50% discount on the usual price. Registrants must have an active IDA Pro license.

  * This offer cannot be used in conjunction with any other offer.

  * Hex-Rays reserves the right to refuse any purchase or registration deemed invalid or fraudulent.

  * Eligibility of participants will be subject to Hex-Rays due diligence process.

  * The offer is only valid for purchases made directly through the Hex-Rays website and correctly submitted registration forms on the Training page.

  * The offer can only be redeemed for one seat/person per order/registration.

  * **Orders/registration are limited to private users only.**

  * Hex-Rays may request identification (ID, passport, etc.) to process your purchase.

  * Hex-Rays reserves the right to cancel or refuse any individual’s benefit from the offer, amend these terms and conditions, or limit the number of discount redemptions online.




Act fast – these exclusive deals are available for a limited time only! All purchases and registrations made during this period must be **fulfilled and paid by Friday, December 1st, at 4:59 pm (CET)**.

For more information or assistance, contact us at [customer_service@hex-rays.com](<mailto:customer_service@hex-rays.com>).

---

## 5. 

**Date:** Posted: Nov 21, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-symless](https://hex-rays.com/blog/plugin-focus-symless)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: Symless

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-symless>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-symless>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-symless>)

A 

Alex Petrov ✦ Posted: Nov 21, 2023

![Plugin focus: Symless](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

_This is a guest entry written by Baptiste Verstraeten from the Thalium Team. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author._

The **Symless** plugin aims to simplify the process of retrieving and defining structures, classes, and virtual tables. Building structures with their fields and managing cross-references is an unavoidable task when reversing a binary that often proves to be very time-consuming. As we will show below, **Symless** allows automating a large part of this process.

## Building a structure

When looking at a piece of code, the analyst may find a new structure being used. IDA has a _create new struct type_ feature in its decompiler view that can be used to recreate the structure in a few clicks. **Symless** offers an enhanced version of this vanilla feature.

It is presented in the form of a new "propagate structure" shortcut in the register submenu of the disassembly view (accessible via a right-click on a register). Using it on a register containing a reference to our new structure allows us to build it, as illustrated in the following screenshot:

![Creating a new structure using Symless](https://hex-rays.com/hubfs/Imported_Blog_Media/propag-new-how-3.png)

Here, we want to create the `CElement` class being used in the `CElement::GetMarkupPtr` class method. We use our "propagate structure" feature on the first use of the `rcx` register (referencing our structure) to start **Symless** static analysis from this point. In the displayed popup, a "build new" tab allows to create a new named structure. Two options are available:

  * _shifted by_ : in case `rcx` contains a shifted structure pointer, specify the shift amount.
  * _spread in callees_ : continue static analysis, and thus structure building, in callees that are given the structure as parameter



Hitting the _propagate_ button starts the analysis. Once completed, the result is as follows:

![New structure created by Symless](https://hex-rays.com/hubfs/Imported_Blog_Media/propag-new-result-3.png)

A basic structural backbone is built using the fields found to be used in the analyzed code. The enhancements from a structure built with the vanilla feature are the following:

  * Accesses made to the structure's fields are typed in the disassembly.
  * Cross-references are placed in the structure, allowing to track the usage of each field.
  * The analysis is not restricted to the current function (by default), fields used in callees are also retrieved.



After defining the structure, the responsibility of assigning meaning to each identified field (by naming and specifying their types) falls upon the analyst.

## Completing an existing structure

Adding fields to an existing structure requires frequent navigation between the code view and the structure panel. **Symless** ' goal is to allow the user to complete their structures from the code view.

In the preceding example, we built an incomplete `CElement` structure from the `CElement::GetMarkupPtr` function, which only uses two fields from `CElement`. It is possible to apply **Symless** on other code samples using `CElement` to complete this structure.

The best way to rebuild a class is to analyze its constructor (here `CElement::CElement`), which initializes most of the class fields. The following screenshot illustrates how to start the analysis in `CElement::CElement`:

![Complete an existing structure](https://hex-rays.com/hubfs/Imported_Blog_Media/propag-existing-how-3.png)

In **Symless** structure builder window, the "from existing" tab enables the selection of a structure for propagation and completion with the gathered information. Once the analysis is completed, the `CElement` structure looks like this:

![Completed structure](https://hex-rays.com/hubfs/Imported_Blog_Media/propag-existing-result-3.png)

We observe that the majority of 'CElement' fields have been successfully retrieved. Existing fields remained unchanged. **Symless** aims to avoid disruption of existing compositions and user modifications, whenever possible.

## Retrieving virtual tables

**Symless** is also able to identify virtual tables. In the last example, the first field of `CElement` was typed with the class virtual table (i.e. `CElement_vtbl`). This virtual table was recreated by **Symless** and cross-references were added to the virtual methods. This is particularly useful for determining where a specific method is used or for resolving indirect calls.

![Created virtual table](https://hex-rays.com/hubfs/Imported_Blog_Media/virtual-table-3.png)

**Symless** analysis is automatically applied to the identified virtual methods as well. This enables more cross-references within our structure and provides a deeper understanding of its composition.

## Conclusion

**Symless** offers a more advanced approach to reconstructing structures through static code analysis. It automates various tasks that users typically perform, including defining fields, setting cross-references, and retrieving virtual tables.

The interactive plugin still requires the analyst to specify the location of a structure being used. An automatic version is also available, capable of retrieving multiple structures and classes without any user interaction. More information about this automatic analysis is available in the project’s `README`.

The **Symless** plugin is available on Github: <https://github.com/thalium/symless>

---

## 6. 

**Date:** Posted: Nov 17, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-165-defining-floating-point-data](https://hex-rays.com/blog/igors-tip-of-the-week-165-defining-floating-point-data)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #165: Defining floating-point data

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-165-defining-floating-point-data>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-165-defining-floating-point-data>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-165-defining-floating-point-data>)

I 

Igor Skochinsky ✦ Posted: Nov 17, 2023

![Igor’s Tip of the Week #165: Defining floating-point data](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

IDA supports different representations for the [instruction operands](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>) and data items. However, only the most common of them are listed in the context menu or have hotkeys assigned. Let’s imagine that you’ve discovered an area in a firmware binary which looks like a table of floating-point values:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fpdata1-3.png?width=535&height=169&name=fpdata1-3.png)

You can confirm that it looks plausible by switching the representation in the [Hex View](<https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view/>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fpdata2-3.png?width=641&height=222&name=fpdata2-3.png)

However, in the disassembly it’s just plain hex bytes:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fpdata3-3.png?width=588&height=375&name=fpdata3-3.png)

How to make a nice table of floating-point values? You have two options:

  1. make items or [arrays](<https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays/>) of integers (dwords in this case) and then change their representation to floating-point (Edit > Operand type > Number > Floating point):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fpdata4-3.png?width=646&height=189&name=fpdata4-3.png)
  2. directly create floating-point data using the Options > Setup data types… dialog (Shortcut `Alt`–`D`). You can quickly pick a data item to create by pressing the underlined [accelerator](<https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida/>) key (e.g. `F` for float or `U` for double):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fpdata5-3.png?width=301&height=430&name=fpdata5-3.png)  
as usual, after creating one item, you can use * to create an array



![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fpdata6-3.png?width=765&height=172&name=fpdata6-3.png)

NB: Tbyte (aka ten-byte) corresponds to the legacy 80-bit [extended precision](<https://en.wikipedia.org/wiki/Extended_precision>) format used in the original 8087 floating point coprocessor and its descendants. It is rarely encountered outside of legacy DOS or Windows software.

See also:

[Igor’s tip of the week #46: Disassembly operand representation](<https://hex-rays.com/blog/igors-tip-of-the-week-46-disassembly-operand-representation/>)

[Igor’s tip of the week #10: Working with arrays](<https://hex-rays.com/blog/igor-tip-of-the-week-10-working-with-arrays/>)

[Igor’s tip of the week #38: Hex view](<https://hex-rays.com/blog/igors-tip-of-the-week-38-hex-view/>)

---

## 7. 

**Date:** Posted: Nov 11, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-164-wheres-my-code-the-case-of-missing-function-arguments](https://hex-rays.com/blog/igors-tip-of-the-week-164-wheres-my-code-the-case-of-missing-function-arguments)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #164: Where’s my code? The case of missing function arguments

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-164-wheres-my-code-the-case-of-missing-function-arguments>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-164-wheres-my-code-the-case-of-missing-function-arguments>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-164-wheres-my-code-the-case-of-missing-function-arguments>)

I 

Igor Skochinsky ✦ Posted: Nov 11, 2023

![Igor’s Tip of the Week #164: Where’s my code? The case of missing function arguments](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Let’s consider this snippet from decompilation of an x86 Windows binary:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/missing-arg1-3.png?width=1087&height=161&name=missing-arg1-3.png)

The same function is called twice with the same argument and the last one doesn’t seem to use the result of the `GetComputerNameExW` call.  
By switching to disassembly, we can see that `eax` is initialized before each call with a string address:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/missing-arg2-3.png?width=675&height=221&name=missing-arg2-3.png)

However the decompiler does not consider it, because on x86 the stack is the usual way of passing arguments and `eax` is most commonly just a temporary scratch register.

One option is to edit the prototype of `sub_10006FC7` to use the [__usercall calling convention](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>) and add `eax` to the argument manually. But when the function is situated in the same binary, it is usually easier to simply go inside the function and decompile it so that the decompiler can see that it does use `eax` before initializing, and so it is added to the argument list:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/missing-arg3-3.png?width=1468&height=544&name=missing-arg3-3.png)

In addition, `esi` is also detected to be an argument.

If we go back to the caller now, we should see the previously missing arguments being passed to the calls:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/missing-arg4-3.png?width=1102&height=165&name=missing-arg4-3.png)

NB: usage of non-standard, custom calling convention is often a sign of either functions using hand-written assembly, or whole program optimization (aka LTO or LTCG) being enabled.

See also:

[Igor’s tip of the week #51: Custom calling conventions](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>)

---

## 8. 

**Date:** Posted: Nov 8, 2023  
**Link:** [https://hex-rays.com/blog/madame-de-maintenons-cryptographic-pursuit-unmasking-the-traitors](https://hex-rays.com/blog/madame-de-maintenons-cryptographic-pursuit-unmasking-the-traitors)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [CTF](<https://hex-rays.com/blog/tag/ctf>) [CTF challenge](<https://hex-rays.com/blog/tag/ctf-challenge>)

# Madame De Maintenon’s Cryptographic Pursuit – Unmasking the Traitors

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/madame-de-maintenons-cryptographic-pursuit-unmasking-the-traitors>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/madame-de-maintenons-cryptographic-pursuit-unmasking-the-traitors>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/madame-de-maintenons-cryptographic-pursuit-unmasking-the-traitors>)

A 

Alex Petrov ✦ Posted: Nov 8, 2023

![Madame De Maintenon’s Cryptographic Pursuit – Unmasking the Traitors](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In the heart of Versailles, an unexpected discovery sent ripples through the palace. Madame de Maintenon (the IDA Lady), the secret wife of Louis XIV, stumbled upon an unusual letter, containing a hidden plot for a coup. This letter, with its strange symbols, triggered a quest for answers. With unwavering determination, Madame de Maintenon (aka Françoise d’Aubigné or Marquise de Maintenon), set out to uncover its secrets – very much like you, the adventurous participants of this CTF challenge. Each passing day brought new discoveries. Clues emerged, and with every piece found, the suspense deepened. Francoise’s journey reflects your own – a testament to preservance and intellect. As you delve into the story, remember that**your goal is to uncover the location of the traitors** , just as she sought the truth behind the enigmatic letter.

Once, the fate of France hung in the balance. N**ow, it is your expertise and ingenuity that will determine the outcome of this "manhunt".**

## **How to participate**

  1. Fill in the registration form: <https://forms.gle/ZdEScRKjrna2eMdj8>
  2. Download the file provided in the registration form above
  3. It is not mandatory, but we strongly suggest using IDA Pro, Home or Free to solve the challenge. Not an IDA user? [Download IDA Free](<https://hex-rays.com/ida-free/#download>).
  4. Solve corectly the challenge and send the flag to [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>).



## Prizes

**1st Prize:** The winner can choose from 

  * IDA Pro Standalone Named License, or
  * Full IDA Swag (F5 cap, T-shirt, neck warmer, mug, and a cup coaster).



**2nd Prize:** F5 Cap and a T-shirt   
**3rd Prize:** One F5 Cap  


**Special prize for the Attendants of the Code Blue 2023 conference:** 50% off a new IDA Pro Standalone Named License & a full IDA Swag!

## Rules of the challenge

  * The challenge is free and open to everyone, interested to participate;
  * Participants must have a valid email address for communication. Employees, affiliates, and immediate family members of the organizing entity are not eligible to participate;
  * Purchase of products and/or services is not required to take part in the challenge. Participants could use any tools to solve the challenge but we encourage using IDA;
  * Any form of cheating will not be tolerated;
  * The organizers reserve the right to disqualify participants for violations of these rules;
  * We may use your personal informationm provided in the registration form, for the following purposes: 
    * We will use your contact information to send you announcements, news, articles, product information, and promotions;
    * We may use third-party email marketing software, such as MailChimp, to facilitate our communication. Your information may be stored in and processed by such services for email marketing purposes.
  * We will not share your personal information with third parties, other companies, or individuals, except as explicitly mentioned in these Rules;
  * We take data security seriously and implement reasonable measures to protect your personal information. However, no method of data transmission over the internet or electronic storage is entirely secure, and we cannot guarantee absolute security;
  * The organizers reserve the right to cancel, modify, or suspend the Challenge for any reason, including but not limited to unforeseen circumstances;
  * Submitting the flag more than once does not guarantee a prize. Only one submission per player will be counted as successful;
  * To be counted as successful, a submission should include: the flag, an overview of the solution, and used plugins;
  * Submissions made after the deadline will not be considered;
  * The winners will be contacted to provide us with their complete shipping details;
  * We reserve the right not to send the prizes to participants from countries under international sanctions;
  * We may share the names or nicknames of the winners on our website and/or our social networks, except in the cases where the participants have explicitly mentioned that they don’t want their names to be mentioned in our publicity;
  * The winners will be determined by a random draw, except for the special prize for the attendants of the Code Blue 2023 conference. The winner of the special prize will be the first who solved and correctly submitted the flag, following the requirements, mentioned above.
  * By participating in the “Madame De Maintenon’s Cryptographic Pursuit – Unmasking the Traitors,” you agree to these rules.Hex-Rays reserve the right to update or modify these terms as necessary.
  * For any inquiries or issues related to the Challenge, please contact marketing@hex-rays.com



## Important Dates:

  1. CTF start date: 8 November 2023
  2. Submission Deadline for Code Blue 2023 participants: 9 November 2023, 18:00
  3. Submission Deadline for all other participants: 30 November 2023, 23:59 CET
  4. Announcement of the winners: 5 December 2023



**Best of luck as you step into this thrilling adventure!**

---

## 9. 

**Date:** Posted: Nov 3, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-163-names-list](https://hex-rays.com/blog/igors-tip-of-the-week-163-names-list)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #163: Names list

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-163-names-list>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-163-names-list>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-163-names-list>)

I 

Igor Skochinsky ✦ Posted: Nov 3, 2023

![Igor’s Tip of the Week #163: Names list](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

The [Functions list](<https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list/>) is probably the most known and used part of IDA’s default desktop layout. It includes all detected functions in the current database and offers a quick way to find and navigate to any of them. However, the database consists not only of functions but also data items or instructions which are not included into any function. For example, there may be a name provided by the debug info but IDA can fail to create a function for some reason. In that case you can have a _name_ but no function. You can [force create](<https://hex-rays.com/blog/igors-tip-of-the-week-152-force-creating-functions/>) a function, but how to even discover such situation? If you know the name beforehand, you can use the [“Jump to address”](<https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places/>) (`G`) action to jump to it, but it can get complicated if the name is long or has uncommon characters. How to discover such names without scrolling through the whole listing?

### Names window

A separate view with the list of names can be opened using View > Open subviews > Names (`Shift`–`F4`).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/names1-3.png?width=608&height=266&name=names1-3.png)

Many entries will be the same as in the Functions list:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/names2-3.png?width=784&height=461&name=names2-3.png)

But there are also differences:

  * the Functions list includes _all_ functions, including unnamed ones (they get a [dummy name](<https://hex-rays.com/blog/igors-tip-of-the-week-34-dummy-names/>) beginning with `sub_`)
  * the Names list includes not only function names (marked with 𝑓) but also other names, for example global labels (`i`) or data items (`D`).



### Jump by name

Instead of permanent Names window, you can also use Jump > Jump by name… (`Ctrl`–`L`). This shows the same list as a modal chooser, meaning you can [use filtering or incremental search](<https://hex-rays.com/blog/igors-tip-of-the-week-36-working-with-list-views-in-ida/>) to find the name you need and jump to it with `Enter` or double-click.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/names3-3.png?width=595&height=632&name=names3-3.png)

See also:

[Igor’s tip of the week #28: Functions list ](<https://hex-rays.com/blog/igors-tip-of-the-week-28-functions-list/>)

[Igor’s tip of the week #20: Going places ](<https://hex-rays.com/blog/igors-tip-of-the-week-20-going-places/>)

---

