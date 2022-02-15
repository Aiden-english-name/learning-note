# 第 1 章	第一行代码

## 1.2	搭建开发环境

### 1.2.1	准备工具

- JDK	Java 语言开发工具包
- SDK  安卓开发工具包
- Android Studio

### 1.2.2	搭建开发环境

官网下载AS



#第 2 章	活动

## 2.1	活动

- 定义：包含用户界面的组件，主要用于和用户界面进行交互

## 2.2	活动的基本用法

### 2.2.1	手动创建活动

### 2.2.2	创建和加载布局

### 2.2.3	在清单文件中注册

<manifest>--><application>--><activity>-->android:label 来设置标题名称

### 2.2.4	Toast在活动中使用

Toast.maketext(context,"内容",Toast.LENGTH_SHORT).show();

### 2.2.5	在活动中使用 Menu

1. 创建 menu 文件夹

2. 创建 main 资源文件

3. 写 item 标签，设置 id，title 属性

4. 重写onCreateOptionsMene()

   ​	//得到MenuInflater对象，inflater创建当前菜单

   ​	//返回 ture，表示显示出来，false 相反

   - getMenuInflater().inflater(R.menu.main,menu)

5. 设置点击事件，重写 onOptionsItemSelected()

   - 使用 switch( item.getItemId() ) 传入点击的菜单 Id

### 2.2.6	销毁一个活动

调用 finish( ) 方法

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

## 2.4	活动的生命周期

### 2.4.1	返回栈

### 2.4.2	活动状态

1. 运行状态：看见的活动

2. 暂停状态：被遮挡的活动
3. 停止状态：完全看不见的活动
4. 销毁状态：从返回栈中移除的活动



### 2.4.3	活动的生存期

1. onCreate( )	第一次创建
2. onStart( )       不可见变为可见
3. onResume( )       处于栈顶，准备交互
4. onPause( )      被遮挡
5. onStop( )      完全不可见
6. onDestroy( )       销毁之前
7. onRestart( )       由不可见到可见之前



三种生存期

- 完整生存期:onCreate onDestroy

- 可见生存期:onStart onStop.      处于返回栈中
- 前台生存期:onResume onPause。   日常生活中的可见



一个活动启动 onCreate--> onStart-->  onResume

另一个活动启动onPause --> onStop

另一个活动退出onRestart --> onStart --> onResume

一个 dialog 启动onPause

dialog 退出 onResume

### 2.4.4	体验活动的生命周期

dialog 对话框

1. 普通的创建活动
2. 在清单文件活动标签中设置主题属性
   - Android:theme="@style/Theme.AppCompat.Dialog"

### 2.4.5	活动被回收了怎么办

1. onSaveInstanceState( ) 保留实例状态





2. 使用 Bundle 存数据



- Bundle对象.put+++( "键值 " , 对应类型的数据 )；+++为基本数据类型

```java
@Override
protected void onSaveInstanceState(Bundle outState){
  super.onSaveInstanceState(outState);
  String tempData = "Something you just typed";
  outState.putString("data_key" , tempData);
}
```



3. 利用 onCreate 的 Bundle 对象取出数据



- Bundle对象.getString("键值")

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        if (savedInstanceState != null){
          String tempData = savedInstanceState.getString("data_key");
        }
    }
```



## 2.5	活动的启动模式

**stardard、singleTop、singleTask、singleInstance**

可以在清单文件中活动标签中设置 android : launchMode 来设置

### 2.5.1 standart

每创建一个新的活动，它就会在返回栈中入栈，处于栈顶



### 2.5.2 singleTop

如果一个活动处于栈顶，再开启一个相同的活动，则不会创建新的该活动，而是重复使用该活动



### 2.5.3 singleTask

一个返回栈中相同类型的活动只会有一个

a 开启 b ，当 b 重新开启 a 时，b会出栈



### 2.5.4 singleInstance

开启新的返回栈

a 开启 b ，b 处在新的返回栈，b 开启 c ，c 和 a 一起在第一个返回栈，c 返回 ，返回到 a ，因为a ，c在同一个返回栈，c 处在栈顶 ，a 返回，返回到 b，因为第一个返回栈已空，所以到另一个返回栈，b返回，退出应用。



## 2.6	活动的最佳实践

### 2.6.1	知晓当前是在哪一个活动

1. 创建类（不是活动），继承AppCompatActivity，重写onCreate
2. 打上Log,Log.d( "BaseActivity" , getClass( ).getSimpleName( ));获取当前实例的类名



### 2.6.2	随时随地的退出程序

```java
//集合管理类
public class ActivityCollector {
  public static List<Activity> activities = new ArrayList<>( );
  
  public static void addActivity(Activity activity){
    activities.add(activity);
  }
  
  public static void removeActivity(Activity activity){
    activities.remove(activity);
  }
  
  public static void finishAll( ){
    for(Activity activity : activities){
      if(!activity.isFinishing()){
        activity.finish();
      }
      activities.clear();
    }
  }
}
```

1. 在 onCreate 中加入活动
2. 在 onDestroy 中删除活动
3. 如果需要关闭所有活动使用 finishAll( )



使用这个来杀掉当前进程代码

android.os.Process.killProcess//杀掉当前进程(android.os.Process.myPid( )//获得进程id)

### 2.6.3	启动活动的最佳写法

```java
//在第二个活动写上这个方法来说明需要哪些参数
public static void actionStart(Context context,String data1,String data2){
  Intent intent = new Intent(context,SecondActivity.class);
  intent.putExtra("param1",data1);
  intent.putExtra("param2",data2);
  context.startActivity(intent);
}
```

```java
//第一个活动调用
按钮监听{
  SecondActivity.actionStart(FirstActivity.this,"data1","data2");
}
```



# 第 3 章	UI 开发

## 3.1	如何编写程序界面

## 3.2	常用控件的使用方法

### 3.2.1	Textview

- android:id
- android:layout_width
- android:layout_height
- android:text
- android:gravity
- android:textSize sp
- android:textColor = "#00000000" 透明度，红绿蓝

### 3.2.2	Button

- android : textAllCaps="false"来禁用英文字母自动大写转化



两种点击事件

1. 用匿名类的方式

```java
button.setOnClickListener(new View.onClickListener(){
  @Override
  public void onClick(View v){
	}
})
```



2. 用接口的方式

```java
活动名 implement View.OnClickListener{
  省略其他代码
  按钮对象.setOnClickListener(this);
  @Override
  public void onClick(View v){
    switch(v.getId()){
        cast R.id.button:
        	//
        	break;
      default:
        break;
    }
	}
}
```



### 3.2.3	EditText

- android : hint 	//一段提示性内容
- android : maxLines    //指定最大行数
- edittext对象.getText( ).toString( )

### 3.2.4	ImageView

1. drawable 没有指定分辨率，所以一般不放图片

2. 新建 drawable - xhdpi 目录
3. android : src = " @drawable/img_1"
4. 代码中更换图片 imageView对象.setImageResource(R.drawable.img_2)

### 3.2.5	ProgressBar

- android : visibility 有三个值 visible , invisible , gone
- setVisibility

```java
if(progressBar.getVisivility == View.GONE)
  progressBar.setVisibility(View.VISIBLE);
else
  progressBar.setVisibility(View.GONE);
```

- setmax 设置进度条最大值
- getProgress()
- setProgress(数值)
- style=""可以改成水平进度条

### 3.2.6	AlertDialog

```java
AlertDialog.Builder dialog = new AlertDialog.Builder (MainActivity.this);
dialog.setTitle("");
dialog.setMessage("");
dialog.setCancelable(false);
dialog.setPositiveButton("OK",new DialogInterface.
                        OnClickListener(){
                          @Override
                          public void onClick(DialogInterface dialog,int which){
                            
                          }
                        });
dialog.setNegativeButton("NO",new DialogInterface.
                        OnClickListener(){
                          @Override
                          public void onClick(DialogInterface dialog,int which){
                            
                          }
                        });
dialog.show();

```



### 3.2.7	ProgressDialog

```java
ProgressDialog progressDialog = new ProgressDialog(MainActivity.this);
progressDialog.setTitle("");
progressDialog.setMessage("");
progressDialog.setCancelable(true);
progressDialog.show();
```



## 3.3	详解 4 种基本布局

### 3.3.1	线性布局LinearLayout

- android : orientation
- layout_gravity
- Android:layout_weight

### 3.3.2	相对布局RelativeLayout

相对父布局

- android : layout_alignParentLeft
- android : layout_alignParentRight
- android : layout_alignParentTop
- android : layout_alignParentButton
- android :  layout_alignParentInParent

相对其他控件边缘对齐

- android : layout_aligntLeft
- android : alignRight
- android : layout_alignTop
- android :  layout_alignButton

相对其他控件摆放

- android : above
- android : toLeftOf
- android : toRightOf
- android : below



### 3.3.3	帧布局FrameLayout

默认放在左上角



### 3.3.4	百分比布局

## 3.5	ListView

### 3.5.1	ListView 的简单用法

1. 放置 ListView 组件
2. 准备 String [ ] data 数据
3. 给适配器赋值

```java
ArrayAdapter<String> adapter = new ArrayAdapter<String>(MainActivity.this,子项布局, data);
  //androidx.appcompat.R.layout.support_simple_spinner_dropdown_item系统自带的布局，只有一个textview
```

4. 获取listview实例，设置适配器 listview.setAdapter( adapter );



### 3.5.2	定制 ListView 的界面

1. 定义实体类，作为适配器的适配类型 Fruit

```java
public class Fruit {

    private String name;

    public Fruit(String name){
        setName(name);
    }



    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```



2. 为listview的子项自定义布局
3. 自定义适配器继承ArrayAdapter指定泛型

```java
public class FruitAdapter extends ArrayAdapter<Fruit> {

    private int resourceId;
	
 		//重写构造方法用来将上下文，子项布局id，资源加载进来 
    public FruitAdapter(@NonNull Context context, int resource, @NonNull List<Fruit> objects) {
        super(context, resource, objects);
        resourceId = resource;
    }

  	//当滚到屏幕中央时getView被调用
    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        //得到Fruit实例
      	Fruit fruit = getItem(position);
      	View view = LayoutInflater.from(getContext()).inflate(resourceId,parent,false);
        Textview Fruitname = (TextView)view.findViewById(R.id.fruit_name);
      	Fruitname.setText(fruit.getname());
        return view;
    }
  
}

```

4. 修改 MainActivity 代码

```java
public class MainActivity extends AppCompatActivity {

    private List<Fruit> fruitList = new ArrayList<>();
  
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initFruits();
        FruitAdapter fruitAdapter = new FruitAdapter(MainActivity.this,R.layout.fruit_item,fruitList);
        ListView listView = (ListView) findViewById(R.id.listview);
        listView.setAdapter(fruitAdapter);
    }

    private void initFruits() {
        for(int i = 0;i<3;i++){
            Fruit apple = new Fruit("Apple");
            fruitList.add(apple);
            Fruit banana = new Fruit("Banana");
            fruitList.add(banana);
            Fruit orange = new Fruit("Orange");
            fruitList.add(orange);
            Fruit watermelon = new Fruit("Watermelon");
            fruitList.add(watermelon);
            Fruit pear = new Fruit("Pear");
            fruitList.add(pear);
            Fruit grape = new Fruit("Grape");
            fruitList.add(grape);
            Fruit pineapple = new Fruit("Pineapple");
            fruitList.add(pineapple);
            Fruit strawberry = new Fruit("Strawberry");
            fruitList.add(strawberry);
            Fruit cherry = new Fruit("Cherry");
            fruitList.add(cherry);
        }
    }
}
```



### 3.5.3	提升 ListView 运行效率

利用converView，这个参数将加载好的布局进行缓存，以便之后重用，解决重复加载布局问题

```java
@Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        //得到Fruit实例
      	Fruit fruit = getItem(position);
      	if(convertView == null){
            view = LayoutInflater.from(getContext()).inflate(resourceId,parent,false);
            
        }else {
            view = convertView;
           
        }
        Textview Fruitname = (TextView)view.findViewById(R.id.fruit_name);
      	Fruitname.setText(fruit.getname());
        return view;
    }
```



利用ViewHolder 来解决重复获取实例

用内部类viewholder来存实例，然后再用view.setTag存到view里，用的时候通过view.getTag()取出

```java
 @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        Fruit fruit = getItem(position);
        View view ;
        ViewHolder viewHolder;
        if(convertView == null){
            view = LayoutInflater.from(getContext()).inflate(resourceId,parent,false);
            viewHolder = new ViewHolder();
            viewHolder.fruitName = (TextView) view.findViewById(R.id.fruit_name);
            view.setTag(viewHolder);
        }else {
            view = convertView;
            viewHolder  = (ViewHolder) view.getTag();
        }
        viewHolder.fruitName.setText(fruit.getName());
        return view;
    }

    class ViewHolder{
        TextView fruitName;
    }
```



### 3.5.4	ListView 的点击事件

```java
//Main...代码中
listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Fruit fruit = fruitList.get(position);
                Toast.makeText(MainActivity.this,fruit.getName(),Toast.LENGTH_LONG).show();
            }
        });
```

使用RecyclerView时每一个列表项占满整个屏幕问题

将item 布局的width 和 height属性设置为wrap_content即可



## 3.6	RecyclerView

### 3.6.1	RecyclerView 的基本用法

1. 设置 androidx.recyclerview.widget.RecyclerView 控键

2. 写adapter 继承 RecyclerView.Adapter指定泛型为<FruitAdapter.ViewHolder>

   ```java
   public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {
     重写三个方法，一个构造方法，一个ViewHolder内部类 继承 RecyclerView.ViewHolder
     	//数据实例
       private List<Fruit> mFruitList;
   		
     	获取控件
       static class ViewHolder extends RecyclerView.ViewHolder{
           TextView fruitName;
   				
         	//构造方法来给属性赋值
         	public ViewHolder(View view){
               super(view);
               fruitName  = (TextView) view.findViewById(R.id.fruit_name);
           }
   
       }
   		
     	 	//构造方法来给属性赋值
       public FruitAdapter(List<Fruit> fruitList){
           mFruitList = fruitList;
   
       }
   		
     	
       @NonNull
       @Override
       public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
           View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item,parent,false);
           ViewHolder holder = new ViewHolder(view);
           return holder;
       }
   		
     	//列表展示出来的时候调用
       @Override
       public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
           Fruit fruit = mFruitList.get(position);
           holder.fruitName.setText(fruit.getName());
       }
   
     	//项目条数
       @Override
       public int getItemCount() {
           return mFruitList.size();
       }
   }
   ```

3. 代码中的准备

```java
1.初始化数据，传入适配器
2.获取RecyclerView实例，设置布局类型
private List<Fruit> fruit = new ArrayList<>();

onCreate{
  initFruits();
  FruitAdapter adapter = new FruitAdapter(fruitList);
  RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recyclerView);
  LinearLayoutMannager linearLayoutManager = new LinearLayoutMannager((context)this);
  recycleView.setLayoutManager(linearLayoutManager);
  recycleView.setAdapter(adapter);
}

 private void initFruits() {
        for(int i = 0;i<3;i++){
            Fruit apple = new Fruit("Apple");
            fruitList.add(apple);
            Fruit banana = new Fruit("Apple");
            fruitList.add(banana);
            Fruit orange = new Fruit("Apple");
            fruitList.add(orange);
            Fruit watermelon = new Fruit("Apple");
            fruitList.add(watermelon);
            Fruit pear = new Fruit("Apple");
            fruitList.add(pear);
            Fruit grape = new Fruit("Apple");
            fruitList.add(grape);
            Fruit pineapple = new Fruit("Apple");
            fruitList.add(pineapple);
            Fruit strawberry = new Fruit("Apple");
            fruitList.add(strawberry);
            Fruit cherry = new Fruit("Apple");
            fruitList.add(cherry);
        }
    }
```

### 3.6.2	实现横向滚动和瀑布流布局

```java
1.调整子项布局，让控件纵向排列
2.linearLayoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);
```



实现瀑布流

1. 子项布局宽度match_parent,和周围margin 5dp
2. StaggeredGridLayoutManager layoutManager = new StaggeredGridLayoutManager (3,StaggeredGridLayoutManager.VERTIVAL);
3. 增加水果名长度

```java
private String getRandomLengthName(String name){
        Random random = new Random();
        int length = random.nextInt(20) + 1;
        StringBuilder builder = new StringBuilder();
        for (int i = 0 ;i<length ;i++){
            builder.append(name);
        }
        return builder.toString();
    }
```



### 3.6.3	RecycleView 的点击事件

1. 利用onCreateViewHolder里的holder的成员属性与view写点击事件

```java
 holder.fruitView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int position = holder.getAdapterPosition();
                Fruit fruit = mFruitList.get(position);
                Toast.makeText(v.getContext(),"you clicked view" + fruit.getName(),Toast.LENGTH_SHORT).show();
            }
        });

        holder.fruitName.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                int position = holder.getAdapterPosition();
                Fruit fruit = mFruitList.get(position);
                Toast.makeText(v.getContext(),"you clicked image" + fruit.getName(),Toast.LENGTH_SHORT).show();
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



# 第 6 章	数据存储

## 6.3	SharedPreferences 存储

存

1. getSharedPreferences(指定SharedPre...名称,默认操作模式MODE_PRIVATE);//得SharedPreferences对象
2. sharePreferences对象.edit( );得到//得到SharedPreferencesEdior对象
3. editor.putInt("键值",数据);
4. editor.apply()



取

1. getSharedPreferences(指定SharedPre...名称,默认操作模式MODE_PRIVATE);
2. String a = getInt("键值",要替换的值);//没有该键值的时候替换



从文件中看存的内容

1. 打开 Devisce File Explorer(右下角竖着)
2. /data/data/包名/share_prefs/



## 6.5	LitePal 数据库

### 6.5.1	LitePal 简介

项目主页:http:///github.com/LitePalFramework/LitePal //查看最新开源库依赖

```
implementation 'org.litepal.guolindev:core:3.2.3'
2022.2.15 开源库崩了，不能用，得导 jar 包
```



### 6.5.2	配置 LitePal

1. 导包（依赖不行导jar包）
2. 配置litepal.xml文件

```java
//右击main，new，directory命名assets
//右击assets，new，files命名litepal.xml

<?xml version="1.0" encoding="UTF-8" ?>
<litepal>
    <dbname value = "BookStore" ></dbname>//数据库名

    <version value = "2"></version>//版本号，改表，加表，删表后数字加1

    <list>
       // <mapping class="com.example.test.Book"/>//添加表
       // <mapping class="com.example.test.Category"/>
    </list>
</litepal>
```

3. AndroidManifest中配置

```java
android:name="org.litepal.LitePalApplication"
```

### 

### 6.5.3	创建和升级数据库

1. 写表类，进行封装,如：

```java
public class Book  {
  //每个变量相当于表中的一列
    private int id;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

}

```

2. litepal.xml中写上 <mapping class="com.example.test.Book"/>//添加表
3. 然后代码中写按钮的点击事件，用LitePal.getDatabase();创建表，只需创建一次

注：表及内容从App Inspection看

添加表

4. 写新表列

```java
public class Category {
    private int id;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
}
```

5. litepal.xml中写上 <mapping class="com.example.test.Category"/>，版本号加1



### 6.5.4 	往LitePal 里添加数据

1. 让表继承 LitePalSupport ，才能进行CRUD操作
2. 设置按钮的点击事件

```
用表对象设置值，save一下就存储了
Book book = new Book();
book.setName("The Da Vinci Code");
book.setAuthor("Dan Brown");
book.setPages(456);
book.setPrice(16.96);
book.setPress("Unknown");
book.save();
```



### 6.5.5	使用 LitePal 更新数据

第一种，通过对已存储的对象重新save来更新

已存储包括两种，之前save的，和通过查询，查询出来的

```
Book book = new Book();
book.setName("The Lost Symbol");
book.setAuthor("Dan Brown");
book.setPages(510);
book.setPrice(19.95);
book.setPress("Unknown");
book.save();
book.setPrice(10.99);
book.save();
```

第二种，用book实例，调用set方法设置要更新的值，最后用updateAll执行更新操作，并且指定要更新的对象。

```
Book book = new Book();
book.setAuthor("Jiang");
book.setPrice(15);
book.updateAll("author = ? and price = ?","Dan Brown","10.99");
```

表中的数据的默认值

- int 0
- String null
- boolean false

要是想把数据更新成默认值

```
Book book = new Book();
book.setToDefault("pages");//指定列设置为默认值
book.updateAll();//updateAll()方法不指定对象，则更新所有值
```



### 6.5.6	删除 LitePal 

```
LitePal.delete(Book.class,22); 删除指定行 （class，id）
LitePal.deleteAll(Book.class); 删除某表 class
LitePal.deleteAll(Book.class,"id < ?" , "26");//删除满足条件的全部内容

如果已经存储，调用存储对象的delete()方法就可删掉
```



### 6.5.7	使用 LitePal 查询数据

```
List<Book> books = LitePal.findAll(Book.class);//查询表数据，返回列表类型
for(Book book : books){
Log.d(TAG,"author" + book.getAuthor());
}
```

- LitePal.findFirst(Book.class)//查询第一行
- LitePal.findLast(Book.class)//查询最后一行
- LitePal.select("name","author").find(Book.class);//查询哪几列
- LitePal.where("pages > ?" , "400").find(Book.class);//查询哪些条件
- LitePal.order("price (desc降序 asc升序 不写 升序)").find(Book.class)//返回结果的排序
- LitePal.limit(3).find(Book.class)//指定查询结果的数量,前三条
- LitePal.limit(3).offset(1).find(Book.class)//偏移1位，2到4条
- Liteal.的以上查询操作可以嵌套



待补充。。。。

补充LitePal也可用原生SQL操作

```java
Cusor c = LitePal.findBySQL()方法来进行原生查询，暂时略。。。
```



# 第 8 章	多媒体

## 8.1	将程序运行到手机上

USB 调试选项

## 8.2	使用通知

### 8.2.1	通知的基本用法

**需要版本判断**

（大于安卓8）

1. 使用**NotificationManager**创建一个通道
2. **创建一个NotificationChannel（通知通道）对象**
3. 编写通知**Notification**，把**通道id**传入到第二个参数
4. 调用**NotificationManager**的notify()方法发送通知

（小于安卓8）

1. 使用**NotificationManager**
2. 编写通知**Notification**
3. 调用**NotificationManager**的notify()方法发送通知

System.currentTimeMillis() 系统当前时间，毫秒为单位

```java
NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {

            String id = "channel_1";
            String name = getString(R.string.app_name);
            NotificationChannel notificationChannel =
                    new NotificationChannel(id, name, NotificationManager.IMPORTANCE_HIGH);

            manager.createNotificationChannel(notificationChannel);

            Notification notification = new NotificationCompat.Builder(this, id)
                    .setContentText("hello world")
                    .setContentTitle("This is a test")
                    .setWhen(System.currentTimeMillis())
                    .setSmallIcon(R.mipmap.ic_launcher)
                    .build();
            manager.notify(1, notification);
        } else {
            Notification notification = new NotificationCompat.Builder(this)
                    .setContentText("hello world")
                    .setContentTitle("This is a test")
                    .setWhen(System.currentTimeMillis())
                    .setSmallIcon(R.mipmap.ic_launcher)
                    .build();
            manager.notify(1, notification);
        }


```

- setAutoCancel(boolean boolean) 设置点击通知后自动清除通知



- **NotificationManager.cancel(int id) 设置点击通知后自动清除通知**,这是在活动2设置，id为之前给通知设置的id



- 使用 PendingInent 设置延时 intent

```java
Intent intent = new Intent(context,class);
PendingIntent pendingIntent = PendingIntent.getActivity(context,0,intent, PendingIntent.FLAG_IMMUTABLE)
  
notification的连缀setContentIntent(pendingIntent)
```

```
 PendingIntent.FLAG_IMMUTABLE//flag需要用到这个不然会闪退
```





### 8.2.2	???通知的进阶技巧

- setSound(Uri.fromFile(new File("音频路径")))-->用内容提供器先确定路径
- setVibrate(new long[]{0,1000,1000,1000})

静止，震动一秒，静止一秒，震动一秒

权限VIBRATE

- setLight(颜色,亮起的时长，暗去的时长)
- setDefaults(NotificationCompat.DEFAULT_ALL)



### 8.2.3	通知的高级功能

- ```
  notification.setStyle(new NotificationCompat.BigTextStyle().bigText(String)
  ```

- ```
  notification.setStyle(new NotificationCompat.BigPictureStyle().bigPicture(BitmapFactory.decodeResource(getResources(), R.drawable.tupian)))
  ```

- 在通道创建的构造方法的第三个参数设置重要程度

  - PRIORITY_MIN
  - PRIORITY_LOW
  - PRIORITY_HIGH
  - PRIORITY_DEFAULT





# 补充

## 1. 安卓版本判断

软件运行的硬件版本和（SDK26比较，安卓8）

 if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {

}else{

}

## 2.drawable 转化为bitmap

Resources res=getResources(); Bitmap bmp=BitmapFactory.decodeResource(res, R.drawable.sample); 

## 3.Attempt to invoke virtual method 'void android.widget.Button.setOnClickListener(android.view.View$OnClickListener)'

有可能是忘写setContentView()



## 4.导入开源库的时候出现 Failed to resolve （开源库）错误

有可能是该开源库钱充少了，已经被服务器删除，这时候要自己导入第三方jar包

1. 去github上找最新的开源库，能找到作者最好，下载jar包
2. 复制到test ， app ，libs 里
3. 右击add to Library-->ok
4. 然后jar包被解析可以展开说明就可以用了

