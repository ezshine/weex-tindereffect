# weex-tindereffect
based on weex to make card stack and drag effect  
![weex-tindereffect](weex-tantan.gif)  
其实是一个挺简单的效果，各种框架下都有类似的源码。但由于weex内没有z-index等，所以实现起来还是挺麻烦的。    
真机效果很流畅，大家可尽情使用。

		1、完全基于pan事件实现，实现了手滑方向的判定，以及扔出的判定。
		2、计算了拖动的加速度，clone的朋友们可以改变animation里的duration寻找更合适的缓冲效果。
		3、三卡片层叠，可重用。卡片的内容在交换时可更新下一张卡的内容，有注释。

thanks for eros , the best solution to build iOS/Android apps.