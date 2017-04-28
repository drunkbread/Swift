# 函数
### 无参数函数
```swift
func sayHelloWord() -> String {
	return "Hello, World!"
}

```

### 多参数函数
```swift
func greet(person: String, hasGreeted: Bool) -> String {
    
    if hasGreeted {
        return "Hello Again \(person)"
    } else {
        return "Hello \(person)"
    }
}
```
### 无返回值函数
```swift
func greet(person: String, hasGreeted: Bool) {
    
    if hasGreeted {
        print("Hello Again \(person)")
    } else {
        print("Hello \(person)")
    }
}
```

被调用时，函数的返回值可以被忽略

```swift
func printAndCount(string: String) -> Int {
    
    print(string)
    return string.characters.count
}

func printWithoutCount(string: String) {
    
    let _ = printAndCount(string: string)
}

printWithoutCount(string: "Kaka")
printAndCount(string: "kaka")
```
### 多重返回值函数
可以用元组（tuple）类型让多个值作为一个复合值从函数中返回。

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}

minMax(array: [1, 3,4,5,6])
```
### 可选元组返回类型
```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    guard !array.isEmpty else {
        return nil
    }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}

if let bounds = minMax(array: [1, 43, 2, -45]) {
    
    print(bounds.min, bounds.max)
}
```
### 函数参数标签和参数名称
默认情况下，函数参数使用参数名称作为它们的参数标签。

```swift
func takeARest(when: String, atPlace: String) {
    print("hahaha")
}
takeARest(when: "ss", atPlace: "cc")
```
### 指定参数标签
可以在函数名称前指定它的参数标签，中间以空格分隔。

```swift
func takeARest(when: String, stay atPlace: String) {
    print("hahaha")
}
takeARest(when: "ss", stay: "cc") // stay 是参数标签
```
### 忽略参数标签
如果不希望为某个参数添加一个标签，可以使用一个下划线`_`来代替一个明确的参数标签。

```swift
func takeARest(when: String, _ atPlace: String) {
    print("hahaha")
}
takeARest(when: "ss", "cc")
```
### 默认参数值
在函数体中通过给参数赋值来为任意一个参数定义默认值。当默认值被定义后，调用这个函数的时候可以忽略这个参数。

```swift
func takeARest(when: String, _ atPlace: String = "Beijing") {
    print("I will take a rest at \(atPlace) \(when)")
}
takeARest(when: "tomorrow")
```
### 可变参数
一个可变参数可以接受零个或多个值。函数调用时，可以用可变参数来指定函数参数可以被传入不确定数量的输入值。通过在变量类型名后面加入`...`的方式来定义可变参数。   
   
可变参数的传入值在函数体中变为此类型的一个数组。

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1,2,3,4,5)
```

> 注意：   
> 一个函数最多只能拥有一个可变参数。

### 输入输出参数
函数参数默认是常量。不能修改参数值。如果想要一个函数可以修改参数的值，并且想要在这些修改在函数调用结束后仍然存在，就应该把这个参数定义为输入输出参数。   
定义输入输出参数时，在参数定义前加`inout`关键字。   
你只能传递变量给输入输出参数。不能传入常量或者字面量，因为这些量是不能被修改的。当传入的参数作为输入输出参数时，需要在参数名前加`&`符，表示这个值可以被函数修改。
> 注意：   
> 输入输出参数不能有默认值，而且可变参数不能用`inout`标记。

```swift
func swapTwoInts (_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var someInt = 3
var anotherInt = 107
swap(&someInt, &anotherInt)

print("SomeInt is now \(someInt), anotherInt is now \(anotherInt)")
// SomeInt is now 107, anotherInt is now 3
```

### 函数类型
每个函数都有特定的函数类型，函数的类型由函数的参数类型和返回类型组成。

```swift
func addTwoInts(_ a:Int, _ b:Int) -> Int {
    return a + b
}
// 此函数类型为 (Int, Int) -> Int，可以解读为“这个函数类型有两个Int型的参数并返回一个Int型的值”

func addTwoInts() {
    
}
// 此函数类型是() -> void
```

### 使用函数类型
在swift中，使用函数类型就像使用其他类型一样。

```swift
var mathFunc: (Int, Int) -> Int = addTwoInts
mathFunc(2, 3)
// 得到5
```
当赋值一个函数给常量或者变量时，可以让Swift推断其函数类型

```swift
let anotherMathFunction = addTwoInts
// anotherMathFunction 被推断为 (Int, Int) -> Int 类型
```
### 函数类型作为参数类型

```swift
func addTwoInts(_ a:Int, _ b:Int) -> Int {
    return a + b
}
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    
    print("Result:\(mathFunction(a, b))")
}
printMathResult(addTwoInts, 8, 6)
// Result:14
```

### 函数类型作为返回类型
你可以用函数类型作为另一个函数的返回类型。你需要做的是在返回箭头后写一个完整的函数类型。

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}

func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)

while currentValue != 0 {
    print("\(currentValue)...")
    currentValue = moveNearerToZero(currentValue)
}
print("Zero!")
3...
2...
1...
Zero!
```

### 嵌套函数
```swift

func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepBackward(input: Int) -> Int { return input - 1 }
    func stepForward(input: Int) -> Int { return input + 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
while currentValue != 0 {
    print("\(currentValue)...")
    currentValue = moveNearerToZero(currentValue)
}
print("Zero!")
-4...
-3...
-2...
-1...
Zero!
```
