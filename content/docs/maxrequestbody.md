---
title: maxrequestbody
type: docs
directive: true
---

maxrequestbody limits the size of request bodies. When the number of bytes read from a request body exceeds the limit, reading will terminate and an error will be sent to the client. (Technically, it depends on each middleware that reads a request body to handle the error properly, but standard Caddy directives should respond with HTTP 413.) By default, there is no size limit.

The size values must be positive integers and are interpreted as bytes unless a unit is given. Valid examples: `3500` (3,500 bytes), `500KB` (500 kilobytes), `10MB` (10 megabytes), `1GB` (1 gigabyte).

### Syntax

<code class="block"><span class="hl-directive">maxrequestbody</span> <span class="hl-arg"><i>size</i></span></code>

*   **size** is the maximum size for all request bodies.

<code class="block"><span class="hl-directive">maxrequestbody</span> <span class="hl-arg"><i>path size</i></span></code>

*   **path** is the base path for which to apply the size limit.
*   **size** is the maximum size.

You can also list paths and size limits in a table format, with as many rows as desired:

<code class="block"><span class="hl-directive">maxrequestbody</span> {
	<span class="hl-subdirective"><i>path</i></span> <i>size</i>
}</code>

### Examples

Limit all request bodies to 7.5 kilobytes:

<code class="block"><span class="hl-directive">maxrequestbody</span> <span class="hl-arg">7500</span></code>

Limit requests within /upload to 50 megabytes:

<code class="block"><span class="hl-directive">maxrequestbody</span> <span class="hl-arg">/upload 50MB</span></code>

Various request body size limits:

<code class="block"><span class="hl-directive">maxrequestbody</span> {
	<span class="hl-subdirective">/upload</span>  100MB
	<span class="hl-subdirective">/profile</span> 25KB
	<span class="hl-subdirective">/api</span>     10KB
}</code>
