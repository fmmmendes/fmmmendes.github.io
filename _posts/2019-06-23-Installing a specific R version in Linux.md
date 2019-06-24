---
title:  "Installing a specific R version in Linux"
date:   2019-06-23 23:30:00
categories: []
tags: [R,linux]
comments: true
---

Some times you need to use a specific version of your tools. When can be necessary? One example is when you want to replicate a production environment to a development environemnt, and ensure the minimum of divergence between versions. 
In this post I'll demonstrate an installation of R 3.5.1 (current release is v3.6.1) into my virtual machine running Ubuntu OS. 

Here some macro steps:

1. Check your linux distro version
2. Download the R 3.5.1 
3. Unzip tar file to a specific folder
4. Make 
5. Run R
6. Test install some packages (tidyverse) 

## Check Distribution version

There is diferent ways to check this information in your terminal console. Here one command example:

```sh
$ /etc/*-release

DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.10
DISTRIB_CODENAME=cosmic
DISTRIB_DESCRIPTION="Ubuntu 18.10"
NAME="Ubuntu"
VERSION="18.10 (Cosmic Cuttlefish)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.10"
VERSION_ID="18.10"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=cosmic
UBUNTU_CODENAME=cosmic

```

So, I'm running Ubuntu 18.10 (Cosmic Cuttflefish), then I'll need to find the realase version for this distribution. 

## Download the R 3.5.1 tar file

In this step you haver 2 options: 

* [R Sources](https://cloud.r-project.org/sources.html)
* [R Binaries](https://cloud.r-project.org/bin/)

What's the diference between them? In R Source you will need to compille in your OS, R Binaries are already compiled and there are several releases for each OS.

In each link you can search for [old R releases](https://cloud.r-project.org/bin/linux/ubuntu/cosmic-cran35/)

![alt text](/images/r_binaries.PNG) "R Binaries"

Copy the link location (https://cloud.r-project.org/bin/linux/ubuntu/cosmic-cran35/r-base_3.5.1.orig.tar.gz) and download 

```sh
# from your home path

$ cd Downloads

wget "https://cloud.r-project.org/bin/linux/ubuntu/cosmic-cran35/r-base_3.5.1.orig.tar.gz"

```

## Unzip tar file

```sh

$ tar xvzf r-base_3.5.1.orig.tar.gz

```

```sh

$ cd R-3.5.1

$ ls
ChangeLog    configure     COPYING  etc      m4           Makefile.fw  po      share  SVN-REVISION  tools    VERSION-NICK             
config.site  configure.ac  doc      INSTALL  Makeconf.in  Makefile.in  README  src    tests         VERSION  VERSION-NICK

```

There is a README and INSTALL file

You can look with more detail on those files by pressing the following commands:

```sh
$ cat README
$ cat INSTALL
```

The INSTALL file is one that we need for the next steps

## Configura, Make and Install

Base on the information from the "INSTALL" file, lets procedure with our installation. First step is the configuration. It might be some issues realated to unvaulable packages, I've solved it step by step in each case, google is always a good friend in this moments.

```sh
$ ./configure

checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
loading site script './config.site'
loading build-specific script './config.site'
checking for pwd... /bin/pwd
checking whether builddir is srcdir... yes
checking whether ln -s works... yes
checking for ar... ar
checking for a BSD-compatible install... /usr/bin/install -c
checking for sed... /bin/sed
checking for which... /usr/bin/which
checking for less... /usr/bin/less
checking for gtar... no
checking for gnutar... no
checking for tar... /bin/tar
checking for tex... no
checking for pdftex... no
configure: WARNING: you cannot build PDF versions of the R manuals
checking for pdflatex... no
configure: WARNING: you cannot build PDF versions of vignettes and help pages
checking for makeindex... no
checking for texi2any... no
configure: WARNING: you cannot build info or HTML versions of the R manuals
checking for texi2dvi... no
checking for kpsewhich... no
checking for latex inconsolata package... checking for unzip... /usr/bin/unzip
checking for zip... /usr/bin/zip
checking for gzip... /bin/gzip
checking for bzip2... /bin/bzip2
checking for firefox... /usr/bin/firefox
using default browser ... /usr/bin/firefox
checking for acroread... no
checking for acroread4... no
checking for xdg-open... /usr/bin/xdg-open
checking for working aclocal... missing
checking for working autoconf... missing
checking for working autoheader... missing
checking for bison... no
checking for byacc... no
checking for yacc... no
checking for notangle... false
checking for realpath... /usr/bin/realpath
checking for pkg-config... /usr/bin/pkg-config
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking how to run the C preprocessor... gcc -E
checking for grep that handles long lines and -e... /bin/grep
checking for egrep... /bin/grep -E
checking whether gcc needs -traditional... no
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking minix/config.h usability... no
checking minix/config.h presence... no
checking for minix/config.h... no
checking whether it is safe to define __EXTENSIONS__... yes
checking how to run the C preprocessor... gcc -E
checking for f95... no
checking for fort... no
checking for xlf95... no
checking for ifort... no
checking for ifc... no
checking for efc... no
checking for pgf95... no
checking for lf95... no
checking for gfortran... no
checking for ftn... no
checking for g95... no
checking for f90... no
checking for xlf90... no
checking for pgf90... no
checking for pghpf... no
checking for epcf90... no
checking for g77... no
checking for f77... no
checking for xlf... no
checking for frt... no
checking for pgf77... no
checking for cf77... no
checking for fort77... no
checking for fl32... no
checking for af77... no
checking for fc... no
configure: error: No F77 compiler found


```
It seems there is something wrong with the compiler, after some "googling" there was some tips related to a old compile required.

```sh
apt-get install gfortran
```

configure: error: --with-readline=yes (default) and headers/libs are not available

```sh
$ ./configure --with-readline=no --with-x=no

...

checking whether zlib support suffices... yes
checking mmap support for zlib... yes
checking for BZ2_bzlibVersion in -lbz2... no
checking whether bzip2 support suffices... configure: error: bzip2 library and headers are required


```

Solved it with:

```sh
$ sudo apt-get install libbz2-dev
```
Trying again

```sh
$ ./configure --with-readline=no --with-x=no

...

checking mmap support for zlib... yes
checking for BZ2_bzlibVersion in -lbz2... yes
checking bzlib.h usability... yes
checking bzlib.h presence... yes
checking for bzlib.h... yes
checking if bzip2 version >= 1.0.6... yes
checking whether bzip2 support suffices... yes
checking for lzma_version_number in -llzma... no
configure: error: "liblzma library and headers are required"
```

Solved it with:

```sh
sudo apt-get install liblzma-dev
```

Trying again

```sh
$ ./configure --with-readline=no --with-x=no

...

configure: error: libcurl >= 7.22.0 library and headers are required with support for https

```

Solved it with:

```sh
sudo apt-get install libcurl4-openssl-dev
```

Note: In some forums there are some tips about how to install your linux packages before install R.

```sh
sudo apt-get install build-essential
sudo apt-get install fort77
sudo apt-get install xorg-dev
sudo apt-get install liblzma-dev  libblas-dev gfortran
sudo apt-get install gcc-multilib
sudo apt-get install gobjc++
sudo apt-get install aptitude
sudo aptitude install libreadline-dev
```

Ok, no more errors, it is expect to see this output at the end of configuration procedure.

```sh
R is now configured for x86_64-pc-linux-gnu

  Source directory:          .
  Installation directory:    /usr/local

  C compiler:                gcc  -g -O2
  Fortran 77 compiler:       f95  -g -O2

  Default C++ compiler:      g++   -g -O2
  C++98 compiler:            g++ -std=gnu++98 -g -O2
  C++11 compiler:            g++ -std=gnu++11 -g -O2
  C++14 compiler:            g++ -std=gnu++14 -g -O2
  C++17 compiler:            g++ -std=gnu++17 -g -O2
  Fortran 90/95 compiler:    gfortran -g -O2
  Obj-C compiler:             

  Interfaces supported:      
  External libraries:        curl
  Additional capabilities:   NLS
  Options enabled:           shared BLAS, R profiling

  Capabilities skipped:      PNG, JPEG, TIFF, cairo, ICU
  Options not enabled:       memory profiling

  Recommended packages:      yes

configure: WARNING: you cannot build info or HTML versions of the R manuals
configure: WARNING: you cannot build PDF versions of the R manuals
configure: WARNING: you cannot build PDF versions of vignettes and help pages

```

By default, the instalation path will be at: /usr/local .  
Next step is to perfom the make and install (just following the "INSTALL" documentation)

```sh
$ make install
```

After several minutes, R is installed and ready to run.

## Run R

```sh
$ R

R version 3.5.1 (2018-07-02) -- "Feather Spray"
Copyright (C) 2018 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> 

```

R is running!

## Test it with dplyr package 

[Dplyr](https://dplyr.tidyverse.org/) is a very popular package in R community, let's try to install it in R session. Open and R session and type:

```r
install.packages("dplyr")
```
After install, enable the package and do som transformation in default dataframe ("starwars").
```r
library(dplyr)

starwars %>% 
  mutate(name, bmi = mass / ((height / 100)  ^ 2)) %>%
  select(name:mass, bmi)

# A tibble: 87 x 3
   name               height  mass
   <chr>               <int> <dbl>
 1 Luke Skywalker        172    77
 2 C-3PO                 167    75
 3 R2-D2                  96    32
 4 Darth Vader           202   136
 5 Leia Organa           150    49
 6 Owen Lars             178   120
 7 Beru Whitesun lars    165    75
 8 R5-D4                  97    32
 9 Biggs Darklighter     183    84
10 Obi-Wan Kenobi        182    77
# … with 77 more rows

```
Ok, seems to be done! This was my procedure (and self-learning) about how to install R not from a traditional way (repository).

## References

[Basic Linux -- How to Compile Software Yourself (on Ubuntu)](https://www.youtube.com/watch?v=uRQ4QBegur8)

[error: --with-readline=yes (default) and headers/libs are not available](https://stackoverflow.com/questions/17473547/error-with-readline-yes-default-and-headers-libs-are-not-available)

[Linux make command](https://www.computerhope.com/unix/umake.htm)