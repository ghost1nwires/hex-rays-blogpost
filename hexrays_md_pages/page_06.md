# Hex-Rays Blog – Page 6

**Archived:** 2026-02-20 12:11
**URL:** https://hex-rays.com/blog/page/6

---

## 1. 

**Date:** Posted: Sep 5, 2024  
**Link:** [https://hex-rays.com/blog/unveiling-ida-pro-9-0-c-exceptions-support-in-the-decompiler](https://hex-rays.com/blog/unveiling-ida-pro-9-0-c-exceptions-support-in-the-decompiler)

[Back](<https://hex-rays.com/blog>)

[IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [c++](<https://hex-rays.com/blog/tag/c>) [IDA 9.0](<https://hex-rays.com/blog/tag/ida-9-0>)

# Unveiling IDA Pro 9.0: C++ Exceptions Support in the Decompiler

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/unveiling-ida-pro-9-0-c-exceptions-support-in-the-decompiler>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/unveiling-ida-pro-9-0-c-exceptions-support-in-the-decompiler>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/unveiling-ida-pro-9-0-c-exceptions-support-in-the-decompiler>)

A 

Alex Petrov ✦ Posted: Sep 5, 2024

![Unveiling IDA Pro 9.0: C++ Exceptions Support in the Decompiler](https://hex-rays.com/hubfs/9.0-Hero.png)

One of the more challenging parts of reverse engineering programs written in C++ is the accurate extraction of exception information. Due to the complexity of the language’s features and runtime behavior, recovering the missing information currently requires a lot of manual work and considerable effort.

However, **with the release of IDA Pro 9.0, a significant advancement has been introduced to tackle this issue** : the decompiler now supports the emission of try/catch blocks, starting with the C++ exception scheme for x64 binaries compiled with the Microsoft Visual C++ compiler.

IDA Pro 9.0 provides more accurate decompilation by presenting **exception-handling structures** as they appear in the source code. This helps reverse engineers better understand how the program handles exceptions and error states, reducing the manual reconstruction time of such code paths.

The introduction of high-level try/catch blocks greatly enhances the readability of decompiled code. Instead of creating separate functions for exception handlers and thus splitting off important information from the decompiled code, **users can now see the intended error-handling code in a familiar C++ form** , making the code easier to comprehend and analyze. The image below shows the difference between decompiling a function with and without the support of try/catch blocks. It nicely illustrates how the error-handling code hidden earlier is now part of try/catch constructs.

![ida_9-try-catch-support](https://hex-rays.com/hs-fs/hubfs/ida_9-try-catch-support.png?width=1280&height=776&name=ida_9-try-catch-support.png)

This can be especially helpful in more complex scenarios, where the creator of some binary relied on exception handling to obfuscate code to make analysis tedious and difficult. **The ability to decode and visualize such code structures provides a powerful tool for malware analysts trying to unravel complex and obfuscated control flows**. 

Whether performing vulnerability research, malware analysis, or software auditing, the option to accurately reflect exception-handling mechanisms in decompiled output will undoubtedly save time and effort while offering a more complete understanding of the code at hand. 

This enhancement, **introduced with the September 30th release of IDA Pro 9.0** , sets the stage for even more sophisticated decompilation features in the future as IDA Pro continues to evolve alongside the complexity of modern software development.

---

## 2. 

**Date:** Posted: Sep 2, 2024  
**Link:** [https://hex-rays.com/blog/ida-9-0-sdk-idapython-porting-guides](https://hex-rays.com/blog/ida-9-0-sdk-idapython-porting-guides)

[Back](<https://hex-rays.com/blog>)

[Programming](<https://hex-rays.com/blog/tag/programming>) [documentation](<https://hex-rays.com/blog/tag/documentation>)

# IDA 9.0: SDK & IDAPython porting guides

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-9-0-sdk-idapython-porting-guides>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-9-0-sdk-idapython-porting-guides>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-9-0-sdk-idapython-porting-guides>)

Geoffrey Czokow ✦ Posted: Sep 2, 2024

![IDA 9.0: SDK & IDAPython porting guides](https://hex-rays.com/hubfs/9.0-Hero.png)

We are excited to announce the upcoming release of IDA version 9.0! This new version introduces major changes to the** C++ SDK** and **IDAPython API** , and we want to ensure you are prepared for the transition. To support you in updating your plugins and scripts, we have released new documentation that includes a **comprehensive Porting Guide**. This guide provides detailed instructions and best practices for migrating your work from IDA 8.x to IDA 9.0 API. Additionally, the documentation features complete SDK and API references, giving you all the tools you need to ensure your plugins are fully compatible with IDA 9.0.

If you currently have an active IDA license, we offer access to the IDA 9.0 beta program. Simply send an email to [beta@hex-rays.com](<mailto:beta@hex-rays.com>) with your license details. This is an excellent opportunity to test your updated plugins in the new environment before the official release.

You can access the new documentation environment here: [docs.hex-rays.com/pre-release](<http://docs.hex-rays.com/pre-release>)

Thank you for your continued support and contributions to the IDA community! We look forward to your feedback and hope to see your plugins fully compatible with IDA 9.0 soon.

__

---

## 3. 

**Date:** Posted: Aug 7, 2024  
**Link:** [https://hex-rays.com/blog/madame-de-maintenons-enigmatic-bouillotte-game](https://hex-rays.com/blog/madame-de-maintenons-enigmatic-bouillotte-game)

[Back](<https://hex-rays.com/blog>)

[CTF](<https://hex-rays.com/blog/tag/ctf>) [CTF challenge](<https://hex-rays.com/blog/tag/ctf-challenge>)

# Madame de Maintenon’s Enigmatic Bouillotte Game

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/madame-de-maintenons-enigmatic-bouillotte-game>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/madame-de-maintenons-enigmatic-bouillotte-game>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/madame-de-maintenons-enigmatic-bouillotte-game>)

A 

Alex Petrov ✦ Posted: Aug 7, 2024

![Madame de Maintenon’s Enigmatic Bouillotte Game](https://hex-rays.com/hubfs/MDM-Enigmatic-Bouillotte-Game.png)

![MDM-Enigmatic-Bouillotte-Game](https://hex-rays.com/hs-fs/hubfs/MDM-Enigmatic-Bouillotte-Game.png?width=1920&height=1080&name=MDM-Enigmatic-Bouillotte-Game.png)

Step into the lavish world of 17th-century France in our latest CTF challenge, “Madame de Maintenon’s Enigmatic Bouillotte Game”. As Madame de Maintenon joins a high-stakes game of Bouillotte, you must navigate a web of intrigue, solve cryptographic puzzels, and avoid deadly traps. Will you uncover the hidden flag and outsmart your opponents in this thrilling historical adventure? Join the game and find out!

## **How to participate**

  1. Fill in the registration form: <https://share-eu1.hsforms.com/1ZIit-n46SeazXCubLyQAVA2dgu4h>
  2. Download the file provided in the registration form above.
  3. It is not mandatory, but we strongly suggest using IDA Pro, Home or Free to solve the challenge. Not an IDA user? [Download IDA Free](<https://hex-rays.com/ida-free/#download>).
  4. Find the flag.
  5. Send the correct flag to [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)



## Prizes

**The quickest three of you to send the flag to the email above, will win a silver coin!**

## Rules of the challenge

  * The challenge is free and open to everyone attending Black Hat USA 2024;
  * Participants must have a valid email address.
  * Participants could use any tools to solve the challenge but we encourage using IDA;
  * Any form of cheating will not be tolerated;
  * We may use your personal informationm provided in the registration form, for the following purposes: 
    * We will use your contact information to send you announcements, news, articles, product information, and promotions;
    * We may use third-party email marketing software, such as Hubspot, to facilitate our communication. Your information may be stored in and processed by such services for email marketing purposes.
  * We will not share your personal information with third parties, other companies, or individuals, except as explicitly mentioned in these Rules;
  * The organizers reserve the right to cancel, modify, or suspend the Challenge for any reason, including but not limited to unforeseen circumstances;
  * Only one submission per player will be accepted;
  * Submissions made after the deadline will not be considered;
  * The winners will be contacted and will have to come and collect their prize at the Hex-Rays booth.
  * We may share the names or nicknames of the winners on our website and/or our social networks, except in the cases where the participants have explicitly mentioned that they don’t want their names to be mentioned in our publicity;
  * By participating in the “Madame de Maintenon’s Enigmatic Bouillotte Game,” you agree to these rules. Hex-Rays reserves the right to update or modify these terms as necessary.
  * For any inquiries or issues related to the Challenge, please contact [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)



## Important Dates:

  1. CTF start date: 7 August 2024
  2. Submission Deadline: 8 August 2024, 15:00 (Las Vegas Time Zone)
  3. Winners will be announced below and on our X and LinkedIn pages.



**Best of luck as you step into this thrilling adventure!**

---

## 4. 

**Date:** Posted: Aug 6, 2024  
**Link:** [https://hex-rays.com/blog/idas-maze-chase](https://hex-rays.com/blog/idas-maze-chase)

[Back](<https://hex-rays.com/blog>)

[CTF](<https://hex-rays.com/blog/tag/ctf>) [CTF challenge](<https://hex-rays.com/blog/tag/ctf-challenge>)

# IDA’s Maze Chase

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/idas-maze-chase>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/idas-maze-chase>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/idas-maze-chase>)

A 

Alex Petrov ✦ Posted: Aug 6, 2024

![IDA’s Maze Chase](https://hex-rays.com/hubfs/IDA-Maze-Chase-Main-Image-1.png)

In the ancient palace,** a daring thief steals from Madame de Maintenon** and vanishes into the catacombs. With the help of a guard and an old sage, **she learns of legendary items that could aid her pursuit**. She must havigate the treachous maze, overcoming obstacles and solving puzzles to retrieve her stolen treasure. Her journey through the catacombs is fraught with challenges, but her determination to reclaim what is hers drives her onward.

## **How to participate**

  1. Fill in the registration form: <https://share-eu1.hsforms.com/1Jqe0MPcjS86kEbPZtUwGTg2dgu4h>
  2. Download the file provided in the registration form above.
  3. It is not mandatory, but we strongly suggest using IDA Pro, Home or Free to solve the challenge. Not an IDA user? [Download IDA Free](<https://hex-rays.com/ida-free/#download>).
  4. Reverse-engineer the binary and get valuable instructions on how to solve the challenge.
  5. You will need to **come to our booth #3003 and play a game** on our GAMEPI20 device.
  6. Send the correct flag to [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)



## Prizes

**The quickest three of you to send the flag to the email above, will win a silver coin – a part of the retrieved treasure!**

## Rules of the challenge

  * The challenge is free and open to everyone attending Black Hat USA 2024;
  * Participants must have a valid email address;
  * Participants could use any tools to solve the challenge but we encourage using IDA;
  * Any form of cheating will not be tolerated;
  * We may use your personal informationm provided in the registration form, for the following purposes: 
    * We will use your contact information to send you announcements, news, articles, product information, and promotions;
    * We may use third-party email marketing software, such as Hubspot, to facilitate our communication. Your information may be stored in and processed by such services for email marketing purposes.
  * We will not share your personal information with third parties, other companies, or individuals, except as explicitly mentioned in these Rules;
  * The organizers reserve the right to cancel, modify, or suspend the Challenge for any reason, including but not limited to unforeseen circumstances;
  * Only one submission per player will be accepted;
  * Submissions made after the deadline will not be considered;
  * The winners will be contacted and will have to come and collect their prize at the Hex-Rays booth.
  * We may share the names or nicknames of the winners on our website and/or our social networks, except in the cases where the participants have explicitly mentioned that they don’t want their names to be mentioned in our publicity;
  * By participating in the “IDA’s Maze Chase,” you agree to these rules. Hex-Rays reserves the right to update or modify these terms as necessary.
  * For any inquiries or issues related to the Challenge, please contact [marketing@hex-rays.com](<mailto:marketing@hex-rays.com>)



## Important Dates:

  1. CTF start date: 7 August 2024
  2. Submission Deadline: 8 August 2024, 15:00 (Las Vegas Time Zone)
  3. Winners will be announced below and on our X and LinkedIn pages.



**Best of luck as you step into this thrilling adventure!**

__

---

## 5. 

**Date:** Posted: Jun 7, 2024  
**Link:** [https://hex-rays.com/blog/exclusive-ida-pro-online-starter-training-spring-deal](https://hex-rays.com/blog/exclusive-ida-pro-online-starter-training-spring-deal)

[Back](<https://hex-rays.com/blog>)

[Training](<https://hex-rays.com/blog/tag/training>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>) [IDA Pro Training](<https://hex-rays.com/blog/tag/ida-pro-training>) [Starter Training](<https://hex-rays.com/blog/tag/starter-training>)

# Exclusive IDA Pro Online Starter Training Spring Deal!

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/exclusive-ida-pro-online-starter-training-spring-deal>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/exclusive-ida-pro-online-starter-training-spring-deal>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/exclusive-ida-pro-online-starter-training-spring-deal>)

A 

Alex Petrov ✦ Posted: Jun 7, 2024

![Exclusive IDA Pro Online Starter Training Spring Deal!](https://hex-rays.com/hubfs/2200x1368.jpg)

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/starter-training-spring-deal-1024x576.png?width=1024&height=576&name=starter-training-spring-deal-1024x576.png)

Are you ready to begin your IDA journey? Our IDA Pro online Starter Training is designed to equip you with the essential skills needed to use IDA confidently and start analyzing simple binaries. We now offer this training at a reduced price!

For a limited time, **we have launched our Spring Deal**. Register before June 30th, 2024, and seize the opportunity to attend our **introductory course at a discounted rate of just 700 EUR/USD** instead of the regular price of 999 EUR/USD. Don’t forget to mention the **#IDA700** promo code in your registration form! 

[Register Now](<https://forms.gle/YaeMay5uoCYPwxXU7>)

### Terms and conditions

  1. **Validity Period:** This exclusive offer is valid for registrations made before June 30th, 2024.
  2. **Private Users Only:** Please note that this deal is applicable to private users only. Corporate or organizational registrations are not eligible for this special deal.
  3. **Provisional License:** For registrants who do not have an IDA Pro license, we will provide a temporary license during the training period.
  4. **Cancellation and Refunds:** Cancellations are accepted and fully refunded if requested at least one week prior to the start date of the chosen training sessions.
  5. **Course Access:** Hex-Rays will provide access to the training page and a videoconference link, once the payment has been cleared.
  6. **Limited Seats:** Please note that the seats are limited and will be allocated on a first-come, first-served basis.



Secure your seat today and take advantage of this special Spring Deal before spaces run out!

[Register Now](<https://forms.gle/YaeMay5uoCYPwxXU7>)

---

## 6. 

**Date:** Posted: May 27, 2024  
**Link:** [https://hex-rays.com/blog/ida-8-4-service-pack-2-released](https://hex-rays.com/blog/ida-8-4-service-pack-2-released)

[Back](<https://hex-rays.com/blog>)

[IDA](<https://hex-rays.com/blog/tag/ida>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.4 Service Pack 2 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-4-service-pack-2-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-4-service-pack-2-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-4-service-pack-2-released>)

A 

Alex Petrov ✦ Posted: May 27, 2024

![IDA 8.4 Service Pack 2 released](https://hex-rays.com/hubfs/2200x1368.jpg)

We are pleased to announce that IDA 8.4 Service Pack 2 (SP2) is now available for download! This latest release includes mostly bug fixes. 

### How to request the new versions

All new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively, you can [submit your ida.key](<https://www.hex-rays.com/updida/>), and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially shortly after the release. Please be patient.

If you have not received anything within an hour or so, send us a message to [support@hex-rays.com](<mailto:support@hex-rays.com>).

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist](<https://hex-rays.com/products/ida/news/8_4sp2/>)

---

## 7. 

**Date:** Posted: Apr 10, 2024  
**Link:** [https://hex-rays.com/blog/an-overview-of-the-makesig-plugin](https://hex-rays.com/blog/an-overview-of-the-makesig-plugin)

[Back](<https://hex-rays.com/blog>)

[IDA 8.0](<https://hex-rays.com/blog/tag/ida-8-0>) [plugin](<https://hex-rays.com/blog/tag/plugin>) [IDA](<https://hex-rays.com/blog/tag/ida>) [firmware](<https://hex-rays.com/blog/tag/firmware>) [IDA Pro](<https://hex-rays.com/blog/tag/ida-pro>) [patfind](<https://hex-rays.com/blog/tag/patfind>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# An overview of the makesig plugin

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/an-overview-of-the-makesig-plugin>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/an-overview-of-the-makesig-plugin>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/an-overview-of-the-makesig-plugin>)

A 

Alex Petrov ✦ Posted: Apr 10, 2024

![An overview of the makesig plugin](https://hex-rays.com/hubfs/GKzrsGnXYAAm2V-.jpg)

## makesig plugin overview 

The makesig plugin was introduced in the IDA 8.4 release, and it is a convenient tool for generating FLIRT signatures from a current database. As you probably already know, FLIRT stands for Fast LibrarybIdentification and Recognition Technology, allowing IDA to recognize standard library functions generated by supported compilers. This technology improves the disassembly listing by making it more readable and usable. It is important to mention that it isn’t possible for IDA to cover all existing libraries, compilers, and linkers. For that reason, users can create their own signatures from known code. 

Until IDA 8.4, making signatures from working database was possible but not straightforward: 

  * Exporting patterns from the database to a `.pat` file;
  * Compiling a `.pat` into a signature file (`.sig`);
  * Re-importing the `.sig `file into the target database.



![](https://hex-rays.com/hubfs/Imported_Blog_Media/makesig-1-4.png) Creation of a signature before IDA 8.4 

Since the new release, this process has been significantly improved thanks to the built-in makesig plugin. You just need to:   


  * Export the patterns from the database into a `.sig` file;
  * Re-import the `.sig` into the target database.



Let’s see how that would work in a real scenario. Imagine working on a long-term reversing project with frequent new versions. With the makesig plugin, we can migrate the carefully curated list of functions that we already reverse-engineered and exported as a signature file, into the current binary (given that compiler flags didn’t change too much between releases).   
Let’s say we identified an interesting function In the older release (source) binary and wanted to port that information to the newer binary: 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/makesig-2-4.png?width=1417&height=898&name=makesig-2-4.png)

We can export a signature file for this function via the new menu item File -> Produce File -> Create SIG file 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/makesig-3-4.png?width=691&height=273&name=makesig-3-4.png)

Then, in the new binary file, we can import this signature file in the Signatures window: 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/makesig-4-4.png?width=361&height=171&name=makesig-4-4.png)

As we can see, IDA applies the signature and reports that it found a match in the new database! And indeed, we can find the function, labeled as a library function, because its function name came from the signature file: 

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/makesig-5-4.png?width=1266&height=897&name=makesig-5-4.png)

---

## 8. 

**Date:** Posted: Mar 27, 2024  
**Link:** [https://hex-rays.com/blog/igors-tip-of-the-week-179-bitmask-enums](https://hex-rays.com/blog/igors-tip-of-the-week-179-bitmask-enums)

[Back](<https://hex-rays.com/blog>)

[idapro](<https://hex-rays.com/blog/tag/idapro>) [shortcuts](<https://hex-rays.com/blog/tag/shortcuts>) [idatips](<https://hex-rays.com/blog/tag/idatips>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Igor’s Tip of the Week #179: Bitmask enums

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/igors-tip-of-the-week-179-bitmask-enums>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/igors-tip-of-the-week-179-bitmask-enums>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/igors-tip-of-the-week-179-bitmask-enums>)

I 

Igor Skochinsky ✦ Posted: Mar 27, 2024

![Igor’s Tip of the Week #179: Bitmask enums](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

We’ve covered simple enums [previously](<https://hex-rays.com/blog/igors-tip-of-the-week-99-enums/>), but there is a different kind of enum that you may sometimes encounter or need to create manually. They are used to represent various bits (or _flags_) which may be set in an integer value. For example, the file mode on Unix filesystems contains Access Permission bits (you can see them in the output of ls as string like `-rwxr-xr-x`), and each bit has a [corresponding constant](<https://www.gnu.org/software/libc/manual/html_node/Permission-Bits.html>):
    
    
    #define S_IRWXU 00700
    #define S_IRUSR 00400
    #define S_IWUSR 00200
    #define S_IXUSR 00100
    
    #define S_IRWXG 00070
    #define S_IRGRP 00040
    #define S_IWGRP 00020
    #define S_IXGRP 00010
    
    #define S_IRWXO 00007
    #define S_IROTH 00004
    #define S_IWOTH 00002
    #define S_IXOTH 00001
    

Whenever you have a value which can be represented as a combination of bit values, you can use _bitmask enums_ (used to be called _bitfields_ before 8.4 but renamed to reduce confusion with bitfields in structures).

To create a bitmask enum, check “Bitmask” on the Enum tab in the “Add type” dialog:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bitmask1-4.png?width=351&height=401&name=bitmask1-4.png)

The new enum gets the IDA-specific `__bitmask` attribute:
    
    
    FFFFFFFF enum __bitmask __oct FILE_PERMS // 4 bytes
    FFFFFFFF {
    FFFFFFFF };
    

And when you add new members (`N` shortcut), IDA automatically offers the next free bit as the value:

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bitmask2-4.png?width=519&height=407&name=bitmask2-4.png)

You can also use the C syntax tab editor but you’ll have to ensure that there are no overlapping bits yourself.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bitmask3-4.png?width=376&height=359&name=bitmask3-4.png)

To apply the enum to an integer value in disassembly or pseudocode, you may need to explicitly invoke Edit > Operand type > Enum member… action (or use M shortcut) as bitmask enums do not always appear in the context menu.

![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/bitmask4-4.png?width=669&height=186&name=bitmask4-4.png)

IDA will then display the value as a combination of enum’s members:
    
    
    .data:00001000 dd S_IWGRP or S_IXGRP or S_IXUSR

See also:

[Igor’s tip of the week #99: Enums](<https://hex-rays.com/blog/igors-tip-of-the-week-99-enums/>)

[IDA Help: Set function/item type](<https://hex-rays.com//products/ida/support/idadoc/1361.shtml>)

[Hex-Rays interactive operation: Set Number Representation](<https://hex-rays.com/products/decompiler/manual/cmd_numform.shtml>)

---

## 9. 

**Date:** Posted: Mar 20, 2024  
**Link:** [https://hex-rays.com/blog/ida-8-4-service-pack-1-released](https://hex-rays.com/blog/ida-8-4-service-pack-1-released)

[Back](<https://hex-rays.com/blog>)

[IDA](<https://hex-rays.com/blog/tag/ida>) [Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# IDA 8.4 Service Pack 1 released

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/ida-8-4-service-pack-1-released>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/ida-8-4-service-pack-1-released>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/ida-8-4-service-pack-1-released>)

A 

Alex Petrov ✦ Posted: Mar 20, 2024

![IDA 8.4 Service Pack 1 released](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

IDA 8.4 Service Pack 1 (SP1) is now live and ready to download. This release includes mainly bug fixes and refinements.

### How to request the new versions

All new versions are free for users with an active support plan. Please use the “Help > Check for free update” menu item in IDA. It is also possible to configure automatic checks of new versions. Alternatively, you can [submit your ida.key](<https://www.hex-rays.com/updida/>), and our servers will prepare new download links for all your licenses. Your request might take some time to be processed, especially shortly after the release. Please be patient.

If you have not received anything within an hour or so, send us a message to [support@hex-rays.com](<mailto:support@hex-rays.com>).

**Has your support plan expired?**

If your key is too old for a free update, you might still be eligible for a discounted upgrade. Support plans can be [renewed online](<https://www.hex-rays.com/cgi-bin/quote.cgi/renewals>). Please upload your ida.key file to get the cart automatically filled with the correct product IDs.

[Release highlights and complete changelist](<https://hex-rays.com/products/ida/news/8_4sp1/>)

---

