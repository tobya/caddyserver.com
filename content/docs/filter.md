---
title: filter
type: docs
directive: true
plugin: true
link: https://github.com/echocat/caddy-filter
---

filter allows you to modify the responses.

This could be useful to modify static HTML files to add (for example) Google Analytics source code to it.

### Syntax

<code class="block"><span class="hl-directive">filter rule</span> {
    <span class="hl-subdirective">path</span>               <i>regexp pattern</i></span>
    <span class="hl-subdirective">content_type</span>       <i>regexp pattern</i></span>
    <span class="hl-subdirective">search_pattern</span>     <i>regexp pattern</i></span>
    <span class="hl-subdirective">replacement</span>        <i>replacement pattern</i></span>
}
<span class="hl-directive">filter rule</span> ...
<span class="hl-directive">filter max_buffer_size</span> <i>maximum buffer size in bytes</i>
</code>


* **rule**: Defines a new filter rule for a file to respond.
    > **Important:** Define ``path`` and/or ``content_type`` not to open. Slack rules could dramatically impact the system performance because every response is recorded to memory before returning it.

    * **path**: Regular expression that matches the requested path.
    * **content_type**: Regular expression that matches the requested content type that results after the evaluation of the whole request.
    * **search_pattern**: Regular expression to find in the response body to replace it.
    * **replacement**: Pattern to replace the ``search_pattern`` with. 
        <br>You can use parameters. Each parameter must be formatted like: ``{name}``.
        * Regex group: Every group of the ``search_pattern`` could be addressed with ``{index}``.
        <br>Example: ``"My name is (.*?) (.*?)." => "Name: {2}, {1}."``
        * Request context: Parameters like URL ... could be accessed.
        <br>Example: ``Host: {request_host}``
            * ``request_header_<header name>``: Contains a header value of the request, if provided or empty.
            * ``request_url``: Full requested url
            * ``request_path``: Requested path
            * ``request_method``: Used method
            * ``request_host``: Target host
            * ``request_proto``: Used proto
            * ``request_proto``: Used proto
            * ``request_remoteAddress``: Remote address of the calling client
            * ``response_header_<header name>``: Contains a header value of the response, if provided or empty.
* **max_buffer_size**: Limit the buffer size to the specified maximum number of bytes. If a rules matches the whole body will be recorded at first to memory before delivery to HTTP client. If this limit is reached no filtering will executed and the content is directly forwarded to the client to prevent memory overload. Default is: ``10485760`` (=10 MB)

### Examples

Replace in every text file ``Foo`` with ``Bar``.

<code class="block"><span class="hl-directive">filter rule</span> {
    <span class="hl-subdirective">path</span>           .*\.txt</span>
    <span class="hl-subdirective">search_pattern</span> Foo</span>
    <span class="hl-subdirective">replacement</span>    Bar</span>
}</code>

Add Google Analytics to every HTML page.

<code class="block"><span class="hl-directive">filter rule</span> {
    <span class="hl-subdirective">path</span>           .*\.html</span>
    <span class="hl-subdirective">search_pattern</span> &lt;/title&gt;</span>
    <span class="hl-subdirective">replacement</span>    &quot;&lt;/title&gt;&lt;script&gt;(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)})(window,document,'script','//www.google-analytics.com/analytics.js','ga');ga('create', 'UA-12345678-9', 'auto');ga('send', 'pageview');&lt;/script&gt;&quot;</span>
}</code>

Insert server name in every HTML page

<code class="block"><span class="hl-directive">filter rule</span> {
    <span class="hl-subdirective">content_type</span>   text/html.*</span>
    <span class="hl-subdirective">search_pattern</span> Server</span>
    <span class="hl-subdirective">replacement</span>    &quot;This site was provided by {response_header_Server}</span>
}</code>
