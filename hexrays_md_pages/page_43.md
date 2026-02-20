# Hex-Rays Blog – Page 43

**Archived:** 2026-02-20 12:23
**URL:** https://hex-rays.com/blog/page/43

---

## 1. 

**Date:** Posted: May 18, 2011  
**Link:** [https://hex-rays.com/blog/precompiled-pyside-binaries-for-ida-pro](https://hex-rays.com/blog/precompiled-pyside-binaries-for-ida-pro)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Precompiled PySide binaries for IDA Pro

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/precompiled-pyside-binaries-for-ida-pro>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/precompiled-pyside-binaries-for-ida-pro>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/precompiled-pyside-binaries-for-ida-pro>)

Elias Bachaalany ✦ Posted: May 18, 2011

![Precompiled PySide binaries for IDA Pro](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In a [previous blog post](<http://www.hexblog.com/?p=229>) we mentioned that it is possible to use IDA Pro with [PySide](<http://www.pyside.org/>) (Python + Qt) after applying some minor code patches to PySide.  
For convenience purposes, we precompiled the PySide libraries that work with IDA Pro 6.0+ and Python 2.6/2.7. Below is a brief explanation on how to install and use those [binaries](<http://www.hex-rays.com/products/ida/support/download.shtml>).  
**Edit** : 2012-06-29 updated links for IDA 6.3/Python 2.7  


### Installing on Windows

Please [download](<http://www.hex-rays.com/products/ida/support/download.shtml>) the package matching your IDA and Python version and unpack it to _Path_to_Python_ (e.g. C:\Python27).

### Installing on Linux or OS X

Please [download](<http://www.hex-rays.com/products/ida/support/download.shtml>) the appropriate package and unpack it:

> sudo tar xvfz _os_package_ _pyside_python27_package.tgz -C /

### Building PySide from the sources

In order to build PySide for IDA Pro you need to use the [modified PySide source files](<http://www.hex-rays.com/products/ida/support/download.shtml>). Please note that those modifications only apply to the PySide version indicated. To use newer versions of PySide merge those files with the new files.

### Testing the installation

To test if PySide is installed properly, you can try running the following [script](<https://www.hex-rays.com/wp-content/static/ida_pro/files/ImportExportViewer.py>) which creates a tree widget and displays all imports and exports from the opened database:  
[![pyside_bins](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/pyside_bins_thumb-4.gif?width=572&height=268&name=pyside_bins_thumb-4.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/pyside_bins-4.gif>)

### Mixing C++ and Python

With PySide and IDAPython it is possible to write powerful scripts with a rich UI. Now the question is “how to write a UI in Python (with PySide) and still be able to call C/C++ functions?”  
To achieve this, you need to expose your C/C++ functions to Python. It can be done with [Ctypes](<http://docs.python.org/library/ctypes.html>), [SIP](<http://www.riverbankcomputing.co.uk/software/sip/download>), [SWIG](<http://swig.org/>), Shiboken, other Python binding generators or with simple manual wrapping as demonstrated [here](<http://www.hexblog.com/?p=126>).  
On the [Lynxline ](<http://lynxline.com/>)website you can find two interesting blog posts demonstrating how to mix C++ and Python using [SIP](<http://lynxline.com/qt-python-superhybrids/>) or [Shiboken](<http://lynxline.com/superhybrids-part-2-now-qt-pyside/>).

Happy coding! 🙂

---

## 2. 

**Date:** Posted: Apr 21, 2011  
**Link:** [https://hex-rays.com/blog/virustotal-plugin-for-ida-pro](https://hex-rays.com/blog/virustotal-plugin-for-ida-pro)

[Back](<https://hex-rays.com/blog>)

[IDAPython](<https://hex-rays.com/blog/tag/idapython>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [IDA61](<https://hex-rays.com/blog/tag/ida61>) [VirusTotal](<https://hex-rays.com/blog/tag/virustotal>)

# VirusTotal plugin for IDA Pro

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/virustotal-plugin-for-ida-pro>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/virustotal-plugin-for-ida-pro>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/virustotal-plugin-for-ida-pro>)

Elias Bachaalany ✦ Posted: Apr 21, 2011

![VirusTotal plugin for IDA Pro](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

In this blog post, we are going to illustrate how to use some of the new UI features introduced in [IDA Pro 6.1](<http://www.hex-rays.com/idapro/61/index.html>) ([embedded choosers](<http://www.hexblog.com/?p=265>), custom icons, etc…) by writing a [VirusTotal](<http://www.virustotal.com/>) reporting and file submission plugin for IDA Pro. The plugin will allow you to get reports from VirusTotal based on the input file MD5 or a file of your choice. The plugin will offer to upload the file if the file was not analyzed before.  
[![vt_ui_dlg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_ui_dlg_thumb-3.gif?width=685&height=530&name=vt_ui_dlg_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_ui_dlg-3.gif>)  


## The VirusTotal public API

The VirusTotal API is web-service that uses HTTP POST requests with JSON object responses. To work with it you need an API key (get one by creating a VT Community account) and a programming language/library that can make HTTP POST requests and is able to parse JSON strings. In this article, we are going to borrow some code from _Bryce Boe_ ’s blog entry “[Submitting Binaries to VirusTotal](<http://www.bryceboe.com/2010/09/01/submitting-binaries-to-virustotal/>)” and modify it a bit. The resulting changes can be found in the **BboeVt** module as part of this article’s files. Our plugin is going to use the **get_file_report()** to get a report given a file’s MD5 and the **scan_file()** to submit a new file to VT

## Writing the plugin

Let us first breakdown the plugin requirements into small and simple steps and then at the end we can glue everything together.  
Here are the components we need:

  1. A Python wrapper for the VT API: We will use the module mentioned above.
  2. A configuration utility: A simple utility class to save persistent information (like the API key or the reporting options)
  3. A chooser control: This control will store the VT report result in a nice ListView with two columns (AV Vender/Malware name). This control should function in embedded or standalone modes.
  4. A form control: It will allow us to take input information and display the results
  5. The plugin: A plugin class that will orchestrate all of the above components



### The configuration utility class

We need a simple class to store persistent data. The data store location will be determined using the **idaapi.get_user_idadir()**. It resolves to “%APPDATA%\Hex-Rays\IDA Pro” on MS Windows and to “~/.idapro” on Linux or Mac OS.  
The persistent information we need to store are the _apikey_ and the plugin dialog options. The other configuration fields are computed during runtime:

  * _md5sum_ : when IDA Pro loads a file, it stores the input file’s MD5 in the database. We can use the **idc.GetInputMD5()** to retrieve the store MD5 value.
  * input file: by default we take the debugged input file path (this value can be changed in the “Debug/Process options” menu).



[![vt_func_cfg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_cfg_thumb-3.gif?width=525&height=488&name=vt_func_cfg_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_cfg-3.gif>)

### The chooser control

The chooser control will be a subclass of the **Choose2** class. This Python class is a wrapper for the **choose2()** IDA Pro SDK function. Embedded choosers (introduced in IDA Pro 6.1) allow choosers not only to exist as separate dockable windows but also to be embedded inside forms. Please see Daniel’s [post](<http://www.hexblog.com/?p=265>).  
Before creating the chooser class, let us illustrate how to work with custom icons:  
[![vt_func_custom_icon](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_custom_icon_thumb-3.gif?width=522&height=359&name=vt_func_custom_icon_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_custom_icon-3.gif>)  
As you can see, two function calls are involved:

  * **load_custom_icon()** : The icon data can be a path to a file or a binary string. In the former you pass _filename_ =”path_to_img” parameter and in the latter you need to pass _data_ and _format_.  
The resulting icon id can be used with any API function that needs an icon id.
  * **free_custom_icon():** When done using the icon, make sure you free it with this function.



This is the chooser class (important aspects in the code are marked):  
[![vt_func_echooser](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_echooser_thumb-3.gif?width=521&height=492&name=vt_func_echooser_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_echooser-3.gif>)

  * The Chooser2 class takes a new (optional) parameter named _embedded_. If set to true, then this chooser will embeddable and can be hosted by a form (as we will see in the next section).
  * The _icon_ parameter will contain an icon_id that is created with a load_custom_icon() call.
  * The _items_ parameter is a list of lists. Since we are working with two columns then it should have the following format: [ [“row1_col1”, “row1_col2”, “row2_col1”, “row2_col2”], …. ]
  * The _OnSelectLine()_ is a callback that is triggered whenever the user double-clicks on an item. We made our plugin open a web-browser and issue a search query



[![vt_ui_browser](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_ui_browser_thumb-3.gif?width=688&height=255&name=vt_ui_browser_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_ui_browser-3.gif>)  
To create the chooser it is sufficient to create an instance (and pass the mandatory parameters) then call the Show() method.

### The form control

The form control is implemented by subclassing the new **Form** class found in IDAPython >= 1.5.  
In essence, the **idaapi.Form** class wraps around **AskUsingForm()** IDA Pro SDK function.  
If you’ve worked with **AskUsingForm()** before then you will find it so easy to work with the **Form** class.  
The advantage of using **AskUsingForm()** or IDAPython’s **Form** class, instead of operating system UI functions, is that your UI will be portable and will work across platforms and most importantly you don’t need any external dependencies or 3rd party UI libraries installed on the system.  
For example, this is how the form will be rendered on Mac OS:  
[![vt_ui_dlg_mac_upload](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_ui_dlg_mac_upload_thumb-3.gif?width=595&height=484&name=vt_ui_dlg_mac_upload_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_ui_dlg_mac_upload-3.gif>)  
This is how the form creation code looks like (explanation will follow):  
[![vt_func_form_create](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_form_create_thumb-3.gif?width=642&height=408&name=vt_func_form_create_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_form_create-3.gif>)  
The following sections will explain in more details how to use this class.

#### The form string syntax

The form string syntax is based on the description found in _kernwin.hpp_ SDK header. The syntax is slightly different; the difference is that you should describe form controls using special tags (surrounded by { and }). Please search for “Format string for AskUsingForm_c()” inside the _kernwin.hpp_ header file to get more documentation about the form string syntax.  
The form string explained:

  1. The first line contains some special **AskUsingForm()** directives. Here we use the _STARTITEM_ to specify which control takes the initial focus. A special function {id:ControlName} is used. It will be replaced by an actual id later when the form is compiled.
  2. After the directives comes the form title
  3. Then we declare that we have a form change callback (notice the _{FormChangeCb}_)
  4. Input form controls are denoted inside “<” and “>”: 
     1. The general syntax is: **<** #_Hints if any_ #S~o~me label:**{ControlName} >**
     2. You can escape the curly braces with “\\{“
     3. Group controls (such as radio or checkbox groups) should have an extra “**>**{**GroupControl**}**>** ” termination
     4. The tilde character is equivalent to the ampersand character (on MS Windows). It is used to specify an access key to a given control.
  5. Label controls (address and string labels) can be created too
  6. All other text in this section will be rendered as-is in the dialog



If you’re curious, this is how the form would look like if we did not using the Form class syntax:  
[![vt_func_compform](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_compform_thumb-3.gif?width=541&height=219&name=vt_func_compform_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_compform-3.gif>)  
As you see, all the {Control} strings are expanded to the actual syntax required by **AskUsingForm()**(also notice how the IDs are assigned as well).

#### Form controls

All the allowed form controls (input or label controls) are declared as inner-classes in the **Form** class. When you create the form, you need to pass the form string (as we seen above) and a dictionary of controls with key=ControlName and value=Control_Object_Instance.  
Please refer to Form documentation or the ex_askusingform.py example.  
Embedded choosers cannot be directly embedded, first you need to create an embedded chooser control and then link it to the embedded chooser instance.

#### Form change callback

The _FormChangeCb_ control will allow you to specify a callback that will be triggered on form events. Form change callback resemble dialog procedures on MS Windows. This callback will be triggered whenever a form control is focused or changed. From this callback you can hide/enable/move/modify other form controls.  
[![vt_func_form_chgcb](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_form_chgcb_thumb-3.gif?width=605&height=656&name=vt_func_form_chgcb_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_form_chgcb-3.gif>)  
In the code above, we check which control id generated an event and handle actions accordingly.  
For example we are enabling/disabling certain checkboxes and handling the “Report” button click by calling the **VtReport()** function and updating the embedded chooser contents by calling **Form.RefreshField()**.

#### Showing the form

A form must be compiled with **Compile()** before it can be displayed with**Execute()**. No need to compile the form more than once (unless you change the form string). To check if the form is already compiled use the **Compiled()** function.  
It is possible to populate the initial form controls values in two ways:

  * When a form is compiled and before calling Execute() you can directly write to the “value”, “selected” or “checked” field of a form control
  * In the Form change callback: control id == –1 will be passed when the form is created and control id == –2 when the form is closed by pressing Ok.



This is how we show the form (first compile, populate and then display):  
[  
![vt_func_form_show](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_form_show_thumb-3.gif?width=572&height=461&name=vt_func_form_show_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_form_show-3.gif>)

### Writing the plugin

Now we need to glue all the components together by writing a **plugin_t** sub-class (a regular IDA Pro [script plugin](<http://www.hexblog.com/?p=120>)).  
[![vt_func_plg](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_func_plg_thumb-3.gif?width=525&height=671&name=vt_func_plg_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_func_plg-3.gif>)  
The run() method does the following:

  * Load a custom icon (the VT icon)
  * Create the form
  * Populate and display the form
  * Free the form
  * If requested by the user, create a regular (not embedded) chooser to show the results in IDA Pro (after the plugin has terminated)



The results shown in a chooser:  
[![vt_ui_pop_results](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_ui_pop_results_thumb-3.gif?width=661&height=418&name=vt_ui_pop_results_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_ui_pop_results-3.gif>)  
The results (but this time docked):  
[![vt_ui_docked_results](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/vt_ui_docked_results_thumb-3.gif?width=634&height=375&name=vt_ui_docked_results_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/vt_ui_docked_results-3.gif>)

### Using the VirusTotal IDA Pro plugin

Before using the plugin, you need to do the following things:

  * Install [simplejson](<http://pypi.python.org/pypi/simplejson/>) Python package in your system
  * Download and install IDAPython 1.5.1 (Unfortunately, idaapi.AskUsingForm() was mistakenly not compiled into IDAPython 1.5.0).Download the article [files](</ida_pro/files/virustotal_plugin.zip>) (_BboeVt_ module) and the _VirusTotal plugin_
  * Copy the _BboeVt_ module (from the article files) to your Python site-packages
  * Copy the _VirusTotal.py_ plugin to $IDA\plugins folder



Using the plugin:

  * Run IDA Pro and open a database
  * Invoke the VirusTotal plugin (the default hotkey is Alt-F8)
  * You may fetch a report using a file path (if you browse a file) or a file hash (By default the file path and then file hash will be populated from the currently opened database)
  * Double click on an result item to open the browser and read more about the given report



That’s it! Comments, questions or suggestions are welcome.

---

## 3. 

**Date:** Posted: Apr 20, 2011  
**Link:** [https://hex-rays.com/blog/challenging-job-for-software-developers](https://hex-rays.com/blog/challenging-job-for-software-developers)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Challenging job for software developers

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/challenging-job-for-software-developers>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/challenging-job-for-software-developers>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/challenging-job-for-software-developers>)

Ilfak Guilfanov ✦ Posted: Apr 20, 2011

![Challenging job for software developers](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We should permanently and prominently publish this ad on our site 🙂

We are looking for strong software engineers to join our team and participate in the development of unique software security tools. The candidates must know low-level details of modern software as well as high-level data structures and algorithms.

Requirements:

  * strong knowledge of C/C++ 
  * knowledge of the x86 assembler and unwillingness to use it in development 
  * cross platform development (Windows/Linux/Mac) is a plus 
  * knowing the graph theory and how compilers work is a plus 
  * ability and willingness to write secure yet fast code 
  * good problem solving and communication skills 



If you want a challenging job in a friendly environment, please apply by sending your resume to info@hex-rays.com  
Thanks!

---

## 4. 

**Date:** Posted: Feb 18, 2011  
**Link:** [https://hex-rays.com/blog/when-choosers-invade-forms-2](https://hex-rays.com/blog/when-choosers-invade-forms-2)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# When choosers invade forms

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/when-choosers-invade-forms-2>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/when-choosers-invade-forms-2>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/when-choosers-invade-forms-2>)

Daniel Pistelli ✦ Posted: Feb 18, 2011

![When choosers invade forms](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

With the upcoming IDA 6.1 it will be possible to create forms which host chooser controls. This feature will be available in the Qt and text version (not so in the VCL one).  


![](https://hex-rays.com/hubfs/Imported_Blog_Media/formchooser-3.gif)   
  
While this may not be important news, it is part of our effort to make the components of IDA Pro communicate even more among each other.   
The plugin sample which demonstrates this new feature is called **formchooser**. 
    
    
    static void idaapi run(int)
    {
      static const char form[] =
        "STARTITEM 0\n"
        "Form with choosers\n\n"
        "%/"
        "Select an item in the main chooser:\n"
        "\n"
        "
     
      
       \n\n" "
       
        \n" "\n"; chooser_info_t main_chi = { sizeof(chooser_info_t), CH_NOIDB, // flags (doesn't need an open database) 0, 0, // width, height NULL, //title NULL, // obj qnumber(header), // columns widths, icon_id, // icon 0, // deflt NULL, // popup_names main_choose_sizer, main_choose_getl, NULL, // del NULL, // ins NULL, // update NULL, // edit NULL, // enter NULL, // destroyer NULL // get_icon }; chooser_info_t aux_chi = main_chi; aux_chi.flags |= CH_MULTI; aux_chi.sizer = aux_choose_sizer; aux_chi.getl = aux_choose_getl; intvec_t main_sel, aux_sel; char str[MAXSTR] = {0}; // default selection for the main chooser main_sel.push_back(main_current_index); if ( AskUsingForm_c(form, modcb, &main_chi, &main_sel, &aux_chi, &aux_sel, str) > 0 ) { msg("Selection: %s\n", str); } }
       
      
     

The new tag introduced in the forms syntax is the letter **E**. As you can see, it is mostly a matter of filling the **chooser_info_t** structure with the same fields which you would pass to the **choose2** function. Other than this structure, a intvec_t class has to be passed for every chooser. The intvec_t will specify the initial selection and return the final one.  
For those of you who noticed the smiley icon and wonder what that is all about: it is now possible to load custom icons in idaq and to display them in the chooser. Three new APIs have been introduced for this task:
    
    
    int load_custom_icon(const char *file_name);
    int load_custom_icon(const void *ptr, unsigned int len, const char *format);
    void free_custom_icon(int icon_id);

The icon can be loaded from file or memory. In the latter case, it is necessary to specify the format (say “png” or “jpg”). The “load” functions return an icon id which can be used in the chooser. If the icon is not freed with free_custom_icon, it will be automatically released when IDA terminates.

---

## 5. 

**Date:** Posted: Jan 4, 2011  
**Link:** [https://hex-rays.com/blog/ida-qt-under-the-hood](https://hex-rays.com/blog/ida-qt-under-the-hood)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA & Qt: Under the hood

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-qt-under-the-hood>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-qt-under-the-hood>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-qt-under-the-hood>)

Hex-Rays ✦ Posted: Jan 4, 2011

![IDA & Qt: Under the hood](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Generally speaking most plugins for IDA can be written by using only the provided SDK. The API environment provided by IDA is vast and gives the plugin writer the capability to display graphical elements such as colored text views, graphs, forms and choosers.  
However, there are cases when this is not enough. In idag the developer could use the Windows/.NET environment to go beyond the limits of the IDA SDK. While this is still possible in idaq, it is not advised, as it binds the code of the plugin to Windows and forces idaq to switch from alien widgets to system windows (more about that later).  
Since accessing Qt from C++ requires setting up a development environment on every platform the developer wishes to deploy his plugin, one might take into consideration using [PySide](<http://www.hexblog.com/?p=229>) to access the Qt environment. The advantages of this approach are many. The first one is that the code once written will work on every platform without additional work. Moreover, there’s no need to recompile a plugin for every major Qt release deployed with idaq.  
That being said, there might be cases where the developer/company needs or prefers to access the Qt framework directly from C++ and that is what is going to be covered in this article.  


## Setting up the Qt environment

It is necessary to build the Qt environment, because IDA is shipped with a custom version of Qt which wraps its classes inside the **QT** namespace (we’ll see later why that is so).  
On Windows the same runtime has to be used. IDA 6.0, for instance, has been compiled with Visual C++ 2008. The express edition will do just fine.  
Open the Visual C++ x86 prompt, go to the Qt source directory and type:
    
    
    configure -debug-and-release -platform win32-msvc2008 -no-qt3support -qtnamespace QT

The same line on Linux will be:
    
    
    ./configure -debug-and-release -platform linux-g++ -no-qt3support -qtnamespace QT -prefix /home/daniel/Qt/4.6.3

And on OSX:
    
    
    ./configure -debug-and-release -cocoa -arch x86 -qtnamespace QT -prefix /Users/Shared/Qt/4.6.3

It should be noted that the Qt framework deployed with IDA 6.0 was linked against carbon (the configuration defaults to it). In the next release the framework will be linked against cocoa:
    
    
    ./configure -debug-and-release -cocoa -arch x86 -qtnamespace QT -prefix /Users/Shared/Qt/4.7.0

After the configuration process is over, the build process can begin. Type “nmake” on Windows and “make” on all other platforms. If the **prefix** parameter has been specified, the libraries have to be installed: type “sudo make -j1 install”.  
Optional: if you want to use Qt Creator as IDE, download & install the stand-alone version and add the path of the freshly compiled Qt framework in the settings. Additionally, to set up the CDB debugger, the debugging tools for x86 are required. You can find them inside the WDK: Debuggers\setup_x86.exe. It is also necessary to specify the path for the symbols in the settings.

## General guidelines

The **QT** namespace is necessary because of an unfortunate coincidence: the default prefix for many functions in the IDA SDK is “q”. Generally this is not a problem, since Qt is all about classes which begin with “Q”, there are some plain C lower-case functions in the Qt SDK which do, in fact, collide against some IDA SDK functions. The colliding functions are string functions such as: qstrlen, qstrdup, qstrcmp, etc.  
Thus, in the beginning of the development, it was decided to wrap the Qt classes inside the QT namespace. Fortunately, the Qt environment has foreseen this case and allows itself to be wrapped with a simple configuration parameter.  
In this scenario, the developer has to pay attention to mainly four things.  
1) The **QT** namespace has to be specified in the qmake project file (or as build parameter). In a qmake project it is sufficient to add the line:
    
    
    QT_NAMESPACE = QT

2) Forward declarations to Qt classes have to be wrapped by the namespace. Which means that:
    
    
    namespace Ui {
      class MyDialog;
    }
    class QTreeWidget;
    class QTableWidget;

becomes:
    
    
    QT_BEGIN_NAMESPACE
    namespace Ui {
      class MyDialog;
    }
    class QTreeWidget;
    class QTableWidget;
    QT_END_NAMESPACE

Pay attention that Qt Creator doesn’t add these delimiters when adding a QWidget/QDialog designer class to the project.  
3) From inside a Qt class it is possible to call a colliding function this way:
    
    
    len = ::qstrlen(str);

4) When calling **open_tform** the **FORM_QWIDGET** flag has to be specified. This flag has been introduced for backwards compatibility on Windows, in order to allow plugins to still obtain a native window handle when the flag is not specified. All new cross-platform plugins have to specify this flag.  
What happens behind the scenes is the following.  
The windows opened by the IDA process (more specifically the main window) should look like this:  


![](https://hex-rays.com/hubfs/Imported_Blog_Media/alien_widgets-3.jpg)   
There are no children, because by default Qt uses alien widgets. These widgets exist in the context of Qt, but are not real system windows.   
However, when a plugin doesn’t specify **FORM_QWIDGET** , the **winId()** method is called on the created widget. This method returns a valid system handle and forces all alien widgets to become native widgets.   
![](https://hex-rays.com/hubfs/Imported_Blog_Media/native_widgets-3.jpg)

## The Qt project sample

Although the SDK shipped with IDA 6.0 already provided a sample of how to access Qt from C++, it is minimalistic. That’s why a new sample plugin called “qproject” has been added. This plugin features a fully commented qmake project (which can also be used to obtain compiler specific project files if the developer wishes). It’s based on the classic Qt SDK example “elastic nodes”.  


![](https://hex-rays.com/hubfs/Imported_Blog_Media/qproject-3.jpg)   
To use this project for your own plugins, just replace the **TARGET** name inside “qproject.pro”, remove all source/header files apart of “qproject.cpp”, edit the configuration settings contained in “qproject.cpp” like for all other IDA plugins and finally replace this line with the class name of your own widget: 
    
    
    GraphWidget *userWidget = new GraphWidget();

Build with Qt Creator or type:
    
    
    qmake theprojectname.pro
    nmake/make

## OSX adjustments

While on Linux it is sufficient to specify the rpath to load the correct dependencies, there’s additional work to do on OSX.  
To list the dependencies for your plugin, type:
    
    
    otool -L pluginname.pmc

The output will be something like:
    
    
    /Users/Shared/Qt/4.6.3/lib/QtGui.framework/Versions/4/QtGui (compatibility version 4.6.0, current version 4.6.3)
    /Users/Shared/Qt/4.6.3/lib/QtCore.framework/Versions/4/QtCore (compatibility version 4.6.0, current version 4.6.3)
    /usr/lib/libstdc++.6.dylib (compatibility version 7.0.0, current version 7.9.0)
    /usr/lib/libgcc_s.1.dylib (compatibility version 1.0.0, current version 103.0.0)
    /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 125.2.1)

It is necessary to change the path of the dependencies through the **install_name_tool** utility. For instance:
    
    
    install_name_tool -change /Users/Shared/Qt/4.6.3/lib/QtGui.framework/Versions/4/QtGui @executable_path/../Frameworks/QtGui.framework/Versions/4/QtGui pluginname.pmc

The executable_path is a variable specifying the initial search path.  
You can invoke the **install_name_tool** utility as a post link action:
    
    
    mac:QMAKE_POST_LINK += install_name_tool -change /idapathsample/libida$${SUFF64}.dylib @executable_path/libida$${SUFF64}.dylib $(TARGET)

## IDA SDK controls

While almost the entire IDA SDK is agnostic in regards to Qt, there are a few exceptions:  
1) the CLI (command line interpreter)  
2) the custom viewer  
These controls offer now a Qt awareness option: the **CLIF_QT_AWARE** flag for the CLI and the **set_custom_viewer_qt_aware** function for the custom viewer.  
When these controls are set as Qt aware, they will return the actual Qt key and modifier codes to their key press notification callbacks. Vice versa, when they’re not Qt aware, the Win32/VCL key and shift codes will be passed, even on non-Win32 platforms. Check out “kernwin.hpp” for the Win32-compatible defined key codes (they all start with “IK_” instead of “VK_”). This basic emulation allows non-Qt graphical plugins to be recompiled for other platforms by just changing the key code prefix.

## Conclusions

This should be all. In case I’ve forgotten something, let me know and it will be added.

---

## 6. 

**Date:** Posted: Dec 2, 2010  
**Link:** [https://hex-rays.com/blog/ida-pro-6-licenses](https://hex-rays.com/blog/ida-pro-6-licenses)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 6 licenses

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-6-licenses>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-6-licenses>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-6-licenses>)

Ilfak Guilfanov ✦ Posted: Dec 2, 2010

![IDA Pro 6 licenses](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

As many of you already know, IDA6 copies ship separately for Windows/Linux/Mac. Before we were giving the Linux/Mac versions for free because there was no GUI for them. Now we have full fledged GUIs for all platforms (and our development/techsupport costs increased because of that), so we separated the licenses. We could simply have increased the price of the whole package but since the absolute majority of our users stick to one platform, it is fairer to separate licenses. This way only the customers who really use multiple platforms pay extra. We believe this is the fairest solution for everyone.  
  
Some of your may ask, why, did your development and support costs really increase? You are using Qt and need only to recompile the same code for Linux and Mac, aren’t you?  
While in theory this is how it works and Qt allows us to write code once and use it on multiple platforms, in practice it means lots of additional work.  
First, Qt behaves differently on different platforms. We already have lots of #ifdef’ed code to address this or that platform specific issue. Mac OS X is notorious for having tons of special cases. To start with, it does not have the ‘insert’ key 🙂 More seriously, there are more important issues with Mac and they will take time to fix.  
Second, we have more platform specific support questions from users, not only Qt related, but general. We spend more time trying to reproduce bugs. For example: take dual monitor mac machine, open a ida listing window, move it to the second monitor; hover the mouse: the hint will be displayed on the wrong monitor. Another: on linux, bochs emulator works slower than expected. Yet another: trying to disassembly an avr input file on mac produces error messages in the config files. One more: on Linux, opening the breakpoint list with Ctrl-Alt-B causes a crash on some distros. I’m not even talking about debugging…  
Third, we have lots of new code. Moving to Qt meant that we had to rewrite a substantial part of the user interface. Naturally, not everything works as before. Our users are very demanding and they want every minute detail to be addressed: http://www.hex-rays.com/forum/viewtopic.php?f=6&t=2680 (for the readers without access to the forum, this is a thread about using Alt-K to close dialog boxes; apparently some our users miss it despite of easier Cltr-Enter).  
Fourth, we will have solve other inconveniences experienced by our Unix users. As it was justly mentioned, the auxiliary utilities will have to be rebuilt for every release not only for Windows, but also for Linux and Mac. Also, we will have to address the PDB/WinDbg unavailability under Unix. I’m not sure about WinDbg, but we will come up with a solution for PDB files (a windows based pdb server?)  
Fifth, we will have to test IDA on all platforms. While testing the kernel is already done and it is a routine, testing GUI is difficult to automate. This means additional work for us in the future, unavoidably.  
It is quite inconvenient, but currently every change we make into the user interface is done three times:  
– in the new idaq interface  
– in the old idag interface (we will release it at least one more time, so we have to keep it functional)  
– in the old text interface (yes, we have some customers who still use it and it is a good idea to keep it around; we can not really kill it)  
We invested lots of time and efforts into idaq: Daniel worked on it full time nine months. And he is a brilliant programmer who knows how to do things, yet there is a lot to do – just to achieve the same level of comfort as with idag.  
All the above can not be done for free, sorry. While we could simply increase the package price, it wouldn’t be fair for the users who are happy with MS Windows. So we split the licenses. For most of our users the price stays the same. Only the ones who run IDA on multiple platforms pay extra.  
We took this option because it keeps the price low for most our users. It looks as the fairest option to us.  
So, we do not offer Linux/Mac versions for free anymore. Sorry.  
One more interesting detail, so give you an idea of the situation. As it turned out, IDA Pro 5.7 for Mac was completely broken as shipped. In other words, nobody could run it because it immediately displayed a fatal error about a missing import. Guess how many complaints we received about this? I do not remember exactly, but it was a low number like 3 or 5. Naturally, we sent them the corrected version immediately. We did not fix the distributed version since it was easier for us to send a copy a few times rather than to change the build server. This is incomparable to what we have today, when we receive lots of bug reports is about Linux or Mac. Fortunately, most of them are about small details but they require us to find the correct version of the operating system, try to find out what went wrong, etc. At the same time the absolute majority of our users use MS Windows (around 5% of ida are from Linux copies and even less than that are from Mac copies).  
As one of our customers correctly put it “you spoiled us” 🙂

---

## 7. 

**Date:** Posted: Oct 30, 2010  
**Link:** [https://hex-rays.com/blog/ida-pro-python-and-qt](https://hex-rays.com/blog/ida-pro-python-and-qt)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro, Python and Qt

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-python-and-qt>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-python-and-qt>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-python-and-qt>)

Elias Bachaalany ✦ Posted: Oct 30, 2010

![IDA Pro, Python and Qt](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

[IDA Pro 6.0](<http://www.hex-rays.com/idapro/60/index.html>) implements a cross-platform UI with the use of Qt framework. The good thing about it is that plugin writers can also develop cross-platform UI directly with Qt. But what about script writers?  
In this blog post we are going to illustrate how to use [PySide](<http://www.pyside.org/>) to create UI interfaces for IDA Pro using IDAPython.  
[![ipq_intro](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ipq_intro_thumb-3.gif?width=657&height=572&name=ipq_intro_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/ipq_intro-3.gif>)  


## Background

In previous versions of IDA Pro it was possible to create custom UIs with the **create_tform()** /**display_tform()** APIs, nonetheless the code was platform specific. On MS Windows, the programmer receives an HWND of the parent form and then populates it with custom controls and then handles the window messages from a custom WindowProc().  
Since previously only an MS Windows UI existed, users could not create complex UIs on other platforms and were bound to use IDA Pro SDK/form related functions such as **AskUsingForm()**.  
With IDA Pro 6.0, C++ plugin writers can directly use the Qt SDK to develop a cross-platform UI. Please refer to the **qwindow** same in the IDA Pro SDK.  
Script writers can also use Python Qt bindings to achieve the same result.

## Python bindings for the Qt framework

We evaluated both [PySide](<http://www.pyside.org/>) and [PyQt](<http://www.riverbankcomputing.co.uk/software/pyqt/intro>) and found that both bindings work fine with IDA Pro 6.0 (We had to compile them with –DQT_NAMESPACE=QT and had to add one method to pass a QWidget* from C++ to Python).  
While PyQt is much more mature and adopted by many users, we opted for PySide which works equally well and has a less restrictive license.

## Writing a Hello world UI using IDAPython and PySide

In order to write a UI from IDAPython, you have to subclass the **idaapi.PluginForm** class. This class which essentially wraps the create_tform()/display_tform() and provides some helper functions (such as passing to Python a QWidget* that can be used by PySide as the parent widget).  
A sample code will make things clearer:  
[![ipq_hello_code](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ipq_hello_code_thumb-3.gif?width=529&height=516&name=ipq_hello_code_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/ipq_hello_code-3.gif>)  
After running this script, we get this form:  
[![ipq_hello](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/ipq_hello_thumb-3.gif?width=631&height=277&name=ipq_hello_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/ipq_hello-3.gif>)  
And of course, the form can be docked as any other built-in form. While this example is so simple, users can now create much more elaborate and complex UIs. No surprise about it but now plugin development using scripts (IDAPython) is becoming more interesting than before.  
We plan to release, soon, an IDA Pro 6.0 update (bug fix release) along with IDAPython 1.4.3 (and a custom build of PySide) which are needed to run the [sample](</ida_pro/files/ImportExportViewer.py>) code used in this post.  
And lastly, for those interested to learn more about IDA Pro and its latest features you can benefit from the IDA Pro [training](<http://www.hex-rays.com/idapro/training/>) taking place in December.  
We hope that you found this blog post useful. All comments and suggestions are welcome.

---

## 8. 

**Date:** Posted: Oct 9, 2010  
**Link:** [https://hex-rays.com/blog/calculating-api-hashes-with-ida-pro](https://hex-rays.com/blog/calculating-api-hashes-with-ida-pro)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Calculating API hashes with IDA Pro

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/calculating-api-hashes-with-ida-pro>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/calculating-api-hashes-with-ida-pro>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/calculating-api-hashes-with-ida-pro>)

Elias Bachaalany ✦ Posted: Oct 9, 2010

![Calculating API hashes with IDA Pro](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

Many times when debugging malware you discover that the malware does not import any function, replaces API names by hashes and tries to resolve the addresses by looking up which API name has the desired hash!  
In this blog post we are going to demonstrate how to use IDA Pro to solve this problem and uncover all API hashes.  
[![hash_calc](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hash_calc_thumb-3.gif?width=598&height=317&name=hash_calc_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/hash_calc-3.gif>)  


# Background

To illustrate the problem, imagine this piece of code:  
[![example1](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/example1_thumb-3.gif?width=340&height=171&name=example1_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/example1-3.gif>)  
After tracing this code, we discovered that the call at 0x405DA4 should be renamed as ‘call_by_hash’ because it takes the first argument as the HASH of the API to be called followed by the actual parameters to the desired function.  
The **call_by_hash()** starts by reading the PEB from pointer from the TIB, locates the LDR_MODULE structure of kernel32.dll, parses its PE structure and finally retrieves the IMAGE_EXPORT_DIRECTORY in order to get both the **AddressOfFunctions** and **AddressOfNames** fields:  
[![resolve_by_hash](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/resolve_by_hash_thumb-3.gif?width=621&height=832&name=resolve_by_hash_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/resolve_by_hash-3.gif>)  
With this information, the malware will then iterate through all the exported names trying to match the hash of the given exported name against the passed hash value using the **calc_hash()** function:  
[![calc_hash](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calc_hash_thumb-3.gif?width=604&height=722&name=calc_hash_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/calc_hash-3.gif>)  
We notice that this function is self-contained and is easy to extract out of the code, reassemble and use in an external program to calculate the hashes. Instead of going through this hassle, we will write an IDAPython script that will make use of this function to calculate the hashes of all the exported names in this debugging session.

# Writing the script

Before writing the script, let us break down into smaller steps:

  1. Extract the **calc_hash** function body and make it available for use from our script
  2. Find all exported names
  3. Pass each name to the calc_hash() and remember the result
  4. Display all the hashes in a nice chooser window



### Using calc_hash() from the IDAPython script

Since the calc_hash() is self-contained, we can directly extract its body from the database and use [ctypes](<http://docs.python.org/library/ctypes.html>) to call it:  
[![calc_by_hash](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/calc_by_hash_thumb-3.gif?width=593&height=260&name=calc_by_hash_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/calc_by_hash-3.gif>)  
(Remember to free the memory when done)  
Or we can use the [Appcall](<http://www.hexblog.com/?p=113>) mechanism to do just the same thing:  
[![appcall](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/appcall_thumb-3.gif?width=533&height=142&name=appcall_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/appcall-3.gif>)  
Because we will be calling **calc_hash()** at least 9000 times, we opt for the first solution because it is much much faster (Gladly we have another solution because it is not always a luxury to have a self-contained function that we can map into the Python host and execute as is).

### Finding exported names and calculating their hashes

Everytime a debugging session starts, the IDA Pro debugger asks the debugger module to provide a set of debug names. The debug names are essentially the exported names of all loaded modules. To retrieve this list programmatically, we resort to using **idaapi.get_debug_names()** :  
[![get_dn](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/get_dn_thumb-3.gif?width=624&height=260&name=get_dn_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/get_dn-3.gif>)  
Each returned debug name has the following format ‘Modulename**_** ApiName’. This is why we had to split (after the first underscore character) the returned debug name.  
Now to calculate the hashes of all the names, it is sufficient to loop through the list like this:  
[![get_hashes](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/get_hashes_thumb-3.gif?width=444&height=178&name=get_hashes_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/get_hashes-3.gif>)

### Putting it all together

Now that we solved all the issues, we will present the results in a nice chooser and provide two facilities: one to import the hashes into IDA Pro’s enumeration window and the other to export the API hash list to a text file for external processing.  
Writing a Chooser window is trivial (please refer to the source code or the ex_choose2 sample in the IDAPython package).  
This is how the end result looks like:  
[![hashcalc_menu](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hashcalc_menu_thumb-3.gif?width=629&height=317&name=hashcalc_menu_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/hashcalc_menu-3.gif>)  
The chooser window displays the module name, the hash value and the API name. A popup menu is created so that one can either export the list to a text file or import the values as enums:  
[![hashcalc_enums](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hashcalc_enums_thumb-3.gif?width=522&height=127&name=hashcalc_enums_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/hashcalc_enums-3.gif>)

# Using the script

Now to use the script, launch the debugger, trace until/locate the function that calculates the hash and name it as **calc_func**.  
If the function is not self-contained you need to prototype the function properly (by pressing ‘y’ in IDA Pro) and use the Appcall method instead of the ctypes method.  
If you want to enhance the disassembly listing, then after you run the script, select “Import as Enum”.  
Later when you encounter something like this:  
[![ex-b4](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/exb4_thumb-3.gif?width=353&height=137&name=exb4_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/exb4-3.gif>)  
You may simply press ‘m’ and select the appropriate hash constant, for example:  
[![apply_enum](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/apply_enum_thumb-3.gif?width=551&height=185&name=apply_enum_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/apply_enum-3.gif>)  
And after that you will get something like this:  
[![ex-a4](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/exa4_thumb-3.gif?width=348&height=118&name=exa4_thumb-3.gif)](<https://hex-rays.com/hubfs/Imported_Blog_Media/exa4-3.gif>)  
Hope you found this blog post useful.  
This script has been tested with [IDA Pro 6.0](<http://www.hex-rays.com/idapro/60/index.html>) and may be downloaded from [here](</ida_pro/files/ApiHash.py>).

Your comments and suggestions are welcome.

---

## 9. 

**Date:** Posted: Jul 19, 2010  
**Link:** [https://hex-rays.com/blog/implementing-command-completion-for-idapython](https://hex-rays.com/blog/implementing-command-completion-for-idapython)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Implementing command completion for IDAPython

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/implementing-command-completion-for-idapython>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/implementing-command-completion-for-idapython>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/implementing-command-completion-for-idapython>)

Elias Bachaalany ✦ Posted: Jul 19, 2010

![Implementing command completion for IDAPython](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

In this blog post we are going to illustrate how to use the command line interpreter (CLI) interface from Python and how to write a basic command completion functionality for the Python CLI.  


## Understanding the CLI structure

IDA Pro SDK allows programmers to implement command line interpreters with the use of the **cli_t** structure:
    
    
    struct cli_t
    {
      size_t size;                  // Size of this structure
      int32 flags;                  // Feature bits
      const char *sname;            // Short name (displayed on the button)
      const char *lname;            // Long name (displayed in the menu)
      const char *hint;             // Hint for the input line
      // callback: the user pressed Enter
      bool (idaapi *execute_line)(const char *line);
      // callback: the user pressed Tab (optional)
      bool (idaapi *complete_line)(
            qstring *completion,
            const char *prefix,
            int n,
            const char *line,
            int prefix_start);
      // callback: a keyboard key has been pressed (optional)
      bool (idaapi *keydown)(
            qstring *line,
            int *p_x,
            int *p_sellen,
            int *vk_key,
            int shift);
    };

For example, the IDAPython plugin defines its CLI like this:
    
    
    static const cli_t cli_python =
    {
        sizeof(cli_t),
        0,
        "Python",
        "Python - IDAPython plugin",
        "Enter any Python expression",
        IDAPython_cli_execute_line,
        NULL, // No completion support
        NULL // No keydown support
    };

And registers/unregisters it with:
    
    
    void install_command_interpreter(const cli_t *cp);
    void remove_command_interpreter(const cli_t *cp);

## The CLI in Python

The CLI functionality has been added in [IDAPython 1.4.1](<http://code.google.com/p/idapython/source/detail?r=315>). Let us suppose that we want to reimplement the Python CLI in Python (rather than in C++), we would do it like this:
    
    
    class pycli_t(idaapi.cli_t):
        flags = 0
        sname = "PyPython"
        lname = "Python - PyCLI"
        hint  = "Enter any Python statement"
        def OnExecuteLine(self, line):
            """
            The user pressed Enter.
            @param line: typed line(s)
            @return Boolean: True-executed line, False-ask for more lines
            """
            try:
                exec(line, globals(), globals())
            except Exception, e:
                print str(e) + "\n" + traceback.format_exc()
            return True

Summary:

  1. Subclass **idaapi.cli_t**
  2. Define the CLI short name, long name and hint
  3. Declare the OnExecuteLine handler and use Python’s exec() to execute a line



To try this code, simply instantiate a CLI and register it:
    
    
    pycli = pycli_t()
    pycli.register()

And later to unregister the CLI:
    
    
    pycli.unregister()

### Adding command completion

In order to add command completion, we need to implement the OnCompleteLine() callback:
    
    
    class cli_t:
        def OnCompleteLine(self, prefix, n, line, prefix_start):
            """
            The user pressed Tab. Find a completion number N for prefix PREFIX
            This callback is optional.
            @param prefix: Line prefix at prefix_start (string)
            @param n: completion number (int)
            @param line: the current line (string)
            @param prefix_start: the index where PREFIX starts in LINE (int)
            @return: None or a String with the completion value
            """
            print "OnCompleteLine: pfx=%s n=%d line=%s pfx_st=%d" % (prefix, n, line, prefix_start)
            return None

This callback is invoked everytime the user presses TAB in the CLI. The callback receives:

  * prefix: the prefix at the cursor position
  * n: the completion number. If this callback succeeded the first time (when n == 0), then IDA will call this callback again with n=1 (and so on) asking for the next possible completion value
  * line: the whole line
  * prefix_start: the index of the prefix in the line



For example typing “os.path.ex” and pressing TAB would call:
    
    
    OnCompleteLine(prefix="ex", n=0, line="print os.path.ex", prefix_start=14)

For demonstration purposes, here is a very simple algorithm to guess what could complete the “os.path.ex” expression:

  1. Parse the identifier: Since IDA passes the line, prefix and the prefix start index, we need to get the whole expression including the prefix (we need “os.path.ex”).  
This can be done by scanning backwards from prefix_start until we encounter a non identifier character:
         
         def parse_identifier(line, prefix, prefix_start):
             id_start = prefix_start
             while id_start > 0:
                 ch = line[id_start]
                 if not ch.isalpha() and ch != '.' and ch != '_':
                     id_start += 1
                     break
                 id_start -= 1
             return line[id_start:prefix_start + len(prefix)]

  2. Fetch the attributes with dir(): Given the full identifier name (example “os.path.ex”), we split the name by ‘.’ and use a getattr() loop starting from the __main__ module up to the last split value: 
         
         def get_completion(id, prefix):
             try:
                 parts = id.split('.')
                 m = sys.modules['__main__']
                 for i in xrange(0, len(parts)-1):
                     m = getattr(m, parts[i])
             except Exception, e:
                 return None
             else:
                 completion = [x for x in dir(m) if x.startswith(prefix)]
                 return completion if len(completion) else None

  3. Implementing OnCompleteLine(): We parse the identifier and get a list of possible completion values only if n==0, otherwise we return the next possible value in the list we previously built: 
         
         def OnCompleteLine(self, prefix, n, line, prefix_start):
             # new search?
             if n == 0:
                 self.n = n
                 id = self.parse_identifier(line, prefix, prefix_start)
                 self.completion = self.get_completion(id, prefix)
             return None if (self.completion is None) or (n >= len(self.completion)) else \
                      self.completion[n]




With this approach we could complete something like this:
    
    
    f = open('somefile.txt', 'r')
    f.re^TAB => f.read, f.readinto, f.readline or f.readlines

Or:
    
    
    print idau^TAB.GetRe^TAB => print idautils.GetRegisterList

The sample script can be downloaded from [here](</ida_pro/files/pycli.py>) and a win32 build of the latest IDAPython plugin (with completion integrated in the plugin) can be downloaded from [here](<http://code.google.com/p/idapython/downloads/detail?name=idapython-1.4.1_ida5.7_py2.5_win32.zip>).

---

