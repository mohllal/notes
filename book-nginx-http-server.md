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
