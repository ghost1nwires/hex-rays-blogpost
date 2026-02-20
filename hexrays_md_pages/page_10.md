# Hex-Rays Blog – Page 10

**Archived:** 2026-02-20 12:12
**URL:** https://hex-rays.com/blog/page/10

---

## 1. 

**Date:** Posted: Oct 27, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-162-wheres-my-code-the-case-of-no-return-call](https://hex-rays.com/blog/igors-tip-of-the-week-162-wheres-my-code-the-case-of-no-return-call)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #162: Where’s my code? The case of no-return call

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-162-wheres-my-code-the-case-of-no-return-call>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-162-wheres-my-code-the-case-of-no-return-call>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-162-wheres-my-code-the-case-of-no-return-call>)

I 

Igor Skochinsky ✦ Posted: Oct 27, 2023

![Igor’s Tip of the Week #162: Where’s my code? The case of no-return call](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Let’s say you found a promising-looking string in the binary, followed the cross reference to the function using it, then decompiled it to see how the string is used, only to see no signs of it in the pseudocode. What’s happening?

  
In such situation it often helps to set up two [synchronized](<https://hex-rays.com/blog/igors-tip-of-the-week-154-synchronized-views/>) disassembly<->pseudocode views and scroll through them looking for oddities. As a rule of thumb, most pseudocode lines should map to one or few assembly instructions and most assembly instructions (except [skippable](<https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions/>) ones such as function prolog or epilog) should map to some line in pseudocode.

Here’s an example of a strange case: a single function call in pseudocode maps to not only the call instruction but also a bunch of seemingly unrelated instructions after it:  
  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/misssing-noret1-3.png?width=931&height=418&name=misssing-noret1-3.png)  
It is then followed by instructions which do not have any mapping in the pseudocode:  
  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/misssing-noret2-3.png?width=950&height=374&name=misssing-noret2-3.png)  
[Hovering](<https://hex-rays.com/blog/igors-tip-of-the-week-66-decompiler-annotations/>) the mouse on the call gives a clue: it’s been marked as no-return:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/misssing-noret3-3.png?width=303&height=108&name=misssing-noret3-3.png)

Since there’s obviously some valid code after it, it seems to be a false positive. Removing the `__noreturn` attribute from the prototype (hint: [`Y` shortcut](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)) brings back the missing code and more regular instruction mapping:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/misssing-noret4-3.png?width=1093&height=379&name=misssing-noret4-3.png)

NB: in some cases you may have to clear the noret flag in [function’s properties](<https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions/>) in addition to fixing the prototype, or the `__noreturn` attribute will keep coming back.

The reasons why the function could be incorrectly marked as no-ret are numerous; for example, it may have been found to end in an infinite loop or have code paths only leading to other no-ret functions due to other issues. It may be worth investigating such functions more closely, especially if you discover multiple instances of them.

Note that in some cases you may see valid-looking code after a function call even though the function **really** does not return. This could be caused by:

  *     * the compiler not deducing that the function does not return
    * old compiler which does not perform dead code removal
    * optimization settings during compilation
    * other reasons (e.g. [incorrect assumptions](<https://hex-rays.com/blog/igors-tip-of-the-week-159-wheres-my-code-the-case-of-not-so-constant-data/>) by the decompiler)



See also:

[Igor’s Tip of the Week #126: Non-returning functions](<https://hex-rays.com/blog/igors-tip-of-the-week-126-non-returning-functions/>)  
[Igor’s Tip of the Week #159: Where’s my code? The case of not-so-constant data](<https://hex-rays.com/blog/igors-tip-of-the-week-159-wheres-my-code-the-case-of-not-so-constant-data/>)

---

## 2. 

**Date:** Posted: Oct 20, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-161-extracting-substructures](https://hex-rays.com/blog/igors-tip-of-the-week-161-extracting-substructures)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #161: Extracting substructures

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-161-extracting-substructures>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-161-extracting-substructures>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-161-extracting-substructures>)

I 

Igor Skochinsky ✦ Posted: Oct 20, 2023

![Igor’s Tip of the Week #161: Extracting substructures](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

As [covered before](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>), the action “Create struct from selection” can be used to quickly create structures from existing data items. 

![](https://hex-rays.com/hubfs/Imported_Blog_Media/sel_struct1-Jun-18-2024-08-49-28-1135-AM.png)

However, Disassembly view not the only place where it can be used. For example, let’s imagine you’ve created a structure to represent some context used by the binary being analyzed:
    
    
    00000000 Context         struc ; (sizeof=0x1C)
    00000000 version         dd ?
    00000004 pid             dd ?
    00000008 tid             dd ?
    0000000C listhead        dd ?                    ; offset
    00000010 listtail        dd ?                    ; offset
    00000014 count           dd ?
    00000018 filename        dd ?
    0000001C Context         ends

But as you analyze the code further, you realize that the list structure is generic and is used in other places independently. In this simple scenario you can, of course, create a separate `List` structure and replace the fields inside `Context` with it, but what if the substructure is big and contains hundreds of fields? Using “Create struct from selection” allows you to perform the task easily and quickly:

  1. [select](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>) the subset of fields you want to extract;
  2. invoke the action  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/substruct1-3.png?width=577&height=346&name=substruct1-3.png)
  3. rename the new structure and/or fields as necessary.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/substruct2-3.png?width=999&height=508&name=substruct2-3.png)



### Extracting structures from the stack frame

One more place where this action can be used is the [stack frame view](<https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view/>), because internally it is a kind of a structure.![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/substruct3-3.png?width=720&height=678&name=substruct3-3.png)

See also:

[Igor’s tip of the week #11: Quickly creating structures](<https://hex-rays.com/blog/igor-tip-of-the-week-11-quickly-creating-structures/>)

[Igor’s tip of the week #03: Selection in IDA](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>)

---

## 3. 

**Date:** Posted: Oct 18, 2023  
**Link:** [https://hex-rays.com/blog/mycreepycodecontest-unleash-your-scariest-code-snippets](https://hex-rays.com/blog/mycreepycodecontest-unleash-your-scariest-code-snippets)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [plugin repository](<https://hex-rays.com/blog/tag/plugin-repository>)

# MyCreepyCodeContest – Unleash Your Scariest Code Snippets!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/mycreepycodecontest-unleash-your-scariest-code-snippets>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/mycreepycodecontest-unleash-your-scariest-code-snippets>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/mycreepycodecontest-unleash-your-scariest-code-snippets>)

A 

Alex Petrov ✦ Posted: Oct 18, 2023

![MyCreepyCodeContest – Unleash Your Scariest Code Snippets!](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/idaloween2023_blog-3.png?width=1000&height=352&name=idaloween2023_blog-3.png)

Halloween is approaching, and we’ve decided to celebrate it by launching the **#MyCreepyCodeContest**. Whether you are a seasoned reverser or just an enthusiast, our **#MyCreepyCodeContest** invites you to dig up and share the most spine-chilling pieces of code you’ve encountered in the wild.

Everyone is welcome to participate, regardless of experience. The goal is to…well, to have fun! We spend most of our time analyzing code, and there are always bits that are simply terrifying and haunt our reverse engineering journey. It is time to expose these spooky pieces of code.

This year, the winners will be two. We will select one, and the community will decide the other one.

## How to participate

It is as simple as pumpkin soup!

  1. Follow us on Twitter, LinkedIn or Mastodon;
  2. Post a snapshot of the scary code using **#MyCreepyCodeContest** ;
  3. Mention us in the post;
  4. Keep an eye on our website. We will publish all entries and allow you to vote for the best entry.



## Prizes

There will be two prizes this year:

  * **Hex-Ray’s Choice Award** – our team will choose the creepiest code snippet, and the winner will get an exclusive T-shirt with his/her scary code snippet printed on it.
  * **Community Choice Award** – the community will decide on their favorite submission. Again, the winner will get an exclusive T-shirt with her/his scary code snippet printed on it.



## Guidelines

  1. All conditions from "How to participate" should be met;
  2. The code snippet you share should be something you’ve encountered in your reverse engineering journey;
  3. Please remember that your code snippet should adhere to the theme of being creepy;
  4. Excerpts from any programming languages are welcome;
  5. Multiple entries are allowed, so dare & share!
  6. The shorter the code, the better. Take into account that the winning snippet would be printed on a T-shirt, and it has to fit in it;
  7. Submissions made after the deadline will not be considered;
  8. The two winners will be contacted to provide us with their complete shipping details;
  9. We reserve the right not to send the prizes to participants from certain countries;
  10. Cheating will not be tolerated. Any manipulation of the voting process and/or results will lead to disqualification, and the award will be given to the next in the ranking;
  11. Should you have any questions with regards to the contest, please get in touch with [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>).



## Voting Process

For the Community Choice Award, we’re spicing things up with a voting process…

  * **Submission Period** : Submissions will be accepted until the contest deadline;
  * **Voting Period:** After the submission deadline, we will publish on our website all submissions gathered from our social networks;
  * **Community Voting:** During the voting period, visitors to our website can cast their votes for their favorite submission;
  * **Announcement:** We will announce the winner of the Community Choice Award after the voting period ends.



## Important Dates:

**Contest Start Date:** October 18th, 2023  
**Submission Deadline:** October 31st, 2023  
**Community Voting Period:** November 1st – November 6th, 2023

**Good luck, and may the creepiest code snippets win!**

## Vote for the Community Choice Award

We have received seven really scary submissions, and we need your help to decide which one deserves the second prize in this year’s #MyCreepyCodeContest.

Here are the challengers:

---

## 4. 

**Date:** Posted: Oct 17, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-idaclu](https://hex-rays.com/blog/plugin-focus-idaclu)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: IdaClu

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-idaclu>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-idaclu>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-idaclu>)

A 

Alex Petrov ✦ Posted: Oct 17, 2023

![Plugin focus: IdaClu](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

_This is a guest entry written by Sergejs Harlamovs from IKARUS Security Software GmbH. His views and opinions are his own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the author._

## IdaClu: Finding clues without knowing what to seek

**_[IdaClu](<https://plugins.hex-rays.com/idaclu>)_** , as the name suggests, is about _"clusterization"_ and _"finding the clues"_. The plugin offers a toolset to group functions based on various criteria. This makes it particularly valuable for analyzing large samples with minimal or no context.

## The problem and the solution

When reverse engineering in **_IDA_** , identifying function groups is a common task. To understand how a specific program works, it's more effective to work with groups rather than focusing on each function separately. However, it's not always the case that connected functions are referencing each other with _xrefs_ and will eventually appear in the same call stack. **_Sometimes the connection between the functions is weak making building the group a non-trivial task._**

**_IDA_** provides rich search functionality out of the box and there are some handy plugins that are pushing it even further. Finding common patterns in code might help partially with the discussed problem, but still requires a significant amount of time to identify the relevant parts.

**_A reasonable solution would be to have an automatic tool that highlights common points between functions…_** and to make it extensible… and to make it cross-compatible across different versions of **_IDA_** … and a helicopter available on a rooftop and a lot of explosions and sharks.

## Use Cases

Some of the most prominent use cases include:

  * identifying functions with similar code structure using fuzzy hashing and a combination of the following methods:

    * function byte pattern similarity
    * function opcode similarity
    * functon pseudocode text similarity
![Figure 1: Cluster of functions with similar structure](https://hex-rays.com/hubfs/Imported_Blog_Media/res_clusters-1-3.png) Figure 1: Cluster of functions with similar structure 
  * identifying all immediates in the code, excluding those that can be interpreted as addresses and find the corresponding functions referencing these values

![Figure 2: Numeric constant and references to it](https://hex-rays.com/hubfs/Imported_Blog_Media/res_constants-1-3.png) Figure 2: Numeric constant and references to it 
  * identifying frequently referenced _VFT_ -offsets and the various argument variations received by corresponding functions

![Figure 3: Implicit function call contexts](https://hex-rays.com/hubfs/Imported_Blog_Media/res_arguments-1-3.png) Figure 3: Implicit function call contexts 
  * identifying most referenced strings and the functions referencing them

![Figure 4: Common string references](https://hex-rays.com/hubfs/Imported_Blog_Media/res_strings-1-3.png) Figure 4: Common string references 
  * identifying functions implementing specific control-flow constructs

![Figure 5: Functions containing switch-case and for-loop statements](https://hex-rays.com/hubfs/Imported_Blog_Media/res_control-1-3.png) Figure 5: Functions containing switch-case and for-loop statements 



## Features

### Toolset

The functions can be grouped differently based on the chosen algorithm (_"tool"_ in terms of **_IdaClu_**). Each tool is represented by a corresponding button in the main GUI dialog's sidebar and is backed by a separate script in the **_IdaClu_** plugin subfolder.

![Figure 6: Tool explorer](https://hex-rays.com/hubfs/Imported_Blog_Media/nav_toolset-1-3.png) Figure 6: Tool explorer 

Most tools are self-sufficient by design, but some may require user input for proper function grouping. In such cases, tool button must be clicked twice: first to display input controls, and then again to submit input data and initiate grouping/clustering.

![Figure 7: Tool arguments](https://hex-rays.com/hubfs/Imported_Blog_Media/toolset_args-1-3.png) Figure 7: Tool arguments 

As of the time of this writing, the toolset consists of _18_ grouping/clustering algorithms available out of the box, organized into sections for convenience. A detailed description of these tools is available on the GitHub repository [page](<https://github.com/harlamism/IdaClu#standard-scripts>).

### Labeling

**_IdaClu_** offers several function labeling tools. They allow to flag grouped functions in bulk. These tools are built on top of native and well-known **_IDA_** features:

  * prefixing/renaming the functions
  * highlighting functions with a color
  * moving functions to a specific folder



Functions can have multiple labels applied, and there is a toggle for recursive mode. When switched on, all the functions down the tree that are referenced by currently selected functions will be labeled in the same way.

![Figure 8: Labeling tools](https://hex-rays.com/hubfs/Imported_Blog_Media/nav_labeling-1-3.png) Figure 8: Labeling tools 

The labeling tools activate when functions are grouped and when at least one function is selected in a tree/table view. In case something goes wrong or there's a need to alter a specific function name, it can all be done inside **_IdaClu_** without switching to the standard _Functions_ subview. To clear _prefix/folder_ labels, click the _"CLEAR"_ button in the corresponding mode. To rename a specific function, right-click it and select the _"Rename"_ option.

### Filtering

The grouping iteration count can be arbitrary. It makes sense to continue as long as new connections between the functions can be found and new conclusions about program functionality can be made. **_IdaClu_** considers all functions discovered by **_IDA_** as potential candidates for grouping. The filtering feature helps narrow down the scope and makes grouping more specific. To do so, it uses user defined labels – prefixes, folders, and colors.

![Figure 9: Filter explorer](https://hex-rays.com/hubfs/Imported_Blog_Media/nav_filters-1-3.png) Figure 9: Filter explorer 

Labeled functions can form distinct sets for upcoming grouping iterations. There are as many filters as labeling tools. To focus on the payload of the analyzed sample and exclude library functions from grouping, set prefix filter to **_sub__** value. This is the only standard prefix.

### Compatibility

While many of us try to stick to the very last version of **_IDA_** there are several reasons why some opt for the older versions. For instance, not upgrading the _.idb_ version to _7.x_ or _8.x_ may be necessary if some team members are using older versions. In rare cases, older versions might produce more consistent decompiled code. Regardless of the reason, many will agree that checking plugin compatibility with the installed **_IDA_** version can be inconvenient.

![Figure 10: Environment descriptor object printout](https://hex-rays.com/hubfs/Imported_Blog_Media/env_desc-1-3.png) Figure 10: Environment descriptor object printout 

**_IdaClu_** aims to be as **_IDA_** -version agnostic as possible. Using a set of **_IdaPython_** and **_Qt_** -shims alone proved insufficient. To address this, the plugin introduces an object that stores the current IDA setup and supported feature state. This object can be passed to any plugin component, ensuring cross-compatibility. When there is a replacement for specific features, shims are utilized. Otherwise, these features, along with related UI controls and functionality, are excluded, ensuring graceful degradation.

### Ecosystem

**_IdaClu_** was designed to be highly extensible from the very beginning. There were three main reasons for this:

  * Predicting all potential use cases for this plugin was challenging due to the diverse contexts of software reverse engineering. Many other _tools/sub-plugins_ for **_IdaClu_** will likely appear in the future.
  * Advanced users should be able to influence the plugin easily, without having to reverse-engineer it or wait for feature requests to be fulfilled by the author.
  * Writing an **_IdaPython_** script is much simpler than creating a plugin and delving into the **_Qt_** framework. The community scripts could benefit from **_IdaClu's_** GUI interface if converted to corresponding _tools/sub-plugins_.



If any of the mentioned points resonate with you, there might be a reason to write a _sub-plugin_ /_tool_ for **_IdaClu_**. Any valid **_IdaPython_** script that returns a dictionary of lists, where each list element is a function address, is a good candidate for conversion.

It's super-easy to start:

  1. Get the existing **_IdaPython_** script or write a new simplistic one
  2. Add the following block at the start
         
         SCRIPT_NAME = '<script_name>'  # arbitrary name that will appear on the corresponding button
          SCRIPT_TYPE = 'func'           # 'func' or 'custom' depending on whether the script iterates on functions or some other data structures to produce the output
          SCRIPT_VIEW = 'tree'           # 'tree' is the only currently supported view, 'table' is to be added
          SCRIPT_ARGS = []               # experimental feature, supports tuples of the form ('<control_name>', '<control_type>', '<control_placeholder>')
         

  3. Add/replace the main function header with one of the following prototypes:
         
         # Case #1: SCRIPT_TYPE == 'func':
          def get_data(func_gen=None, env_desc=None, plug_params=None):
              # 1. Iterate over pre-filtered functions via func_gen() generator
              # 2. Progress bar values are calculated automatically
         
         
         # Case #2: SCRIPT_TYPE == 'custom':
          def get_data(progress_callback=None, env_desc=None, plug_params=None):
              # 1. Iterate over custom data structures
              # 2. Use `progress_callback(<current_index>, <total_count>)` to report current progress
         

  4. Make sure **_get_data()_** function returns a dictionary of lists, where each list element is a function address

  5. Navigate to **_IdaClu_** plugin _"plugin"_ sub-folder and place your script file under any of the existing **_group_x_** folders




Now, launch **_IDA_** and **_IdaClu_** and see if it works. If you were lucky to make it work and you find it possible to share please do so and contribute to the tool repository of **_IdaClu_** 😉

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/plugin-repo-Jun-18-2024-08-41-28-3769-AM.png?width=530&height=324&name=plugin-repo-Jun-18-2024-08-41-28-3769-AM.png)](<https://plugins.hex-rays.com/idaclu>)

---

## 5. 

**Date:** Posted: Oct 13, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-160-hiding-casts-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-160-hiding-casts-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #160: Hiding casts in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-160-hiding-casts-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-160-hiding-casts-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-160-hiding-casts-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Oct 13, 2023

![Igor’s Tip of the Week #160: Hiding casts in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

In order to faithfully represent the behavior of the code and to conform to the rules of the C language, the decompiler may need to add casts in the pseudocode. A few examples:

  * a variable has been detected to be unsigned but participates in a signed comparison:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cast1-3.png?width=386&height=239&name=cast1-3.png)
  * An argument being passed to a function does not match the prototype:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cast2-3.png?width=480&height=590&name=cast2-3.png)  
  

  * A narrow value (less that register size) is being loaded from or stored to memory:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cast3-3.png?width=625&height=214&name=cast3-3.png)
  * Actual arguments being passed to a function call do not match the prototype (or the prototype is not known):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cast4-3.png?width=837&height=40&name=cast4-3.png)
  * and so on



In some cases you may want to only look at the overall structure of the function and casts can be distracting. In such case you can hide them by using the “Hide casts” context menu action, or the shortcut `\` (backslash).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/cast5-3.png?width=463&height=241&name=cast5-3.png)

To turn them back on, use the “Show casts” action (same shortcut).

NB: while hiding casts may result in “cleaner-looking” pseudocode, the result may be no longer correct C and hide various issues visible with casts. So it is not recommended to leave them off permanently. And if you notice that the output seems to be wrong (for example, [pointer math](<https://hex-rays.com/blog/igors-tip-of-the-week-138-pointer-math-in-the-decompiler/>) does not correspond to the assembly), it may be caused by the accidental pressing of the backslash, so check the context menu and show casts again if they were off.

See also:

[Decompiler Manual: Hex-Rays interactive operation: Hide/unhide cast operators](<https://www.hex-rays.com/products/decompiler/manual/cmd_hide_casts.shtml>)

---

## 6. 

**Date:** Posted: Oct 6, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-159-wheres-my-code-the-case-of-not-so-constant-data](https://hex-rays.com/blog/igors-tip-of-the-week-159-wheres-my-code-the-case-of-not-so-constant-data)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #159: Where’s my code? The case of not-so-constant data

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-159-wheres-my-code-the-case-of-not-so-constant-data>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-159-wheres-my-code-the-case-of-not-so-constant-data>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-159-wheres-my-code-the-case-of-not-so-constant-data>)

I 

Igor Skochinsky ✦ Posted: Oct 6, 2023

![Igor’s Tip of the Week #159: Where’s my code? The case of not-so-constant data](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In order to show the user only the most relevant code and hide the unnecessary clutter, the decompiler performs various optimizations before displaying the pseudocode. Some of these optimizations rely on various assumptions which are usually correct in well-behaved programs. However, in some situations they may be incorrect which may lead to wrong output, so you may need to know how to fix them.

### Constant data and dead code removal

Consider this example from a recent iOS kernelcache:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const3-3.png?width=2175&height=910&name=const3-3.png)

The function looks non-trivial (I’ve had to zoom out the graph) but the pseudocode is very short. Most of the conditional branches visible in the graph have disappeared. What’s happening?

You may get some hints from the warnings shown by the decompiler when you first decompile the function:

![\[Information\]
The decompiler assumes that the segment 'com.apple.kernel:__cstring' is read-only because of its NAME.
All data references to the segment will be replaced by constant values.
This may lead to drastic changes in the decompiler output.
If the segment is not read-only, please change the segment NAME.

In general, the decompiler checks the segment permissions, class, and name
to determine if it is read-only.](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const1-3.png?width=923&height=370&name=const1-3.png)

If you have checked “Don’t display message again” previously, the message will be shown in the Output window:

![\[autohidden\] The decompiler assumes that the segment 'com.apple.kernel:__cstring' is read-only because of its NAME.
All data references to the segment will be replaced by constant values.
This may lead to drastic changes in the decompiler output.
If the segment is not read-only, please change the segment NAME.

In general, the decompiler checks the segment permissions, class, and name
to determine if it is read-only.
 -> OK
FFFFFFF007E6ABAC: conditional instruction was optimized away because cf.1==1](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const2-3.png?width=1275&height=203&name=const2-3.png)

The last message is also a useful hint: if you double-click the address you’ll land on the instruction which was “optimized away”.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const4-3.png?width=1205&height=485&name=const4-3.png)

If we double-click the variables involved in the condition, we can see that they both are situated in the segment mentioned in the warning message (`com.apple.kernel:__const`):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const5-3.png?width=791&height=133&name=const5-3.png)

So, apparently, because the segment is considered read-only and both variables are set to zero, the decompiler deduced that the branch is always taken and removed the check itself as dead code. Similarly it removed many of the previous comparisons. But is this assumption correct?

### Not very constant

If we check cross-references to the variables, we can see that they’re actually written to:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const6-3.png?width=1214&height=563&name=const6-3.png)

And if we try to decompile the function doing it, we’ll see some “interesting” messages:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const7-3.png?width=937&height=591&name=const7-3.png)

And again, the addresses mentioned are all in the same supposedly-constant segment.

So we can make this conclusion: despite the name, the segment is not actually constant. How do we tell the decompiler that? 

The easiest solution is to add the Write flag to the segment’s permission. This can be done via the Edit > Segments > Edit segment… command (shortcut Alt-S).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const8-3.png?width=717&height=521&name=const8-3.png)

After refreshing the pseudocode (F5), we no longer see the red warning or mentions of “write access to const memory”. And if we go back to the previously “too short” function, it now has all the checks which were missing:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/const9-3.png?width=1173&height=892&name=const9-3.png)

See also:

[Igor’s tip of the week #16: Cross-references](<https://hex-rays.com/blog/igor-tip-of-the-week-16-cross-references/>)

[Igor’s tip of the week #56: String literals in pseudocode](<https://hex-rays.com/blog/igors-tip-of-the-week-56-string-literals-in-pseudocode/>)

[Decompiler Manual > Tips and tricks: Constant memory](<https://docs.hex-rays.com/user-guide/decompiler/tricks#constant-memory>)

---

## 7. 

**Date:** Posted: Sep 29, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-158-refreshing-pseudocode](https://hex-rays.com/blog/igors-tip-of-the-week-158-refreshing-pseudocode)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #158: Refreshing pseudocode

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-158-refreshing-pseudocode>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-158-refreshing-pseudocode>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-158-refreshing-pseudocode>)

I 

Igor Skochinsky ✦ Posted: Sep 29, 2023

![Igor’s Tip of the Week #158: Refreshing pseudocode](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

When working with the decompiler, you probably spend most of the time in the pseudocode view, since most interactive operations (e.g. [renaming, retyping](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>) and [commenting](<https://hex-rays.com/blog/igor-tip-of-the-week-14-comments-in-ida/>)) can be done right there. IDA is usually smart enough to detect important changes during such actions and update the pseudocode as necessary.

However, occasionally you may perform actions outside of the pseudocode view which potentially affect decompilation, but IDA may not detect it and continue showing stale decompilation. How can you force IDA to use the new information? The following options may be used:

  1. close (with saving) and reopen the database. This is the most invasive option but is probably most reliable;
  2. close just the pseudocode view and reopen it by decompiling the function again (e.g. by pressing `Tab`);
  3. refresh the pseudocode by pressing `F5` while in the Pseudocode view.



Usually the methods above are enough, but in some cases you may need to [reset decompiler caches](<https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information/>) for a complete refresh.

See also:

[Igor’s tip of the week #40: Decompiler basics](<https://hex-rays.com/blog/igors-tip-of-the-week-40-decompiler-basics/>)

[Igor’s tip of the week #42: Renaming and retyping in the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)

[Igor’s tip of the week #102: Resetting decompiler information](<https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information/>)

---

## 8. 

**Date:** Posted: Sep 21, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-season-03](https://hex-rays.com/blog/igors-tip-of-the-week-season-03)

[Back](<https://hex-rays.com/blog>)

[idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [“Igor’s tip of the week.”](<https://hex-rays.com/blog/tag/igors-tip-of-the-week->)

# Igor’s tip of the week: Season 03

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-season-03>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-season-03>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-season-03>)

A 

Alex Petrov ✦ Posted: Sep 21, 2023

![Igor’s tip of the week: Season 03](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Welcome to a new chapter of Igor’s invaluable insights! At Hex-Rays, we understand the importance of continuous learning in our ever-evolving field. Therefore, we are thrilled to introduce you to **[Igor’s Tip of the Week – Season 3.](</hubfs/freefile/igor-tip-of-the-week-S03.pdf>)**

Three years ago, we embarked on a mission to empower IDA’s community with Igor’s practical bits of advice. Over time, these tips have become an indispensable resource for both beginners and seasoned professionals.

This latest edition of **[“Igor’s tip of the week.”](<https://hex-rays.com/blog/tag/idatips/>)** promises to offer you an enriching learning experience. We’ve curated Igor’s tips into six categories, each designed to sharpen your skills and deepen your understanding of IDA and Decompilers:

  * **Usage:** basic and advanced usage of IDA features
  * **Types:** working with types
  * **Hidden:** hidden gems, not widely known but useful functionality
  * **Decompiler:** related to the Hex-Rays decompiler
  * **Automation:** automating repetitive tasks
  * **Customization:** customizing IDA UI to better suit your workflow.



### Missed the previous two seasons?

If you haven’t read **Season 1** and **Season 2** , there is no better time to start. Igor’s tips from the past are timeless and extremely useful. Take this opportunity to revisit and absorb the knowledge that laid the foundation of what we offer today.  
  
**[Read Season 1](<https://hex-rays.com/blog/igors-tip-of-the-week-season-01/>)** | **[Read Season 2](<https://hex-rays.com/blog/igors-tip-of-the-week-season-02/>)**

### Want to further enhance your skills?

Are you ready to dive really deep into IDA and elevate your skills? Join us for one of our upcoming training sessions, and take the opportunity to learn, grow, and connect with like-minded colleagues. Discover how to leverage the full power of IDA and significantly improve your workflow. **[Register Today!](<https://hex-rays.com/training/?utm_source=Blog-Post-Article&utm_medium=Blog-Post&utm_campaign=ITOTW-S3>)**

### Hungry for more valuable insights?

Why don’t you follow us on our social networks and stay up-to-date with the latest tips, tricks, and news:  
  
[Follow us on Twitter](<https://twitter.com/hexrayssa?lang=en>) | [Follow us on LinkedIn](<https://www.linkedin.com/company/hex-rays-sa>) | [Follow us on Mastodon](<https://infosec.exchange/@HexRaysSA>) | [Follow us on YouTube](<https://www.youtube.com/channel/UCqNQfYIIJw1L4ou5ej6iysw/featured>)

[![Igor-tip-book-S03](https://hex-rays.com/hubfs/Imported_Blog_Media/igor-poster-season-3-v4-3.png)](<https://hex-rays.com/hubfs/freefile/igor-tip-of-the-week-S03.pdf>)

[ Download Season 3](</hubfs/freefile/igor-tip-of-the-week-S03.pdf>)

---

## 9. 

**Date:** Posted: Sep 15, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-157-removing-function-arguments-in-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-157-removing-function-arguments-in-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #157: Removing function arguments in decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-157-removing-function-arguments-in-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-157-removing-function-arguments-in-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-157-removing-function-arguments-in-decompiler>)

I 

Igor Skochinsky ✦ Posted: Sep 15, 2023

![Igor’s Tip of the Week #157: Removing function arguments in decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

When you need to change the prototype of a function in the decompiler, the standard way is to use the “Set item type…” action (shortcut `Y`).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_args1-3.png?width=1024&height=141&name=hr_args1-3.png)

One case where you may need to do it is to add or remove arguments. Especially in embedded code or when decompiling variadic functions, the decompiler may deduce the argument list wrongly. A good test for bogus arguments is to check whether they’re referenced in the function’s body. For this, use “Jump to xref” (shortcut `X`) on the argument:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_args2-3.png?width=1220&height=288&name=hr_args2-3.png)

If there are no references to an argument, it’s likely that it (and probably the following ones) are fake. You can remove them by editing the prototype, but there is an easier way: “Remove function argument” action (shortcut `Shift`–`Del`).

### Deleting return value

In the absence of reliable information to the contrary, the decompiler assumes that a function returns something and produces the pseudocode accordingly, which can lead to unoptimized or awkward output. For example, consider this small function from an ARM firmware:
    
    
    _BYTE *sub_25C4()
    {
      char CPSR; // r1
      _BYTE *result; // r0
    
      CPSR = __get_CPSR();
      __get_CPSR();
      __disable_irq();
      result = &byte_10001E5C;
      if ( !byte_10001E5C )
      {
        unk_100014CC = (CPSR & 1) == 0;
        byte_10001E5C = 1;
      }
      return result;
    }

Because an intermediate address is stored in the register `R0` (the standard return value register on ARM), and decompiler assumes that the function returns a value, which leads to awkward-looking code. Since here it seems to be an incorrect assumption, we can remove the return value in the same fashion as the arguments:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_args3-3.png?width=507&height=240&name=hr_args3-3.png)

The pseudocode gets updated with the new assumption and the `return` statement is gone:
    
    
    void __fastcall sub_25C4()
    {
      char CPSR; // r1
    
      CPSR = __get_CPSR();
      __get_CPSR();
      __disable_irq();
      if ( !byte_10001E5C )
      {
        unk_100014CC = (CPSR & 1) == 0;
        byte_10001E5C = 1;
      }
    }

In addition to removing return value using the context action on it, you can use a shortcut which works anywhere in the function: `Ctrl`–`Shift`–`R`. It can also be use to re-introduce a return value to a void function.

If you prefer a different shortcut, look for `AddRemoveReturn` in the [shortcut editor](<https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/>):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hr_args4-3.png?width=1025&height=553&name=hr_args4-3.png)

See also:

[Igor’s tip of the week #42: Renaming and retyping in the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)

---

