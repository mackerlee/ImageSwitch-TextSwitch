# ImageSwitch-TextSwitch

Lesson 90
1.ImageSwitcher:是android中控制图片展示效果的一个控件，如幻灯片效果,粗略的理解就是ImageView的选择器.
  --原理：ImageSwitcher有两个子View:ImageView，当左右滑动的时候，就在这俩个ImageView之间来回切换显示图片;既然有两个ImageView，
          那么我们要创建两个ImageView给ImageSwitcher，创建ImageSwitcher是通过工厂来实现的：ViewFactory.
  
