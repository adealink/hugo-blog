+++
weight = 5
title = "Lottie源码分析"
date = "2017-02-18T11:47:57+08:00"
toc = true
next = "/next/path"
prev = "/prev/path"

+++

## LottieComposition

根据`After Effects/Bodymovin`生成的.json数据文件生成的动画数据Model，将用来动画的创建。它被`LottieAnimationView`和`LottieDrawable`使用。

关键代码部分在`static LottieComposition fromJsonSync(Resources res, JSONObject json)`方法中，其中将.json数据解析成LottieComposition，该方法中调用了Layer的`static Layer fromJson(JSONObject json, LottieComposition composition)`方法，来解析图层数据。

## Layer

.json文件中`layers`数组中一个json对象解析生成的数据对象，代表一个图层。

## LottieDrawable

其可用在任何可使用Drawable的地方，展示一个Lottie动画。

如果有mask（遮罩）和matte（蒙版），当使用完后必须调用`recycleBitmaps()`，否则将会造成内存泄露。

它与LottieAnimationView结合使用更加完美，因为LottieAnimationView处理了bitmap回收和compositions的异步加载。

其中使用了ValueAnimator控制动画执行。

其主要实现了根据LottieComposition中动画数据创建各图层视图AnimatableLayer（Drawable），并根据动画信息控制各图层的绘制的功能。

#### 根据LottieComposition中动画数据创建各图层视图AnimatableLayer（Drawable）

主要在方法`void buildLayersForComposition(LottieComposition composition)`中实现。



#### 根据动画信息控制各图层AnimatableLayer的绘制

通过ValueAnimator控制和Drawable的`draw(@NonNull Canvas canvas) `方法绘制实现。其中`draw(@NonNull Canvas canvas) `中会对各图层遍历绘制。

### AnimatableLayer

