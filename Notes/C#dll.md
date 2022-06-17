# 内存对齐规则

1. 结构体的数据成员，第一个成员偏移量为0，垢面每个数据成员存储的起始位置要从自己大小的整数倍开始。
2. 子结构体的第一个成员偏移量应当是子结构体中最大成员的整数倍。
3. 结构体总大小必须是其内部最大成员的整数倍。

查看结构体偏移量函数：

```c++
#define FIELDOFFSET(TYPE, MEMBER) (int)(&(((TYPE*)0)->MEMBER))
```

示例：

```c++
struct Info
{
    char username[10]; // 0 - 9
    double userdata; // 16 - 24
}
// sizeof(Info) = 24
struct Frame
{
    unsigned char id; // 0 - 1
    int width; // 4 - 8
    long long height; // 8 - 16
    unsigned char *data; // 16 - 20
    Info info; // 24 - 34 40 -48
}
// sizeof(Frame) = 48
```

如果使用一字节对齐

```c++
#pragma pack(push)
#pragma pack(1)
struct Info
{
    char username[10]; // 0 - 9
    double userdata; // 10 - 18
}
#pragma pack(pop)
sizeof(Info) = 18
struct Frame
{
    unsigned char id; // 0 - 1
    int width; // 4 - 8
    long long height; // 8 - 16
    unsigned char *data; // 16 - 20
    Info info; // 24 - 34 40 -48
}
sizeof(Frame) = 48
```

# 调用约定

- _cdecl称之为C调用约定，参数按照从右至左的方式入栈，函数本身不清理栈，此工作由调用者负责，所以允许可变参数函数存在。
- _stdcall称之为标准调用约定，参数按照从右至左的方式入栈，函数本身清理栈。

示例：

```c++
int add(int num1, int num2)
{
    return num1 + num2;
}
int main
{
    add(1,2);
    return 0;
}
```

右键项目选择

​		属性 >> C/C++ >> 高级 >> 调用约定

可查看默认调用约定

# C#结构体

示例：

```csharp
public struct TestStruct
{
    public byte id; // 0 - 1
    public int width; // 4 - 8
    public long height; // 8 - 16
    public int num1; // 16 - 20
}
/*
	TestStruct ts = new TestStruct();
	int len = Marshal.SizeOf(ts); // len = 24
*/
```

```csharp
[StructLayout(LayoutKind.Sequential, Pack = 1)]
// Pack = 1 表示采用1字节对齐方式
public struct TestStruct
{
    public byte id; // 0 - 1
    public int width; // 1 - 5
    public long height; // 5 - 13
    public int num1; // 13 - 17
}
/*
	TestStruct ts = new TestStruct();
	int len = Marshal.SizeOf(ts); // len = 17
*/
```

```csharp
[StructLayout(LayoutKind.Explicit, Pack = 1)]
// 手动设置偏移量
public struct TestStruct
{
    [FieldOffset(0)]
    public byte id; // 0 - 1
    [FieldOffset(10)]
    public int width; // 10 - 14
    [FieldOffset(15)]
    public long height; // 15 - 19
    [FieldOffset(40)]
    public int num1; // 40 - 44
}
/*
	TestStruct ts = new TestStruct();
	int len = Marshal.SizeOf(ts); // len = 48
*/
```

# C#与C/C++之间类型的对应关系

| C#         | C/C++             |
| ---------- | ----------------- |
| ubyte      | char              |
| byte       | unsigned char     |
| short      | short             |
| int        | int32_t           |
| long       | int64_t           |
| float      | float             |
| double     | double            |
| IntPtr, [] | void * (任意指针) |



- # 创建并调用动态库

1. 创建工程文件夹project，在之中新建 src 和 bin

2. 创建一个C#项目（下称项目1）， 目录为 project/src

3. 右键解决方案，创建一个C++空项目（下称项目2），目录为 project/src

4. 右键解决方案，创建一个动态链接库（DLL）项目（下称项目3），目录为 project/src

5. 右键项目3，选择属性 >> C/C++ >> 预编译头 >> 预编译头 >> 不使用预编译头

6. 在项目3中新建头文件 Native.h 和源文件 Native.cpp , 删除项目中的其他文件

   ```c++
   // Native.h
   #pragma once
   ```

   ```c++
   // Native.cpp
   #include "Native.h"
   ```

7. 右键项目3，选择属性 配置改为所有配置，平台设为 x64

8. 右键项目3，选择属性 >> 常规 >> 输出目录 设置为 ../../bin

9. 右键项目2，选择属性 >> 常规 >> 输出目录 设置为 ../../bin

10. 右键项目2，选择属性 >> 调试 >> 工作目录 设置为 ../../bin

11. 项目1新建 main.cpp文件

    ```c++
    // main.cpp
    #include <iostream>
    
    int main(int argc, char* argv[])
    {
        return 0;
    }
    ```

12. 右键项目1，选择属性 >> 生成 首选32位的勾去掉，平台目标选择 x64, 输出路径改为 ../../bin

13. 右键项目1，生成解决方案

14. 右键解决方案，选择属性 >> 通用属性 >> 项目依赖项 

    C#项目依赖于动态链接库项目， C++项目依赖于动态链接库项目

15. 右键解决方案，选择重新生成解决方案

注意事项：

- 项目编辑完成后右键项目重新生成

## 项目3 文件

```c++
// Native.h
#pragma once

#ifdef __cplusplus
#define EXTERNC extern "C"
#else
#define EXTERNC
#endif

#ifdef DLL_IMPORT
#define HEAD EXTERNC __declspec(dllimport)
#else
#define HEAD EXTERNC __declspec(dllexport)
#endif
#define CallingConvention _cdecl

HEAD void CallingConvention Test1();

HEAD void CallingConvention TestLogA(const char *log);

//HEAD void CallingConvention TestLog(const char *log);

HEAD void CallingConvention Test_BasicData(char d1, short d2, int d3, long long d4, float d5, double d6);

HEAD void CallingConvention Test_BasicDataRef(char &d1, short &d2, int &d3, long long &d4, float &d5, double &d6);

HEAD void CallingConvention Test_BasicDataPointer(char *d1, short *d2, int *d3, long long *d4, float *d5, double *d6);

HEAD float CallingConvention Test_Add(float num1, float num2);

HEAD void CallingConvention Test_BasicDataArr(int *arr1, float *arr2);

HEAD void* CallingConvention Test_BasicDataRet();

HEAD void CallingConvention Test_BasicDataString(char *str);

HEAD void CallingConvention Test_BasicDataByteArr(char *str);


struct ChildStruct
{
	int num;
	double pi;
};
struct StructA
{
	short id;
	ChildStruct cs;
	ChildStruct *pcs;
	int nums[5];
};
HEAD void CallingConvention Test_Struct(StructA param);

HEAD void* CallingConvention Test_StructRet();

HEAD void* CallingConvention ConvertChildStruct(ChildStruct *cs);

HEAD float CallingConvention Sum(int length, ...);

//int Log(int level, IntPtr ptr)
typedef int(*pfun)(int level, void* ptr);

//void SetLogFuncPointer(Log logptr);

HEAD void CallingConvention SetLogFuncPointer(pfun p);
```

```c++
// Native.cpp

#include "Native.h"
#include <iostream>
#include <windows.h>
HEAD void CallingConvention Test1()
{
	SetLastError(2);
	printf("call success\n");
}

HEAD void CallingConvention TestLogA(const char *log)
{
	printf("logA:%s\n", log);
}

HEAD void CallingConvention Test_BasicData(char d1, short d2, int d3, long long d4, float d5, double d6)
{

}

HEAD void CallingConvention Test_BasicDataRef(char &d1, short &d2, int &d3, long long &d4, float &d5, double &d6)
{
	d1 = 1;
	d2 = 2;
	d3 = 3;
	d4 = 4;
	d5 = 5.5f;
	d6 = 6.6;
}

HEAD void CallingConvention Test_BasicDataPointer(char *d1, short *d2, int *d3, long long *d4, float *d5, double *d6)
{
	*d1 = 10;
	*d2 = 20;
	*d3 = 30;
	*d4 = 40;
	*d5 = 15.5f;
	*d6 = 16.6;
}

HEAD float CallingConvention Test_Add(float num1, float num2)
{
	return num1 + num2;
}

HEAD void CallingConvention Test_BasicDataArr(int *arr1, float *arr2)
{
	arr1[0] = 11;
	arr1[1] = 22;
	arr1[2] = 33;

	arr2[0] = 22.2f;
	arr2[1] = 22.2f;
	arr2[2] = 22.2f;
	arr2[3] = 22.2f;
}

int intarr[5] = {1,2,3,4,5};
HEAD void* CallingConvention Test_BasicDataRet()
{
	return intarr;
}

HEAD void CallingConvention Test_BasicDataString(char *str)
{
	printf("%s\n", str);
}

HEAD void CallingConvention Test_BasicDataByteArr(char *str)
{
	printf("%s\n", str);
}

HEAD void CallingConvention Test_Struct(StructA param)
{
	param.id = 123;
}

StructA structa;

struct FrameInfo
{
	char username[20];
	double pts;
};

struct Frame
{
	int width; //0-4
	int height;//4-8
	int format;//8-12
	int linesize[4]; //12-28
	unsigned char* data[4];//32-64
	FrameInfo *info; //64-72
};
Frame frame;
FrameInfo info;
HEAD void* CallingConvention Test_StructRet()
{
	frame.width = 1920;
	frame.height = 1080;
	frame.format = 0;
	for (int i = 0; i < 4; i++)
	{
		frame.linesize[i] = 100 * i;
		frame.data[i] = new unsigned char[10];
		for (int j = 0; j < 10; j++)
		{
			frame.data[i][j] = i;
		}
	}
	info.pts = 12.5;
	memset(info.username, 0, 20);
	memcpy(info.username, "hello world", strlen("hello world"));
	frame.info = &info;
	return &frame;
}

HEAD void* CallingConvention ConvertChildStruct(ChildStruct *cs)
{
	return cs;
}

HEAD float CallingConvention Sum(int length, ...)
{
	char* head = (char*)&length;
	int num1 = *(long long*)(head + 8);
	int num2 = *(long long*)(head + 16);
	int num3 = *(long long*)(head + 24);
	int num4 = *(long long*)(head + 32);
	double num5 = *(double*)(head + 40);

	return (num1 + num2 + num3 + num4 + num5) / length;
}

HEAD void CallingConvention SetLogFuncPointer(pfun p)
{
	int ret = p(0, NULL);
	printf("");
}

//HEAD void CallingConvention TestLog(const char *log)
//{
//	printf("log:%s\n", log);
//}

```

## 项目1 文件

```csharp
// Program.cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace CallNativeDllCSharp
{
    class Program
    {
        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl, SetLastError = false)]
        public static extern void Test1();

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl, CharSet = CharSet.Ansi, EntryPoint = "TestLog",ExactSpelling = false)]
        public static extern void TestLogNative(string log);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern void Test_BasicData(char d1, short d2, int d3, long d4, float d5, double d6);


        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern void Test_BasicDataRef(ref sbyte d1, ref short d2, ref int d3, ref long d4, ref float d5, ref double d6);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern void Test_BasicDataPointer(ref sbyte d1, ref short d2, ref int d3, ref long d4, ref float d5, ref double d6);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern float Test_Add(float num1, float num2);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern float Test_BasicDataArr(int[] arr1, float[] arr2);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern IntPtr Test_BasicDataRet();

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern void Test_BasicDataString(string str);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern void Test_BasicDataByteArr(byte[] str);

        [StructLayout(LayoutKind.Sequential)]
        public struct ChildStruct
        {
            public int num;
            public double pi;
        };
        [StructLayout(LayoutKind.Sequential)]
        public struct StructA
        {
            public short id;
            public ChildStruct cs;
            public IntPtr pcs;
            [MarshalAs(UnmanagedType.ByValArray, SizeConst =5)]
            public int[] nums;
        };

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern void Test_Struct(ref StructA param);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern IntPtr Test_StructRet();

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern IntPtr ConvertChildStruct(ref ChildStruct cs);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern float Sum(int length, __arglist);

        [UnmanagedFunctionPointer(CallingConvention.Cdecl)]
        public delegate int Log(int level, IntPtr ptr);

        [DllImport("NativeDll.dll", CallingConvention = CallingConvention.Cdecl)]
        public static extern void SetLogFuncPointer(Log logptr);

        public static Log logfuncptr = LogCallback;

        static int LogCallback(int level, IntPtr ptr)
        {

            return 0;
        }
        static void Main(string[] args)
        {

            SetLogFuncPointer(logfuncptr);
            Console.Read();
        }
    }
}
```

## 项目2文件

```c++
// main.cpp
#include <stdio.h>
#include <stdarg.h>
#define DLL_IMPORT
#include "../NativeDll/Native.h"
#pragma comment(lib, "../../bin/NativeDll.lib")

void test(int length, ...)
{
	va_list ap;
	va_start(ap, length);


	int num1 = va_arg(ap, int);
	int num2 = va_arg(ap, int);
	double num3 = va_arg(ap, double);
	va_end(ap);
}
int main(int argc, char* argv[])
{
	test(3, 123, 456, 12.3);
	return 0;
}
```

