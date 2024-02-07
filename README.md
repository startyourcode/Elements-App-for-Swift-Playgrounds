# Elements-App-for-Swift-Playgrounds
対話ブックで作成する元素学習アプリケーション。

## 準備
ブック内の余計なページを削除する（StringExtensionは残すこと）

## 全体のコード

```swift
// main.swift
let elementData = [
    1: "H", 
    2: "He", 
    8: "O"
]        

var elements = [Element]()
for (key, value) in elementData {
    let newElement = Element(atomicNumber: key, symbol: value)
    elements.append(newElement)
}

let myDataModel = DataModel(name: "Elements Illustrated Book", array: elements)
var myLearningCenter = LeaningCenter(dm: myDataModel)
myLearningCenter.start()
show("Bye!")

// LearningCenter.swift

public struct LeaningCenter {
    var student: String?
    var availableDataModel: DataModel
    
    public init(dm: DataModel) {
        self.availableDataModel = dm
    }
    
    public mutating func start() {
        introduction()
        setStudent()
        runLoop()        
    }
    
    func introduction() {
        show("Welcome to The \(availableDataModel.name)!")
        show("There are \(availableDataModel.quantityWithUnits).")
    }
    
    mutating func setStudent() {
        show("What is your name?")
        student = ask("Your name")
        show("Hello, \(student!).")
    }
    
    func runLoop() {
        var decision: String
        repeat {  
            show("Select element Randomly?")
            decision = ask("Y or N")
            switch decision {
            case "Y":
                show("OK! Learning element randomly.")
                let randomAtomicNumber = availableDataModel.elements.randomElement()?.atomicNumber
                learn(to: randomAtomicNumber!)
            case "N":
                show("OK! Select atomic number of element...")
                let userResponse = askForNumber()
                learn(to: userResponse)
            default:
                show("Sorry, I didn't get that.")
            }
        } while !(decision == "Y" || decision == "N")
    }
    
    func learn(to number: Int) {
        for e in availableDataModel.elements {
            if e.atomicNumber == number {
                show("The element with atomic number \(number) is \(e.symbol).")
                return
            }
        }
        show("No match for any element in the data.")
    }
    
}

// DataModel.swift

public struct DataModel {
    let name: String
    var elements: [Element]
    
    var quantityWithUnits: String {
        let quantity = String(self.elements.count) + " element"
        return elements.count > 1 ? quantity + "s" : quantity
    }
    
    public init(name: String, array: [Element]) {
        self.name = name
        self.elements = array
    }
}

// Element.swift
public struct Element {
    let atomicNumber: Int
    let symbol: String
    
    public init(atomicNumber: Int, symbol: String){
        self.atomicNumber = atomicNumber
        self.symbol = symbol
    }
}
```
