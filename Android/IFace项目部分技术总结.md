### IFace项目部分技术总结

#### 1、okhttp的使用方法

```java
###
implementation 'com.squareup.okhttp3:okhttp:3.12.0'
###

public void toLogin() {
    //
    new Thread(new Runnable() {
        @Override
        public void run() {
        OkHttpClient client = new OkHttpClient();
        FormBody body = new FormBody.Builder()
                .add("userId",userId)
               		......
                .build();

        final Request request = new Request.Builder()
               .url(server+"login/")//请求地址
               .post(body)
               .build();
        Call call = client.newCall(request);
        call.enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
			  // 	请求失败的回执
                runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText( LoginActivity.this, "网络请求失败！" , 												Toast.LENGTH_LONG).show();
                		......
                	}
                });
            }
            @Override
            public void onResponse(Call call, Response response) throws IOException {
                if(response.isSuccessful()){
                    //获取result 再使用json 解析
                    final String result = response.body().string();
                    parseJSONWithJSONObject(result);//json 解析
                   		  ......
                }
            }
        });

    }).start();
}
```



#### 2、json 数据解析

```java
//json解析  解析对象为一整条数据
private void parseJSONWithJSONObject(String jsonData){
        try {
            JSONObject jsonObject= new JSONObject( jsonData ); 
            int res =jsonObject.getInt( "RESULT" );
            String user =jsonObject.getString( "user" );
           		......
            HandleResponse();//获取数据后，对数据或者视图的后续操作
        }catch (Exception e){
            e.printStackTrace();
        }
}

//json  数据解析  对多条类似数据格式的数据进行操作
private void parseJSONWithJSONObjectArray(String jsonData) {
      try {
         JSONArray jsonArray = new JSONArray( jsonData );
         for (int i = 0; i < jsonArray.length(); i++) {
            JSONObject jsonObject = jsonArray.getJSONObject( i );

            String student_id = jsonObject.getString( "student_id" );
            String student_name = jsonObject.getString( "student_name" );
             	......
            //由于是多组数据所以这里需要有相应的接收容器或变量
          }
            HandleResponse();
       } catch (Exception e) {
          e.printStackTrace();
    }
}

private void HandleResponse(final int res){
    //具体处理过程，对视图的·操作要在主线程进行。但是像启动活动则不需要主线程依然可以启动
    ......
}

```



#### 3、material design 组件使用

	>1.首先需要引入：
	>
	> implementation 'com.google.android.material:material:1.0.0'
	>
	>2.资料查找的一些建议：
	>
	>百度  --  Material Design Button / EditText / TextView ...
	>
	>包名查找  如：<com.google.android.material.button.MaterialButton/>
	链接https://developer.android.google.cn/reference/com/google/android/material/package-summary

##### MaterialButton

主要说明： 按钮 可以设置按钮附带图标  圆角等操作

参考链接：https://www.jianshu.com/p/bc71b4179cb2

###### 简单示例：

```java
//困难在于找到对应的包名 正确引入
<com.google.android.material.button.MaterialButton
            style="@style/Widget.MaterialComponents.Button.UnelevatedButton"
            android:id="@+id/denglu"
            android:layout_width="match_parent"
            android:layout_height="60dp"
            android:text="登  录"
            android:background="@color/colorPrimary"
            android:textColor="@android:color/white"
            android:textSize="18sp"
            android:layout_marginLeft="5dp"
            android:layout_gravity="center"
/>
```



##### TextInputLayout

主要说明： 文本框  内置文字可以在输入是自动上浮  可以设置输入字数限制等

参考链接：https://www.jianshu.com/p/4c99e4c0fe90

###### 简单示例：

```java
//示例1
<com.google.android.material.textfield.TextInputLayout
           android:id="@+id/text_input_layout_user1"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           app:counterOverflowTextAppearance="@style/Platform.MaterialComponents.Light"
           android:scrollbarAlwaysDrawHorizontalTrack="true"
           app:counterEnabled="true"
           app:counterMaxLength="15"
           app:hintAnimationEnabled="true"
           app:hintEnabled="true"
           android:textColorHint="@color/colorPrimary">

       <EditText
           android:layout_width="match_parent"
           android:layout_height="wrap_content
           android:hint="账号"
           android:inputType="text"
           android:textColor="@color/black"/>
</com.google.android.material.textfield.TextInputLayout>
```

```java
//示例2   带密码框 与  图标的常用输入框
<LinearLayout
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:orientation="horizontal">

        <ImageView
            android:layout_width="48dp"
            android:layout_height="48dp"
            android:src="@drawable/ic_lock_outline_black_24dp"
            android:layout_margin="6dp"
            android:layout_alignParentBottom="true"/>

        <com.google.android.material.textfield.TextInputLayout
            android:id="@+id/text_input_layout_user3"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:counterOverflowTextAppearance="@style/Platform.MaterialComponents.Light"
            android:scrollbarAlwaysDrawHorizontalTrack="true"
            app:counterEnabled="true"
            app:counterMaxLength="15"
            app:hintAnimationEnabled="true"
            app:hintEnabled="true"
            android:textColorHint="@color/colorPrimary"
            app:passwordToggleEnabled="true"
            app:passwordToggleTint="@color/colorPrimary"
            app:passwordToggleDrawable="@drawable/ic_visibility_black_24dp">

            <com.google.android.material.textfield.TextInputEditText
                android:id="@+id/et_psw"
                android:layout_width="match_parent"
                android:layout_height="48dp"
                android:hint="请输入密码"
                android:textColor="@color/black"
                android:inputType="textPassword"
                android:singleLine="true" />
        </com.google.android.material.textfield.TextInputLayout>
</LinearLayout>
```

#### 4、动态权限处理方法

#### 5、图片通过okhttp上传分析总结

#### 6、相机的使用

#### 7、图库的使用

#### 8、Android 文件的操作问题

#### 9、ImageView 的缩放拖动操作实现

#### 10、Glide 通过url 获取网站静态资源图片

```java
####
implementation 'com.github.bumptech.glide:glide:3.7.0'
###

String url =server+ "statics/images/" + courseCode + "_" + checkNum + ".jpg";

String updateTime = String.valueOf( System.currentTimeMillis() );
Glide.with( KaoqinActivity.this ).load( url )
      .signature( new StringSignature( updateTime ) )
      .override( 800, 600 )//设置大小
      .into( image );
```



#### 11、记住密码 与 自动登录  

​			设计思路：

​			使用SharedPreferences将 账号密码保存到本地，删除自动登录的操作提示（如果账号密码正确，自动保存账号，下次自动登录）。这里说明下在LoginActivity 到MainActivity 的过程：LoginActivity只需要判断账号密码是否正确，反馈给主页面活动的数据只需要  ***intent.putExtra("user",user)***即可，所以自动登录不需要再次查询进行访问。对于自动登录我的思路是在Login 之前设置一个新的界面  （App启动动画页面--最好是动态效果界面完全显示急需要1 到 2s 的时间）动图播放完之后再开始判断该进入那个页面（主界面 or Login 界面）

​			SharedPreferences的使用方法：





















#### 12、忘记密码手机验证码的使用（后续补充）

#### 13、百度地图的使用（加载地图、显示坐标、计算距离）

#### 14、RecycleView  的使用（添加点击事件）

#### 15、RadioGroup 的使用

```java
//xml 布局 
<RadioGroup
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:id="@+id/myRadioGroup"
            android:checkedButton="@+id/sound"
            android:layout_centerVertical="true"
            android:orientation="horizontal"
            >

            <RadioButton
                android:id="@+id/check_teacher"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:layout_centerVertical="true"
                android:text="教师" />

            <RadioButton
                android:id="@+id/check_student"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:gravity="center_vertical"
                android:text="学生" />
</RadioGroup>
```

#### 16、自定义控件的使用 

#### 17、SwipeRefreshLayout 列表刷新操作

#### 19、底部导航栏的使用

#### 20、内容提供器（MediaStore）