# Hex-Rays Blog – Page 3

**Archived:** 2026-02-20 12:10
**URL:** https://hex-rays.com/blog/page/3

---

## 1. 

**Date:** Posted: Sep 5, 2025  
**Link:** [https://hex-rays.com/blog/mapping-relationships-in-ida-9.2-dynamic-xref-graph-and-xref-tree](https://hex-rays.com/blog/mapping-relationships-in-ida-9.2-dynamic-xref-graph-and-xref-tree)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [UI](<https://hex-rays.com/blog/tag/ui>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>) [Xref Graph](<https://hex-rays.com/blog/tag/xref-graph>) [Xref Tree](<https://hex-rays.com/blog/tag/xref-tree>)

# Mapping Relationships in IDA 9.2: Dynamic Xref Graph and Xref Tree

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/mapping-relationships-in-ida-9.2-dynamic-xref-graph-and-xref-tree>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/mapping-relationships-in-ida-9.2-dynamic-xref-graph-and-xref-tree>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/mapping-relationships-in-ida-9.2-dynamic-xref-graph-and-xref-tree>)

Hex-Rays ✦ Posted: Sep 5, 2025

![Mapping Relationships in IDA 9.2: Dynamic Xref Graph and Xref Tree](https://hex-rays.com/hubfs/Maintenon-9.2.jpg)

IDA 9.2 is bringing a fresh set of tools for exploring cross-references (xrefs). These new views bring clarity to function relationships and data flows, making it easier to trace code paths and understand complex binaries.

Let’s take a closer look at the **Xref Graph** and **Xref Tree**.

# **Dynamic Xref Graph**

For years, IDA users relied on _qwingraph_ that allowed users to make graphs out of references between functions. While useful, it was not interactive, and the rendered graphs could quickly get confusing.

The new **Xref Graph** in IDA 9.2 evolves that concept, offering a more modern, integrated way to explore relationships between functions, code and data in an interactive manner. Though still early in development, it provides a strong foundation for richer visualization features in future versions.

The Xref Graph**** complements the Xref Tree, which we discuss in a bit, by showing relationships in a different perspective. It also helps users quickly understand dependencies and call hierarchies at a higher level.

**Simple controls**

  * Dragging nodes around moves them
  * Click and drag any point in the graph to pan around (Hold the Shift key to pan without unintentionally grabbing a node)
  * Hold Ctrl/CMD while scrolling to zoom in and out
  * Double-click on a node to jump to the corresponding item in an IDAView
  * Right-click on a node to add or remove from the graph (e.g. "Add xrefs from node")
  * Use the Space key to play or pause the layout mechanism 



**Current state and future improvements**

  * Still in its early stages and will continue to evolve in upcoming releases.
  * Planned to be improved and more tightly integrated with xrefs in general.
  * Intended to work side by side with the Xref Tree to provide both textual and graphical overviews



![](https://hex-rays.com/hs-fs/hubfs/image-png-Sep-05-2025-04-30-59-0617-PM.png?width=831&height=782&name=image-png-Sep-05-2025-04-30-59-0617-PM.png)

# **Tips & Tricks**

Xref Graph is still relatively nascent and we’re working on improving the default workflows.

Here is an example walkthrough of a workflow available today:

Finding code paths to a particular syscall

Let’s take _some program_ and try to find paths to one of the “edges” - the “read” syscall for example.

  * First we jump to the `_read` function; From there, we have a few options to generate an Xref Graph.
  * This approach invokes the plugin directly (“Edit>Plugins>Xref Graph”), this will create a graph with one level of reference depth to and from the root node.
  * From there, we can select code paths we’re interested in and expand their callers (references _to_) by using the “Add xrefs to node” action (`T` shortcut).



It’s also possible to delete unwanted nodes by using “Delete selected nodes” (`D` shortcut).

![](https://hex-rays.com/hs-fs/hubfs/Screenshot%202025-09-05%20at%2011-23-46-png.png?width=1964&height=1544&name=Screenshot%202025-09-05%20at%2011-23-46-png.png)

On smaller binaries, the “Xrefs graph to…” action, available in the right-click menu, would be a great candidate; it will gather callers recursively. This approach has caveats in 9.2: at the moment, Xref Graph’s layout algorithm performs slowly on sizable graphs and on a large enough binary, graphs will quickly reach thousands of nodes.

Techniques for dealing with large graphs:

  * Pause the layout while browsing (if the graph hasn’t reached an equilibrium)
  * To speed up the initial layout: open the Xref Graph options panel (right-click menu) and hide all Node types (“Filters”, “Nodes” checkboxes) while the layout runs. The graph will take shape faster while the rendering load is reduced.  
  




The search bar at the bottom can be used to swiftly select nodes that match a certain string. Once the nodes are selected, the node manipulation actions will be enabled on those. For example this can let one rapidly select and remove functions that match a certain library prefix.

# **Xref Tree**

Another new addition is the **Xref Tree** view. Instead of relying solely on graphs, this feature presents cross-references in a browsable, hierarchical structure. Users can quickly expand and collapse branches, making it easier to trace references without losing the broader context.

This feature consolidates and replaces the **Function Calls** and **Cross References** widgets.

**Highlighted Features**  


  * Shows references _to_ and _from_ the current function, much like call hierarchy views in IDEs (both code and data references are displayed).
  * The tree is non-modal, so there can be multiple instances open, each focused on a different function.
  * Tree nodes load lazily and update in real time as the user navigates.
  * Function and object renames are immediately reflected in the view.
  * When switching to the Xref Tree window, the last child widget you worked on will remain as the focus



**Customization and options**  


  * Synchronize with the current IDA View using the **“Sync”** checkbox.
  * Filter out unnecessary functions via the **“Add filter”** button, Ctrl+F (apply filter), or Ctrl+Shift+F (remove filter).
  * Toggle between simplified names (main(argc, argv) and full function names (int main(int argc, char **argv) .
  * Navigate easily with mouse and keyboard using the common cursor keys.
  * Double click/Enter to navigate the IDA View to the current item
  * Multiple xrefs to the same function are deduplicated by default. Remove this step by checking “**Allow Duplicates** ”.



![](https://hex-rays.com/hs-fs/hubfs/undefined.png?width=473&height=604&name=undefined.png)

# **Wrapping it Up**

Together, the Dynamic Xref Graph and Xref Tree give analysts both a **visual overview** and a **detailed textual map** of code and data references. So whether you gravitate towards a big-picture diagram or a precise, navigable tree, IDA 9.2 brings new tools to better understand the relationships hidden inside complex binaries.

Do you have suggestions on what you’d like to see in the next versions of Xref Graph and Xref tree?

Feel free to contact us at product@hex-rays.com or let us know on our [Discourse Forum](<https://community.hex-rays.com/>)!

---

## 2. 

**Date:** Posted: Aug 28, 2025  
**Link:** [https://hex-rays.com/blog/stop-guessing-and-start-going](https://hex-rays.com/blog/stop-guessing-and-start-going)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [Decompilation](<https://hex-rays.com/blog/tag/decompilation>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>) [Golang](<https://hex-rays.com/blog/tag/golang>)

# Stop Guessing and Start GOing: Cleaner Golang Decompilation in IDA 9.2

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/stop-guessing-and-start-going>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/stop-guessing-and-start-going>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/stop-guessing-and-start-going>)

Hex-Rays ✦ Posted: Aug 28, 2025

![Stop Guessing and Start GOing: Cleaner Golang Decompilation in IDA 9.2](https://hex-rays.com/hubfs/golang2.jpg)

[Go](<https://go.dev/>) has rapidly become a popular programming language for developing cloud software, command-line tools, and even malware. Yet for reverse engineers, Go binaries have always been a headache: unlike C or C++, the Golang compiler generates binary code following conventions that make decompilation harder. For example, the common Golang idiom of returning multiple values from a function previously decompiled into cryptic Pseudo-C with our [decompiler](<https://hex-rays.com/decompiler>). The good news? IDA 9.2 (coming soon) substantially improves on Go decompilation, saving analysts from the hassles of hand-reconstructing function prototypes and enabling them to focus on the real logic.

# Current state of Golang decompilation

If you’ve ever opened a Go binary in IDA and Hex-Rays, you’ve probably seen something like this: functions with undefined stack variables, mystery return values, and function calls with argument lists the size of a skyscraper. This makes it tough to follow data flow, especially when functions chain multiple returns or pass complex structures.

For example, the innocent Fscanf function from the fmt package at source level looks like this:

func Fscanf(r io.Reader, format string, a ...interface{}) (n int, err error) {

s, old := newScanState(r, false, false)

n, err = s.doScanf(format, a)

s.free(old)

return

}

If we were to compile this function and ask the 9.1 decompiler for its opinion, then we would see the following:

![golang_fscanf_9.1_cropped](https://hex-rays.com/hs-fs/hubfs/golang_fscanf_9.1_cropped.png?width=670&height=616&name=golang_fscanf_9.1_cropped.png)

Comparing this to the Golang source, we immediately spot several problems:

  * The function call argument lists of newScanState, doScanf, and free are _long_
  * Many local variables are marked in orange as potentially undefined. For example, it is not evident who initializes v9, v10, or v11, which are used to initialize v17, v18, and v19, respectively.
  * We have _no clue_ how the return value v16 (returned by doScanf) corresponds to the actual return values of fmt.Fscanf (int n, and error err).



These woes are a consequence of Go’s application binary interfaces (ABIs). If we hop back to IDA’s disassembly view, then this is what the epilogue of fmt.Fscanf looks like on x86-64:

; <snip>

.text:00000000004997CA call fmt__ptr_ss_free

.text:00000000004997CF mov rax, [rsp+90h+var_38]

.text:00000000004997D4 mov rbx, [rsp+90h+var_40]

.text:00000000004997D9 mov rcx, [rsp+90h+var_8]

.text:00000000004997E1 add rsp, 90h

.text:00000000004997E8 pop rbp

.text:00000000004997E9 retn

Starting from the bottom: the add rsp, 90h; pop rbp; retn sequence is a standard stack frame teardown. The interesting part is what happens right before it: all three of rax, rbx, and rcx registers are used to pass return values to the caller. This will be a surprise to those accustomed to C/C++ x86-64 calling conventions, where we would typically only expect the rax register to receive the (integral) return value of the function. The calling context confirms that these multiple return registers are indeed used.

; <snip>

.text:00000000004A7B86 call fmt_Fscanf

.text:00000000004A7B8B mov rax, rbx

.text:00000000004A7B8E mov rbx, rcx

; <snip>

In C terms, what is happening here is that the compiler is returning a struct by value and spreading its component fields across three separate registers. This apparently simple example only scratches the surface – Go binary aficionados will know that this is not the only way that return values are handled in Golang.

# Remedying Golang decompilation

IDA 9.2 remedies Go’s decompilation woes with the introduction of tuple types into IDA’s type system. In short, tuples are like structs, but with a few key differences:

  * tuple members can be scattered across registers and memory (stack) locations
  * Two tuples are considered to be equal if their field offsets, sizes, and types match field names and alignments are ignored



We use tuples to model Golang return values that are spread across multiple locations. Tuple types enable the decompiler to produce much more readable output: the same fmt.Fscanf function decompiled using IDA 9.2 looks as follows:

![golang_fscanf_9.2_cropped](https://hex-rays.com/hs-fs/hubfs/golang_fscanf_9.2_cropped.png?width=889&height=440&name=golang_fscanf_9.2_cropped.png)

This is much more readable! The following improvements are evident

  * No more orange-marked uninitialized stack variables.
  * The return value of newScanState is passed into doScanf and free.
  * Three members of the value of doScanf form the return value of fmt.Fscanf. On closer inspection, we would find _r0 to correspond to int n and _r1/_r2 to error err (see [https://pkg.go.dev/cmd/compile/internal/types#pkg-variables](<https://pkg.go.dev/cmd/compile/internal/types#pkg-variables>))



The retval_4996C0, retval_49E420, retval_499F60, retval_4996C0 types are defined as tuples (no automated recovery of member names for standard library functions (yet), sorry):  


00000000 __tuple retval_4996C0 // sizeof=0x18

00000000 {

00000000 _QWORD _r0;

00000008 _QWORD _r1;

00000010 _QWORD _r2;

00000018 };

00000000 __tuple retval_49E420 // sizeof=0x18

00000000 {

00000000 _QWORD _r0;

00000008 _QWORD _r1;

00000010 _QWORD _r2;

00000018 };

00000000 __tuple retval_499F60 // sizeof=0x38

00000000 {

00000000 _QWORD _r0;

00000008 _QWORD _r1;

00000010 _QWORD _r2;

00000018 _QWORD _r3;

00000020 _QWORD _r4;

00000028 _QWORD _r5;

00000030 _QWORD _r6;

00000038 };

00000000 __tuple retval_4996C0 // sizeof=0x18

00000000 {

00000000 _QWORD _r0;

00000008 _QWORD _r1;

00000010 _QWORD _r2;

00000018 };

# Taking the pain out of Flare-On 11, Challenge 2

Putting everything together, let's look at a [Golang binary](<https://services.google.com/fh/files/misc/flare-on11-challenge2-checksum.pdf>) from the 2024 edition of the annual Flare-On reversing competition.

In IDA 9.1, we see the following:

![flareon_checksum_9.1_cropped](https://hex-rays.com/hs-fs/hubfs/flareon_checksum_9.1_cropped.png?width=1297&height=974&name=flareon_checksum_9.1_cropped.png)  


The same location in IDA 9.2 (also notice the correctly recovered string lengths):

![flareon_checksum_9.2_cropped](https://hex-rays.com/hs-fs/hubfs/flareon_checksum_9.2_cropped.png?width=1034&height=984&name=flareon_checksum_9.2_cropped.png)

# Conclusion

Reverse engineering Go binaries has always been a bit like wrestling with an octopus: too many moving parts, and never quite where you expect them. With the IDA’s new tuple types and the latest Hex-Rays decompiler improvements, the task gets a whole lot easier. By better tracking Golang ABI parameters and return values, analysts can finally read decompiled Go functions in a way that feels natural and intuitive.

Whether you’re dissecting a new malware family, auditing Go-based tooling, or just curious about what’s hiding inside a binary, the new version of Hex-Rays helps you spend less time fixing prototypes and more time doing real reverse engineering.

Grab the latest IDA release and try it on your Go targets - you might be surprised how much smoother the workflow feels.

---

## 3. 

**Date:** Posted: Aug 22, 2025  
**Link:** [https://hex-rays.com/blog/ida-debugger-new-register-subview](https://hex-rays.com/blog/ida-debugger-new-register-subview)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [debugger](<https://hex-rays.com/blog/tag/debugger>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>)

# IDA Debugger: New Register Subview and More Accurate Call Stacks

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-debugger-new-register-subview>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-debugger-new-register-subview>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-debugger-new-register-subview>)

Hex-Rays ✦ Posted: Aug 22, 2025

![IDA Debugger: New Register Subview and More Accurate Call Stacks](https://hex-rays.com/hubfs/debugger%20blog%20image%209.2.png)

In this week’s blog post, we highlight two improvements to the IDA debugger for the upcoming 9.2 release: 

  * A redesigned **register subview** that provides a much richer context. 
  * A more **accurate call stack reconstruction mechanism** for certain x64 PE files. 



Since a picture is worth a thousand words, here is the IDA 9.1 debugger running** cmd.exe** on a Windows 10 machine:

![ida 9.1 debugger](https://hex-rays.com/hs-fs/hubfs/ida%209.1%20debugger.png?width=1156&height=730&name=ida%209.1%20debugger.png)

And here’s the new version, featuring the updated register subview in the bottom left and a more precise call stack reconstruction in the top right:

![ida 9.2 debugger](https://hex-rays.com/hs-fs/hubfs/ida%209.2%20debugger.png?width=1155&height=730&name=ida%209.2%20debugger.png)  


# New Register View

If you use IDA for dynamic analysis, you’re already familiar with the Register View, which displays the current state of CPU registers during debugging. 

In the new version, values that can be interpreted as pointers (i.e., addresses falling within a mapped range of the current address space) are **automatically dereferenced**. If the result is itself a pointer, IDA will recursively follow that chain. 

While walking the pointer chain, the widget applies color-coding to help you immediately recognize the memory type. For example, it’s instantly clear whether a value points to read-only memory, executable code, the stack, or the heap.

You can control the dereference depth through a new context menu entry, **“Dereference options”** , with the default limit set to 5. Setting it to 0 disables the feature entirely.

# More Robust Reconstruction of Call Stacks in x64 PE Binaries

Now, let’s look at a more challenging problem.

For example, one execution path leading to a call to RtlValidSid inside ntdll.dll looks approximately like this:

main 

↳ RegOpenKeyEx

↳ RegOpenKeyExInternal

↳ MapPredefinedHandleInternal

↳ (cfguard protected thunk function)

↳ RtlOpenCurrentUser

↳ RtlFormatCurrentUserKeyPath

↳ RtlLengthSidAsUnicodeString

↳ RtlValidSid

When you pause execution or hit a breakpoint, IDA attempts to reconstruct this information from CPU registers, values found on the architectural stack, and executable memory.

This process ranges from trivial (when debug information, standard function prologues, or frame pointer chains are present) to very complex (in cases of obfuscated code, dynamically-sized stack frames, non-standard calling conventions/stack organization). As a result, the reconstructed call stack is often only an approximation of the actual execution path.

In the first screenshot, the guessed call stack looks like this:

0x7FF7DE898ECD (within cmd.exe, no symbol)

↳ 0x7FF7DE89387C (within cmd.exe, no symbol)

↳ 0x7FF7DE89055B (within cmd.exe, no symbol)

↳ 0x7FF7DE89055B (within cmd.exe, no symbol)

↳ RtlOpenCurrentUser

↳ LdrGetProcedureAddressForCaller

↳ RtlLookupFunctionEntry

↳ RtlFormatCurrentUserKeyPath

↳ RtlLengthSidAsUnicodeString

↳ RtlValidSid

Here, IDA gets confused by the control transfer from MapPredefinedHandleInternal to RtlOpenCurrentUser: The topmost frames don’t make much sense, though the deeper frames closer to RtlValidSid were reconstructed correctly. (Stack reconstruction works in reverse, which explains this behavior.)

# Leveraging Exception Handling Metadata

Starting with IDA 9.2, we enhanced the stack reconstruction algorithm by incorporating **unwind information stored in**[**exception records**](<https://learn.microsoft.com/en-us/cpp/build/exception-handling-x64>).

These records contain metadata used by the runtime to resume execution after exceptions. A critical part of this process is **stack unwinding** , which resets the stack to the correct state when switching function contexts. The unwind metadata describes how to restore stack frames using a sequence of operations.

This is how the Windows operating system itself (such as the RtlVirtualUnwind API) and its official debugging tools perform stack unwinding, although our implementation does not rely on any Windows operating system functionality.

IDA now detects, parses, and applies this metadata to significantly improve call stack accuracy.

Looking at our example, this means that IDA will produce the following call stack:

Example: Improved Stack Trace

In our example, IDA 9.2 produces the following stack trace:

0x7FF7DE893875 (within cmd.exe, no symbol)

↳ RegOpenKeyEx

↳ RegOpenKeyExInternal

↳ MapPredefinedHandleInternal

↳ GetTempPath2

↳ RtlOpenCurrentUser

↳ RtlFormatCurrentUserKeyPath

↳ RtlLengthSidAsUnicodeString

↳ RtlValidSid

This result is **much closer to the actual execution path**. 

(Sharp-eyed readers may also notice another subtle improvement in the second screenshot: the displayed addresses now refer to the call instruction in the caller, rather than the instruction right behind the call that the callee would return to.)

With these improvements, IDA 9.2 makes dynamic analysis more reliable, giving you better insights into what’s really happening inside your binaries.

---

## 4. 

**Date:** Posted: Aug 18, 2025  
**Link:** [https://hex-rays.com/blog/introducing-the-ida-domain-api](https://hex-rays.com/blog/introducing-the-ida-domain-api)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [SDK](<https://hex-rays.com/blog/tag/sdk>) [Domain API](<https://hex-rays.com/blog/tag/domain-api>)

# Introducing the IDA Domain API

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-the-ida-domain-api>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-the-ida-domain-api>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-the-ida-domain-api>)

Geoffrey Czokow ✦ Posted: Aug 18, 2025

![Introducing the IDA Domain API](https://hex-rays.com/hubfs/domainapi.jpg)

Today, we’re excited to announce the [IDA Domain API](<https://github.com/HexRaysSA/ida-domain>), a new **open-source** Python API designed to make scripting in IDA simpler, more consistent, and more approachable. 

This is v0.1.0 - the first step in a longer journey. It’s not the finish line, but rather a foundation for ongoing collaboration between Hex-Rays and the reversing community.

The Domain API is available now for IDA 9.1 and will also work with the upcoming IDA 9.2 release.

# Why the Domain API?

If you’ve scripted in IDA before, you know the power of the IDA Python SDK, but you also know it can be verbose, and that common tasks sometimes require more boilerplate than you’d like.  
  


The “ _Domain_ ” in Domain API refers to the **domain of reverse engineering** \- the core problem space we work in. In this domain, concepts like functions, types, xrefs, and more are central. The Domain API makes these concepts first-class citizens, giving you a cleaner, more natural way to work with them.

The Domain API sits on top of the IDA Python SDK, offering a cleaner, domain-focused abstraction for frequent tasks. It’s designed to complement, not replace, the SDK. You can use both side by side, combining the simplicity of Domain API calls with the full flexibility of the SDK when needed.

# What Makes It Different

  * **Domain-focused design:** concepts like functions, types, xrefs, ... are first-class citizens
  * **Open source from day one:** anyone can read the code, suggest improvements, or contribute
  * **Versioned independently:** upgrade at your own pace and pin versions for stability
  * **Developer-centric:** built to reduce friction and improve the scripting experience



# What’s in v0.1.0

This first release is intentionally focused. You’ll find:

  * Simple APIs for navigating and querying functions, xrefs, and other core reversing concepts
  * Consistent naming and return types to reduce surprises
  * The ability to use the Domain API both inside IDA or outside, thanks to [idalib](<https://docs.hex-rays.com/user-guide/idalib>)
  * Full interoperability with the existing IDA Python SDK



# Getting Started

The Domain API is available on PyPI:
    
    
    pip install ida-domain

If idalib cannot automatically locate your IDA installation, set the `IDADIR` environment variable to point to it and you’re ready to go.

-> **[Read the Getting Started Guide](<https://ida-domain.docs.hex-rays.com/getting_started/>)** for installation details, code samples, and usage tips.

# What’s Next

We’re treating this as an **open-ended collaboration**. Over the next weeks, we’ll share:

  * **Use case spotlights** showing real-world workflows
  * **Community Q &A threads** to gather feedback and ideas
  * **Contribution guides** so you can help shape the API



This is the start of a faster, more transparent development cycle where community input directly shapes the tool.

# Join the Conversation

We want to hear from you:

  * Try it out in your scripts and plugins
  * Tell us what’s missing
  * Help us decide what to build next  
  




-> **[Join the discussion on the Hex-Rays forum](<https://community.hex-rays.com/t/about-the-domain-api-category/439>)**

-> **[Explore the Domain API on GitHub](<https://github.com/HexRaysSA/ida-domain>)**

-> **[Check out the reference documentation](<https://ida-domain.docs.hex-rays.com/faq/>)**[](<https://community.hex-rays.com/t/about-the-domain-api-category/439>)

# Watch An introduction to the IDA Domain API

Excited about Domain API? Watch I do a quick introduction about the new IDA Domain API from Hex-Rays.a release on the _All Things IDA_ channel.

Courtesy of Elias Bachaalany ([@allthingsida](<https://www.youtube.com/@allthingsida>))

---

## 5. 

**Date:** Posted: Aug 7, 2025  
**Link:** [https://hex-rays.com/blog/spying-on-decompiler-internals-the-hex-rays-microcode-viewer](https://hex-rays.com/blog/spying-on-decompiler-internals-the-hex-rays-microcode-viewer)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [beta](<https://hex-rays.com/blog/tag/beta>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>)

# Spying on Decompiler Internals: The Hex-Rays Microcode Viewer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/spying-on-decompiler-internals-the-hex-rays-microcode-viewer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/spying-on-decompiler-internals-the-hex-rays-microcode-viewer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/spying-on-decompiler-internals-the-hex-rays-microcode-viewer>)

Hex-Rays ✦ Posted: Aug 7, 2025

![Spying on Decompiler Internals: The Hex-Rays Microcode Viewer](https://hex-rays.com/hubfs/microcode-1.jpg)

# **What Is the Hex-Rays Microcode Viewer?**

If you’ve ever had to decompile tricky code that demanded writing a decompiler plugin, you probably know about the Hex-Rays [Microcode](<https://hex-rays.com/blog/microcode-in-pictures>)—an intermediate representation (IR) generated from CPU-specific assembler and refined through multiple _maturity levels_.

For each function, microcode (mba_t) is organized into blocks (mblock_t), each containing microinstructions (minsn_t) that operate on zero to three microoperands (mop_t). As microcode matures, instructions often get optimized away, while those that remain tend to grow more complex. Microoperands can themselves be the output of other microinstructions (sub-instructions), making it possible to nest microcode several levels deep.

# Why would anyone want to poke around in this?

Simple: the microcode layer is where you can write custom optimization rules (optinsn_t and optblock_t) to [catch compiler or obfuscator patterns](<https://hex-rays.com/blog/hex-rays-microcode-api-vs-obfuscating-compiler>) missed by the decompiler. These user-induced tweaks still go through the existing optimization passes, so you get all the benefits of the decompiler’s higher-level analyses.

The [Hex-Rays Decompiler SDK](<https://docs.hex-rays.com/developer-guide/c++-sdk>) lets you hook into the pipeline and intercept microcode generation, so plugins can reshape the hierarchy however they see fit. (Pro tip: always run verify() on your mba_t after you’re done messing with improving it. verify.cpp will translate those cryptic interr numbers—seriously, use it!)

# Community Contributions & What's New in IDA 9.2

Over the years, the community has built IDA plugins to visualize microcode at different stages, embedding subviews directly into IDA. (Shoutout to [Markus Gaasedelen,](<https://github.com/gaasedelen/lucid>) [patois](<https://github.com/patois/genmc>), and [Rolf Rolles](<https://github.com/RolfRolles/>)—we’ve been piggybacking on your work for far too long.) Now, with IDA 9.2 (coming soon), we’re introducing a built-in subview for exploring generated microcode at any maturity level. 

Note: This is just the first iteration of the microcode viewer; there’s more work to be done. We have plans to continue to add more key features in the near future.

![](https://lh7-qw.googleusercontent.com/docsz/AD_4nXdtLlTafrW9QyTWZGzbdaeoq5NBNzCXATz54iFLD9Nt5Ez6zW7aJT1qRYG0ZpVaCXyRpet4W0ZigT0GiVdXE9egTzMe23c2NdmzZhCakP_mfOCnHCRoAOxBgJ7riZDwdRVe6C5XkA?key=sIBjkaX6scMKvZ8RD4QYBA)

# Sync & Interact

This new microcode view syncs with disassembly, pseudocode, or other microcode views—at any stage in the decompilation process. In 9.2, it’s a straightforward tool for inspecting transformations and a first step toward our goal of making the decompiler more accessible and interactive. (Remember our mention of interactivity in the[ 2025 roadmap](<https://hex-rays.com/blog/2025-product-vision-and-goals>)? Here we are.)

# Looking Ahead

We’ve got plenty of exciting ideas for expanding this widget in future releases. Stay tuned—things are about to get interesting.

---

## 6. 

**Date:** Posted: Jul 29, 2025  
**Link:** [https://hex-rays.com/blog/open-sourcing-ida-sdk](https://hex-rays.com/blog/open-sourcing-ida-sdk)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [beta](<https://hex-rays.com/blog/tag/beta>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>)

# Open-Sourcing the IDA SDK: Simpler, Sharable, Scriptable

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/open-sourcing-ida-sdk>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/open-sourcing-ida-sdk>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/open-sourcing-ida-sdk>)

Hex-Rays ✦ Posted: Jul 29, 2025

![Open-Sourcing the IDA SDK: Simpler, Sharable, Scriptable](https://hex-rays.com/hubfs/SDK_Image.jpg)

IDA SDK and IDA Python became available with the 9.2 launch.

This change makes it easier for users to build and adapt the SDK to their specific needs, whether that’s integrating IDA into a custom workflow, extending its capabilities, or maintaining tooling across teams. By opening the SDK, we’re inviting the community to explore, contribute, and collaborate in a way that was previously more limited.

# **What’s New in IDA 9.2**

Starting with IDA 9.2, the SDK will be available in our GitHub repository. This includes the data that was previously shipped with IDA that is now conveniently accessible in one central place. All the necessary files to build the SDK on all supported platforms are still included.

We’ve also unified IDAPython and the C++ SDK into a single repository. Previously, IDAPython was hosted separately and required its own SDK download. Bringing them together simplifies setup and maintenance for developers working with both.

At launch, we won’t be accepting pull requests just yet, but the repository will continue to grow over time. Our long-term goal is to open development further and enable contributions from the community.

### **Why It Matters**

This update is a quality-of-life improvement for plugin authors, researchers, and companies that build their own tooling over IDA. It removes friction in CI/CD workflows, allows developers to script and extend IDA more easily, and supports reproducibility and collaboration across teams.

This marks a shift in how we think about development. The SDK is the bridge to our tool, and it’s essential that we get it right. By co-creating with our users—listening, iterating, and learning—we believe we can shape something genuinely meaningful and useful for the entire RE community.

### **Where You’ll Find It**

As mentioned, the SDK will be available in our GitHub repository when IDA 9.2 is released. It’s no longer tied to Pro-only distributions. Both IDA Pro and IDA Free users will be able to access it.

### **Looking Ahead**

This is just the beginning. Open-sourcing the IDA SDK marks the start of a longer-term effort to support a more open, collaborative development model. We’re laying the groundwork now and look forward to evolving it based on your feedback and collaboration.

### **Enrolling in our Beta Program just got easier**

We’re always looking for power users to help test and refine new and updated features for our next release. And now, enrolling in as a Beta User is as simple as clicking **Subscribe** in the [customer portal](<https://my.hex-rays.com/>). You’ll see a new prompt at the top of your Dashboard when you log in.

Accessing Beta Releases  


  * Once you’re subscribed, you will be able to access the beta release in the [Download Center](<https://my.hex-rays.com/login>). Moving forward, we’ll notify you by email when beta versions are ready for download. 
  * Your current active IDA license will match your Beta license:


  *     * IDA Home → IDA Home Beta
    * IDA Pro → IDA Pro Beta



Beta Testing Duration  


  * All beta testing closes on the **day of the product launch**.



Your feedback is invaluable  


  * Please report any bugs or suggestions (yes, suggestions!) on [support.hex-rays.com](<https://support.hex-rays.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-9tXBfPhqo4XldjL2Zr6nfjNaEU3oFBYjOrPcaaiBe8ngpFFVnrXyKF-MSBjGml1UahCGeM>) in the **Early access feedback** form or send an email to [support@hex-rays.com](<mailto:support@hex-rays.com>)—your insights make a real impact!

---

## 7. 

**Date:** Posted: Jul 25, 2025  
**Link:** [https://hex-rays.com/blog/ctf-sponsorship-program](https://hex-rays.com/blog/ctf-sponsorship-program)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [Giveaway](<https://hex-rays.com/blog/tag/giveaway>) [CTF](<https://hex-rays.com/blog/tag/ctf>)

# Introducing the Hex-Rays CTF Sponsorship Program

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ctf-sponsorship-program>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ctf-sponsorship-program>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ctf-sponsorship-program>)

Justine Benjamin ✦ Posted: Jul 25, 2025

![Introducing the Hex-Rays CTF Sponsorship Program](https://hex-rays.com/hubfs/CTF.png)

We’re excited to launch a new initiative supporting the global CTF community, from organizers building top-tier events to the teams solving challenges at the highest level.

The Hex-Rays CTF Sponsorship Program is designed to empower ethical hackers, amplify rising talent, and give back to the community that push reverse engineering forward. So whether you’re hosting a competition or competing on the world stage, we want to help.

# **Two Ways to Get Sponsored**

### **CTF Event Sponsorship**

We sponsor one (1) CTF event per quarter, providing support through:

  * Free IDA Pro licenses for winners and organizers (product-tier varies)
  * Swag for participants (depending on event size)



In return, we ask for logo placement and sponsor mentions in the CTF event promotions.

We accept event sponsorship applications year-round. Events must be established and submitted at least three months in advance. Full details can be found on our detailed webpage below.

### **Team Sponsorship**

We’re sponsoring four (4) CTF teams for a full year - three (3) Top Tier teams and one (1) Rising Star team.

Each selected team receives:

  * Free IDA Expert-6 licenses, for up to 5 team members
  * Monetary support for travel to one in-person event per sponsored year
  * Swag and gear for your squad
  * A platform to share your skills with the broader community



In return, we ask that teams contribute at least one (1) public write-up and create one (1) CTF challenge during their sponsorship year.

# **Learn More & Apply**

For more information, head on over to our official sponsorship page for full details, application deadlines, and other requirements: [https://hex-rays.com/ctf-sponsorship-program](</ctf-sponsorship-program>)

---

## 8. 

**Date:** Posted: Jul 24, 2025  
**Link:** [https://hex-rays.com/blog/tricore-ida-support](https://hex-rays.com/blog/tricore-ida-support)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [beta](<https://hex-rays.com/blog/tag/beta>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>)

# TriCore TC1.8 Support Lands in IDA: New Instructions and Updates

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/tricore-ida-support>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/tricore-ida-support>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/tricore-ida-support>)

Alexandru Petenchea ✦ Posted: Jul 24, 2025

![TriCore TC1.8 Support Lands in IDA: New Instructions and Updates](https://hex-rays.com/hubfs/TC1.8.png)

IDA 9.2 is right around the corner.

In this upcoming release, we're introducing full support for the TriCore TC1.8 instruction set used in 4.x chips. This upgrade is especially meaningful for users analyzing automotive and embedded firmware.

# **What’s New in IDA 9.2**

****- > **50+ new instructions** from the TC1.8 architecture are now fully supported in the disassembler. This includes:

  * Double-precision FPU instructions

  * Virtualization instructions

  * New Q (quad-sized) registers




![TriCore_Blog_Img_1](https://hex-rays.com/hs-fs/hubfs/TriCore_Blog_Img_1.png?width=700&height=332&name=TriCore_Blog_Img_1.png)

****- > **Chipset definitions have been added** for the following:

  * TC1765

  * TC1724

  * TC1728

  * TC1130

  * and more…




These are used across the automotive and railway industries, including real-world train firmware.

![TriCore_Blog_Img_2](https://hex-rays.com/hs-fs/hubfs/TriCore_Blog_Img_2.png?width=800&height=333&name=TriCore_Blog_Img_2.png)

****- > ****Global address registers (A0, A1, A8, A9) can now be configured as segment registers.

TriCore uses these registers for global address computation—typically via GP-relative access.

You can now set them via Edit > Segments > Change segment register value… or by pressing Alt+G. This helps IDA resolve memory references more accurately during analysis.

****  


![TriCore_Blog_Img_3](https://hex-rays.com/hs-fs/hubfs/TriCore_Blog_Img_3.gif?width=700&height=365&name=TriCore_Blog_Img_3.gif)

### 

### **Why It Matters**

TriCore is widely used in safety-critical systems that keep our daily lives running smoothly - from the ECUs in modern vehicles to control units in railway networks. These embedded systems play a vital role in passenger safety, traffic coordination, and system reliability.

With this update, analysts working in automotive or transportation sectors can better understand and audit the firmware that underpins our global transit infrastructure.

Expanding support for architectures like this has been in our sights as a priority for 2025.

### **Behind the Scenes**

This update was heavily shaped by user demand (many requests have been made). 

Implementing chipset definitions like TC1765 or TC1724 wasn’t always smooth sailing: documentation for these parts can be sparse, outdated, or contain subtle inconsistencies. To ensure reliable behavior, we had to cross-reference and validate multiple sources.

### **Where You’ll Find It**

Just start analyzing TriCore code. 

To edit segment registers, go to Edit -> Segments -> Change segment register value… or press Alt+G.

_TriCore is one of the processor modules that we continue to refine and extend in the near future, so be sure to stay tuned for more updates._

### **Enrolling in our Beta Program just got easier**

We’re always looking for power users to help test and refine new and updated features for our next release. And now, enrolling in as a Beta User is as simple as clicking **Subscribe** in the [customer portal](<https://my.hex-rays.com/>). You’ll see a new prompt at the top of your Dashboard when you log in.

Accessing Beta Releases  


  * Once you’re subscribed, you will be able to access the beta release in the [Download Center](<https://my.hex-rays.com/login>). Moving forward, we’ll notify you by email when beta versions are ready for download. 
  * Your current active IDA license will match your Beta license:


  *     * IDA Home → IDA Home Beta
    * IDA Pro → IDA Pro Beta



Beta Testing Duration  


  * All beta testing closes on the **day of the product launch**.



Your feedback is invaluable  


  * Please report any bugs or suggestions (yes, suggestions!) on [support.hex-rays.com](<https://support.hex-rays.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-9tXBfPhqo4XldjL2Zr6nfjNaEU3oFBYjOrPcaaiBe8ngpFFVnrXyKF-MSBjGml1UahCGeM>) in the **Early access feedback** form or send an email to [support@hex-rays.com](<mailto:support@hex-rays.com>)—your insights make a real impact!

---

## 9. 

**Date:** Posted: Jul 18, 2025  
**Link:** [https://hex-rays.com/blog/ida-9.2-beta-is-live](https://hex-rays.com/blog/ida-9.2-beta-is-live)

[Back](<https://hex-rays.com/blog>)

[beta program](<https://hex-rays.com/blog/tag/beta-program>) [Collaboration](<https://hex-rays.com/blog/tag/collaboration>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>)

# IDA 9.2 Beta is Live!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.2-beta-is-live>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.2-beta-is-live>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.2-beta-is-live>)

Hex-Rays ✦ Posted: Jul 18, 2025

![IDA 9.2 Beta is Live!](https://hex-rays.com/hubfs/9.2.jpg)

If you're enrolled in our Beta Program, you can grab it right now from the Download Center in your customer portal. As always, your involvement is crucial. Testing, feedback, and even small bug reports help us refine each release and ensure IDA meets the needs of power users like you.

  
Not in the program yet? Enrollment is open, and it only takes a click from your dashboard to get started.

### What's new in IDA 9.2 Beta

This release includes:

  * Modernized UI, now running on Qt6

  * Smarter cross-reference trees and graphs

  * Improvements to Golang analysis

  * Expanded support for ARM, MIPS, RISC-V, and more

  * New tools for navigating and debugging complex binaries




-> View the full feature list here: <https://docs.hex-rays.com/release-notes/9_2beta>

### Enrolling in the Beta Program is simple

  * Log in to your[ customer portal](<https://my.hex-rays.com/>).
  * Click **Subscribe** when prompted at the top of your dashboard.



![beta subscribe screenshot](https://hex-rays.com/hs-fs/hubfs/beta%20subscribe%20screenshot.png?width=1129&height=339&name=beta%20subscribe%20screenshot.png)

Once enrolled, you will be able to access the beta release in the **Download Center**. Moving forward, we’ll notify you by email when beta versions are ready for download. Note that beta licenses will automatically match your active IDA license (e.g., IDA Home → IDA Home Beta, IDA Pro → IDA Pro Beta).*

_*You must have an active license to beta test._

Get full program details plus the terms and conditions here: [https://hex-rays.com/beta-program](</beta-program>)

### Your feedback drives IDA forward

We rely on your feedback to shape the future of IDA. Please report bugs or suggestions using the[ Early Access Feedback Form](<https://support.hex-rays.com/>) or by emailing [support@hex-rays.com](<mailto:support@hex-rays.com>).

**- > IMPORTANT NOTE:** Beta access closes on the day of the final release, so don’t wait too long to dive in.

---

