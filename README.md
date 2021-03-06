# Apple-Auto-Layout-Tec-For-Myself
Auto Layout Guide:
https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/index.html#//apple_ref/doc/uid/TP40010853

一些常见的外部改变来源：

1.用户调整窗口的大小（OS X）。

2.用户进入或者离开Splite View，iPad（iOS）。

3.旋转设备。

4.支持不同的size classes（Compact-紧凑、Regular-正常、Any-任意）。

5.支持不同的屏幕尺寸。

大多数这类变化可能发生在运行时状态。尽管屏幕尺寸不会在运行时改变，一样可以创建一个自适应接口来允许程序运行在iphone4s、iPhone+甚至是iPad上。

内部改变来源：
一些用户界面或者是试图控件的大小发生改变

1.应用改变内容显示（文本和图像）。

2.应用支持国际化。国际化的过程使应用程序能够适应不同的语言、地区,和文化，国际化在布局上有三个主要的影响：用户界面转换语言时标签所需要的空间大小，例如德语比英语需要更多的标签空间，而日语有时候需要更少的空间；由于地区的变化，时间和日期格式也会产生微妙的变化，用户界面尺寸仍然需要适应这些变化；语言的改变不仅会影响文本的大小，也会影响组织的布局，例如英语、阿拉伯语和希伯来语的组织布局就是相反的，一个从左往右，一个从右往左。

3.应用支持动态类型（iOS）。用户可以随意更改文本大小，比如字体大小、格式等，用户界面则需要做出适应。

自动布局框架

1.使用代码来制定用户界面。
应用通过编码计算出视图层次结构上的每一个视图的frame来实现布局，而frame则定义了视图在父视图坐标系统中的origin、height和width。为了能够布局用户界面，你不得不计算视图层次结构上的每一个视图的大小和位置，如果一个发生改变，那么你不得不为每一个受影响的视图计算出frame。在许多方面，通过编码定义一个界面的frame提供了很大的灵活性和可行性，当一个变化产生的时候，你可以做任何你想要的改变。然而布局一个简单的用户界面需要相当多的努力设计、调试和维护，因为你必须要管理自己的变化，那么创建一个自适应的用户界面的困难将会是以数量级的增加。

2.使用autoresizing masks自动响应外部变化。
可以使用autoresizing msaks来帮助缓解代码布局带来的压力。autoresizing masks定义一个视图的父视图frame改变的时候，这个视图的frame该如何变化。这简化了为适应外部变化的布局创建工作。然而autoresizing masks支持一些相对较小的子集布局，对于复杂的用户界面，还是需要相应代码辅助布局。此外，autoresizing masks只适用于外部布局变化，不适用于内部布局变化。

3.使用Auto Layout
autoresizing masks仅仅是布局改进的一个迭代，auto layout是一种全新的模式。auto layout思考的不应该是视图的frame，应该是视图之间的关系。Auto Layout通过使用一系列的约束constraints来定义用户界面，约束constraints一般代表了两个视图之间的关系。基于约束的每一个视图，Auto Layout都会计算出它们的大小和位置。这样产生的布局适应内部布局和外部布局。

设计一组约束来创建特定行为的逻辑是非常不同于通过写代码或者面向对象编码的逻辑。幸运的是，掌握Auto Layout与掌握其他的编程任务并没有什么不同。第一：理解基于约束的layouts背后的逻辑;第二：学习API。在学习其他的编程任务的时候，你已经遵循了这些步骤，Auto Layout也不例。

上面所记载的文字旨在帮助我过渡到Auto Layout，下面的文字将帮助我进一步学习Auto Layout。

stack view提供了一种简单的方式来利用Auto Layout的功能，而不引入约束的复杂性。一个satck view管理用户界面元素的行或列。stack view通过属性来管理元素。

    axis：（UIStackView Only）定义了satck view的方向，水平或者垂直。轴
    
    orientation：（NSStackView Only）定义了satck view的方向，水平或者垂直。轴
    
    distribution：定义了沿着axis（轴）方向的视图集的布局。
    
    alignment：定义了垂直于stack view axis（轴）方向的视图集的布局。
    
    sapcing：定义了相邻视图的间隔。

stack view同样的对于管理元素视图的 content-hugging 和 compression-resistance 的属性起作用。

可以使用嵌套的stack view来构建复杂的视图。事实上，应该尽可能的多的使用stack view来管理布局，除非单独使用stack view已经不能达到目的，这是就可以使用约束。

注意：尽管嵌套的satck view能够构建复杂的用户界面，也避免不了使用约束来辅助构建界面，因为至少也要为最外层的stack view添加约束来获取位置。

UIStackView Class Reference: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/index.html#//apple_ref/doc/uid/TP40015256

解析约束: https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW1

视图的布局被定义成一系列的线性方程，每一个约束都代表一个方程，你的目标就是解析那些有且只有一个可能值的方程。

例子：
    RedView.Leading = 1.0 x BlueView.trailing + 8.0 

item1.Attribute1 Relationship Multiplier x Item2.Attribute2 + Constant

这个约束表示的状态是RedView的前缘必须离BlueView得后缘8.0个点。

    Item 1: RedView，这个Item必须是一个view或者一个layout guide。
    
    Attribute 1: 被约束在Item1上的属性 leading edge.
    
    RelationShip: 左右之间的关系，三个可能的值：=、<=、>=.
    
    Multiplier: attribute2被乘的浮点数值 1.0。
    
    Item2: 与Item1不同，这个值可以被留白。
    
    Attribute2: 被约束在Item2上的属性 trailing。如果Item2被留白，它一定一个属性 Not an Attribute.
    
    Constant: 常数，浮点偏移值，被加在attribute2上的值 8.0。

attribute：属性    Item：元素

大多数约束定义了用户界面上两个元素之间的关系，这些元素可以是视图（集）或者layout guide。约束可以也可以定义单一元素的两个不同属性关系，例如设置一个元素的width和height的宽高比aspect ratio。还可以常数值赋值给一个元素的width和height，这时Item2会被留白，attribute2被设置为Not An Attribute，multiplier也被置0.0。

在Auto Layout中，属性定义了一个可以被约束的功能，通常属性包括了四个边缘edge（leading、trailing、top、bottom）、宽高（width、height） 、水平和垂直的中心点（vertical and horizontal centers），可以查看NSLayoutAttribute来查看所有的属性列表。（OS X和iOS都使用NSLayoutAttribute来定义属性，它们的区别很小，使用时要注意）

多种变化的方程组合可以创建许多不同类型的约束，你可以定义视图间的空隙、定义两个视图的相对大小、甚至是定义一个视图的宽高比。然而，并不是所有的属性都是兼容的。这里有两种基本类型的属性：Size attributes（width、height）和 location attributes（Leading、trailing、top、bottom）。Size attributes指示一个元素究竟有多大，而没有任何location的信息。Location attributes指示一个元素相对于其他的位置，而没有任何元素大小的信息。

记住上面的差异，下面的规则是适用的:

You cannot constrain a size attribute to a location attribute. 

You cannot assign constant values to location attributes. 

You cannot use a nonidentity multiplier (a value other than 1.0) with location attributes. 

For location attributes, you cannot constrain vertical attributes to horizontal attributes. 

For location attributes, you cannot constrain Leading or Trailing attributes to Left or Right attributes.

    // Setting a constant height
    View.height = 0.0 * NotAnAttribute + 40.0
    	 
    // Setting a fixed distance between two buttons
    Button_2.leading = 1.0 * Button_1.trailing + 8.0
     
    // Aligning the leading edge of two buttons
    Button_1.leading = 1.0 * Button_2.leading + 0.0
    
    // Give two buttons the same width
    Button_1.width = 1.0 * Button_2.width + 0.0
    
    // Center a view in its superview
    View.centerX = 1.0 * Superview.centerX + 0.0
    View.centerY = 1.0 * Superview.centerY + 0.0
     
    // Give a view a constant aspect ratio
    View.height = 2.0 * View.width + 0.0

注意：这是方程表达式，不是简单的赋值运算。

当Auto Layout解析这些方程的时候，它并不是简单的将右边的值赋值给左边，相反，它计算出attribute1和attribute2的值来使得等式成立，这就意味着我们可以自由地重组这些方程式：

		// Setting a fixed distance between two buttons
		Button_1.trailing = 1.0 * Button_2.leading - 8.0
		 
		// Aligning the leading edge of two buttons
		Button_2.leading = 1.0 * Button_1.leading + 0.0
		 
		// Give two buttons the same width
		Button_2.width = 1.0 * Button.width + 0.0
		 
		// Center a view in its superview
		Superview.centerX = 1.0 * View.centerX + 0.0
		Superview.centerY = 1.0 * View.centerY + 0.0
		 
		// Give a view a constant aspect ratio
		View.width = 0.5 * View.height + 0.0

这些列子和上面的列子最终实现的效果是一致的。当然在重组这些方程等式的时候，你也要适当的变换约束，比如说 +8.0 变成 -8.0、 2.0* 变成 0.5* 以达到一致的效果。

你可能会发现Auto Layout经常提供多种不同方式来解决相同问题，理想情况下，你应该选择最能明显地描述你意图的解决方案。但是，不同的开发者可能会在到底哪个解决方案是最佳的问题上产生分歧。如果你只选择一种方法并且一直实践下去，从长远来看。你可能会经历更少的问题，而不是花更多的时间去纠结到底哪个是最佳方案。例如，这个教程遵从的经验法则是：

1.整数乘积要好过分数乘积；

2.正数要好多负数；

3.只要有可能，视图在布局中的出现次序应该是：从左往右、从上到下。

到目前为止，所有的列子都是作为等式来表达的约束。约束也能够表达不等式。具体来说，约束关系可以是=、>=、<=.

使用约束来定义一个视图的最大、最小尺寸Size：

		// Setting the minimum width
		View.width >= 0.0 * NotAnAttribute + 40.0
		 
		// Setting the maximum width
		View.width <= 0.0 * NotAnAttribute + 280.0
		
也可以使用两个不等式来代替一个恒等式：

		// A single equal relationship
		Blue.leading = 1.0 * Red.trailing + 8.0
		 
		// Can be replaced with two inequality relationships
		Blue.leading >= 1.0 * Red.trailing + 8.0
		Blue.leading <= 1.0 * Red.trailing + 8.0

两个不等式也不总是相当于一个等式的关系，比如上面例子中的视图width只是定义了最大、最小范围，并没有直接定义width，仍然需要添加一个合理范围内的width来定义视图的width或者中心点。

一般所有的约束都是必要的，Auto Layout必须计算出一个解决方案来满足所有的约束。如果不能满足，那么就会出现一个错误，Auto Layout会在控制台打印出相关信息，并且它会选择一个约束来终断，然后会抛开这个中断的约束再计算出解决方案。

也可以创建可选的约束，所有的约束有一个1~1000的优先级priority。约束必须要有一个1000优先级的，其他所有的约束是可选的。

当在计算解决方案的时候，Auto Layout会尝试以优先级从高到低的顺序来满足所有的约束。如果不能满足一个可选的约束，这个可选约束会被跳过，然后Auto Layout继续满足下一个约束。

尽管一个可选约束不能被满足，但是它还是会影响到布局。如果在跳过skip一个约束后出现了不太明了的布局，系统会选择最接近约束的解决方案。换句话说，不满足条件的可选约束扮演着一个把视图往它们那个结果拉取的角色。
可选的约束和不等式通常会一起起作用。例如：

    Blue.leading >= 1.0 * Red.trailing + 8.0
    Blue.leading <= 1.0 * Red.trailing + 8.0
	
这里为两个不等式提供了不同的约束，>=需要一个高优先级（1000priority）的关系，<=有一个低优先级（250priority）的关系。这就意味着Blueview和Redview之间不能少于8.0点的距离（满足高优先级>=，<=低优先级会被按顺序被满足），然而鉴于布局中的其他约束条件，其他约束可以把Blueview往远处拉。同时，可选约束会把Blueview拉向Redview这边，以尽可能地让它们之间的空隙值接近8.0点。这就是为什么可选的约束和不等式通常会一起起作用。

注意：不要勉强所有的约束优先级都是1000。实际上，属性大体被定义在low（250）、medium（500）、high（750）、required（1000）。你只需要一个或者两个优先级比这些值高一点或者低一点的约束，来防止冲突。如果除此之外，你可能需要重新审视你的布局逻辑。

内在内容大小（intrinsic content size）：

迄今为止，所有的例子都使用约束来定义视图的位置和它们的大小，然而有一些视图会根据它们当前的内容给出一个自然值。（intrinsic content size）比如按钮的intrinsic content size就是它的标题大小加上一点缩进。并不是所有的视图都有intrinsic content size，对于那些有intrinsic content size的视图而言，intrinsic content size能够定义视图的height或者width或者 height和width。

View
Intrinsic content size
UIView and NSView
No intrinsic content size.
Sliders
Defines only the width (iOS).
Defines the width, the height, or both—depending on the slider’s type (OS X).
Labels, buttons, switches, and text fields
Defines both the height and the width.
Text views and image views
Intrinsic content size can vary.

Intrinsic content size是基于视图当前的内容。一个Label或者Button的Intrinsic content size是基于显示文字的数量和所使用的字体大小。对于其他视图，Intrinsic content size甚至更加复杂。例如，一个空的image view没有Intrinsic content size，一旦你添加上了一个image，那么它的Intrinsic content size就会被设置为image的大小。

一个文本视图text view的Intrinsic content size取决于它的内容、是否开启滚动功能、应用在视图上的约束。例如，开启滚动功能，那么视图将不会有Intrinsic content size。With scrolling disabled, by default the view’s intrinsic content size is calculated based on the size of the text without any line wrapping. For example, if there are no returns in the text, it calculates the height and width needed to layout the content as a single line of text. If you add constraints to specify the view’s width, the intrinsic content size defines the height required to display the text given its width.  （这段确实不好翻译）

Auto Layout通过为每一个维度使用一对约束来展示视图的Intrinsic content size。content hugging（内容环抱力）把视图向里拉，以恰到好处地适应内容大小。 compression（压缩阻力）把视图往外推，防止视图将内容剪辑。

这些维度的约束通过不等式来定义：

		// Compression Resistance
		View.height >= 0.0 * NotAnAttribute + IntrinsicHeight
		View.width >= 0.0 * NotAnAttribute + IntrinsicWidth
		 
		// Content Hugging
		View.height <= 0.0 * NotAnAttribute + IntrinsicHeight
		View.width <= 0.0 * NotAnAttribute + IntrinsicWidth

IntrinsicHeight、IntrinsicWidth约束视图的Intrinsic content size

每一个约束都有它们自己的优先级。默认的是，视图的content hugging优先级为250，compression resistance优先级为750.因此，拉伸视图比缩小视图更容易。对于大多数的控件，都有一个期望的行为：例如可以很容易的通过Intrinsic content size来把按钮拉伸得更大，然而，想要缩小它，它的内容就可能会被剪辑掉。

注意：Interface Builder可能偶尔会修改这个属性来防止冲突。

不管怎么样，尽可能使用视图的Intrinsic content size。它可以帮助你动态的布局视图内容，也可以减少约束的创建，避免冲突，但是你需要管理视图的 content-hugging 和 compression-resistance 的优先级。

简称CHCR

> CR: 优先级越高，越晚被压缩。
> CH: 优先级越高，越晚被拉伸。

有时候在使用stack view的时候，需要修改CHCR的值，来适应视图被拉伸或是被压缩的情况。看官方的教程中有个例子

建议：

		When stretching a series of views to fill a space, if all the views have an identical content-hugging priority, the layout is ambiguous. Auto Layout doesn’t know which view should be stretched.  A common example is a label and text field pair. Typically, you want the text field to stretch to fill the extra space while the label remains at its intrinsic content size. To ensure this, make sure the text field’s horizontal content-hugging priority is lower than the label’s.  In fact, this example is so common that Interface Builder automatically handles it for you, setting the content-hugging priority for all labels to 251. If you are programmatically creating the layout, you need to modify the content-hugging priority yourself. 
		Odd and unexpected layouts often occur when views with invisible backgrounds (like buttons or labels) are accidentally stretched beyond their intrinsic content size. The actual problem may not be obvious, because the text simply appears in the wrong location. To prevent unwanted stretching, increase the content-hugging priority. 
		Baseline constraints work only with views that are at their intrinsic content height. If a view is vertically stretched or compressed, the baseline constraints no longer align properly. 
		Some views, like switches, should always be displayed at their intrinsic content size. Increase their CHCR priorities as needed to prevent stretching or compressing. 
		Avoid giving views required CHCR priorities. It’s usually better for a view to be the wrong size than for it to accidentally create a conflict. If a view should always be its intrinsic content size, consider using a very high priority (999) instead. This approach generally keeps the view from being stretched or compressed but still provides an emergency pressure valve, just in case your view is displayed in an environment that is bigger or smaller than you expected.

The intrinsic content size acts as an input to Auto Layout. When a view has an intrinsic content size, the system generates constraints to represent that size and the constraints are used to calculate the layout.
The fitting size, on the other hand, is an output from the Auto Layout engine. It is the size calculated for a view based on the view’s constraints. If the view lays out its subviews using Auto Layout, then the system may be able to calculate a fitting size for the view based on its content.//satck view根据内容大小和属性来计算自身大小

Select the type of rectangle you want to display.
-
The frame rectangle is the raw frame of the view element.

The alignment rectangle is the total visual space occupied by the view element including any runtime adornments. It can be different than the frame rectangle.

![视图构建器界面](https://developer.apple.com/library/ios/recipes/xcode_help-IB_objects_media/Art/IB_OM_H_frame_selector_2x.png)

![选项](https://developer.apple.com/library/ios/recipes/xcode_help-IB_objects_media/Art/IB_OM_H_frame_align_rect_popup_2x.png)

frame rectangle指的是视图的原始的frame，而alignment rectangle指的是布局视图中的视图可视区域

![frame rectangle 和 alignment rectangle的对比](http://img.blog.csdn.net/20150626144908579?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWFzaV94aQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

Auto Layout默认创建一个约束的优先级是1000，一般情况是不需要修改的，但是也可以通过修改优先级来处理特殊情况。例如：当container view的width变小的时候，它里面的subview的space就缩小，通过降低subview的优先级。

Size classes
-
为了更好地适应不同设备和不同方向的设备，使用约束和size classes来布局。首先设计好通用设备的布局，然后对不同的size classes做出添加、删除、修改约束来调整布局。

使用嵌套的stack view
-
* 隐藏的视图仍然在stack的管理视图中，它们不会消失，也不会对被管理的其他视图布局产生影响。
* 添加一个视图到stack view的管理视图中，自动的就把它添加到了视图层级中。
* 把视图从stack view的管理视图中移除并不会自动的从视图层级中移除出去。然而，从视图层级中移除视图却会把它从管理视图中移除出去。
* 在iOS中，视图的hidden属性不会被动画化。但是，这个属性一旦被添加到stack view的管理视图中就变得可以被动画化。真正的动画被stack view管理中，而不是视图本身。使用hidden属性来动画向stack view中添加或者移除视图。

约束错误类型：
-
* 不满足条件的约束。没有有效解决方案的约束

不满足条件的布局发生在系统不能为当前的一套约束找到一个有效的解决方案。两个或者两个以上的约束冲突，因为它们不能在同时有效。

所有的约束冲突都在界面上用红色标记出来，并且在错误信息导航栏作为警告被显示出来，在IB界面的右上角以红色箭头显示。

注意：IB并不会将所有的可能布局错误检测出来，因此，即时可以修复那些被IB标记的显而易见错误，这依然不够。仍然需要在运行时测试各种屏幕尺寸、方向、动态类型尺寸，和你需要支持的语言。

* 模棱两可的约束。有两种或者多种可能的解决方案的约束

模棱两可的约束相比不满足条件的约束更难检测和标记，即时模棱两可的约束有一个显而易见、可见的错误，它也是很难被检测出来是否是约束模棱两可还是约束的逻辑错误。

幸运的是，这里有一些方法来帮助你识别那些模棱两可的约束。所有的这些方法应该只用于调试，设置断点的地方你可以访问视图层次，然后在控制台调用以下方法：

* hasAmbiguousLayout. Available for both iOS and OS X. Call this method on a misplaced view. It returns YES if the view’s frame is ambiguous. Otherwise, it returns NO.
*exerciseAmbiguityInLayout. Available for both iOS and OS X. Call this method on a view with ambiguous layout. This will toggle the system between the possible valid solutions.
* constraintsAffectingLayoutForAxis:. Available for iOS. Call this method on a view. It returns an array of all the constraints affecting that view along the specified axis.
* constraintsAffectingLayoutForOrientation:. Available for OS X. Call this method on a view. It returns an array of all the constraints affecting that view along the specified orientation.
* _autolayoutTrace. Available as a private method in iOS. Call this method on a view. It returns a string with diagnostic information about the entire view hierarchy containing that view. Ambiguous views are labeled, and so are views that have translatesAutoresizingMaskIntoConstraints set to YES.

note:You may need to use Objective-C syntax when running these commands in the console. For example, after the breakpoint halts execution, type call [self.myView exerciseAmbiguityInLayout]into the console window to call the exerciseAmbiguityInLayout method on the myView object. Similarly, type po [self.myView autolayoutTrace] to print out diagnostic information about the view hierarchy containing myView.

Be sure to fix any issues found by Interface Builder before running the diagnostic methods listed above. Interface Builder attempts to repair any errors it finds. This means that if it finds an ambiguous layout, it adds constraints so that the layout is no longer ambiguous.

As a result, hasAmbiguousLayout returns NO. exerciseAmbiguityInLayout does not appear to have any effect, and constraintsAffectingLayoutForAxis: may return additional, unexpected constraints.

* 逻辑错误。布局逻辑存在bug

通过编码方式创建约束
=
尽可能的使用IB创建约束，IB提供工具来可视化、编辑、管理和debug约束，在设计时分析约束错误，让你能够发现并且修复问题。当然你也可以使用编码方式来创建约束：

* Layout Anchors

NSLayoutAnchor class为创建约束提供了一套连贯的接口。使用这个API来访问你想要约束元素的anchor属性。例如，视图控制器的top和bottom layout guides有topAnchor、bottomAnchor和heightAnchor属性。另外，视图为它们的edges、centers、size和baselines提供anchors。

注意：在iOS中，视图也有layoutMarginsGuide和readableContentGuide属性，这些属性分别提供了展示视图margins和readable content guide的UILayoutGuide对象。反过来说，这些guides为它们的edges、centers、size提供了anchors。

使用这些guides通过编码方式来为margins或者readable Content guides创建约束。

layout archors有几个创建约束不同的方法，每一个方法中的参数仅仅对等是两边的元素产生影响。

layout anchors也提供了其他安全的类型。NSLayoutAnchors有一些子类，这些子类为创建约束添加类型信息和特定子类的方法。这样可以有效地防止创建不可用的约束。

你不能直接使用NSLayoutAnchors，相反，你应该根据你所创建的约束类型使用NSLayoutAnchors的一个子类：

> NSLayoutXAixsAnchor 创建水平方向的约束

> NSLayoutYAxisAnchor 创建垂直方向的约束

> NSLayoutDimension 创建维度上的约束，长度和宽度

一旦你通过使用UIView、NSView、UILayoutGuide来访问NSLayoutAnchor，自动的就为你选择了一个正确的子类。

注意：

> * UIView并没有为layout margin提供anchor属性，而是通过layoutMarginGuide属性提供一个UILayoutGuide对象（展示这些margins），使用guide的anchor来创建约束。

> * 尽管NSLayoutAnchor class提供了额外的类型检查，也有可能产生不可用的约束。leadingAnchor与leftAnchor约束的时候（它们都是NSLayoutXAxisAnchor实例），autolayout并不允许使用混合的约束，最终在运行时约束就会崩溃。

	self.view.translatesAutoresizingMaskIntoConstraints = false
	
	let leftButton = UIButton.init(type: .Custom)
	
	leftButton.setTitle("leftButton", forState: .Normal)
	
	leftButton.backgroundColor = UIColor.blueColor()
       	
       	leftButton.translatesAutoresizingMaskIntoConstraints = false
       	
       	let rightButton = UIButton.init(type: .Custom)
       	
       	rightButton.setTitle("rightButton", forState: .Normal)
       	
       	rightButton.backgroundColor = UIColor.redColor()
       	
       	rightButton.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(leftButton)
	
	self.view.addSubview(rightButton)
	
	let margins = self.view.layoutMarginsGuide
       	
       	leftButton.leadingAnchor.constraintEqualToAnchor(margins.leadingAnchor).active = true
       	
       	margins.bottomAnchor.constraintEqualToAnchor(leftButton.bottomAnchor, constant: 20).active = true
       	
       	rightButton.leadingAnchor.constraintEqualToAnchor(leftButton.trailingAnchor, constant: 30).active = true
       	
       	rightButton.trailingAnchor.constraintEqualToAnchor(margins.trailingAnchor).active = true
       
       	margins.bottomAnchor.constraintEqualToAnchor(rightButton.bottomAnchor, constant: 20).active = true
       
       	leftButton.widthAnchor.constraintEqualToAnchor(rightButton.widthAnchor).active = true

* 使用NSLayoutConstraint class

也可以通过NSLayoutConstraint class的constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:方法来直接创建约束。这个方法显示的把约束等式转化为了代码，代码中的每一个参数对应一个约束等式的一部分。
	
不像采用anchor API，你必须为每一个参数指定一个值，即时它不会对布局产生影响。这就导致了大量的样板代码，通常是很难阅读的。
	
	NSLayoutConstraint(item: myView, attribute: .Leading, relatedBy: .Equal, toItem: view, attribute: .LeadingMargin, multiplier: 1.0, constant: 0.0).active = true
 
	NSLayoutConstraint(item: myView, attribute: .Trailing, relatedBy: .Equal, toItem: view, attribute: .TrailingMargin, multiplier: 1.0, constant: 0.0).active = true
 
	NSLayoutConstraint(item: myView, attribute: .Height, relatedBy: .Equal, toItem: myView, attribute:.Width, multiplier: 2.0, constant:0.0).active = true
	
注意：在iOS中，NSLayoutAttribute枚举值包含了视图的margins，这就意味着你可以不通过layoutMarginsGuide属性来创建margins约束。然而，你仍然需要使用readableContentGuide属性来创建readable content guides约束。注意上面提到的注意事项里面的两点

这个方法并不会突出那些重要的约束，乍一看、扫一眼代码可能会漏掉一些细节。此外，编译器不会对约束执行任何的静态分析，可以自由地创建无效的约束，这些约束会在运行时抛出异常。除非需要支持iOS8、OS X v10.10或者更早版本，考虑将代码迁移到更新的anchor布局API。

* 使用Visual Format Language

VFL让你使用像字符串一样的ASCII艺术形式来描述约束。

VFL有以下的几个优点和缺点：

1. auto layout使用VFL向控制台打印约束信息。正是这个原因，debug信息看起来就像是使用代码创建约束。
2. VFL通过使用一个很紧凑的表达式来一次性创建多个约束。
3. VFL只会创建有效的约束。
4. 符号强调了好的可视性而不是完整性。因此，有些约束不能通过VFL来创建，例如 aspect ratios
5. 编译器不会检查字符串的可用性，只能通过测试来修复错误。

很多地方都建议说不要使用VFL来添加约束，但是我觉得这个存在肯定有它的意义所在，就好比debug信息，反正觉得VFL挺好玩的。

VFL规范 <https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1>

	self.view.translatesAutoresizingMaskIntoConstraints = false
        
        let leftButton = UIButton.init(type: .Custom)
        leftButton.setTitle("leftButton", forState: .Normal)
        leftButton.backgroundColor = UIColor.blueColor()
        leftButton.translatesAutoresizingMaskIntoConstraints = false

        let rightButton = UIButton.init(type: .Custom)
        rightButton.setTitle("rightButton", forState: .Normal)
        rightButton.backgroundColor = UIColor.redColor()
        rightButton.translatesAutoresizingMaskIntoConstraints = false

        self.view.addSubview(leftButton)
        self.view.addSubview(rightButton)
        
	let views: [String: AnyObject] = ["leftButton" : leftButton, "rightButton": rightButton]
        
        let formatStringH: String = "H:|-margin-[leftButton(==rightButton)]-spacing-[rightButton]-margin-|"
        let formatStringVLeft: String = "V:[leftButton]-bottom-|"
        let formatStringVRight: String = "V:[rightButton]-bottom-|"
        
        let metrics = ["leading": 0, "trailing": 0, "spacing": 30, "bottom": 20, "margin": 20]
        
        let constraintsH = NSLayoutConstraint.constraintsWithVisualFormat(formatStringH, options: .AlignAllFirstBaseline, metrics: metrics, views: views)
        let constraintsVLeft = NSLayoutConstraint.constraintsWithVisualFormat(formatStringVLeft, options: .AlignAllFirstBaseline, metrics: metrics, views: views)
        let constraintsVRight = NSLayoutConstraint.constraintsWithVisualFormat(formatStringVRight, options: .AlignAllFirstBaseline, metrics: metrics, views: views)
        
        NSLayoutConstraint.activateConstraints(constraintsH)
        NSLayoutConstraint.activateConstraints(constraintsVLeft)
        NSLayoutConstraint.activateConstraints(constraintsVRight)
        
同上面的使用anchor添加约束的效果一样

使用scroll view
=
当使用scroll view的时候，你需要定义scroll view在它的父视图中frame的size和posiiton，还有scroll view的content 区域大小，所有这些工作都可以通过Auto layout实现。

* scroll view和scroll view外部的对象之间的frame约束关系就像任何其他的视图一样。可以理解为就像两个视图之间的约束关系一样来处理scroll view。
* 对于scroll view和它的content之间的约束，取决于约束条件的性质：
	
	- scroll view的edges或margins和它的content区域之间的，连接scroll view的content区域的约束。（理解为content view的leading、trilling、top、bottom的四个方向的约束）
	- height，width或centers，连接scroll view的frame的约束。（理解为对宽、高或者centers的约束）
	
* 通过使用scroll view的content和scroll view外部对象的约束来为scroll view的content提供一个完整的position，使content就像是悬浮在scroll view上方。

当你使用一个dummy view或者layout group来管理scroll view的content，这个逻辑就会变得非常简单：

1. 添加一个scroll view到界面上。
2. 像平常一样，定义scroll view的size和position。
3. 添加一个view到scroll view上。设置这个view的Xcode specific label值为 Content View。（这个view就作为一个dummy view来管理scroll view的content）
4. 定义content view的的top、bottom、leading和trailing edges到scroll view相应的edges。这个content view定义了scroll view的content区域。
> 这个content view还没有一个完整的size。它会被拉伸和压缩以适应那些被你放入其中的视图和控件。（content view被拉伸和压缩，这样才能够放下更多或是刚好装下你放入的控件或视图，并不是去拉伸或压缩那些控件）
5. （可选）为了禁止水平方向的scrolling，设置content view的width等于scroll view的width，content view现在填满了scroll view的水平方向。
6. （可选）为了禁止垂直方向的srolling，设置content view的height等于scroll view的height，content view现在填满了scroll view的垂直方向。
7. 在content view中布局scroll view的content，像平常一样使用约束来定位content view中的content。（集中精力沿着view的edges来布局你想要布局的界面内容，而不是考虑scroll view的contentSize这些乱七八糟的东西）

> 重要：你必须完全地定义content view的size（除了5和6设置步骤的定义）。为了设置基于你的content的内在大小的height（垂直方向的scrolling），你必须从content view的top edge到bottom edge有一套完整约束链和拉伸的视图。相似的（水平方向的scrolling），width，你也必须从content view的leading edge到trailing edge的一套完整的约束链和拉伸视图。（这里理解为 要确定content view固定的height或width，以确定scroll view的contenSize）
>
> 如果你的content没有一个内在内容大小，你就必须添加适当的size约束，要么是content view的，要么是content的。（照应上一段，必须要有一个固定的content size才能确定scroll view的contentSize）
>
> 当content view比scroll view更高的时候，scroll view运行垂直方向的scrolling；当content view比scroll view更宽的时候，scroll view允许水平方向的scrolling；否则，默认不允许scrolling。

Working with Self-Sizing Table View Cells
=
在iOS中，可以使用Auto Layout来定义table view cell的高度，然而，这个功能默认是关闭的。

通常，一个cell的高度是被table view的代理方法所定义的：`tableView:heightForRowAtIndexPath:`,为了使用自适应的table view cells，你必须设置table view的`rowHeight`属性为`UITableViewAutomaticDimension`，你也必须为` estimatedRowHeight`属性定义一个值。只要这些属性都被设置了，系统就会使用auto layout来计算实际的行高值。
> tableView.estimatedRowHeight = 85.0
> 
> tableView.rowHeight = UITableViewAutomaticDimension

下一步，在table view的content view中定义cell的content。为了定义cell的高度，你需要一套完整的约束和视图（定义了高度）来填充concontent view的top edge到bottom edge区域。如果你的视图没有intrinsic content heights，系统就会使用这个值。如果没有，你必须添加对应的高度约束，不管是对视图还是对content view自身。（和上面的scroll view的要求很类似，必须要有一套完整的约束）

通常，尽可能的让`estimated row height`精确。系统计算像scroll bar的高度就是基于这些estimates。越精确，用户体检越好。

> 注意：
> 
> 当使用table view cells的时候，你不能改变`预定义内容`的约束，比如`textLabel`、`detailTextLabel`和`imageView`属性。
>
> 支持下面的约束：
>
> * 依靠cell的content view定位子视图的约束
> * 依靠cell的bounds定位子视图的约束
> * 依靠预定义内容定位子视图的约束
> * Constraints that position your subview relative to the cell’s content view.
> * Constraints that position your subview relative to the cell’s bounds.
> * Constraints that position your subview relative to the predefined content.

# 改变约束
任何约束的改变都会改变约束的底层数学表达式。

下面所有的行为都会改变一个或者更多的约束：

* 激活或者注销一个约束
* 改变约束的常数`constant`值
* 改变约束的优先级
* 从视图层级中移除视图

像设置控件的属性、修改视图层级的其他改变都会改变约束。当一个改变发生的时候，系统会传递一个延时布局的时间表。（`a deferred layout pass` ，有点像调用setNeedsLayout之类响应布局的延时布局，看下面一小节）

一般来说，你可以在任何时候改变这些。理想情况下，大多数的约束应该在IB中创建，或者通过编码方式在view controller初始（比如在viewDifLoad中）建立的时候创建。如果你需要在运行时动态的改变约束，通常最适合改变的时候是在应用状态改变的时候。比如，你想通过改变一个约束来响应一个按钮被点击，直接在按钮点击事件方法中实现改变。

有时候，你可能想要批量地处理一些性能变化的原因。`Batching Changes`注意看下面小结。

## The Deferred Layout Pass
auto layout并不是立刻就更新受影响视图的frames，它会将布局列一个表给最近的刷新时刻（很像setNeedsLayout，标记需要重新布局，然后等待下一次刷新时刻）。The Deferred Layout Pass更新布局的约束，然后计算出在视图层级中所有视图的frames。

你可以通过调用`setNeedsLayout`和`setNeedsUpdateConstraints`方法来设计自己的延时布局时间表。

The Deferred Layout Pass实际上对视图层级包含两个步骤：
> 1. 必要的，`update pass`更新约束
> 2. 必要的，`layout pass`重新定位reposition视图的frames

### Update Pass
系统遍历视图层级结构，并且对所有的view controller调用`updateViewConstraints`方法，为所有的view调用`updateConstraints`方法。你可以覆盖重写这些方法来优化你的约束变化。（see `Batching Changes`）

### Layout Pass
系统再次遍历视图层级结构，并且对所有的view controller调用` viewWillLayoutSubviews`方法，对所有的view调用`layoutSubviews`（OS X中调用 layout）方法。默认情况下，layoutSubviews方法会更新每一个有reatangle（被auto layout引擎计算出来的rectangle，注意前面有提到的`alignment rectangle`和`frame rectangle`的区别）的subview的frames。你可以覆盖重写这些方法来修改layout（see `Custom Layouts`）

## Batching Changes
在改变发生时候，立即更新一个约束是简洁明了的。在之后的方法中延迟这些改变是的代码更加复杂，也更加不易理解。

有些时候，你可能想要批量处理性能引起的变化。只有在改变约束太慢的地方，或是当视图有一个冗余的变化的时候才会这样做。

在拥有约束的视图上调用`setNeedsUpdateConstraints`来批量处理改变，而不是立刻就改变。然后，覆盖重写视图的`updateConstraints`方法来修改受影响的约束。

> 注意：
你的`updateConstraints`实现必须尽可能的高效。不要注销掉你所有的约束，然后又重新激活你需要的约束。相反，你的app应该追踪你的约束，并且在每一个更新过程中确保它们。仅仅改变那个需要被改变的项。在每一个更新过程中，你必须确保对app当前的状态有相对应的约束。

在` updateConstraints `的实现方法最后始终调用超类实现。

不要在` updateConstraints `中调用`setNeedsUpdateConstraints`方法。调用`setNeedsUpdateConstraints`延时安排另一个update pass，会造成一个回路、循环（递归死循环）。

## Custom Layout

覆盖重写`viewWillLAyoutSubviews`和`layoutSubviews`方法来修改被layout引擎返回的结果。

> 重要：
> 如果可能的话，使用约束来定义所有的布局。这样的布局结果更加地健壮、更容易调试。当你需要创建一个不能单独用约束表达的布局的时候，你应该只重写覆盖`viewWillLayoutSubviews`或` layoutSubviews`方法。

当重写这些方法的时候，布局在一个不一致的状态。一些视图可能已经被加载了，另外的可能还没有被加载出来。对于如何修改视图层级结构你需要非常的小心，否则你会创建一个回路循环（递归死循环）。下面的规则可以帮助你避免这个死循环：
> * 在你的方法定义中必须调用超类定义
> * 你可以在你的子树结构中安全的使布局视图无效，但是，你必须在这之前调用超类实现。
> * 不要在你的子树结构层以外使任何的布局视图无效，这会造成一个死循环。
> * 不要调用`setNeedsUpdateConstraint`方法，You have just completed an update pass. Calling this method creates a feedback loop.
> * 不要调用`setNeedsLayout`方法，会造成死循环。
> * 改变约束时要仔细。你不会想要在你子树层结构以为的任何视图的布局意外失效的。
