# build qt4 project with qmake from qt5

copy needed feature files from qt5 to your qt4 project
```shell
$ cp /usr/lib64/qt5/mkspecs/features/spec_post.prf \
     /usr/lib64/qt5/mkspecs/features/spec_pre.prf  \
     .
```

create local mkspec files from default and modify it for qmake-qt5
```shell
$ LOCAL_MKSPEC=$(pwd)/local-mkspec/
$ LOCAL_QMAKE_CONF=${LOCAL_MKSPEC}/qmake.conf

$ mkdir $LOCAL_MKSPEC
$ cp  -r /usr/lib64/qt4/mkspecs/default/* $LOCAL_MKSPEC
$ sed -i 's,../common,/usr/lib64/qt4/mkspecs/common,g' $LOCAL_QMAKE_CONF
$ echo QMAKE_COMPILER=g++ >> $LOCAL_QMAKE_CONF
```

create local link to qmake-qt5 to use local qt.conf
```shell
$ ln -s /bin/qmake-qt5 qmake-qt5
```

configure right path for qt4 build
```shell
$ cat > qt.conf << EOF
[Paths]
Prefix=/usr/lib64/qt4/
Headers=/usr/include/
EOF
```


setup env var to qt4 specific mkspecs and features files
```shell
$ export QMAKESPEC=$LOCAL_MKSPEC
$ export QMAKEFEATURES=$(pwd)
```


and now run local qmake-qt5 to build the qt4 project
```shell
$ ./qmake-qt5
$ make
```
