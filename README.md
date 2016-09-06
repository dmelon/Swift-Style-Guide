# Swift 编程规范

### 结构

* 使用 Xcode 默认的缩进配置：
1 indent = 1 tab = 4 spaces
* 不用 ```{```、```else``` 起行

**Preferred:**
```swift
func run() {

}

if true {

} else {

}
```

**Not Preferred:**

```swift

func run() 
{

}

if ture {

}
else {

}
```

* 删除不必要的代码。

**Preferred:**

```swift 
override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return Database.contacts.count
}
```

**Not Preferred:**

```swift 
override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
   // #warning Incomplete implementation, return the number of sections
   return 1
}

override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
    return Database.contacts.count
}
```

* 尽量将一个文件中只包含一种类型定义，除非多个类型之间需要引用对方的私有变量、方法或者多个类型之间必须相互配合，无法分割。不过在将多个类型放在一个文件内之前，考虑一下是否有更好的低耦合的设计方案。

**Preferred:**

```swift 
// VideoCell.swift
class VideoCell: UITableViewCell {

}

// VideoItem.swift
class VideoItem: NSObject {

}
```

* 1. 善于使用换行对代码进行划分
  2. 善于使用```//MARK:``` 对属性和方法进行分类
  3. 善于使用 extension 对协议实现进行划分
  4. 善于使用```///```、```/* */``` 进行注释

**Preferred:**

```swift 
class VideoViewController: UIViewController {
    
    var actionButton: UIButton
    var textLabel: UILabel
    
    // MARK: - UI Functions
    /// 设置 action button
    func setActionButton() {
        
        // configure button
        actionButton.setTitle("Send", forState: .Normal)
        ...
        
        // configure button constraints
        actionButton.topAnchor.constraintEqualToAnchor(view.topAnchor)
        ...
    }
    
    /// 设置 text label
    func setTextLabel() {
        ...
    }
    
    // MARK: - Network Functions
    func fetchVideoList() {
        
    }
}

extension VideoViewController: UITableViewDataSource {
    
    func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 2
    }
}
```

> *Note*：从功能上来讲，由于一个 extension 不能添加 stored properties，因此很多情况下，extension 的实现中经常要使用原类中的属性，因此 extension 的实现，特别是像 UITableViewDelgate、UITableViewDataSource 这种 Delegate、DataSource 的实现，要放在原类源文件中去，以便能够访问一些 private 的属性和方法。这种情况下，extension 无法放在独立的代码文件中，此时 extension 的作用也就相当于 ```//MARK:``` 的作用。
> 
> 而对于像 CustomStringConvertible 这种协议，extension 大都可以写在原类之外的。此时 extension 的作用才算是真正体现出来：保持低耦合，可以独立删除 extension 所在的文件而不影响原类的功能。

### 语言

* class，structure、enumeration、protocol 等关于 type 的命名都采用“大驼峰”的写法。variable、function 采用“小驼峰”的写法。这里需要注意的是，在 swift 3 之前，enumeration 的 case 采用的是“大驼峰”，但在 swift 3 时就更改为“小驼峰”了，以保持习惯一致。
* 用“驼峰”写法代替 “XX_XXX_XX” 写法。
* 为了保持代码的清晰可读，易于理解，一般来说是要将一个单词进行全部拼写，而不应该像传统 c 语言中包含了大量缩写。但有一些缩写是作为一种传统遗留下来的，[这些](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285)是例外。当然，不鼓励擅自开辟新的传统。在允许的缩写中，大小写要保持一致：要么全部大写，要么全部小写。
* swift 中有命名空间，因此不像 Objective C 那样写类型前缀了。
5. 使用美式英语，不要英式。

**Preferred:**

```swift
private let maximumWidgetCount = 100

class WidgetContainer {
    var widgetButton: UIButton
    let widgetHeightPercentage = 0.85
}

let urlString: URLString
let userID: UserID

class Player {
}

var color: UIColor
```

**Not Preferred**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
    var wBut: UIButton
    let wHeightPct = 0.85
}

let uRLString: UrlString
let userId: UserId

class CBPlayer {
}

var colour: UIColor
```

* 泛型之命名：泛型中，对于类型变量尽量使用有意义的单词作为类型参数。如果实在没有，实在实在没有意义，使用 swift 标准库中使用过的 T，U 或 V 吧。

**Preferred:**

```swift
struct Stack<Element> { ... }
func writeTo<Target: OutputStream>(inout target: Target)
func max<T: Comparable>(x: T, _ y: T) -> T
```


**Not Preferred:**

```swift
struct Stack<T> { ... }
func writeTo<target: OutputStream>(inout t: target)
func max<Thing: Comparable>(x: Thing, _ y: Thing) -> Thing
```

* 协议之命名：使用名词或者形容词来命名协议，不要使用动词。官方文档上说，当一个协议描述“是什么”时，使用名词命名协议，如 Collection。当一个协议描述“怎么样”时，使用形容词命名协议，如 Equatable。个人觉得，“是什么”和“怎么样”很难区分，Collection 改成 Collective，并不觉得有何不妥。一个比较简单的原则就是不使用除名词、形容词以外的词性（如动词 Collect）去命名协议。

* 代码语意要清晰易懂，避免歧义

**Preferred:**

```swift
extension List {
  public mutating func remove(at position: Index) -> Element
}
employees.remove(at: x)
```

**Not Preferred:**

```swift
extension List {
  public mutating func remove(at position: Index) -> Element
}
employees.remove(x) // unclear: are we removing x?
```

* 符合正常英语阅读语法

**Preferred:**
```swift
x.insert(y, at: z)          “x, insert y at z”
x.subViews(havingColor: y)  “x's subviews having color y”
x.capitalizingNouns()       “x, capitalizing nouns”
```

**Not Preferred:**

```swift
x.insert(y, position: z)
x.subViews(color: y)
x.nounCapitalize()
```

* 尽量不写 self

**Preferred:**

```swift
class ViewController {
    let row: Int, column: Int

    func setupViews() {
        view.addSubview(label)
        view.addSubview(button)
    }
}
```

* 尽量使用类型推断

**Preferred:**

```swift
let name = "Merry"
var count = 5
let list = [Int]()
```

* 尽量使用语法糖，比如 ```if let```，```[String]()```, ```for in```, ```??```，尾随闭包, return 缺省等

**Preferred:**

```swift
if let asset = asset {
    print(asset.URL)
}

var names = [String]()

for name in names { name in 
    print(name)
}

let value: Bool?
let success = value ?? true

manager.fetchData { datas in 
    ...
}

var diameter: Double {
    return radius * 2
}
```

**Not Preferred:**

```swift 

if asset != nil {
    print(asset!.URL)
}

var names = Array<String>()

for i in 0..<names.count {
    let name = names[i]
    print(name)
}

let value: Bool?
var success: Bool = true 
if let value = value {
    success = value
}

manager.fetchData(block: { datas in 
    ...
})

var diameter: Double {
    get {
        return radius * 2
    }
}
```

* 尽量使用 final、private、let 来避免不可预期的后果。
* 尽量避免声明全局方法，尽量将方法都归于某个类型内。

## Q&A

### 使用 Class 还是 Structure？
答：大部分情况下，使用 Class，但 Structure 还有其特殊的应用场景。然而，在 [github/swift-style-guide](https://github.com/github/swift-style-guide) 中，倾向使用的是 structure，这里个人意见还是偏向于官方文档建议的。 参考：[Classes and Structures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html)、[Value and Reference Types](https://developer.apple.com/swift/blog/?id=10)

### 在闭包里如何使用 self ？
答：只有在确认闭包执行时 self 不会被释放，才使用 [unowned self]，否则使用 [weak self]。如果使用 [weak self]，首先使用 optional binding 来 retain self，保证闭包执行期间 self 不会被释放。

**Preferred:**

```swift
resource.request().onComplete { [weak self] response in
  guard let strongSelf = self else { return }
  strongSelf.model.addObserver(self, forKeyPath: "some", options: .New, context: nil)
  strongSelf.model.removeObserver(self, forKeyPath: "some")
}
```

**Not Preferred:**

```swift
resource.request().onComplete { [weak self] response in
    // self exists
    self?.model.addObserver(self, forKeyPath: "some", options: .New, context: nil) 
    // self is not retained and ARC may deallocate self at this point which will cause crash
    
    // this line may not be executed
    self?.model.removeObserver(self, forKeyPath: "some")
}
```

## 更多
* [Offical API Design Guidelines](https://swift.org/documentation/api-design-guidelines/#follow-case-conventionshttps://swift.org/documentation/api-design-guidelines/#follow-case-conventions)
* [raywenderlich/swift-style-guide](https://github.com/raywenderlich/swift-style-guide#protocols)
* [github/swift-style-guide](https://github.com/github/swift-style-guide)
