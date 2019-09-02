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
