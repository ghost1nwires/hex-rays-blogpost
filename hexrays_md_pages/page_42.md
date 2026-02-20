# Hex-Rays Blog – Page 42

**Archived:** 2026-02-20 12:23
**URL:** https://hex-rays.com/blog/page/42

---

## 1. 

**Date:** Posted: Oct 20, 2011  
**Link:** [https://hex-rays.com/blog/code-viewer-forms-timers](https://hex-rays.com/blog/code-viewer-forms-timers)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Code viewer, forms & timers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/code-viewer-forms-timers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/code-viewer-forms-timers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/code-viewer-forms-timers>)

Hex-Rays ✦ Posted: Oct 20, 2011

![Code viewer, forms & timers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

In this post I’ll present some new things in IDA 6.2. There’s a new control, the code viewer, some additions to forms and the introduction of timers to discuss. All these new features have been exposed to the SDK, so that our users can benefit from them too. 😉  
![code viewer](https://hex-rays.com/hubfs/Imported_Blog_Media/codeviewer-3.gif)  


## The code viewer

The first new thing I’m going to talk about is the code viewer. This control is just a container for a generic custom viewer. In fact, I just took the custom viewer sample plugin and added some lines to add the code viewer:
    
    
      // create a custom viewer
      si->cv = create_custom_viewer("", NULL, &s1, &s2, &s1, 0, &si->sv);
      // create a code viewer container for the custom viewer
      si->codeview = create_code_viewer(form, si->cv, CDVF_LINEICONS);
      // set handlers for the code viewer
      set_code_viewer_line_handlers(si->codeview, lines_click, NULL, NULL, lines_icon, lines_linenum);
      // draw maximal 2 icons in the lines widget
      set_code_viewer_lines_icon_margin(si->codeview, 2);
    

After having created the custom viewer with **create_custom_viewer** , it is possible to create a code viewer by passing the custom viewer as argument to **create_code_viewer**.  
The code viewer basically features for the moment a widget left to the embedded custom viewer called ‘the line widget’. This control can show the current line number, either automatically or by providing it ourselves. It can also show one or more icons and it informs us about user interaction such as mouse clicks.  
As you can see, **create_code_viewer** was called specifying the CDVF_LINEICONS flag, which instructs the code viewer that we want icons to be drawn inside the line widget.  
Available flags are:
    
    
    #define CDVF_NOLINES        0x0001    // don't show line numbers
    #define CDVF_LINEICONS      0x0002    // icons can be drawn over the line control
    #define CDVF_STATUSBAR      0x0004    // keep the status bar in the custom viewer
    

However, specifying the flag to show the icons is not enough. We also need to specify how the maximum number of icons we want to display on a single line:
    
    
      // draw maximal 2 icons in the lines widget
      set_code_viewer_lines_icon_margin(si->codeview, 2);

Then there’s the callback we have set by calling **set_code_viewer_line_handlers** :
    
    
    static int idaapi lines_icon(
            TCustomControl * /*cv*/,
            const place_t *p,
            int *x,
            void * /*ud*/)
    {
      bool b = p->touval(NULL) == 6 && *x == 0;
      // setting the highest bit signals that there's another icon
      // to draw on the current line
      int icon_id = *x == 0 ? 51 : 177;
      return b ? 0x80000000 | icon_id : icon_id;
    }

This callback needs some explanation. The line widget calls this callback for every line. The argument **x** represents the position of the icon to draw. Thus, for every line the callback is called the first time with * **x** == 0. The return value of the callback is the id of the icon to display. If you want to display another icon for the current line, then set the highest bit in the return value. If you don’t want to display an icon at all for the current line, simply return -1.  
You might wonder why **x** is a pointer. Imagine you don’t want to display an icon at position 0, but skip that position and display an icon at the next one. This can be done without calling the callback again, since we can set the value of **x**. Remember that you can [load custom icons](<http://www.hexblog.com/?p=265> "custom icons") and use them in the code viewer.  
Apart from the usual mouse and context menu callbacks, there’s a handy callback to be notified when the user clicks in the space reserved for icons:
    
    
    static void idaapi lines_click(
            TCustomControl * /*cv*/,
            const place_t *p,
            int pos,
            int /*shift*/,
            void * /*ud*/)
    {
      if ( p != NULL )
        msg("Line click at position: %d\n", pos);
      else
        msg("Click occurred outside of a line\n");
    }

This is useful to set/unset an icon for instance.  
I’ve also created another sample plugin with the code viewer featuring a hex view.  
![hex view](https://hex-rays.com/hubfs/Imported_Blog_Media/hexview-3.gif)  
This can give you some more ideas of what this control might good for. Most of the logic behind this sample stands in the custom view and not in the code view, which, in this case, handles everything automatically without the use of callbacks:
    
    
      // create a code viewer container for the custom view
      si->hexview = create_code_viewer(form, si->cv);
      // set the radix and alignment for the offsets
      set_code_viewer_lines_radix(si->hexview, 16);
      set_code_viewer_lines_alignment(si->hexview, si->data.size() > 0xFFFFFFFF ? 16 : 8);

## New forms controls

Forms were certainly lacking two very important controls: a generic combo box and a multi-line edit. The combo box comes in two variants: editable and read-only. This is how to declare a combo:
    
    
    <title:b:is_editable:width::>

When **is_editable** is omitted or zero, then the combo is read-only, otherwise it’s editable. Every combo takes two arguments. The first argument is a qstrvec_t * to populate the combo with items, while the second one is either an int * or a qstring * to specify the current selection. If the combo is read-only, then the second argument is an int * specifying an index into the qstrvec_t list, otherwise it’s a qstring * with the current text of the combo. Getting and setting the value of a combo follows the same int/qstring rule.  
A multi-line edit is declared in this way:
    
    
    <title:t::width::>

This control is more complex than the combo and requires a structure as argument:
    
    
    // Multi line text control: used to embed a text control in a form
    struct textctrl_info_t
    {
       size_t  cb;                 // size of this structure
       qstring text;               // in, out: text control value
       uint16  flags;
    #define TXTF_AUTOINDENT 0x0001 // auto-indent on new line
    #define TXTF_ACCEPTTABS 0x0002 // Tab key inserts 'tabsize' spaces
    #define TXTF_READONLY   0x0004 // text cannot be edited (but can be selected and copied)
    #define TXTF_SELECTED   0x0008 // shows the field with its text selected
    #define TXTF_MODIFIED   0x0010 // gets/sets the modified status
    #define TXTF_FIXEDFONT  0x0020 // the control uses IDA's fixed font
       uint16  tabsize;            // how many spaces a single tab will indent
       textctrl_info_t(): cb(sizeof(textctrl_info_t)), flags(0), tabsize(0) { }
    };

Most of the fields are self-evident, but two flags require an explanation. **TXTF_ACCEPTTABS** tells the control that the user can use tabs and they will be converted to the number of spaces specified by **tabsize**. **TXTF_AUTOINDENT** enables auto-indentation, not only when return is pressed, but also by removing tab block spaces on backspace. This feature is very useful when you want code to be entered for instance. In fact, the new script dialog in IDA 6.2 was created by using it.  
![script dialog](https://hex-rays.com/hubfs/Imported_Blog_Media/scriptdlg-3.gif)  
Don’t forget the **TXTF_FIXEDFONT** flag to use a fixed font. Here’s a small sample demonstrating both the combo and edit:
    
    
    //--------------------------------------------------------------------------
    static int idaapi modcb(int fid, form_actions_t &fa)
    {
      switch ( fid )
      {
        case 10:
          {
            msg("The selection of the combo has changed!\n");
            // set the text of edit to the text of the current combo item
            int sel;
            if ( fa.get_field_value(10, &sel) )
            {
              qstrvec_t *list = (qstrvec_t *)fa.get_ud();
              textctrl_info_t ti;
              ti.flags = TXTF_SELECTED;
              ti.text = list->at(sel);
              fa.set_field_value(11, &ti);
            }
          }
          break;
        case 11:
          msg("The multi-line edit text has changed!\n");
          break;
      }
      return 1;
    }
    //--------------------------------------------------------------------------
    static void idaapi run(int)
    {
      static const char form[] =
        "Combo - Multi-line Edit sample\n"
        "%/%*"
        " 
     
      \n"
        " 
      
       \n" "\n"; qstrvec_t list; static const char *items[] = { "first", "second", "third", "fourth" }; for ( int i = 0; i < qnumber(items); i++ ) list.push_back(items[i]); int sel = 1; textctrl_info_t ti; ti.flags = TXTF_SELECTED; ti.text = "Some text"; if ( AskUsingForm_c(form, modcb, &list, &list, &sel, &ti) > 0 ) { msg("Combo selection: %s\n", list[sel]); msg("Text: %s\n", ti.text.c_str()); } }
      
     

In this small sample you can see another small addition to the forms syntax: following the usual **%/** to specify the callback of the form there’s a **%***. This new sequence makes it possible to specify a user data pointer (&list), which can later on be retrieved from the callback like this:
    
    
    qstrvec_t *list = (qstrvec_t *)fa.get_ud();

Both the combo box and the multi-line edit can be used in the text version of IDA as well.

## Timers

Timers are very easy to use and are available in the text version of IDA as well. These are the prototypes of the functions:
    
    
    qtimer_t register_timer(int interval, int (idaapi *callback)(void *ud), void *ud);
    bool unregister_timer(qtimer_t t)

Timer functions are thread-safe and the callback is executed in the context of the main thread. The callback can return -1 to unregister the timer, while any other return greater than or equal to 0 defines the new interval of the timer.
    
    
    // the timer event message will be displayed 5 times
    int idaapi timer_callback(void *)
    {
      static int i = 0;
      msg("timer event\n");
      return ++i == 5 ? -1 : 1000;
    }
    // register the timer with a 1-second interval
    register_timer(1000, timer_callback, NULL);

That’s all! You can download both code viewer plugin samples from [here](<https://www.hex-rays.com/wp-content/uploads/2011/10/codeviewer_samples.zip> "code viewer samples").

---

## 2. 

**Date:** Posted: Oct 10, 2011  
**Link:** [https://hex-rays.com/blog/new-features-in-hex-rays-decompiler-1-6](https://hex-rays.com/blog/new-features-in-hex-rays-decompiler-1-6)

[Back](<https://hex-rays.com/blog>)

[hexrays](<https://hex-rays.com/blog/tag/hexrays>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# New features in Hex-Rays Decompiler 1.6

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/new-features-in-hex-rays-decompiler-1-6>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/new-features-in-hex-rays-decompiler-1-6>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/new-features-in-hex-rays-decompiler-1-6>)

I 

Igor Skochinsky ✦ Posted: Oct 10, 2011

![New features in Hex-Rays Decompiler 1.6](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Last week we released IDA 6.2 and Hex-Rays Decompiler 1.6. Many of the new IDA features have been described in previous posts, but there have been notable additions in the decompiler as well. They will let you make the decompilation cleaner and closer to the original source. However, it might be not very obvious how to use some of them, so we will describe them in more detail.

### 1\. Variable mapping

This is probably the simplest new feature and can be used without any extra preparation.

Sometimes the compiler stores the same variable in several places (e.g. a register and a stack slot). While the decompiler often manages to combine such locations, sometimes it’s not able to prove that they always contain the same value (especially in presence of calls that take address of stack variables). In such cases the user can help by performing such a merge or mapping manually.

Consider the following very common case:
    
    
    int __stdcall SciFreeFilterInstance(_FILTER_INSTANCE *pFilterInstance) { _FILTER_INSTANCE *v1; // esi@1
      v1 = pFilterInstance; if ( pFilterInstance->Signature != 'FrtS' ) RtlAssert( "(pFilterInstance)->Signature==SIGN_FILTER_INSTANCE", "d:\\xpsprtm\\drivers\\wdm\\dvd\\class\\codinit.c", 0x17A2u, 0); StreamClassDebugPrint(2, "Freeing filterinstance %p still open streams\n", v1);
    

The compiler copied an incoming argument (`pFilterInstance`) into a register (`v1==esi`). To get rid of the extra name, right-click the left-hand variable and choose “Map to another variable”, or place cursor on it and press ‘=’:

[![mapvar2](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mapvar2_thumb-3.png?width=292&height=248&name=mapvar2_thumb-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/mapvar2-Jun-18-2024-08-43-32-7894-AM.png>)

Choose the right-hand variable from the list.

[![mapvar3](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mapvar3_thumb-3.png?width=517&height=182&name=mapvar3_thumb-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/mapvar3-Jun-18-2024-08-43-31-2286-AM.png>)

Once decompilation is refreshed, both the left-hand variable (v1) and the assignment are gone. Now we have only one variable – the incoming argument.
    
    
    int __stdcall SciFreeFilterInstance(_FILTER_INSTANCE *pFilterInstance) {
      if ( pFilterInstance->Signature != 'FrtS' ) RtlAssert( "(pFilterInstance)->Signature==SIGN_FILTER_INSTANCE", "d:\\xpsprtm\\drivers\\wdm\\dvd\\class\\codinit.c", 0x17A2u, 0); StreamClassDebugPrint(2, "Freeing filterinstance %p still open streams\n", pFilterInstance);

You can map several variables to the same name, if necessary.

Made a mistake or mapped too much? It’s simple to fix. Right-click the wrongly mapped name and choose “Unmap variables”. Then choose the variable you want to see again.

### 2\. Union selection.

This feature, naturally, only applies to unions. That means that you need to have union types in your database and assign the types to some variables or fields.

Normally the decompiler tries to choose a union field which matches the expression best, but sometimes there are several equally valid matches, and sometimes other types in the expression are wrong. In such cases, you can override the decompiler’s decision. For example, this code is common in Windows drivers:
    
    
    NTSTATUS __stdcall DispatchDeviceControl(PDEVICE_OBJECT DeviceObject, PIRP Irp) { PIO_STACK_LOCATION stacklocation; // ebx@1 stacklocation = Irp->Tail.Overlay.CurrentStackLocation; if ( *&stacklocation->Parameters.Create.FileAttributes == 0x224010 ) { v8 = stacklocation->Parameters.Create.Options == 20; if ( !v8 ) goto LABEL_18; if ( stacklocation->Parameters.Create.SecurityContext < 1 ) goto LABEL_87; v23 = Irp->AssociatedIrp.MasterIrp;

Since we know we’re in a DeviceControl handler, it’s likely the code is inspecting the Parameters.DeviceIoControl substructure and not Parameters.Create.

Right-click the field and choose “Select union field”, or place cursor on it and press Alt-Y.

[![selunion2](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/selunion2_thumb-3.png?width=429&height=170&name=selunion2_thumb-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/selunion2-3.png>)

Choose the Parameters.DeviceIoControl.IoControlCode field.

[![selunion3](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/selunion3_thumb-3.png?width=615&height=297&name=selunion3_thumb-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/selunion3-3.png>)

Other references to Parameters.Create can be fixed the same way. The updated decompilation makes more sense:
    
    
    NTSTATUS __stdcall DispatchDeviceControl(PDEVICE_OBJECT DeviceObject, PIRP Irp) { PIO_STACK_LOCATION stacklocation; // ebx@1 stacklocation = Irp->Tail.Overlay.CurrentStackLocation; if ( stacklocation->Parameters.DeviceIoControl.IoControlCode == 0x224010 ) { v8 = stacklocation->Parameters.DeviceIoControl.InputBufferLength == 20; if ( !v8 ) goto LABEL_18; if ( stacklocation->Parameters.DeviceIoControl.OutputBufferLength < 1 ) goto LABEL_87;

### 3\. CONTAINING_RECORD macro

[This macro](<http://msdn.microsoft.com/en-us/library/windows/hardware/ff542043>) is commonly use in Windows drivers to get a pointer to the parent structure when we have a pointer to one of its fields.

For example, consider these two structures, used in a driver:
    
    
    struct _HW_STREAM_OBJECT {
      ULONG  SizeOfThisPacket;
      ULONG  StreamNumber;
      PVOID  HwStreamExtension;
      ...
    } HW_STREAM_OBJECT, *PHW_STREAM_OBJECT;
    struct _STREAM_OBJECT
    {
      _COMMON_OBJECT ComObj;
      _FILE_OBJECT *FilterFileObject;
      _FILE_OBJECT *FileObject;
      _FILTER_INSTANCE *FilterInstance;
      **_HW_STREAM_OBJECT HwStreamObject;**
      ...
    };

The following function accepts a pointer to _HW_STREAM_OBJECT:
    
    
    void __cdecl StreamClassStreamNotification(
      int NotificationType,
      _HW_STREAM_OBJECT *StreamObject,
      _HW_STREAM_REQUEST_BLOCK *pSrb,
      _KSEVENT_ENTRY *EventEntry,
      GUID *EventSet,
      ULONG EventId);

But immediately converts it into the containing _STREAM_OBJECT:
    
    
    mov eax, [ebp+StreamObject] test eax, eax push ebx push esi lea esi, [eax-_STREAM_OBJECT.HwStreamObject]
    

Default decompilation doesn’t look great:
    
    
     char *v6; // esi@1
      v6 = (char *)&StreamObject[-2] - 36;
    

There are two ways to make it nicer:

  1. Change type of v6 to be _STREAM_OBJECT*. The decompiler will detect that the expression “lines up” and convert it to use the macro. 
  2. Right-click on the delta being subtracted (-36), select “Structure offset” and choose _STREAM_OBJECT from the list. 



In both cases you should get a nice expression:
    
    
     v6 = CONTAINING_RECORD(StreamObject, _STREAM_OBJECT, HwStreamObject);

_N.B._ : currently you need to refresh the decompilation (press F5) to see the changes. We’ll improve it to happen automatically in future.

### 4\. Kernel and user-mode macros involving fs segment access.

On Windows, the `fs` segment is used to store various thread-specific (for user-mode) or processor-specific (for kernel mode) data. Hex-Rays Decompiler 1.6 detects the most common ways of accessing them and converts them to corresponding macros. However, this functionality requires presence of specific types in the database. For user mode, it is the `_TEB` structure, for kernel mode it’s the `KPCR` structure.

For example, consider the following code:
    
    
    mov eax, large fs:18h mov eax, [eax+30h] push 24h push 8 push dword ptr [eax+18h] call ds:__imp__RtlAllocateHeap@12 ; RtlAllocateHeap(x,x,x) mov esi, eax

If you don’t have the `_TEB` structure in types, this will be decompiled to:
    
    
      v5 = RtlAllocateHeap(*(_DWORD *)(*(_DWORD *)(__readfsdword(24) + 48) + 24), 8, 36);

However, if you do add the type, it will look much nicer:
    
    
      v5 = RtlAllocateHeap(NtCurrentTeb()->ProcessEnvironmentBlock->ProcessHeap, 8, 36);

Currently we support the following macros:

Macro | Required types  
---|---  
NtCurrentTeb | _TEB  
KeGetPcr | KPCR  
KeGetCurrentPrcb | KPCR, KPCRB  
KeGetCurrentProcessorNumber | KPCR  
KeGetCurrentThread | KPCR, _KTHREAD  
  
_Hint_ : the easiest way to get `_TEB` or `KPCR` types into your database is using the PDB plugin. Invoke it from File|Load file|PDB file…, enter a path to kernel32.dll (for user-mode code) or ntoskrnl.exe (for kernel-mode code), and check the “Types only” checkbox.

[![kernpdb](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/kernpdb_thumb-3.png?width=685&height=206&name=kernpdb_thumb-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/kernpdb-3.png>)

PDBs for those two files usually contain the necessary OS structures.

We hope you will like these new additions. Note that the version 1.6 includes even more improvements and fixes, see [the full list](<http://www.hex-rays.com/products/decompiler/news.shtml#111005>) of the new features and [the comparison page](<http://www.hex-rays.com/products/decompiler/v16_vs_v15.shtml>).

---

## 3. 

**Date:** Posted: Sep 12, 2011  
**Link:** [https://hex-rays.com/blog/ida-pro-6-2-beta](https://hex-rays.com/blog/ida-pro-6-2-beta)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [beta](<https://hex-rays.com/blog/tag/beta>) [IDA62](<https://hex-rays.com/blog/tag/ida62>)

# IDA Pro 6.2 beta

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-6-2-beta>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-6-2-beta>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-6-2-beta>)

I 

Igor Skochinsky ✦ Posted: Sep 12, 2011

![IDA Pro 6.2 beta](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Soon we are going to start testing the next IDA version. There will be many improvements. Some of them we have mentioned previously:  
[Proximity view](<http://www.hexblog.com/?p=468> "New feature in IDA 6.2: The proximity browser")  
[PE+ support for Bochs (64-bit PE files)](<http://www.hexblog.com/?p=403> "Unpacking mpress’ed PE+ DLLs with the Bochs plugin")  
[UI shortcut editor](<http://www.hexblog.com/?p=437> "Filters & Shortcuts")  
[Filters in choosers](<http://www.hexblog.com/?p=437> "Filters & Shortcuts")  
[Database snapshots](<http://www.hexblog.com/?p=415> "IDA Pro 6.2 with database snapshots support")  
Other new major features:

  * GUI installers for Linux and OS X  
[  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/setup_linux-3.png?width=510&height=379&name=setup_linux-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/setup_linux-3.png>)  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/setup_osx1-e1315834636840-3.png?width=580&height=452&name=setup_osx1-e1315834636840-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/setup_osx1-e1315834636840-3.png>)
  * Automatic check for new versions:  
[  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/autocheck1-3.png?width=600&height=344&name=autocheck1-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/autocheck-3.png>)
  * Cross-references to structure members:  
[  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/strxref-3.png?width=600&height=368&name=strxref-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/strxref-3.png>)
  * Floating licenses: our licensing system is now more flexible and allows big enterprises to purchase floating licenses. Contact [sales@hex-rays.com](<mailto:sales@hex-rays.com>) for more information.



If you have an active license and would like to test the beta, please send a message to support@hex-rays.com.

---

## 4. 

**Date:** Posted: Sep 5, 2011  
**Link:** [https://hex-rays.com/blog/filters-shortcuts](https://hex-rays.com/blog/filters-shortcuts)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Filters & Shortcuts

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/filters-shortcuts>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/filters-shortcuts>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/filters-shortcuts>)

Hex-Rays ✦ Posted: Sep 5, 2011

![Filters & Shortcuts](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Two of the new UI highlights in the upcoming IDA release are filtering capability for choosers and shortcut management. I’ll be discussing them in this post, although seeing them live in action is much nicer. 😉

## Filters

Filters make it possible to either show, hide or highlight one or more categories of items. But enough talk, let’s start with a screenshot.  
![Filters demo](https://hex-rays.com/hubfs/Imported_Blog_Media/filters_demo-3.gif)  
  
This list was created by including only items containing “str” and “mem” in their function name and then by highlighting with different colors the remaining items.  
Here’s the dialog which allows to add specific filters for any chooser.  
![Modify filters dialog](https://hex-rays.com/hubfs/Imported_Blog_Media/filters_modify_dialog-3.gif)  
Don’t worry about the contrast, the chooser will automatically establish the best foreground color for each item given its background.  
However, opening a dialog to set a filter may be cumbersome if the goal is just to quickly visualize a particular category of items. That’s why it will be possible to filter choosers by pressing a shortcut (Ctrl-F), via a small edit field popping up below the list.  
![Quick filter demo](https://hex-rays.com/hubfs/Imported_Blog_Media/quick_filter_demo-3.gif)  
This edit field can filter items by additionally specifying three options: case sensitivity, regular expressions and whole words only.  
![Quick filter context menu](https://hex-rays.com/hubfs/Imported_Blog_Media/quick_filter_ctx-3.gif)  
A lot of rewriting and optimizing had to be done to make sure that even with very huge lists (50,000+ items) the filtering would be fast.

## Shortcut management

I think many users will appreciate that IDA Pro finally features a complete and advanced shortcut editor.  
![Shortcut editor](https://hex-rays.com/hubfs/Imported_Blog_Media/shortcut_editor-3.gif)  
It’s not only possible to change shortcuts for built-in IDA actions, but also the default shortcuts of plugins, external menu entries and scripts. The shortcut editor will signal modified shortcuts (yellow), conflicting shortcuts (red) or whether both conditions are met (orange). It also provides some additional information about the action such as its origin (built-in, menu, plugin, script) and context. The context tells the user when this action can be executed, e.g.: some actions are globally active, while others may only be executed in the disassembly.  
One interesting new addition to the action management is that an action now regains its lost shortcut if the conflict which caused the action to lose it ceases to exist.  
And in case you wish to migrate your shortcut settings: all the modifications accomplished through the shortcut editor are saved to the ‘shortcuts.cfg’ file inside the user’s directory.

---

## 5. 

**Date:** Posted: Aug 8, 2011  
**Link:** [https://hex-rays.com/blog/new-feature-in-ida-6-2-the-proximity-browser](https://hex-rays.com/blog/new-feature-in-ida-6-2-the-proximity-browser)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# New feature in IDA 6.2: The proximity browser

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/new-feature-in-ida-6-2-the-proximity-browser>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/new-feature-in-ida-6-2-the-proximity-browser>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/new-feature-in-ida-6-2-the-proximity-browser>)

Joxean Koret ✦ Posted: Aug 8, 2011

![New feature in IDA 6.2: The proximity browser](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

The new IDA Pro 6.2 release will be featuring a new view called the “proximity browser” (only available in the Qt version).

[![Proximity view](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pv-300x135-3.png?width=300&height=135&name=pv-300x135-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pv-3.png>)

The proximity view

  
**The proximity view**  
The proximity view (“PV” in short) allows the reverser to see and browse the relationships between functions, global variables, constants, etc… We can use the PV, for example, to visualize the complete callgraph of a program, to see the path between 2 functions or what global variables are referenced from some function. This view is accessible from the disassembly view by “zomming out” from the current function (using the “-” hotkey or from the “Proximity view” context menu).  
In the PV, it is possible to double-click a node to show the relationships of a function (equally, one can jump to a new function by pressing “G”), double-click the elliptic nodes to show/expand the children of a node (x-refs from) or select the “Show/Hide parents” from the context menu to show or hide the parents (x-refs to) of the selected node.

[![Proximity view contextual menu](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pv-menu1-300x296-3.png?width=300&height=296&name=pv-menu1-300x296-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pv-menu1-3.png>)

Proximity view contextual menu

**Finding paths******  
One of the most interesting feature in this initial version of the PV is the “Find path” (accessible from the context menu). This feature allows us to show graphically (new nodes will be imported to the view with appropriate edge connections) the path between 2 nodes (for example, a function and a global variable).  
Let’s imagine you want to know where the function ShellExecuteA in shell32.dll verifies if the path is UNC or not. To do this press “G” to go to the function ShellExecuteA, then “zoom out” to the proximity view by pressing “-”. A view similar to the following will appear:  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pv-shellexec-300x84-3.png?width=300&height=84&name=pv-shellexec-300x84-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pv-shellexec-3.png>)Now, select the green node (aka central node), right-click and, from the context menu, select “Hide childs” in order to hide all the other nodes as we aren’t interested in them. Now, right click and “Add name” from the context menu and in the newly opened dialog find and select “PathIsUNCW”. The proximity view will look like this:  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pv-shellexec1-300x82-3.png?width=300&height=82&name=pv-shellexec1-300x82-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pv-shellexec1-3.png>)As you noticed, there is no direct relationship between the nodes and, as so, both nodes appear disconnected. Now, we will search a path between them: select the node “__imp_PathIsUNCW” and select from the contextual menu the option “Find path”. A new dialog will be opened showing all the nodes of the current callgraph. Select “ShellExecuteA” and click OK. A new graph will be displayed:  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pv-shellexec2-300x207-3.png?width=300&height=207&name=pv-shellexec2-300x207-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pv-shellexec2-3.png>)  
**Layouts**  
Before finishing this post, we wanted to show you some more screenshots showing different layouts that will be available for the proximity view with the 6.2 release:

[![Circle layout](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/circle-300x247-3.png?width=300&height=247&name=circle-300x247-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/circle-3.png>)

Circle layout

[![Polar tree layout](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/layout1-300x221-3.png?width=300&height=221&name=layout1-300x221-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/layout1-3.png>)

Polar tree layout

[![Polar tree layout 2](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/layout2-300x183-3.png?width=300&height=183&name=layout2-300x183-3.png)](<https://hex-rays.com/hubfs/Imported_Blog_Media/layout2-3.png>)

Polar tree layout 2

And that’s all! We hope you like the new feature!

---

## 6. 

**Date:** Posted: Aug 3, 2011  
**Link:** [https://hex-rays.com/blog/book-review-ida-pro-book-2nd-edition](https://hex-rays.com/blog/book-review-ida-pro-book-2nd-edition)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Book review: IDA Pro Book, 2nd Edition

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/book-review-ida-pro-book-2nd-edition>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/book-review-ida-pro-book-2nd-edition>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/book-review-ida-pro-book-2nd-edition>)

Elias Bachaalany ✦ Posted: Aug 3, 2011

![Book review: IDA Pro Book, 2nd Edition](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

A few weeks ago we received an electronic copy of the “[IDA Pro Book, 2nd Edition](<http://nostarch.com/idapro2.htm>)”. In the second edition of his 26 chapters book, Chris Eagle did a good job updating the book and covering the latest changes in IDA Pro 6.1: the IDA Qt graphical interface is illustrated in this edition (all screenshots are up to date), some chapters are slightly updated whereas some have new sections that cover topics such as IDAPython, various debugger plugins and other features.  
  
In this edition, though the book structure remained the same, the chapters have been updated to cover the new features in IDA Pro 6.1. In this blog post we are not going to review the whole book, instead we will take the opportunity to review the most obvious changes and additions.  
For a complete review of the previous edition, please check Sebastian Porst’s review [here](<http://www.the-interweb.com/serendipity/index.php?/archives/111-Book-Review-The-IDA-Pro-Book.html>).  
**Part IV – Extending IDA’s capabilities:** This part has been heavily updated to cover the major additions to the SDK and the [IDA kernel](<http://www.hexblog.com/?p=127>). For example, in chapter 17 to chapter 19, there is a new section explaining how to write plugins, file loaders or processor modules using scripts (withIDC or IDAPython).  
**Chapter 15:** formerly called “Scripting with IDC” is now called “IDA Scripting”. This chapter not only talks about scripting with IDC but with Python too.  
The IDC language section has been updated to cover the [new](<http://www.hexblog.com/?p=115>) IDC language features (since IDA 5.6) such as IDC objects, string slicing operations and other changes to the language.  
There are two new sections, one introducing IDAPython and the other a set of useful IDAPython examples.  
**Chapter 17 – The IDA Plug-in Architecture:** has a new section covering how to use Qt to write UI rich plugins for the [idaq](<http://www.hexblog.com/?p=118>) interface.  
**Chapter 23 – Real-World IDA Plugins:** the list of real world IDA plugins have been updated. The new plugins are:

  * [MyNav](<http://code.google.com/p/mynav/>): a plugin by Joxean Koret, helps reverse engineers in the most typical task like discovering what functions are responsible of some specifical tasks, finding paths between _interesting_ functions and data entry points”
  * Class informer: a plugin by Sirmabus, is a plug-in designed to assist in the process of reverse engineering C++ code that was compiled using Microsoft Visual Studio
  * IdaPdf: a plugin by Chris Eagle, is a very handy “PDF loader and plug-in for dissecting and navigating PDF files”



**Chapter 26 – Additional debugger features:** introduces the Bochs debugger plugin and its three modes of operation, while giving real life examples about how/where best to use each mode. The chapter is concluded by a section covering the basics of the [Appcall](<http://www.hexblog.com/?p=112>) feature. Nonetheless, the Windbg debugger plugin and the crash dump analysis facilities have been briefly covered.  
Chris proves again his captivating and informative writing style. We highly recommend this book for new users that want to learn how to apply reverse engineering skills with IDA or seasoned users who want to take their IDA expertise to the next level and start writing extensions.

---

## 7. 

**Date:** Posted: Aug 2, 2011  
**Link:** [https://hex-rays.com/blog/recon-2011-practical-c-decompilation](https://hex-rays.com/blog/recon-2011-practical-c-decompilation)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Recon 2011: Practical C++ Decompilation

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/recon-2011-practical-c-decompilation>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/recon-2011-practical-c-decompilation>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/recon-2011-practical-c-decompilation>)

I 

Igor Skochinsky ✦ Posted: Aug 2, 2011

![Recon 2011: Practical C++ Decompilation](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Last month I visited the [Recon conference](<http://recon.cx/>) and had a great time again. I gave a talk on C++ decompilation and how to handle it in IDA and Hex-Rays decompiler. You can get the slides [here](<https://www.hex-rays.com/wp-content/uploads/2011/08/Recon-2011-Skochinsky.pdf>), and download the recorded talk [here](<http://www.archive.org/details/Recon_2011_Practical_Cpp_decompilation>).  
**Edit** : for some reason the streaming version does not show anything after the intro, please download the Quicktime version until it’s fixed.

---

## 8. 

**Date:** Posted: Jul 29, 2011  
**Link:** [https://hex-rays.com/blog/ida-pro-6-2-with-database-snapshots-support](https://hex-rays.com/blog/ida-pro-6-2-with-database-snapshots-support)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 6.2 with database snapshots support

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-6-2-with-database-snapshots-support>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-6-2-with-database-snapshots-support>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-6-2-with-database-snapshots-support>)

Elias Bachaalany ✦ Posted: Jul 29, 2011

![IDA Pro 6.2 with database snapshots support](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

The most frequently asked question we get during the IDA Pro [trainings](<http://www.hex-rays.com/idapro/training/>), on the support [forum](<//hex-rays.com/forum/>) or via support emails is: “When will IDA Pro support the undo feature?” or “How can I undo an operation in IDA Pro”.  
Our answer has always been: “Sorry, it is not possible to undo in IDA Pro” or “This feature will eventually be implemented sometime in the future”.  
In this blog post, we introduce the new database snapshots feature that will be present in IDA Pro 6.2:  
[![snap_man](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/snap_man_thumb-3.gif?width=580&height=341&name=snap_man_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/snap_man-3.gif>)  


## Why there is no undo option in IDA Pro

The lack of the undo facility stems from the fact that IDA Pro’s database format is not transactional. Each operation in IDA Pro may entail a great deal of other operations that can change the database contents massively.  
Take for instance the case when the user goes over an unexplored area and press “C” to create code. This is what happens:

  * IDA tries to create instructions
  * For each instruction there could be side effects: 
    * Creating code, functions or data items
    * A new target address is added to the analysis queue
    * Another unexplored area will become explored
  * The whole algorithm keeps on repeating itself until the analysis queue becomes empty



So, sometimes pressing “C” in one place can completely change the database (functions will be created, data items will be defined, xrefs will be generated, etc…).  
What about the case of deleting a segment:

  * Segment deletion will also entail deletion of all instructions
  * Deletion of all related cross references
  * etc…



What we discussed so far are the extreme cases, but what about simply undoing a rename operation? It is true that such a simple operations can be easily tracked, recorded and undone if needed.  
In fact, IDA Pro provides a set of callbacks (IDB/IDP callbacks) that allow the programmer to register a callback function that will be triggered in a pre/post manner. The programmer will have a chance to record the operation, modify it before it is carried by the kernel or just handle it completely without passing it to the kernel.  
Here’s an excerpt from “idp.hpp”:
    
    
    // IDB event group. Some events are still in the processor group, so you will
    // need to hook to both groups. These events do not returns anything.
    // The callback function should return 0 but the kernel won't check it.
    // Use the hook_to_notification_point() function to install your callback.
      enum event_code_t
      {
        byte_patched,           // A byte has been patched
                                // in: ea_t ea, uint32 old_value
        cmt_changed,            // An item comment has been changed
                                // in: ea_t ea, bool repeatable_cmt
        enum_created,           // An enum type has been created
                                // in: enum_t id
        enum_deleted,           // An enum type has been deleted
                                // in: enum_t id
        enum_renamed,           // An enum or member has been renamed
                                // in: tid_t id
        ....
        enum_cmt_changed,       // An enum or member type comment has been changed
                                // in: tid_t id, bool repeatable
        destroyed_items,        // Instructions/data have been destroyed in [ea1,ea2)
                                // in: ea_t ea1, ea_t ea2, bool will_disable_range
        ....

  
Real life plugins that use the IDP/IDB callback mechanism include the collabREate plugin by Chris Eagle and the [IDA Sync plugin](<http://www.openrce.org/downloads/details/2>) written by Pedram Amini. Nonetheless, those plugins do not aim at providing an undo functionality rather a way to make reverse engineering with IDA Pro a collaborative effort.

## Introducing the database snapshot feature

Since the “undo” feature may not be implemented in the near future, we thought of implementing a nice and convenient way to take database snapshots and restore them easily from IDA Pro.  
In a nutshell, an IDA Pro database snapshot is a copy of the current database with the following name: **databasename_mmddyyyy_hhmmss.idb**. In the future, we could optimize the database storage requirement so that only the difference will be stored on disk.  
This new database snapshot feature is very similar to “VM snapshots” feature found in most virtualization products (such as VMWare, VirtualBochs, QEmu, etc…) where the user can take a snapshot of the VM at any point in time and work with it (restore, delete, etc…).  
Taking a snapshot will be accessible from two places:

  * The first is in the file menu (or by pressing the Ctrl-Shift-W hotkey):



[![snap_quick](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/snap_quick_thumb-3.gif?width=427&height=244&name=snap_quick_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/snap_quick-3.gif>)

  * And the second method is through the database snapshot manager interface:



[![snap_man_menu](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/snap_man_menu_thumb-3.gif?width=277&height=370&name=snap_man_menu_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/snap_man_menu-3.gif>)

## The snapshot manager interface

In the snapshot manager interface window, the user will be able to restore, rename, delete or take a snapshot:  
[![snap_man](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/snap_man_thumb1-3.gif?width=580&height=341&name=snap_man_thumb1-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/snap_man1-3.gif>)  
Finally, we would like to thank our kind customers that keep on giving us suggestions and ideas that help us improve the product.

---

## 9. 

**Date:** Posted: Jul 1, 2011  
**Link:** [https://hex-rays.com/blog/unpacking-mpressed-pe-dlls-with-the-bochs-plugin](https://hex-rays.com/blog/unpacking-mpressed-pe-dlls-with-the-bochs-plugin)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Unpacking mpress’ed PE+ DLLs with the Bochs plugin

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/unpacking-mpressed-pe-dlls-with-the-bochs-plugin>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/unpacking-mpressed-pe-dlls-with-the-bochs-plugin>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/unpacking-mpressed-pe-dlls-with-the-bochs-plugin>)

Elias Bachaalany ✦ Posted: Jul 1, 2011

![Unpacking mpress’ed PE+ DLLs with the Bochs plugin](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In IDA Pro 6.1 we extended the Bochs debugger plugin to support debugging of 64bit code snippets. With IDA Pro 6.2 it will be possible to debug PE+ executables as well. Since the execution will be emulated inside Bochs, a 64bit operating system is not required and one could be equally running a 32 or 64bit Linux, Mac OS or Windows operating system and still be able to debug 64bit PE files from IDA Pro.  
To illustrate this new feature, we are going to unpack and briefly analyze a PE+ trojan that is compressed with [MPRESS](<http://www.matcode.com/mpress.htm>) from [MATCODE Software](<http://www.matcode.com/>).We will illustrate how to unpack the DLL, recover the import table and cleanup the database to get it ready for analysis.  
[![bochs_options](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bochs_options_thumb-3.gif?width=391&height=322&name=bochs_options_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/bochs_options-3.gif>)  


## Unpacking the DLL

Our target is a 64bit trojan DLL identified as “Win32/Giku”. We start analyzing this DLL by loading it in _idaq64_ and checking the segment list by pressing Ctrl-S:  
[![mpress](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/mpress_thumb-3.gif?width=594&height=300&name=mpress_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/mpress-3.gif>)  
Notice how the section names and attributes signal the presence of the MPRESS packer.  
To debug this DLL, make sure that “Bochs debugger plugin” is selected in PE and 64bit emulation mode.  
After starting the debugger, we observe the following code which calls the unpack():  
[![bochs_unpacked](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bochs_unpacked_thumb-3.gif?width=807&height=370&name=bochs_unpacked_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/bochs_unpacked-3.gif>)  
If we step a bit further, we reach the code that reconstructs the imports by calling LoadLibrary()/GetModuleHandle() in a loop followed by another nested loop that calls GetProcAddress() and stores the result in the IAT:  
[![bochs_restore_imports](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bochs_restore_imports_thumb-3.gif?width=656&height=590&name=bochs_restore_imports_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/bochs_restore_imports-3.gif>)  
Noting down the value of the **rdi** register once just before the **stosq** is executed will give us the IAT _start_ address and by noting **rdi** once more at the end of both the loops we get the IAT _end_ address.  
Shortly after the IAT has been restored, we notice the jump to the original entry point:  
[![bochs_jump_oep](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bochs_jump_oep_thumb-3.gif?width=632&height=186&name=bochs_jump_oep_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/bochs_jump_oep-3.gif>)  
And the entry point code:  
[![nosigs](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/nosigs_thumb-3.gif?width=607&height=618&name=nosigs_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/nosigs-3.gif>)  
This is the real DllEntryPoint() of the unpacked program. Now with the OEP and the IAT start/end values at hand, we are ready to cleanup the database so it represents the unpacked program.

## Reconstructing and cleaning the database

There are many ways to clean the database from stale / unused information after the program has been unpacked. This involves the following steps:

  1. Locating the IAT and creating an XTRN segment to represent the imports
  2. Deleting the packer’s entry point and adding the original entry point (after unpacking)
  3. Re-analyzing the code
  4. Applying FLIRT signatures
  5. Deleting the unused packer’s segments (optional)



Steps 1 to 3 can be automated using the _uunp_ plugin that ships with IDA. Select from the menu “Edit/Plugins/Universal unpacker manual reconstruct”:  
[![uunp_reconstruct_plg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/uunp_reconstruct_plg_thumb-3.gif?width=269&height=122&name=uunp_reconstruct_plg_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/uunp_reconstruct_plg-3.gif>)  
to invoke the _uunp_ plugin directly after reaching the OEP. Fill in all the previously gathered information:  
[![uunp_dlg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/uunp_dlg_thumb-3.gif?width=538&height=230&name=uunp_dlg_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/uunp_dlg-3.gif>)  
and press OK. A new imports segment will be created, the code segment will be reanalyzed and finally a memory snapshot of the unpacked program will be taken.  
We are now ready to apply the appropriate FLIRT signatures:  
[![apply_sig](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/apply_sig_thumb-3.gif?width=504&height=634&name=apply_sig_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/apply_sig-3.gif>)  
After selecting the “vc64rtf” signature from the signatures window (Shift-F5) we notice how IDA identified library functions and colored them with light blue making reverse engineering even easier.

## Analyzing the unpacked code

After the code is unpacked, a quick inspection, the _strings window_ reveals a set of encrypted strings:  
[![decrypt_str1](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decrypt_str1_thumb-3.gif?width=882&height=224&name=decrypt_str1_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/decrypt_str1-3.gif>)  
With the help of cross-references, the decryption function was identified. After giving it a proper prototype, we can [Appcall](<http://www.hexblog.com/?p=112>) it to decrypt the strings:  
[![decrypt_str2](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decrypt_str2_thumb-3.gif?width=740&height=252&name=decrypt_str2_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/decrypt_str2-3.gif>)  
We got a URL pointing to an encrypted text file. After digging a bit in the database, the function used to decrypt files was located:  
[![decfileasm](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decfileasm_thumb-3.gif?width=643&height=466&name=decfileasm_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/decfileasm-3.gif>)  
And this is the Appcall version of decrypt_file():  
[![decfile](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/decfile_thumb-3.gif?width=333&height=327&name=decfile_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/decfile-3.gif>)  
We use it to decrypt _spm.txt_ :  
[![spm](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/spm_thumb-3.gif?width=517&height=250&name=spm_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/spm-3.gif>)  
x32.jpg is DLL packed with UPX and x64.jpg is a PE+ DLL packed with mpress.  
Hope you found this blog post useful. Comments and suggestion are welcome.

---

