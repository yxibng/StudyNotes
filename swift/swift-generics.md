# 范型
* 在尖括号内定义类型 `<Typename>`
* 类型名字一般用大些
* 使用`where`指定匹配的条件,遵循协议，类型一致，值相等

一个数组的的例子

```swift
func makeArray<Item>(repeation item: Item, numberOfTimes: Int)->[Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes{
        result.append(item)
    }
    return result
}
makeArray(repeation: "knock", numberOfTimes: 4)
```

一个枚举的例子
```swift
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}

var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```
一个遵循协议的例子，使用`where`来指定条件

```swift
func anyCommonElements<T: Sequence, U: Sequence> (_ lhs: T, _ rhs: U) -> Bool  where T.Element: Equatable, T.Element == U.Element{
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}

anyCommonElements([1,2,3], [3])
```











