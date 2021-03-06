---
layout: post
title: Extending and customizing Google Code project hosting
description: Extending and customizing Google Code project hosting
commentIssueId: 6
---

<a href="http://code.google.com/hosting/" target="_blank"><img class="aligncenter" style="margin-top:25px;margin-bottom:15px;" title="Google Code" src="http://www.gstatic.com/codesite/ph/images/code_small.png" alt="" width="161" height="40" /></a>

<p>I've used <a href="http://code.google.com/hosting/" target="_blank"><strong>Google Code project hosting</strong></a> for almost every project I've started in the last few years. It's really great and I love both the Web UI and all the functionality-related features (which Google is <a href="http://code.google.com/p/support/wiki/WhatsNew" target="_blank">continuously extending</a>). However, one aspect in which I've found Google Code project hosting<strong> strangely lacking </strong><strong>is customizability and extendability</strong> in adding rich content and even external applications to a project site. Wouldn't it be great if we could add a special "Testing" and "Group discussion" tab or an easy way to embed presentations and JavaScript code into wiki pages? This would boost the usability of project sites and make the visitor experience so much better.</p>
So, I've been thinking about this idea and playing with <strong>hacks and workarounds for extending Google code sites</strong> <a href="http://code.google.com/p/pmrpc/" target="_blank">for</a> <a href="http://code.google.com/p/urlecho/" target="_blank">my</a><a href="http://code.google.com/p/feed-buster/" target="_blank"> projects</a>, and what I've discovered is that I'm not alone - <a href="http://code.google.com/p/pubsubhubbub/" target="_blank">several</a> <a href="http://code.google.com/p/google-caja/" target="_blank">other</a> <a href="http://code.google.com/p/closure-library/" target="_blank">projects</a> have been doing this also. So in this post I'll try to cover two approaches to extending and customizing Google Code project hosting: embedding 3rd party content using <strong>Google Gadgets</strong> and viewing non-source files in the repository using the <strong>SVN mime-types property</strong>.

The first idea for extending project sites is to <strong>embed 3-rd party content into wiki pages</strong> (Google Docs presentations, JavaScript programs and such). Although it is possible to use a <a href="http://code.google.com/p/support/wiki/WikiSyntax#HTML_support" target="_blank">large subset of HTML</a> in project wiki pages, &lt;iframe&gt; and &lt;script&gt; tags are not available. However, embedding Google Gadgets <em>is </em>supported. <a href="http://code.google.com/apis/gadgets/" target="_blank"><strong>Google Gadgets</strong></a> are " simple HTML and JavaScript applications that can be embedded in webpages and other apps" (think clock, calendar, twitter and similar widgets) and are, in essence, an HTML &lt;iframe&gt; embedded in the parent page. Although there is a <a href="http://www.google.com/ig/directory?synd=open" target="_blank">public directory of existing Gadgets</a> which you could add to your wiki pages, what's more interesting is that you can build a Gadget yourself and that it's really easy to do. So, finally, the first idea for extending Google Code project hosting is to<strong> build your own Google Gadgets which wrap the content you want to embed in your wiki page</strong>. Here's what you need to do:
<ol style="padding-left:30px;">
	<li>Create a file containing a blank, content-less XML Gadget specification -- you can just copy/paste the code from the <a href="http://code.google.com/apis/gadgets/docs/gs.html#Hello_World" target="_blank">Google Gadgets tutorial</a>.</li>
	<li>Define the content of the Gadget. Depending on what you want to embed in your wiki page, there are two ways to go here:
<ol>
	<li>If you want to embed an existing web page or rich content available on the Web, the content of the Gadget will be a single &lt;iframe&gt; element with the src attribute pointing to the URL of that web-page.</li>
	<li>If you want to embed content which does not exist on the web, you will have to write the HTML + JavaScript code that defines that content yourself and inline it in the Gadget. Be sure to check out the <a href="http://code.google.com/apis/gadgets/docs/dev_guide.html" target="_blank">Google Gadgets developer guide</a>.</li>
</ol>
</li>
	<li>Save and upload the Gadget XML specification file into your project repository.</li>
	<li>Edit one of your wiki pages and create a Google Gadget element that references the URL of the uploaded specification. <strong>Note</strong> that you must reference the raw-file URL of the XML spec which you can get by viewing the uploaded file in your project's Source tab and then clicking "View raw file" under File info.</li>
</ol>

<table>
<tr>
<td>
<p><a href="http://code.google.com/p/pubsubhubbub"><img class="aligncenter" title="Example 1" src="/images/blog_pres1.png" alt="Example 1" width="90%" /></a></p>
</td>
<td>
<p><a href="http://code.google.com/p/feed-buster"><img class="aligncenter" title="Example 2" src="/images/blog_ff1.png" alt="Example 2" width="90%"  /></a></p>
</td>
</tr>
<tr>
<td>
<center> 1. PubSubHubBub </center>
</td>
<td>
<center> 2. Feed-buster </center>
</td>
</tr>
<tr>
<td>
<p><a href="http://code.google.com/p/pmrpc/wiki/PmrpcTesting"><img class="aligncenter"  title="Example 3" src="/images/blog_qunit1.png" alt="Example 3" width="90%"  /></a></p>
</td>
<td>
<p><a href="http://code.google.com/p/urlecho"><img class="aligncenter" title="Example 4" src="/images/blog_form1.png" alt="Example 4" width="90%" /></a></p>
</td>
</tr>
<tr>
<td>
<center> 3. Pmrpc testing </center>
</td>
<td>
<center> 4. UrlEcho </center>
</td>
</tr>
</table>

And here are some examples that show the usefulness of this approach (images above, in order of appearance):
<ol style="padding-left:30px;">
	<li><strong>Embedding Google Docs presentations</strong> that visually explain details of your project (<a href="http://code.google.com/p/pubsubhubbub/" target="_blank">example project wiki page</a> and <a href="http://code.google.com/p/pubsubhubbub/source/browse/trunk/presentation_gadget.xml" target="_blank">XML spec</a>). To get a Google Docs URL which you can use for embedding, go the presentation's page in Google Docs, click "Share - Publish/Embed", and copy the URL under "Your document is publicly viewable at:".</li>
	<li><strong>Embedding a FriendFeed discussion groups</strong> for in-place discussions (<a href="http://code.google.com/p/feed-buster/#Feedback" target="_blank">example project wiki page</a> and <a href="http://hosting.gmodules.com/ig/gadgets/file/105320743644204138718/feedbuster.xml" target="_blank">XML spec</a> -- if you see a blank page, view the source of the page). To get a FriendFeed group URL which you can use for embedding, use the <a href="http://friendfeed.com/embed/realtime" target="_blank">official FriendFeed instructions</a> (basically, it's http://friendfeed.com/GROUP_NAME_HERE/embed e.g. <a href="http://friendfeed.com/feed-buster/embed">http://friendfeed.com/feed-buster/embed</a>).</li>
	<li><strong>Embedding QUnit testing pages</strong> for following project stability and cross-browser testing (<a href="http://code.google.com/p/pmrpc/wiki/PmrpcTesting" target="_blank">example project wiki page</a> and <a href="http://code.google.com/p/pmrpc/source/browse/trunk/testing/testingGadget.xml" target="_blank">XML spec</a>). This one is a bit complex -- first you'll need to create and upload a HTML page containing the QUnit test page (setting the mime-type property to text/html using instructions below) into your repository, and then link to the raw page URL in the &lt;iframe&gt; inside your Gadget.</li>
	<li><strong>Embedding dynamic content</strong> and forms (<a href="http://code.google.com/p/urlecho/#AppEngine_server" target="_blank">example project wiki page</a> and <a href="http://code.google.com/p/urlecho/source/browse/trunk/googlecodegadget.xml" target="_blank">XML spec</a>). There is no outside content in the Gadget spec here, everything is in the Gadget. <strong>Note </strong>that the XML spec in the example also contains CSS styles so that the Gadget content has the same look-and -feel as the rest of the wiki page so visitors won't event know that a Gadget is there.</li>
</ol>
The second idea for extending project sites involves <strong>using the source repository for hosting non-source files</strong> which will be viewed by project site visitors. This is useful if you want the freedom of creating custom web pages (not embed them into project wikis) and if you want to keep all those files in the project repository (and not on some other server). The idea here is to use the raw-file URL for files in the repository - visiting the raw-file view URL returns only the content of the file without the UI stuff of Google Code. The problem is that, by default, any file uploaded to the repository is attributed to be of type <em>text</em>, so when you view the raw-file URL in the browser you get the content as text, which is useless if you want to see a HTML page. The good thing is that using custom SVN per-file properties it is possible to set the type of the file when it is uploaded in the repository. So, finally, the second idea for extending Google Code project hosting is to <strong>upload your content files into the repository and explicitly set their mime-types to the correct value</strong>. Here's what you need to do :
<ol>
	<li>Create your content and save it into a file on your hard drive in the project repository folder.</li>
	<li>Right-click the file and under TortoiseSVN -&gt; Properties click "New". In the "Property name" drop-down list choose "svn:mime-type" and ender the correct mime type for that file. You can find a list of all mime types here. E.g. for HTML pages it's "text/html" (this is for TortoiseSVN, command-line version is <a href="http://code.google.com/p/support/wiki/SubversionFAQ#How_can_I_make_SVN_serve_HTML_and_images_with_the_correct_Conten" target="_blank">here</a>).</li>
	<li>Upload the file to the repository.</li>
	<li>Obtain the URL to the raw-file by viewing the uploaded file in your project's Source tab and then clicking "View raw file" under File info.</li>
	<li>Create a link to the obtained URL from one of your wiki pages.</li>
</ol>

<table>
<tr>
<td>
<p><a href="http://pubsubhubbub.googlecode.com/svn/trunk/pubsubhubbub-core-0.2.html"><img class="aligncenter" title="Example 1" src="/images/blog_pshb.png" alt="Example 1" width="90%" /></a></p>
</td>
<td>
<p><a href="http://closure-library.googlecode.com/svn/trunk/closure/goog/docs/index.html"><img class="aligncenter" title="Example 2" src="/images/blog_closure.png" alt="Example 2" width="90%" /></a></p>
</td>
</tr>
<tr>
<td>
<center> 1. PubSubHubBub specification </center>
</td>
<td>
<center> 2. Closure library API docs </center>
</td>
</tr>
</table>

And here are two examples that show the usefulness of this approach (images above, in order of appearance):
<ol>
	<li><strong>Creating a protocol specification with custom styling</strong> (<a href="http://pubsubhubbub.googlecode.com/svn/trunk/pubsubhubbub-core-0.2.html" target="_blank">example project page</a> and <a href="http://code.google.com/p/pubsubhubbub/source/browse/trunk/pubsubhubbub-core-0.2.html" target="_blank">source file</a>)</li>
	<li><strong>Creating advanced API docs</strong> (<a href="http://closure-library.googlecode.com/svn/trunk/closure/goog/docs/index.html" target="_blank">example project page</a> and <a href="http://code.google.com/p/closure-library/source/browse/#svn/trunk/closure/goog/docs/index.html" target="_blank">source file</a> -- note: you will not be able to view the source file, access is being blocked somehow)</li>
</ol>
And that's it. Go fancy-up your project sites and let me know if you know any other cool ways of extenting Google Code project hosting. <a href="http://www.twitter.com/izuzak" target="_blank">@izuzak</a>
