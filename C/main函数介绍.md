### main函数入参
运行 C 程序，main(int argc, char *argv[]) 函数传参，argc 为参数个数，argv 是字符串数组， 下标从 0 开始，第一个存放的是可执行程序的文件名字，然后依次存放传入的参数，举个例子 HelloWorld.c ：

```
#include <stdio.h> 

int main(int argc, char *argv[])
{ 
    /* 我的第一个 C 程序 */ 
    printf("可执行程序 %s ,参数个数为[%d], 运行输出：[%s]\n",argv[0],argc,argv[1]); 
    return 0;
}
```

编译 gcc HelloWorld.c，得到可执行程序 a.out，运行程序：

```
./a.out Hello,World!
可执行程序 ./a.out ,参数个数为[2], 运行输出：[Hello,World!]
```

### main函数返回类型
当是 int main() 时，main() 的返回值是 int 类型，所以是 return 0; 现在 C 标准里规定 main() 返回值必须为 int，所以必须写成是 int main()。
```
#include <stdio.h>
int main(){
    /* 我的第一个 C 程序 */
    printf("Hello, World!\n");
    return 0;
}
```

当是 void main() 时，main() 的返回值是空，所以可以不写或者是 return; 但这是以前的写法了，现在很少用 void main() 了，也不推荐这么用。
```
#include <stdio.h>
void main(){
    /* 我的第一个 C 程序 */
    printf("Hello, World!\n");
    return ;
}
```
