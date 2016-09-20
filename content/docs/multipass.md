---
title: multipass
type: docs
directive: true
plugin: true
link: https://github.com/namsral/multipass
---

Multipass can be used to protect web resources and services using user access control. Users can be registered by including their email address in the multipass directive. These users can then be authenticated using a challenge; prove they are the owner of the registered email address by following a login link.

Multipass implements the idea to authenticate users based on __something they own__ instead of __something they know__. This is better known as the second factor of [Two-factor Authentication][2fa].

Login links are encoded [JSON Web Tokens][jwt] containing information about the user and their accessible resources. These tokens are used as access tokens with an expiration date and are signed using a random RSA key pair to prevent forgeries.


### Syntax

<code class="block"><span class="hl-directive">multipass</span> {
	<span class="hl-subdirective">resources /fhloston /paradise</span>
	<span class="hl-subdirective">handles leeloo@dallas korben@dallas</span>
	<span class="hl-subdirective">basepath /multipass</span>
	<span class="hl-subdirective">expires 24h</span>
	<span class="hl-subdirective">smtp_addr localhost:2525</span>
	<span class="hl-subdirective">smtp_user vito</span>
	<span class="hl-subdirective">smtp_pass secret</span>
	<span class="hl-subdirective">mail_from "Multipass <no-reply@dallas>"</span>
}</code>

- __resources__ Path of resources to protect. _Default: /_
- __handles__ The handles which identify the users. _Required_
- __basepath__ Path to the log-in and sign-out page. _Default: /multipass_
- __expires__ Time duration after which the access token expires. Any time duration Go can [parse][goduration]. _Default: 24h_
- __smtp_addr__ SMTP address used for sending login links. _Default: localhost:25_
- __smtp_user__, __smtp_pass__ Access credentials to the SMTP server.
- __mail_from__ From address used in email messages sent to users. _Required_


### Examples

Protect all resources at /fhloston and /paradise so only users with handles leeloo@dallas and korben@dallas can access them. Unauthenticated users are redirected to /multipass. From here users can request an access token, sent using the specified SMTP server.

<code class="block"><span class="hl-directive">multipass</span> {
	<span class="hl-subdirective">resources /fhloston /paradise</span>
	<span class="hl-subdirective">handles leeloo@dallas korben@dallas</span>
	<span class="hl-subdirective">basepath /multipass</span>
	<span class="hl-subdirective">smtp_addr localhost:2525</span>
	<span class="hl-subdirective">mail_from "Multipass <no-reply@dallas>"</span>
}</code>


A minimal example with only required fields:

<code class="block"><span class="hl-directive">multipass</span> {
	<span class="hl-subdirective">handles leeloo@dallas</span>
	<span class="hl-subdirective">mail_from "Multipass <no-reply@dallas>"</span>
}</code>


### RSA Private key

Tokens are signed using a private RSA key which is randomly generated on startup. You can set your own RSA private key using the `MULTIPASS_RSA_PRIVATE_KEY` environment variable. Make sure the private key is PEM encoded.


[lets]:https://letsencrypt.org
[caddy]:https://caddyserver.com
[caddydocs]:https://caddyserver.com/docs
[jwt]:https://jwt.io
[goduration]:https://golang.org/pkg/time/#ParseDuration
[releases]:https://github.com/namsral/multipass/releases
[2fa]:https://en.wikipedia.org/wiki/Multi-factor_authentication
[using-pull-requests]:https://help.github.com/articles/using-pull-requests/
[preview]: https://namsral.github.io/multipass/img/multipass.png "Multipass preview image"
