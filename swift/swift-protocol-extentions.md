# 协议和扩展

## 协议
使用`protocol`来定义协议
* `class`、`enum`、`struct`都可以遵从协议
* 协议里面可以包含属性
* 协议里面可以包含方法
* 协议可以作为类型去使用
* 协议里面包含optional方法，必须被标记为`@objc`
* 任何实现了optional protocol的类必须继承自`NSObject`
* 协议可以继承协议
* 需要指定一个属性是`get` 或者 `get set`
* 如果需要修改变量，添加`mutating`关键字在`func`前
* 必须实现所有的属性和方法
* 在`extention`里面遵从协议
* 使用`extention`可以给一个协议添加默认的实现
```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}

class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class"
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "Now 100% adjusted"
    }
}

var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStruct: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += "(adjusted)"
    }
}

var b = SimpleStruct()
b.adjust()
let bDescription = b.simpleDescription

//作为类型去使用
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)

```

## 扩展
使用关键字`extention`,
* 给已知类型添加新特性，如：新的方法，新的计算属性。
* 使用扩展使定义在其他地方的一个`type`遵从`protocol`，甚至是一个你从`library`或者`framework`中import的一个`type`

```swift
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
```

