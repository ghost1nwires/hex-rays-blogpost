# Hex-Rays Blog – Page 4

**Archived:** 2026-02-20 12:10
**URL:** https://hex-rays.com/blog/page/4

---

## 1. 

**Date:** Posted: Jul 15, 2025  
**Link:** [https://hex-rays.com/blog/unlocking-risc-v-and-arm-next-level-switch-detection](https://hex-rays.com/blog/unlocking-risc-v-and-arm-next-level-switch-detection)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [ARM](<https://hex-rays.com/blog/tag/arm>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>) [RISC-V](<https://hex-rays.com/blog/tag/risc-v>)

# Unlocking RISC-V and ARM: Next-Level Switch Detection in IDA Pro

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/unlocking-risc-v-and-arm-next-level-switch-detection>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/unlocking-risc-v-and-arm-next-level-switch-detection>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/unlocking-risc-v-and-arm-next-level-switch-detection>)

Alexandru Petenchea ✦ Posted: Jul 15, 2025

![Unlocking RISC-V and ARM: Next-Level Switch Detection in IDA Pro](https://hex-rays.com/hubfs/Maintenon-9.2.jpg)

IDA 9.2 is right around the corner

This upcoming release improves the automatic reconstruction of complex control flow, including popular obfuscation techniques in ARM64 Android libraries, aggressive “case-forcing” patterns found in RISC-V binaries, and newly recognized switch forms from the Xuantie toolchain. These enhancements reduce friction for analysts and streamline binary auditing.

# **What’s New in IDA 9.2**

**- > ARM: Detecting Obfuscated Switches**

Let’s look at an ARM example demonstrating several switch-related patterns that can be considered as obfuscation or, at the very least, non-standard control flow that complicates reverse engineering.

![](https://lh7-qw.googleusercontent.com/docsz/AD_4nXcfU2KyoUKjhh4hu5lYgEvJ4-Ucmd3Emik6hlD4t1stAJXFeZXCVBxU-4HHqkjf-U7prTU8rueRrFpgNw314cxgTOIC2NOG_zTKc548iP1x8Ha5P_Ygx-rWhXSOQAYzBp2aLeW7bQ?key=_WqTlU4MYkPpJSiVIuEUGw)

What’s happening in this code?

  * **Multiple chained indirect jumps (jump tables)****  
**The function repeatedly computes a value, bounds it ( CMP X8, #3 + CSEL X8, XZR, X8, GT), loads a function pointer from a table, and jumps to it with BR X8.  
This is a `switch` statement implemented with computed gotos.  
  

  * **Bounds-forcing via CSEL****  
**The use of CMP X8, #3 and CSEL X8, XZR, X8, GT is a form of bounds-forcing. CSEL X8, XZR, X8, GT means “If X8 > 3, set X8 to 0 (force to a valid case); otherwise keep X8 as-is.”  
Such patterns can be used to “force” an index into a valid range, but also as a trick to obscure the actual set of cases.  
  

  * **Switch pattern re-used inside each case****  
**After entering a case block, the function often sets up new values, recomputes an index, and _again_ jumps through the switch table.  
This makes the control flow non-linear and difficult for static analysis and decompilation.



![](https://hex-rays.com/hs-fs/hubfs/image-png-Jul-10-2025-07-32-58-3861-PM.png?width=1133&height=581&name=image-png-Jul-10-2025-07-32-58-3861-PM.png)

**- > ARM: Detecting Obfuscated Switches**

The real improvement comes in the decompiler. Cases that are never executed are now detected and eliminated from the output. In the example below, the switch jump depends on v1, but we can see the value can be either 3 or 1.

**_Output before 9.2 _****↓**![](https://hex-rays.com/hs-fs/hubfs/image-png-Jul-10-2025-07-31-19-0539-PM.png?width=400&height=388&name=image-png-Jul-10-2025-07-31-19-0539-PM.png)

******_Output after 9.2 _****↓******

![](https://hex-rays.com/hs-fs/hubfs/image-png-Jul-10-2025-07-38-06-2573-PM.png?width=400&height=388&name=image-png-Jul-10-2025-07-38-06-2573-PM.png)

**New config option:****  
**This optimization can be controlled through the new`OPT_VALRNG_SWITCH_NCASES` option added to hexrays.cfg.

Set to 32 by default, the value indicates that any switches with a higher number of cases are to be skipped from this optimization.

* * *

**- > RISC-V: Recognizing Non-Standard Patterns**

Several patterns were added to the RISC-V processor module. Here’s a typical example of position-independent jump tables: the table stores offsets, not absolute addresses, so you add back the base.  
Note how the number of cases is manipulated through bitwise operations at 279E2A and 279E2C. And IDA 9.2 can easily detect that.

![](https://hex-rays.com/hs-fs/hubfs/image-png-Jul-10-2025-07-44-46-8833-PM.png?width=989&height=508&name=image-png-Jul-10-2025-07-44-46-8833-PM.png)

Sometimes custom instructions are used to extract the switch index and access the jump table. This is the case with **Xuantie’s custom instructions** (e.g., th.extu and th.lrw), which make jump tables more compact and efficient, but are not standard RISC-V.

We’ve collected lots of non-standard RISC-V samples and used them to improve our switch pattern recognition.

![](https://hex-rays.com/hs-fs/hubfs/image-png-Jul-10-2025-07-45-54-4435-PM.png?width=991&height=324&name=image-png-Jul-10-2025-07-45-54-4435-PM.png)

* * *

# **Why it Matters**

Automatic switch recognition dramatically helps decompilers and analysts. Prior to these improvements, users had to manually reconstruct the real control flow, or risk misunderstanding how the program works.

## **Availability & Access**

These improvements will be included in **IDA Pro 9.2** , available in all editions where decompilers are supported.

However, please note:

  * The new ARM switch optimization can be configured via the `OPT_VALRNG_SWITCH_NCASES` setting in hexrays.cfg.



These changes are **bundled with the product release**.  


## **Looking Ahead**

As architectures and obfuscation techniques continue to evolve, we’ll keep expanding support and refining analysis. Thanks to our community for their real-world examples and feedback that helped drive the direction of these updates—keep it coming!

### **Enrolling in our Beta Program just got easier**

We’re always looking for power users to help test and refine new and updated features for our next release. And now, enrolling in as a Beta User is as simple as clicking **Subscribe** in the [customer portal](<https://my.hex-rays.com/>). You’ll see a new prompt at the top of your Dashboard when you log in.

  


**Here’s a quick rundown**

Accessing Beta Releases  


  * Once you’re subscribed, you’ll receive an email from us when the Beta version is ready for download
  * You download all beta versions from the [Download Center](<https://my.hex-rays.com/dashboard/download-center?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-9tXBfPhqo4XldjL2Zr6nfjNaEU3oFBYjOrPcaaiBe8ngpFFVnrXyKF-MSBjGml1UahCGeM>)
  * Your current active IDA license will match your Beta license:


  *     * IDA Home → IDA Home Beta
    * IDA Pro → IDA Pro Beta



Beta Testing Duration  


  * All beta testing closes on the **day of the product launch**.



Your feedback is invaluable  


  * Please report any bugs or suggestions (yes, suggestions!) on [support.hex-rays.com](<https://support.hex-rays.com/?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-9tXBfPhqo4XldjL2Zr6nfjNaEU3oFBYjOrPcaaiBe8ngpFFVnrXyKF-MSBjGml1UahCGeM>) in the **Early access feedback** form or send an email to [support@hex-rays.com](<mailto:support@hex-rays.com>)—your insights make a real impact!

---

## 2. 

**Date:** Posted: Jun 26, 2025  
**Link:** [https://hex-rays.com/blog/4-powerful-applications-of-idalib-headless-ida-in-action](https://hex-rays.com/blog/4-powerful-applications-of-idalib-headless-ida-in-action)

[Back](<https://hex-rays.com/blog>)

[IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [idablib](<https://hex-rays.com/blog/tag/idablib>) [use cases](<https://hex-rays.com/blog/tag/use-cases>)

# 4 Powerful Applications of IDALib: Headless IDA in Action

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/4-powerful-applications-of-idalib-headless-ida-in-action>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/4-powerful-applications-of-idalib-headless-ida-in-action>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/4-powerful-applications-of-idalib-headless-ida-in-action>)

Chris Hernandez ✦ Posted: Jun 26, 2025

![4 Powerful Applications of IDALib: Headless IDA in Action](https://hex-rays.com/hubfs/ida%20oem.webp)

In the world of reverse engineering and vulnerability research, efficiency and scalability are everything. While IDA Pro is widely recognized for its powerful interactive disassembler and decompiler, fewer experienced practitioners take full advantage of idalib*, Hex-Rays’ headless automation interface. 

idalib opens the door for bulk scripted static analysis, massive-scale processing, and integration into CI pipelines, all without launching a GUI. Whether you’re reverse engineering malware, triaging firmware, or automating vulnerability research across a binary corpus, idalib can drastically improve or expand your workflow.

_*For commercial applications of idalib, including embedding IDA into your own software, leveraging IDA as a SaaS platform engine, or integrating IDA into a private server having more than 1 beneficiary, users require an_[ _IDA OEM license_](</ida-pro-oem>) _to ensure compliance with our license agreement_.

### Here are four compelling applications of idalib in action:

**1\. Rhabdomancer: Rust-Powered Vulnerability Discovery**

[Rhabdomancer](<https://security.humanativaspa.it/streamlining-vulnerability-research-with-ida-pro-and-rust/?utm_source=chatgpt.com>) is a headless IDA Pro plugin written in Rust that automatically identifies calls to dangerous APIs across binaries. By categorizing these APIs based on their risk level, it helps researchers quickly focus on high-priority audit targets. With idalib and Rust bindings, it offers rapid, reliable static analysis that runs entirely headlessly. This makes it ideal for integrating into batch pipelines or CI/CD systems.

![](https://lh7-qw.googleusercontent.com/docsz/AD_4nXflO3qhM5BPx4NHmoDo45QkyoaiWmmvIufhCXpNRXgxAHtwR5vFk1XI17Vj7jkzh3pBoCrgnoBZBCKovAyXFI9XCY_8y4lqzfhPCMA2o14QACXEhGjSdadRazvNlZs_L0n2xv9voQ?key=6uWcBWsRLQuYnZX_KfvODQ)

^ Screenshot of rhabdomancer in action, finding risky API calls in a binary

* * *

**2\. Haruspex & Augur: Headless Decompilation and Static Analysis**

Also built in Rust on top of idalib, [Haruspex](<https://github.com/0xdea/haruspex>) and Augur are tools designed for headless decompilation and automated triage.

  * Haruspex exports every decompiled function to standalone files, enabling integration with external static analyzers like Semgrep or weggli.  
  

  * Augur extracts strings and cross-references them with surrounding pseudocode, making it easy to correlate textual IOCs with relevant code logic.  
  




Both tools highlight how idalib enables large-scale, scriptable analysis at the function or project level. Most interesting is how easily a researcher can target specific functions after they are decompiled. This makes it possible to find interesting binary behaviors at scale that IDA may not have identified initially.

![](https://lh7-qw.googleusercontent.com/docsz/AD_4nXeHUs_6o3IebiKzyY4B_btJZH6HgBpHPT66pS8wsVskULEN6JV0hhJEK5AOfApGndyDfa2VxLyIdnMnliMv8aX0mTpW1rO0Mt51c-b6bDMvUjBSBkerx63CYcYCV7jo3Ijb0FN04g?key=6uWcBWsRLQuYnZX_KfvODQ)

^Screenshot of Haruspex running on an arbitrary binary.

* * *

**3\. Headless IDA Python Module: Batch Binary Analysis at Scale**

The [headless-ida Python module](<https://github.com/DennyDai/headless-ida?utm_source=chatgpt.com>) is a wrapper for launching IDA Pro in headless mode. It allows analysts to batch-process directories of executables, automatically execute Python scripts, and extract structured metadata from binaries, all without manual interaction. Whether you’re scanning firmware images, clustering malware samples, or building a corpus, this project demonstrates the utility of headless scripting at scale. It’s recently been updated to support idalib instead of idat thus leveraging the performance benefits of idalib. Here you can apply any custom scripts you normally use to binaries at scale.

* * *

**4\. “Bring your own AI”**

The [headless-ida-mcp-server](<https://github.com/cnitlrt/headless-ida-mcp-server?utm_source=chatgpt.com>) project is a great example of how Idalib allows IDA to function as a remote analysis backend. Built on a Model Context Protocol (MCP), it enables you to remotely control IDA databases: rename variables, inspect functions, manage memory regions, and more, all from a client. This setup is ideal for distributed reversing environments or orchestrating workflows across multiple headless workers… this idea is repeatable with your own MCP server whether you use a publicly available AI agent or a private one.

![](https://lh7-qw.googleusercontent.com/docsz/AD_4nXe54lB-M60IgghqWK1M3tTxKxTgK49op79peAbhHQC9lNDQL7XtQyKdVMFspjvT5taDaxbgs3L7uZaToYlLBwBsKRNk-HBKV1m6rrj86AjPKuvv9uUakY1AkJRHhLXqC6Sj7sdXDQ?key=6uWcBWsRLQuYnZX_KfvODQ)

* * *

**Conclusion**

From vulnerability discovery to large-scale triage and automation, idalib can empower your reversing workflow, turning IDA Pro into a powerful backend for static analysis. If you’re already an experienced reverser using IDA Pro interactively, consider what you could build by tapping into its full headless potential. These tools are just the beginning. With idalib, your disassembler can scale as far as your imagination.

[ Start a conversation about idalib __](<https://community.hex-rays.com/>)

[ Talk to us about IDA OEM licensing __](<mailto:sales@hex-rays.com>)

---

## 3. 

**Date:** Posted: Jun 24, 2025  
**Link:** [https://hex-rays.com/blog/reversing-hanwha-security-cameras-a-deep-dive-by-matt-brown](https://hex-rays.com/blog/reversing-hanwha-security-cameras-a-deep-dive-by-matt-brown)

[Back](<https://hex-rays.com/blog>)

[firmware](<https://hex-rays.com/blog/tag/firmware>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>)

# Reversing Hanwha Security Cameras: A Deep Dive by Matt Brown

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/reversing-hanwha-security-cameras-a-deep-dive-by-matt-brown>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/reversing-hanwha-security-cameras-a-deep-dive-by-matt-brown>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/reversing-hanwha-security-cameras-a-deep-dive-by-matt-brown>)

Hex-Rays ✦ Posted: Jun 24, 2025

![Reversing Hanwha Security Cameras: A Deep Dive by Matt Brown](https://hex-rays.com/hubfs/Minimalistic%20Surface%20Map%201-2.png)

Matt Brown of Brown Fine Security recently published a video and blog post detailing how he reverse-engineered the firmware decryption mechanism used in Hanwha WiseNet security cameras. After acquiring a WiseNet XNF-8010RW camera and dumping its NAND flash, he discovered that the available firmware image was encrypted using OpenSSL with a “Salted__” header. The twist? The key needed to decrypt the firmware was embedded within the encrypted firmware itself, posing the classic chicken-and-egg problem.

CLICK FOR FULL VIDEO

To unravel this, Matt began with a string analysis of the firmware, uncovering OpenSSL decryption calls and several Base64-encoded key candidates labeled as `MODELINFO_MODEL_DECRYPTIONKEY`. By carving out ELF binaries and analyzing them with IDA Pro, he identified a key decryption function that used a hardcoded string (“zeppelin”), hashed it with SHA-256, and applied AES-256-CFB8 decryption. This yielded multiple model-specific passphrases, such as `HTWXNF-8010R`.

Using these passphrases with the appropriate digest method (MD5), he successfully decrypted the firmware image, granting access to a complete root file system. Further testing revealed that the decryption key format is predictable—simply the model name prefixed by “HTW.” This method was validated on other models, like the TNO-L4040TR, proving the scheme’s consistency.

Matt's [full blog post](<https://brownfinesecurity.com/blog/hanwha-firmware-file-decryption/>) is packed with code samples, screenshots, and detailed reverse engineering steps. You can also watch the accompanying video walkthrough, which walks through the entire process using IDA Pro.

Nicely done, Matt!

---

## 4. 

**Date:** Posted: Jun 19, 2025  
**Link:** [https://hex-rays.com/blog/ida-free-9.1-now-preinstalled-in-flare-vm](https://hex-rays.com/blog/ida-free-9.1-now-preinstalled-in-flare-vm)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA Free](<https://hex-rays.com/blog/tag/ida-free>) [FLARE VM](<https://hex-rays.com/blog/tag/flare-vm>)

# IDA Free 9.1 Now Preinstalled in FLARE VM

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-free-9.1-now-preinstalled-in-flare-vm>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-free-9.1-now-preinstalled-in-flare-vm>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-free-9.1-now-preinstalled-in-flare-vm>)

Hex-Rays ✦ Posted: Jun 19, 2025

![IDA Free 9.1 Now Preinstalled in FLARE VM](https://hex-rays.com/hubfs/enterprise%204-1.png)

Bringing accessible reverse engineering tools into the hands of malware analysts and beginners alike

We’re excited to share that IDA Free 9.1 is now bundled with [FLARE](<https://github.com/mandiant/flare-vm>)[ VM](<https://github.com/mandiant/flare-vm>), the open-source Windows-based virtual machine distribution developed by Mandiant's FLARE (Frontline Applied Research & Expertise) team—now part of Google Cloud.

Described by Google as a _“threat intelligence lab in a box”_ , FLARE VM is a go-to environment for learning and performing reverse engineering, malware analysis, and digital forensics on Windows-based threats. It’s widely adopted by aspiring analysts, hobbyists, and professionals alike for its comprehensive toolset and hands-on training potential.

IDA Free 9.1 is the latest lightweight version of our disassembler, ideal for those learning the fundamentals of reverse engineering. It includes cloud-based decompilers for x86 32-bit and 64-bit architectures and runs on Windows, macOS, and Linux. With IDA Free 9.1 now preinstalled in FLARE-VM, users can immediately dive into binary analysis using one of the most recognizable tools in the field—no additional setup required. 

In addition, FLARE-VM adds a convenient "Open with IDA option" to the right-click menu, which automatically looks for the highest available version of IDA (prioritizing IDA Pro). FLARE-VM also helps install IDA Pro by selecting the idapro.vm package and providing an IDA Pro installer, and optionally an IDA license. As part of the FLARE-VM installation, users can select several [IDA plugins](<https://github.com/mandiant/VM-Packages/wiki/Packages#ida-plugins>), which are installed even if IDA Pro is added manually after setup. This adds to the ease of getting started with IDA Pro while also benefiting from a curated plugin ecosystem tailored for reverse engineering workflows.

This integration makes FLARE VM an even stronger foundation for those entering the field of reverse engineering and threat analysis. It also supports our ongoing effort to make IDA Pro more accessible by helping new users become familiar with its interface, capabilities, and core workflows through IDA Free.

We’d also like to thank the FLARE VM community for their active role in improving and maintaining the platform, particularly when it comes to the IDA integration. Community members are often among the first to flag issues when new versions are released, contribute improvement ideas, bug fixes, and so much more. This kind of hands-on collaboration helps ensure that FLARE VM remains a reliable, up-to-date, and user-friendly environment for reverse engineers at every level. Thank you! 

Learn more about IDA Free:[ ](<https://hex-rays.com/ida-free>)[hex-rays.com/ida-free](</ida-free>)

Learn more about FLARE VM:[ github.com/mandiant/flare-vm](<https://github.com/mandiant/flare-vm>)

---

## 5. 

**Date:** Posted: May 30, 2025  
**Link:** [https://hex-rays.com/blog/why-you-should-claim-ownership-of-your-plugin](https://hex-rays.com/blog/why-you-should-claim-ownership-of-your-plugin)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA Pro Plugin](<https://hex-rays.com/blog/tag/ida-pro-plugin>) [plugin repository](<https://hex-rays.com/blog/tag/plugin-repository>) [Plugins](<https://hex-rays.com/blog/tag/plugins>)

# Why You Should Claim Ownership of Your Plugin?

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/why-you-should-claim-ownership-of-your-plugin>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/why-you-should-claim-ownership-of-your-plugin>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/why-you-should-claim-ownership-of-your-plugin>)

Geoffrey Czokow ✦ Posted: May 30, 2025

![Why You Should Claim Ownership of Your Plugin?](https://hex-rays.com/hubfs/blog2.png)

We’ve upgraded the Hex-Rays plugin repository to make it easier than ever to submit and manage your plugins.  The old submission process needed an upgrade, so we rebuilt the system to streamline everything. Now, submitting a plugin is as simple as providing a GitHub URL and including an `ida-plugin.json` file. Full details and examples are available in our [documentation](<https://docs.hex-rays.com/user-guide/plugins/plugin-submission-guide#define-plugin-metadata-with-ida-pluginjson>).

If you’ve created a plugin or are actively maintaining one, there’s a simple yet powerful step you might be missing: **reclaiming ownership of your plugin**. It’s more than just a formality — it’s a strategic move that unlocks visibility, support, and growth opportunities.

Why do you have to reclaim your plugin?  When we migrated all plugins from the previous site to the new repository, we integrated the process with our customer portal, [my.hex-rays.com](<http://my.hex-rays.com/>)[. ](<http://my.hex-rays.com/>)So now we need the original authors to reclaim their plugins due to this system change. 

This step ensures you can manage updates, and control your plugin(s) going forward. Here are some other benefits of reclaiming ownership of your plugin is essential:

* * *

## **🚀 Boost Your Plugin’s Visibility**

When you’re officially listed as the plugin owner, your plugin benefits from improved exposure across our ecosystem. Ownership helps users identify who’s actively maintaining the plugin, which builds trust and increases adoption. Plus, your plugin is more likely to appear in curated listings and recommendation feeds — helping it reach a wider audience.

* * *

## **🛠 Get Priority Support**

As a verified plugin owner, you gain access to our dedicated support team. Whether you run into a tricky issue or need guidance on best practices, you’ll be able to reach out and get tailored help — fast. This direct line to assistance can save you hours of troubleshooting and give you peace of mind as you continue developing.

* * *

## **🧪 Stay Ahead with New Features**

We’re constantly evolving our platform, and as an official plugin owner, **you’ll be the first to know** when new repository features launch — you’ll stay in the loop and ahead of the curve.

* * *

## **🎯 Ownership Means Empowerment**

By claiming your plugin, you don’t just gain access to tools — you gain a voice. You can influence how your plugin is represented, reach out for collaboration opportunities, and help shape the plugin ecosystem for everyone.

* * *

## **📝 How to Request Ownership**

Requesting ownership is easy — and only takes a few minutes:

  1. **Log in to My Hex-Rays.**

Start by logging into our customer portal with your account credentials. It's best that you use the email associated with your IDA license.

  2. **Submit Your GitHub Repository Link.**

Once you’re logged in, click "My plugins" in the side menu bar, then hit the "Submit a plugin" button on the top right. Fill out the form by providing the link to your GitHub repository. (You'll notice we've supplied some sample files to help with the process.)

  3. **We’ll Transfer the Plugin to You.**

After verifying your request, we’ll transfer ownership of the existing plugin entry to your account. You’ll then be officially recognized as the plugin owner and gain access to all the associated benefits. Ta-da!

---

## 6. 

**Date:** Posted: Feb 28, 2025  
**Link:** [https://hex-rays.com/blog/ida-9.1](https://hex-rays.com/blog/ida-9.1)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA Teams](<https://hex-rays.com/blog/tag/ida-teams>) [IDAPython](<https://hex-rays.com/blog/tag/idapython>) [debugger](<https://hex-rays.com/blog/tag/debugger>) [FLIRT](<https://hex-rays.com/blog/tag/flirt>) [IDA 9.1](<https://hex-rays.com/blog/tag/ida-9-1>)

# Introducing IDA 9.1: IDB Storage Optimizations, Processor Updates, and Time Travel Debugging

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.1>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.1>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.1>)

Hex-Rays ✦ Posted: Feb 28, 2025

![Introducing IDA 9.1: IDB Storage Optimizations, Processor Updates, and Time Travel Debugging](https://hex-rays.com/hubfs/ida-9.jpg)

We’re pumped to share the latest features and improvements in IDA 9.1. 

This release begins to tackle a number of our [2025 product goals](<https://hex-rays.com/blog/2025-product-vision-and-goals>) like expanding architecture support for non-mainstream chips and boosting collaborative features, specifically quicker sync cycles within IDA Teams. 

Let’s dig in and see what’s new…

## **IDA 9.1 Highlights**

### **Smaller, Faster, Better: IDB Compression with zstd**

IDA now uses **zstd compression** in IDB files. 

  * The higher compression results in smaller IDBs for faster saving and loading, especially when working with large databases. 
  * This also improves performance when working with remote storage or syncing with version control.



**Sharper Disassembly & Analysis: Processor Module Updates**

  * **TMS320C6** – The compact (16-bit) encodings from TMS320C66x and TMS320C674x series are now disassembled.
  * **RISC-V & RH850** – Improved switch table recognition, broader relocation handling, and extended register tracking.
  * **TriCore** – **mfcr/mtcr** instructions now use symbolic names for Core Special Function Registers (CSFRs) when known, making code easier to read.



**Debugger Updates: Time Travel, Intel Mixed Mode Debugging, and IPv6 Support**

  * **Windbg Time Travel Debugging** – You can now use **Time Travel Debugging (TTD)** with Windbg, allowing you to track and replay the execution of your code (requires an updated version of dbgeng.dll).
  * **Wow64 Process Debugging** – You can now switch between 32-bit and 64-bit modes in Wow64 processes (aka Heaven’s Gate) - this can be debugged now.
  * **IPv6 Addresses –** Our (debug) servers now speak both IPv4 and IPv6 (we hear you, sysadmins).



**Expanding Decompilation: Latest Updates for ARM64, PPC, and RISC-V**

We’ve made improvements for decompiler support across multiple architectures. These updates bring more precision and (hopefully) fewer headaches:

  * **ARM64** – Now supports **ILP32 mode** , making decompilation more accurate for systems like Apple’s watchOS.
  * **ARM64** – Displaying of symbolic **system register names** in pseudocode means better, more readable decompilation for OS-level applications.
  * **PPC** – Support for **EFP** (Embedded Floating Point) extension instructions of the Signal Processing Engine (SPE), expanding your analysis capabilities.
  * **RISC-V** – Added more **intrinsics** , reducing the number of __asm fragments in your pseudocode for a cleaner view. The decompiler now also handles Atomic Memory operation (AMO) instructions seamlessly.

  


**Smarter Versioning with IDA Teams**

Keeping your IDB versions in sync just got faster and more efficient. 

  * IDA Teams versioning functionality can now send and receive small binary delta files instead of whole IDBs, delivering faster version management operations and less network traffic by only sending what has changed. 
  * The delta files can also be stored on the Vault server, keeping down disk usage.



**Automated Rust Version Detection & FLIRT Signature Generation**

  * IDA now detects the Rust version of the loaded binary, enabling the automated creation of custom, version-specific FLIRT signatures.



# **Bugfixes and More**

  * Check out the release notes with all the updates and bugfixes [here](<https://docs.hex-rays.com/9.1/release-notes/9_1>).




**Keep us in the loop!**

  * If you see something, say something. You can report bugs here: [https://support.hex-rays.com](<https://get.support.hex-rays.com/servicedesk/customer/portals>)
  * Have ideas on how we can improve IDA? We want to know! Send us a note with your feedback directly to [product@hex-rays.com](<mailto:product@hex-rays.com>), or post it on [Discourse](<https://community.hex-rays.com>).



### **Getting the 9.1 update**

If you already purchased an IDA 9 subscription, you will see 9.1 in the Download Center of [My Hex-Rays](<https://my.hex-rays.com/login>) portal.

* * *

### **Getting your hands on IDA 9.1**

If you have a perpetual license in active support, you can download a trial version of IDA 9.1! To start using IDA 9 today you can access the installer in our new customer portal, [My Hex-Rays](<https://my.hex-rays.com/login>). There you can request a license key for your free subscription. Please note that the license key for your free IDA 9.1 subscription will expire at the end of your active support period. You can find detailed instructions on how to upgrade to the IDA 9 series [here](<https://hex-rays.com/faqs/how-do-i-upgrade-to-ida-9>).

**Support plan expired? No problem.**

You can purchase an updated license online [here](</pricing>). You’ll see we’ve updated our product packages. If your previous plan doesn’t align with our new offerings, fear not, we’re here to help. We’ve been working with all our customers to ensure they get a plan that is the right fit (and price) for them. Just email us at [sales@hex-rays.com](<mailto:sales@hex-rays.com>).

---

## 7. 

**Date:** Posted: Feb 28, 2025  
**Link:** [https://hex-rays.com/blog/ida-8.5](https://hex-rays.com/blog/ida-8.5)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA 8.5](<https://hex-rays.com/blog/tag/ida-8-5>)

# Releasing IDA 8.5: nanoMIPS, WASM, and More

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8.5>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8.5>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8.5>)

Hex-Rays ✦ Posted: Feb 28, 2025

![Releasing IDA 8.5: nanoMIPS, WASM, and More](https://hex-rays.com/hubfs/ida-9.jpg)

With IDA 8.5, we bring you some critical fixes, UI enhancements, and updates to the IDA API/SDK and IDB. IDA 8.5 will help users on perpetual terms gradually migrate to the IDA 9-series. 

Please mind that IDA 8.5 does not support features specific to the 9-series platform, such as x64 exceptions, IDA feeds and others.

If you have a perpetual license with active support and have not yet started using IDA 9.0 or IDA 9.1, you can now download **IDA 8.5** from our customer portal.  
  
Alternatively, please note that you can also download a free version of IDA 9.1 by requesting a license key from our customer portal, [**My Hex-Rays**](<https://hub.hex-rays.com/e3t/Ctc/ZX+113/djVGn-04/VXgWHZ2ZMM4tN373r7FyQ-GCW3LfLlX5sB85VN2C5cyd3qgyTW69sMD-6lZ3lYVpyJPz4ck0wDVJCP0-31D3__N7x1LDFmgmgtW49wPdB7nwsQMW3hBfqj6MG0D2N1x6MDz-7L86VCftdP7PPtpGW336M-c6-rK4_W50V_TC8lHnDpN4WG_bwbR_nqW8DnFXP21GBVvW2bd6xR58Nr05VzkTwm83tHypW2PBdTk7gJL8JW5VPLM29677CkW7JrtFb3jwdFRW4wsyXf4CKr-nN11rrZ5V6hqcW4Xph7M6MD3RXW5kscnL1rCNjLf3cysKn04>)."

IDA 8.5 brings a range of enhancements. This update introduces nanoMIPS support, along with a new WebAssembly (WASM) disassembler and loader. We've also refined the IDAPython API, and more. Read on for a closer look at what’s new.

**nanoMIPS support**

In IDA 8.5, the MIPS disassembler and decompiler now support nanoMIPS instructions. Since firmware compiled for nanoMIPS is often distributed in md1rom format, we’ve also added a md1rom file loader that parses and applies debug symbols when available.

nanoMIPS support is included in the classic MIPS (HEXMIPS) decompiler, so if you already have the MIPS decompiler, you won’t need an additional decompiler.

**WebAssembly (WASM) Disassembler and File Format Loader**

With the shift toward client-side browser applications, we saw the need for a dedicated WebAssembly (WASM) disassembler. IDA 8.5 now includes WASM code embedded into its own binary file format as well as a loader that decodes the WASM file format.

**IDAPython Improvements**

The C++ SDK and IDAPython API have been revised. We've added several helpers to improve our API user experience. These new endpoints, among other things, streamline managing and operating on types.

We also have a few other goodies to assist with accessibility and usage of our API: 

  * An updated [Porting Guide](<https://docs.hex-rays.com/8.5/developer-guide/idapython/idapython-porting-guide-ida-9>) which reflects recent API improvements
  * New [IDAPython examples](<https://docs.hex-rays.com/8.5/developer-guide/idapython/idapython-examples>), mainly for the Working with types category
  * Revamped IDAPython [reference documentation](<https://python.docs.hex-rays.com/8.5/index.html>) to improve cross-referencing  


  


**Other Feature Enhancements Include:**

  * Deprecated IDA32
  * Metadata Descriptors for Plugins
  * UI Improvements



# **Bugfixes and More**

  * Full release notes and a complete list of bug fixes can be viewed [here](<https://docs.hex-rays.com/release-notes/8_5>).




**Keep us in the loop!**

  * If you see something, say something. You can report bugs here: [https://support.hex-rays.com](<https://get.support.hex-rays.com/servicedesk/customer/portals>)
  * Have ideas on how we can improve IDA? We want to know! Send us a note with your feedback directly to [product@hex-rays.com](<mailto:product@hex-rays.com>), or post it on [Discourse](<https://community.hex-rays.com>).



### **Getting the 8.5 update**

If you have IDA 8.4 in active support you will still be able to download your hexlic key for IDA 8.5 in the "Licenses" tab of the [MyHex-rays portal](<https://my.hex-rays.com>). 

* * *

### **Getting your hands on IDA 9.1**

If you have a perpetual license in active support, you can download IDA 9.1 free of charge! To start using IDA 9.1 today you can access the installer in our new customer portal, [My Hex-Rays](<https://my.hex-rays.com/login>). There you can request a license key for your free subscription. Please note that the license key for your free IDA 9.1 subscription will expire at the end of your active support period. You can find detailed instructions on how to upgrade to IDA 9 [here](</faqs/how-do-i-upgrade-to-ida-9>).

**Support plan expired? No problem.**

You can purchase an updated license online [here](</pricing>). You’ll see we’ve updated our product packages. If your previous plan doesn’t align with our new offerings, fear not, we’re here to help. We’ve been working with all our customers to ensure they get a plan that is the right fit (and price) for them. Just email us at [sales@hex-rays.com](<mailto:sales@hex-rays.com>).

---

## 8. 

**Date:** Posted: Feb 17, 2025  
**Link:** [https://hex-rays.com/blog/2024-plugin-contest-winners](https://hex-rays.com/blog/2024-plugin-contest-winners)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA Pro Plugin](<https://hex-rays.com/blog/tag/ida-pro-plugin>) [Plugins](<https://hex-rays.com/blog/tag/plugins>)

# Announcing the 2024 Plugin Contest Winners

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/2024-plugin-contest-winners>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/2024-plugin-contest-winners>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/2024-plugin-contest-winners>)

Hex-Rays ✦ Posted: Feb 17, 2025

![Announcing the 2024 Plugin Contest Winners](https://hex-rays.com/hubfs/plugin%20contest%20blog%20featured%20image%20v2.png)

The 2024 Hex-Rays IDA Plugin Contest has come to a close, and we are thrilled to announce this year's winners! With an impressive array of 20 submissions, participants showcased innovative solutions to enhance IDA Pro’s functionality. 

After rigorous testing and deliberation, our panel of judges selected the following winners:

  * **First Prize:** hrtng – A feature-rich plugin offering decryption, deobfuscation, patching, and automated control flow unflattening.
  * **Second Prize:** aiDAPal – A locally hosted AI-powered assistant for Hex-Rays pseudocode analysis.
  * **Third Prize:** idalib Rust bindings – High-level Rust bindings for the IDA SDK, enabling seamless standalone tool development.  


  


Congratulations to our winners, and a huge thank you to all participants for their hard work and creativity!

# **Highlights from the Winning Plugins**

**hrtng** impressed our judges with its extensive feature set, including automated control flow unflattening and robust function construction. This tool streamlines reverse engineering workflows with well-integrated functionality and comprehensive documentation. Malware analysts will find these features useful and can be found exclusively in IDA.

**aiDAPal** introduced an AI-assisted approach to pseudocode analysis, leveraging a fine-tuned large language model to enhance code comprehension and variable renaming. While performance is hardware-dependent, its potential for future AI-driven enhancements is immense.

**idalib Rust bindings** provided an idiomatic way to interact with IDA’s public API in Rust, facilitating batch processing and standalone analysis. Though still evolving, it presents a promising foundation for Rust-based tooling in IDA Pro.

# **Other Notable Submissions**

Beyond the winners, this year’s contest featured a lineup of plugins addressing a variety of reverse engineering challenges:

  * **Delphi Helper** – Streamlines the analysis of Delphi binaries by parsing RTTI structures and extracting binary resources.
  * **LabSync** – Enables partial synchronization of IDBs among reverse engineers for improved collaboration.
  * **Graffiti** – Enhances IDA’s call graph visualization, aiding navigation and understanding of complex binaries.
  * **Xrefer** – Provides a structured navigation interface with function clustering to accelerate static analysis.
  * **RevEng.AI** – An AI-powered binary analysis platform offering function renaming, packer identification, and vulnerability assessment.
  * **NavColor** – Enhances IDA’s navigation band by visually distinguishing functions with custom colors.
  * **YaraVM** – A processor module and loader enabling disassembly and analysis of compiled Yara rules.



We extend our gratitude to all participants for their ingenuity and dedication. If you’re eager to explore these plugins, visit our [plugin repo](<https://plugins.hex-rays.com>) where we link to their repositories and see how they can enhance your IDA Pro experience.

Stay tuned for future contests, and we look forward to even more groundbreaking innovations next year!

  
_*The 2024 Hex-Rays IDA Plugin Contest winners participated as private individuals, and their submissions were evaluated solely on their merits. Hex-Rays has no business relationship with the winners’ employer. The outcome of this contest does not establish or imply any affiliation between Hex-Rays and the winners’ employer, nor does it constitute an endorsement of their products or services._

**We want to hear from you!**

Let us know your thoughts by sending a direct message to [product@hex-rays.com](<mailto:product@hex-rays.com>) or share your ideas on our [Discourse forum](<https://community.hex-rays.com/>).

---

## 9. 

**Date:** Posted: Jan 29, 2025  
**Link:** [https://hex-rays.com/blog/2025-product-vision-and-goals](https://hex-rays.com/blog/2025-product-vision-and-goals)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA](<https://hex-rays.com/blog/tag/ida>) [Decompilation](<https://hex-rays.com/blog/tag/decompilation>) [Collaboration](<https://hex-rays.com/blog/tag/collaboration>) [Plugins](<https://hex-rays.com/blog/tag/plugins>) [c++](<https://hex-rays.com/blog/tag/c>) [FLIRT](<https://hex-rays.com/blog/tag/flirt>) [SDK](<https://hex-rays.com/blog/tag/sdk>) [Discourse](<https://hex-rays.com/blog/tag/discourse>)

# 2025 Product Vision & Goals

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/2025-product-vision-and-goals>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/2025-product-vision-and-goals>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/2025-product-vision-and-goals>)

Hex-Rays ✦ Posted: Jan 29, 2025

![2025 Product Vision & Goals](https://hex-rays.com/hubfs/Minimalistic%20Surface%20Map%201.png)

# We're excited to share our 2025 product vision, shaped by your invaluable feedback. This year, we're honing in on four focus areas. Read on to discover what's ahead, and keep the ideas coming—let us know what's working, what's not, and how we can take things to the next level together.

# **1 | Enhancing IDA Core Functionalities: Not Just a Facelift**

IDA is the backbone of what we do. It’s like the Swiss Army knife of reverse engineering, but even the best tools need sharpening now and then. This year, we’re focusing on improving its core functionalities to ensure it remains not just useful—but _essential_.

  * **Advanced Decompilation Improvements.** We are planning to enhance decompiler support for “higher level” compiled languages like C++, Rust, and Golang. Remember our most recent addition of decompiling Microsoft Visual C++ exception handlers? We plan to surface similar efforts this year. 
  * **Expanding Architecture Support**. As CPU architectures evolve, so do we. Updates are coming for IDA’s disassembler that covers more extensions to current architectures, including non-mainstream chips like Tricore, RH850, TMS320, and NDS32, as well as common platforms like Intel, ARM, and PowerPC. And maybe—just maybe—a new decompiler for additional architectures.


  * **Interactive Decompiler.** Today, decompilation is largely a one-shot process, translating machine code into (mostly) human-readable pseudocode. We’re creating a way for advanced users to interact with and customize intermediate decompilation steps, such as microcode generation, optimization, and CTree construction, directly within IDA.



These efforts are part of a broader push to address tech debt, focusing on areas like data storage, threading models, and user experience. While this is a long-term initiative we’ve already begun work on, you’ll start seeing the benefits this year.

**2 | Boosting Collaborative Features: Smarter, Seamless Teamwork**

Reverse engineering isn’t always a solo sport, so we’re introducing better collaborative features. Real-time collaboration and enhanced integration with IDA Teams will enable faster, more efficient teamwork. One highlight? **Quicker synchronization cycles for Teams users** , allowing you to share insights instantly and feel like you’re in the same room, even from miles apart.

**3 |****Empowering the Community: Plugins, and Discourse, and SDKs, oh my!**

We’re not just building tools for today’s problems—we’re laying the groundwork for a thriving, long-term ecosystem. That means empowering the community to innovate with us.

  * **Open-Source SDK** : Down the road, our C++ SDK will be open-sourced on GitHub, complete with tagged releases and tons of example code to spy on/learn from. This will simplify life for plugin authors and foster more innovation.
  * **Plugin Repository & Enhanced Management Tools**: We’re making it easier to customize and extend IDA’s capabilities with an improved plugin repository where discovery and management are a breeze. You’ll know when a plugin was last updated, which versions of IDA it’s compatible with, and more. Plus, in the future, we’re introducing a program to reward plugin creators who maintain compatibility with API updates.
  * **Simplified, Low-code API** : We’re introducing a low-code API extension to make coding and scripting more accessible while keeping the current API fully supported. This extension will expose additional functions, classes, and entities that minimize the learning curve without disrupting the current API. (This was on our 2024 to-do list. And while we were unable to make the progress we aimed for, making this extension a reality is a top priority for 2025.)
  * **Discourse Online Community:** We’ve launched a new platform for open dialogue, idea-sharing, and problem-solving. Expect updates on events, contests, and other happenings. You can join the conversation [here](<https://community.hex-rays.com/>).  


  


**4 | Broadening Our Scope: Going Beyond FLIRT & Lumina**

As we look forward, we want to create and enhance features that streamline your workflows and help you tackle challenges (or repetitive work) faster than ever. Two initiatives, in particular, have us excited:

  * **IDA Feeds Signature Bundles:** Building on last year’s efforts, we’ll continue to expand and improve the set of signatures available to our users. We’ll also introduce an on-demand signature generation flow for custom configurations.
  * **FLIRT 2.0:** To complement this expansion, we're revamping the underlying tech (FLIRT / Lumina) to make function matching faster and more accurate, while keeping false positives low. 



### **Shaping What’s Next for IDA—Together**

We’re dedicated to making our tools better, and we do that by listening to you. Feedback, ideas, even complaints (hey, we can take it)—we want to hear it all. Together, we’re going to keep pushing forward, improving, and building a product that works _for_ _you_. So stay tuned for the updates, the improvements, and the occasional nerdy joke. 

Until then, we’ll be tackling the list above—one byte at a time. (Oh, look at that. We can cross the nerdy joke off the list already.)

**We want to hear from you!**

Let us know your thoughts by sending a direct message to [product@hex-rays.com](<mailto:product@hex-rays.com>) or share your ideas on our [Discourse forum](<https://community.hex-rays.com/>). Last year we had several feedback sessions with our Power Users that resulted in features and enhancements.

---

