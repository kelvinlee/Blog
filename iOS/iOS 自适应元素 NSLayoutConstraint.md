```swift
self.view.addSubview(label1)
// Do any additional setup after loading the view, typically from a nib.
label1.text = "123你好啊？?"
label1.setTranslatesAutoresizingMaskIntoConstraints(false)
var constraints = [NSLayoutConstraint]()
NSLayoutConstraint.constraintsWithVisualFormat("[label1]-20-|",options:NSLayoutFormatOptions.allZeros,metrics: nil,views: ["label1": label1!]).map {
    constraints.append($0 as NSLayoutConstraint)
}
self.view.addConstraints(constraints)
```

需要注意，一定要先将元素放到view当中，才能触发。否则会报错。

几种格式：

Standard Space: `[button]-[textField]`

Width Constraint: `[button(>=50)]`

Connection to Superview: `|-50-[purpleBox]-50-|`

Vertical Layout: `V:[topField]-10-[bottomField]`

Flush Views: `[maroonView][blueView]`

Priority: `[button(100@20)]`

Equal Widths: `[button1(==button2)]`

Multiple Predicates: `[flexibleButton(>=70,<=100)]`

A Complete Line of Layout: `|-[find]-[findNext]-[findField(>=20)]-|`


这样制作的时候就可以通用于 3.5/4/4.7/5.5 寸的屏幕。

好像是只适用于 iOS6+ 。(如果用 swift 只适用于 iOS7+)
