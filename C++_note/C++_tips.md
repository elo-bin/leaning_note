# C++_tips
---
## 问题集
>[#include的作用是什么](##include的作用是什么？)
>[问题二](#问题二)
>[问题三](#问题三)
> ......
>[复习感悟](#复习感悟)
### #include的作用是什么？
#include发生在预处理阶段，整个编译链接过程。它会直接将头文件中的内容完完整整的包含进来。
例如：
```C++
    //file a.cpp start
    #include<iostream>
    int a=10;
    #include"b.h"
    //file a.cpp end

    //file b.h start
    int main(){
        cout<<a;
        return 0;
    }
    //file b.h end

    //include之后，源文件就变为了如下的状态
    #include<iostream>
    int a=10;
    int main(){
        cout<<a;
        return 0;
    }
```
### 问题二
问题二的解决方法（答案）
### 问题三
问题三的解决方法（答案）
### ...


### 复习感悟
复习时间
复习感悟