# Ae
##中英文切换方法：
  Win：C:\Program Files\Adobe\Adobe After Effects CC 2015.3\Support Files\zdictionaries
Mac：应用程序/Adobe After Effects CC 2015.3/Adobe After Effects CC 2015.3/右键显示包内容/Contents/Resources/zdictionaries
找到“after_effects_zh-Hans.dat”文件,重命名为after_effects_en-us.dat，打开AE就是英文版了

## 关于预合成改变颜色问题
ctrl+shirt+c,将所选图层预合成。
在时间轴上将预合成进行复制，然后改变复制体的颜色，会发现复制体和原体同时改变
reason：由于在项目窗口中，对于上述的原体和复制体都源于一个合成，因此对于时间轴上的两者的组成成分是一样的

![](https://i.imgur.com/Eu5vlN9.png)

解决方案：在项目窗口中对需要的合成进行复制，再将复制的拖放到时间轴上。



