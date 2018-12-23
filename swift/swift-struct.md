## 结构体
1. 用`struct`关键词来创建结构体
    * 可以有构造方法
    * 可以有属性
    * 可以有方法
    * struct是值类型，传递的时候被拷贝
    * class是引用类型，传递的时候是传的引用
    * 方法前面加`mutating`关键字允许方法修改结构体的属性

    ```swift
    struct Person {
        var name: String
        var age: Int
        func simpleDescription() -> String {
            return "name is \(name), age is \(age)"
        }

        mutating func changeName(name: String) {
            self.name = name
        }
    }

    let person1 = Person(name: "name1", age: 10)
    print(person1.simpleDescription())
    ```

