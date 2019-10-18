# Nginx HTTP Server, 3rd Edition

By: Clément Nedelcu. [Purchase the book](https://www.packtpub.com/networking-and-servers/nginx-http-server-third-edition)!

Date Started: Saturday, August 31, 2019

Date Finished: ongoing

## Chapter 1: Downloading and Installing Nginx

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

## Chapter 2: Basic Nginx Configuration

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

## Chapter 3: HTTP Configuration

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

## Chapter 4: Module Configuration

- URL rewriting is a key element of ***Search Engine Optimization (SEO)***. URL rewriting is performed by the rewrite directive, which accepts a pattern followed by the replacement URI.
- Once rewritten, the URI is matched against the location blocks in order to find the configuration that should be applied to the request.
- There are two different types of internal requests:
  - ***Internal redirects***: Nginx redirects the client requests internally.
  - ***Sub-requests***: These are additional requests that are triggered internally to generate content that is complementary to the main request.
- The number of internal redirect cycles is restricted to 10. Anything past this limit and Nginx will produce a 500 Internal Server Error.
- The purpose of ***SSI*** module is for the server to parse documents before sending the response to the client in a fashion somewhat similar to PHP or other preprocessors.
- The `rewrite` directive allows rewriting the URI of the current request, thus resetting the treatment of the said request.
- The `break` directive is used to prevent further rewrite directives. Past
this point, the URI is fixed and cannot be altered.
- The `return` directive interrupts the processing of the request, and returns the specified HTTP status code or specified text.
- The `set` directive initializes or redefines a variable. Note that some variables cannot be redefined, for example, you are not allowed to alter $uri.
- The `rewrite_log` directive issues log messages for every operation performed by the rewrite engine at the notice error level.
- ***SSI*** or Server Side Includes, is actually a sort of server-side programming language interpreted by Nginx. Its name originates from the fact that the most-used functionality of the language is the include command.
- The `ssi` directive enables parsing files for SSI commands. Nginx only parses the files corresponding to the MIME types selected with the `ssi_types` directive.
- The ***Index*** module provides a simple directive named `index`, which defines the page that Nginx will serve by default if no filename is specified in the client request (in other words, it defines the website index page).
- If Nginx cannot provide an index page for the requested directory, the default behavior is to return a ***403 Forbidden*** HTTP error page.
- The `autoindex` directive enables an automatic listing of the files that are present in the requested directory.
- The `autoindex_exact_size` directive if set to on, this directive ensures that the listing displays the file sizes in bytes. Otherwise, another unit is employed, such as KB, MB, or GB.
- The `autoindex_format` directive enables to serve the directory index in different formats: ***HTML***, ***XML***, ***JSON***, or ***JSONP*** (by default, HTML is used).
- The ***Log*** module controls the behavior of Nginx regarding the access logs. It allows analyzing the runtime behavior of web applications.
- The `access_log` directive defines the access log file path, the format of entries in the access log by selecting a template name, or disables access logging.
- The `log_format` directive defines a template to be utilized by the access_log directive, describing the contents that should be included in an entry of the access log.
- The `$request_time` variable defines the total length of the request processing, in milliseconds.
- The ***Auth_basic*** module enables the basic authentication functionality.
- The `auth_basic` directive can be set to either off or a text message, usually referred to as ***authentication challenge*** or ***authentication realm***. This message is displayed by the web browsers in a username/password box when a client attempts to access the protected resource.
- The `auth_basic_user_file` directive defines the path of the password file relative to the directory of the configuration file.
- The `allow` and `deny` directives allow or deny access to a resource for a specific IP address or IP address range.
- The ***Limit Connections*** module allows defining the maximum number of simultaneous connections to the server for a specific zone. If the limit is reached, all additional concurrent requests will be answered with a ***503 Service unavailable*** HTTP response.
- The ***Auth_request*** module  used to allow or deny access to a resource based on the result of a sub-request. Nginx calls the URI that you specify via the auth_request directive: if the sub-request returns a 2XX response code (that is, HTTP/200 OK), access is allowed. If the sub-request returns a 401 or 403 status code, access is denied, and Nginx forwards the response code to the client. Should the backend return any other response code, Nginx will consider it to be an error and deny access to the resource.
- The ***Addition*** module allows you (through simple directives) to add content before or after the body of the HTTP response.
- The ***Substitution*** module allows to search and replace text directly from the response body.
- The ***SSL*** module enables ***HTTPS*** support, ***HTTP over SSL/TLS*** in particular. It gives the option to serve secure websites by providing a certificate, a certificate key, and many other parameters.
- The `ssl` directive enables HTTPS for the specified server. This directive is the equivalent of `listen 443 ssl`.
- The `ssl_certificate` directive sets the path of the *PEM certificate*.
- The `ssl_certificate_key` directive sets the path of the *PEM secret* key file.
- The `ssl_client_certificate` directive sets the path of the client *PEM certificate*.
- The `$ssl_protocol` variable indicates the protocol in use for the current request.
- ***SSL Stapling***, also called ***Online Certificate Status Protocol (OCSP) Stapling***, is a technique that allows clients to easily connect and resume sessions to an SSL/TLS server without having to contact the Certificate Authority, thus reducing the SSL negotiation time.
- In the case of high traffic websites, this can cause a huge stress on the CA servers. An intermediary solution was designed—Stapling. The OCSP record is obtained periodically from the CA by your server itself, and is stapled to exchanges with the client. The OCSP record is cached by your server for a period of up to 48 hours in order to limit communications with the CA.
- ***Memcached*** is a daemon application that can be connected to via sockets. Its main purpose, as the name suggests, is to provide an efficient *distributed key/value memory caching system*. The Nginx ***Memcached module*** provides directives allowing you to configure access to the Memcached daemon.
- Note that the Nginx Memcached module is only able to retrieve data from the cache; it does not store the results of requests. Storing data in the cache should be done by a server-side script. You just need to make sure to employ the same key-naming scheme in both your server-side scripts and the Nginx configuration.
- The ***Gzip*** allows you to compress the response body with the Gzip algorithm before sending it to the client. To enable ***Gzip compression***, use the gzip directive (on or off) at the http, server, location, and even the if level (though that is not recommended).
- The `gzip_comp_level` directive defines the compression level of the algorithm. The specified value ranges from 1 (low compression, faster for the CPU) to 9 (high compression, slower).
- The `gzip_min_length` directive determine if the response body length is inferior to the specified value, it is not compressed.
- The ***Gunzip filter*** module decompress a gzip-compressed response sent from the backend in order to serve it raw to the client.
- The ***Image*** module provides image processing functionalities through the ***GD Graphics Library*** (also known as gdlib).
- The `image_filter` directive applies a transformation on the image before sending it
to the client. There are five options available:
  - ***test***: Makes sure that the requested document is an image file, returns a ***415 Unsupported Media Type*** HTTP error if the test fails.
  - ***size***: Composes a simple JSON response indicating information about the image such as the size and type.
  - ***resize*** width height: Resizes the image to the specified dimensions.
  - ***crop*** width height: Selects a portion of the image of the specified dimensions.
  - ***rotate*** 90 | 180 | 270: Rotates the image by the specified angle (in degrees).
- The Nginx ***XSLT*** module allows you to apply an XSLT transform on an XML file or response received from a backend server (proxy, FastCGI, and so on) before serving the client.
- The ***Browser*** module parses the User-Agent HTTP header of the client request in order to establish values for some variables.
- The ***Geo*** provides a functionality that is quite similar to the map directive—affecting a variable based on the client data (in this case, the IP address).
- The ***GeoIP*** provides accurate geographical information about your visitors by making use of the ***MaxMind*** (http://www.maxmind.com) GeoIP binary databases.
- The ***Real IP*** module provides one simple feature—it replaces the client IP address by the one specified in the ***X-Real-IP*** HTTP header for clients that visit your website behind a proxy, or for retrieving IP addresses from the proper header if Nginx is used as a backend server.
- Firstly insert the real_ip_header directive that defines the HTTP header to be exploited—either ***X-Real-IP*** or ***X-Forwarded-For***. The second step is to define the trusted IP addresses, in other words, the clients that are allowed to make use of those headers.
- The ***Split Clients*** module provides a resource-efficient way to split the visitor base into subgroups based on the percentages that you specify. To distribute the visitors into one group or another, Nginx hashes a value that you provide (such as the visitor's IP address, cookie data, query arguments, and so on), and decides which group the visitor should be affected to.
- The ***Secure link*** module is totally independent from the SSL module. It provides  basic protection by checking the presence of a specific hash in the URL before allowing the user to access a resource.
- The ***Stub status*** module was designed to provide information about the current state of the server, such as the amount of active connections, the total handled requests, and more. To activate it, place the stub_status directive in a location block. All requests matching the `location` block will produce the status page.
- The ***HTTP Degradation*** module configures the server to return an error page when the server runs low on memory. It works by defining a memory amount that is to be considered low, and then specifies the locations for which to enable the degradation check.
- The ***Google-perftools*** module interfaces the Google Performance Tools profiling mechanism for the Nginx worker processes. The tool generates a report based on the performance analysis of the executable code.
- To integrate a third-party module into the Nginx build:
  - Download the .tar.gz archive associated with the module that you wish
to download.
  - Extract the archive with the following command: `tar xzf module.tar.gz`.
  - Configure your Nginx build with the following command: `./configure --add-module=/module/source/path […]`