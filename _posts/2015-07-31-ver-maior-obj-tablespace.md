---
title: Select para recupar o tamanho dos objetos em determinada tablespace
date: 2015-07-31 11:30:23 Z
categories:
- posts
---

Um simples select para mostrar quais os maiores objetos de uma tablespace do oracle.

<!--more-->

Basta copiar e colar ele e entrar o nome da tablespace desejada.
{% highlight sql %}
select owner,segment_name,segment_type
,bytes/(1024*1024) size_m
from dba_segments
where tablespace_name = UPPER('&TBS')
--    bytes(1024*1024) 
order by size_m asc;
{% endhighlight %}
