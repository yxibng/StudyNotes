## 枚举和结构体

1. 使用`enum`关键字来创建枚举，
    * 枚举内部可以关联方法， 
    * 通过`rawValue`获取枚举代表的值,
    * 默认情况下`rawValue`从0开始，每次累加1。
    * 可以指定 `rawValue`的值
    * 创建实例的话使用`init?(rawValue:)`
    * case的值是真实的值，不是rawValue的另一种形式。
    * 如果rawValue没有意义的话，可以不提供

    ```swift
    enum Rank: Int {
        case ace = 1
        case two, three, four, five, six, seven, eight, nine, ten
        case jack, queen, king
        
        func simpleDescription() -> String {
            switch self {
            case .ace:
                return "ace"
            case .jack:
                return "jack"
            case .queen:
                return "queen"
            case .king:
                return "king"
            default:
                return String(self.rawValue)
            }
        }
    }
    let ace = Rank.ace
    let aceRawValue = ace.rawValue
    
    //构建实例
    if let convertedRank = Rank(rawValue: 3) {
        let threeDescription = convertedRank.simpleDescription()
        print(threeDescription)
    }
    ```
2. 枚举关联变量值，这些值在枚举被实例化的时候决定，每个枚举的实例都不相同。用这种方法，可以让枚举去存储属性。结合`swith`可以读出已经存储的值。

    ```swift
    enum ServerResponse {
        case result(String, String)
        case failure(String)
    }

    let success = ServerResponse.result("6:00 am", "8:09 pm")
    let failure = ServerResponse.failure("Out of cheese.")

    switch success {
    case let .result(sunrise, sunset):
        print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
    case let .failure(message):
        print("Failure...  \(message)")
    }

    ```