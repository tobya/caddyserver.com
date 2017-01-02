---
title: startup
type: docs
directive: true
---

startup executes a command when the server begins. This is useful for preparing to serve a site by running a script or starting a background process like php-fpm. (Also see [shutdown](/docs/shutdown).)

Each command that is executed at startup is blocking, unless you suffix the command with a space and `&`, which will cause the command to be run in the background. The output and error of the command go to stdout and stderr, respectively. There is no stdin.

A command will only be executed once for each time it appears in the Caddyfile.

### Syntax

<code class="block"><span class="hl-directive">startup</span> <span class="hl-arg"><i>command</i></span></code>

*   **command** is the command to execute; it may be followed by arguments

### Examples

Start php-fpm before the server starts listening:

<code class="block"><span class="hl-directive">startup</span> <span class="hl-arg">/etc/init.d/php-fpm start</span></code>

On windows, you might need to use quotes when the command path contains spaces:

<code class="block"><span class="hl-directive">startup</span> <span class="hl-arg">"\"C:\Program Files\PHP\v7.0\php-cgi.exe\" -b 127.0.0.1:9123" &amp;</span></code>
