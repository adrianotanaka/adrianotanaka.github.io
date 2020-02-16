---
title: 'Corrigindo o erro bad interpreter: No such file or directory no Oracle Linux,
  CentOS ou RedHat'
date: 2015-06-17 10:05:23 Z
categories:
- posts
---

Hoje ao tentar instalar um pacote usando o comando yum install em uma máquina de teste, o seguinte erro foi apresentado:

{% highlight bash %}
[root@presizing ~]# yum install httpd
-bash: /usr/bin/yum: /usr/bin/python: bad interpreter: No such file or directory
{% endhighlight %}

<!--more-->

Achei estranho esse erro, mas ao verificar o local para onde ele estava apontando(/usr/bin/python), acabei achando o erro:
{% highlight bash %}
[root@presizing ~]# cd /usr/bin/
[root@presizing bin]# ls -lhtr python*
-rwxr-xr-x. 1 root root 1.4K Jan 22  2014 python2.6-config
-rwxr-xr-x. 1 root root 8.9K Jan 22  2014 python2.6
--->lrwxrwxrwx. 1 root root    6 Feb 19 10:57 python2 -> python
lrwxrwxrwx. 1 root root   16 Feb 19 11:08 python-config -> python2.6-config
--->lrwxrwxrwx. 1 root root   18 Feb 19 12:04 python -> /usr/bin/python2.7
{% endhighlight %}

O caminho /usr/bin/python aponta para /usr/bin/python2.7 , um caminho que não existe, para resolver esse erro, tive que recriar os links simbólicos:
{% highlight bash %}
[root@presizing bin]# ln -f -s python2.6 python
[root@presizing bin]# ln -f -s python2.6 python2
{% endhighlight %}

Feito isso o pacote foi instalado com sucesso:
{% highlight bash %}
[root@presizing bin]# yum install httpd
Loaded plugins: refresh-packagekit, security
...
Updated:
  httpd.x86_64 0:2.2.15-39.0.1.el6                                                         

Dependency Updated:
  httpd-tools.x86_64 0:2.2.15-39.0.1.el6                                                   

Complete!
{% endhighlight %}
