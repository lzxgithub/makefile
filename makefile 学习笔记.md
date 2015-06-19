# makefile
工程管理文件
1、什么是makefile有什么用？
   当我们使用make命令执行某文件时就要需要有一个东西去指导它应该需要怎样去编译和链接程序。
这是makefile就发挥他的作用了，当使用make命令的时候，编译器会自动去当前目录下查找makefile
文件，有时候如果我们建的是一个的工程的话会在每个模块的目录下也放置makefile文件，这样既能减少
复杂的工作量。
2、makefile遵循的几条原则.
答：1）如果这个工程没有编译过，那么我们的所有 C 文件都要编译并被链接。
       2）如果这个工程的某几个 C 文件被修改，那么我们只编译被修改的 C 文件，并链接目
标程序。
    3）如果这个工程的头文件被改变了，那么我们需要编译引用了这几个头文件的 C 文件，
并链接目标程序。
3、它是怎么工作的？
    1、 make 会在当前目录下找名字叫“Makefile”或“makefile”的文件。
    2、如果找到，它会找文件中的第一个目标文件（ target），在上面的例子中，他会找到“edit”
这个文件，并把这个文件作为最终的目标文件。
  3、如果 edit 文件不存在，或是 edit 所依赖的后面的 .o 文件的文件修改时间要比 edit
这个文件新，那么，他就会执行后面所定义的命令来生成 edit 这个文件。
  4、如果 edit 所依赖的.o 文件也存在，那么 make 会在当前文件中找目标为.o 文件的依
赖性，如果找到则再根据那一个规则生成.o 文件。（这有点像一个堆栈的过程）
  5、当然，你的 C 文件和 H 文件是存在的啦，于是 make 会生成 .o 文件，然后再用 .o
文件生命 make 的终极任务，也就是执行文件 edit 了。

注意点：在编写makefile时要注意将以Tab开头。

4、简单的makefile小demo：
1）假设编译器是gcc, 代码结构很简单, 源文件有main.c, test.h, test.c, 代码为:
/*file:test.h*/
#ifndef TEST_H
#define TEST_H
void func();
#endif

/*file:test.c*/
#include "test.h"
void func() {
  /*some codes*/
}

/*file:main.c*/
#include "test.h"
int main() {
  func();
  return 0;
}
makefile内容如下:
objects= main.o test.o
demo:$(objects)
	gcc -o demo $(objects)
main.o:main.c test.h
	gcc -c -o main.o main.c
test.o:test.c test.h
	gcc -c -o test.o test.c
.PHONY:clean
clean:
	rm -f *.o demo
生成demo可执行文件的命令为: make 或 make demo
2）较大项目的makefile（小技巧）
较大项目的源文件很多, 为每个.o文件编写生成的规则, 会非常的麻烦, 将来新增或修改源文件的同时也要小心地维护makefile, 这样就带来了很多的不便. 利用make程序的自动推导功能和模式规则, 可以批量生成.o文件.
第一个例子的makefile可以改写为:

objects=main.o test.o
CC=gcc
CFLAGS=-Iadd -Isub -O2
%o:%.c
        $(CC) -c $< -o $@ $(CFLAGS)
demo:$(objects)
        gcc -o $@ $(objects)
.PHONY:clean
clean:
        rm -f *.o demo
objects= main.o test.o
%.o:%.c
	gcc -c $< -o $@
demo:$(objects)
	gcc -o $@ $(objects)
.PHONY:clean
clean:
	rm -f *.o demo
执行命令: make demo

http://www.xuebuyuan.com/527716.html

一个小工程带你学会makefile工程管理:http://www.tuicool.com/articles/RR3Mvib
 http://blog.sina.com.cn/s/blog_73d4d5fa0100paiy.html

