---
id: 222
title: Resharper Naming Style for Machine.Specifications
date: 2010-02-11T01:59:37+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=222
permalink: /2010/02/11/resharper-naming-style-for-machine-specifications/
dsq_thread_id:
  - "261600799"
categories:
  - Uncategorized
tags:
  - Machine.Specifications
  - Resharper
---
If you&#8217;re doing BDD-style specifications and using underscores within your variable names, the default Resharper settings will warn you about violating the naming style rules as shown below:

[<img class="size-full wp-image-223" title="ResharperUnconfigured" src="/wp-content/uploads/2010/02/ResharperUnconfigured.png" alt="" width="661" height="442" />](/wp-content/uploads/2010/02/ResharperUnconfigured.png)

Fortunately, <noindex></noindex> Machine.Specifications (MSpec) extends Resharper to allow for the creation of custom rules which affect only your MSpec types. To create a custom rule for MSpec, follow these steps:

**Step 1**: Open the Resharper Options Dialog.

[<img class="alignnone size-full wp-image-255" title="ResharperOptions" src="/wp-content/uploads/2010/02/ResharperOptions.png" alt="" width="229" height="453" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptions.png 229w, http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptions-151x300.png 151w" sizes="(max-width: 229px) 100vw, 229px" />](/wp-content/uploads/2010/02/ResharperOptions.png)

**Step 2**: Navigate to Languages | Naming Style and under &#8220;User defined naming rules&#8221; click &#8220;Add&#8221;.

[<img class="size-full wp-image-224" title="ResharperOptionsLanguagesNamingStyle_add" src="/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyle_add.png" alt="" width="553" height="533" />](/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyle_add.png)

**Step 3**: This displays the &#8220;Edit Extended Naming Rule dialog.  In the &#8220;Rule Definition:&#8221; text box, give your new rule a name such as &#8220;Machine.Specifications Rules&#8221;.

[<img class="alignnone size-full wp-image-261" title="ResharperOptionsLanguagesNamingStyleRuleDescription" src="/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescription.png" alt="" width="492" height="446" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescription.png 492w, http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescription-300x271.png 300w" sizes="(max-width: 492px) 100vw, 492px" />](/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescription.png)

**Step 4**: Within the Affected entities list, un-check Class and scroll to the bottom to locate the entries starting with Machine.Specifications. Select all of these entries.

[<img class="alignnone size-full wp-image-260" title="ResharperOptionsLanguagesNamingStyleRuleDescriptionModified" src="/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescriptionModified.png" alt="" width="492" height="443" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescriptionModified.png 492w, http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescriptionModified-300x270.png 300w" sizes="(max-width: 492px) 100vw, 492px" />](/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleDescriptionModified.png)

**Step 5**: Select all options under Access rights: and leave options under Static/non-static: selected.

[<img class="alignnone size-full wp-image-263" title="ResharperOptionsLanguagesNamingStyleAccessRights" src="/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleAccessRights.png" alt="" width="483" height="190" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleAccessRights.png 483w, http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleAccessRights-300x118.png 300w" sizes="(max-width: 483px) 100vw, 483px" />](/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleAccessRights.png)

**Step 6**: Add a new naming style to associate this rule to by clicking the Add icon and selecting the desired naming style (e.g. all_lower).

[<img class="alignnone size-full wp-image-264" title="ResharperOptionsLanguagesNamingStyleRuleStyle" src="/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleStyle.png" alt="" width="224" height="622" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleStyle.png 224w, http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleStyle-108x300.png 108w" sizes="(max-width: 224px) 100vw, 224px" />](/wp-content/uploads/2010/02/ResharperOptionsLanguagesNamingStyleRuleStyle.png)

The following diagram shows the dialog after these steps:

[<img class="size-full wp-image-249" title="ResharperEditExtendedNamingRuleDialog" src="/wp-content/uploads/2010/02/ResharperEditExtendedNamingRuleDialog.png" alt="" width="578" height="531" />](/wp-content/uploads/2010/02/ResharperEditExtendedNamingRuleDialog.png)

<p style="padding: 10px;">
  After selecting OK, Resharper now ignores the naming warnings for the MSpec types:
</p>

<div>
  <a href="/wp-content/uploads/2010/02/ResharperConfigured.png"><img class="size-full wp-image-266" title="ResharperConfigured" src="/wp-content/uploads/2010/02/ResharperConfigured.png" alt="" width="741" height="502" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperConfigured.png 926w, http://aspiringcraftsman.com/wp-content/uploads/2010/02/ResharperConfigured-300x203.png 300w" sizes="(max-width: 741px) 100vw, 741px" /></a>
</div>