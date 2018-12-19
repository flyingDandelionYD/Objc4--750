#配置运行objc4-750和使用
背景：
由于要看runtime的底层，自己下载了[官方的源码](https://opensource.apple.com/tarballs/objc4/)(objc4-750.tar.gz)，结果编译各种报错。😭😭😭

---
下面是本人搭建的过程:  
参考链接：  
[1.最新Runtime源码objc4-750编译](http://www.imooc.com/article/268031?block_id=tuijian_wz)  
[2.objc - 编译Runtime源码objc4-680](https://blog.csdn.net/wotors/article/details/52489464)

- 准备工作
- 下载[objc4-750.tar.gz](https://opensource.apple.com/tarballs/objc4/)
- 其他所需要的库([下载地址点我](https://opensource.apple.com/tarballs/))
  ![](http://www.flyingdandelion.top/BAsserts/objc4-33.jpeg)
- 将所有下载的压缩包全部解压到一个文件夹（暂且定为libs，可以直接放在桌面哟）里面（**待会儿用搜索来查找缺失的文件**）
- Xcode打开<i>**objc.xcodeproj**</i>工程文件
- 编译Objc
	- **问题1**![](http://www.flyingdandelion.top/BAsserts/objc4-1.jpg)
		- 解决 ![](http://www.flyingdandelion.top/BAsserts/objc4-2.jpg)
	- **问题2**![](http://www.flyingdandelion.top/BAsserts/objc4-3.jpeg)
		- 解决 首先在工程的根目录下创建**include**文件夹，再在该文件夹下创建一个**sys**文件夹（因为报错的就是**#include&lt;sys/reason.h&gt;**）
		![](http://www.flyingdandelion.top/BAsserts/objc4-4.jpeg)
		- 在libs文件夹里面搜索**reason.h**，将其拖入**sys**文件夹里面
		- 在工程文件里面进行配置:（$(SRCROOT)/include）![](http://www.flyingdandelion.top/BAsserts/objc4-5.jpeg)
	- **问题3**![](http://www.flyingdandelion.top/BAsserts/objc4-6.jpeg)
	    - 解决 类似问题2，创建文件夹、搜索文件、拖入文件![](http://www.flyingdandelion.top/BAsserts/objc4-7.jpeg)
	- **问题4**![](http://www.flyingdandelion.top/BAsserts/objc4-8.jpeg) （类似问题2）
	- **问题5**![](http://www.flyingdandelion.top/BAsserts/objc4-9.jpeg) （类似问题2）
	- **问题6**![](http://www.flyingdandelion.top/BAsserts/objc4-10.jpeg)（类似问题2）
	- **问题7**![](http://www.flyingdandelion.top/BAsserts/objc4-11.jpeg) 删除了**bridgeos(3.0)**
	- **问题8**![](http://www.flyingdandelion.top/BAsserts/objc4-12.jpeg) 注意是**machine/cpu_capabilities.h**
 （类似问题2）
    - **问题9**![](http://www.flyingdandelion.top/BAsserts/objc4-13.jpeg) 注意是**os/tsd.h**（类似问题2）
    - **问题10**![](http://www.flyingdandelion.top/BAsserts/objc4-14.jpeg)（类似问题2）
    - **问题11**![](http://www.flyingdandelion.top/BAsserts/objc4-15.jpeg)![](http://www.flyingdandelion.top/BAsserts/objc4-16.jpeg)
     	- 解决  出现了重复定义了 这里我是把pthread_machdep.h文件里面的给注释掉的(详见demo)
    - **问题10**![](http://www.flyingdandelion.top/BAsserts/objc4-17.jpeg) 注意是**BlocksRuntime/Block_private.h**（类似问题2）
    - **问题11**![](http://www.flyingdandelion.top/BAsserts/objc4-18.jpeg)（类似问题2）
    - **问题12**![](http://www.flyingdandelion.top/BAsserts/objc4-19.jpeg)（类似问题2）
    	- 解决在 dyld_priv.h 文件中：添加
    	
        ```
        #define DYLD_MACOSX_VERSION_10_11 0x000A0B00
        #define DYLD_MACOSX_VERSION_10_12 0x000A0C00
        #define DYLD_MACOSX_VERSION_10_13 0x000A0D00
        #define DYLD_MACOSX_VERSION_10_14 0x000A0E00
        ```
    - **问题13**![](http://www.flyingdandelion.top/BAsserts/objc4-20.jpeg)
      - 解决(类似问题2:文件路径见下)![](http://www.flyingdandelion.top/BAsserts/objc4-21.jpeg)
    - **问题14**![](http://www.flyingdandelion.top/BAsserts/objc4-22.jpeg)注意是**private/_simple.h**（类似问题2）
    - **问题15**![](http://www.flyingdandelion.top/BAsserts/objc4-23.jpeg)
      - 解决：改一下工程配置文件: 
Build Settings->Preprocessor Macros（Debug & Release）加入:LIBC_NO_LIBCRASHREPORTERCLIENT
		![](http://www.flyingdandelion.top/BAsserts/objc4-35.jpeg)
    - **问题16**![](http://www.flyingdandelion.top/BAsserts/objc4-24.jpeg)
      - 解决 改一下工程配置: Build Settings->Linking->Order File，改成**$(SRCROOT)/libobjc.order**
        ![](http://www.flyingdandelion.top/BAsserts/objc4-25.jpeg)
    - **问题17**![](http://www.flyingdandelion.top/BAsserts/objc4-26.jpeg)
      - 解决：在 Build Settings -> Linking -> Other Linker Flags里删掉"-lCrashReporterClient"（Debug和Release都删了）
       ![](http://www.flyingdandelion.top/BAsserts/objc4-27.jpeg)
    - **问题18**![](http://www.flyingdandelion.top/BAsserts/objc4-28.jpeg)
       - 解决把Target-objc的Build Phases->Run Script(markgc)里的内容macosx.internal改为macosx
         ![](http://www.flyingdandelion.top/BAsserts/objc4-29.jpeg)
    - **问题19**![](http://www.flyingdandelion.top/BAsserts/objc4-30.jpeg)
       - 解决把Target-objc的Build Settings->Other Text-Based InstallAPI Flags里的内容设为空,把Text-Based InstallAPI Verification Model里的值改为Errors Only
         ![](http://www.flyingdandelion.top/BAsserts/objc4-31.jpeg)
         ![](http://www.flyingdandelion.top/BAsserts/objc4-32.jpeg)
- 总的路径如下
   ![](http://www.flyingdandelion.top/BAsserts/objc4-34.jpeg)
   
---

使用：  
![](http://www.flyingdandelion.top/BAsserts/objc4-36.jpeg)
![](http://www.flyingdandelion.top/BAsserts/objc4-37.jpeg)
我们在自己建的工程 main.m 文件里

```
#import <Foundation/Foundation.h>
#import <objc/runtime.h>
#import <malloc/malloc.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSObject *obj = [[NSObject alloc] init];
        
        //获得NSObject实例对象的成员变量所占用的大小 >> 8
        NSLog(@"%zd", class_getInstanceSize([NSObject class]));
        
        //获得obj指针所指向内存的大小 >> 16
        //malloc_size(const void *ptr):Returns size of given ptr
        NSLog(@"%zd", malloc_size((__bridge const void *)obj));
    }
    return 0;
}
```
测试：**激动人心的时刻**
![](http://www.flyingdandelion.top/BAsserts/objc4-38.jpeg)

