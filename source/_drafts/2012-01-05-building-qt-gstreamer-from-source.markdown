---
layout: post
title: "How to build GStreamer from source"
date: 2011-12-10 09:09
comments: true
categories:
 - gstreamer
 - opensource
---

## QtGStreamer

Também deveria compilar o Qt a partir do código fonte.

Deveria instalar o GCC mais recente, segundo o README

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