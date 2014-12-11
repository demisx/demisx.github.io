---
layout: post
title:  "How to Compile Tomcat mod_jk connector on Mac OS X Mavericks"
date:   2014-02-06 8:46:12
categories: tomcat connector
---

In my ongoing quest of porting apps from Windows to Mac OS X environment, I had to install Tomcat's [mod_jk
connector](http://tomcat.apache.org/download-connectors.cgi) to proxy requests from Apache to
JBoss application server. This is not my favorite setup, of course, but rather a legacy environment
for one of the clients. Everything was going smoothly until mod_jk compilation part. After doing
some research and reading some blogs, here is these are the steps that finally worked for me:

<!--more-->

1. Download the latest JK connector (1.2.37 at time of writing) for Mac OS X
from [Apache Tomcat Download](http://tomcat.apache.org/download-connectors.cgi) page.
2. Configure/make/install (if you didn't install Apache via Homebrew and/or the path to
`apxs` differs--adjust accordingly). {% highlight console %}
[dmoore]$ cd tomcat-connectors-1.2.37-src/native
[dmoore]$ ./configure CFLAGS='-arch x86_64' APXSLDFLAGS='-arch x86_64' --with-apxs=/usr/local/sbin/apxs
[dmoore]$ make
[dmoore]$ make install
{% endhighlight %}
3. Create directory for storing shared memory file, if missing. {% highlight console %}
mkdir /usr/local/Cellar/httpd/2.2.26/run
{% endhighlight %}

And you are done! The `mod_jk.so` module should be in Apache folder (in my case the path is
`/usr/local/Cellar/httpd/2.2.26/libexec/`, since Apache 2 has been installed via Homebrew)

### Resolving "Abort trap: 6" Error During Apache Start

If after following the steps above, you are running into "Abort trap: 6" error upon starting
Apache server, this may be due to the current buffer overlap bug in the connector. It still hasn't been
fixed in the latest version of the connector, so you'd need to manually patch it and recompile.
I've read about this fix on
[this blog](http://pablotips.blogspot.com/2013/12/compiling-modjk-on-mavericks.html) and was able
to successfully implement it on my end.

Follow these steps to patch this bug:


1. Open `tomcat-connectors-1.2.37-src/native/common/jk_map.c` and locate this block
of code {% highlight c# %}
int jk_map_get_int(jk_map_t *m, const char *name, int def)
{
    char buf[100];
    const char *rc;
    size_t len;
    int int_res;
    int multit = 1;

    sprintf(buf, "%d", def);
    rc = jk_map_get_string(m, name, buf);
    ... ... ...
{% endhighlight %}

2. Replace it with the following snippet: {% highlight c# %}
int jk_map_get_int(jk_map_t *m, const char *name, int def)
{
    char buf[100];
    char buf2[100];
    const char *rc;
    size_t len;
    int int_res;
    int multit = 1;

    sprintf(buf2, "%d", def);
    rc = jk_map_get_string(m, name, buf2);
    ... ... ...
{% endhighlight %}

3. Configure/make/install again following original step 2 at the beginning of this post.
The error should be gone now.


Hope this saves someone some wasted time.

___
##### Today my environment was:

- Mac OS X Mavericks 10.9.1
- Homebrew 0.9.5
- Apache/2.2.26 (Unix) via Homebrew
- Tomcat connector 1.2.37
