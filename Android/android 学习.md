# android 学习

## 2-26

#### >概述

​		在App的登陆界面一般会有这样的一个功能 “记住历史登陆的账号密码” 。具体而言，可以类比QQ的登陆界面，如果该账号在此前有被记住密码，则添加到本地记录（SharePreference/LitePal.SQLite），登陆时点击账号<EditText>框或者<CheckBox>弹出历史账号同时自动填充账号密码。

<img src="C:\Users\Administrator\Desktop\QQ截图20200226113008.png" style="zoom: 50%;" />



> ###### 关于登陆界面的一些思考：
>
> 1、‘记住密码’框应该是没有必要显示在这个页面上的，可以设置为默认。当账号密码正确 ，登录成功后自动记住密码，下次进入时进行自动登录。
>
> 2、添加一个“用户协议选项”

#### 1、添加 '历史账号'功能	

##### 1.1添加图标

```java
1.在登录界面的xml中添加  <CheckBox>

    <RelativeLayout
 	    ......
        <CheckBox
            android:id="@+id/historyCB"
            style="@style/EditTextHistoryCheckboxTheme"
            android:layout_width="48dp"
            android:layout_height="30dp"
            android:layout_marginBottom="20dp"
            android:layout_alignParentRight="true"
            android:layout_centerVertical="true"/>
    </RelativeLayout>
                
2.在 style.xml 文件中添加   <@style/EditTextHistoryCheckboxTheme> 
                	
    <style name="EditTextHistoryCheckboxTheme" parent="@android:style/Widget.CompoundButton.CheckBox">
        <item name="android:button">@drawable/edittext_history_checkbox_style</item>
    </style>
                
3.在drawablew文件下添加 edittext_history_checkbox_style.xml
                
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@mipmap/login_history_up" android:state_checked="true" />
        <item android:drawable="@mipmap/login_history_down" android:state_checked="false" />
        <item android:drawable="@mipmap/login_history_down" />
    </selector>
4.在mipmap文件中添加 login_history_up.webp  / login_history_down.webp(点击前后向上向下的变化)
```

##### 1.2实现列表







