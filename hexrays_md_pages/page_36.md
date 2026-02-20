# Hex-Rays Blog – Page 36

**Archived:** 2026-02-20 12:21
**URL:** https://hex-rays.com/blog/page/36

---

## 1. 

**Date:** Posted: Jul 28, 2020  
**Link:** [https://hex-rays.com/blog/ida-pro-7-5-sp2-released](https://hex-rays.com/blog/ida-pro-7-5-sp2-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 7.5 SP2 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-7-5-sp2-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-7-5-sp2-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-7-5-sp2-released>)

Fabrice Ovidio ✦ Posted: Jul 28, 2020

![IDA Pro 7.5 SP2 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Hex-Rays announces the release of Service Pack 2 (SP2) for IDA Pro 7.5.

SP2 was introduced due to the major changes in Apple’s new versions of iOS and MacOS and their move to Apple Silicon. Hex-Rays team works constantly to improve the IDA experience and acts to ensure that IDA addresses its users’ feedback and properly responds to the fast-paced changes of the industry.

We thank you for your support and hope you enjoy IDA 7.5 SP2!

**What is new and just added to SP2:**

  * the new MH_FILESET kernelcache format from macOS 11 is now fully supported;
  * handle LC_DYLD_CHAINED_FIXUPS for binaries compiled with Xcode 12;
  * a workaround for slowdowns when loading dyldcache modules on macOS Catalina;
  * type libraries for MacOSX11.0.sdk and iPhoneOS14.0.sdk;



**SP2 improves:**

  * the analysis of dyldcache files from macOS11/iOS14;
  * the analysis of Objective-C metadata in binaries compiled with XCode 12 (specifically __objc_methlist sections);
  * minor improvements to debugging on macOS11/iOS14 (no ARM64 macOS11 debugging support yet);



### How to request the new versions

As usual, the new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to support@hex-rays.com.

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist __](</products/ida/news/7_5sp2/>)[Get a quote __](</cgi-bin/quote.cgi>)

---

## 2. 

**Date:** Posted: Jun 23, 2020  
**Link:** [https://hex-rays.com/blog/updates-to-ios-debugger-tutorial](https://hex-rays.com/blog/updates-to-ios-debugger-tutorial)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Mac](<https://hex-rays.com/blog/tag/mac>) [debugger](<https://hex-rays.com/blog/tag/debugger>) [iOS](<https://hex-rays.com/blog/tag/ios>)

# Updates to iOS Debugger Tutorial

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/updates-to-ios-debugger-tutorial>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/updates-to-ios-debugger-tutorial>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/updates-to-ios-debugger-tutorial>)

Troy Bowman ✦ Posted: Jun 23, 2020

![Updates to iOS Debugger Tutorial](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

We have updated our [iOS Debugging Tutorial](</wp-content/static/tutorials/ios_debugger_primer2/ios_debugger_primer2.pdf>). It has some new sections that should be of particular interest:

  * **“Debugging the DYLD Shared Cache”** discusses how to combine IDA’s incremental dyldcache loading functionality with the iOS Debugger.
  * **“Debugging System Applications”** is a concrete example of how to use IDA to debug an iOS system daemon on a jailbroken device.



We hope you will find these new examples engaging and useful!

Downloads

[iOS Debugging Tutorial __](</wp-content/static/tutorials/ios_debugger_primer2/ios_debugger_primer2.pdf>)

---

## 3. 

**Date:** Posted: Jun 19, 2020  
**Link:** [https://hex-rays.com/blog/ida-pro-7-5-sp1-released](https://hex-rays.com/blog/ida-pro-7-5-sp1-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 7.5 SP1 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-7-5-sp1-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-7-5-sp1-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-7-5-sp1-released>)

Fabrice Ovidio ✦ Posted: Jun 19, 2020

![IDA Pro 7.5 SP1 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Hex-Rays announces the release of Service Pack 1 (SP1) for IDA Pro 7.5.

We recently launched IDA Pro v7.5 and IDA Home, Hex-Rays’ latest, affordable professional-grade solution tailored for reverse engineering hobbyists. We are glad to announce today the release of IDA 7.5 SP1. This service pack contains enhancements and fixes to the 7.5 version, partly thanks to valuable feedback from IDA users.

IDA 7.5 SP1 is designed to improve user experience especially for newly released features such as the tree-like folder view function and the MIPS decompiler.

**We improved:**

  * the decompilation of various MIPS files, in particular with regards to MIPS16 and Micromips;
  * the behavior of folder views during debugging.



**Fixes**

  * this update fixes several potential crash situations as well as minor errors identified internally and by our users since the launch of IDA 7.5.
  * we also fixed various minor issues related to the new IDA Home.



### How to request the new versions

As usual, the new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively you can [submit your ida.key](<https://www.hex-rays.com/updida/>) and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially today after the release. Please be patient.

In case of problems, do not hesitate to send us a message. Sometimes we need your help and cannot proceed without it. Just wait an hour or so, and if you do not hear from us, send a message to support@hex-rays.com.

### Has your support plan expired?

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist __](</products/ida/news/7_5sp1/>)[Get a quote __](</cgi-bin/quote.cgi>)

---

## 4. 

**Date:** Posted: May 25, 2020  
**Link:** [https://hex-rays.com/blog/ida-7-5-qt-5-6-3-configure-options-patch](https://hex-rays.com/blog/ida-7-5-qt-5-6-3-configure-options-patch)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [Qt](<https://hex-rays.com/blog/tag/qt>)

# IDA 7.5: Qt 5.6.3 configure options & patch

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-7-5-qt-5-6-3-configure-options-patch>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-7-5-qt-5-6-3-configure-options-patch>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-7-5-qt-5-6-3-configure-options-patch>)

Fabrice Ovidio ✦ Posted: May 25, 2020

![IDA 7.5: Qt 5.6.3 configure options & patch](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

A handful of our users have already requested information regarding the Qt 5.6.3 build, that is shipped with IDA 7.5.

### Configure options

Here are the options that were used to build the libraries on:

  * Windows: `...\5.6.3\configure.bat "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "win32-msvc2015" "-opengl" "desktop" "-prefix" "C:/Qt/5.6.3-x64"`
    * Note that you will have to build with Visual Studio 2015 to obtain compatible libs
  * Linux: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "linux-g++-64" "-developer-build" "-fontconfig" "-qt-freetype" "-qt-libpng" "-glib" "-qt-xcb" "-dbus" "-qt-sql-sqlite" "-gtkstyle" "-prefix" "/usr/local/Qt/5.6.3-x64"`
  * Mac OSX: `.../5.6.3/configure "-nomake" "tests" "-qtnamespace" "QT" "-confirm-license" "-accessibility" "-opensource" "-force-debug-info" "-platform" "macx-clang" "-debug-and-release" "-fontconfig" "-qt-freetype" "-qt-libpng" "-qt-sql-sqlite" "-prefix" "/Users/Shared/Qt/5.6.3-x64"`



### patch

In addition to the specific configure options, the Qt build that ships with IDA includes [the following patch](</wp-content/uploads/2020/05/qt-5.6.3_full-IDA75.patch_.zip>). You should therefore apply it to your own Qt 5.6.3 sources before compiling, in order to obtain similar binaries (`patch -p 1 < path/to/qt-5_6_3_full-IDA75.patch`)

Note that this patch should work without any modification, against the 5.6.3 release as found [there](<https://download.qt.io/archive/qt>). You may have to fiddle with it, if your Qt 5.6.3 sources come from somewhere else.

Downloads

[qt-5.6.3_full-IDA75.patch_.zip](</wp-content/uploads/2020/05/qt-5.6.3_full-IDA75.patch_.zip>)

---

## 5. 

**Date:** Posted: May 20, 2020  
**Link:** [https://hex-rays.com/blog/introducing-ida-home](https://hex-rays.com/blog/introducing-ida-home)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Introducing IDA Home

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-ida-home>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-ida-home>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-ida-home>)

Fabrice Ovidio ✦ Posted: May 20, 2020

![Introducing IDA Home](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Hex-Rays announces the release of IDA Home.

It is not often that we release a brand new product, so pardon our excitement.

We used all the experience gained throughout the years to propose hobbyists a solution that combines rapidity, reliability with the levels of quality and responsiveness of support that any professional reverse engineers should expect.

[IDA Home](</products/idahome>) is available in 5 different editions covering the most common processor families and gives access to the Lumina server. 

#### IDA Home’s main features:

  * __Ability to analyze both 32-bit and 64-bit applications
  * __Powerful IDAPython scripting with Python 3 support is included
  * __Local or gdbserver debugger included
  * __One processor family of choice from the most common processors:  
PC ARM M68K MIPS PPC
  * __Annual subscription
  * __Named license only
  * __Access to Lumina server
  * __For the price of $365 / year



[More details __](</products/idahome/>)[Get a quote __](</cgi-bin/quote.cgi>)

![IDA Home illustration](https://hex-rays.com/hubfs/Imported_Blog_Media/idahome_fff-3.png)

---

## 6. 

**Date:** Posted: May 20, 2020  
**Link:** [https://hex-rays.com/blog/ida-pro-7-5-released](https://hex-rays.com/blog/ida-pro-7-5-released)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA Pro 7.5 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-pro-7-5-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-pro-7-5-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-pro-7-5-released>)

Fabrice Ovidio ✦ Posted: May 20, 2020

![IDA Pro 7.5 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

Hex-Rays announces the release of IDA Pro 7.5!

[IDA Pro](</products/ida/>) is certainly the fastest and most reliable software solution to support professionals in their reverse-engineering work. Version 7.5 has been developed to improve the IDA experience further. It notably introduces the following features:

  * __Tree folder structure: you can now organize your work in a hierarchical tree structure and gain more efficiency
  * __MIPS Decompiler: A new decompiler for MIPS is now available
  * __Lumina: MIPS and PPC processors are now also available in Lumina
  * __Debugger: coverage extended to 4 additional processors



A lot of work has taken place since the previous release of IDA. Below is quick visual overview of the number of significant changes between 7.4SP1 and 7.5. and cumulatively since version 6.0.

#### Processor module

This release

14 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

347 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### File formats

This release

8 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

254 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### Installer

This release

1 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

7 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### Debugger

This release

6 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

162 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### Kernel / Misc.

This release

3 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

211 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### User interface

This release

11 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

241 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### Scripts / SDK

This release

17 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

339 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### Decompilers

This release

16 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

96 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

#### Bugfixes

This release

131 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

Since v6.0

2593 

![](https://static.hsstatic.net/BlogImporterAssetsUI/ex/missing-image.png)

[Release highlights and complete changelist __](</products/ida/news/7_5/>)[Get a quote __](</cgi-bin/quote.cgi>)

---

## 7. 

**Date:** Posted: Apr 23, 2020  
**Link:** [https://hex-rays.com/blog/tiny-microcode-optimizer](https://hex-rays.com/blog/tiny-microcode-optimizer)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Tiny microcode optimizer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/tiny-microcode-optimizer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/tiny-microcode-optimizer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/tiny-microcode-optimizer>)

Ilfak Guilfanov ✦ Posted: Apr 23, 2020

![Tiny microcode optimizer](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

It is not a surprise to hear the IDA and Decompiler cannot handle all possible cases and eventually fail to recognize a construct, optimize an expression and represent it in its simplest form. It is perfectly understandable — nobody has resources to handle everything. This is why we publish a rich API that can be used to enhance the analysis and add your own optimization rules.

Let us consider the decompiler. Currently, we focus on the compiler-generated code because hand-crafted code can use literally infinite number of tricks to confuse an automatic tool, not to mention seasoned reverses. Nevertheless, some users still want our decompiler to handle some assembler constructs that are usually not used by a compiler. To be concrete, let us take the following example:
    
    
    x | ~x

This expression always evaluates to `-1` (all bits set) but the decompiler does not know about this fact and does not have a dedicated optimization rule for it. However, it is pretty easy to implement a very short plugin for that. The core of the plugin will take just a few lines to carry out the optimization:
    
    
    if ins.opcode == m_or and ins.r.is_insn(m_bnot) and ins.l == ins.r.d.l:
       if not ins.l.has_side_effects(): # avoid destroying side effects
           ins.opcode = m_mov
           ins.l.make_number(-1, ins.r.size)
           ins.r = mop_t()
           self.cnt = self.cnt + 1 # number of changes we made

We check if the current instruction is an `OR` of an operand with its binary negation. If so, we check for the side effects because we do not want to remove function calls, for example. If everything is fine, we replace `x|~x` with `-1` and increment the number of changes that we made to the microcode. 

The rest is just boilerplate code. The full text of the plugin and a sample database are available below.

We hope this very short post and the tiny plugin will inspire you to write more complex optimization rules.

Downloads

[be_ornot_be.py](</wp-content/uploads/2020/04/be_ornot_be.py>)

[be_ornot_be.idb](</wp-content/uploads/2020/04/be_ornot_be.idb>)

Or get both, zipped:  
[be_ornot_be.zip](</wp-content/uploads/2020/04/be_ornot_be.zip>)

---

## 8. 

**Date:** Posted: Mar 5, 2020  
**Link:** [https://hex-rays.com/blog/registration-for-the-2020-european-ida-training-course-is-open](https://hex-rays.com/blog/registration-for-the-2020-european-ida-training-course-is-open)

[Back](<https://hex-rays.com/blog>)

[Training](<https://hex-rays.com/blog/tag/training>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Registration for the 2020 European IDA training course is open

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/registration-for-the-2020-european-ida-training-course-is-open>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/registration-for-the-2020-european-ida-training-course-is-open>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/registration-for-the-2020-european-ida-training-course-is-open>)

Fabrice Ovidio ✦ Posted: Mar 5, 2020

![Registration for the 2020 European IDA training course is open](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

The [2020 European IDA training course](</products/ida/training/>) will take place in Liège, Belgium on December 14–18, 2020.

The first three days (14-15-16 Dec.) will be devoted to the **[standard IDA training](</products/ida/training/#standard>)**. The following two days (17-18 Dec.) to a course on **[Programming for IDA](</products/ida/training/#programming>)**.

Training will be both theoretical and practical. After each theoretical section hands-on exercises will be carried out so as to master thorough understanding of concepts and methods. Training material is always updated to include the latest additions to IDA.

Detailed information including [course programs, cost and registration forms](</products/ida/training/>) can be found in our dedicated training page. If needed, additional information may be requested by emailing our [sales](<mailto:sales@hex-rays.com>) team.

[button type=”info” size=”lg” link=”/products/ida/training/”]Show me the details __[/button]

---

## 9. 

**Date:** Posted: Feb 3, 2020  
**Link:** [https://hex-rays.com/blog/a-refreshed-web-site-for-hex-rays](https://hex-rays.com/blog/a-refreshed-web-site-for-hex-rays)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# A refreshed web site for Hex-Rays

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/a-refreshed-web-site-for-hex-rays>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/a-refreshed-web-site-for-hex-rays>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/a-refreshed-web-site-for-hex-rays>)

Fabrice Ovidio ✦ Posted: Feb 3, 2020

![A refreshed web site for Hex-Rays](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

Our [website](<https://www.hex-rays.com/>) has been active for many years (since 2007, I believe) and its content has grown a lot since then. Coding practices, content management, design and web technologies have evidently evolved at a rapid pace since the early 2000’s but — for lack of time and because everybody was very busy making IDA better every day — the web site has remained unchanged albeit for its textual content.

From today you will be able to check out the first phase of our web redesign plan. The brief here was to make the content more manageable, to refresh the look-and-feel, to integrate the blog within the web site and to redesign the top pages. Focusing on only these goals has allowed us to publish in a short amount of time but has of course left most of the content untouched and indeed even imported in bulk, as-is.

This is why you can expect other redeployment phases in the future and we will start working on (some of) them right away. Existing content must first be analysed and, in many cases, updated. Editorial decisions related to very old (some might say historical) content may also be taken. One thing we would also like to do is to reorganise some sections of the web site, bringing together content that is now scattered, sometimes buried deep inside the old information architecture model. This will make it easier to find specific items and to navigate the site. At the same time we will endeavour to present content in a more attractive, easy to read way.

As there is a lot of content, we will have to decide on what to prioritise. Ideas for future changes that immediately come to mind are:

  * to develop and upgrade graphical design guidelines;
  * to merge support resources in one section and to review how such content is presented;
  * to add more visual elements to text that can sometimes be a little dry in nature;
  * to show IDA in action, through regularly updated image galleries or short movies;
  * to develop a more consistent publication approach across document types (web, PDF files, etc.);
  * and, besides improving content and layout, to also make your customer experience better, with a simpler and more intuitive presentation of our products and license types.



This is by no means a definitive list and it’s likely we will have other ideas as time goes by. This is our first step in that direction and we hope you will already find it useful.

![Web site, before and after](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hexbeforeafter-Jun-18-2024-09-05-45-9779-AM.png?width=500&height=929&name=hexbeforeafter-Jun-18-2024-09-05-45-9779-AM.png)

---

