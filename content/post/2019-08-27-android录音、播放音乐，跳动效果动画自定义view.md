---
title: Android录音、播放音乐，跳动效果动画自定义VIew
author: xfans
type: post
date: 2019-08-27T12:22:09+00:00
url: /archives/178
views:
  - 183
categories:
  - Android
tags:
  - android
  - 自定义View

---
实现了Android录音，语音输入时的竖线波纹动画效果和音乐播放时的跳动效果

## 效果

![][1]![][2] 

## 实现原理

实现了两种运动效果：

### 1.从左到右移动

假设柱状图的高度是一个对列

`27,17,38,91,38,24,8,60,38,14,8`

`8,27,17,38,91,38,24,8,60,38,14`

`14,8,27,17,38,91,38,24,8,60,38`

**从左到右移动实际上就是不断的将队列中的最后一个插入到队列的第一个。**

代码：

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-kotlin">runnable = object : Runnable {
    override fun run() {
        val last = bodyWaveList.pollLast()
        bodyWaveList.addFirst(last)
        invalidate()
        valHandler.postDelayed(this, duration);//循环调用重绘
    }
}
valHandler.post(runnable)
</code></pre>

### 2.上下跳动

假设柱状图是一个队列

`27,17,38,91,38,24,8,60,38,14,8`

上下跳动就是将队列的高度，从0%-100%变化

代码：

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-kotlin">valueAnimator.addUpdateListener {
        valueAnimatorOffset = it.getAnimatedValue() as Float
        invalidate()
}
valueAnimator.start()
</code></pre>

然后在`onDraw`绘制时`高度*valueAnimatorOffset`

## 绘制

代码：

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-kotlin">    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)

        waveList.clear()
        waveList.addAll(headerWaveList)
        waveList.addAll(bodyWaveList)
        waveList.addAll(footerWaveList)

        paintLine?.strokeWidth = lineWidth
        paintLine?.color = lineColor

        for (i in waveList.indices) {
            var startX = 0f
            var startY = 0f
            var endX = 0f
            var endY = 0f

            var offset = 1f
            if (i &gt;= headerWaveList.size && i &lt; (waveList.size - footerWaveList.size)) {//模式1 ，排除掉头尾
                offset = valueAnimatorOffset
            }

            val lineHeight = waveList[i] / 100.0 * measuredHeight * offset

            val absoluteGravity = Gravity.getAbsoluteGravity(showGravity, layoutDirection);

            when (absoluteGravity and Gravity.HORIZONTAL_GRAVITY_MASK) {//左中右的位置
                Gravity.CENTER_HORIZONTAL -&gt; {
                    val lineSize = waveList.size
                    val allLineWidth = lineSize * (lineSpace + lineWidth)
                    if (allLineWidth &lt; measuredWidth) {
                        startX = (i * (lineSpace + lineWidth) + lineWidth / 2) + ((measuredWidth - allLineWidth) / 2)
                    } else {
                        startX = i * (lineSpace + lineWidth) + lineWidth / 2
                    }
                    endX = startX
                }

                Gravity.RIGHT -&gt; {
                    val lineSize = waveList.size
                    val allLineWidth = lineSize * (lineSpace + lineWidth)
                    if (allLineWidth &lt; measuredWidth) {
                        startX = (i * (lineSpace + lineWidth) + lineWidth / 2) + (measuredWidth - allLineWidth)
                    } else {
                        startX = i * (lineSpace + lineWidth) + lineWidth / 2
                    }
                    endX = startX
                }

                Gravity.LEFT -&gt; {
                    startX = i * (lineSpace + lineWidth) + lineWidth / 2
                    endX = startX
                }
            }


            when (showGravity and Gravity.VERTICAL_GRAVITY_MASK) {//上中下的位置
                Gravity.TOP -&gt; {
                    startY = 0f
                    endY = lineHeight.toFloat()
                }

                Gravity.CENTER_VERTICAL -&gt; {
                    startY = (measuredHeight / 2 - lineHeight / 2).toFloat()
                    endY = (measuredHeight / 2 + lineHeight / 2).toFloat()
                }

                Gravity.BOTTOM -&gt; {
                    startY = (measuredHeight - lineHeight).toFloat()
                    endY = measuredHeight.toFloat()
                }

            }

            canvas?.drawLine(
                startX,
                startY,
                endX,
                endY,
                paintLine
            )

        }
    }

</code></pre>

## 地址

[https://github.com/xfans/VoiceWaveView][3]

 [1]: https://xfans.me/wp-content/uploads/2019/08/2.gif
 [2]: https://xfans.me/wp-content/uploads/2019/08/1.gif
 [3]: https://github.com/xfans/VoiceWaveView "https://github.com/xfans/VoiceWaveView"