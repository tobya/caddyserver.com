---
title: basicauth
type: docs
directive: true
---

basicauth implements HTTP Basic Authentication. Basic Authentication can be used to protect directories and files with a username and password. Note that basic auth is *not secure* over plain HTTP. Even with HTTPS, which encrypts the header (including the credentials), the security of basic auth is disputed. Use discretion when deciding what to protect with HTTP Basic Authentication.

When a user requests a resource that is protected, the browser will prompt the user for a username and password if they have not already supplied one. If the proper credentials are present in the Authorization header, the server will grant access to the resource. If the header is missing or the credentials are incorrect, the server will respond with HTTP 401 Unauthorized. Upon a successful authorization, the Authorization header will be stripped so that the credentials are not leaked.

This directive allows use of .htpasswd files by prefixing the password argument with `htpasswd=` and the path to the .htpasswd file to use. <mark>Support for .htpasswd is for legacy sites only and may be removed in the future; do not use .htpasswd with new sites.</mark>

### Syntax

<code class="block"><span class="hl-directive">basicauth</span> <span class="hl-arg"><i>path username password</i></span></code>

*   **path** is the file or directory to protect
*   **username** is the username
*   **password** is the password

This syntax is convenient for protecting a single file or base path/directory with the default realm "Restricted". To protect multiple resources or to specify a realm, use the following variation:

<code class="block"><span class="hl-directive">basicauth</span> <span class="hl-arg"><i>username password</i></span> {
    <i>resource</i>
    <span class="hl-subdirective">realm</span> <i>realm</i>
}</code>

*   **username** is the username
*   **password** is the password
*   **realm** identifies the protection partition; it is optional and cannot be repeated 

A single argument in a nested block indicates a file or directory to protect; this may be repeated.

The **realm** keyword is used to partition protection space on the server, each with its own authorization credentials. This can be convenient for user agents that are configured to remember authentication details.

### Examples

Protect all files in /secret so only Bob can access them with the password "hiccup":

<code class="block"><span class="hl-directive">basicauth</span> <span class="hl-arg">/secret Bob hiccup</span></code>

Protect multiple files and directories in the realm "Mary Lou's documents" so Mary Lou has access with her password "milkshakes":

<code class="block"><span class="hl-directive">basicauth</span> <span class="hl-arg">"Mary Lou" milkshakes</span> {
    /notes-for-mary-lou.txt
    /marylou-files
    /another-file.txt
    <span class="hl-subdirective">realm</span> "Mary Lou's documents"
}</code>
