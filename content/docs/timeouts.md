---
title: timeouts
type: docs
directive: true
---

timeouts configures Caddy's HTTP timeouts:

- **Read:** Maximum duration for reading the entire request, including the body.
- **Read Header:** The amount of time allowed to read request headers.
- **Write:** The maximum duration before timing out writes of the response.
- **Idle:** The maximum time to wait for the next request (when using keep-alive).

By default, timeouts are disabled.

Timeouts are an important way to maintain server connectivity. However, some use cases may necessitate raising or removing the timeouts altogether (for example: legitimate clients on slow networks, long polling, or proxying long requests to trusted upstreams).

Because timeouts apply to an entire HTTP server which may serve multiple sites defined in your Caddyfile, the timeout values for each site will be reduced to their minimum values (with 0, or disabled, being the lowest) across all sites that share that server. It's a good idea to keep your timeouts the same or just set them on one site to avoid confusion. A timeout set on one site will apply to all sites that share that server.

### Syntax

To set all the timeouts to the same value:

<code class="block"><span class="hl-directive">timeouts</span> <span class="hl-arg"><i>val</i></span></code>

*   **val** is a duration value (e.g. 30s, 2m30s, 5m, 1h) that will apply to all timeouts. Set to `0` or `none` to disable timeouts.

You can also configure each timeout individually:

<code class="block"><span class="hl-directive">timeouts</span> {
	<span class="hl-subdirective">read</span>   <i>val</i>
	<span class="hl-subdirective">header</span> <i>val</i>
	<span class="hl-subdirective">write</span>  <i>val</i>
	<span class="hl-subdirective">idle</span>   <i>val</i>
}</code>

Valid values are durations, or `0` or `none` to disable the timeout.

### Examples

Set all timeouts to 1 minute:

<code class="block"><span class="hl-directive">timeouts</span> <span class="hl-arg">1m</span></code>

Set custom read and write timeouts:

<code class="block"><span class="hl-directive">timeouts</span> {
	<span class="hl-subdirective">read</span>  30s
	<span class="hl-subdirective">write</span> 20s
}</code>
