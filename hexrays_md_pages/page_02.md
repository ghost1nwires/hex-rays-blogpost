# Hex-Rays Blog – Page 2

**Archived:** 2026-02-20 12:09
**URL:** https://hex-rays.com/blog/page/2

---

## 1. 

**Date:** Posted: Dec 25, 2025  
**Link:** [https://hex-rays.com/blog/ida-9.3-beta-is-live](https://hex-rays.com/blog/ida-9.3-beta-is-live)

[Back](<https://hex-rays.com/blog>)  
  
[beta program](<https://hex-rays.com/blog/tag/beta-program>) [Collaboration](<https://hex-rays.com/blog/tag/collaboration>) [IDA 9.3](<https://hex-rays.com/blog/tag/ida-9-3>)

# IDA 9.3 Beta is Live!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.3-beta-is-live>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.3-beta-is-live>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.3-beta-is-live>)

Hex-Rays ✦ Posted: Dec 25, 2025

![IDA 9.3 Beta is Live!](https://hex-rays.com/hubfs/9.3.png)

If you're part of our Beta Program, the new build is now available in the [Download Center](<https://my.hex-rays.com/login>) of your customer portal. Your testing and feedback are essential in validating new features, surfacing regressions, and ensuring this release is ready for production use.

Not enrolled yet? Joining the program is quick and easy, just click Subscribe from your customer portal dashboard - see below for details.

## What's new in IDA 9.3 Beta 1

  * **Decompiler:**  
New V850 decompiler, microcode manipulation, viewer improvements and more


  * **Type system:**  
Objective-C parser, more Golang types


  * **Teams add-on:**  
Improved Teams UI now fully integrated into IDA, eliminating the need for HVUI


  * **Architecture support:**  
Apple SVE, SME, MTE ARM64 extensions, Andes Andestar V3 NDS32, improvements to Tricore, ARC, x86/x64, PowerPC, RISC-V, ARM64


  * **Debuggers:**  
Android 14-17 native debugging, Stack view dereferencing


  * **UI:**  
Improved Xref Tree and Xref Graph, along with other performance improvements


  * **FLIRT 2.0** (Beta 1 includes support for ARM64 Windows signatures only)
  * **New Linux ARM64 Installers**


  * And more ...



  


-> View the full feature list here: <https://docs.hex-rays.com/release-notes/9_3beta#whats-new-in-beta-1>

### Enrolling in the Beta Program is simple

  * Log in to your[ customer portal](<https://my.hex-rays.com/>).
  * Click **Subscribe** when prompted at the top of your dashboard.



![beta subscribe screenshot](https://hex-rays.com/hs-fs/hubfs/beta%20subscribe%20screenshot.png?width=1129&height=339&name=beta%20subscribe%20screenshot.png)

Once enrolled, you will be able to access the beta release in the **Download Center**. Moving forward, we’ll notify you by email when beta versions are ready for download. Note that beta licenses will automatically match your active IDA license (e.g., IDA Home → IDA Home Beta, IDA Pro → IDA Pro Beta).*

_*You must have an active IDA Pro or IDA Home license to beta test._

Get full program details plus the terms and conditions here: [https://hex-rays.com/beta-program](</beta-program>)

### Your feedback drives IDA forward

We rely on your feedback to shape the future of IDA. Please report bugs or suggestions using the[ Early Access Feedback Form](<https://support.hex-rays.com/>) or by emailing [support@hex-rays.com](<mailto:support@hex-rays.com>).

**- > IMPORTANT NOTE:** Beta access closes on the day of the final release, so don’t wait too long to dive in.

---

## 2. 

**Date:** Posted: Nov 25, 2025  
**Link:** [https://hex-rays.com/blog/idalib-powers-products-with-oem-license](https://hex-rays.com/blog/idalib-powers-products-with-oem-license)

[Back](<https://hex-rays.com/blog>)

[idalib](<https://hex-rays.com/blog/tag/idalib>) [IDA Pro OEM License](<https://hex-rays.com/blog/tag/ida-pro-oem-license>)

# Use idalib To Power Your Tools and Products (And Do It For Free During Dev)

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/idalib-powers-products-with-oem-license>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/idalib-powers-products-with-oem-license>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/idalib-powers-products-with-oem-license>)

Hex-Rays ✦ Posted: Nov 25, 2025

![Use idalib To Power Your Tools and Products \(And Do It For Free During Dev\)](https://hex-rays.com/hubfs/idalib%20oem%20blog%20image%203.png)

If you’ve worked with IDA beyond the UI — scripting, automation, headless tasks, or custom tooling — you may have heard of [idalib](<https://docs.hex-rays.com/user-guide/idalib>). For those who haven’t, it’s the programmatic interface to the IDA analysis engine, designed to let you call IDA as a library, run analysis headlessly, or integrate IDA logic into your own systems.

idalib is what teams use when they want IDA to work behind the scenes: driving custom workflows, powering automated analysis, or supporting large-scale processing without launching the full application. If you’re looking to embed IDA into your toolchain or run the decompiler at scale, idalib is the component that makes that possible.

# **Who uses idalib today?**

Across industries, we’re seeing idalib adopted by:

  * security product teams
  * threat intelligence platforms
  * malware analysis labs
  * exploit development teams
  * firmware and embedded analysis teams
  * cloud analysis providers
  * enterprise internal security teams



These teams use idalib to solve very different problems, but the common thread is **automation, scale,** and**integration**.

# **What Workflows Does idalib Power?**

Today, idalib is being used for:

  * Headless decompilation workers running in VMs, containers, and cloud environments
  * Automated pipelines that ingest binaries and analyze them at scale
  * Regression and triage systems that handle thousands of samples
  * Static analysis backends behind internal tools or SaaS offerings
  * Code similarity and enrichment workflows
  * Integrated reverse-engineering features inside commercial products
  * …



If you want detailed examples, we’ve published two relevant blogs:

  * Introducing HCLI — a modern CLI for automated IDA deployments  
[ https://hex-rays.com/blog/introducing-hcli](</blog/introducing-hcli>)
  * 4 Powerful Applications of idalib: Headless IDA in Action  
[ https://hex-rays.com/blog/4-powerful-applications-of-idalib-headless-ida-in-action](</blog/4-powerful-applications-of-idalib-headless-ida-in-action>)



# **Two Ways to Use idalib and the License Type That Fits Each One**

There are really two categories of idalib usage, and each one maps to a different license model. Below is a simple breakdown.

## **1) Personal automation, experimentation & solo workflows**

➥ License: Standard IDA Pro (Expert or Ultimate)

This fits if you are:

  * using idalib locally
  * experimenting with scripts
  * building personal utilities
  * running small-scale headless tasks
  * automating your own workflow (not a team workflow)
  * working solo, on one machine or with one named license, not on servers
  * _not_ embedding IDA into a product or service



A standard IDA Pro license is designed for analysts doing their own work, including using idalib for personal automation.

As an example, you can use idalib to host the [IDA MCP server](<https://github.com/mrexodia/ida-pro-mcp>) with Claude, Gemini, or even local LLMs to enable Q&A-style reverse engineering. You can also use [HCLI](<https://hcli.docs.hex-rays.com/>) and idalib in GitHub Actions or other CI/CD solutions to develop and test personal utilities like [capa](<https://github.com/mandiant/capa>) or [Augur](<https://plugins.hex-rays.com/0xdea/augur>).

## **2) Team workflows, server-side analysis, or product integration**

➥ License: IDA Pro OEM

> OEM stands for **O** riginal **E** quipment **M** anufacturer. It simply means you’re licensed to run IDA as part of another system. Like a video conferencing app embedding third-party noise-reduction tech into its platform.__

This applies when idalib is:

  * running on servers, VMs, clusters, or cloud workers **when the output is used by multiple people**
  * integrated into internal platforms or dashboards
  * powering features inside a commercial or closed-source product
  * supporting multiple analysts or users
  * used as a backend component in a service
  * accessed programmatically across nodes or systems
  * used by AI agents to serve multiple end-users
  * … 



Once IDA Pro (via idalib) becomes part of a system, not just an individual user’s workflow, you’re in OEM territory.

# **Why the IDA Pro OEM License Exists & It’s (Many) Benefits**

Standard IDA Pro licensing (named, computer or floating) are designed for use by individual analysts.

The IDA Pro OEM license exists for everything else: scaling, server deployments, pipelines, clusters, and product integration for multiple users.

> Under the hood, an OEM deployment is essentially:
> 
> ###  ➥ idalib + a standard IDA Pro 
> 
> configured to operate legally and efficiently on servers or inside products.

The OEM license also includes:

  * scalability for headless/server workflows
  * flexible deployment (containers, VMs, cloud)
  * embedding IDA’s decompiler engine in your tooling
  * permission for team use and multi-user access
  * the ability to integrate IDA functionality into commercial offerings
  * access to all 11 decompilers (quite an upgrade if you don’t already have them!)
  * direct access to our technical experts for architecture guidance
  * accelerated support for setup, integration, and ideation
  * collaborative product feedback and feature-proposal pathways



# **Build Freely: Why Development is FREE**

One of the most important things about the IDA Pro OEM license is that it supports innovation rather than restricting it.

That’s why the entire development phase is free and you only move into a fee structure once you’re ready to ship, scale, or commercialize. This means that all of the following are typically free, and allow you to explore and develop without worry:

  * proof-of-concepts
  * prototypes
  * pilots
  * research projects
  * dev and testing
  * beta stages



What’s the fee structure? The cost of an IDA Pro OEM license will depend on your use case and we’re happy to discuss the options.

For production use, whether in a commercial product/platform or an internal workflow serving multiple users, we tailor pricing to the value IDA delivers: usage-based, revenue share, per internal seat, and per server or CPU, for example. We’re flexible and transparent, and we believe that when IDA meaningfully enables or accelerates your solution, it’s fair that Hex-Rays is appropriately compensated. Tell us about your scenario and we’ll propose a simple structure that works for both sides.

Remember, during development (and beyond), we’re here to support, consult and collaborate. 

We want you to innovate without friction. That’s the core of **Build Freely**.

_Note: We evaluate every IDA OEM license request individually to ensure resources go to projects with a clear technical, commercial, or community path. While development access is free for most qualifying integrations, some projects may not meet the criteria for complimentary development licensing._

# **A Quick Note on Compliance**

If you're currently using a standard IDA Pro license in a server, automated, or product-integrated setup, you may be outside the scope of your EULA. The OEM license is designed exactly for these scenarios, and moving you into the right model is simple.

# **Next Steps: Tell us what you’re building (or what you want to build)!**

If any of this describes your workflow, or where you’d like to take it, let’s talk so we can give you more power to enhance your work.

**Email us** at [oem@hex-rays.com](<mailto:oem@hex-rays.com>) or fill out the form below.[](<mailto:oem@hex-rays.com>)

---

## 3. 

**Date:** Posted: Nov 20, 2025  
**Link:** [https://hex-rays.com/blog/introducing-the-ida-plugin-manager](https://hex-rays.com/blog/introducing-the-ida-plugin-manager)

[Back](<https://hex-rays.com/blog>)

[plugin repository](<https://hex-rays.com/blog/tag/plugin-repository>) [Plugins](<https://hex-rays.com/blog/tag/plugins>) [HCLI](<https://hex-rays.com/blog/tag/hcli>) [Plugin Manager](<https://hex-rays.com/blog/tag/plugin-manager>)

# Introducing the IDA Plugin Manager

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-the-ida-plugin-manager>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-the-ida-plugin-manager>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-the-ida-plugin-manager>)

Willi Ballenthin ✦ Posted: Nov 20, 2025

![Introducing the IDA Plugin Manager](https://hex-rays.com/hubfs/plugin%20manager%20-%20blog%20hero%20image.png)

_A modern ecosystem for discovering, installing, and sharing IDA plugins_

The IDA community has created more than 800 plugins and extensions over the years. Today, we're introducing the **IDA Plugin Manager** \- a modern, self-service ecosystem that makes extending IDA as simple as this:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-13-38-6948-PM.png?width=1600&height=364&name=undefined-Nov-20-2025-06-13-38-6948-PM.png)

Browse over 180 active plugins at [plugins.hex-rays.com](<https://plugins.hex-rays.com/>), install them with automatic dependency resolution, and publish your own plugins through a transparent, self-service ecosystem built on GitHub.

_The Plugin Manager is currently built into_[ _HCLI_](<https://hcli.docs.hex-rays.com/>) _, Hex-Rays' command-line tool._

* * *

I always used to have a love-hate relationship with IDA plugins: the right plugin could make my analysis so much easier, but initially finding it and then keeping it updated was almost impossible. I’d rely on tips from my friends for the best plugins, but that surely meant missing new ideas. And when migrating to a new system, I could rarely remember where to download the plugin and how I had configured it.

The new IDA Plugin Manager addresses these challenges: it provides a centralized place to explore what’s available and makes installation, configuration, and updating single commands and/or button clicks. _Buckle up_ and _strap in_ as we tour the key commands, overall architecture, specific design points and enhancements, and the pending roadmap.

# **Exploring the Plugin Repository**

To get started, visit [plugins.hex-rays.com](<https://plugins.hex-rays.com/>) to browse the updated Plugin Repository in your web browser. You can explore plugins by category, search by keyword, and read detailed descriptions. Look for the "Plugin Manager Compatible" badge on plugin entries to identify which plugins support easy installation. This number grows weekly as more authors package their plugins and as Hex-Rays proposes packaging changes via PRs under our [HexRays-plugin-contributions](<https://github.com/HexRays-plugin-contributions>) GitHub account.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-16-23-7446-PM.png?width=1600&height=331&name=undefined-Nov-20-2025-06-16-23-7446-PM.png)

From the command line, searching is equally straightforward. Use the [Hex-Rays CLI (HCLI) tool](<https://hcli.docs.hex-rays.com/>) to list all the available plugins: ![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-16-38-8942-PM.png?width=1600&height=1458&name=undefined-Nov-20-2025-06-16-38-8942-PM.png)

The Plugin Manager automatically detects your IDA version and platform, showing you only compatible plugins. Incompatible plugins appear grayed out.

You can also search by plugin name or keyword to find plugins for specific tasks:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-17-04-1025-PM.png?width=1600&height=491&name=undefined-Nov-20-2025-06-17-04-1025-PM.png)

# **One-Command Installation**

Once you've found a plugin, installation is a single command:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-17-03-6809-PM.png?width=1600&height=282&name=undefined-Nov-20-2025-06-17-03-6809-PM.png)

While it’s only a few lines of output, quite a bit happened:

  1. **Verified compatibility** with your IDA version and platform
  2. **Resolved Python dependencies** and installed them via pip
  3. **Downloaded the plugin** from the official source
  4. **Extracted and installed** to $IDAUSR/plugins/capa/
  5. **Prompted for configuration** if the plugin requires settings (like API keys)



Our goal was to take all the manual toil out of IDA plugin installation and make it as streamlined as possible. Ideally you shouldn’t have to edit Python source files or invoke pip anymore to upgrade your IDA experience.![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-17-03-2724-PM.png?width=1600&height=364&name=undefined-Nov-20-2025-06-17-03-2724-PM.png)

You can check the status of installed plugins like this, which is helpful to figure out what can be upgraded to newer versions:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-19-06-5023-PM.png?width=1600&height=416&name=undefined-Nov-20-2025-06-19-06-5023-PM.png)

(As I write this blog post, I want to be able to see the plugin release notes for pending upgrades, so we’ll have to design and build that! Unfortunately the Plugin Manager doesn’t do this yet.)

And you can use hcli plugin upgrade and hcli plugin uninstall to… upgrade and uninstall plugins.

The Plugin Manager works with IDA Pro 9.0 and above, bringing modern package management to recent IDA versions.

* * *

# **The Architecture: Transparent and Self-Service**

When designing the Plugin Manager, we wanted to build an ecosystem where plugin authors can publish without gatekeeping, users can verify what they're installing, and the entire process is transparent. We've achieved this by open sourcing all the components and hosting the repository index on GitHub. 

Here's how it works. The ecosystem has three core components:

  1. **GitHub Project Discovery** A [daily indexer](<https://github.com/HexRaysSA/plugin-repository/actions/workflows/sync.yml>) scans GitHub for repositories containing ida-plugin.json metadata files. When it finds one, it looks for releases and inspects attached ZIP archives. Any plugin with valid metadata is automatically added to the central index. There is no longer any manual submission or approval process.



We use GitHub today because it's the most prevalent platform where IDA plugins are hosted. However, we're not permanently tied to GitHub - it's simply the starting point. As the ecosystem grows and we hear from users who prefer other platforms, we can extend the indexer to support additional code forges.

  2. **Central Plugin Repository** The index is published as a JSON manifest at [github.com/HexRaysSA/plugin-repository](<https://github.com/HexRaysSA/plugin-repository>). This repository is completely public and auditable. Anyone can inspect it, verify its contents, and see exactly what plugins are available and where they're hosted. Please fork and/or watch the Git history to see plugin additions and (hopefully unlikely) moderation events.
  3. **Plugin Manager Client** HCLI, and later an in-IDA GUI, reads the index, verifies compatibility with your environment, and handles installation. Plugins are packaged as ZIP archives containing the plugin code and metadata. For Python plugins, the manager resolves dependencies and installs them via pip. For native plugins (compiled C/C++), it extracts the appropriate .dll, .so, or .dylib file for your platform.



# **Python Dependency Support**

One of the most significant improvements brought about by the Plugin Manager is automatic Python dependency installation. Plugin authors declare their requirements in ida-plugin.json, and the Plugin Manager handles installation automatically.![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-19-06-9770-PM.png?width=1600&height=645&name=undefined-Nov-20-2025-06-19-06-9770-PM.png)

# **Safety and Moderation**

The plugin repository contains an [explicit denylist](<https://github.com/HexRaysSA/plugin-repository/blob/v1/ignored-repositories.txt>) for malicious or problematic plugins. All moderation actions are visible in the repository's git history as edits to the file - no behind-the-scenes removals. If you encounter a suspicious plugin, report it through the repository's issue tracker or [support@hex-rays.com](<mailto:support@hex-rays.com>), and the Hex-Rays team will investigate.

# **Centralized Settings Management (Under Development)**

We're also [designing](<https://docs.google.com/document/d/1DKSEN9_8qrIF0HBYEaJwD7LYXPCJ4B7EjhiibtyLtv4/edit?usp=sharing>) a solution for centralized plugin configuration. Many plugins need access to API keys, preferences, or other settings. Historically, each plugin handled this differently, some by asking users to edit source files, while others used configuration files stored in various locations.

We’ll introduce a standardized approach in which plugins can declare their settings in ida-plugin.json, and the manager prompts users during installation. Settings are stored in a central location ($IDAUSR/ida-config.json) and can be managed through HCLI or (eventually) through a GUI within IDA itself. This means:

  * No more editing source code to configure plugins
  * Consistent configuration experience across all plugins
  * Easy backup and migration of settings



This infrastructure is actively being developed using the [ida-settings](<https://github.com/williballenthin/ida-settings>) library.

* * *

# **For Authors: Publishing Your Plugin**

The Plugin Manager ecosystem is growing rapidly, with dozens of plugins already available and more being packaged right now. If you've developed an IDA plugin, or are thinking about creating one, now is the perfect time to join the ecosystem. We think the Plugin Manager will bring some immediate benefits:

  * **Reach more users** : Discoverability through [plugins.hex-rays.com](<http://plugins.hex-rays.com/>) and hcli plugin search
  * **Simplified distribution** : No more writing manual installation instructions
  * **Automatic dependency installation**



Plus, upcoming initiatives will further boost visibility. The IDA Plugin Contest will require Plugin Manager-compatible submissions, meaning all contest entries will be immediately installable by the community. Every interesting tool from the contest will be just one command away.

By the way, the [Plugin Contributor Program](</contributor-program>) provides free IDA licenses to plugin researchers, developers, and maintainers who are actively building tools for the community. If you’d like to take part in the IDA plugin community, but don’t have a license, reach out to [contributor@hex-rays.com](<mailto:contributor@hex-rays.com>)!

# **The Publishing Process**

Publishing an IDA plugin via the Plugin Repository is self-service and requires three steps:

  1. Add ida-plugin.json metadata
  2. Package your plugin code
  3. Create a GitHub release



## **1\. Add****ida-plugin.json****Metadata**

As described on [docs.hex-rays.com](<https://docs.hex-rays.com/user-guide/plugins/plugin-submission-guide#define-plugin-metadata-with-ida-plugin.json>), each plugin needs an ida-plugin.json metadata file that describes your plugin. Here’s a basic example:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-20-19-0425-PM.png?width=1600&height=1310&name=undefined-Nov-20-2025-06-20-19-0425-PM.png)

The required fields are name, version, entryPoint, urls.repository, and authors. Everything else is optional but recommended for better discoverability. Additional fields let you target specific versions and/or platforms, show up in more search results, declare settings fields, and more.

For a complete reference of all available fields, see:

  * **Plugin Format Reference** : [hcli.docs.hex-rays.com/reference/plugin-packaging-and-format](<https://hcli.docs.hex-rays.com/reference/plugin-packaging-and-format>)
  * **IDA Plugin Documentation** : [docs.hex-rays.com/user-guide/plugins](<https://docs.hex-rays.com/user-guide/plugins>)



## **2\. Package Your Plugin**

**For Pure Python Plugins** (the easy path):

Most Python plugins require minimal work. Add ida-plugin.json to your repository, and GitHub's automatically generated source archive becomes your plugin archive. That's it.

If your plugin depends on external Python packages, declare them:

![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-20-36-6560-PM.png?width=1600&height=420&name=undefined-Nov-20-2025-06-20-36-6560-PM.png)

**For Native (C/C++) Plugins** :

Compiled plugins require building .dll, .so, and .dylib files for different platforms. GitHub Actions provides the best experience that I’ve found, though it's not strictly required - any CI/CD system that produces ZIP artifacts can work. Ultimately, you'll need to archive the native shared library with the ida-plugin.json metadata file and distribute it via GitHub Releases.

We provide workflow templates and are happy to help. See Hex-Rays-maintained [BinDiff build workflow](<https://github.com/HexRays-plugin-contributions/bindiff/blob/ci-gha/.github/workflows/build.yml>) as a reference implementation. It uses HCLI to fetch IDA SDKs, [ida-cmake](<https://github.com/allthingsida/ida-cmake>) for configuration, and builds across Windows, Linux, and macOS runners.

## **3\. Create a GitHub Release**

Tag your release following semantic versioning (e.g., v1.0.0), and attach any necessary build artifacts. For pure Python plugins, the source archive is often sufficient. For native plugins, attach the ZIP archives containing compiled assets.

The daily indexer will discover your plugin automatically.

**Note on Discovery:** The indexer uses GitHub's code search API to find repositories containing ida-plugin.json files. If you've published your plugin but it hasn't appeared after 24 hours, this may be due to GitHub’s indexing latency or other limitations. In these cases, you can explicitly register your repository by adding it to [known-repositories.txt](<https://github.com/HexRaysSA/plugin-repository/blob/v1/known-repositories.txt>) via pull request, which ensures your plugin is discovered on the next indexer run.

# **Testing Before Publishing**

Validate your plugin locally before releasing:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-20-37-0951-PM.png?width=1600&height=522&name=undefined-Nov-20-2025-06-20-37-0951-PM.png)

![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-20-37-7306-PM.png?width=1600&height=276&name=undefined-Nov-20-2025-06-20-37-7306-PM.png)

The linter catches common problems: missing required fields, JSON syntax errors, incorrect entry points, invalid dependency names, and more. Fix any issues before releasing. The indexer won't add invalid plugins to the repository, so the first step to triaging a missing plugin is to ensure there are no lint failures.

# **Resources for Authors**

  * **Packaging Guide** : [hcli.docs.hex-rays.com/reference/packaging-your-existing-plugin](<https://hcli.docs.hex-rays.com/reference/packaging-your-existing-plugin>)
  * **Plugin Format Reference** : [hcli.docs.hex-rays.com/reference/plugin-packaging-and-format](<https://hcli.docs.hex-rays.com/reference/plugin-packaging-and-format>)
  * **Architecture Details** : [hcli.docs.hex-rays.com/reference/plugin-repository-architecture](<https://hcli.docs.hex-rays.com/reference/plugin-repository-architecture>)
  * **Plugin Repository** : [github.com/HexRaysSA/plugin-repository](<https://github.com/HexRaysSA/plugin-repository>)
  * **Get Help** : Open an issue at [github.com/HexRaysSA/ida-hcli/issues](<https://github.com/HexRaysSA/ida-hcli/issues>)



Need assistance? We're here to help. The Hex-Rays team actively supports plugin authors through the packaging process. Contact us through the repository or reach out to [support@hex-rays.com](<mailto:support@hex-rays.com>).

* * *

# **Plugin Configuration: A Shared Settings System**

Many plugins require configuration: API keys for LLM services, custom paths, user preferences, etc. The Plugin Manager introduces a standardized configuration system that works consistently across all plugins.

## **How It Works**

When installing a plugin that requires settings, the Plugin Manager prompts you interactively:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-20-57-8544-PM.png?width=1600&height=457&name=undefined-Nov-20-2025-06-20-57-8544-PM.png)

For non-interactive environments (CI/CD, scripts), provide settings via command-line flags:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-20-57-4035-PM.png?width=1600&height=347&name=undefined-Nov-20-2025-06-20-57-4035-PM.png)

View and update plugin settings at any time:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-20-58-2887-PM.png?width=1600&height=631&name=undefined-Nov-20-2025-06-20-58-2887-PM.png)

Settings are stored in $IDAUSR/ida-config.json, making it easy for teams to script backups or version control their configurations. However, be careful: this file may contain sensitive data like API keys, so avoid committing it to public repositories.

**GUI Support Coming Soon:** We're developing a graphical settings editor that works directly within IDA. Install the preview with hcli plugin install ida-settings-editor to manage plugin configurations through a native IDA interface.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-21-15-5393-PM.png?width=1596&height=1272&name=undefined-Nov-20-2025-06-21-15-5393-PM.png)

## **For Plugin Authors**

Plugin authors declare their settings in ida-plugin.json:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-21-14-7635-PM.png?width=1600&height=902&name=undefined-Nov-20-2025-06-21-14-7635-PM.png)

Read settings in your plugin code using the [ida-settings](<https://github.com/williballenthin/ida-settings>) library:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-21-14-3764-PM.png?width=1600&height=352&name=undefined-Nov-20-2025-06-21-14-3764-PM.png)

This standardized approach ensures all plugins use the same configuration mechanism, so users never need to hunt through source files or scattered config directories. Settings are portable, meaning that you can export and import them. Future versions will support editing through IDA's native GUI, bringing everything into one place.

**Migrating existing plugins?** Adopting the centralized settings system may require refactoring how your plugin handles configuration. We're here to help - reach out through the [GitHub repository](<https://github.com/HexRaysSA/ida-hcli/issues>) or support channels, and we'll work with you to smooth the transition.

* * *

# **What's Next**

The Plugin Manager launches with a strong foundation, but we have further plans for the ecosystem.

## **Near-Term Roadmap**

**Native IDA GUI for Plugin Management** While HCLI provides command-line functionality, we're designing a GUI within IDA Pro itself. Browse, install, upgrade, and configure plugins without leaving your analysis environment. We may have to tweak the plugin lifecycle to enable plugins to be reloaded and/or upgraded within the same IDA session.

**Enhanced Settings Management** Building on the ida-settings foundation, we're expanding the centralized configuration system. The [ida-settings](<https://github.com/williballenthin/ida-settings>) project will enable richer configuration options and seamless integration with the IDA GUI. We'd love to support hierarchical settings, layered across the: IDB, workspace, user, and/or organization. Perhaps if this is successful, we can migrate more of IDA's settings management to a similar design.

**Offline Installation** Support for airgapped networks and virtual machines with Host-Only networking is coming. Export a plugin bundle on a connected machine, transfer it to your isolated environment, and install without internet access. We may also distribute an initial set of plugins with the IDA installer, so you can easily bootstrap common workflows.

## **The Growing Ecosystem**

The plugin repository expands weekly as authors package their tools and new developers join the community. We're seeing plugins across all categories: UI improvements ([ShowComments](<https://plugins.hex-rays.com/merces/showcomments>) and [deREferencing](<https://plugins.hex-rays.com/danigargu/deREferencing>)), mobile reversing ([iOSHelper](<https://plugins.hex-rays.com/yoavst/ida-ios-helper>)), firmware analysis ([efiXplorer](<https://plugins.hex-rays.com/binarly-io/efixplorer>)), vibe reversing ([gepetto](<https://plugins.hex-rays.com/JusticeRage/Gepetto>)), and more. 

This is just the beginning. The Plugin Manager provides a better foundation for an ecosystem where innovation can flourish. We're excited to see what the community builds.

* * *

# **Get Started Today**

### **For Users**

Explore the plugin ecosystem by visiting [plugins.hex-rays.com](<https://plugins.hex-rays.com/>) to browse available plugins in your browser. Then, install HCLI. For detailed installation instructions, see [Introducing HCLI](</blog/introducing-hcli>) or visit [hcli.docs.hex-rays.com](<https://hcli.docs.hex-rays.com/>).![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-21-35-7930-PM.png?width=1600&height=987&name=undefined-Nov-20-2025-06-21-35-7930-PM.png)

Then, install your first plugin:![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-21-35-2874-PM.png?width=1600&height=559&name=undefined-Nov-20-2025-06-21-35-2874-PM.png)

## **For Authors**

**Package your plugin:**

Read the packaging guide at [hcli.docs.hex-rays.com/reference/packaging-your-existing-plugin](<https://hcli.docs.hex-rays.com/reference/packaging-your-existing-plugin>)

**Validate before releasing:****![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-20-2025-06-21-49-8219-PM.png?width=1600&height=388&name=undefined-Nov-20-2025-06-21-49-8219-PM.png)****Join the ecosystem:**

Your users are waiting. Make your plugin discoverable, simplify installation, and reach more IDA Pro users than ever before.

**Need help?**

  * **Documentation** : [hcli.docs.hex-rays.com](<https://hcli.docs.hex-rays.com/>)
  * **Plugin Repository** : [github.com/HexRaysSA/plugin-repository](<https://github.com/HexRaysSA/plugin-repository>)
  * **Get Support** : [github.com/HexRaysSA/ida-hcli/issues](<https://github.com/HexRaysSA/ida-hcli/issues>)



The foundation is built. The ecosystem is growing. Now it's your turn to explore, install, and create.

---

## 4. 

**Date:** Posted: Nov 17, 2025  
**Link:** [https://hex-rays.com/blog/introducing-hcli](https://hex-rays.com/blog/introducing-hcli)

[Back](<https://hex-rays.com/blog>)

[IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [HCLI](<https://hex-rays.com/blog/tag/hcli>)

# Introducing HCLI: The Modern Command-Line Interface for IDA

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/introducing-hcli>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/introducing-hcli>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/introducing-hcli>)

Willi Ballenthin ✦ Posted: Nov 17, 2025

![Introducing HCLI: The Modern Command-Line Interface for IDA](https://hex-rays.com/hubfs/hcli-1.png)

If you've been working with IDA Pro for any length of time, you know the toil of maintaining a tidy reverse engineering workspace: manually downloading installers from the portal, hunting for the right license file, copying plugins into obscure directories, and wrestling with SDK paths when building native plugins. For years, these were just accepted as part of the IDA experience. Today, we're going to show you a better way.

We're excited to introduce [HCLI](<https://hcli.docs.hex-rays.com/>) \- a modern command-line interface for managing Hex-Rays software, licenses, and plugins. It's designed for both interactive use and automated workflows. Whether you're a reverse engineer setting up a new workstation with IDA or a tool developer building CI/CD pipelines, HCLI makes many things simpler.

HCLI makes it easier to install IDA and its license key, including a single command line to download and install headlessly - perfect for automated pipelines or Docker environments like GitHub Actions. Mix this with [idalib](<https://docs.hex-rays.com/user-guide/idalib>), and you have a really clean experience for building automated solutions on IDA's decompiler engine.

# **Let’s take HCLI for a spin:**

Installation is a single command:

**macOS and Linux:**

curl -LsSf https://hcli.docs.hex-rays.com/install | sh

**Windows:**

iwr -useb https://hcli.docs.hex-rays.com/install.ps1 | iex

HCLI installs as a standalone executable that just works.

**![](https://hex-rays.com/hs-fs/hubfs/undefined-1.png?width=1364&height=1022&name=undefined-1.png)**

Authentication is equally straightforward. For interactive use, simply run:

hcli login

Your browser opens, you sign in to [my.hex-rays.com](<http://my.hex-rays.com/>), and you're done. You can use either Google OAuth or receive a magic token to your email inbox. For automation, create an API key once:

hcli auth key create \--name "my-ci-pipeline"

Save the key to your CI/CD secrets as HCLI_API_KEY, and HCLI automatically picks it up - no configuration files needed.![](https://hex-rays.com/hs-fs/hubfs/undefined-2.png?width=1364&height=1414&name=undefined-2.png)**Example 1: New Workstation Setup**

Let's say you just got a new laptop and need to set up your IDA Pro environment, perhaps within [FLARE-VM](<https://github.com/mandiant/flare-vm>). The old way? Download installers, click through wizards, hunt down license files, manually install plugins. The new way with HCLI - much easier.

First, check what licenses you have available:

hcli license list

HCLI displays a table with all your licenses, their status, expiration dates, and available add-ons like [Teams](</teams>) and [Private Lumina](</lumina>).**![](https://hex-rays.com/hs-fs/hubfs/undefined-3.png?width=1364&height=826&name=undefined-3.png)**

Now, let's interactively browse the downloads available to you:

hcli download

HCLI presents a textual interface where you navigate through folders using arrow keys. Choose "release" → "9.2" → "ida-pro" → and select your platform. HCLI downloads it and shows you progress bars. Of course, if you know the resource you want, you can provide this as a CLI argument.**![](https://hex-rays.com/hs-fs/hubfs/undefined-4.png?width=1364&height=798&name=undefined-4.png)**

But here's where it gets really good. Install IDA with a single command:

hcli ida install \

\--set-default \

\--accept-eula \

\--yes \

\--license-id 96-0000-0000-01 \

\--download-id ida-pro:latest

No GUI dialogs. No clicking "Next" eight times. HCLI downloads the installer (if you didn't already), extracts it, installs it to a standard location, fetches your license file, installs the license, and marks this as your default IDA installation. **![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-14-2025-01-07-13-2122-AM.png?width=1364&height=1190&name=undefined-Nov-14-2025-01-07-13-2122-AM.png)**

Let's discover and install plugins. Want to see what's available?

hcli plugin search

HCLI shows you every plugin in the central repository, indicating which ones are already installed, which have available upgrades, and which are compatible with your IDA version.**![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-14-2025-01-07-57-0789-AM.png?width=1364&height=938&name=undefined-Nov-14-2025-01-07-57-0789-AM.png)**

Now, as a sneak peek into our next big initiative, you can use HCLI to install plugins, too:

hcli plugin install capa

Done. HCLI downloads the plugin, installs any Python dependencies it requires, and places everything in the right location. Next time you open IDA, it's there.

Want to see what's installed?

hcli plugin status

This shows all your plugins, upgrade availability, and warns about incompatible or legacy plugins that might need attention.**![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-14-2025-01-08-16-5049-AM.png?width=1364&height=434&name=undefined-Nov-14-2025-01-08-16-5049-AM.png)****Example 2: idalib in Docker for Automated Analysis**

Here's where HCLI really shines: automation. Let's say you're building a tool that uses IDA to analyze binaries. A reasonable way to package and deploy this is in a Docker container. Previously, this was tedious to get just right - you'd need to manually download IDA, figure out how to install it without a GUI (or temporarily with an X Server that you subsequently uninstall), handle license installation, quickly tag a Docker image or VM snapshot, and hope nothing broke.

With HCLI, your Dockerfile can look like this: [Dockerfile](<https://github.com/HexRaysSA/ida-hcli/blob/main/docs/advanced/docker/Dockerfile>), and you can build and run with:

docker build \

\--build-arg HCLI_API_KEY=${HCLI_API_KEY} \

\--build-arg IDA_LICENSE_ID=${IDA_LICENSE_ID} \

-t my-ida-analyzer \

.

****

docker run \--rm my-ida-analyzer

****

Tip: please take a look at [idalib](<https://docs.hex-rays.com/user-guide/idalib>), which allows you to use the C++ and IDA Python APIs outside IDA as standalone applications, as well as the new [IDA Domain API](<https://ida-domain.docs.hex-rays.com/>):

****

import sys

from ida_domain import Database

****

with Database() as db:

if db.open(sys.argv[1]):

for func in db.functions:

print(f'{func.name}')****![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-14-2025-01-13-05-7839-AM.png?width=1364&height=1022&name=undefined-Nov-14-2025-01-13-05-7839-AM.png)****

We’re excited to see what you build with idalib now that it’s easy to install and configure. We’ll share some of our own ideas in upcoming blog posts. 

Note that all IDA users are welcomed to use idalib to develop local tools and scripts; however, using idalib to support many users, such as a web service, requires an [OEM license](</ida-pro-oem>). OEM licenses are free during development, research or beta phases. If this interests you, please get in touch!

# **Example 3: Building Native Plugins with the SDK**

If you develop native plugins in C or C++, you know the pain of building and distributing across versions and platforms. With HCLI managing the download and installation and CI/CD providers like GitHub Actions offering a matrix of build environments ({Windows, Linux, and macOS} x {x86 and aarch64}), we think it becomes a lot easier to build and share native IDA plugins.

For example, [here's](<https://github.com/HexRaysSA/goomba/blob/main/.github/workflows/build.yml>) how we've configured GitHub Actions to build [gooMBA](</blog/deobfuscation-with-goomba>). The key lines are: 

jobs:

build:

runs-on: $

strategy:

matrix:

environment:

- os: ubuntu-latest

z3_url: https://github.com/Z3Prover/z3/releases/download/z3-4.15.3/z3-4.15.3-x64-glibc-2.39.zip

********

- os: windows-latest

z3_url: https://github.com/Z3Prover/z3/releases/download/z3-4.15.3/z3-4.15.3-x64-win.zip

********

- os: macos-latest

z3_url: https://github.com/Z3Prover/z3/releases/download/z3-4.15.3/z3-4.15.3-arm64-osx-13.7.6.zip

********

- name: Download IDA SDK

run: |

uvx \--from ida-hcli hcli \--disable-updates download "release/9.2/sdk-and-utilities/idasdk92.zip"

unzip idasdk*.zip -d ./ida-temp/

mv ./ida-temp/src/ ./ida-sdk

env:

HCLI_API_KEY: $

********

- name: Fetch Z3

run: |

curl -L -o z3.zip $

unzip z3.zip -d z3-temp

********

mv z3-temp/*/include/ ida-sdk/plugins/goomba/z3/include

mv z3-temp/*/bin/ ida-sdk/plugins/goomba/z3/bin

- name: Build gooMBA

working-directory: ./ida-sdk/plugins/goomba

run: |

make __EA64__=1

With this workflow, we can build and test a new version of gooMBA for Windows, macOS, and Linux with each commit, PR, or update to the open source IDA SDK. As we tag releases in GitHub, the build artifacts are attached for easy distribution. This is much easier than manually running commands within virtual machines or dedicated hardware to update, build, and package plugins.

![](https://hex-rays.com/hs-fs/hubfs/undefined-Nov-14-2025-01-13-41-6603-AM.png?width=1600&height=827&name=undefined-Nov-14-2025-01-13-41-6603-AM.png)

We've contributed similar setups for other open source plugins, too, like:

  * [milankovo/zydisinfo](<https://github.com/milankovo/zydisinfo/blob/main/.github/workflows/build.yml>)
  * [HexRaysSA/goomba](<https://github.com/HexRaysSA/goomba/blob/main/.github/workflows/build.yml>)
  * [HexRays-plugin-contributions/bindiff](<https://github.com/HexRays-plugin-contributions/bindiff/blob/ci-gha/.github/workflows/build.yml>)



# **Why This Matters**

HCLI represents a fundamental shift in how Hex-Rays is building tools for the reverse engineering community. We're moving from occasional major releases to continuous delivery of useful utilities that make your daily work easier.

Found a bug? Have a feature request? Open an issue on [GitHub](<https://github.com/HexRaysSA/ida-hcli/issues>). HCLI is under active development, and we're committed to rapidly responding to your feedback. We're not just building tools - we're building them WITH you.

The benefits extend beyond individual convenience:

  * Training: Security training courses can automate participant setup
  * Enterprise: Organizations can standardize IDA installations across teams
  * Tool Development: Researchers can build robust IDA-based tooling with confidence



# **Try It Today**

Getting started takes less than a minute:

# Install HCLI

curl -LsSf https://hcli.docs.hex-rays.com/install | sh

# Authenticate

hcli login

# Explore

hcli download

hcli plugin search

hcli license list

# Install IDA

hcli ida install \--help

Complete documentation is available at [hcli.docs.hex-rays.com](<https://hcli.docs.hex-rays.com/>), including:

  * [Quick Start Guide](<https://hcli.docs.hex-rays.com/getting-started/quick-start/>)
  * [CI/CD Integration Examples](<https://hcli.docs.hex-rays.com/advanced/ci-cd-integration/>)
  * [Plugin Packaging Guide](<https://hcli.docs.hex-rays.com/reference/packaging-your-existing-plugin/>)



# **Looking Forward**

HCLI is just the beginning. We're actively working on:

  * GUI integration for plugin management directly within IDA
  * Enhanced plugin configuration management
  * More automation capabilities for enterprise environments
  * Additional utilities based on community feedback



This is Hex-Rays' commitment to you: providing tools that respect your time, enable your creativity, and remove friction from your reverse engineering workflow.

---

## 5. 

**Date:** Posted: Nov 13, 2025  
**Link:** [https://hex-rays.com/blog/blackhoodie-interview-2025](https://hex-rays.com/blog/blackhoodie-interview-2025)

[Back](<https://hex-rays.com/blog>)

[Community Spotlight](<https://hex-rays.com/blog/tag/community-spotlight>)

# BlackHoodie Interview: Building Community, Opportunity, & Confidence

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/blackhoodie-interview-2025>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/blackhoodie-interview-2025>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/blackhoodie-interview-2025>)

Justine Benjamin ✦ Posted: Nov 13, 2025

![BlackHoodie Interview: Building Community, Opportunity, & Confidence](https://hex-rays.com/hubfs/blackhoodie-1.png)

From a fateful “failed class” to founding one of the most inclusive communities in cybersecurity, **Marion Marschalek** has built her career on turning challenges into opportunities. A reverse engineer, educator, and founder of **BlackHoodie** , Marion has spent over a decade empowering women and nonbinary people to enter and thrive in the field. In this interview, she shares how she got started, the pivotal moments that shaped her advocacy, and how BlackHoodie continues to grow as a global movement for skill-building, mentorship, and inclusion.

# **Interview**

## _**JB: What drew you into technology and eventually cybersecurity?**_

**Marion:** Let’s start with the school system In Austria - where I am from. Basically, at the age of 14 we can make the decision of which direction our career should go. At the time I had the choice of continuing in general education, economics or engineering. And I remember specifically looking at the engineering schools and hearing my brother tell me that I wouldn't be able to do that.

## _**JB: And let me guess, that fueled you to want to do it more?**_

**Marion:** Yeah. I wasn’t sure at the time, but after he said that, I thought, “Well, let me show you.” We've always had a bit of a rivalry, and that comment may have been the best thing he's ever done for me. 

So I looked at the engineering schools and found computer science to be particularly peculiar. I liked computers but at the time I had no idea how they worked. I was intrigued and back in the day the internet was still something new and exciting. 

After that engineering school, I moved on to University and one of my previous teachers was heading the cyber security department. I thought, “Alright, I like him, I like the subject; let’s do that.” And that’s how I got into security.

## _**JB: Where does reverse engineering come in?**_

**Marion:** Funny enough, what got me into**** reverse engineering, like malware, happened in my last year at University. I did an exchange semester and failed a subject. In order to gain those missed credits, I agreed to do a project with an antivirus company located in Vienna. And that antivirus company offered me a job after I graduated. 

> **“That failure led to success. It was a string of lucky coincidences, I suppose.”**

When I started at the antivirus company, I was a malware analyst but didn’t actually know how to reverse engineer. Other analysts in the lab knew how to, and I was very fascinated by it, but there weren't any classes or training available to learn. 

Then one of my colleagues sent me a link to a women’s reverse engineering challenge, also with a smirk on his face. I joined the challenge, reverse engineered the sample, and sent in a report. **And then I won**. 

The challenge was organized by Thomas Dullien, also known as Halvar Flake, a Google engineer at the time and veteran of the binary analysis space. The first prize was a trip to a conference in Singapore. That trip changed my life. I met Thomas, met others in the field, and found a community that was welcoming and supportive. Through those connections, I got access to more conferences, more learning opportunities, and eventually, a more profound job that kickstarted my career.

## _**JB: Was there a specific moment or experience that shaped your decision to advocate for women in cybersecurity?**_

**Marion:** The women’s reverse engineering challenge was that moment. Somebody stepped up and showed me that I am wanted and needed in this community. The warm welcome I received when I showed up to my first conferences where people were excited that I was around… that showed me that the lack of women is not necessarily because there is no space for us. That’s just what we needed to give us this confidence to step into those spaces and claim our own spot.  
  
**BlackHoodie** was inspired by this supportive community (and that challenge).

## _**JB: For those unfamiliar, what is BlackHoodie?**_

**Marion:** BlackHoodie is a women’s reverse-engineering community that started as a small experiment back in 2015. At the time, I was doing a lot of research and traveling to conferences. I was giving a talk at a conference in Austria about banking Trojans — showing how I reverse-engineered them and explaining how they worked. Afterward, a woman approached me and said she found it fascinating and wanted to learn how to do that herself.

I told her, “Let’s sit down for a weekend and I’ll show you.” I wrote a short blog post inviting other women who might be interested to join. I honestly expected maybe one or two; instead, **fifteen women contacted me and showed up — literally traveling out to the countryside of Austria** , far from anything. 

We spent the whole weekend together, reverse-engineering malware, learning, talking, laughing, and sharing food. I kept in touch with many of them afterward — some became close friends, and a few even stayed in the field. One of those first-year participants recently started a senior engineering position at Apple.

I don’t like to take credit for their success; what I did was simply give them a place to start, show them the tools, and let them take it from there. But that first weekend showed me how powerful it can be to create a space where people feel comfortable learning together. That’s really how BlackHoodie was born.

## _**JB: How has BlackHoodie evolved since those early days?**_

**Marion:** The first year we had 15 participants and no plan of creating a series. The next year, we had 30. By year three, there were too many to fit in one room. In year four, we decided that we needed multiple events. That’s when we started **regional BlackHoodie events** , hosted at conferences or universities that offered space. 

I think the goals and missions have reshaped over the years. Initially it was to teach women how to reverse engineer - and it still is. Then we realized we're building community - and we still do. And I think eventually I realized it's about opportunity as well. Participants were finding not just knowledge, but **friends, mentors, and professional networks**. It became clear that what we were really building was a community, a place where people could belong, learn, and find opportunities.

## _**JB: What challenges have you faced entering and navigating the industry?**_

**Marion:** Honestly, I feel like people rolled out the red carpet when I came along. There was an interesting balance between difficulties and opportunities. And I think the red carpet experience started at the first challenge where people suddenly believed that I had a skill and thus a spot in this community.

Sure there are moments where people rolled rocks in my path, but with the support of the community, that was easy to navigate and I came to realize it's all about community. 

> Sure there are moments where people rolled rocks in my path, but with the support of the community, that was easy to navigate and I came to realize it's all about community.

It's about having friends and having that support kept me from being scared of facing challenges._  
_

## _**JB: What are the guiding pillars of BlackHoodie today?**_

**Marion:** Over the years, I’ve realized that success in any technical field rests on three pillars:  


  1. **Skills.** We can teach those.
  2. **Self-motivation.** That has to come from within.
  3. **Opportunity.** That’s where community comes in.



Beyond training, we started looking for ways to provide opportunities — free training seats, conference tickets, mentorship programs, job introductions. The goal is to ensure talented people not only gain knowledge but also have access to the spaces where they can use it.

## _**JB: Do you find yourself having to balance between developing the skill set and creating an inclusive environment? Or do they naturally flow together, where one leads to the other?**_

**Marion:** They go hand in hand. Building technical skills often means long hours alone at your computer. But it’s hard to learn if you don’t feel comfortable in your environment, in your own skin.

So if you’re surrounded by people who look like you, behave like you, and communicate like you, then you’re automatically more comfortable. It's a complicated topic. I don't want to go too deep into it, but it's not just like that for women. It works the same way for men, people of color, and any other group, especially marginalized groups.

## _**JB: How do you define success for BlackHoodie?**_

**Marion:** Success is in the **community itself**. People don’t just attend - they come back, they volunteer, they bring others. Many of our alumni stay active on our Discord server, sharing job opportunities, conference info, and mentorship. Everyone brings their own network, and when those networks combine, the result is powerful.

> Seeing participants return as speakers, trainers, or mentors… that’s success.

## _**JB: What advice would you give companies that want to meaningfully support underrepresented groups?**_

**Marion:** The first thing that comes to mind is a company’s culture, it matters in attracting and retaining women in their workforce. 

In companies like Intel and Amazon, inclusivity was built into the culture, and leadership enforced it. People treated each other with respect, and that created trust. I’ve also worked at places where certain behaviors were ignored or excused, and that eroded trust almost instantly.

If companies want to attract and retain diverse talent, they need to make sure that when people walk through the door, they feel genuinely **welcome and safe to stay**.

## _**JB: How can people or organizations get involved?**_

**Marion:** There are so many ways: hosting a training, offering mentorship, sharing job openings, providing travel support, or making a donation. Since we’re a registered nonprofit, financial contributions are tax-deductible.

But the best first step is simple, **reach out**. Every collaboration begins with a conversation.

## _**JB: Finally, what message do you want readers to take away?**_

**Marion:** Make this community a better place for everyone.

## **Closing Thoughts**

Marion’s journey shows how one opportunity — or even one “failure” — can open the door to an entire career. Her story captures the essence of what makes cybersecurity communities thrive: curiosity, support, and a willingness to lift others along the way. Through BlackHoodie, she’s turned that ethos into a global network where the next generation of reverse engineers can learn, connect, and belong.

➥ Learn more about BlackHoodie: <https://blackhoodie.re/>

➥ Get involved today: <https://blackhoodie.re/contact/>

---

## 6. 

**Date:** Posted: Oct 24, 2025  
**Link:** [https://hex-rays.com/blog/2025-sponsored-ctf-teams](https://hex-rays.com/blog/2025-sponsored-ctf-teams)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [CTF](<https://hex-rays.com/blog/tag/ctf>)

# Announcing the 2025 Hex-Rays Sponsored CTF Teams

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/2025-sponsored-ctf-teams>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/2025-sponsored-ctf-teams>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/2025-sponsored-ctf-teams>)

Justine Benjamin ✦ Posted: Oct 24, 2025

![Announcing the 2025 Hex-Rays Sponsored CTF Teams](https://hex-rays.com/hubfs/CTF.png)

In 2025, we launched our first-ever **CTF Team Sponsorship Program** to recognize and support standout teams in the global Capture The Flag (CTF) community. Our goal is to empower skilled competitors who embody the spirit of curiosity, collaboration, and technical excellence — values that have long defined the reverse engineering community.

Through this program, Hex-Rays provides three top tier teams and one rising star team with **IDA Pro licenses** , **travel support for in-person competitions** , and opportunities to share their expertise through research, write-ups, and challenge creation.

After reviewing dozens of outstanding applications from around the world, we’re proud to introduce the **inaugural class of Hex-Rays Sponsored Teams**.

* * *

# **r3kapig**

**CTFtime:**[ https://ctftime.org/team/58979  
](<https://ctftime.org/team/58979>)

Formed in 2018 through the merger of **Eur3kA** and **FlappyPig** , r3kapig has become one of the most dominant and respected teams in the global CTF arena. With **eight consecutive DEF CON Finals appearances** and (at the time of this post) a **#1 worldwide ranking** , the team represents the pinnacle of technical skill and coordination.

r3kapig has a strong history of sharing detailed [write-ups](<https://r3kapig.com/writeup/>) and research with the community, building an impressive archive of challenges and technical insights over the years.

We’re excited to support r3kapig’s continued pursuit of excellence as they compete around the world in 2025.

# **Kalmarunionen**

**CTFtime:**[ https://ctftime.org/team/114856  
](<https://ctftime.org/team/114856>)

Founded in 2020, **Kalmarunionen** unites top players across Denmark, Sweden, and Norway with the goal of building a strong Northern European CTF community. In just five years, they’ve grown from rank #66 to a consistent top-three global standing, finishing **#1 in 2024** and maintaining **#2 in 2025**(at the time of this post).

Beyond their competition success, the team also **organizes KalmarCTF** , a well respected event, known for its original, high-quality challenges. They actively mentor new players, run workshops, and collaborate with universities and industry partners to develop the next generation of reverse engineers. Check out their [website](<https://www.kalmarunionen.dk/>) for the latest and greatest.

We’re proud to continue supporting Kalmarunionen’s outstanding contributions to the technical and educational side of the CTF community.

## **justCatTheFish**

**CTFtime:**[ https://ctftime.org/team/33893  
](<https://ctftime.org/team/33893>)

**justCatTheFish** is Poland’s premier cybersecurity competition team and a consistent top performer globally. Started as the merger of two university-based teams from **AGH UST** and **UWr** , they’ve quickly become a fixture among the world’s best — placing **4th overall on CTFtime in 2023**.

In addition to their competitive success, the team organizes **justCTF** , an annual international competition recognized for its creative and technically demanding challenges. The event attracts some of the top CTF teams worldwide and exemplifies their commitment to advancing the community’s technical excellence.

justCatTheFish’s members are known for their strong presence in the open-source, research and bug bounty space. They maintain the [Pwndbg](<https://github.com/pwndbg/pwndbg>) (GDB+LLDB plugin) and [Pwntools](<https://github.com/Gallopsled/pwntools/>) (CTF swiss-army knife) open source projects, published lots of research like [XS-Leaks](<https://terjanq.medium.com/massive-xs-search-over-multiple-google-products-416e50dd2ec6>) or [cstrnfinder](<https://github.com/disconnect3d/cstrnfinder>), found interesting [Steam](<https://www.techspot.com/news/90805-valve-pays-researcher-7500-discovering-exploit-could-add.html>) and [Shopify](<https://hackerone.com/haqpl?type=user#:~:text=%24200%2C000-,Bug%20reported%20by,was%20awarded%20a%20bounty,-2%20years%20ago>) bugs. Some of their members also have blogs or a [YouTube channel](<https://www.youtube.com/c/BugBountyReportsExplained/1000>) about bug bounty reports. Also check out their write-ups for CTF challenges on [GitHub!](<https://github.com/justcatthefish/ctf-writeups>)

Their combination of technical leadership, high-quality content, and community engagement made them a natural choice for inclusion in the inaugural Hex-Rays sponsorship program.

# **PwnSec**

**CTFtime:**[ https://ctftime.org/team/28797  
](<https://ctftime.org/team/28797>)

A rising star in the international scene, **PwnSec** was founded in 2022 at Amman, Jordan and has rapidly climbed the ranks from #206 in 2023 to **#38 globally in 2025**(at the time of this post)! With more than 40 competitions per year, PwnSec has built a reputation for consistency, technical depth, and teamwork. Today, PwnSec is proud of its diverse, international fabric with over 50 team members across 14+ nationalities.

The team’s members are active contributors to the broader security community, publishing research and [write-ups](<https://blog.elmosalamy.com/>) across multiple platforms and even organizing their own CTF event, **PwnSec CTF**. Their culture of mentorship and continuous learning makes them a perfect fit for Hex-Rays’ mission to support skill development and collaboration in reverse engineering.

* * *

## **Supporting the Global CTF Ecosystem**

Each sponsored team receives:

  * IDA Expert-6 licenses for their team
  * Funds for traveling to in-person CTFs
  * Team + Hex-Rays co-branded swag
  * Participation in the Hex-Rays Power User Feedback and Beta Program
  * And more…



In return, teams share their insights and creativity with the broader community by publishing content showcasing IDA, creating new CTF challenges for the community and more.

* * *

We’re inspired by the dedication, skill, and community focus of all four teams and we can’t wait to see what they achieve in the coming sponsorship year!

Follow their journeys on [CTFtime](<https://ctftime.org/>) and across social media, and stay tuned for updates on their progress throughout the 2025-2026 season.

---

## 7. 

**Date:** Posted: Oct 22, 2025  
**Link:** [https://hex-rays.com/blog/-ida-plugin-contest-2025](https://hex-rays.com/blog/-ida-plugin-contest-2025)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [IDA Pro Plugin](<https://hex-rays.com/blog/tag/ida-pro-plugin>) [Contest](<https://hex-rays.com/blog/tag/contest>)

# IDA Plugin Contest Returns

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/-ida-plugin-contest-2025>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/-ida-plugin-contest-2025>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/-ida-plugin-contest-2025>)

Justine Benjamin ✦ Posted: Oct 22, 2025

![IDA Plugin Contest Returns](https://hex-rays.com/hubfs/ida-plugin-contest-hero.png)

# Our annual IDA Plugin Contest is back with **bigger rewards and new perks!**

If you’ve built something creative, powerful, or just plain cool with IDA or the Hex-Rays Decompiler, this is your chance to showcase it. Whether it’s a Python script, a C/C++ plugin, or a side app using **idalib** , we want to see it.

NOTE: Now that the [IDA SDK is open-sourced](</blog/open-sourcing-ida-sdk>), IDA Free users are able to submit to the contest. An active IDA Pro license is not required.

**Contest highlights:**

  * Submissions accepted from **15 October 2025** to**15 January 2026**

  * Open to all **active IDA license holders**

  * **Cash prizes:** $5,000 / $2,500 / $1,000

  * Plus: **free licenses** and a **conference pass** for the top winner




Entries are judged by a Hex-Rays review and voting committee for creativity, performance, and overall coolness.

**Submission Deadline:** January 15, 2026 (CET)  
**Learn More & Submit:** [hex-rays.com/annual-plugin-contest](</annual-plugin-contest>)

---

## 8. 

**Date:** Posted: Oct 7, 2025  
**Link:** [https://hex-rays.com/blog/streamlining-vulnerability-research-idalib-rust-bindings](https://hex-rays.com/blog/streamlining-vulnerability-research-idalib-rust-bindings)

[Back](<https://hex-rays.com/blog>)

[rust](<https://hex-rays.com/blog/tag/rust>) [SDK](<https://hex-rays.com/blog/tag/sdk>) [idalib](<https://hex-rays.com/blog/tag/idalib>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>)

# Streamlining Vulnerability Research with the idalib Rust Bindings for IDA 9.2

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/streamlining-vulnerability-research-idalib-rust-bindings>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/streamlining-vulnerability-research-idalib-rust-bindings>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/streamlining-vulnerability-research-idalib-rust-bindings>)

Marco Ivaldi, HN Security ✦ Posted: Oct 7, 2025

![Streamlining Vulnerability Research with the idalib Rust Bindings for IDA 9.2](https://hex-rays.com/hubfs/vulnerabilities.jpg)

_“The attack surface is the vulnerability._

_Finding a bug there is just a detail.”_ _  
_

— Mark Dowd

A [previous version](<https://hnsecurity.it/blog/streamlining-vulnerability-research-with-ida-pro-and-rust/>) of this article was published on the [HN Security blog](<https://hnsecurity.it/blog/>) in February 2025. This is an updated and revised version.

# **About the guest author**

Marco Ivaldi ([raptor](<https://0xdeadbeef.info/>)) is a seasoned security researcher and tech leader with over 25 years in offensive security. He is the technical director and co-founder of [HN Security](<https://hnsecurity.it/>), a boutique firm specializing in tailored security assessments. Known as a prolific exploit writer, Marco is also a Phrack author and a core developer of the OSSTMM, the international standard for security testing. He has been recognized as a Microsoft Most Valuable Security Researcher and has competed as a Zero Day Quest hacker. His journey began in the 1990s, when he co-founded Linux&C, the first Italian magazine dedicated to Linux and open source.

# **TL;DR**

Following the release of [IDA Pro 9.2](</blog/ida-9.2-release>), I updated my tools that were built to assist with **reverse engineering and vulnerability research against binary targets** :

  * [rhabdomancer](<https://github.com/0xdea/rhabdomancer>): a headless IDA Pro plugin that **locates calls to potentially insecure API functions** in a binary file. Auditors can backtrace from these points to find pathways allowing access from untrusted input.
  * [haruspex](<https://github.com/0xdea/haruspex>): a headless IDA Pro plugin that **extracts pseudocode** generated by IDA Pro’s decompiler in a format suitable for import into an IDE or to be parsed by static analysis tools such as Semgrep or weggli.
  * [augur](<https://github.com/0xdea/augur>): a headless IDA Pro plugin that **extracts strings and related pseudocode** from a binary file. It stores pseudocode of functions that reference strings in an organized directory tree.  
  




These plugins are based on my [previous work](<https://hnsecurity.it/blog/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/>) and leverage Hex-Rays’ [idalib](</blog/4-powerful-applications-of-idalib-headless-ida-in-action>) alongside Binarly’s [idiomatic Rust bindings](<https://github.com/binarly-io/idalib>) for the IDA SDK to achieve a **blazing** **fast, headless user experience.** They have**** assisted me in finding **real-world vulnerabilities at scale**.

# **Introducing the award-winning idalib Rust bindings**

Last year, after approaching [Rust](<https://hnsecurity.it/blog/tag/rust/>) and having explored some basic [offensive ](<https://hnsecurity.it/blog/learning-rust-for-fun-and-backdoo-rs/>)[applications](<https://hnsecurity.it/blog/an-offensive-rust-encore/>), I decided it was time for **my first serious Rust project**. When [idalib v0.1.0](<https://crates.io/crates/idalib/0.1.0%2B9.0.240930>) was announced, I had an idea: port to IDA Pro some of my original [Ghidra scripts](<https://github.com/0xdea/ghidra-scripts>) that aim to **streamline vulnerability research** , using Rust! 🦀💡

> Our REsearch team is thrilled about the new IDA v9.0! [#efiXplorer](<https://twitter.com/hashtag/efiXplorer?src=hash&ref_src=twsrc%5Etfw>) is fully compatible with v9.0 and still supports IDA v8.4🚀  
> 🔬<https://t.co/WHYGifmjGS>  
>   
> We are thrilled to announce IDAlib — idiomatic Rust bindings for the IDA SDK 🎉 Kudos to [@xorpse](<https://twitter.com/xorpse?ref_src=twsrc%5Etfw>)!  
> ⚙️<https://t.co/PLoNkf8sQn> [pic.twitter.com/J1no6oFatO](<https://t.co/J1no6oFatO>)
> 
> — BINARLY🔬 (@binarly_io) [October 1, 2024](<https://twitter.com/binarly_io/status/1841176852964204561?ref_src=twsrc%5Etfw>)

Binarly’s **idalib Rust bindings** allow [IDA Pro 9.x](<https://docs.hex-rays.com/release-notes/9_0#headless-processing-with-idalib>) users to **develop standalone analysis tools** with the [IDA SDK](<https://cpp.docs.hex-rays.com/>), using Rust in an idiomatic way and fitting Rust's ownership model, type system, and API conventions. Tool authors can leverage the entire Rust ecosystem, so IDA Pro can be easily combined with existing Rust libraries and tools.

The availability of [idalib](<https://github.com/binarly-io/idalib>) marked the start of a new chapter in my Rust journey that saw me publish **new vulnerability research tools** built on top of it, and contribute a number of [new features](<https://github.com/binarly-io/idalib/pulls?q=author%3A0xdea>) to idalib itself. And I learned a lot in the process!

My main**idalib contributions** to date are:

  * Support for the [comments](<https://github.com/binarly-io/idalib/pull/1>) API.
  * Support for the (mostly undocumented) [bookmarks](<https://github.com/binarly-io/idalib/pull/4>) APIs.
  * Support for [searching](<https://github.com/binarly-io/idalib/pull/12>) text and immediate values, which incidentally led me to discover [a curious bug](<https://github.com/binarly-io/idalib/pull/12#issuecomment-2476708362>) in IDA Pro.
  * Support for working with the list of [strings](<https://github.com/binarly-io/idalib/pull/23>) present in a binary file.
  * Various improvements and bug reports.



> We [@binarly_io](<https://twitter.com/binarly_io?ref_src=twsrc%5Etfw>) have just released idalib v0.2.0, an update to our [@HexRaysSA](<https://twitter.com/HexRaysSA?ref_src=twsrc%5Etfw>) IDASDK Rust bindings. It includes many new features: bookmarks, comments, and plugins APIs, hex-rays support, and documentation!<https://t.co/FlBo1obu3p>  
>   
> Thanks to our contributors: [@yeggorv](<https://twitter.com/yeggorv?ref_src=twsrc%5Etfw>), [@0xdea](<https://twitter.com/0xdea?ref_src=twsrc%5Etfw>)!
> 
> — Sam Thomas (@xorpse) [November 13, 2024](<https://twitter.com/xorpse/status/1856548604007133499?ref_src=twsrc%5Etfw>)

Fast-forward to last February, the idalib Rust bindings were **awarded** third place in the annual [Hex-Rays Plugin Contest](</blog/2024-plugin-contest-winners>). 🎊 It was a well-deserved recognition, and I am happy to have been a humble contributor.

> We are thrilled to announce the winners of the 2024 Hex-Rays Plugin Contest!  
>   
> 🥇1st Place: hrtng  
> 🥈2nd Place: aiDAPal  
> 🥉3rd Place: idalib Rust bindings  
>   
> Check out our reviews of the winners and other notable submissions here: <https://t.co/XgkQHfktAF>  
> Huge thank you to all… [pic.twitter.com/rw1qzmLjdf](<https://t.co/rw1qzmLjdf>)
> 
> — Hex-Rays SA (@HexRaysSA) [February 17, 2025](<https://twitter.com/HexRaysSA/status/1891516388742766720?ref_src=twsrc%5Etfw>)

The maintainers of idalib are wonderful people, and I **encourage you to contribute** to its development. In particular, [Sam](<https://github.com/xorpse/>) has published related repositories worth exploring:

  * [parascope](<https://github.com/xorpse/parascope>): a [weggli](<https://github.com/weggli-rs/weggli>) ruleset scanner for source code and binaries.
  * [weggli-ruleset](<https://github.com/xorpse/weggli-ruleset>): a utility crate to manage weggli patterns, such as [those I published](<https://hnsecurity.it/blog/a-collection-of-weggli-patterns-for-c-cpp-vulnerability-research/>) last year.
  * [wegglix](<https://github.com/xorpse/weggli>): a modified fork of weggli designed for a more pleasant library experience.

  


It's now time to take a look at my tools and see how they fare against some **real-world targets**!

# **Spinning rhabdomancer against a legendary target**

_Rhabdomancer_ _  
_/răb′də-măn″sər/  
Someone who uses a divining rod to find underground water.

[Rhabdomancer](<https://github.com/0xdea/rhabdomancer>) is the Rust port of [one](<https://github.com/0xdea/ghidra-scripts/blob/main/Rhabdomancer.java>) of my original [Ghidra scripts](<https://github.com/0xdea/ghidra-scripts>), previously described in the article [Automating binary vulnerability discovery with Ghidra and Semgrep](<https://hnsecurity.it/blog/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/>).

It is an **IDA Pro headless plugin** that **locates calls to potentially insecure API functions** in a binary file. Auditors can backtrace from these candidate points to find pathways allowing access from untrusted input. Its main features are:

  * **Blazing fast, headless user experience** courtesy of IDA Pro 9.x and Binarly’s idalib Rust bindings.
  * Support for C/C++ binary targets compiled for **any architecture supported by IDA Pro**.
  * **Bad API function call locations** are printed to stdout and marked in the IDB.
  * Known bad API functions are **grouped into tiers of severity** to help prioritize auditing: 
    * [BAD 0] High priority — functions that are generally considered insecure.
    * [BAD 1] Medium priority — interesting functions that should be checked for insecure use cases.
    * [BAD 2] Low priority — code paths involving these functions should be carefully reviewed.
  * The list of known bad API functions can be **easily customized** by editing _conf/rhabdomancer.toml_.  
  




Additional information on rhabdomancer’s features and usage are available on [crates.io](<https://crates.io/crates/rhabdomancer>) and in the official [documentation](<https://0xdeadbeef.info/rhabdomancer/rhabdomancer/>).

### **Let’s install and take it for a spin!**

Since the release of [IDA Pro 9.2](</blog/ida-9.2-release>) and its freshly open-sourced [SDK](<https://github.com/HexRaysSA/ida-sdk>), installation has become much simpler:

  1. Download, install, and configure IDA Pro ([https://hex-rays.com/ida-pro](</ida-pro>)).
  2. Install LLVM/Clang ([https://rust-lang.github.io/rust-bindgen/requirements.html](<https://rust-lang.github.io/rust-bindgen/requirements.html>)).



On Linux/macOS, install as follows:  
  
_export IDADIR=/path/to/ida # if not set, the build script checks common locations_

_cargo install rhabdomancer_

On Windows, instead use the following commands:  
  
_$env:LIBCLANG_PATH="\path\to\clang+llvm\bin"_

_$env:PATH="\path\to\ida;$env:PATH"_

_$env:IDADIR="\path\to\ida" # if not set, the build script checks common locations_

_cargo install rhabdomancer_

### **To use rhabdomancer**

  1. Ensure IDA Pro is properly configured with a valid license.
  2. Customize the list of known bad API functions in _conf/rhabdomancer.toml_ if needed.
  3. Run the tool: rhabdomancer _< binary_file>_. Any existing _.i64_ IDB file will be updated; otherwise, a new IDB file will be created.
  4. Open the resulting _.i64_ file in IDA Pro.
  5. Select **View > Open subviews > Bookmarks** (comments are also added at marked call locations).
  6. Enjoy your results conveniently collected into an IDA Pro window.



# **Now let's run rhabdomancer against a real-world binary file******

Our target of choice is the legendary **dtprintinfo SPARC binary** , which was featured in countless [advisories](<https://github.com/0xdea/raptor_infiltrate20>), [exploits](<https://github.com/0xdea/exploits>), [talks](<https://youtu.be/Nc9ZLTb2hQ8>), and [articles](<https://hnsecurity.it/blog/nothing-new-under-the-sun/>) (including [Phrack](<https://phrack.org/issues/70/13#article>)) by yours truly and other old-school hackers over the years… Here's to CDE! So long, and thanks for all the shells! 🥂 #️

  
To run rhabdomancer against our target binary, simply specify its path as the only argument: 

[![rhabdomancer01](https://hex-rays.com/hs-fs/hubfs/rhabdomancer01.png?width=2940&height=1838&name=rhabdomancer01.png)](<https://hex-rays.com/hs-fs/hubfs/rhabdomancer01.png?width=4410&height=2757&name=rhabdomancer01.png>)

Rhabdomancer is blazing fast! It took less than 3 seconds to fully analyze and process a 350 KB binary file:

[![rhabdomancer02](https://hex-rays.com/hs-fs/hubfs/rhabdomancer02.png?width=2940&height=1838&name=rhabdomancer02.png)](<https://hex-rays.com/hs-fs/hubfs/rhabdomancer02.png?width=4410&height=2757&name=rhabdomancer02.png>)

Bad API function call locations are bookmarked in the IDB. Enjoy your results conveniently collected into an IDA Pro window:

[![rhabdomancer03](https://hex-rays.com/hs-fs/hubfs/rhabdomancer03.png?width=2940&height=1836&name=rhabdomancer03.png)](<https://hex-rays.com/hs-fs/hubfs/rhabdomancer03.png?width=4410&height=2754&name=rhabdomancer03.png>)

Can you spot the vulnerability? Hint: this is a vulnerability class from another era that should've gone extinct a long time ago. Yet, here it is. Find out the answer in my [Phrack article](<https://phrack.org/issues/70/13#article>), or watch my [RomHack keynote](<https://youtu.be/Nc9ZLTb2hQ8>) if you're so inclined. 

# **Automating binary vulnerability discovery with haruspex and augur**

_Haruspex_ _  
_/hə-rŭs′pĕks″/  
A priest in ancient Rome who practiced divination by inspecting animal entrails.

_Augur_ _  
_/ô′gər/  
One who foretells events by omens.

[Haruspex](<https://github.com/0xdea/haruspex>) is the Rust port of [another](<https://github.com/0xdea/ghidra-scripts/blob/main/Haruspex.java>) Ghidra script of mine. It is an **IDA Pro headless plugin** that **extracts pseudocode** generated by IDA Pro's decompiler in a format that should be suitable for import into an IDE or to be parsed by static analysis tools such as [Semgrep](<https://github.com/0xdea/semgrep-rules>) and [weggli](<https://github.com/0xdea/weggli-patterns>).

Its main features include:

  * **Fast, headless user experience** courtesy of IDA Pro 9.x and Binarly's idalib Rust bindings.
  * Support for binary targets for **any architectures** supported by IDA Pro’s decompiler.
  * Pseudocode of each function is **stored in a separate file** in the output directory for easy inspection.
  * **External crates** can invoke _decompile_to_file_ to decompile a function and save its pseudocode to disk.  
  




Additional information on **haruspex's features** and usage is available at [crates.io](<https://crates.io/crates/haruspex>) and in the official [documentation](<https://0xdeadbeef.info/haruspex/haruspex/>).

**Installation** and **usage** are akin to what I have described earlier for rhabdomancer. The most notable difference is that haruspex **can also be used as a library** by third-party crates to decompile specific functions and save pseudocode to disk. An example of this is [augur](<https://github.com/0xdea/augur>), another IDA Pro headless plugin I wrote that **extracts strings and related pseudocode** from a binary file. I encourage you to [check it out](<https://crates.io/crates/haruspex>).

#   
**Example run for haruspex**

Coming back to haruspex, let's try it out against a sample binary. This time, our target is an **ARM aarch64 setuid binary** distributed with the latest **Zyxel appliances** , in which I recently discovered some [vulnerabilities](<https://hnsecurity.it/blog/local-privilege-escalation-on-zyxel-usg-flex-h-series-cve-2025-1731/>).  
  


Again, to run haruspex against our target binary, simply specify its path as the only argument:  


[![haruspex01](https://hex-rays.com/hs-fs/hubfs/haruspex01.png?width=2940&height=1838&name=haruspex01.png)](<https://hex-rays.com/hs-fs/hubfs/haruspex01.png?width=4410&height=2757&name=haruspex01.png>)

Haruspex is blazing fast! It took slightly more than 1 second to fully analyze and process a 43 KB binary file:

[![haruspex02](https://hex-rays.com/hs-fs/hubfs/haruspex02.png?width=2940&height=1838&name=haruspex02.png)](<https://hex-rays.com/hs-fs/hubfs/haruspex02.png?width=4410&height=2757&name=haruspex02.png>)

Enjoy decompiled pseudocode and [Semgrep](<https://github.com/0xdea/semgrep-rules>) scan results conveniently loaded in your favorite IDE, courtesy of the [SARIF Explorer extension](<https://marketplace.visualstudio.com/items?itemName=trailofbits.sarif-explorer>) for VS code.

[![haruspex03](https://hex-rays.com/hs-fs/hubfs/haruspex03.png?width=3440&height=1440&name=haruspex03.png)](<https://hex-rays.com/hs-fs/hubfs/haruspex03.png?width=5160&height=2160&name=haruspex03.png>)

Unsurprisingly, the vulnerability lies at the highlighted line marked with _Vuln_ 🙄. **The binary running with elevated privileges can be tricked into following a symbolic link** placed in _/tmp/register_status_ by a local low-privileged user. This can be exploited to **overwrite arbitrary files** or **escalate privileges** to root.

The full [advisory](<https://github.com/hnsecurity/vulns/blob/main/HNS-2025-10-zyxel-fermion.txt>) and proof-of-concept [exploit](<https://github.com/0xdea/exploits/blob/master/zyxel/raptor_fermion>) are available on GitHub. Here's a screenshot of the exploit in action for your viewing pleasure:  


[![haruspex04](https://hex-rays.com/hs-fs/hubfs/haruspex04.png?width=2940&height=1914&name=haruspex04.png)](<https://hex-rays.com/hs-fs/hubfs/haruspex04.png?width=4410&height=2871&name=haruspex04.png>)

For additional information on my**vulnerability research tools and workflow** , please refer to the following articles:

  * [https://hnsecurity.it/blog/semgrep-ruleset-for-c-c-vulnerability-research](<https://hnsecurity.it/blog/semgrep-ruleset-for-c-c-vulnerability-research>)
  * [https://hnsecurity.it/blog/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/](<https://hnsecurity.it/blog/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/>)
  * [https://hnsecurity.it/blog/big-update-to-my-semgrep-c-cpp-ruleset](<https://hnsecurity.it/blog/big-update-to-my-semgrep-c-cpp-ruleset>)
  * [https://hnsecurity.it/blog/a-collection-of-weggli-patterns-for-c-cpp-vulnerability-research](<https://hnsecurity.it/blog/a-collection-of-weggli-patterns-for-c-cpp-vulnerability-research>)



# **Conclusion**

The [award-winning](</blog/2024-plugin-contest-winner>) idalib Rust bindings open **endless possibilities**. Developers can leverage the entire Rust ecosystem to **combine IDA Pro with existing Rust libraries and tools** , such as [weggli](<https://github.com/0xdea/weggli-patterns>), or **use it as part of larger static/dynamic analysis pipelines** alongside, for example, [libafl](<https://crates.io/crates/libafl>).  
  
I hope this article has served as a useful introduction to idalib, and that you'll consider contributing to this powerful Rust library and **building your own tools** on top of it. Meanwhile, you can **download my updated vulnerability research tools** from [Hex-Rays community plugins](<https://plugins.hex-rays.com/author/0xdea>), [lib.rs](<https://lib.rs/~0xdea>), [crates.io](<https://crates.io/users/0xdea>), or [GitHub](<https://github.com/0xdea>).  
  
I would like to **thank idalib's maintainers** at Binarly and especially Sam L. Thomas ([@xorpse](<https://github.com/xorpse/>)), who made me feel welcome since my first pull request. You are awesome ✊  
  
Finally, huge thanks to Chris Hernandez for inviting me to write on the Hex-Rays blog. It's been an honor. Until next time!

---

## 9. 

**Date:** Posted: Sep 8, 2025  
**Link:** [https://hex-rays.com/blog/ida-9.2-release](https://hex-rays.com/blog/ida-9.2-release)

[Back](<https://hex-rays.com/blog>)

[News](<https://hex-rays.com/blog/tag/news>) [UI](<https://hex-rays.com/blog/tag/ui>) [debugger](<https://hex-rays.com/blog/tag/debugger>) [decompiler](<https://hex-rays.com/blog/tag/decompiler>) [ARM](<https://hex-rays.com/blog/tag/arm>) [IDA 9.2](<https://hex-rays.com/blog/tag/ida-9-2>) [RISC-V](<https://hex-rays.com/blog/tag/risc-v>) [Golang](<https://hex-rays.com/blog/tag/golang>) [Xref Graph](<https://hex-rays.com/blog/tag/xref-graph>) [Xref Tree](<https://hex-rays.com/blog/tag/xref-tree>) [TriCore](<https://hex-rays.com/blog/tag/tricore>) [Microcode Viewer](<https://hex-rays.com/blog/tag/microcode-viewer>)

# IDA 9.2 Release: Golang Improvements, New UI Widgets, Types Parsing and More

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9.2-release>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9.2-release>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9.2-release>)

Hex-Rays ✦ Posted: Sep 8, 2025

![IDA 9.2 Release: Golang Improvements, New UI Widgets, Types Parsing and More](https://hex-rays.com/hubfs/emailheader92.jpg)

Over the past few months, we’ve given you sneak peeks into some of the new capabilities coming to IDA 9.2. Now it’s time to bring them all together (with unannounced features as well) in the official launch.

This release features more useful output when decompiling Golang binaries, dozens of user interface and workflow refinements, a new type parser based on LLVM’s LibTooling, a refresh of the disassemblers for the v850/rh850 and TriCore microcontrollers, the usual additions of instruction set extensions, and countless quality-of-life improvements.

Below is a summary of the key features, and here is the link to the [9.2 release notes](<https://docs.hex-rays.com/release-notes/9_2>).

# **Smarter Analysis Across Architectures**

IDA 9.2 extends support for a wide range of processors and instruction sets, making embedded and hardware analysis more powerful.

  * Improved **switch detection** for RISC-V and ARM, as well as improved **stack pointer tracking** for ARM. Read the full blog [here](</blog/unlocking-risc-v-and-arm-next-level-switch-detection>).
  * Expanded **TriCore chipset coverage** (tc1x - tc4x), support for recently introduced tc4x (TC1.8) instructions, support for registers with global, user-specifiable values. Read the full blog [here](</blog/tricore-ida-support>).
  * More macro instructions for **v850/rh850** , better handling of relocatable objects for creating FLIRT signatures, support for registers with global, user-specifiable values.
  * Support 32-bit SIMD instructions of **TMS320 C6 Series.**



# **Clearer, More Accurate Decompilation**

Decompiler improvements help you work faster with Go, Windows binaries, and cross-references.

  * Tuple types for **Golang multi-value returns** , producing much cleaner output. Read the full blog [here](</blog/stop-guessing-and-start-going>).
  * New action: “Show all xref decompilations”, for code and data references inside the decompiler.



# **Insights Into the Decompiler Pipeline**

For the first time, IDA opens a window into the Decompiler’s internals. Read the full blog [here](<https://hex-rays.com/blog/spying-on-decompiler-internals-the-hex-rays-microcode-viewer>).

  * New **Microcode Viewer** reveals the intermediate representation (Hex-Rays Microcode).
  * Inspect transformations at every stage.
  * Build advanced **custom plugins** with deeper insight.



# **Debugger Enhancements**

The debugger now offers richer context and more reliable call stacks. Read the full blog [here](</blog/ida-debugger-new-register-subview>).

  * Redesigned **Register Subview** with automatic pointer dereferencing and **color coding** for memory types.
  * More accurate **call stack reconstruction** for x64 PE binaries.



# **Productivity & UI Improvements**

We’ve smoothed out day-to-day workflows with a range of quality-of-life updates:

  * **Jump Anywhere** is a new dialog created to simplify quick jumps to locations anywhere in the IDB. 
  * **Unified Location History** enables a history stack across multiple widgets. Remember how hitting ESC after jumping from the decompiler to some data item in the disassembler wouldn’t bring you back to the decompiler? That behavior is remedied. If you don’t like it, the new history sharing can disabled globally.
  * New **Dynamic** **Xref Graph** widget that gives a graphical representation of inter-function relationships (code and data). Replaces the static _qwingraph_ charts with an interactive, OpenGL accelerated version of the same functionality. Read the full blog [here](</blog/mapping-relationships-in-ida-9.2-dynamic-xref-graph-and-xref-tree>).
  * New **Xref Tree** widget that gives a textual representation of inter-function relationships (code and data). It unifies the now-legacy Function Calls and Cross References subviews.
  * The ones among you who regularly fire up IDAs during presentations will be delighted to hear that the common Ctrl-+/Ctrl-- **shortcuts** to increase/decrease font size now work in IDA.
  * The type editor under Local Types now supports **tab completion**.
  * Qt6 brings much more stable support for **Wayland Linux Desktops**.



# **Under the Hood**

  * As of this release, IDA is relying on **Qt6 for its UI**. For plugin authors with UI components this implies (a very limited amount of) porting work. In the meantime, we provided some shims that expose a limited amount of the now-legacy Qt5 API to Plugins using Qt6.
  * The **Type Parsers** inside and outside of IDA have been unified and now rely on LLVM’s LibTooling. The benefits of this simplification will start to show in upcoming releases, so stay tuned.

We also tackled a handful of bug fixes and threw in a few other improvements. [Read the release notes for all the details.](<https://docs.hex-rays.com/release-notes/9_2>)  


# **But wait, there's more...**

  * Check out the new, open-source, easy-to-use API available: IDA Domain-API

    * Read the article: [https://hex-rays.com/blog/introducing-the-ida-domain-api ](</blog/introducing-the-ida-domain-api>)
    * Clone the repo: [https://github.com/HexRaysSA/ida-domain](<https://github.com/HexRaysSA/ida-domain>)
  * And the C++ SDK and IDAPython SDK are now open-source and available!

    * Read the article: [https://hex-rays.com/blog/open-sourcing-ida-sdk](</blog/open-sourcing-ida-sdk>)
    * Clone the repo: <https://github.com/HexRaysSA/ida-sdk>



# **_____________________________________________________________________________**

# **Keep us in the loop!**

  * If you see something, say something. You can report bugs here: [https://support.hex-rays.com](<https://get.support.hex-rays.com/servicedesk/customer/portals>)
  * Have ideas on how we can improve IDA? We want to know! Send us a note with your feedback directly to product@hex-rays.com, or post it on [Discourse](<https://community.hex-rays.com/>).



# Getting the 9.2 update

If you already purchased an IDA 9 subscription, you will see 9.2 in the **Download Center** of My Hex-Rays [portal.](<https://my.hex-rays.com/login>)  
  
If you have a perpetual license in active support, and you have note requested a trial before, you can download a trial version of IDA 9.2.  
To start using IDA 9.2 today you can access the installer in our new customer portal, My Hex-Rays.   
There you can request a license key for your free subscription. Please note that the license key for your free IDA 9.2 subscription will expire at the end of your active support period. You can find detailed instructions on how to upgrade to the IDA 9 series [here.](<https://hex-rays.com/faqs/how-do-i-upgrade-to-ida-9>)   
  
**Support plan expired? No problem.** You can purchase an updated license online [here](<https://hex-rays.com/pricing>). You’ll see we’ve updated our product packages. If your previous plan doesn’t align with our new offerings, fear not, we’re here to help. We’ve been working with all our customers to ensure they get a plan that is the right fit (and price) for them. Just email us at **sales@hex-rays.com.**

****

****

****

****

****

****

**

******

****

****

****

---

