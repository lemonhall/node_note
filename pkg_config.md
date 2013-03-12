1、pkg-config --list-all

2、编译时找不到glib.h？
http://stackoverflow.com/questions/1146010/why-cant-i-build-a-hello-world-for-glib
gcc test.c -Wall -o test `pkg-config --cflags --libs glib-2.0`

3、pkg-config