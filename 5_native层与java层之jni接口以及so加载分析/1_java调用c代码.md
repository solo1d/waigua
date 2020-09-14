> **通过 `ndk-build` 可以将 c/c++  代码直接编译成库文件, 由java调用, 并在android上运行**

**纯java代码是使用SDK进行开发的,   c/c++ 是使用 NDK 进行开发的**



**C/C++的代码就是 native 层**

**JNI是链接 C/C++(native层) 与 jva 层的 接口(interface)**

- **C/C++ 通过 JNI提供的接口去 访问 java**
- **java 通过 JNI提供的接口去 访问 C/C++**





