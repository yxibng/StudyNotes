## objects and classes

1.  `class`关键字来创建类， class内部可以添加变量，常量和方法等， 通过`.`语法来访问属性和方法，
class是引用类型，闭包也是引用类型
2. 使用`init`来初始化一个类
    * 初始化的时候，先给自身属性赋值，
    * 之后初始化父类
    * 之后可以修改继承自父类的属性
3. 使用`deinit`在一个类销毁之前，来执行一些清理工作
4. 继承的写法 `classB : classA` ,B 继承了 A
5. 不是必须有一个基类
6. 重写父类的方法，前面要加关键字`override`,不然编译器会报错，不是重写父类的方法，但是加了`override`也会报错

7. 计算属性的`get` 和 `set`

    `set`方法的时候，新的值被隐式命名为`newValue`,当然自己也可以显示的命名这个新的值，在 `set(xxx){}`

8. 观察一个属性的变更`willSet` 和 `didSet`

9. 如果一个可选类型的值是nil，后续操作不会进行，否则的话，后续操作是解包后的操作
    ```swift
    let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
    let sideLength = optionalSquare?.sideLength
    ```
10. 上面的操作的代码
```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides"
    }
}


var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()



class NamedShape {
    var numberOfSides: Int = 0
    var name: String
    init(name: String) {
        self.name = name
    }
    
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides"
    }
}

class Square: NamedShape {
    var sideLength: Double
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }
    
    func area() -> Double {
        return sideLength * sideLength
    }
    
    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)"
    }
    
}

let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()


class Circle: NamedShape {
    var radius: Double = 0.0
    init(radius: Double, name: String) {
        self.radius = radius
        super.init(name: name)
    }
    
    func area() -> Double {
        return 3.14 * radius * radius
    }
    override func simpleDescription() -> String {
        return "a circle with radiux \(radius)"
    }
}

let circle = Circle(radius: 10, name: "my test circle")
circle.area()
circle.simpleDescription()


class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }
    
    var perimeter: Double {
        get {
            return 3 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }
    
    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of lenght \(sideLength)"
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "triangle")
print(triangle.perimeter)
triangle.perimeter = 9.9
print(triangle.sideLength)


class TriangleAndSquare {
    var triangle: EquilateralTriangle{
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    
    var square: Square{
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
    
}


var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
print(triangleAndSquare.triangle.sideLength)

triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)


let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength

```