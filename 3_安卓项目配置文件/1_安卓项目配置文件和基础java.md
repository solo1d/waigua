- [AndroidManifest.xml](#AndroidManifest.xml)
- [MainActivity.java](#MainActivity.java)
- [activity_main.xml](#activity_main.xml)
- 





## AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">
<!-- package=  包名   -->


    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
<!--如果上面 application 里面拥有 android:name=".MainActivity" 类似属性, 就会比MainActivity 类还要先执行 -->

        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

<!-- java文件在安卓的编译过程   .java => .class => .smali => .dex   dex就是最终可执行的文件,他还依赖 .so动态库文件才行 -->
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>


    </application>

</manifest>
```







## MainActivity.java

```java
package com.example.myapplication;  // 定义当前的包名

// 导入相关
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.TextView;

//全类名 :  包名 + 类名       com.example.myapplication.MainActivity
//继承  :  关键字 extends   , 当前类 的基类是 AppCompatActivity


public class MainActivity extends AppCompatActivity {

    // Used to load the 'native-lib' library on application startup.
    static {
        System.loadLibrary("native-lib");
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);   // 调用父类的方法 super
        setContentView(R.layout.activity_main);   // 加载布局界面

        // Example of a call to a native method
        TextView tv = findViewById(R.id.tv_conext);
        tv.setText(stringFromJNI());
    }

    /**
     * A native method that is implemented by the 'native-lib' native library,
     * which is packaged with this application.
     */
    public native String stringFromJNI();
}
```



## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

<!-- 将当前控件内部元素的baseline(基线) 与给定ID 的 基线 对齐   -->
    <TextView
        android:id="@+id/tv_conext"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/android_title"
        />


    <TextView
        android:id="@+id/tv_two"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/android_title"
        />

</LinearLayout>
```

