---
title: ratelimit
type: docs
directive: true
plugin: true
link: https://github.com/xuqingfeng/caddy-rate-limit
---

ratelimit is used to limit the request processing rate based on client's IP address. Excessive requests will be terminated with an error 429 (Too Many Requests).

### Syntax

For single resource:

<code class="block"><span class="hl-directive">ratelimit</span> <span class="hl-arg"><i>path rate burst unit</i></span></code>

*   **path** is the file or directory to apply rate limit
*   **rate** is the limited request in every time unit (r/s, r/m, r/h) (eg. 1)
*   **burst** is the maximum burst size client can exceed; burst >= rate (eg. 2)
*   **unit** is the time interval (currently support: second, minute, hour)

For multiple resources:

<code class="block"><span class="hl-directive">ratelimit</span> <span class="hl-arg"><i>rate burst unit</i></span> {
    <span class="hl-subdirective"><i>resources</i></span>
}</code>

*   **resources** is a list of files/directories to apply rate limit, one per line

### Examples

Limit clients to 2 requests per second (bursts of 3) to any resources in /r: 

<code class="block"><span class="hl-directive">ratelimit</span> <span class="hl-arg">/r 2 3 second</span></code>

For the listed paths, limit clients to 2 requests per minute (bursts of 2):

<code class="block"><span class="hl-directive">ratelimit</span> <span class="hl-arg">2 2 minute</span> {
    <span class="hl-subdirective">/foo.html</span>
    <span class="hl-subdirective">/dir</span>
}</code>
