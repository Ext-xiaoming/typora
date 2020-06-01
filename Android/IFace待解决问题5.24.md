#### 1.Response 结果集的处理方法:

```java
/*
1、获取结果集
2、json 实体类  user.class
3、json 函数获取指定字段值
4、根据结果集做出相应处理
5、测试方法：.login/ + Myapp..


*/
response.body().string();
//!ok
//http响应
@Override
public void onResponse(Call call, Response response) throws IOException {
       if(response.isSuccessful()){
       final String result = response.body().string();
       parseJSONWithJSONObject(result);
       Log.d( "PhotoActivity", result );
       }
}
//json 数据解析
    private void parseJSONWithJSONObjectArray(String jsonData){
        try {
            JSONArray jsonArray = new JSONArray( jsonData );
            for (int i=0 ;i<jsonArray.length();i++){
                JSONObject jsonObject = jsonArray.getJSONObject( i );
                String falg=jsonObject.getString( "flag" );
                String id =jsonObject.getString( "id" );
                Log.d( "PhotoActivity","flag is"+falg );
                Log.d( "PhotoActivity","id is"+id );
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    //json解析
    private void parseJSONWithJSONObject(String jsonData){
        try {
            JSONObject jsonObject= new JSONObject( jsonData );
                String falg=jsonObject.getString( "flag" );
                String id =jsonObject.getString( "id" );
                Log.d( "PhotoActivity","flag is"+falg );
                Log.d( "PhotoActivity","id is"+id );
		   //数据解析完成之后进行 进行相应的处理   
            HandleResponse(flag,id);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

//结果处理
 private void HandleResponse(final String flag,final String id) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText( PhotoActivity.this,falg+id,Toast.LENGTH_SHORT ).show();
            }
        });
    }

```

```python
#Django 返回json 格式数据
data={"flag":"111","id":"1213"}
return HttpResponse(json.dumps(data, ensure_ascii=False), 				                                         content_type="application/json,charset=utf-8")
```





#### 2.修改密码功能

```python
/*
#问题所在：django 数据库修改函数需要继续测试（大概率有问题）
		onResponse需要做出相应的处理
*/

/*
#!ok
#忘记添加save()
*/

#修改数据库方法一       #models.Teacher.objects.filter(teacher_id=userId).update(teacher_password=new_password)
#修改数据库方法二
res2=models.Teacher.objects.get(teacher_id=userId)
res2.teacher_password=new_password
res2.save()

```

#### 3.登陆功能

```java
/*
1、数据库查询语句迁移
2、返回结果集，根据结果集做出相应的处理
3、返回形式：flag=true(教师) + userId="1111"(根据ID识别)
*/
//!OK

//返回user身份标志 RESULT=1 为教师 (-1 为账号或密码错误)

```

```python
# 如果使用get（）当查询结果不存在时会出现异常 ,在此处使用filter
#这里的user是一个QuerySet结果集不能直接使用 passeord =user.teacher_password通过for可处理

#此处的功能是判断是否是教师账号以及密码是否正确
user = models.Teacher.objects.filter(teacher_id=userId)   
    if user.exists():
        for res in user:
            passeord =res.teacher_password
        if passeord==userPassword:
            print("the teacher  password  is true")
            data = {"RESULT": 1}
            return HttpResponse(json.dumps(data, ensure_ascii=False), 											content_type="application/json,charset=utf-8")
```



#### 4.fragment单例模式

```java 
/*
1、百度单例模式
2、从static入手
3、寻求帮助
*/
//!ok
public static StuMainFragmentClass getMainFragment(String studentId){
        if(mf == null){
            mf = new StuMainFragmentClass();
        }
        mf.studentId= studentId;
        return mf;
    }

//Fragment只实例化一次 对于前面出现过的LIstView 重复加载的问题原因分析如下：
/*既然是ListView 重复加载了相同的内容 在实例化一次的前提下去思考  */
	final CoursesAdapter coursesAdapter = new CoursesAdapter(getActivity() ,
                           	 R.layout.course_item , listItemCourses);
	lvListView.setAdapter(coursesAdapter);


/********************************************************************************************
	原因只会是coursesAdapter 构建时listItemCourses的内容本身就是重复的；
如果每次在onCreateView()视图加载时 重新实例化对象：
	List<CourseListItem> listItemCourses = new ArrayList<>();
	然后根据该次查询结果添加数据，构建coursesAdapter这样一定是准确不重复的数据
再看原来的代码，将listItemCourses实例化放到了onCreateView()的外边，在切换到该页面时由于使用单例模式，StuMainFragmentClass类对象只有一个里面的的属性listItemCourses也只有一份，所以当查询到数据后也是在原来的基础上进行listItemCourses.add(item)操作。

	解决方案：在onCreateView()内部实例化  或者作为StuMainFragmentClass类对象的一个属性 ，在onCreateView()内部进行初始化操作，清空以前的数据。
********************************************************************************************/


/********************************************************************************************
	改进思路：由于大部分的数据其实不会改变，所以可以考虑 保存到本地xml文件中，需要时读取；当有可能出现的更新数据时直接在xml 文件中更新。（或者在App第一次启动时创建一个xml 文件保存可能需要的大部分数据，后台服务类型）
********************************************************************************************/

```

```python
#django 数据库操作--加载教师端课程列表

    course_id=[]         		#列表
    course_name=[]
    i=0
    course = models.Course.objects.filter(teacher_id=teacherId)
    print(type(course),course)
    for c in course:			#QuerySet只能通过for操作
        course_id.append(c.course_id)#需要使用append()方法添加
        course_name.append(c.course_name)

    len_id = len(course_id)#获取长度
    data=[]		#列表字典 列表的单条内容是字典
    j = 0
    while j < len_id:
        data.append({"course_name":course_name[j],
                     "course_id":course_id[j],"teacher_name":teacher_name})
        j+=1
	#最终的返回data即可(当然首先需要转为json)
    

```



#### 5、学生端签到

```java 
/*
--首先在直连mysql的情况下完成功能
--该迁移数据库操作到后台的工作放到最后完善，功能较为复杂
1、通过post 提交所有的参数
2、位置比对和时间比对在django 上完成
3、在服务器上完成签到
4、根据响应结果Toast信息
*/

//---受到定位定位信息的影响，暂时无法处理，晚上回去后在进行处理！！！！
//！ok

```

```java 
/*地图显示和定位功能*/
//参考《第一行代码》

    private MapView mapView=null;
    private BaiduMap baiduMap;
    private LocationClient locationClient;
    private boolean isFirstLocte=true;
    private TextView positionText;

    private double sbjingdu2;
    private double sbweidu2;

    private String TAG = "NumqiandaoActivity";
    public LocationClient mLocationClient;
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
       
        mLocationClient = new LocationClient( getApplicationContext() );
        mLocationClient.registerLocationListener( new MyLocationListener() );
        SDKInitializer.initialize( getApplicationContext() );
        setContentView( R.layout.num_qaindao_sec);

		......
            
        mapView=(MapView) findViewById(R.id.bmapView3);
        baiduMap=mapView.getMap();
        baiduMap.setMyLocationEnabled(true);

        List<String> permissionList=new ArrayList<>();
        if (ContextCompat.checkSelfPermission( NumqiandaoActivity.this,
                Manifest.permission.ACCESS_FINE_LOCATION)!= PackageManager.PERMISSION_GRANTED){
            permissionList.add(Manifest.permission.ACCESS_FINE_LOCATION);
        }
        if (ContextCompat.checkSelfPermission(NumqiandaoActivity.this,
                Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_GRANTED){
            permissionList.add(Manifest.permission.READ_PHONE_STATE);
        }
        if (ContextCompat.checkSelfPermission(NumqiandaoActivity.this,
                Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED){
            permissionList.add(Manifest.permission.WRITE_EXTERNAL_STORAGE);
        }
        if (ContextCompat.checkSelfPermission(NumqiandaoActivity.this,
                Manifest.permission.WRITE_SETTINGS) != PackageManager.PERMISSION_GRANTED){
            permissionList.add(Manifest.permission.WRITE_SETTINGS);
        }
        if (!permissionList.isEmpty()){
            String[] permissions =permissionList.toArray(new String[permissionList.size()]);
            ActivityCompat.requestPermissions(NumqiandaoActivity.this, permissions, 1);
        }else {
            requestLocation();
        }
    }

    private void requestLocation(){
        initLocation();
        mLocationClient.start();
    }

    private void initLocation() {
        LocationClientOption option = new LocationClientOption(  );
        option.setCoorType("bd09ll");// 坐标类型
        option.setScanSpan(5000);
        option.setLocationMode( LocationClientOption.LocationMode.Device_Sensors);
        mLocationClient.setLocOption(option);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.num_submit:           
                break;
            default:
                break;
        }
    }

    //权限
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult( requestCode, permissions, grantResults );
        switch (requestCode){
            case 1 :
                if(grantResults.length>0 ) {
                    for(int result :grantResults){
                        if(result!=PackageManager.PERMISSION_GRANTED){
                            Toast.makeText( this,"必须同意权限",Toast.LENGTH_LONG ).show();
                            //finish();
                            //return;
                        }
                    }
                    requestLocation();
                }else{
                    Toast.makeText( this,"未知错误",Toast.LENGTH_SHORT ).show();
                    finish();
                }
                break;
                
            default:
        }
    }

    public class GPSUtils {
        private double EARTH_RADIUS = 6378.137;
        private double rad(double d) {
            return d * Math.PI / 180.0;
        }
        /**
         * Lat1 Lung1 表示A点经纬度，Lat2 Lung2 表示B点经纬度； a=Lat1 – Lat2 为两点纬度之差 	b=Lung1
         * -Lung2 为两点经度之差； 6378.137为地球半径，单位为千米；  计算出来的结果单位为千米。
         * 通过经纬度获取距离(单位：千米)
         * @param lat1
         * @param lng1
         * @param lat2
         * @param lng2
         * @return
         */
        public double getDistance(double lat1, double lng1, double lat2,
                                  double lng2) {
            double radLat1 = rad(lat1);
            double radLat2 = rad(lat2);
            double a = radLat1 - radLat2;
            double b = rad(lng1) - rad(lng2);
            double s = 2 * Math.asin(Math.sqrt(Math.pow(Math.sin(a / 2), 2)
                    + Math.cos(radLat1) * Math.cos(radLat2)
                    * Math.pow(Math.sin(b / 2), 2)));
            s = s * EARTH_RADIUS;
            s = Math.round(s * 10000d) / 10000d;
            // s = s*1000;    乘以1000是换算成米
            return s;
        }
    }

    public class MyLocationListener implements BDLocationListener {//经纬度的确认
        @Override
        public void onReceiveLocation(final BDLocation location) {

            if(location.getLocType()==BDLocation.TypeGpsLocation 												||location.getLocType()==BDLocation.TypeNetWorkLocation )
            {
                navigateTo(location);
            }

            runOnUiThread( new Runnable() {
                @Override
                public void run() {
                    sbjingdu2=location.getLongitude();
                    sbweidu2=location.getLatitude();
                    StringBuilder currentPosition= new StringBuilder();
           currentPosition.append( "纬度" ).append( location.getLatitude() ).append( "\n" );
           currentPosition.append( "经度" ).append( location.getLongitude() ).append( "\n" );
                    currentPosition.append( "定位方式： " );
                    if(location.getLocType()==BDLocation.TypeGpsLocation){
                        currentPosition.append( "Gps" );
                    }else if(location.getLocType()==BDLocation.TypeNetWorkLocation){
                        currentPosition.append( "网络" );
                    }
                    positionText.setText( currentPosition );
                }
            } );

        }
    }

    private void navigateTo(BDLocation location) {
        if(isFirstLocte){
            LatLng ll = new LatLng( location.getLatitude(),location.getLongitude() );
            MapStatusUpdate update = MapStatusUpdateFactory.newLatLng( ll );
            baiduMap.animateMapStatus( update );
            update = MapStatusUpdateFactory.zoomTo( 16f );
            baiduMap.animateMapStatus( update );
            isFirstLocte=false;
        }

        MyLocationData.Builder locationBuilder=new MyLocationData.Builder();
        locationBuilder.latitude( location.getLatitude() );
        locationBuilder.longitude( location.getLongitude() );
        MyLocationData locationData=locationBuilder.build();
        baiduMap.setMyLocationData(locationData);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mLocationClient.stop();
        mapView.onDestroy();
        baiduMap.setMyLocationEnabled(false);
    }

    @Override
    protected void onResume() {
        super.onResume();
        mapView.onResume();
    }

    @Override
    protected void onPause() {
        super.onPause();
        mapView.onPause();
    }

//以上就是较为完整的百度地图定位于显示的代码  xml文件为：

<LinearLayout
        android:layout_gravity="center"
        android:layout_width="360dp"
        android:layout_height="360dp">
        <com.baidu.mapapi.map.MapView
            android:id="@+id/bmapView"
            android:layout_width="match_parent"
            android:layout_height="match_parent">
        </com.baidu.mapapi.map.MapView>
</LinearLayout>

```

```python
#django 后台 -- 学生数字签到
#分为两个过程 首先看是否已经签到了 然后没有签到就返回相应的地址和时间信息 提供给客户端比较 如果可以
#则继续进行 下一个请求插入签到信息

#主要代码
# 验证是否已经签到  
def verIsCheck(request):
    
    XXX = request.POST.get('xxx')
   
 	#查询：post_id已知student_id已知 只需要查 sign_in是否有 有 就已经签到了不用再签了
    #多条件查询 添加(from django.db.models import Q )
    sign=models.SignIn.objects.filter(Q(post_id=qiandaoNum) & Q(student_id=studentId))
    #判断结果是否为空 .exists()  注意raw(sql)的结果集没有这个操作
    if sign.exists():
        #如果结果不为空则表明已经签到了 RESULT=-1为已经签到了
        data = {"RESULT": -1, ......}
        return HttpResponse(json.dumps(data,......))
    else:
        #结果为空表明没有签到，则需要获取一些列信息   
        # !!! 由于一直报错  Raw query must include the primary key 放弃 !!!
        # sql = "select * from  post_check_in  where  post_num in( select max(post_num) from post_check_in where post_id = " + qiandaoNum + ")"
        # print(sql)
        # res = models.Student.objects.raw(sql)

        #先查询 获取最大的post_num
        res_post_num = models.PostCheckIn.objects.filter(post_id=qiandaoNum)
        
        # 这里必须注意添加.get('post_num__max'))  
        #否则错误 int() argument must be a string, a bytes-like object or a number, not 'dict'
        max = int(res_post_num.aggregate(Max('post_num')).get('post_num__max'))
        
        #根据post_num和post_id查询
        res4 = models.PostCheckIn.objects.filter(Q(post_num=max) & Q(post_id=qiandaoNum))
        
        if res4.exists():
            for r in res4:
                post_id=r.post_id
                 ......
                data = {"RESULT": 1, "post_longitude": post_longitude, ......}
                return HttpResponse(json.dumps(data, ...))
        else:
            #此时结果为空表明postID签到码不正确
            data = {"RESULT": 0, ......}
            return HttpResponse(json.dumps(data, .....))

    data = {"RESULT": 0, ......}
    return HttpResponse(json.dumps(data, .....))



#学生将签到信息插入数据库
def numQiandaoS(request):
    post_id = request.POST.get('post_id')
    	.......
        
    # 查询获取做大的sign_id
    res = models.SignIn.objects.all()
    if res.exists():
        sign_id = int(res.aggregate(Max('sign_id')).get('sign_id__max')) + 1
    else:
        sign_id=1
    # 插入数据库操作
    opreateAdd = models.SignIn(sign_id=sign_id, student_id=student_id, post_type=post_type, 			post_num=post_num,post_id=post_id)
    opreateAdd.save()

    data = {"RESULT": 1}
    return HttpResponse(json.dumps(data, ensure_ascii=False),                                                 content_type="application/json,charset=utf-8")

```

#### 6、django 实现 https 访问

```java
/*------5/26-12:30
1、再次测试
2、服务器运行环境搭建
3、初步调试
4、
...
*/
```



#### 7、 照片上传功能

```java
/*---暂未进行测试
1、照片命名规范1.jpg
2、学生  .sanePicture/ 完善逻辑功能
3、教师上传  
4、python 图片保存到本地
5、在App显示一张带标记人脸的图片
*/

//--暂时ok

```

#### 8、完善django上算法的接口对接处理

```java
/*---暂未进行测试
1、索引处理
2、参数获取
...
*/

//--暂时ok
```

#### 9、全部数据库操作APP转向django

```java
/*----!ok 
1、AddCourseActivity
2、学生端的StuMainFragmentClass--ok
3、StuMainFragmentUser--ok
4、教师端的个人中心（这里采用登陆时获取信息）--ok
5、学生端的考勤列表
思路 ：显示内容 post_num 、post_data、出勤/缺勤
	通过course_id 找到该课程所有的老师发布的签到记录（post_id、post_num 、post_data）
	由post_id 在 sign_in 中查询是否存在 存在则表明“出勤”

6、教师端的考勤列表
思路 ：显示内容 num 、post_data、post_num、post_type
	通过course_id 找到该课程所有的老师发布的签到记录  表 post_check_in （post_id、post_num 、post_data）
	由post_id 在 sign_in 中查询人数即可（有几条记录就是几个人签到了）

7、考勤学生名单
显示内容 student_id 、student_name、CheckStatu
通过course_id 在SC表中找到该课程所有的学生（student_id）
由student_id 在 student 中找到姓名
由post_id、 student_id 在 sign_in 中查询有则出勤否则反之

8、发布课程
传递三个参数直接插入即可
9、添加课程
传递三个参数直接插入即可
10、发布签到

----！ok
*/
```

#### 10、ListView优化为RecycleView

```java
/*
----!ok!
主要问题：数据源 及其 加载

*/
/*
需要修改的主要内容是适配器的内容 其次是 xml 
1、AdapterKaoqin
2、AdapterQStatus
*/
```

#### 11、优化界面

```java
/*
1、登录界面
2、注册界面
3、Fragment 主页和个人页面优化(添加动态效果)
4、签到或上传时的 进度加载条
...
*/



```

#### 12、功能添加

```java 
/*
1、短信验证码--忘记密码
2、显示所有的签到学生头像
3、个人空间的头像替换
4、修改签到识别错误的情况
5、规范 账号密码的注册格式 规范输入内容
6、人脸识别时在学生端的提醒 消息服务
7、代码更简洁 运行更稳定流畅
8、


*/
```

#### 13、完善功能

			* listview 界面设计
			* 签到信息添加人脸签到还是数字签到状态
			* 返回签到信息列表（识别结果）
			* 人脸识别坐标绘制
			* 













































