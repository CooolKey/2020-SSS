# KLEE

一款开源的自动软件测试工具，基于LLVM编译底层基础，能够自动生成测试样例检测软件缺陷

## 实验过程

**tutorial 1**

测试一个判断正负数的程序，打开前文提到的get_sign.c文件，可以看到测试函数get_sign和main函数。

```c++
#include <klee/klee.h>

int get_sign(int x) {
  if (x == 0)
     return 0;

  if (x < 0)
     return -1;
  else 
     return 1;
} 

int main() {
  int a;
  klee_make_symbolic(&a, sizeof(a), "a");
  return get_sign(a);
} 
```


其中klee_make_symbolic是KLEE工具自带的测试函数，通过自定义的变量，不断产生值赋给a，以此完成自动生成样例功能。

编译该c文件：

```shell
$ clang -I ../../include -emit-llvm -c -g get_sign.c
```

同目录下生成了一个get_sign.bc字节码文件，然后进行测试：

```shell
$ klee get_sign.bc
```

可以看到输出结果：

```
KLEE: output directory = "klee-out-0"

KLEE: done: total instructions = 31
KLEE: done: completed paths = 3
KLEE: done: generated tests = 3
```

列出当前目录所有文件：

```shell
$ ls get_sign.bc  get_sign.c  klee-last  klee-out-0
```

其中klee-out-0是本次测试结果，klee-last是最新测试结果，每次测试后覆盖。

```shell
$ ls klee-last/
assembly.ll      run.istats       test000002.ktest
info             run.stats        test000003.ktest
messages.txt     test000001.ktest warnings.txt
```

klee-last中包含最新测试的缺陷说明和测试样例等文件。

**测试实例**

在examples文件夹中新建一个mytest文件夹，编写一个带有软件缺陷的程序：

```c++
#include<stdio.h>
#include<stdlib.h>

void kleeTest(int a){
	int arr[10];
	int d[10];

	for (int i = 0; i < 10; i++){ //赋初始值
		arr[i] = i;
	}

	if (a < -50){  //求余分母为0
		for (int i = 0; i < 10; i++){
			int num = i;
			d[i] = arr[i] % num;
		}
	}
	else if(a < -25){  //除法分母为0
		for (int i = 0; i <= 10; i++){
			int num = i ;
			d[i] = arr[i] / num;
		}
	}
	else if (a < 0){  //数组越界
		for(int i = 0; i<= 11; i++){
			arr[i] = i;
		}
	}
	else if (a < 25){  //空指针
		int *a = NULL;
		int b = *a + 1;
	}
	else if(a < 50){  //内存泄漏
		free(arr);
	}
}

int main(){
	int n;
	klee_make_symbolic(&n, sizeof(n), "n");
	kleeTest(n);
	return 0;
}
```
在编写缺陷程序时，帖主尝试直接用vi编写，但是预装版的vi有输入问题，在删除之后尝试用sudo命令重装vi（文档有说明画像中有sudo命令），显示ip地址无法访问无法安装成功，估计原因是画像隔离无法联网。建议在外部编写之后复制粘贴到vi中直接保存。
