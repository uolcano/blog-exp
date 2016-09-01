---
title: Install Java On Ubuntu By CommandLine
date: 2016-05-07
category: Evironment Configuration
tags:
- Java
- Ubuntu
---

## Installing JRE/JDK

{% highlight bash %}
sudo apt-get update  # update the package index
java -version  # check if java installed
sudo apt-get install default-jre  # install JRE
sudo apt-get install default-jdk  # install JDK, if necessary
{% endhighlight %}

## Setting the "JAVA_HOME" environment variable  
1\. find out the path of JAVA installation

{% highlight bash %}
sudo update-alternatives --config java
{% endhighlight %}

It perhaps returns:

{% highlight bash %}
There is only one alternative in link group java (providing /usr/bin/java): /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java
{% endhighlight %}

Then, _the path_ is "/usr/lib/jvm/java-7-openjdk-amd64", hold it.

2\. use nano to edit the file /etc/environment

{% highlight bash %}
sudo nano /etc/environment
{% endhighlight %}

In this file, append _the path_, holden just now, into the quotes, seperate by colon, like this:

{% highlight text %}
JAVA_HOME="your/other/paths:/usr/lib/jvm/java-7-openjdk-amd64"
{% endhighlight %}

3\. reload the file /etc/environment

{% highlight bash %}
source /etc/environment
{% endhighlight %}

4\. check if java installed

{% highlight bash %}
java -version
{% endhighlight %}

Finally, it maybe not have to set the environment variable. But I don't test that.

## Refer
- [How To Install Java on Ubuntu with Apt-Get \| DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get/)