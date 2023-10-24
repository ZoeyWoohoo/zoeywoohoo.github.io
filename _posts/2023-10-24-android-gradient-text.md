---
title: Android 渐变色昵称实现探索
date:  2023-10-24 21:51:00 +0800
category: Android
tags: [Android]
excerpt: 本文将探究并实现 Android 的渐变色 Span
reward: true
---

## 需求场景

想要对拥有特定特权用户增加昵称渐变的权益，以彰显个性化的昵称标识。这个场景历史上也讨论过，但因为昵称换行不好处理，所以就搁置了，如今产品和设计同学又提出了这个想法。于是调研了市面上的竞品，却也没有发现哪家竞品有渐变昵称，最多的是单色的高亮昵称。难道就真的无法实现吗？实现的难点在哪里？

## 设计中的渐变色

我们可以先从设计师的视角看看渐变色文本是怎么实现的，打开 Figma 添加一段文本，在文本的颜色填充中选择渐变色，而通过调整控制点「起点」和「终点」位置，可调整渐变的范围，甚至可以添加多个中途点实现多色渐变：

![Figma 渐变色文本](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/FigmaGradientColorText.png)

可以看出 Figma 中对于文本渐变色的处理，不管是「整个段落级别」还是「指定某段文本级别」，都可以看做是在文本上层绘制了一个渐变色矩形块，然后与文本混合渲染：

![Figma 渐变着色](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/FigmaGradientShader.png)

而在超出控制点范围外的区域，其策略是使用控制点处的单色延伸填充：

![Frame 渐变延伸方式](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/FrameGradientTileMode.png)

如果出现文本换行，这里有简单的两种策略：「另起一行时重头开始渐变」、「另起一行时连续渐变」。重头开始渐变好理解一点，就是每行的文本单独渐变处理：

![Figma 换行重复渐变](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/FigmaBreakRepeat.png)

但是作为选中的一段文本，换行后的连续渐变又如何实现？符合直觉的逻辑是对选中的文本单行铺平，渐变后再进行换行，那 Figma 里如何去实现呢？其实可以按照直觉的逻辑做一些特殊的处理，同样理解成文本单行铺平，对另起一行的文本实现同样一段渐变着色：

![Figma 换行连续渐变](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/FigmaBreakContinue.png)

## Android 中的渐变色

### 文本画笔着色器

Android 中想要实现文本的渐变，可以通过 TextPaint 指定着色器来实现：

```kotlin
val gradientColors = intArrayOf(
    Color.parseColor("#32C1FE"),
    Color.parseColor("#AB1EED")
)
textView.paint.shader = LinearGradient(
    0F, 0F, 100.dpFloat, 100.dpFloat,
    gradientColors, null, Shader.TileMode.CLAMP
)
```

> 需要注意的是：对画笔设置着色器 Shader 后，通过 setShadowLayer() 方法设置的文字投影效果将失效

这里创建了一个线性渐变着色器，指定了：

- 渐变范围坐标，「起点」和「终点」的坐标位置为 (0, 0) 到 (100dp, 100dp)
- 渐变颜色数组，可传入多个颜色的数组实现多色渐变
- 渐变颜色位置数组，与颜色数组对应，范围 [0, 1.0]，为空时默认均分，如两色为 [0, 1.0]，三色为 [0, 0.5, 1.0]
- 渐变渲染模式，有四种模式: CLAMP、REPEAT、MIRROR、DECAL

四种模式下 Android 的最终渲染效果如下：

![Android 着色器渐变](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/AndroidGradientShader.png)

对四种模式的定义为：

- CLAMP：超出范围外，以边界颜色填充（与 Figma 相同）
- REPEAT：超出范围外，重复渐变
- MIRROR：超出范围外，重复渐变但颜色方向会镜像交替替换
- DECAL：超出范围外，不绘制，透明处理

可能描述不够直白，这里结合真机渲染效果，模拟上层渐变色块的四种模式状态（色块按模式无限延伸）：

![Android 着色器渐变模式](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/AndroidGradientTileMode.png)

也就是说，这里针对画笔设置着色器实现渐变，是对整个 TextView 文本内容生效的，而不能指定某一段文本，这个时候会想到用 Span 去实现指定文本的渐变效果。

### 自定义 Span

在 Android 中 Span 可以用来修改「字符级」或「段落级」的文本样式，其一般都是通过操作 TextPaint 和 Canvas 来达成想要的效果样式。在渐变昵称的这个场景下，使用的是「字符级」的样式修改，就如同我们常用的前景色 ForegroundColorSpan 一样，继承 CharacterStyle 类及实现 UpdateAppearance 接口，在 updateDrawState() 回调方法中我们可以获取到文本绘制的画笔，设置相应的渐变着色器就可以了：

```kotlin
class GradientColorSpan : CharacterStyle(), UpdateAppearance {

    private val gradientColors = intArrayOf(
        Color.parseColor("#32C1FE"),
        Color.parseColor("#AB1EED")
    )
    
    override fun updateDrawState(tp: TextPaint?) {
        tp?.shader = LinearGradient(
            0F, 0F, 100.dpFloat, 100.dpFloat,
            gradientColors, null, Shader.TileMode.REAPEAT
        )
    }
}
```

我们尝试给其中一段文本应用上 Span：

```kotlin
content.text = buildSpannedString {
    append("渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐")
    inSpans(GradientColorSpan()) {
        append("渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐")
    }
    append("渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐渐")
}
```

同样一段文本，我们可以看看直接给 TextView 设置着色器和 Span 里设置着色器的区别：

![Android Span 着色器](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/AndroidSpanShader.png)

其实从这里可以看出，Android 中的渐变着色器与设计软件 Figma 中的渐变实现，基本上如出一辙，其原理也是相同的，也就是说这里最关键的点，就是渐变着色器中指定的「起点」和「终点」坐标，如果想要实现从指定文本头开始渐变（假设是从左上角到右下角的渐变），就需要把起点设置在指定文本开始第一个字符的左上角，但是终点又应该在哪呢？

1. 如果指定的文本段落在同一行，那么终点坐标设置在最后一个字符右下角即可
2. 如果指定的文本段落不在同一行，产生了换行行为，单靠这个方法我们的预期效果无法实现

### 实现难点

呼应文章开头，这里结合场景及实现效果，总结下目前实现的难点：

1. 设置着色器后，通过 setShadowLayer() 方法设置的文字投影会失效，而现有场景下需要对昵称设置投影
2. 设置着色器的起点终点坐标，需要知道确切的文本段落起始、终止的坐标信息
3. 因为昵称前面会有一些装扮标签，昵称难以避免会有换行行为，而目前单一的 Span 处理方式不能解决换行问题

## 实现探索

### 实现方案

对于换行的渐变处理，回过头来重新看设计视角分析的渐变色实现，其实已经给出了解决方案，对于换行的文本段落，无论是重新渐变还是连续渐变，都需要将换行后的文本片段单独处理，重新设置新的着色器：如果是重新渐变，重新指定起点、终点为片段头尾字符的坐标即可；如果是连续渐变，则需要计算出片段前后文本占据的空间，将起点、终点设置为同行情况下文本头尾字符坐标。

![Android 实现方案](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/AndroidSolution.png)

此外，还有投影问题的处理，既然 TextView 对这两个效果不可兼得，那如果绘制两个一模一样的文本，投影文本在下，渐变文本在上叠加起来呢？这好像是一个解决的方法，既然需要对换行后的文本分段处理，那只需将每段文本都使用双层叠加绘制就好了。方向有了，思路也就清晰了，目前需要解决的是，如何通过指定的 Span 获取换行后的文本片段，以及各文本片段在 TextView 中的起点、终点位置坐标。

### 实现方式

基于以上总结的实现方案，这里给出了一种解法。总的来说，需要一个用于指定文本范围位置的渐变标记 Span，记录了渐变的参数信息，其中换行策略包括重新渐变和连续渐变：

```kotlin
class LiveGradientColorTagSpan @JvmOverloads constructor(
    val gradientColor: IntArray,
    val gradientPositions: FloatArray? = null,
    val gradientStrategy: GradientBreakStrategy = GradientBreakStrategy.BREAK_RESTART
) : CharacterStyle(), UpdateAppearance {

    init {
        require(gradientColor.size > 1) {
            "gradient color array length must great than 1"
        }

        require(!(gradientPositions != null && gradientPositions.size != gradientColor.size)) {
            "color and position arrays must be of equal length"
        }
    }

    override fun updateDrawState(tp: TextPaint?) {
        // no-op
    }
}
```

其次还需要一个对换行分段后的片段文本绘制的 Span，而这里选择使用 Canvas 绘制双层文本实现，创建出 Drawable 后用 ImageSpan 去替换原本的文本片段，其伪代码如下：

```kotlin
class LiveGradientColorSpanDrawable @JvmOverloads constructor(
    private val text: String, specificWidth: Int = 0
) : Drawable() {

    /**
     * 绘制文本的宽
     */
    private val textMeasureWidth: Int = if (specificWidth > 0) {
        specificWidth
    } else {
        sourcePaint.measureText(text).toInt()
    }

    // ...

    override fun draw(canvas: Canvas) {
        // ...

        // 绘制文本及阴影
        canvas.save()
        canvas.drawText(text, x, y, shadowPaint)
        canvas.restore()

        // 绘制文本及渐变
        canvas.save()
        canvas.drawText(text, x, y, gradientPaint)
        canvas.restore()
    }
}
```

而 ImageSpan 作为一个整体，当一行剩余空间不够时，会被整体换行，所以这里对于 Drawable 的尺寸大小需要非常精确，确保不会绘制出来后比原有占位文本宽度还大。Android 中 TextView 之所以复杂，其中一个原因是因为它有不同的折行策略，比如最简单的规则：符号不能置于行首、英文单词不能被换行分割等。甚至有些软件，还对中文方块字的排版进行了优化，会自动调整字间距以尽量满足两端对齐。所以使用 TextPaint 的 measureText() 方法，并不一定能准确的获得其真实绘制的宽度。其次由于换行规则的存在，我们又要获得准确的换行文本分段信息，那这些都需要在文本被布局完成后再获取，这里选择自定义 TextView，在布局后获取渐变标记 Span 信息，再进行渐变处理，其代码如下：

```kotlin
open class LiveGradientTextView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyle: Int = 0
) : AppCompatTextView(context, attrs, defStyle) {

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        // 每次重新布局前，先清空渐变 Span，因为内部实现为 drawable 替换，可能影响换行逻辑
        clearGradientColorSpan()

        super.onMeasure(widthMeasureSpec, heightMeasureSpec)
    }

    override fun onLayout(changed: Boolean, left: Int, top: Int, right: Int, bottom: Int) {
        super.onLayout(changed, left, top, right, bottom)

        // 布局完成后通过渐变标记 Span 获取位置信息，并填充渐变 Span
        (text as? Spannable)?.also { spannable ->
            spannable.getSpans(0, spannable.length, LiveGradientColorTagSpan::class.java).forEach {
                inflateGradientColorSpan(spannable, it)
            }
        }
    }

    private fun clearGradientColorSpan() {
        (text as? Spannable)?.also {
            it.getSpans(0, it.length, LiveGradientColorSpan::class.java).forEach { span ->
                it.removeSpan(span)
            }
        }
    }

    private fun inflateGradientColorSpan(spannable: Spannable, tagSpan: LiveGradientColorTagSpan) {
        // ...
    }
}
```

这里对每个渐变标记 Span 的具体处理代码如下：

```kotlin
open class LiveGradientTextView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyle: Int = 0
) : AppCompatTextView(context, attrs, defStyle) {

    // ...

    private fun inflateGradientColorSpan(spannable: Spannable, tagSpan: LiveGradientColorTagSpan) {
        // 得到标记 Span 的起始和终止位置坐标
        val startIndex = spannable.getSpanStart(tagSpan)
        val endIndex = spannable.getSpanEnd(tagSpan) - 1
        val gradientColor = tagSpan.gradientColor
        val gradientPositions = tagSpan.gradientPositions
        val gradientBreakStrategy = tagSpan.gradientStrategy

        try {
            layout?.also {
                // 计算渐变文本头尾字符所在行号，用于计算文本折行分段
                val startLine = it.getLineForOffset(startIndex)
                val endLine = it.getLineForOffset(endIndex)
                // 渐变分段信息
                val gradientSlices = mutableListOf<GradientSliceInfo>()
                // 渐变文本的总长度
                var gradientTotalWidth = 0

                for (line in startLine..endLine) {
                    val lineStartIndex = it.getLineStart(line)
                    val lineEndIndex = it.getLineEnd(line) - 1

                    // 获取当前行的渐变文本内容并设置渐变 Span
                    val sliceStart = max(startIndex, lineStartIndex)
                    val sliceEnd = min(endIndex, lineEndIndex)

                    val sliceContent = spannable.slice(sliceStart..sliceEnd).toString()

                    // 因为文本刚好置于行尾，使用 measureText() 计算的宽度不一定准确，可能导致换行，所以需要精准计算当前布局后的宽度
                    val specificWidth: Int = if (endIndex >= lineEndIndex) {
                        min(
                            paint.measureText(sliceContent).toInt(),
                            (measuredWidth - it.getPrimaryHorizontal(sliceStart)).toInt()
                        )
                    } else {
                        it.getPrimaryHorizontal(sliceEnd + 1) - it.getPrimaryHorizontal(sliceStart)
                    }.toInt().also { sliceWidth ->
                        gradientTotalWidth += sliceWidth
                    }

                    gradientSlices.add(
                        GradientSliceInfo(
                            sliceContent, sliceStart, sliceEnd, specificWidth,
                            gradientColor, gradientPositions, null
                        )
                    )
                }

                /**
                 * 根据换行渐变策略计算出最后的渐变色和渐变位置，正确设置渐变效果
                 */
                var preTotalWidth = 0
                gradientSlices.forEach { slice ->
                    when (gradientBreakStrategy) {
                        GradientBreakStrategy.BREAK_RESTART -> {}
                        GradientBreakStrategy.BREAK_CONTINUE -> {
                            slice.updateGradientColorPosition(preTotalWidth, gradientTotalWidth)
                            preTotalWidth += slice.specificWidth
                        }
                    }

                    spannable.setGradientColorSpan(slice)
                }
            }
        } catch (e: Exception) {
            Logger.e("LiveGradientTextView", "Layout Error: ${e.printStackTrace()}")
        }
    }

    /**
     * 根据当前 slice 文本前面的文本总长度 [preTotalWidth] 和所有文本总长度 [totalWidth] 计算出渐变色值位置并更新
     */
    private fun GradientSliceInfo.updateGradientColorPosition(preTotalWidth: Int, totalWidth: Int) {
        val startPoint = Point(-preTotalWidth, 0)
        val endPoint = Point(totalWidth - preTotalWidth, 0)
        points = Pair(startPoint, endPoint)
    }

    private fun Spannable.setGradientColorSpan(gradientSlice: GradientSliceInfo) {
        setSpan(
            LiveGradientColorSpan.make(
                gradientSlice.sliceContent, paint,
                gradientSlice.gradientColor, gradientSlice.gradientPositions,
                gradientSlice.points, gradientSlice.specificWidth
            ),
            gradientSlice.startIndex, gradientSlice.endIndex + 1,
            Spanned.SPAN_EXCLUSIVE_EXCLUSIVE
        )
    }

    /**
     * 渐变文本分片信息
     */
    @Suppress("ArrayInDataClass")
    data class GradientSliceInfo(
        val sliceContent: String, val startIndex: Int, val endIndex: Int,
        val specificWidth: Int, var gradientColor: IntArray, var gradientPositions: FloatArray?,
        var points: Pair<Point, Point>?
    )
}
```

这里涉及的关键方法为 Layout 下：getLineForOffset() 获取 index 处字符所处行号、getPrimaryHorizontal() 获取 offset 处字符偏移距离。记录每一段的分片信息，在不同策略下设置不同的着色器「起点」和「终点」坐标。最终实现的效果：

![Android 实现效果](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/20231024T1430/AndroidFinalEffect.png)

> 可能会遇到的问题：给 TextView 的内容设置一个 SpannableStringBuilder 时，其内部默认会创建一个副本对象，如果外部持有这个 SpannableStringBuilder 并更新，重新设置后会丢失渐变信息，必须确保重新触发布局。