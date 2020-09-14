- [框架](#框架)
- [注册方式](#注册方式)
  - [静态注册](#静态注册)
  - [动态注册](#动态注册)
- 



## 框架

![Android系统框架](5_native层与java层之jni接口/png/Android系统框架.jpg)

## 注册方式

### 静态注册

**安全性并不高, 应该使用动态注册**

```c++
#include <string.h>
#include <jni.h>

// 当前文件名为 native.cpp , 生成之后是 native-lib 文件
// 使用  Java_com_example_hellojni_HelloJni 的就是 静态注册
/* 
 *  加载这个文件的是  com/example/hellojni/ HelloJni.java   
 *  java 这样来进行注册:  
 * 			static { System.loadLibrary("native-lib");  
 *  		public native String stringFromJNI();
 */

extern "C" JNIEXPORT jstring JNICALL
Java_com_example_hellojni_HelloJni_stringFromJNI(
        JNIEnv* env,
        jobject /* this */) {

#if defined(__arm__)
  #if defined(__ARM_ARCH_7A__)
    #if defined(__ARM_NEON__)
      #if defined(__ARM_PCS_VFP)
        #define ABI "armeabi-v7a/NEON (hard-float)"
      #else
        #define ABI "armeabi-v7a/NEON"
      #endif
    #else
      #if defined(__ARM_PCS_VFP)
        #define ABI "armeabi-v7a (hard-float)"
      #else
        #define ABI "armeabi-v7a"
      #endif
    #endif
  #else
   #define ABI "armeabi"
  #endif
#elif defined(__i386__)
   #define ABI "x86"
#elif defined(__x86_64__)
   #define ABI "x86_64"
#elif defined(__mips64)  /* mips64el-* toolchain defines __mips__ too */
   #define ABI "mips64"
#elif defined(__mips__)
   #define ABI "mips"
#elif defined(__aarch64__)
   #define ABI "arm64-v8a"
#else
   #define ABI "unknown"
#endif

      return env->NewStringUTF(ABI);
}

```

```java
package com.example.myapplication;  // 定义当前的包名

// 导入相关
import androidx.appcompat.app.AppCompatActivity;

import android.text.method.PasswordTransformationMethod;
import android.text.method.TransformationMethod;

public class MainActivity extends AppCompatActivity {

    // Used to load the 'native-lib' library on application startup.
    static {
        System.loadLibrary("native-lib");
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);   // 调用父类的方法 super
        setContentView(R.layout.activity_main);   // 加载布局界面
        et_name.setText(stringFromJNI());
    }

    /**
     * A native method that is implemented by the 'native-lib' native library,
     * which is packaged with this application.
     */
    public native String stringFromJNI();
}

```









### 动态注册

**文件中出现了 `JNI_OnLoad`  这个函数名字, 就代表使用的是动态注册**

**除了 `JNI_OnLoad` 这个函数, 还需要 为所有类注册本地方法和  为某一个类注册本地方法.**

```c++
#include <stdlib.h>
#include <string.h>
#include <stdio.h>
#include <jni.h>
#include <assert.h>
#define JNIREG_CLASS "com/losileeya/registernatives/JNIUtil"//指定要注册的类
jstring call(JNIEnv* env, jobject thiz)
{
    return (*env)->NewStringUTF(env, "动态注册JNI,居然可以把头文件删掉也不会影响结果，牛逼不咯");
}
jint sum(JNIEnv* env, jobject jobj,jint num1,jint num2){
    return num1+num2;
}
/**
* 方法对应表
*/
static JNINativeMethod gMethods[] = {
        {"stringFromJNI", "()Ljava/lang/String;", (void*)call},
        {"sum", "(II)I", (void*)sum},
};

/*
* 为某一个类注册本地方法
*/
static int registerNativeMethods(JNIEnv* env
        , const char* className
        , JNINativeMethod* gMethods, int numMethods) {
    jclass clazz;
    clazz = (*env)->FindClass(env, className);
    if (clazz == NULL) {
        return JNI_FALSE;
    }
    if ((*env)->RegisterNatives(env, clazz, gMethods, numMethods) < 0) {
        return JNI_FALSE;
    }

    return JNI_TRUE;
}


/*
* 为所有类注册本地方法
*/
static int registerNatives(JNIEnv* env) {
    return registerNativeMethods(env, JNIREG_CLASS, gMethods,
                                 sizeof(gMethods) / sizeof(gMethods[0]));
}

/* 动态注册起始函数
* System.loadLibrary("lib")时调用
* 如果成功返回JNI版本, 失败返回-1
*/
JNIEXPORT jint JNICALL JNI_OnLoad(JavaVM* vm, void* reserved) {
    JNIEnv* env = NULL;
    jint result = -1;

    if ((*vm)->GetEnv(vm, (void**) &env, JNI_VERSION_1_4) != JNI_OK) {
        return -1;
    }
    assert(env != NULL);

    if (!registerNatives(env)) {//注册
        return -1;
    }
    //成功
    result = JNI_VERSION_1_4;

    return result;
}
```





**下面是 `com/losileeya/registernatives/JNIUtil` 文件**

```java

package com.losileeya.registernatives;

/**
 * User: Losileeya (847457332@qq.com)
 * Date: 2016-07-23
 * Time: 14:55
 * 类描述：
 *
 * @version :
 */
public class JNIUtil {
    static {
        System.loadLibrary("RegisterNatives");//与build.gradle里面设置的so名字，必须一致
    }

    public static native String stringFromJNI();
    public static native int sum(int num1,int num2);
}
```

