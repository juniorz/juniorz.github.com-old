---
layout: post
title: "How to build GStreamer from source"
date: 2011-12-10 09:09
comments: true
categories:
 - gstreamer
 - opensource
---

This post shows how to build and install [GStreamer](http://gstreamer.freedesktop.org/) from source. It targets building GStreamer for Mac OS X Lion. The procedure is quite similar if you want to build GStreamer for Linux.

The GStreamer is organized into [several modules](http://gstreamer.freedesktop.org/modules/). The required modules are **gstreamer**, **gst-plugins-base** and **gst-plugins-good** (if you want to do anything useful). This post will show how to build also **gst-plugins-bad** and **qt-gstreamer** modules.

### Pre-requisites

``` bash
brew update
brew install gettext pkg-config glib

#autopoint is missing on Lion
PATH="$PATH:`brew --prefix gettext`/bin/"

#Required libs provided by homebrew
brew install libiconv libffi
```

### Environment

``` bash
BUILD_ROOT=~/gstreamer-src
BUILD_TARGET=~/gstreamer

mkdir $BUILD_TARGET
mkdir $BUILD_ROOT && cd $BUILD_ROOT
```

### Getting the source

``` bash
git clone git://anongit.freedesktop.org/gstreamer/gstreamer
git clone git://anongit.freedesktop.org/gstreamer/gst-plugins-base
git clone git://anongit.freedesktop.org/gstreamer/gst-plugins-good
git clone git://anongit.freedesktop.org/gstreamer/gst-plugins-bad
git clone git://anongit.freedesktop.org/gstreamer/qt-gstreamer
```

## Building the base module (gstreamer)

``` bash
cd $BUILD_ROOT/gstreamer

GST_CONFIG_PATH="`brew --prefix libiconv`/lib/pkgconfig:`brew --prefix libffi`/lib/pkgconfig:`brew --prefix gettext`/lib/pkgconfig"

./autogen.sh --noconfigure
./configure --disable-gtk-doc --disable-debug --with-pkg-config-path=$GST_CONFIG_PATH --prefix=$BUILD_TARGET

make
make install

# Required
GSTP_CONFIG_PATH="$BUILD_TARGET/lib/pkgconfig"
```

## Building the plugin base module (gst-plugins-base)

``` bash
# liborc support
# http://code.entropywave.com/projects/orc/
brew install orc

# Vorbis support
brew install libvorbis

cd "$BUILD_ROOT/gst-plugins-base"
./autogen.sh --noconfigure
./configure --disable-gtk-doc --disable-debug --with-pkg-config-path=$GSTP_CONFIG_PATH --prefix=$BUILD_TARGET

make
make install
```

## Building the "good" plugins (gst-plugins-good)

``` bash
cd "$BUILD_ROOT/gst-plugins-good"
./autogen.sh --noconfigure
./configure --disable-gtk-doc --disable-debug --disable-goom --disable-libpng --with-pkg-config-path=$GSTP_CONFIG_PATH --prefix=$BUILD_TARGET

make
make install
```

## Building the "bad" plugind (gst-plugins-bad)

**Note**: *osxvideosrc* and *qtwrapper* are disabled because they use deprecated APIs ([more info](http://code.google.com/p/ossbuild/wiki/MacBuild)).

``` bash
# dependencies
brew install rtmpdump libvpx libmms faac faad2

cd "$BUILD_ROOT/gst-plugins-bad"
./autogen.sh --noconfigure
./configure --disable-gtk-doc --disable-debug --disable-apexsink --with-pkg-config-path=$GSTP_CONFIG_PATH --prefix=$BUILD_TARGET

make
make install
```

## QtGStreamer

Deveria instalar o GCC ais recente, segundo o README

Install QtSDK from Nokia site

    brew install cmake

    #this will not install qt bottle
    brew install automoc4 --ingnore-dependencies --build-from-source

    #had problems with boost 1.48
    cd /usr/local/Library/Formula/
    git checkout 57665ff /usr/local/Library/Formula/boost.rb

    brew install boost

    cd "$BUILD_ROOT/qt-gstreamer"

    export PKG_CONFIG_PATH=$BUILD_TARGET/lib/pkgconfig
    export LDFLAGS="-L`brew --prefix gettext`/lib/"

    #pra usar o Qt SDK
    export PKG_CONFIG_PATH="$PKG_CONFIG_PATH:/Users/juniorz/QtSDK/Desktop/Qt/474/gcc/lib/pkgconfig/"
    export PATH="/Users/juniorz/QtSDK/Desktop/Qt/474/gcc/bin/:$PATH"


    mkdir build && cd build

    #usei -DQTGSTREAMER_STATIC=ON -DQTGSTREAMER_EXAMPLES=ON
    cmake .. -DCMAKE_INSTALL_PREFIX=$BUILD_TARGET/qt

    # it will show a lot of warnings like
    # #warning "This version of Mac OS X is unsupported"
    make
    make install


### Exemplos

Vc pode usar o exemplo fornecido pelo qt-gstreamer, mas tem que alterar seu projeto, para compilar no QtCreator


1) Altere o .pro

No QtCreator
2) Clique em Projects, Targets, Build
Em Build Environment
Set PKG_CONFIG_PATH to /Users/juniorz/QtSDK/Desktop/Qt/474/gcc/lib/pkgconfig:/Users/juniorz/qt-gstreamer/lib/pkgconfig

3) CLique em Projects, Targets, Run
Em Run Environment
Set DYLD_LIBRARY_PATH to /Users/juniorz/qt-gstreamer/lib



Ler: 
http://doc.trolltech.com/4.5/qmake-project-files.html#declaring-other-libraries
http://doc.qt.nokia.com/4.7/qmake-project-files.html