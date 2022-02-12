#第 2 章	活动

## 2.3	使用 Intent 在活动间穿梭

### 2.3.1	使用显式 Intent

```java
Intent(Context packageContext,Class<?>cls)//意图
 // 第一个参数是提供一个启动活动的上下文
 // 第二个参数是想要启动的目标活动
  
 // 启动活动 
startactivity(intent);
```

### 2.3.2	使用隐式 Intent

```java
//在<activity>标签下配置<intent-filter>内容
//指定action和category
new Intent(action);
//category使用默认值，开启活动时自动存入intent,android.intent.category.DEFAULT
  	
	//action只能指定一个
  //category可以指定多个
  对象.addCategoty();

```



### 2.3.3	更多隐式 Intent 使用

```java
打开网页
//指定意图action为Intent.ACTION_VIEW-->系统内置的action
intent对象.setDate(Uri.parse(网址字符串))//传入Uri
//Uri.parse将网址字符串转化为Uri
  
  //设置Intent-filter标签下date标签
  //指定scheme属性为http
  
打开拨号界面
//内置action为Inent.ACTION_DIAL
setDate("tel:Uri/电话号码");
```



### 2.3.4	向下一个活动传递数据

```java
启动代码省略
intent对象.putExtra(键值,数据);

//下一个活动
Intent intent = getIntent();
String date = getStringExtra(键值);
//get(Boolean/String/Int...)Extra(键值);
```



### 2.3.5	返回数据给上一个活动

```java
//活动1
Intent intent = new Intent(Context,class);//获得意图对象
startActivityForResult(意图,请求码)
//请求码用于回调中判断数据来源
  
  //活动2
  Intent intent = new Intent();
	intent.putExtra(键值,数据);
//用于向上一个活动返回数据
//第一个参数用于向上一个参数返回处理结果
//第二个参数把带有数据的Intent传递回去
	setResult(RESULT_OK,intent);
  
通过点击按钮返回
//活动1
//写回调函数onActivityResult()
//第一个参数为我们启动活动时候写的请求码
//第二个参数为返回数据时传入的处理结果
//第三个参数为携带返回数据的intent
@Override
protected void onActivityResult(int requestCode,int resultCode,Intent data){
  switch(requestCode){
    case 1:
      if(resultCode == RESULT_OK){
        String returnedData = data.getStringExtra("data_return");
			}
  }
}


下面是通过按下Back键返回
  在第二个活动
  //当按下Back键就会返回结果
  @Override
  public void onBackPressed(){
  上面第二个活动的操作
    创建意图
    存入数据
    设置结果
    销毁活动
}
```

# 第 5 章 广播

## 5.1	广播机制简介

广播类型

- 标准广播：发送出去的广播所有接收器同时收到
- 有序广播：发送出去的广播广播接收器按顺序接收，可以截断

### 5.2	接收系统广播

#### 5.2.1	动态注册广播

当注册广播的组件打开才能接收

```java
//监听网络状态改变的广播
public class MainActivity extends AppCompatActivity {
    NetworkChangeReceiver networkChangeReceiver;

    private IntentFilter intentFilter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
//用意图过滤器来指定接收的广播类型
        intentFilter = new IntentFilter();
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
        networkChangeReceiver = new NetworkChangeReceiver();
      //注册广播
        registerReceiver(networkChangeReceiver,intentFilter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
      //取消广播
        unregisterReceiver(networkChangeReceiver);
    }
//定义内部广播类继承广播，重写onReceiver方法-->注册广播
    class NetworkChangeReceiver extends BroadcastReceiver{

        @Override
        public void onReceive(Context context, Intent intent) {
            ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if ( networkInfo != null && networkInfo.isAvailable()){
                Toast.makeText(context,"network is available" , Toast.LENGTH_SHORT).show();

            }else
            {
                Toast.makeText(context,"network is unavailable" , Toast.LENGTH_LONG).show();
            }

        }
    }
}
```

#### ???5.2.2	静态注册广播

虚拟机 25以上不能静态注册

不用它，漏洞太多

未打开应用就能接收（未成功）

```java
实现开机广播
//new -> Other -> Broadcast
创建广播类
//清单文件中
  加入权限
```

### 5.3	发送自定义广播

#### 5.3.1	发送标准广播

未实现

```java
1.动态注册广播，设置意图
2.代码中用按钮发送广播
Intent intent = new intent("……");
sendBroadcast(intent);
```



#### 5.3.2	发送有序广播

```java
设置优先级
  intentfilter.setPriority(100);
发送有序广播
  sendOrderedBroadcast(intent , null);
拦截广播(onReceive)
  abortBroadcast();
```



### 5.4	使用本地广播



优点：不会泄漏自己的内部资料

```java
//需要用到LocalBroadcastManager来注册和发送广播
调用LocalBroadcastManager.getInstance(context)来获得实例
调用xx.registerReceiver(MyBroadcast,intentfilter)来注册广播
调用xx.sendBroadcast(intent)发送广播
调用xx.unregisterReceiver(MyBroadcast,)取消注册
 
```