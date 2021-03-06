# NginxHtmlHeadBlankFilter
A simple nginx filter module that inserts a text string after &lt;head> tag 

# Introduction
The repository contains an Nginx Resonse Body Filter Module that will filter a HTTP response and insert a specific text string
after the html &lt;head&gt; tag. For example, it can insert a monitoring javascript after the &lt;head&gt; tag.

This is a customized version of the 
[https://github.com/ngchianglin/NginxHtmlHeadFilter](https://github.com/ngchianglin/NginxHtmlHeadFilter)

In this version, a blank page will be displayed if the &lt;head> tag is not found. 

This filter module can be used together with Nginx proxy_pass, to insert a text string into HTTP responses from an upstream web server. 
The module will process HTTP 200 OK responses where the content type is text/html. The module will not process content that is compressed (gzip, deflate etc...).
If the &lt;head&gt; tag is not found within the first 256 characters of the HTTP response, a blank empty page will be sent. 

## Module Directives
The module takes a single directive that can be configured in the Nginx 's location context. 

  **html_head_filter**  

  * syntax: html_head_filter [text string]
  * default: none
  * context: Location

This directive enables the html head blank filter module. The argument "text string" will be inserted after the first &lt;head&gt; tag in the HTTP response body. 
If the &lt;head&gt; tag is not found within the first 256 characters, the module will log an alert message in the nginx error log and display a blank empty page. 


## Example Configuration

An example showing the insertion of a mymonitor1.js script after the &lt;head&gt; tag of html content. 
In this case, nginx is configured as a reverse proxy to a backend service running on localhost at port 8080. 

        location / {

            index  index.html index.htm;

            proxy_set_header HOST $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://127.0.0.1:8080;

            html_head_filter "<script src=\"mymonitor1.js\"></script>";

        }

## Compiling and Installation

The module works with the latest stable version of [Nginx 1.18.0](https://nginx.org/download/). 
Download the latest stable version of Nginx and its corresponding pgp signature.  Verify the signature of the downloaded tarball. 
Refer to [Nginx website](https://nginx.org/en/pgp_keys.html) for the public signing keys that can be used to verify the signature. 
Extract the nginx source tarball if the signature verification is ok. 

To obtain a copy of the Filter Module. 

    git clone https://github.com/ngchianglin/NginxHtmlHeadBlankFilter.git
    
Refer to the Source signature section below for instructions on verifying the module 's signature. 

To compile the module statically into nginx. The following configure option can be used.  

    --add-module=<Path to>/NginxHtmlHeadBlankFilter 

For example, the following commands can be used to compile the module and install nginx into **/usr/local/nginx** 
Note: These example commands do not include signature verification of the downloaded packages. 
For security, do verify the signatures of the downloads. 

    git clone https://github.com/ngchianglin/NginxHtmlHeadBlankFilter.git
    wget https://nginx.org/download/nginx-1.18.0.tar.gz
    tar -zxvf nginx-1.18.0.tar.gz
    cd nginx-1.18.0
    ./configure --with-cc-opt="-Wextra -Wformat -Wformat-security -Wformat-y2k -fPIE -O2 -D_FORTIFY_SOURCE=2 -fstack-protector-all" --with-ld-opt="-pie -Wl,-z,relro -Wl,-z,now -Wl,--strip-all"  --add-module=../NginxHtmlHeadBlankFilter
    make
    sudo make install


## Further Details

Please note that this filter should not be installed together with the original [NginxHtmlHeadFilter](https://github.com/ngchianglin/NginxHtmlHeadFilter) 
on the same instance of nginx. 

Refer to the original version [https://github.com/ngchianglin/NginxHtmlHeadFilter](https://github.com/ngchianglin/NginxHtmlHeadFilter), if you do not want the
page blanking feature. 

## Source signature
Gpg Signed commits are used for committing the source files.

> Look at the repository commits tab for the verified label for each commit, or refer to [https://www.nighthour.sg/git-gpg.html](https://www.nighthour.sg/git-gpg.html) for instructions on verifying the git commit.
>
> A userful link on how to verify gpg signature is available at [https://github.com/blog/2144-gpg-signature-verification](https://github.com/blog/2144-gpg-signature-verification)
