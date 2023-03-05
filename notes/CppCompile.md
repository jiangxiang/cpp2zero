# C++的编译过程举例

以下以hello world代码为例，简单描述一下编译过程。

其中内容不十分精确，但可帮助理解。


已知有如下的c++代码为

```
#include <iostream>

int main() {
  std::cout << "Hello World!" << std::endl;
  return 0;
}
```

首先，进行预编译，得到如下代码

```
# 1 "hello_world.cpp"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "hello_world.cpp"

#include <iostream>
using namespace std;

int main()
{
    cout << "Hello World!" << endl;
    return 0;
}
```

其中`#include <iostream>`被替换为了一段代码

* 第一行 `# 1 hello_world.cpp` 表示预处理器处理到了名为 `hello_world.cpp` 的源代码文件。
* 第二行 `# 1 <built-in>` 表示该行指令是内置的。
* 第三行 `# 1 <command-line>` 表示该行指令是从命令行中输入的。
* 第四行 `# 1 hello_world.cpp` 表示预处理器继续处理名为 "hello_world.cpp" 的源代码文件。

之后，上述预处理的代码被转化为汇编语言

```
    .file   "hello_world.cpp"
    .text
    .section    .rodata
.LC0:
    .string "Hello, world!"
    .text
    .globl  main
    .type   main, @function
main:
.LFB0:
    .cfi_startproc
    pushq   %rbp
    .cfi_def_cfa_offset 16
    .cfi_offset 6, -16
    movq    %rsp, %rbp
    .cfi_def_cfa_register 6
    movl    $0, %eax
    leaq    .LC0(%rip), %rdi
    call    puts@PLT
    movl    $0, %eax
    popq    %rbp
    .cfi_def_cfa 7, 8
    ret
    .cfi_endproc
.LFE0:
    .size   main, .-main
    .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
    .section    .note.GNU-stack,"",@progbits
```

其中，

* `.LC0` 标签表示存储了一个字符串 
* `"Hello, world!"`，`.globl main` 表示 `main` 函数是全局可见的，
* `.type main, @function` 表示 `main` 函数是一个函数类型，
* `.cfi_startproc` 和 `.cfi_endproc` 表示这是一个函数的起始和结束位置。

再后，上述汇编指令，根据机器内置的指令代码，将其转化为机器语言

```
00000000: 55                     push        ebp
00000001: 8B EC                  mov         ebp,esp
00000003: 6A 00                  push        0
00000005: 68 00 00 00 00         push        offset __stringlit_0
0000000A: E8 00 00 00 00         call        _printf
0000000F: 83 C4 08               add         esp,8
00000012: B8 00 00 00 00         mov         eax,0
00000017: C9                     leave
00000018: C3                     ret
__stringlit_0:
00000000: 48 65 6C 6C 6F 2C 20 77 6F 72 6C 64 21 00   "Hello, world!"

```
其中每个指令被编码成一串二进制序列，代表相应的操作码和操作数，最终形成二进制文件


最后，将多个目标文件链接成一个可执行文件。对于hello world程序，只需要将hello world.o文件和库文件libc.so进行连接即可。

其中，库文件中包含主程序中所涉及的操作的具体实现。例如，libc.so中就包含了输入输出函数。




