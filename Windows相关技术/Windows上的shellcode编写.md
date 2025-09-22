# 介绍
引用自[Windows下Shellcode编写详解-先知社区](https://xz.aliyun.com/news/1808)

我们编写Shellcode不是只为了在本机上运行，而是要通用于任何机器。所以，我们需要不依赖外部查找函数地址，那么，我们需要一段代码能够自己定位任意函数地址。

我们要调用一个函数，必须要知道其地址，而我们在调用函数时又必须要载入链接库，那么我们就必须要知道LoadLibrary()函数地址，获取地址需要函数GetProcAddress()，而GetProcAddress()函数在“kernel32.dll”的里面。所以，我们在寻找地址时，需要用到这么几个关键字“kernel32.dll”、”GetProcAddress()”、”LoadLibrary()”。

正如我们在前面讲的的那样，为了生成可靠的shellcode代码，我们需要遵循一些步骤。我们知道要调用什么函数，但是首先，我们必须找到这些函数，在前面已经讨论了怎么调用函数地址的步骤。

必要的步骤如下:

> 1.找到kernel32.dll被加载到内存中  
> 2.找到其导出表  
> 3.找到由kernel32.dll导出的GetProcAddress函数  
> 4.使用GetProcAddress查找LoadLibrary函数的地址  
> 5.使用LoadLibrary来加载动态链接库  
> 6.在动态链接库中找到函数的地址  
> 7.调用函数  
> 8.查找ExitProcess函数的地址  
> 9.调用ExitProcess函数

以上就是一个完整的Shellcode编写过程，具体为什么要这么写，网上也有许多的资料。这里主要是利用PEB结构来查找关键dll文件的，这个和PELoader有关系，这里就不再详细介绍了。我们还是用最开始的那个例子，system("dir")。

# PEB结构体
![](https://img2022.cnblogs.com/blog/1586953/202203/1586953-20220301144150999-1354806358.png)

PEB结构体的Ldr会指向一个_PEB_LDR_DATA的结构体,这个结构体里面存在三个循环链表,每一个列表的节点都是一个结构体,其中储存了当前进程所需要的DLL文件的相关信息.

知道了这个消息,我们就可以通过遍历链表的方式来寻找我们所需要的DLL文件,或者说kernel32.dll

## 代码实现
首先我们先定义三个函数指针,来储存我们需要的函数

```c
typedef void* (WINAPI* FnGetProcAddress)(HMOUDULE, const char*);
typedef void* (WINAPI* FnLoadLibarary)(char*);
typedef void* (WINAPI* FnVirtualProtect)(LPVOID, SIZE_T, DWORD, PWORD);

FnGetProcAddress MyGetProcAddress;
FnLoadLibraryA MyLoadLibraryA;
FnVirtualProtect MyVirtualProtect;
```

接下来通过内联汇编的方式,通过PEB结构体从进程中获得我们需要的函数地址

```c
void GetApis()
{
	HMODULE hKernel32;

	_asm
	{
		pushad;
		//获取kernel32.dll的加载基址
		
		/*
		找到PEB的首地址
		*/
		
		mov eax, fs: [0x30];	// eax = peb
		mov eax, [eax + 0ch];	// eax = PEB_LDR_DATA
		mov eax, [eax + 0ch];	// eax = Ldr->InMemoryOrderModuleList

		mov eax, [eax];			// eax = ntdll.dll
		mov eax, [eax];			// eax = kernel32.dll

		/*
		将搜索到的kernel32.dll的基地址存入变量中
		*/
		
		mov eax, [eax + 018h];	// eax = Base Address
		mov hKernel32, eax;		// hKernel32 = Base Address

		mov ebx, [eax + 03ch];	// ebx = DOS->e_lfanew
		add ebx, eax;			// ebx = PE Header
		add ebx, 078h;			// ebx = IMAGE_EXPORT_DIRECTORY
		mov ebx, [ebx];			// ebx = Offset Export Table
		add ebx, eax;			// ebx = Export Table

		/*
		准备遍历导出表,查找GetProcAddress函数
		*/

		lea ecx, [ebx + 020h];	// ecx -> 导出表RVA 
		mov ecx, [ecx];			// ecx = 导出表RVA
		add ecx, eax;			// ecx => 名称表的首地址(va);
		xor edx, edx;			// 作为index来使用.

	_WHILE:;

		mov esi, [ecx + edx * 4];			// esi = Names Table + edx * 4
		lea esi, [esi + eax];				// esi -> names

		cmp dword ptr[esi], 050746547h;		// GetP
		jne _LOOP;
		cmp dword ptr[esi + 4], 041636f72h; // rocA
		jne _LOOP;
		cmp dword ptr[esi + 8], 065726464h; // ddre
		jne _LOOP;
		cmp word  ptr[esi + 0ch], 07373h;	// ss
		jne _LOOP;
		
		mov edi, [ebx + 024h];		// edi = Offset Name Ordinaries			
		add edi, eax;				// edi = Name Ordinaries

		mov di, [edi + edx * 2];	// di = edi + edx * 2
		and edi, 0FFFFh;			// 保留word

		mov edx, [ebx + 01ch];		// edx = 导出函数地址表RVA
		add edx, eax;				// edx = 导出函数地址表

		mov edi, [edx + edi * 4];	// edi = Offset GetProcAdrress
		add edi, eax; ;				// edi = GetProcAddress

		mov MyGetProcAddress, edi;

		jmp _ENDWHILE;

	_LOOP:;
		
		inc edx; // ++index;
		jmp _WHILE;

	_ENDWHILE:;
		popad;
	}
	
	MyGetProcAddress = (FnGetProcAddress)MyGetProcAddress(
	hKernel32,
	"LoadLibararyA");
	MyVirtualProtect = (FnVirtualProtect)MyGetProcAddress(
	hKernel32,
	"VirtualProtect");
	// 通过得到的GetProcAddress函数获得我们需要的函数地址
	
}	
```

