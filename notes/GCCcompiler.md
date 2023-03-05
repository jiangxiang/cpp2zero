### g++编译运行C++代码过程


#### 第一步：打开命令行并进入路径


windows平台下，按住win+r键弹出对话框，输入cmd，即可打开命令行；

之后，通过以下代码进入待编译文件的文件夹

```
cd path/to/folder
```
其中，`path/to/file`是指待编译文件的文件夹路径


#### 第二步：生成预处理文件

```
g++ -E input.cpp -o output.i

``` 

#### 第三步：预处理文件至汇编文件

```
g++ -S output.i -o output.s
```

#### 第四步：


```
g++ -c output.s -o output.o
```

#### 第五步：

```
g++ output.o -o output.exe

```

至此，已经完成了编译。

最后，可以直接运行结果如下：
```
output.exe
```