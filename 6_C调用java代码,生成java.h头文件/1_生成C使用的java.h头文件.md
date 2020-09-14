**需要命令  `javah`  来生成 c使用的 .h 头文件**



**生成java.h 头文件步骤:**

- **文件目录结构如下:**
  - **`src/com/app/mm/MainActivity.java`**
    - 只存在一个文件
- **终端进入 src 目录**
- **执行命令如下**

```bash
$ javah -jni com.example.myapplication.MainActivity

#即可生成  com_example_myapplication_MainActivity.h  头文件
```







