# Hex-Rays Blog – Page 7

**Archived:** 2026-02-20 12:11
**URL:** https://hex-rays.com/blog/page/7

---

## 1. 

**Date:** Posted: Mar 18, 2024  
**Link:** [https://hex-rays.com/blog/plugin-focus-ida-kmdf](https://hex-rays.com/blog/plugin-focus-ida-kmdf)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: ida kmdf

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-ida-kmdf>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-ida-kmdf>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-ida-kmdf>)

A 

Alex Petrov ✦ Posted: Mar 18, 2024

![Plugin focus: ida kmdf](https://hex-rays.com/hubfs/GI8d4Q8WgAA5Cv7.jpg)

_This is a guest entry written by Arnaud Gatignol and Julien Staszewski from the[THALIUM team](<https://blog.thalium.re/about/>). The views and opinions expressed in this blog post are solely those of the authors and do not necessarily reflect the views or opinions of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the authors._

# Helping KMDF driver analysis with an IDA plugin

Drivers are software components that serve as intermediaries between hardware devices and the operating system (OS) on a computer. They play a crucial role in the hardware/software interactions. There are also drivers that only serve software architecture purposes. In every case, they need to abstract interfaces to provide a generic way to communicate with the computer low-level layers.

On Windows, Microsoft has provided, over the time, multiple models and frameworks to implement drivers. The two most famous are:

  * Windows Driver Model (WDM)
  * Windows Driver Framework (WDF)



The introduction of WDF, and more specifically, KMDF (Kernel Mode Driver Framework) has added some complexity to the **reverse engineering** process.

Here comes the plugin we developed in order to facilitate the analysis of KMDF drivers within IDA by applying specific types and be as transparent as possible for the end user.

  * **Before** ![`Before applying types`](https://hex-rays.com/hubfs/Imported_Blog_Media/before_script-4.png)

  * **After** ![`After applying types`](https://hex-rays.com/hubfs/Imported_Blog_Media/after_script-4.png)




With our plugin installed, these changes occur **without a single click**.

In this blog post, we will introduce some concepts regarding KMDF and its impact on analysis. Then, we will introduce the plugin and what it can do to help you. Finally, some insights on the plugin implementation will be given.

**If you are already familiar with basic KMDF concepts, we do invite you to jump to theuse case section. In the other case and if you want to go deeper, the [Microsoft documentation](<https://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/kmdf-arch.doc>) might be helpful.**

## What's KMDF ?

The Kernel-Mode Driver Framework aims to simplify and streamline the process of developing kernel-mode device drivers for the Windows operating system. KMDF offers a higher-level abstraction and a set of libraries that help driver developers create reliable and efficient drivers with reduced complexity, allowing developers to focus on device-specific functionality rather than dealing with complex kernel and hardware intricacies.

KMDF introduces many new types and functions and makes the preliminary analysis steps completely different from a WDM driver analysis.

Going through the example of looking for the **MajorFunction table** , and more specifically for the **IOCTL dispatcher function** , we will see that KMDF-based drivers analysis can be slow at first.

## KMDF: Close Encounters of the Third Kind

The first steps when analyzing a Windows driver might include looking for what it exposes. The most known driver interface is certainly `IRP_MJ_DEVICE_CONTROL` which gives IOCTL support. This `MajorFunction` handler is registered in a table in the `DriverEntry` of the driver.

Two ways to identify the IOCTL dispatcher are:

  * Analyze the `DriverEntry` or the sub-function that registers the `MajorFunction`.
  * Dump dynamically the `nt!_DRIVER_OBJECT.MajorFunction` related to the given driver.



Obviously, sometimes looking at the debug symbol is enough.

For a WDM driver, the output of these two methods is given below.

![Common WDM driver entry](https://hex-rays.com/hubfs/Imported_Blog_Media/wdm_driver_entry-4.png)

The registration of the IOCTL dispatcher (`14`) is clearly and quickly identified (`srv2!Srv2DeviceControl`).

The dump of `nt!_DRIVER_OBJECT.MajorFunction` is also very straightforward:

![Common WDM driver object](https://hex-rays.com/hubfs/Imported_Blog_Media/wdm_driver_object-4.png)

**However, it is not this simple for KMDF drivers, and these two methods do not yield significant results.**

In fact, the driver entry does not expose the `MajorFunction` registration, as KMDF uses a different method to set these function pointers.

![Common KMDF driver entry](https://hex-rays.com/hubfs/Imported_Blog_Media/kmdf_driver_entry-4.png)

Indeed, as the KMDF types are not being used, the [`EvtIoDeviceControl` callback](<https://www.osr.com/nt-insider/2011-issue2/basics-wdf-queues/>) (which handles the I/O control requests in a KMDF driver) cannot be easily found. Moreover, the driver object is full of KMDF stubs (`Wdf01000!FxDevice::DispatchWithLock`) that prevent us from clearly identifying driver-specific code.

![Common KMDF driver object](https://hex-rays.com/hubfs/Imported_Blog_Media/kmdf_driver_object-4.png)

From this, it appears that we need to develop a tool that is be able to quickly reverse engineer a KMDF driver. This tool should aid in identifying standard KMDF structures and functions in order to achieve the same ease of reverse engineering as WDM drivers.

Therefore, the tool is expected to be able to apply and propagate KMDF types in a transparent way with TIL files. TIL stands for `Type Information Library`, it is a file format containing type descriptions that can be used in IDA. Hex-Rays has published two related [`Igor`](<https://hex-rays.com/blog/igors-tip-of-the-week-60-type-libraries/>) [`tips`](<https://hex-rays.com/blog/igors-tip-of-the-week-62-creating-custom-type-libraries/>) blog posts.

## Use case: apply KMDF types to an IDB

Before applying KMDF types, we need to add them to our IDA base. To add new types in an IDB, you need to import a type library (TIL files).

Thus, our tool has three different usages:

  * **TIL file generation** : generate new TIL files containing SDK/WDK/KMDF types
  * **TIL files application** : apply the new types to an already existing collection of IDB
  * **IDA plugin** : apply transparently your types to the IDB you've just opened



The two first usages are discussed below.

### TIL file generation

The generation of the TIL files has been made very simple:
    
    
    python wdfalyzer.py make_til \
        --wdf="C:\Users\admin\Desktop\Windows-Driver-Frameworks" \
        --wdk="C:\Program Files (x86)\Windows Kits\10\Include\10.0.22621.0\"
        --til="C:\Users\admin\AppData\Roaming\Hex-Rays\IDA Pro\til"
    

The `wdfalyzer` python script is the main interface to this tool:

  * `make_til`: command to generate a TIL file
  * `--wdf`: the path to the [Microsoft WDF](<https://github.com/microsoft/Windows-Driver-Frameworks>)
  * `--wdk`: the path to the WDK
  * `--til`: the path to your TIL folder



As an output you should have two TIL files (`x86` and `x86_64`) per WDF version:
    
    
    Created 32 bit til for version 1.15
    Created 64 bit til for version 1.15
    Created 32 bit til for version 1.17
    Created 64 bit til for version 1.17
    Created 32 bit til for version 1.19
    Created 64 bit til for version 1.19
    Created 32 bit til for version 1.21
    Created 64 bit til for version 1.21
    Created 32 bit til for version 1.23
    Created 64 bit til for version 1.23
    Created 32 bit til for version 1.25
    Created 64 bit til for version 1.25
    Created 32 bit til for version 1.27
    Created 64 bit til for version 1.27
    Created 32 bit til for version 1.31
    Created 64 bit til for version 1.31
    Created 32 bit til for version 1.33
    Created 64 bit til for version 1.33
    

### TIL files application

Now that the TIL files have been generated, let's apply them to the IDB:
    
    
    python wdfalyzer.py analyze \     "C:\Users\admin\Desktop\acpiex.sys.i64"
    

The output is straightforward:
    
    
    * idat64:       C:\Program Files\IDA Pro 8.3\idat64.exe
    * log:  C:\Users\admin\AppData\Local\Temp\acpiex_xwe83q0_.log
    * running:      "C:\Program Files\IDA Pro 8.3\idat64.exe" -A            
                        -L"C:\Users\admin\AppData\Local\Temp\acpiex_xwe83q0_.log" -S"\"C:\Users\admin\Desktop\ida_kmdf-main\wdf_plugin\wdf_analysis.py\" 
                            \"--quit\" \"--prefix\" \"cpncdp\"" 
                        "C:\Users\admin\Desktop\acpiex.sys.i64"
    * IDA script success
    

The types are applied, and we are now able to look for cross-references on `WdfFunctions->pfnWdfIoQueueCreate`, which is used to register the callbacks that handle, among others, I/O control.

Go straight to the `Structures view` and look for `WDFFUNCTIONS.pfnWdfIoQueueCreate`. The cross-references are automatically populated by the script.

![`WDFFUNCTIONS.pfnWdfIoQueueCreate`](https://hex-rays.com/hubfs/Imported_Blog_Media/kmdf_iohandler-4.png)

A `_WDF_IO_QUEUE_CONFIG` is passed to the `pfnWdfIoQueueCreate` as argument. A [`_WDF_IO_QUEUE_CONFIG`](<https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfio/ns-wdfio-_wdf_io_queue_config>) is a structure which contains the pointer to the I/O control dispatcher function.
    
    
    typedef struct _WDF_IO_QUEUE_CONFIG {
      ULONG                                       Size;
      WDF_IO_QUEUE_DISPATCH_TYPE                  DispatchType;
      WDF_TRI_STATE                               PowerManaged;
      BOOLEAN                                     AllowZeroLengthRequests;
      BOOLEAN                                     DefaultQueue;
      PFN_WDF_IO_QUEUE_IO_DEFAULT                 EvtIoDefault;
      PFN_WDF_IO_QUEUE_IO_READ                    EvtIoRead;
      PFN_WDF_IO_QUEUE_IO_WRITE                   EvtIoWrite;
      PFN_WDF_IO_QUEUE_IO_DEVICE_CONTROL          EvtIoDeviceControl; // the use case goal
      PFN_WDF_IO_QUEUE_IO_INTERNAL_DEVICE_CONTROL EvtIoInternalDeviceControl;
      PFN_WDF_IO_QUEUE_IO_STOP                    EvtIoStop;
      PFN_WDF_IO_QUEUE_IO_RESUME                  EvtIoResume;
      PFN_WDF_IO_QUEUE_IO_CANCELED_ON_QUEUE       EvtIoCanceledOnQueue;
      union {
        struct {
          ULONG NumberOfPresentedRequests;
        } Parallel;
      } Settings;
      WDFDRIVER                                   Driver;
    } WDF_IO_QUEUE_CONFIG, *PWDF_IO_QUEUE_CONFIG;
    

At first, the tool did not apply the type on this argument:

![`Before typing _WDF_IO_QUEUE_CONFIG`](https://hex-rays.com/hubfs/Imported_Blog_Media/queue_notype-4.png)

But if we manually apply the type, already present in the local types thanks to the tool, and add some naming:

![`After typing _WDF_IO_QUEUE_CONFIG`](https://hex-rays.com/hubfs/Imported_Blog_Media/queue_typed-4.png)

The IOCTL dispatcher `acpiex!EvtIoDeviceControl` has been identified almost automatically. The job is now done and the analysis can now begin!

NB: this is also possible with the `!wdfqueue` WinDbg command.

### KMDF IDA plugin

We also implemented a plugin in order to make the previous type application transparent for your use.

Once the plugin files have been copied to `%IDA_DIR%/plugins` and the TIL files to `%IDA_DIR%/til`, no further action is required. The plugin will be executed on IDA startup to apply the identified types.

## How does it work?

### TIL files creation from headers

In order to load new types in IDA, we need a type library (TIL file).

These can be created using the `tilib` utility. Tilib extracts types and structures from a C header file and gathers them in a type library for IDA.

Creating the TIL file may seem straightforward – locate the WDF headers and call `tilib` on them. However, this is not quite the case as this process can be more complex than it appears. The WDF headers are numerous and contain a lot of generated code, and the dependency graph of the various headers is quite complex.

This complexity makes it very hard for `tilib` (which is **not** a C compiler) to parse everything and generate the type library we want. Moreover, the WDF headers include a lot of types and definitions from the Windows SDK without including the proper headers, or from other WDF headers but either without including them or in a bad order.

To address this issue, we decided to hand-craft some headers and create a wrapper header including all files required to generate a complete enough WDF type library.

Even with all of that, some errors still remain, and the generated TIL files are not complete and do not contain every WDF types and structures, but we have the most important ones for reverse-engineering most of the drivers:

  * The `WDF_DRIVER_GLOBALS` structure, which contains a handle to the driver with some additional information (flags, tag, name…).
  * The `WDFFUNCTIONS` structure, which contains pointers to all WDF functions. Applying this type at the proper address is the core of our plugin.
  * The `WDF_BIND_INFO` structure, which contains the version of WDF used by the driver, and pointers to the two structures above.



### Type propagation

Once the TIL files are generated and placed in the `%IDA_DIR%/til` directory, we need to apply the three types mentioned above to our IDA base.

The first step is to know which version of WDF is used by the driver. To do so, we use two approaches.

First, we search for the imported function `WdfVersionBind`, which is called by the driver to bind itself to the kernel. Once we get the address of this function, we can apply the dummy base type `int WdfVersionBind(int, int, int, int);` to it and check all calls to it to get the major and minor versions passed as arguments, along with the address of the global WDF bind info structure.

If this approach fails, we search for the UTF-16 encoded string `"KmdfLibrary"`. This string is present in every WDF driver and is referenced in the bind info structure. By looking at cross-references to this string, we can identify the one in the bind info structure, and get the version by checking the next field of the structure, which contains the major and minor version, alongside the build number.

Once the version is known, we can load the proper TIL. Upon loading the TIL, IDA will automatically apply some types for us. To be sure everything is loaded properly, we apply the name `BindInfo` to the bind info structure, and apply its type with the Python bindings of IDA's API.

From the bind info structure, we can apply the two other types of interest: `WDF_DRIVER_GLOBALS` and `WDFFUNCTIONS`.

Once these types are applied, IDA's magic will propagate the `WDFFUNCTIONS` type, and all accesses to a function pointer will be translated to a human-readable form thanks to the data in the TIL.

### Cross-references

Having all WDF function calls in a human-readable form is a good thing, but having all the cross-references to the function table members would be even better. Luckily, with all the types already set, this is not too hard to obtain.

Cross-references to structure members are only tracked if the structure is applied in the disassembly, not in the decompiled code, which is why merely applying a type to its address is not sufficient.

To apply a structure offset to an operand (`t` in the GUI), we can use the `op_stroff` function from the API. This function requires the structure type to be imported. Local types won't be sufficient.

It is important to note that when loading a TIL file, its types are loaded in the local types group, the structures are not imported to the structure view. Hence, the first step to get our cross-references is to import the `WDFFUNCTIONS` structure via the API.

Once this is done, we can walk through all the cross-references to the WDF function table. These cross-references will follow the pattern `mov <reg>, <wdf_func_table_ea>`. For each of those, we check inside the function for the instruction pattern `mov <reg>, [<reg>+n]` with the second operand being the same register containing the function's table address.

This pattern matches a WDF function pointer being loaded into a register in order to be called. We simply apply the structure offset to the second operand with `op_stroff`, and the cross-references are now added to the structure member.

  * **Before**



![`before applying struct offset`](https://hex-rays.com/hubfs/Imported_Blog_Media/before_struct_offset-4.png)

  * **After**



![`after applying struct offset`](https://hex-rays.com/hubfs/Imported_Blog_Media/after_struct_offset-4.png)

After walking through all the references to the function table, we get all references to every WDF function in the structure view.

#### Limitations

The heuristic we use when applying the structure offset is very basic and has limitations. 

We are conservative. For each function cross-referencing the function table address, we stop after the first call to `op_stroff` in case the register changes: we do not want to mistype something.

This does not seem to be a problem since we have observed that IDA is clever enough to propagate all cross-references as long as the register does not change anyway.

Currently, we don't handle the following case correctly when the register containing the address of the function table changes before accessing the offset:
    
    
    mov rax, <wdf_func_table_ea>
    mov rbx, rax
    mov rbx, [rbx+<offset>]
    [...]
    call rbx
    

We would miss this cross-reference because we would be looking for the pattern `mov <reg>, [rax+<offset>]`.

In other words, our plugin does not propagate the types among the codebase, which could lead to missed cross-references in some edge cases. Fortunately, those edge cases seem to be rare enough that we did not encounter one during our tests.

Anyway, being able to propagate types among the code base would be great. Perhaps some other plugins, such as [Symless (also made by Thalium)](<https://github.com/thalium/symless>), could come in handy.

## Conclusion

Thank you for reading this blog post. We hope you enjoyed it and that it can be useful to you.

The tools are maintained on the [Thalium github repository](<https://github.com/thalium/ida_kmdf>). 

Besides the code, the pre-made TIL files are included in the repository, so feel free to use them if you don't want to rebuild them yourself.

Feel free to open PR! May the [green shard](<https://blog.thalium.re/>) be with you!

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/plugin-repo-Jun-18-2024-08-47-48-4432-AM.png?width=530&height=324&name=plugin-repo-Jun-18-2024-08-47-48-4432-AM.png)](<https://plugins.hex-rays.com/ida_kmdf>)

---

## 2. 

**Date:** Posted: Mar 13, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-178-field-representation-attributes](https://hex-rays.com/blog/igors-tip-of-the-week-178-field-representation-attributes)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [UI](<https://hex-rays.com/blog/tag/ui>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #178: Field representation attributes

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-178-field-representation-attributes>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-178-field-representation-attributes>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-178-field-representation-attributes>)

I 

Igor Skochinsky ✦ Posted: Mar 13, 2024

![Igor’s Tip of the Week #178: Field representation attributes](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In the past, we’ve seen how structure instance representation can be changed by [editing the structure](<https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation/>) in the Structures window. In IDA 8.4, a new unified view was introduced for Local Types and the same operations can (and should) be done in that window. Instead of comments, additional custom attributes are printed now:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fieldattrs1-4.png?width=620&height=314&name=fieldattrs1-4.png)

In addition to the shortcuts (`H`, `A` and so on) and “Field type” submenu actions, you can also use the C syntax editor to add or edit these attributes:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fieldattrs2-4.png?width=408&height=383&name=fieldattrs2-4.png)

Some may be obvious but what options are there? As of IDA 8.4, the following formatting attributes are supported:
    
    
      __bin         unsigned binary number
      __oct         unsigned octal number
      __hex         unsigned hexadecimal number
      __dec         signed decimal number
      __sbin        signed binary number
      __soct        signed octal number
      __shex        signed hexadecimal number
      __udec        unsigned decimal number
      __float       floating point
      __char        character
      __segm        segment name
      __enum()      enumeration member (symbolic constant)
      __off         offset expression (a simpler version of __offset)
      __offset()    offset expression
      __strlit()    string
      __stroff()    structure offset
      __custom()    custom data type and format
      __invsign     inverted sign
      __invbits     inverted bitwise
      __lzero       add leading zeroes
      __tabform()   tabular form (formatted array)
    

(see the link at the bottom of the post for details)

The new attributes offer possibilities that were not available before; for example, signed fields were not supported explicitly but either always positive or always negated values.

See also:

[Igor’s Tip of the Week #125: Structure field representation](<https://hex-rays.com/blog/igors-tip-of-the-week-125-structure-fields-representation/>)

[IDA Help: Set function/item type](<https://hex-rays.com//products/ida/support/idadoc/1361.shtml>)

---

## 3. 

**Date:** Posted: Mar 8, 2024  
**Link:** [https://hex-rays.com/blog/our-upcoming-ida-pro-starter-training-sessions-have-now-been-published](https://hex-rays.com/blog/our-upcoming-ida-pro-starter-training-sessions-have-now-been-published)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Our upcoming IDA Pro Starter Training sessions have now been published

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/our-upcoming-ida-pro-starter-training-sessions-have-now-been-published>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/our-upcoming-ida-pro-starter-training-sessions-have-now-been-published>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/our-upcoming-ida-pro-starter-training-sessions-have-now-been-published>)

A 

Alex Petrov ✦ Posted: Mar 8, 2024

![Our upcoming IDA Pro Starter Training sessions have now been published](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/starter-training-blog-post-4.png)

We heard you and have developed a cheaper and optimized online training program that provides fundamental knowledge about IDA Pro. Our new sessions take just one day of your time. Still, their content is carefully curated to provide you with all the necessary information to feel confident using IDA Pro and reverse engineer simple binaries containing compiler-generated code.

If you are familiar with the basics, please follow our website and social networks for more updates. Our Intermediate and Advanced training sessions will be published very soon.

[Read more](<https://hex-rays.com/training/>)

---

## 4. 

**Date:** Posted: Feb 28, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-177-unused-argument-attribute](https://hex-rays.com/blog/igors-tip-of-the-week-177-unused-argument-attribute)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #177: Unused argument attribute

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-177-unused-argument-attribute>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-177-unused-argument-attribute>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-177-unused-argument-attribute>)

I 

Igor Skochinsky ✦ Posted: Feb 28, 2024

![Igor’s Tip of the Week #177: Unused argument attribute](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In one of the [past tips](<https://hex-rays.com/blog/igors-tip-of-the-week-52-special-attributes/>) we mentioned the `__unused` attribute which can be applied to function arguments. When can it be useful? 

Let’s consider this code from Apple’s dyld:

![  v19 = \(dyld4::ProcessConfig::PathOverrides *\)_platform_strncmp\(__s, "DYLD_INSERT_LIBRARIES", 0x15uLL\);
  if \( !\(_DWORD\)v19 \)
  {
    result = \(size_t\)dyld4::ProcessConfig::PathOverrides::setString\(v19, a4, this + 12, v15\);
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/unusedarg1-4.png?width=1042&height=111&name=unusedarg1-4.png)

`v19` is passed as fist argument to `dyld4::ProcessConfig::PathOverrides::setString()`. Since its name looks like a class method, the decompiler assigned the class type to the first argument (normally corresponding to the implicit `this` argument). However, `strncmp` returns a simple integer with the comparison result and has no relation to the `PathOverrides` class. What’s going on?

To clarify things, it can be useful to look inside the function being called. It is pretty short so we can show the whole output:
    
    
    const char *__fastcall dyld4::ProcessConfig::PathOverrides::setString(
            dyld4::ProcessConfig::PathOverrides *this,
            lsl::Allocator *a2,
            const char **a3,
            const char *__s)
    {
      size_t v7; // x22
      size_t v8; // x8
      char *v9; // x22
      char *v10; // x0
      const char *result; // x0
      __int64 v12; // [xsp+0h] [xbp-40h] BYREF
    
      if ( *a3 )
      {
        v7 = _platform_strlen(*a3);
        v8 = (v7 + _platform_strlen(__s) + 17) & 0xFFFFFFFFFFFFFFF0LL;
        __chkstk_darwin();
        v9 = (char *)&v12 - v8;
        v10 = strcpy((char *)&v12 - v8, *a3);
        *(_WORD *)&v9[_platform_strlen(v10)] = 58;
        strcat(v9, __s);
        result = (const char *)lsl::Allocator::strdup(a2, v9);
      }
      else
      {
        result = (const char *)lsl::Allocator::strdup(a2, __s);
      }
      *a3 = result;
      return result;
    }

You may notice a curious thing: the `this` argument is **not used** in the body of the function. This can be confirmed by checking the cross-references (shortcut `X`):

![Empty list of "Local cross references to this"](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/unusedarg2-4.png?width=441&height=173&name=unusedarg2-4.png)

An additional confirmation is the assembly code preceding the call to the function:

![ADD             X2, X21, #0x60 ; '`' ; char **
MOV             X1, X22 ; lsl::Allocator *
MOV             X3, X23 ; __s
BL              dyld4::ProcessConfig::PathOverrides::setString\(lsl::Allocator &,char const*&,char const*\)
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/unusedarg3-4.png?width=1059&height=90&name=unusedarg3-4.png)

We can see `X1`, `X2` and `X3` being initialized with values for the three arguments, but `X0` (`this`) is not explicitly initialized, so the decompiler falls back to the last initialized value (result of the call to `__platform_strncmp()`), which is obviously unrelated. Can we make the decompilation nicer?

The solution is to mark the `this` argument as unused by editing either the full function prototype or just the argument’s type:

![\[Please enter the type declaration\]
__unused dyld4::ProcessConfig::PathOverrides *this
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/unusedarg4-4.png?width=694&height=99&name=unusedarg4-4.png)

After returning to the caller and refreshing, the output is much nicer-looking:

![  if \( !_platform_strncmp\(__s, "DYLD_INSERT_LIBRARIES", 0x15uLL\) \)
  {
    result = \(size_t\)dyld4::ProcessConfig::PathOverrides::setString\(UNUSED_ARG\(\), a4, this + 12, v15\);
    v23 = this\[12\];
](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/unusedarg5-4.png?width=1002&height=231&name=unusedarg5-4.png)

The decompiler inlined the `strncmp` call into the if condition because it no longer needs the separate `v19` variable. The bogus this argument got replaced by the dummy placeholder `UNUSED_ARG()`.

---

## 5. 

**Date:** Posted: Feb 26, 2024  
**Link:** [https://hex-rays.com/blog/ida-8-4-qt-5-15-2-sources-build-scripts](https://hex-rays.com/blog/ida-8-4-qt-5-15-2-sources-build-scripts)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.4: Qt 5.15.2 sources & build scripts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-4-qt-5-15-2-sources-build-scripts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-4-qt-5-15-2-sources-build-scripts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-4-qt-5-15-2-sources-build-scripts>)

Geoffrey Czokow ✦ Posted: Feb 26, 2024

![IDA 8.4: Qt 5.15.2 sources & build scripts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

A handful of our users have already requested information regarding the Qt 5.15.2 build, that is shipped with IDA 8.4.

The Qt sources used by IDA are:

  * based on Qt 5.15.2,
  * to which [the KDE Qt5 patch collection](<https://community.kde.org/Qt5PatchCollection>) has been added,
  * plus a few custom patches/fixes



### Rebuilding Qt from source

In order to obtain compatible libs, the simplest way forward is to

  * [download the full archive](<https://hex-rays.com/products/ida/support/freefiles/qt-5.15.2-full-IDA84.tar.bz2>) – it contains the original 5.15.2 source + KDE patches + Hex-Rays patches
  * go to the `build/` directory
  * type `python3 build.py -h`



Note: on Windows, that command should be issued from a Visual Studio 2019 command prompt.

---

## 6. 

**Date:** Posted: Feb 21, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-176-handling-stack-reuse-in-the-decompiler](https://hex-rays.com/blog/igors-tip-of-the-week-176-handling-stack-reuse-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #176: Handling stack reuse in the decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-176-handling-stack-reuse-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-176-handling-stack-reuse-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-176-handling-stack-reuse-in-the-decompiler>)

I 

Igor Skochinsky ✦ Posted: Feb 21, 2024

![Igor’s Tip of the Week #176: Handling stack reuse in the decompiler](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Previously, we [discussed a situation](<https://hex-rays.com/blog/igors-tip-of-the-week-155-splitting-stack-variables-in-the-decompiler/>) where the decompiler wrongly used a combined stack slot for two separate variables. We could solve it because each variable had a distinct stack location, so editing the stack frame to split them worked.

However, modern optimizing compilers can actually reuse the _same stack location_ for different variables active at different times (e.g. in different scopes). Consider this example:
    
    
    int __fastcall getval(char a1)
    {
      int v2; // [esp+0h] [ebp-4h] BYREF
    
      if ( a1 )
      {
        printf("Enter your age:");
        scanf("%d", &v2);
      }
      else
      {
        printf("Enter your height:");
        scanf("%d", &v2);
      }
      return v2;
    }

We can see that `v2` is used for two different values: age and height. Because their uses do not overlap, the compiler decided to use the same stack slot. In this short example we can rename `v2` to a generic `value` and call it a day. But imagine that the code inside the two branches is more complicated and it’s actually useful to know with which one we’re dealing with.

In that case, you can use the “Split variable” action (shortcult `Shift`–`S`) to introduce a new variable for the same stack location.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stkvar1-300x183-4.png?width=300&height=183&name=stkvar1-300x183-4.png)

If you do it for the first time, IDA will warn you that incorrect splitting may produce wrong decompilation.

![\[Information\]
The 'split variable' command allows you to create
a new variable at the specified location.
However, the decompiler does not perform any checks.
A wrong split variable will render the output incorrect.
Use this command cautiously!](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stkvar2-300x161-4.png?width=300&height=161&name=stkvar2-300x161-4.png)

After confirming, a new variable is created:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/stkvar3-300x194-4.png?width=300&height=194&name=stkvar3-300x194-4.png)

Note how it has the same stack location as `v2` but has an annotation **SPLIT**. Now that you have a different variable, you can rename or retype it as necessary.

In case you want to go back to the original situation, you can use “Unsplit variable”, or [reset split assignments](<https://hex-rays.com/blog/igors-tip-of-the-week-102-resetting-decompiler-information/>) for the current function.

Note: before IDA 8.4, the action was called “Force new variable”.

See also:

[Hex-Rays interactive operation: Split variable](<https://hex-rays.com/products/decompiler/manual/cmd_split_lvar.shtml>)

[Igor’s Tip of the Week #155: Splitting stack variables in the decompiler](<https://hex-rays.com/blog/igors-tip-of-the-week-155-splitting-stack-variables-in-the-decompiler/>)

---

## 7. 

**Date:** Posted: Feb 16, 2024  
**Link:** [https://hex-rays.com/blog/introducing-ida-8-4-key-features-and-enhancements](https://hex-rays.com/blog/introducing-ida-8-4-key-features-and-enhancements)

[Back](<https://hex-rays.com/blog>)

[IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Introducing IDA 8.4: Key Features and Enhancements

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-ida-8-4-key-features-and-enhancements>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-ida-8-4-key-features-and-enhancements>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-ida-8-4-key-features-and-enhancements>)

A 

Alex Petrov ✦ Posted: Feb 16, 2024

![Introducing IDA 8.4: Key Features and Enhancements](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

It is official! IDA 8.4 has now been released, and we are beyond excited to share the new features and improvements with you.

This new version combines enhanced support for a bunch of processors, Mach-O file improvements, some signature boosts, standard plugin updates, and a shiny new set of UI refinements that will make your analysis much more productive and convenient.

Here is a brief overview of what awaits you in IDA 8.4:

  * Unified type storage (ASMTIL)
  * Support for common Apple-specific instructions and system registers commonly encountered in iOS and macOS software
  * ARMv8.6-A support
  * ARMv8-M support
  * The Mach-O loader now offers fine-grained control over the selection of dyld shared cache modules and their dependencies
  * The ARM32 decompiler supports hard-float ABI
  * Debugger improvements
  * Fresher icons, crisper look when zoom-in, and a crosshair effect in the minigraph view 
  * and that’s not all…



See full updates here: <https://hex-rays.com/products/ida/news/8_4/>

### How to request the new versions

All new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively, you can [submit your ida.key](<https://www.hex-rays.com/updida/>), and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially shortly after the release. Please be patient.

If you have not received anything within an hour or so, send us a message to [support@hex-rays.com](<mailto:support@hex-rays.com>).

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

##

---

## 8. 

**Date:** Posted: Feb 14, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-175-idb-work-directory](https://hex-rays.com/blog/igors-tip-of-the-week-175-idb-work-directory)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #175: IDB work directory

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-175-idb-work-directory>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-175-idb-work-directory>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-175-idb-work-directory>)

I 

Igor Skochinsky ✦ Posted: Feb 14, 2024

![Igor’s Tip of the Week #175: IDB work directory](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

As we’ve seen [previously](<https://hex-rays.com/blog/igors-tip-of-the-week-174-ida-database-idbdetails/>), an IDB (IDA database) consists of several embedded files which contain the actual database data and which IDA reads/write directly when working with the database. By default, they’re unpacked next to the IDB, which can lead to various issues such as excessive disk usage, or speed (e.g. if IDB is on a remote or removable drive).

If you often work with external IDBs but have a fast local drive, it may be useful to set up a _work directory_

The setting is mentioned in `ida.cfg`:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/workdir1-3.png?width=659&height=261&name=workdir1-3.png)

Instead of editing `ida.cfg` directly, the recommended option is to put the changed variables into a file `idauser.cfg` in the [user directory](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>). This way you won’t need to redo the edits when upgrading IDA. You can also put there other settings, e.g. [skippable instructions](<https://hex-rays.com/blog/igors-tip-of-the-week-68-skippable-instructions/>) colors.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/workdir2-3.png?width=413&height=218&name=workdir2-3.png)

Once the config option is added, embedded files from newly opened IDBs will be unpacked into the specified directory instead of next to the IDB. This can be especially useful for cases like IDBs on remote or removable drives: even if the drive is disconnected, local files remain and IDA will continue working.

See also:

[Igor’s Tip of the Week #174: IDA database (IDB) details](<https://hex-rays.com/blog/igors-tip-of-the-week-174-ida-database-idbdetails/>)

[Igor’s tip of the week #33: IDA’s user directory (IDAUSR)](<https://hex-rays.com/blog/igors-tip-of-the-week-33-idas-user-directory-idausr/>)

[IDA Help: Configuration files](<https://hex-rays.com//products/ida/support/idadoc/419.shtml>)

---

## 9. 

**Date:** Posted: Feb 13, 2024  
**Link:** [https://hex-rays.com/blog/plugin-focus-frinet](https://hex-rays.com/blog/plugin-focus-frinet)

[Back](<https://hex-rays.com/blog>)

[plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Plugin focus: Frinet

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/plugin-focus-frinet>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/plugin-focus-frinet>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/plugin-focus-frinet>)

A 

Alex Petrov ✦ Posted: Feb 13, 2024

![Plugin focus: Frinet](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

_This is a guest entry written by Martin Perrier and Louis Jacotot from[Synacktiv](<https://www.synacktiv.com/>). The views and opinions expressed in this blog post are solely those of the authors and do not necessarily reflect the views or opinions of Hex-Rays. Any technical or maintenance issues regarding the code herein should be directed to the authors._

Frinet is a project that aims at facilitating reverse-engineering, vulnerability research, and root-cause analysis with an execution trace. It combines a Frida-based tracer that supports iOS, Android, Linux, Windows, and most related architectures with an enhanced version of the Tenet trace explorer. This IDA plugin by _Markus Gaasedelen_ won [first place in _Hex-Rays' 2021 Plug-In Contest_](<https://hex-rays.com/contests_details/contest2021/#Tenet>). The improvements we introduced on Tenet are based on our needs while using the plugin on real case studies.

The following image shows a screenshot of IDA Pro after importing a trace generated on the _curl_ utility with this command line:

`python3 trace.py spawn /tmp/curl-7.88.1/bin/curl curl 0x256d9 -a "curl,-k,https://127.0.0.1:5000/test`

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/img_screenshot-3.png?width=1474&height=980&name=img_screenshot-3.png)

# Tenet trace explorer

Upon loading a trace, Tenet opens several subviews:

  * The _Trace Timeline_ , displaying the location of the current instruction in the trace
  * The _CPU Registers_ view, displaying the live values of the registers
  * The _Memory/Stack View_ , useful for inspecting memory and placing _Memory Breakpoints_



The currently inspected instruction is also highlighted in the assembly view. All those features are described and illustrated in more detail in the [blog post of Tenet's original author](<https://blog.ret2.io/2021/04/20/tenet-trace-explorer/>).

# Improving Tenet

Minor improvements have been made to Tenet, such as bug fixes, adding several memory views, and having the possibility to embed the ASLR slide directly in the trace file. However, the most important new features are the _Tree View_ and _Memory Search_.

## Tree view

This new subview opens when a trace is loaded. It shows the equivalent of a call stack trace, but spans the whole trace. This makes the trace easier to explore and navigate, as every function can be clicked to jump to the corresponding index. It is also possible to expand/collapse each function call and filter function names using regular expressions.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/img_treeview-3.png?width=628&height=807&name=img_treeview-3.png)

## Memory Search

This feature is available through the right-click context menu of the _Memory View_. It is possible to select bytes in the _Memory View_ and search for them in the trace or type the search pattern directly in a text box. Search results are then presented to the user in a new window and can be clicked, making a direct jump to the occurrence that was found. It does not simply search the memory at a specific point in time but at any position in the trace. This means, for example, that looking for the parsing of a specific byte sequence in a trace is now very easy, even if this sequence was read byte by byte using different instructions at some unknown point in the trace.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/img_memsearch-3.png?width=364&height=271&name=img_memsearch-3.png)

# Conclusion

Frinet is our attempt at facilitating reverse-engineering of large programs based on a dynamic execution trace. The new features are still experimental, but we welcome feedback from the community to improve the plugin. Furthermore, we plan on refactoring the trace backend with a native implementation for performance and introducing new features such as searching for register values, supporting execution traces with multiple threads and adding user xrefs to indirect calls.

The Frinet project is released on GitHub: <https://github.com/synacktiv/frinet>. The modified IDA Tenet plugin is available in the `tenet/` subrepository.

---

