# ListView

**数据动态可扩充列表**

- [只有数据](#只有数据)
- [带XML控件](#带XML控件)



## 只有数据

### MainActivity.java

```java
package a.mod.listview;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.ListView;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    ListView lv;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        lv = findViewById( R.id.lv);
        lv.setAdapter(new MyAdapter());
    }

    private class MyAdapter extends BaseAdapter{

        @Override
        public int getCount() {
            // 显示 Item 的数量, ListView 展示的数据量, 条目数量
            return 5000;
        }

        @Override
        public Object getItem(int i) {
            return null;
        }

        @Override
        public long getItemId(int i) {
            return 0;
        }

        // 如果getView 里面 ListView 显示的内容过多, 分页显示, 那么 getView() 调用的次数就非常多了,与滑动屏幕的次数有关
        @Override
        public View getView(int i, View view, ViewGroup viewGroup) {
            // i 是索引
            // view 参数，是系统自动回收并发送过来的资源类， 也就是上次创建的， 可以被重复使用来节省资源
            // 这样做会节省非常多的资源,  资源占用只有 一页显示的内容+1, 之后的内容都是被重复使用的
            TextView   tv_text = (TextView) view;
            if(tv_text == null){
                // 说明没有可重用的 view 对象, 需要创建新的 view 对象
                tv_text = new TextView(MainActivity.this);
                Log.i("new:","创建了新的对象");
            }else{
                Log.i("noe:","回收了对象");
            }

            tv_text.setText("第" + i + "个数据");

            // 返回 TextView
            return tv_text;
        }
    }
}
```



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

<!--  android:textAllCaps="false" 关闭自动将字母进行大写的设置 -->
<!--   android:dividerHeight="3dp" 分割线宽度  -->
<!--  android:divider="#b6ac00"  分割线颜色-->
    <ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/lv"
        android:textAllCaps="false"
        android:dividerHeight="3dp"
        android:divider="#b6ac00"
        />
</LinearLayout>
```



## 带XML控件

### MainActivity.java

```java
package a.mod.listview;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.ListView;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    ListView lv;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        lv = findViewById(R.id.lv);
        lv.setAdapter(new MyAdapter());
    }

    private class  MyAdapter extends BaseAdapter{

        @Override
        public int getCount() {
            return 50;
        }

        @Override
        public Object getItem(int i) {
            return null;
        }

        @Override
        public long getItemId(int i) {
            return 0;
        }

        @Override
        public View getView(int i, View view, ViewGroup viewGroup) {
            View vi  = null;
            if (view == null){
                // (上下文环境, 布局的资源文件.xml ID,  将创建出来的视图内容放入到 指定的 ViewGroup 线性布局或相对布局界面)
                //   将 xml 文件转换成 view对象, 从而填充到 ListView 内部去
                // vi = View.inflate(MainActivity.this, R.layout.item,null);


                // 获得布局填充器 ， 或者叫打气筒, 下面是获取的三种方式
                //LayoutInflater inflater = LayoutInflater.from(MainActivity.this);  // 普通方法
                     // LayoutInflater inflater = getLayoutInflater();   //普通方法
                      LayoutInflater inflater = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICE);  //goole 的方法 存在于 goole的源代码中

                vi  = inflater.inflate(R.layout.item, null);
            }else{
                vi = view;
            }
            tv_pid =   vi.findViewById(R.id.tv_pid);
            tv_name =   vi.findViewById(R.id.tv_name);

            tv_pid.setText("1");
            tv_name.setText("2");

            return vi;
        }

        public TextView tv_pid;
        public TextView tv_name;

        public final class tv_name_pid{

        }
    }
}
```



### acttivity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

<!--  android:textAllCaps="false" 关闭自动将字母进行大写的设置 -->
<!--   android:dividerHeight="3dp" 分割线宽度  -->
<!--  android:divider="#b6ac00"  分割线颜色-->
    <ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/lv"
        android:textAllCaps="false"
        android:dividerHeight="3dp"
        android:divider="#b6ac00"
        />
</LinearLayout>
```



### item.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:padding="5dp"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/tv_pid"
        android:hint="[pid]"
        android:textSize="14sp"
        />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/tv_name"
        android:hint="[name]"
        />

</LinearLayout>
```

