# makefile



## VERSION 1

```makefile
targetFileName: file1.cpp file2.cpp file3.cpp
	g++ -o targetFilename file1.cpp file2.cpp file3.cpp
```



## VERSION 2

```makefile
COMPILER = g++ # 或者gcc等编译器名字，左边命名一般CXX，也可以其他
TARGET = targetFileName
OBJ = file1.o file2.o file3.o

$(TARGET): $(OBJ) # 说明这个TARGET变量是依赖与OBJ这些变量
	$(COMPILER) -O $(TARGET) $(OBJ)

file1.o: file1.cpp
	$(COMPILER) -c file1.cpp

file2.o: file2.cpp
	$(COMPILER) -c file2.cpp

file3.o: file3.cpp
	$(COMPILER) -c file3.cpp
```



## VERSION 3

```makefile
COMPILER = g++ # 或者gcc等编译器名字，左边命名一般CXX，也可以其他
TARGET = targetFileName
OBJ = file1.o file2.o file3.o

COMPILERFLAGS = -c -Wall # Wall可以提示小的警告

$(TARGET): $(OBJ) # 说明这个TARGET变量是依赖与OBJ这些变量
	$(COMPILER) -O $@ $^ # $@指的是TARGET $^指的是上面那些所有依赖，如OBJ

%.o: %.cpp # %是匹配符
	$(COMPILER) $(COMPILERFLAGS) $< -o $@ # 这里$<指的是上面依赖的第一个，这里是%.cpp

.PHONY: clean # 隐藏文件防止clean文件冲突
clean:
	rm -f *.o *(TARGET)
```



## VERSION 4

```makefile
COMPILER = g++ # 或者gcc等编译器名字，左边命名一般CXX，也可以其他
TARGET = targetFileName
SRC = $(wildcard *.cpp) # 这里是把当前目录下所有的.cpp扩展名文件放在了SRC里面
OBJ = $(patsubst %.cpp, %.o, $(SRC)) # 这里patsubst的功能是把SRC里面的%.cpp替换成%.o 

COMPILERFLAGS = -c -Wall # Wall可以提示小的警告

$(TARGET): $(OBJ) # 说明这个TARGET变量是依赖与OBJ这些变量
	$(COMPILER) -O $@ $^ # $@指的是TARGET $^指的是上面那些所有依赖，如OBJ

%.o: %.cpp # %是匹配符
	$(COMPILER) $(COMPILERFLAGS) $< -o $@ # 这里$<指的是上面依赖的第一个，这里是%.cpp

.PHONY: clean # 隐藏文件防止clean文件冲突
clean:
	rm -f *.o *(TARGET)
```

