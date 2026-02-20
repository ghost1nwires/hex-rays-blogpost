# Hex-Rays Blog – Page 12

**Archived:** 2026-02-20 12:13
**URL:** https://hex-rays.com/blog/page/12

---

## 1. 

**Date:** Posted: Jul 28, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-150-extract-function](https://hex-rays.com/blog/igors-tip-of-the-week-150-extract-function)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #150: Extract function

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-150-extract-function>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-150-extract-function>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-150-extract-function>)

I 

Igor Skochinsky ✦ Posted: Jul 28, 2023

![Igor’s Tip of the Week #150: Extract function](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

When you open a decompilable file in IDA, you get this somewhat mysterious item in the Help menu:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/extractfunc1-3.png?width=213&height=238&name=extractfunc1-3.png)

And if you invoke it, it shows an even more mysterious dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/extractfunc2-3.png?width=579&height=263&name=extractfunc2-3.png)

So, what is it and when it is useful?

Originally this feature was added to the decompiler to make decompiler bug reporting easier: oftentimes. a decompiler issue cannot really be reproduced or debugged without having the original database. However, in some cases sharing the whole database is impractical or impossible:

  * Whole database may be very large and difficult to share
  * parts of the database may contain private or confidential information
  * the rest of the database is not really relevant to the issue and only adds noise



This feature leaves just the current function plus maybe some potentially relevant information in the database. It can then be sent to support for investigation and fixing, either by email or directly from IDA via Help > Report a bug or an issue…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/extractfunc3-3.png?width=761&height=676&name=extractfunc3-3.png)

See also:

[Igor’s tip of the week #39: Export Data](<https://hex-rays.com/blog/igors-tip-of-the-week-39-export-data/>)

[Igor’s Tip of the Week #135: Exporting disassembly from IDA](<https://hex-rays.com/blog/igors-tip-of-the-week-135-exporting-disassembly-from-ida/>)

[](<https://www.hex-rays.com/products/decompiler/manual/failures.shtml#report>)

[Decompiler Manual: Failures and troubleshooting](<https://www.hex-rays.com/products/decompiler/manual/failures.shtml#report>)

---

## 2. 

**Date:** Posted: Jul 27, 2023  
**Link:** [https://hex-rays.com/blog/plugin-focus-comida](https://hex-rays.com/blog/plugin-focus-comida)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: ComIDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-comida>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-comida>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-comida>)

A 

Alex Petrov ✦ Posted: Jul 27, 2023

![Plugin focus: ComIDA](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

**This is a guest entry written by the Airbus CERT team. Their views and opinions are their own and not those of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the authors.**

The ComIDA plugin is focused on finding usage of [COM objects](<https://learn.microsoft.com/en-us/windows/win32/com/the-component-object-model>) inside Windows modules. When a COM object is identified, ComIDA will infer its type to improve the decompilation.

## What’s COM?

COM, for Component Object Model, is a powerful client-server model created by Microsoft to easily share features between Windows modules. It is based on a dedicated Hive where server components can register interfaces (objects with methods). Then the client will use the `CoCreateInstance`, `CoGetCallContext`, `QueryInterface` Windows API to retrieve a pointer to the server interface, to use it inside the client code.

It is massively used by many Windows core modules, and very often by malicious code to interact with Windows systems.

## ComIDA will list all COM objects

A server component is identified by a unique identifier (GUID). This GUID will be used by the client through the COM API (`CoCreateInstance`, `CoGetCallContext`, `QueryInterface`) to load the appropriate component. This GUID is known at compile time, which is processed as a direct operand value.

ComIDA will get all direct operand values, interpret it as GUID and interact with the HKEY_CLASSES_ROOT hive to check if it corresponds to a well-known COM GUID. If a match is found, it will be listed in the output view, with additional information like where the reference was found, as well as the name and the server module which registered this instance.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comida_image1-3.png?width=2387&height=790&name=comida_image1-3.png)

You can interactively double click on each match to go directly to the reference:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comida_image2-3.png?width=1117&height=845&name=comida_image2-3.png)

## ComIDA will infer your type

The other feature of ComIDA is, for owners of the Hex-Rays plugin, to automatically infer the type of the output variable from the `CoCreateInstance` and `CoGetCallContext` functions, or the `QueryInterface` method.

This method has a known signature. For example:
    
    
    HRESULT CoCreateInstance( [in] REFCLSID rclsid, [in] LPUNKNOWN pUnkOuter, [in] DWORD dwClsContext, [in] REFIID riid, [out] LPVOID *ppv );
    

The 4th argument is the COM GUID of the instance built into the 5th argument.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comida_image3-3.png?width=2365&height=1187&name=comida_image3-3.png)

The variable `ppv` has a `IShellLinkW` type, which we can guess from the `IShellLinkW` GUID in riid. This is exactly what ComIDA do: it will use the Hex-Rays decompiler hook API to automatically set the type of the `ppv` variable, and lets the Hex-Rays plugin do its magic:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/comida_image4-3.png?width=1986&height=1202&name=comida_image4-3.png)

In this example there are two inferred variables, one coming from CoCreateInstance, `ppv`, and the second coming from the QueryInterface method, `v5`.

The ComIDA IDA is available on GitHub: <https://github.com/airbus-cert/comida>

---

## 3. 

**Date:** Posted: Jul 21, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-149-using-symbolic-constants-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-149-using-symbolic-constants-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #149: Using symbolic constants in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-149-using-symbolic-constants-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-149-using-symbolic-constants-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-149-using-symbolic-constants-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Jul 21, 2023

![Igor’s Tip of the Week #149: Using symbolic constants in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We’ve covered the usage of symbolic constants (enums) [in the disassembly](<https://hex-rays.com/blog/igors-tip-of-the-week-99-enums/>). but they are also useful in the pseudocode view.

### Reusing constants from disassembly

If a number has been converted to a symbolic constant in the disassembly and it is present in unchanged form in pseudocode, the decompiler will use it in the output. For example, consider this call:
    
    
    .text:00405D72   push    1               ; nShowCmd
    .text:00405D74   cmovnb  eax, [esp+114h+lpParameters]
    .text:00405D79   push    0               ; lpDirectory
    .text:00405D7B   push    eax             ; lpParameters
    .text:00405D7C   push    offset File     ; "explorer.exe"
    .text:00405D81   push    0               ; lpOperation
    .text:00405D83   push    0               ; hwnd
    .text:00405D85   call    ShellExecuteW
    

Initially, it is decompiled like this:
    
    
    ShellExecuteW(0, 0, L"explorer.exe", v136, 0, 1);

However, we [can look up](<https://learn.microsoft.com/en-us/windows/win32/api/shellapi/nf-shellapi-shellexecutew>) that nShowCmd’s value 1 [corresponds to](<https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-showwindow>) the constant `SW_NORMAL`, and apply it to the disassembly:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums_dec1-3.png?width=986&height=221&name=enums_dec1-3.png)

![push    SW_NORMAL       ; nShowCmd         
cmovnb  eax, \[esp+114h+lpParameters\]       
push    0               ; lpDirectory      
push    eax             ; lpParameters     
push    offset File     ; "explorer.exe"   
push    0               ; lpOperation      
push    0               ; hwnd             
call    ShellExecuteW                      ](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums_dec2-3.png?width=302&height=125&name=enums_dec2-3.png)

After refreshing the pseudocode, the constant appears there as well:
    
    
    ShellExecuteW(0, 0, L"explorer.exe", v136, 0, SW_NORMAL);

### Applying constants in pseudocode

In fact, you can do the same directly in the pseudocode, using the context menu or the same shortcut (`M`):  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums_dec3-3.png?width=321&height=300&name=enums_dec3-3.png)

Note that there is no automatic propagation of the constants applied in pseudocode to disassembly. In fact, sometines it’s not possible to map a number you see in the pseudocode to the same number in the disassembly.

Consider this example from a Windows driver’s initialization routine (`DriverEntry`):

![      DriverObject->DriverStartIo = \(PDRIVER_STARTIO\)sub_1C0001840;
      DriverObject->DriverUnload = \(PDRIVER_UNLOAD\)sub_1C0001910;
      DriverObject->MajorFunction\[0\] = \(PDRIVER_DISPATCH\)&sub_1C0001510;
      DriverObject->MajorFunction\[2\] = \(PDRIVER_DISPATCH\)&sub_1C00011B0;
      DriverObject->MajorFunction\[14\] = \(PDRIVER_DISPATCH\)&sub_1C0001290;
      DriverObject->MajorFunction\[18\] = \(PDRIVER_DISPATCH\)&sub_1C0001070;
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums_dec4-3.png?width=464&height=143&name=enums_dec4-3.png)

[We know](<https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/driverentry-s-required-responsibilities>) that indexes into the MajorFunction array correspond to the major IRP codes (`IRP_MJ_xxx`), so we can convert numerical indexes to the corresponding constants:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums_dec5-3.png?width=460&height=231&name=enums_dec5-3.png)

and the pseudocode becomes:
    
    
    DriverObject->DriverStartIo = (PDRIVER_STARTIO)sub_1C0001840;
    DriverObject->DriverUnload = (PDRIVER_UNLOAD)sub_1C0001910;
    DriverObject->MajorFunction[IRP_MJ_CREATE] = (PDRIVER_DISPATCH)&sub_1C0001510;
    DriverObject->MajorFunction[IRP_MJ_CLOSE] = (PDRIVER_DISPATCH)&sub_1C00011B0;
    DriverObject->MajorFunction[IRP_MJ_DEVICE_CONTROL] = (PDRIVER_DISPATCH)&sub_1C0001290;
    DriverObject->MajorFunction[IRP_MJ_CLEANUP] = (PDRIVER_DISPATCH)&sub_1C0001070;

However, if we check the corresponding disassembly (e.g by using `Tab` or synchronizing pseudocode and IDA View), we can see that the array indexes are not present as such in the instruction operands:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/enums_dec6-3.png?width=1098&height=147&name=enums_dec6-3.png)

Another common situation where you can use symbolic constants in pseudocode but not disassembly is swich cases.

See also:

[Igor’s tip of the week #99: Enums](<https://hex-rays.com/blog/igors-tip-of-the-week-99-enums/>)

[Decompiler Manual: Hex-Rays interactive operation: Set Number Representation](<https://www.hex-rays.com/products/decompiler/manual/cmd_numform.shtml>)

---

## 4. 

**Date:** Posted: Jul 14, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-148-fixing-call-analysis-failed](https://hex-rays.com/blog/igors-tip-of-the-week-148-fixing-call-analysis-failed)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #148: Fixing “call analysis failed”

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-148-fixing-call-analysis-failed>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-148-fixing-call-analysis-failed>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-148-fixing-call-analysis-failed>)

I 

Igor Skochinsky ✦ Posted: Jul 14, 2023

![Igor’s Tip of the Week #148: Fixing “call analysis failed”](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

This error is not very common but may appear in some situations.

![Warning
804D5DD: call analysis failed
OK   
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/callanalysis1-3.png?width=530&height=195&name=callanalysis1-3.png)

Such errors happen when there is a function call in the code, but the decompiler fails to convert it to a high-level function call, e.g.:

  1. the target function’s prototype is wrong;
  2. the decompiler failed to figure out the function arguments: how many of them, or how exactly they’re being passed to the callee;
  3. the usage of the stack by the call does not make sense.



Let’s look at some examples where it happens and how to fix it.

### Wrong function info

The first action on seeing the error should be to inspect the address mentioned and the surrounding code. For example, here’s the snippet around the address in the first screenshot:
    
    
    .text:0804D5CD                 push    [ebp+var_10]
    .text:0804D5D0                 push    offset sub_804D6E8
    .text:0804D5D5                 push    [ebp+var_28]
    .text:0804D5D8                 push    offset sub_804CF24 ; oset
    .text:0804D5DD                 call    sub_8058FF0
    .text:0804D5E2                 mov     edx, [ebp+var_14]
    .text:0804D5E5                 or      dword ptr [edx+28h], 10h
    .text:0804D5E9                 mov     eax, [ebp+var_18]
    .text:0804D5EC                 add     esp, 10h
    .text:0804D5EF                 test    eax, eax
    .text:0804D5F1                 jz      loc_804D1D3
    .text:0804D5F7                 sub     esp, 0Ch
    .text:0804D5FA                 push    [ebp+var_18]
    .text:0804D5FD                 call    sub_8055A0C
    

At the first glance, there doesn’t seem to be anything unusual: four arguments are pushed on the stack before calling the function sub_8058FF0. However, if we go inside the function and try to decompile it, we get another error:

![Warning
8058FF0: function frame is wrong
OK   
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/callanalysis2-3.png?width=530&height=195&name=callanalysis2-3.png)

Also, the header of the function looks strange:
    
    
    .text:08058FF0 ; =============== S U B R O U T I N E =======================================
    .text:08058FF0
    .text:08058FF0 ; Attributes: bp-based frame
    .text:08058FF0
    .text:08058FF0 ; int __cdecl sub_8058FF0(sigset_t oset)
    .text:08058FF0 sub_8058FF0     proc near               ; CODE XREF: sub_804CF6C+671↑p
    .text:08058FF0                                         ; sub_804F798+126↑p ...
    .text:08058FF0
    .text:08058FF0 var_48          = dword ptr -48h
    .text:08058FF0 oset            = sigset_t ptr -38h
    

I.e. the function was detected not to take four arguments, but one structure by value. While this can indeed happen in some cases, the argument is in a wrong location: the local variables area (note the negative offset). 

Fixing the function itself is a topic for another post, but a quick fix for the original issue would be to delete the current prototype and let the decompiler fall back to guessing the arguments. For this, put the cursor on the function name or its first line, then press `Y` ([edit type](<https://hex-rays.com/blog/igors-tip-of-the-week-42-renaming-and-retyping-in-the-decompiler/>)), `Del`, `Enter`. This will clear the wrong prototype and decompilation should succeed, showing the four arguments we’ve seen in the disassembly:

![snippet of pseudocode with the call:
sub_8058FF0\(sub_804CF24, v23, sub_804D6E8, a1\);](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/callanalysis3-3.png?width=570&height=394&name=callanalysis3-3.png)

Sometimes the decompiler’s guessing of the prototype still fails, so try to specify one based on the actual arguments being passed to the call (look at the assembly around the call). In some cases this may require the [`__usercall` calling convention](<https://hex-rays.com/blog/igors-tip-of-the-week-51-custom-calling-conventions/>).

### Indirect calls

Instead of the direct function address, indirect calls use a register or a memory location which holds the destination address to perform the call. For example, on x86 it may look like one of the following:
    
    
    call eax 
    call dword ptr [edx+14h] 
    call [ebp+arg_0] 
    call g_myfuncptr

In rare cases, the decompiler may fail to detect the actual arguments being passed to the call, especially if optimizer interleaves arguments passed to different calls. In that case, you can give it a hint by adding a cross-reference to the actual function being called (if you know it), or a function of the matching type, for example using the [Set callee address](<https://hex-rays.com/blog/igors-tip-of-the-week-115-set-callee-address/>) feature. You should also check that the stack pointer is [properly balanced](<https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer/>) before and after each call for stack-using calling conventions.

See also:

[Igor’s tip of the week #27: Fixing the stack pointer](<https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer/>)

[Decompiler Manual: Failures and troubleshooting](<https://www.hex-rays.com/products/decompiler/manual/failures.shtml>)

---

## 5. 

**Date:** Posted: Jul 7, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-147-fixing-stack-frame-is-too-big](https://hex-rays.com/blog/igors-tip-of-the-week-147-fixing-stack-frame-is-too-big)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [decompiler](<https://hex-rays.com/blog/tag/decompiler>)

# Igor’s Tip of the Week #147: Fixing “stack frame is too big”

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-147-fixing-stack-frame-is-too-big>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-147-fixing-stack-frame-is-too-big>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-147-fixing-stack-frame-is-too-big>)

I 

Igor Skochinsky ✦ Posted: Jul 7, 2023

![Igor’s Tip of the Week #147: Fixing “stack frame is too big”](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

The Hex-Rays decompiler has been designed to decompile compiler-generated code, so while it can usually handle hand-written or unusual assembly, occasionally you may run into a failure, especially if the code has been modified to hinder decompilation. Here is one of such errors:  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stkframe1-3.png?width=530&height=195&name=stkframe1-3.png)

If you have a genuine function with a huge stack frame, you’ll probably have to give up and RE it the hard way – from the disassembly. However, in some situations it is possible to fix the code and get the function decompiled.

### Bogus stack variables

Stack variable with a large offset may be created by mistake (e.g. pressing `K` on an immediate operand), or induced deliberately (e.g. junk code referring to large stack offsets which are not used in reality). The fastest way to check for them is to look at the stack variable definitions at the start of the function and look for unusually large offsets:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stkframe2-3.png?width=582&height=652&name=stkframe2-3.png)

To fix, double-click the variable or press `Ctrl`–`K` to open the [stack frame editor](<https://hex-rays.com/blog/igors-tip-of-the-week-65-stack-frame-view/>), then undefine (`U`) the wrong stackvar(s).

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stkframe3-3.png?width=732&height=660&name=stkframe3-3.png)

Then you need to edit the [function properties](<https://hex-rays.com/blog/igors-tip-of-the-week-127-changing-function-bounds/>) (`Alt`–`P`) and reduce the local variables area to the actually used size (usually equival to the offset of the bottom-most actually used variable):

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stkframe4-3.png?width=708&height=459&name=stkframe4-3.png)

If you still get the error message after all that, the bogus variables may have been re-added during autoanalysis, so it may be necessary to [patch out](<https://hex-rays.com/blog/igors-tip-of-the-week-37-patching/>) or otherwise exclude from analysis the instructions which refer to them.

### Unusual stack pointer manipulation

This trick may cause IDA to decide that the stack pointer changes by a huge value, or not detect stack changes, causing it to grow the stack frame unnecessarily. This can be dealt with by [adjusting the stack pointer delta](<https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer/>) manually, or patching the instructions involved.

See also: 

[Igor’s tip of the week #27: Fixing the stack pointer](<https://hex-rays.com/blog/igors-tip-of-the-week-27-fixing-the-stack-pointer/>)

[Decompiler Manual: Failures and troubleshooting)](<https://www.hex-rays.com/products/decompiler/manual/failures.shtml>)

---

## 6. 

**Date:** Posted: Jun 30, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-146-graph-printing](https://hex-rays.com/blog/igors-tip-of-the-week-146-graph-printing)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #146: Graph printing

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-146-graph-printing>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-146-graph-printing>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-146-graph-printing>)

I 

Igor Skochinsky ✦ Posted: Jun 30, 2023

![Igor’s Tip of the Week #146: Graph printing](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

While exporting text disassembly is enough in many cases, many users nowadays prefer IDA’s [graph view](<https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view/>), and saving its representation may be necessary. What other options are there besides screenshots?

### WinGraph

WinGraph is an external program shipped with IDA which can display graphs. It was used to show function (and other) graphs before introduction of the built-in graph view in IDA 5.0 (2006). You can still use it via the View > Graphs menu. 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graphprint1-3.png?width=732&height=259&name=graphprint1-3.png)

For example, Flowchart action displays the graph of the current function.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graphprint2-3.png?width=1056&height=817&name=graphprint2-3.png)

Once the graph is displayed in WinGraph, you can print it using File > Print… or the first toolbar button. On most platforms this supports printing to PDF in addition to real printers.

### IDA graph view

If you prefer IDA’s graph layout, or have customized it to your liking (groups or custom layouts are ignored by WinGraph), you can also print it directly from IDA. For this, use the print buttion on the Graph View toolbar, or the context menu by right-clicking outside of any node.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graphprint3-3.png?width=345&height=674&name=graphprint3-3.png)

You will be asked about the page layout – this can be useful when printing large graphs

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/graphprint4-3.png?width=459&height=594&name=graphprint4-3.png)

See also:

[Igor’s tip of the week #23: Graph view](<https://hex-rays.com/blog/igors-tip-of-the-week-23-graph-view/>)

[Igor’s Tip of the Week #145: HTML export](<https://hex-rays.com/blog/igors-tip-of-the-week-145-html-export/>)

[Igor’s Tip of the Week #135: Exporting disassembly from IDA](<https://hex-rays.com/blog/igors-tip-of-the-week-135-exporting-disassembly-from-ida/>)

---

## 7. 

**Date:** Posted: Jun 26, 2023  
**Link:** [https://hex-rays.com/blog/rust-analysis-plugin-tech-preview](https://hex-rays.com/blog/rust-analysis-plugin-tech-preview)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [plugin](<https://hex-rays.com/blog/tag/plugin>) [Programming](<https://hex-rays.com/blog/tag/programming>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [rust](<https://hex-rays.com/blog/tag/rust>)

# Rust analysis plugin tech preview

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/rust-analysis-plugin-tech-preview>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/rust-analysis-plugin-tech-preview>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/rust-analysis-plugin-tech-preview>)

I 

Igor Skochinsky ✦ Posted: Jun 26, 2023

![Rust analysis plugin tech preview](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

The Rust language is gaining popularity and nowadays even malware authors started using it, which means our users need to analyze them in IDA. The binaries produced by the Rust compiler have some peculiarities which make them difficult to analyze, such as:

  * non-standard calling conventions
  * non-terminated string literals
  * unusual name mangling scheme



While tackling all of them is a huge undertaking, which is further complicated by regular changes to the language, it is possible to handle some of the low-hanging fruits with modest effort. We’ve tried to first address the problem of non-terminated string literals and release this work-in-progress for our users to test and provide feedback on its performance with “in the wild” binaries.

While the current code does not cover all situations, it can handle the most common code patterns. For example, here’s a fragment of the same binary before using the plugin:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rust_before-3.png?width=1370&height=865&name=rust_before-3.png)

and after:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/rust_after-3.png?width=1306&height=438&name=rust_after-3.png)

NB: the decompiled Rust code will probably still look ugly. Some changes will likely be needed in the decompiler or even IDA kernel to handle Rust’s code patterns properly.

Below are links to the current source code and builds of the plugin for IDA 8.3:

  * [Linux x64](<https://hex-rays.com/wp-content/uploads/2023/06/rust-plugin-linux.zip>)
  * [macOS arm64](<https://hex-rays.com/wp-content/uploads/2023/06/rust-plugin-mac-arm64.zip>)
  * [macOS x64](<https://hex-rays.com/wp-content/uploads/2023/06/rust-plugin-mac-x64.zip>)
  * [Windows x64](<https://hex-rays.com/wp-content/uploads/2023/06/rust-plugin-windows.zip>)
  * [Source code](<https://hex-rays.com/wp-content/uploads/2023/06/rust-plugin-src.zip>)

---

## 8. 

**Date:** Posted: Jun 23, 2023  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-145-html-export](https://hex-rays.com/blog/igors-tip-of-the-week-145-html-export)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #145: HTML export

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-145-html-export>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-145-html-export>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-145-html-export>)

I 

Igor Skochinsky ✦ Posted: Jun 23, 2023

![Igor’s Tip of the Week #145: HTML export](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

We’ve covered [exporting disassembly from IDA](<https://hex-rays.com/blog/igors-tip-of-the-week-135-exporting-disassembly-from-ida/>) before but it was in context of interoperability, when simple text is enough. If you want to preserve formatting and coloring of IDA View (e.g. for a web page or blog post), taking a screenshot is one option, but that has its downsides (e.g. no indexing for search engines). There is an alternative you can use instead.

### HTML export

To export a fragment of disassembly as HTML, [select](<https://hex-rays.com/blog/igor-tip-of-the-week-03-selection-in-ida/>) the desired address range in the listing and invoke File > Produce file > Create HTML file…

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/html1-3.png?width=1113&height=219&name=html1-3.png)

IDA will ask you for a filename and write the formatted text to it. The result will look like similar to the following:
    
    
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>IDA - riscv_lscolors64.elf </title>
    </head>
    <body class="c41">
    <span style="white-space: pre; font-family: Consolas,monospace;" class="c1 c41">
    <span class="c44">.text:0000000000005528 </span><span class="c5">addi </span><span class="c33">s4</span><span class="c9">, </span><span class="c33">sp</span><span class="c9">, </span><span class="c12">248h</span><span class="c9">+</span><span class="c25">var_1A0
    </span><span class="c44">.text:000000000000552C </span><span class="c5">mv </span><span class="c33">a0</span><span class="c9">, </span><span class="c33">s4
    </span><span class="c44">.text:000000000000552E </span><span class="c5">mv </span><span class="c33">a1</span><span class="c9">, </span><span class="c33">s0
    </span><span class="c44">.text:0000000000005530 </span><span class="c5">li </span><span class="c33">a2</span><span class="c9">, </span><span class="c12">0A0h
    </span><span class="c44">.text:0000000000005534 </span><span class="c5">call </span><span class="c37">memcpy
    </span>
    </span><style type="text/css">
    /* line-fg-default */
    .c1 { color: blue; }
    /* line-bg-default */
    .c41 { background-color: white; }
    /* line-pfx-func */
    .c44 { color: black; }
    /* line-fg-insn */
    .c5 { color: navy; }
    /* line-fg-register-name */
    .c33 { color: navy; }
    /* line-fg-punctuation */
    .c9 { color: navy; }
    /* line-fg-numlit-in-insn */
    .c12 { color: green; }
    /* line-fg-locvar */
    .c25 { color: green; }
    /* line-fg-code-name */
    .c37 { color: blue; }
    </style></body></html>

As you can see, the color tags are represented by CSS classes which can be adjusted if necessary. When opened in browser, the result should look pretty close to IDA View:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/html2-3.png?width=996&height=151&name=html2-3.png)

We use this feature on our web site to display disassembly snippets for the [processor gallery](<https://hex-rays.com/products/ida/processor-gallery/>).

### Pseudocode to HTML

HTML can be generated not only for disassembly but also for the decompiled pseudocode; for this use “Generate HTML…” from the context menu in the Pseudocode view.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/html3-3.png?width=367&height=307&name=html3-3.png)

See also:

[IDA Help: Create HTML File](<https://www.hex-rays.com/products/ida/support/idadoc/1504.shtml>)

[Hex-Rays interactive operation: Generate HTML file](<https://www.hex-rays.com/products/decompiler/manual/cmd_html.shtml>)

[Hack of the day #0: Somewhat-automating pseudocode HTML generation, with IDAPython.](<https://hex-rays.com/blog/hack-of-the-day-0-somewhat-automating-pseudocode-html-generation-with-idapython/>)

---

## 9. 

**Date:** Posted: Jun 22, 2023  
**Link:** [https://hex-rays.com/blog/hex-rays-sponsors-the-code-blue-2023-conference-in-tokyo](https://hex-rays.com/blog/hex-rays-sponsors-the-code-blue-2023-conference-in-tokyo)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Code Blue](<https://hex-rays.com/blog/tag/code-blue>) [Event](<https://hex-rays.com/blog/tag/event>)

# Hex-Rays Sponsors the Code Blue 2023 Conference in Tokyo

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-sponsors-the-code-blue-2023-conference-in-tokyo>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-sponsors-the-code-blue-2023-conference-in-tokyo>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-sponsors-the-code-blue-2023-conference-in-tokyo>)

A 

Alex Petrov ✦ Posted: Jun 22, 2023

![Hex-Rays Sponsors the Code Blue 2023 Conference in Tokyo](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Hex-Rays is thrilled to announce its sponsorship and participation at the highly anticipated [Code Blue conference](<https://codeblue.jp/2023/en/>) in Tokyo, Japan, on 8-9 November 2023. The attendance of the leading provider of advanced binary analysis tools confirms its commitment to fostering collaboration and knowledge sharing within the cybersecurity sector.

Code Blue, an internationally renowned gathering of information security specialists, researchers, and enthusiasts, sets the perfect stage for Hex-Rays to connect directly with its community. Attendees will have the exclusive opportunity to meet some of the Hex-Rays developers, engage in insightful conversations, and gain firsthand knowledge about the future development of their solutions.

---

