- [源代码](#源代码)
- [反汇编解读](#反汇编解读)
  - [MainActivity.smali](#MainActivity.smali)
  - [MainActivity$1.smali](#MainActivity$1.smali)
  - 



## 源代码

```java
package com.example.myapplication;  // 定义当前的包名

// 导入相关
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.text.method.PasswordTransformationMethod;
import android.text.method.TransformationMethod;
import android.widget.EditText;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;


//全类名 :  包名 + 类名       com.example.myapplication.MainActivity
//继承  :  关键字 extends   , 当前类 的基类是 AppCompatActivity


public class MainActivity extends AppCompatActivity {

    // Used to load the 'native-lib' library on application startup.
    static {
        System.loadLibrary("native-lib");
    }

    private EditText et_name;
    private EditText et_pwd;
    private Button   btn_login;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);   // 调用父类的方法 super
        setContentView(R.layout.activity_main);   // 加载布局界面

        // Example of a call to a native method
        et_name = findViewById(R.id.et_name);
        et_pwd = findViewById(R.id.et_pwd);
        btn_login = findViewById(R.id.bt_Lonin);

        // 设置密码框 使用密文
        et_pwd.setTransformationMethod(PasswordTransformationMethod.getInstance());

       btn_login.setOnClickListener(new View.OnClickListener() {
           @Override
           public void onClick(View view) {
               String name = et_name.getText().toString().trim();
               String  pwd = et_pwd.getText().toString().trim();
               Boolean temp =  login(name, pwd);
               if(!temp){
                   Toast.makeText(MainActivity.this, "登陆失败,账户密码错误",Toast.LENGTH_SHORT).show();
               }
           }
       });

        et_name.setText(stringFromJNI());
    }

    private  boolean login(String name, String pwd){
        if(name == null || pwd == null){
            Toast.makeText(MainActivity.this, "请输入正确的用户名和密码",Toast.LENGTH_SHORT).show();
            return false;

        }
        // equals()   String 是字符串对比函数
        if (name.equals("123123") && pwd.equals("123123")){
            Toast.makeText(MainActivity.this, "登陆成功",Toast.LENGTH_SHORT).show();
            return true;
        }

        return false;
    }


    /**
     * A native method that is implemented by the 'native-lib' native library,
     * which is packaged with this application.
     */
    public native String stringFromJNI();
}

```

海韵新城



## 反汇编解读

```R
#寄存器    p0  代表 当前类的this 这个指针

.class public Lcom/example/myapplication/MainActivity;   #当前类的名称
.super Landroidx/appcompat/app/AppCompatActivity;     # 源文件 MainActivity类 继承的父类
.source "MainActivity.java"         # 反汇编的源文件为 MainActivity.java

instance fields
.field private btn_login:Landroid/widget/Button;     #btn_login  私有变量


.line 23   #下面的内容都是第23行 进行反汇编出来的

const-string v0, "native-lib"    #不可修改字符串, 赋值给 v0 寄存器



# annotations   下面一部分是注解信息   开始
.annotation system Ldalvik/annotation/EnclosingMethod;
		#	... 内容
.end annotation
# 注解信息    结束


# 方法调用指令,  static静态方法,  super父类方法, virtual虚函数方法, direct直接调用(普通方法)
invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V  


.locals 1       #初始化
.local  v0, "pwd"  #初始化



if-eqz  v2,:cond_0     #判断 v2 == 0, 就跳转到 cond_0 位置
if-nez  v2,:cond_0     #判断 v2 != 0 ,就跳转到 cond_0 位置


move-result-object v0                    #将数据类型为 object 的值放入 v0 寄存器
check-cast v0, Landroid/widget/EditText;  #数据类型转换, 将v0 转换成 EditText类型



.param p1, "view"    # Landroid/view/View;   当前局部使用的寄存器, 并将 "view" 这个变量说存储的值放入进去

```





### MainActivity.smali

文件为  `My Application/smali_classes2/com/example/myapplication/MainActivity.smali`

```R
.class public Lcom/example/myapplication/MainActivity;   #当前类的名称
.super Landroidx/appcompat/app/AppCompatActivity;     # 源文件 MainActivity类 继承的父类
.source "MainActivity.java"         # 反汇编的源文件为 MainActivity.java


instance fields
.field private btn_login:Landroid/widget/Button;     #btn_login 变量

.field private et_name:Landroid/widget/EditText;     #et_name 变量

.field private et_pwd:Landroid/widget/EditText;      #et_pwd 变量


# direct methods
.method static constructor <clinit>()V
    .locals 1

    .line 23
    const-string v0, "native-lib"

# 方法调用指令,  invoke-
    invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V

    .line 24
    return-void
.end method

.method public constructor <init>()V
    .locals 0

    .line 19
    invoke-direct {p0}, Landroidx/appcompat/app/AppCompatActivity;-><init>()V

    return-void
.end method

.method static synthetic access$000(Lcom/example/myapplication/MainActivity;)Landroid/widget/EditText;
    .locals 1
    .param p0, "x0"    # Lcom/example/myapplication/MainActivity;

    .line 19
    iget-object v0, p0, Lcom/example/myapplication/MainActivity;->et_name:Landroid/widget/EditText;

    return-object v0
.end method

.method static synthetic access$100(Lcom/example/myapplication/MainActivity;)Landroid/widget/EditText;
    .locals 1
    .param p0, "x0"    # Lcom/example/myapplication/MainActivity;

    .line 19
    iget-object v0, p0, Lcom/example/myapplication/MainActivity;->et_pwd:Landroid/widget/EditText;

    return-object v0
.end method

.method static synthetic access$200(Lcom/example/myapplication/MainActivity;Ljava/lang/String;Ljava/lang/String;)Z
    .locals 1
    .param p0, "x0"    # Lcom/example/myapplication/MainActivity;
    .param p1, "x1"    # Ljava/lang/String;
    .param p2, "x2"    # Ljava/lang/String;

    .line 19
    invoke-direct {p0, p1, p2}, Lcom/example/myapplication/MainActivity;->login(Ljava/lang/String;Ljava/lang/String;)Z

    move-result v0

    return v0
.end method

.method private login(Ljava/lang/String;Ljava/lang/String;)Z
    .locals 3
    .param p1, "name"    # Ljava/lang/String;
    .param p2, "pwd"    # Ljava/lang/String;

    .line 59
    const/4 v0, 0x0

    if-eqz p1, :cond_2

    if-nez p2, :cond_0

    goto :goto_0

    .line 65
    :cond_0
    const-string v1, "123123"

    invoke-virtual {p1, v1}, Ljava/lang/String;->equals(Ljava/lang/Object;)Z

    move-result v2

    if-eqz v2, :cond_1

    invoke-virtual {p2, v1}, Ljava/lang/String;->equals(Ljava/lang/Object;)Z

    move-result v1

    if-eqz v1, :cond_1

    .line 66
    const-string v1, "\u767b\u9646\u6210\u529f"
										 # 上面这段字符串是 unicode编码, 解码后为字符串   登陆成功

    invoke-static {p0, v1, v0}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;

    move-result-object v0

    invoke-virtual {v0}, Landroid/widget/Toast;->show()V

    .line 67
    const/4 v0, 0x1

    return v0

    .line 70
    :cond_1
    return v0

    .line 60
    :cond_2
    :goto_0
    const-string v1, "\u8bf7\u8f93\u5165\u6b63\u786e\u7684\u7528\u6237\u540d\u548c\u5bc6\u7801"
											# 解码 : 请输入正确的用户名和密码

    invoke-static {p0, v1, v0}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;

    move-result-object v1

    invoke-virtual {v1}, Landroid/widget/Toast;->show()V

    .line 61
    return v0
.end method


# virtual methods
.method protected onCreate(Landroid/os/Bundle;)V
    .locals 2
    .param p1, "savedInstanceState"    # Landroid/os/Bundle;

    .line 32
    invoke-super {p0, p1}, Landroidx/appcompat/app/AppCompatActivity;->onCreate(Landroid/os/Bundle;)V

    .line 33
    const v0, 0x7f0a001c

    invoke-virtual {p0, v0}, Lcom/example/myapplication/MainActivity;->setContentView(I)V

    .line 36
    const v0, 0x7f070076

    invoke-virtual {p0, v0}, Lcom/example/myapplication/MainActivity;->findViewById(I)Landroid/view/View;

    move-result-object v0

    check-cast v0, Landroid/widget/EditText;

    iput-object v0, p0, Lcom/example/myapplication/MainActivity;->et_name:Landroid/widget/EditText;

    .line 37
    const v0, 0x7f070077

    invoke-virtual {p0, v0}, Lcom/example/myapplication/MainActivity;->findViewById(I)Landroid/view/View;

    move-result-object v0

    check-cast v0, Landroid/widget/EditText;

    iput-object v0, p0, Lcom/example/myapplication/MainActivity;->et_pwd:Landroid/widget/EditText;

    .line 38
    const v0, 0x7f070051

    invoke-virtual {p0, v0}, Lcom/example/myapplication/MainActivity;->findViewById(I)Landroid/view/View;

    move-result-object v0

    check-cast v0, Landroid/widget/Button;

    iput-object v0, p0, Lcom/example/myapplication/MainActivity;->btn_login:Landroid/widget/Button;

    .line 41
    iget-object v0, p0, Lcom/example/myapplication/MainActivity;->et_pwd:Landroid/widget/EditText;

    invoke-static {}, Landroid/text/method/PasswordTransformationMethod;->getInstance()Landroid/text/method/PasswordTransformationMethod;

    move-result-object v1

    invoke-virtual {v0, v1}, Landroid/widget/EditText;->setTransformationMethod(Landroid/text/method/TransformationMethod;)V

    .line 43
    iget-object v0, p0, Lcom/example/myapplication/MainActivity;->btn_login:Landroid/widget/Button;

    new-instance v1, Lcom/example/myapplication/MainActivity$1;

   
    invoke-direct {v1, p0}, Lcom/example/myapplication/MainActivity$1;-><init>(Lcom/example/myapplication/MainActivity;)V
		#MainActivity$1	指的是另一个文件

    invoke-virtual {v0, v1}, Landroid/widget/Button;->setOnClickListener(Landroid/view/View$OnClickListener;)V

    .line 55
    iget-object v0, p0, Lcom/example/myapplication/MainActivity;->et_name:Landroid/widget/EditText;

    invoke-virtual {p0}, Lcom/example/myapplication/MainActivity;->stringFromJNI()Ljava/lang/String;

    move-result-object v1

    invoke-virtual {v0, v1}, Landroid/widget/EditText;->setText(Ljava/lang/CharSequence;)V

    .line 56
    return-void
.end method

.method public native stringFromJNI()Ljava/lang/String;
.end method
```



### MainActivity$1.smali

```R
.class Lcom/example/myapplication/MainActivity$1;
.super Ljava/lang/Object;
.source "MainActivity.java"

# interfaces,  这个表示 包含了一个接口, 点击事件
.implements Landroid/view/View$OnClickListener;


# annotations   下面一部分是注解信息   开始
.annotation system Ldalvik/annotation/EnclosingMethod;
    value = Lcom/example/myapplication/MainActivity;->onCreate(Landroid/os/Bundle;)V
.end annotation

.annotation system Ldalvik/annotation/InnerClass;
    accessFlags = 0x0
    name = null
.end annotation
# 注解信息    结束



# instance fields
.field final synthetic this$0:Lcom/example/myapplication/MainActivity;


# direct methods
.method constructor <init>(Lcom/example/myapplication/MainActivity;)V
    .locals 0
    .param p1, "this$0"    # Lcom/example/myapplication/MainActivity;

    .line 43
    iput-object p1, p0, Lcom/example/myapplication/MainActivity$1;->this$0:Lcom/example/myapplication/MainActivity;

    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    return-void
.end method


# virtual methods
.method public onClick(Landroid/view/View;)V
    .locals 6
    .param p1, "view"    # Landroid/view/View;

    .line 46
    iget-object v0, p0, Lcom/example/myapplication/MainActivity$1;->this$0:Lcom/example/myapplication/MainActivity;

    invoke-static {v0}, Lcom/example/myapplication/MainActivity;->access$000(Lcom/example/myapplication/MainActivity;)Landroid/widget/EditText;

    move-result-object v0

    invoke-virtual {v0}, Landroid/widget/EditText;->getText()Landroid/text/Editable;

    move-result-object v0

    invoke-virtual {v0}, Ljava/lang/Object;->toString()Ljava/lang/String;

    move-result-object v0

    invoke-virtual {v0}, Ljava/lang/String;->trim()Ljava/lang/String;

    move-result-object v0

    .line 47
    .local v0, "name":Ljava/lang/String;
    iget-object v1, p0, Lcom/example/myapplication/MainActivity$1;->this$0:Lcom/example/myapplication/MainActivity;

    invoke-static {v1}, Lcom/example/myapplication/MainActivity;->access$100(Lcom/example/myapplication/MainActivity;)Landroid/widget/EditText;

    move-result-object v1

    invoke-virtual {v1}, Landroid/widget/EditText;->getText()Landroid/text/Editable;

    move-result-object v1

    invoke-virtual {v1}, Ljava/lang/Object;->toString()Ljava/lang/String;

    move-result-object v1

    invoke-virtual {v1}, Ljava/lang/String;->trim()Ljava/lang/String;

    move-result-object v1

    .line 48
    .local v1, "pwd":Ljava/lang/String;
    iget-object v2, p0, Lcom/example/myapplication/MainActivity$1;->this$0:Lcom/example/myapplication/MainActivity;

    invoke-static {v2, v0, v1}, Lcom/example/myapplication/MainActivity;->access$200(Lcom/example/myapplication/MainActivity;Ljava/lang/String;Ljava/lang/String;)Z

    move-result v2

    invoke-static {v2}, Ljava/lang/Boolean;->valueOf(Z)Ljava/lang/Boolean;

    move-result-object v2

    .line 49
    .local v2, "temp":Ljava/lang/Boolean;
    invoke-virtual {v2}, Ljava/lang/Boolean;->booleanValue()Z

    move-result v3

    if-nez v3, :cond_0

    .line 50
    iget-object v3, p0, Lcom/example/myapplication/MainActivity$1;->this$0:Lcom/example/myapplication/MainActivity;

    const/4 v4, 0x0

    const-string v5, "\u767b\u9646\u5931\u8d25,\u8d26\u6237\u5bc6\u7801\u9519\u8bef"
				#解码:  登陆失败, 账户密码错误

    invoke-static {v3, v5, v4}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;

    move-result-object v3

    invoke-virtual {v3}, Landroid/widget/Toast;->show()V

    .line 52
    :cond_0
    return-void
.end method

```

