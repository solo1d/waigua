使用 android killer 进行反汇编



## 目录结构

- `my Application`
  - **lib**   存放.so的动态库目录
  - original
  - **res ** 资源目录, 界面绘制, 图标, 字符串等等的内容,都在这里
    - **layout**  界面布局
      - activity_main.xml
    - **values**  里面存放了字符串, 颜色定义 等内容
      - string.xml
      - colors.xml
  - smali   安卓源代码反汇编也可能会出现在这里
  - **smali_classes2**  安卓源代码反汇编目录
    - androidx
    - **com**    安卓pack 包名  `com.examlpe.myapplication`
      - **examlpe**
        - **myapplication**
          - 这里面存放的都是源代码 java文件的反汇编 MainActivity.smali
  - **AndroidManifest.xml**  安卓的主要配置文件
  - apktool.myl