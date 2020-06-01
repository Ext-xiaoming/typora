2-26

概述：

​		学习使用安卓中的列表

##### 1、ListView

###### 1.1 list_item设计

```java
<!-- 代码如下 模仿消息列表  -->
    <!-- 个人感觉和以前的html盒子概念相似，需要大小盒子相套；需要注意LinearLayout的垂直和水平排布，以及其中的控件位置，其中可以使用padding来规划一定的位置，但不够细致。其次需要注意LinearLayout的父子继承性，父亲的属性优先级高于子女；RelativeLayout布局可以和LinearLayout搭配使用，当某个控件的位置较为特殊，可以考虑放入RelativeLayout盒子中再定位。
    -->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingBottom="12dp"
    android:paddingLeft="15dp"
    android:paddingRight="15dp"
    android:paddingTop="10dp">

    <ImageView
        android:id="@+id/touixang"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@mipmap/tx_1"
        android:background="#e3e3e3"/>
            <>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:paddingBottom="5dp">
            
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:paddingBottom="15dp">
                
            <TextView
                android:id="@+id/list_user_name"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="用户名"
                android:textSize="24dp"
                android:textColor="#000"/>
                    
            <RelativeLayout
                android:layout_width="match_parent"
                android:layout_height="match_parent">
                <TextView
                    android:id="@+id/list_message_time"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="上午1:30"
                    android:layout_alignParentTop="true"
                    android:layout_alignParentRight="true"/>
            </RelativeLayout>
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">
            <TextView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:text="消息内容......"
                android:textSize="23dp"
                android:textColor="#b3e6ff"
                android:gravity="bottom">
            </TextView>
        </LinearLayout>

    </LinearLayout>
</LinearLayout>
```

###### 1.2 message_list 

```java
<!-- 只需一个简单的ListView控件,其具体的格式是由list_item来设定的-->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"

        />
</LinearLayout>
```

###### 1.3 List 的Adapter 适配器

```java 
1.3.1
    主要方法介绍：
            @Override
            public int getCount() {
                return 0;
            }
	设置列表的具体列数，这里的列数一般不会直接设置一个固定值，而会根据具体的逻辑需求来确定，return 的返回值即为列数
```

```java
1.3.2
	主要方法介绍：
            @Override
            public View getView(int i, View view, ViewGroup viewGroup) {
                return null;
            }
	最重要的方法，
```

```

```

