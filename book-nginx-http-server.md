# Nginx HTTP Server, 3rd Edition

By: Clément Nedelcu. [Purchase the book](https://www.packtpub.com/networking-and-servers/nginx-http-server-third-edition)!

Date Started: Saturday, August 31, 2019

Date Finished: ongoing

### Chapter 1: Downloading and Installing Nginx

- Nginx is faster by making use of asynchronous sockets, Nginx does not spawn processes as many times as it receives requests.

- Nginx is a completely open source project released under a BSD-like license, but it
  also comes with a powerful plugin system referred to as "modules".

- Installing and building Nginx from source steps:  
  
  - Install a compiler tool, such as the GNU Compiler Collection (GCC).

    ```bash
    apt-get install build-essentials
    ```
  
  - Install the Perl Compatible Regular Expression (PCRE) library.

    ```bash
    apt-get install libpcre3 libpcre3-dev
    ```
  
  - Install the zlib library.

    ```bash
    apt-get install zlib1g zlib1g-dev
    ```
  
  - Install the OpenSSL library.

    ```bash
    apt-get install openssl openssl-dev
    ```
  
  - Configure the Nginx with the optional configuration switches.

    ```bash
    ./configure <configuration_switch>
    ```
  
  - Compile and install Nginx.

    ```bash
    make
    make install
    ```

- Nginx has two levels of processes:
  
  - The Nginx master process.
  
  - The Nginx worker processes.

- The Nginx binary accepts command-line arguments to perform various operations, among which is controlling the background processes.
  
  ```bash
  nginx -h
  ```

- `nginx -t` checks the syntax, validity, and integrity of the configuration.

- Most Linux-based operating systems to date use a System-V style init daemon. In other words, their startup process is managed by a daemon called init, which functions in a way that is inherited from the old System V Unix-based operating system.

### Chapter 2: Basic Nginx Configuration

- The Nginx configuration file can be described as a list of directives organized in a
logical structure. The entire behavior of the application is defined by the values that
you give to those directives.

- Comments in Nginx are start with the `#` sign, they are not interpreted and have no value whatsoever. Its
sole purpose is to be read by whoever opens the file, or to temporarily disable parts
of an existing configuration section.

- The `include` directive will perform an inclusion of the specified file. In other words, the contents of the file will be inserted at this exact location.

- Modules may also enable directive blocks, which allow for a logical construction of the configuration. The root of the configuration file is also known as the main block.

- The `http` block contains a variety of configuration directives as well as one or more server blocks. A `server` block allows to configure a virtual host.
  
- Within the `server` block, may contain one or more location blocks. These allow to enable settings only when the requested URI matches the specified path.

- The `location` block and the `rewrite` directive support complex expressions in order to match particular patterns.

- The diminutives for specifying a file size in the context of a directive value are:
  - k or K: Kilobytes
  - m or M: Megabytes
  - g or G: Gigabytes

- The diminutives for specifying a time value in the context of a directive value are:
  - ms: Milliseconds
  - s: Seconds
  - m: Minutes
  - h: Hours
  - d: Days
  - w: Weeks
  - m: Months (30 days)
  - y: Years (365 days)
  
- Variables in Nginx always start with `$`—the dollar sign.
- Nginx contains three base modules:
  - Core module.
  - Events module.
  - Configuration module.

- A unique process—the Master Process—exists in memory from the very moment that
Nginx starts. It is launched with the current user and group permissions, it does not process any client request itself; instead, it spawns the processes that do, that is, the Worker Processes, which are affected to a customizable user and group.

- `worker_processes`: Defines the number of worker processes. If a process gets blocked due to slow I/O operations, the incoming requests can be delegated to the other worker processes.
- `worker_connections`: Defines the number of connections that a worker process may treat
simultaneously.
- `worker_priority`: Defines the priority for the worker processes. If the system performs other tasks simultaneously, this directive grants a higher priority to the Nginx worker processes.

- Some performance test or load test tools:
  - [httperf](https://github.com/httperf/httperf)
  - [Autobench](https://github.com/menavaur/Autobench)
  - [OpenWebLoad](http://openwebload.sourceforge.net/)

### Chapter 3: HTTP Configuration

- The ***HTTP Core module*** is the component that contains all the fundamental blocks,
directives, and variables of the HTTP server. It's largest of all the standard Nginx modules and enabled by default.
- The HTTP module introduces *three logical blocks*:
  - `http`: This block encompasses the entire web-related configuration. It may contain one or more `server` blocks, defining the different domains and sub-domains.
  - `server`: This block allows declaring a website identified by one or more hostnames.
  - `location`: This block allowing defining a group of settings to be applied to a particular location on a website.
- Defining a setting at the `http` block level (for example, gzip on to enable gzip compression), will make the setting preserves its value in the potentially incorporated server and location blocks.
- The `listen` directive specifies the IP address and/or the port to be used by the listening socket that serves the website.
- The `server_name` directive assigns one or more hostnames to the server block. This directive can accept wildcards as well as regular expressions (in which case, the hostname should start with the `~` character).
- The `reset_timedout_connection` directive erases all memory associated with the connection after it times out.
- The `root` directive defines the document root containing the files that supposed to be served.
- The `alias` directive assigns a different path for Nginx to retrieve documents for a specific request.
- The `error_page` directive affects URIs to the HTTP response code and optionally, to substitute the code with another.
- The `index` directive defines the default page that Nginx will serve if no filename is specified in the request (in other words, the index page).
- The `try_files` directive attempts to serve the specified files (arguments 1 to N-1). If none of these files exist, it jumps to the respective named location block.
- The `keepalive_requests` directive specifies the maximum number of requests served over a single keep-alive connection.
- The `send_timeout` directive specifies the amount of time after which Nginx closes an inactive connection. A connection becomes inactive the moment a client stops transmitting data.
- The `ignore_invalid_headers` directive returns a 400 Bad Request HTTP error in case the request headers are malformed.
- The `types` directive establishes correlations between the MIME types and file extensions. The ***MIME*** type is then sent as the value of the `Content-Type` HTTP header in the response.
- The `satisfy` directive defines whether clients require all access conditions to be valid (satisfy all) or at least one (satisfy any).
- The `internal` directive specifies that the location block is internal. In other words, the specified resource cannot be accessed by external requests.
- The `log_not_found` directive enables or disables the logging of 404 Not Found HTTP errors.
- The `merge_slashes` directive will have the effect of merging multiple consecutive slashes in a URI.
- The `resolver` directive specifies the name servers that should be employed by Nginx to resolve hostnames to IP addresses and vice versa.
- The `post_action` directive defines a ***post-completion action***, a URI that will be called by Nginx after the request has been completed.
- ***Location modifier*** defines the way Nginx matches the specified pattern, and also defines the very nature of the pattern (simple string or regular expression).
- The `=` modifier: The requested document URI ***must match*** the specified pattern exactly. The pattern here is limited to a simple literal string.
- No modifier: The requested document URI must begin with the specified pattern.
- The `~` modifier: The requested URI must be a case-sensitive match to the specified regular expression.
- The `@` modifier: Defines a named location block. These blocks cannot be accessed by the client but only by internal requests generated by other directives.
- When Nginx receives a request, it searches for the location block that best matches the requested URI.

```Nginx
server {
  server_name website.com;
  
  location /files/ {
    # applies to any request starting with "/files/"
    # for example /files/doc.txt, /files/, /files/temp/
  }
  
  location = /files/ {
    # applies to the exact request to "/files/"
    # and as such does not apply to /files/doc.txt
    # but only /files/
  }
}
```

When a client visits http://website.com/files/doc.txt, the first location block
applies. However, when they visit http://website.com/files/, the second block
applies (even though the first one matches), because it has priority over the first one
(it is an exact match).

- Nginx will search for matching patterns in a specific order:
  - `location` blocks with the `=` modifier: If the specified string exactly matches the requested URI, Nginx retains the `location` block.
  - `location` blocks with no modifier: If the specified string exactly matches the requested URI, Nginx retains the `location` block.
  - `location` blocks with the ^~ modifier: If the specified string matches the beginning of the requested URI, Nginx retains the `location` block.
  - `location` blocks with ~ or ~* modifier: If the regular expression matches the requested URI, Nginx retains the `location` block.
  - `location` blocks with no modifier: If the specified string matches the beginning of the requested URI, Nginx retains the `location` block.
