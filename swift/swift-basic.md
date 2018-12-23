1. let 和 var
2. `\()` 字符串插值
3. 三个双引号 可以引用多行文字，像不像python？
4. 使用 `[]`来创建数组和字典

    ```swift
    var shoppingList = ["catfish", "water", "tulips"]
    shoppingList[1] = "bottle of water"

    var occupations = [
        "Malcolm": "Captain",
        "Kaylee": "Mechanic",
    ]
    occupations["Jayne"] = "Public Relations
    
    shoppingList.append("blue paint")
    print(shoppingList)

    let emptyArray = [String]()
    let emptyDictionary = [String: Float]()

    //如果类型可以被推断，比如传参数,可以传空
    shoppingList = []
    occupations = [:]
    ```
5. `for ... in` 遍历数组和字典
6. `if`判断的值只能是bool类型的，不能隐式推断为0
7. 使用`if let`组合去解包可选类型
8. 使用`??`去给一个可选类型提供一个默认值
    ```swift
    let nickName: String? = nil
    let fullName: String = "John Appleased"
    let informalGreeting = "Hi \(nickName ?? fullName)"
    ```
9. `switch` 支持各种数据匹配操作，不再限制是整形和测试是否相等
 
    ```swift
    let vegetable = "red papper"
    switch vegetable {
    case "celery":
        print("celery")
    case "cucumber", "watercress":
        print("cucumber or watercress")
    case let x where x.hasSuffix("papper"):
        print("with papper")
    default:
        print("default")
    }
    ```
10. `while` 循环 `repeat ... while` 
11. `..<`（不包括右边结） 和 `...`（包括右边界） 来规定一个遍历范围
    ```swift
    var total = 0
    for i in 0..<4 {
        total += i
    }
    for j in 0...4 {
        total += j
    }
    ```
12. 函数定义使用`func`关键词， 函数参数分为内参和外参，默认内参外参一致，但是可以添加不一样的外参，可以通过`_`隐藏外参名字

    ```swift
    func greet(person: String, day: String) -> String {
            return "Hello \(person), today is \(day)"
    }

    greet(person: "Bob", day: "Tudesday")


    func greet1(_ person: String, on day: String) -> String {
        return "Hello \(person), today is \(day)"
    }
    greet1("John", on: "Wedensday")
    ```

13. 元组的使用，用来返回多条数据，可以通过name来获取，或者通过下标来获取
    ```swift
    let tuple = (min: 5, max: 100, sum: 105)
    print(tuple.max)
    print(tuple.1)
    ```
14. 函数是一级公民，函数内可以返回函数，函数可以嵌套函数，函数可以作为参数传递给另一个函数， 函数是一种特殊类型的闭包

15. 闭包：一块代码，该代码可以在后面被调用。 闭包可以访问它被创建的的作用域内的变量和函数,即时这个闭包在另一个作用域被执行。

16. 闭包的定义`({  paramsxx in returnxx})`, 使用 `in` 来区分声明和函数体，左边声明，右边函数体

    ```swift
    //完整的闭包的定义
    let numbers = [1, 2, 3, 4]
    numbers.map({ (number: Int) -> Int in
        let result = 3 * number
        return result
    })
    ```
    如果一个闭包的参数类型已知，你可以删除参数的类型 或者 返回值类型，或者两个都删除。
    ``` swift
    //省略参数和返回值
    numbers.map({number in 3 * number})
    ```
    可以通过下下标来访问参数,`$0, $1, $2, $n`
    ```
    numbers.map({3 * $0})
    ```
    如果函数的最后一个参数是一个闭包，这个闭包可以直接写在括号外面

    ```swift
    numbers.map(){
        return 3*$0
    }
    ```
    尾随闭包：如果函数的最后一个参数是闭包，并且只有一个参数，可以直接省略括号
    ```swift
    numbers.map{
        return 3*$0
    }
    ```






