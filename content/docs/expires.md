---
title: expires
type: docs
directive: true
plugin: true
link: https://github.com/epicagency/caddy-expires
---

expires allows you to set expiration header relative to the request time.

It allows you to set different expiration durations base on a path matching a regular expression.

### Syntax

<code class="block"><span class="hl-directive">expires</span> {
	<span class="hl-subdirective">match</span> regex duration
}</code>

* **match** a regular expression matching on path and an expiration duration.

Match subdirective can be repeated as many times as you want but only the first matching will be used.

Duration is a combination of 0y0m0d0h0i0s in that order. Parts can be omitted.

### Examples

Expires various assets.

<code class="block"><span class="hl-directive">expires</span> {
    <span class="hl-subdirective">match</span> some/path/.*\.css$ 1y <span class="hl-comment"># expires
    css files in some/path after one year</span>
    <span class="hl-subdirective">match</span> \.js$ 1m <span class="hl-comment"># expires
    js files after 30 days</span>
    <span class="hl-subdirective">match</span> \.png$ 1d <span class="hl-comment"># expires
    png files after one day</span>
    <span class="hl-subdirective">match</span> \.jpg$ 1h <span class="hl-comment"># expires
    jpg files after one hour</span>
    <span class="hl-subdirective">match</span> \.pdf$ 1i <span class="hl-comment"># expires
    pdf file after one minute</span>
    <span class="hl-subdirective">match</span> \.txt$ 1s <span class="hl-comment"># expires
    txt files after one second</span>
    <span class="hl-subdirective">match</span> \.html$ 5i30s <span class="hl-comment"># expires
    html files after 5 minutes 30 seconds</span>
}</code>
