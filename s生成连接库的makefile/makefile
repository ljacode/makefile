# @file makefile
# @brief 
# @author LJA
# @version 0.0.1
# @date 2012-11-11

# 注意: 该makefile执行过程中使用了名为tmp_make的临时文件，所以当前目录下不能存在名为tmp_make的文件

# 将当前目录下所有的.c文件编译成.o文件，并将其打包成动态库或者静态库
# 生成$(target).so : make dynamic
# 生成$(target).ar : make static

###########################
#
#  配置项
#
###########################
#目标文件
target = 
#源码文件后缀,只能指定一个
suffix=
###########################
#
#  以下内容一般情况下不需要修改
#
###########################
target := $(strip $(target))
ifeq ($(target),)
$(error target is not set)
endif
suffix := $(strip $(suffix))
ifeq ($(suffix),)
$(error suffix is not set!)
endif
ifneq ($(words $(suffix)),1)
$(error suffix is too many)
endif

libdep = $(patsubst %.$(suffix), %.o, $(wildcard *.c))
PHONY:=ALL
ALL:dep dynamic static

PHONY += dynamic
#将dep和$(libdep)分拆在两行是为了使dynmaic:$(libdep)下面的"$<"中只包含$(libdep)
dynamic: dep
dynamic: $(libdep)        
	gcc -shared $< -o $(target).so
static: dep
static:  $(libdep)
	ar rcvs $(target).a $< 

PHONY += clean
clean:
	rm *.o
PHONY += cleanall
cleanall:
	rm *.o *.so *.a

PHONY += dep
dep: 
	sed '/\#\#\#Dependencies/q' < makefile >tmp_make
	(for i in ./*.c; do echo -n; \
	gcc -MM $$i;echo "\tgcc -c -fpic $$< -o $$""@";done) >>tmp_make
	mv tmp_make makefile

###Dependencies
a.o: a.c
	gcc -c -fpic $< -o $@
b.o: b.c
	gcc -c -fpic $< -o $@
