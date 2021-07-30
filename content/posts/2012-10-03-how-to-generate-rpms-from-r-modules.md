---
title: "How to generate RPMs from R modules"
date: 2012-10-03T23:56:00+01:00
draft: false
---

Today I was setting up a CentOS server with some packages including [R](http://www.r-project.org/) and a couple of R modules.
R is generally available from third-party repositories like [EPEL](http://fedoraproject.org/wiki/EPEL) but its modules must be compiled locally, which requires a lot of dependencies like R-devel and GCC.

<!--more-->

As I wanted to keep the number of installed packages as low as possible there was the need to generate RPMs from the R modules allowing me to just install them in the very secure and lightweight server.

After some searching I came across [R2spec](https://fedorahosted.org/r2spec/) which is a tool just for what I need.
It worked like a charm, but not at first.

For instance, lets see what we need to do in order to generate the [randomForest](http://cran.r-project.org/web/packages/randomForest/) RPM.

```
# install packages (from the EPEL repository)
yum install R-devel R2spec rpm-build
 
# create environment
rm -Rf ~/rpmbuild
mkdir -p ~/rpmbuild/SPECS
mkdir -p ~/rpmbuild/SOURCES/
 
# download and generate spec and rpm
R2rpm -p randomForest --no-suggest --no-check --verbose
 
# the generated rpm may be found at
ls ~/rpmbuild/RPMS/x86_64/R-randomForest-4.6.6-1.el6.x86_64.rpm
 
# install rpm in the final machine
rpm -i R-randomForest-4.6.6-1.el6.x86_64.rpm
```