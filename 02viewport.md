移动端手机的尺寸和分辨率千变万化，我们如何去设计页面的大小？在移动端给元素赋予固定的像素值，但奇怪的是在不同手机里看起来都差不多，不需要根据手机的尺寸去适配，这又是为什么呢？

##1、先来看看各种单位
px、pt、pc、sp、em、rem、dp、dip、ppi、dpi、ldpi、mdpi、hdpi、xhdpi、xxhdpi……

cm、mm、pt（磅）是绝对长度单位，长度是固定的物理值  
px、em、rem是相对长度单位，在不同的环境下的物理长度不同

ppi：pixels per inch，屏幕上每英寸可以显示的像素点的数量，即屏幕像素密度。  
dpi：dots per inch，打印机可以在一英寸内打多少个点。当dpi的概念用在计算机屏幕上时，就称之为ppi。Android比较喜欢使用dpi，IOS比较喜欢使用ppi。
dp、dip：dp和dip都是Density Independent Pixels的缩写，密度独立像素，可以想象成是一个物理尺寸，使同样的设置在不同手机上显示的效果看起来是一样的。

在Android中，规定以160dpi为基准，1dp=1px。如果密度是320dpi，则1dp=2px，以此类推。也就是说，同样是1dp，在像素密度大的设备上，多占用几个像素，使得最终的物理尺寸基本一致。  
Android和IOS都会通过转换系数让控件适应屏幕的尺寸。一个按钮给了44x44dp的大小，在160dpi密度的时候，按钮就是44x44px大小；在320dpi密度的时候，按钮就是88x88px的大小。不需要我们去书写多套尺寸。

sp：scale independent pixels，用法与dp类似，是专门用来定义文字大小的，受用户android设备字体设置的影响。

px：就是通常所说的像素，使网页设计中使用最多的长度单位。将显示器分成非常细小的方格，每个方格就是一个像素。（网页重构中使用的px和屏幕分辨率的px不一定是一样的大小。）  
我们重构移动页面的时候使用px其实跟安卓开发中使用dp是一样的，有个背后的系数会帮我们把数值适配到这款手机的大小。

那么这个系数是怎么来的呢？对角线分辨率/屏幕的尺寸 => 屏幕的像素密度，根据像素密度的区间，转换为对应的系数。160dpi 为1x倍， 320dpi为2x， 480dpi为3x。


##2、什么是viewport
viewport可以理解为设备上可以用来显示网页的区域，它可能比浏览器的屏幕大（出现滚动条），也可能比浏览器的屏幕小。默认情况下都是要大的，因为这样可以显示桌面版的网页，我们可以通过平移和缩放来查看整个页面。
iPhone的默认viewport为980px。我们可以手动设置viewport
`<meta name="viewport" content="width=device-width, initial-scale=1.0 maximum-scale=1.0, user-scalable=no" />`
分别设置了宽度、初始缩放值、最大缩放值、是否允许用户进行缩放。

##3、css中的1px并不等于设备的1px
css中常用px作为单位，在桌面浏览器中css的1px代表显示器的1个物理像素，但css中的像素并不等同于物理像素，在不同的设备和环境中，1px所代表的物理像素是不同的。iPhone3的屏幕分辨率为320x480，1px等于一个物理像素，iPhone4的屏幕分辨率提高了一倍，变成了640x960，但是屏幕的尺寸并没有变化。