# ImageSwitch-TextSwitch

Lesson 90
1.ImageSwitcher:是android中控制图片展示效果的一个控件，如幻灯片效果,粗略的理解就是ImageView的选择器.
  --原理：ImageSwitcher有两个子View:ImageView，当左右滑动的时候，就在这俩个ImageView之间来回切换显示图片;既然有两个ImageView，
          那么我们要创建两个ImageView给ImageSwitcher，创建ImageSwitcher是通过工厂来实现的：ViewFactory.
  --实例：在activity_main.xml中：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:paddingBottom="@dimen/activity_vertical_margin"
      android:paddingLeft="@dimen/activity_horizontal_margin"
      android:paddingRight="@dimen/activity_horizontal_margin"
      android:paddingTop="@dimen/activity_vertical_margin"
      tools:context="com.example.mackerlee.android_90.MainActivity">
  
  
      <ImageSwitcher
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:id="@+id/imageSwitcher90"
          android:layout_alignParentStart="true"
          android:layout_alignParentBottom="true"
  
      />
      <!-- 在xml中设置滑动效果
          android:inAnimation="@android:anim/slide_in_left"
          android:outAnimation="@android:anim/slide_out_right"-->
  </RelativeLayout>
  
  在java->包名->MainActiviy.java中：
  package com.example.mackerlee.android_90;

  import android.provider.Settings;
  import android.support.v7.app.AppCompatActivity;
  import android.os.Bundle;
  import android.view.MotionEvent;
  import android.view.View;
  import android.widget.ImageSwitcher;
  import android.widget.ImageView;
  import android.widget.ViewSwitcher;
  
  //--View.OnTouchListener:实现触屏事件接口，用于来回滑动图片
  public class MainActivity extends AppCompatActivity implements ViewSwitcher.ViewFactory,View.OnTouchListener{
  
      ImageSwitcher imageSwitcher;
      //--以下图片资源事先放在res->mipmap文件夹下，注意图片不要太大，否则会报内存溢出错误
      private Integer[] images ={
              R.mipmap.p1,  //资源名称必须以字母开头
              R.mipmap.p2,
              R.mipmap.p3,
              R.mipmap.p4,
              R.mipmap.p5,
              R.mipmap.p6
      };
      private int index = 0; //用于要显示的图片下标的标记
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          imageSwitcher = (ImageSwitcher)findViewById(R.id.imageSwitcher90);
  
          //--设置创建ImageView组件的工厂，this表示在当前类中
          imageSwitcher.setFactory(this);
          //--设置触屏事件
          imageSwitcher.setOnTouchListener(this);
      }
  
      //--ViewFactory接口方法：创建视图,该函数给ImageSwitcher里面调用，从而获取ImageView对象,
      //----给ImageSwitch组件提供两个ImageView切换图片显示用
      public View makeView() {
          ImageView imageView = new ImageView(this);
          imageView.setImageResource(images[index]);
          return imageView;
      }
  
      //--位置的坐标值是float值
      float startX = 0.0f; //开始的X坐标
      float endX = 0.0f;//结束的X坐标
      //--触屏事件接口方法：触屏事件监听方法,本例实现监听按下去的点到放开屏幕的点的距离
      public boolean onTouch(View v, MotionEvent event) {
          //--首先判断按下去的情况下：获取动作
          if(event.getAction() == MotionEvent.ACTION_DOWN){
              startX = event.getX();//获取点下去事件的X坐标
              return true;
          }else if(event.getAction() == MotionEvent.ACTION_UP){
              //--触屏收起时获取结束坐标
              endX = event.getX();
              //--判断左右滑动
              if((startX-endX)>20){ //20dp,向左滑动
                  index = (index+1<=images.length-1)?++index:0;
                  imageSwitcher.setImageResource(images[index]);
                  System.out.println("index:"+index+",len:"+images.length);
                  //--在代码中设置向左滑动动画效果,这样就不会像xml配置一样向左向右都只有一种滑动效果
                  imageSwitcher.setInAnimation(this,android.R.anim.slide_out_right);
                  imageSwitcher.setOutAnimation(this,android.R.anim.slide_in_left);
              }else if((endX-startX)>20){//20dp，向右滑动
                  index = (index - 1>=0)?--index:images.length-1;
                  imageSwitcher.setImageResource(images[index]);
                  System.out.println("index:"+index+",len:"+images.length);
                  //--在代码中设置向右滑动动画效果
                  imageSwitcher.setInAnimation(this,android.R.anim.slide_in_left);
                  imageSwitcher.setOutAnimation(this,android.R.anim.slide_out_right);
              }
              return true;
          }
          return false;
      }
  }

2.textSwitcher:是android中控制文本片展示效果的一个控件，原理和imageSwitcher一样，只是其是显示文本而已。
  --实例：在res->layout->创建新的activity_textswitcher.xml：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <TextSwitcher
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:id="@+id/textSwitcher92"
          android:layout_alignParentStart="true" />
  </RelativeLayout>
  
  在java->包名->创建MainActivity92.java:
  package com.example.mackerlee.android_90;

  import android.os.Bundle;
  import android.support.v7.app.AppCompatActivity;
  import android.view.MotionEvent;
  import android.view.View;
  import android.widget.TextSwitcher;
  import android.widget.TextView;
  import android.widget.ViewSwitcher;
  
  /**
   * Created by mackerlee on 2016/7/17.
   * TextSwitcher
   */
  public class MainActivity92 extends AppCompatActivity implements ViewSwitcher.ViewFactory,View.OnTouchListener{
      TextSwitcher textSwitcher;
      //--用于切换文本的资源
      private String[] texts = {"小白","小黑","小明"};
      private int index = 0; //用于要显示的下标
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_textswitcher);
          textSwitcher = (TextSwitcher)findViewById(R.id.textSwitcher92);
  
          //--设置创建TextSwitcher组件的工厂，this表示在当前类中
          textSwitcher.setFactory(this);
          //--设置触屏事件
          textSwitcher.setOnTouchListener(this);
      }
  
      //--位置的坐标值是float值
      float startX = 0.0f; //开始的X坐标
      float endX = 0.0f;//结束的X坐标
      //--触屏事件接口方法：触屏事件监听方法,本例实现监听按下去的点到放开屏幕的点的距离
      @Override
      public boolean onTouch(View v, MotionEvent event) {
          //--首先判断按下去的情况下：获取动作
          if(event.getAction() == MotionEvent.ACTION_DOWN){
              startX = event.getX();//获取点下去事件的X坐标
              return true;
          }else if(event.getAction() == MotionEvent.ACTION_UP){
              //--触屏收起时获取结束坐标
              endX = event.getX();
              //--判断左右滑动
              if((startX-endX)>20){ //20dp,向左滑动
                  index = (index+1<=texts.length-1)?++index:0;
                  textSwitcher.setText(texts[index]);
                  System.out.println("index:"+index+",len:"+texts.length);
                  //--在代码中设置向左滑动动画效果,这样就不会像xml配置一样向左向右都只有一种滑动效果
                  textSwitcher.setInAnimation(this,android.R.anim.slide_out_right);
                  textSwitcher.setOutAnimation(this,android.R.anim.slide_in_left);
              }else if((endX-startX)>20){//20dp，向右滑动
                  index = (index - 1>=0)?--index:texts.length-1;
                  textSwitcher.setText(texts[index]);
                  System.out.println("index:"+index+",len:"+texts.length);
                  //--在代码中设置向右滑动动画效果
                  textSwitcher.setInAnimation(this,android.R.anim.slide_in_left);
                  textSwitcher.setOutAnimation(this,android.R.anim.slide_out_right);
              }
              return true;
          }
          return false;
      }
  
      @Override
      public View makeView() {
          TextView tv = new TextView(this);
          tv.setText(texts[index]);
          return tv;
      }
  }

  在清单文件中设置启动项未MainActivity92:
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.mackerlee.android_90">
  
      <application
          android:allowBackup="true"
          android:icon="@mipmap/ic_launcher"
          android:label="@string/app_name"
          android:supportsRtl="true"
          android:theme="@style/AppTheme">
          <activity android:name=".MainActivity92">  //设置启动项Activity
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
  
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
      </application>
  
  </manifest>
  

