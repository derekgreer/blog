---
id: 24
title: Localizing values within a DropDownList
date: 2005-10-04T16:13:00+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/2005/10/localizing-values-within-a-dropdownlist/
permalink: /2005/10/04/localizing-values-within-dropdownlist/
dsq_thread_id:
  - "628540193"
categories:
  - Uncategorized
---
For today&#8217;s post I&#8217;ve decided to discuss localizing values within an HTML dropdown list. Populating a list with localized values can be relatively easy since running the “Generate Local Resource” from the VS2005 menu will set up implicit localization for you. The shortcoming of this method is that it doesn’t allow you to vary the items in the list based upon the locale. In some cases there may be a business need to not only localize the values within the list, but also to vary the list depending on the current locale/culture. Using implicit and explicit localization doesn&#8217;t handle this scenario since this form of localization depends on the keys being staticly defined. Of course you can always look to database solutions, but this will typically be too much overhead for localization concerns and doesn&#8217;t afford the flexibility resource files allow. To load varying lists into a dropdown based upon the current locale, you can use the ResourceManager class within the System.Resources namespace. ResourceManager allows you to load values from satellite assemblies or from a resources file. For demonstration purposes I will be discussing the latter.

To begin, create an Assembly resource file such as Country.resx within Visual Studio and added some name value pairs. Once this file is created, use the resgen.exe Visual Studio command line tool to compile this into a resource file named Country.resources. You may then call ResourceManager&#8217;s static CreateFileBasedResourceManager() method from within the code behind of your ASP.Net page or user control to load up the resources contained within Country.resources. Once you have a reference to the newly created ResourceManager, you may then bind your DropDownList control to a ResourceSet obtained by calling GetResourceSet() on your ResourceManager instance. Alternately you could iterate over the resources within the obtained ResourceSet. The following snippet demonstrates this technique:

<pre class="brush:csharp">protected &lt;noindex>&lt;/noindex> override void OnPreRender(EventArgs e)
{
ResourceManager rm = ResourceManager.CreateFileBasedResourceManager("Country",
Server.MapPath("~/Resources") + Path.DirectorySeparatorChar, null);

ResourceSet rs = rm.GetResourceSet(CultureInfo.CurrentCulture, false, true);

CountryList.DataSource = rs;
CountryList.DataTextField = "Value";
CountryList.DataValueField = "Key";
CountryList.DataBind();
}
</pre>

Ultimately you may wish to use a similar method for obtaining resources from a satellite assembly to enable XCOPY deployment of your resources. Happy localizing!