# 错误处理
* 实现`Error`协议的任何类型都可以代表错误信息
* 使用`throws`标记一个方法可能会抛出错误
* 使用`throw`将错误信息抛出
* 使用`do-catch`去处理错误, 可以有多个`catch`
* 使用`try?`将结果转化为`optional`

```swift
enum PrinterError: Error {
    case outOfPaper
    case noToner
    case onFire
}

func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}

//do-catch, 多个catch
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheneg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I'll just put this over here, with the rest of the fire")
} catch let printerError as PrinterError {
    print("Printer error:\(printerError)")
} catch {
    print(error)
}

//try?
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")

```

