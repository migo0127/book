# 結構型模式

結構型模式是介紹如何將「類別」與「物件」結合在一起，組合成功能更強大的結構，從而實現一定的設計目標。類別、物件之間怎樣設計繼承、依賴、組合關係直接影響到整體程式的健壯性、可維護性。



## Bridge（橋接模式）

Bridge（橋接模式）屬於結構型模式，是一種解決繼承後靈活擴展的方案。

![image-20240304150816305](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240304150816305.png)

### 意圖

> 將抽象部份與它的實作部份分離，使它們可以獨立地變化。

橋接模式可將業務邏輯或一個大類拆分為不同的層次結構，從而能獨立地進行開發。

層次結構中的第一層（通常稱為抽象部份）將包含對第二層（實現部份）對象的引用。抽象部份能將一些（有時是絕大部份）對自己的調用委派給實現部份的對象。所有的實現部份都有一個通用接口，因此它們能在抽象部份內部相互替換。



### 意圖解釋

> 將抽象部份與它的實作部份分離，使它們可以獨立地變化。

「抽象」部份與「實作」部份分離，這句話看起來很像介面與實作。確實，如果「抽象」指的是介面（interface)，而實作指的是類別（class）的話，這就是簡簡單單的 ` class MyWindow implements Window` 類別實作過程而已。

但後半句話「使它們可以獨立地變化」會讓你難以和前半句聯繫起來，如果說「抽象」不變，「實作」可以隨意改變還好理解，但反過來就難以解釋了。

其實橋接模式中，抽象指的是一種介面（abstraction），實作指的也是一種介面（implementor），其中 implementor 並不是直接實作了 abstraction 定義的接口，而是提供更底層的方法，使 abstraction 可以基於它們封裝出自己的介面實作。

這樣一來，abstraction 的介面可以隨意變化，畢竟呼叫的是 implementor 提供的函數的組合，只要 implementor 提供的功能全面，implementor 可以不變，對應的，implementor 的實作也可以隨意變化，只要提供的底層函數不變，就不影響 abstraction 對其的使用。

![image-20240304151022462](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240304151022462.png)					<img src="/Users/migo.weng/Library/Application Support/typora-user-images/image-20240305110205135.png" alt="image-20240305110205135" style="zoom:50%;" />

- Abstraction：定義抽象類別的介面。
- RefinedAbstraction：擴充 Abstraction。
- Implementor：定義實作類別的接口，該介面可以與 Abstraction 介面不一致。
- ConcreteImplementor：實作 Implementor 介面並定義它的具體實作。

抽象部份就是 Abstraction，實作部份就是 Implementor，在這個結構圖中，它們是分離的，可以各自獨立變化，橋接模式，就是指 `imp` 這個橋，透過 Implementor 實現 Abstraction 接口，就算是橋接上了，這種組合的橋接相比普通的類別實現更靈活，更具有拓展性。

#### 範例：

```typescript
// 例1：
interface Abstraction {
  operation: () => void;
}

interface Implemention {
  operationImplementation: () => void;
}

// class
class RefindedAbstraction extends Abstraction {
  private _implemention: Implemention;
  
  constructor(implemention: Implemention){
    this._implemention = implemention;
  }
  
  operation(): void {
    this._implemention.operationImplementation();
  }
  
}

class ConcreteImplementationA implements Implemention {
  constructor(){ }
  
  operationImplementation() void {
    console.log(`ConcreteImplementationA: Here's the result on the platform A.`);
  }
}

class ConcreteImplementationB implements Implemention {
  constructor() { }
  operationImplementation(): void {
    console.log(`ConcreteImplementationB: Here's the result on the platform B.`);
  }
}

// instance
const implementationA = new ConcreteImplementationA();
const abstructionA = new RefindedAbstraction(implementationA);
abstructionA.operation(); // ConcreteImplementationA: Here's the result on the platform A.

const implementactionB = new ConcreteImplementationB();
const abstructionB = new RefindedAbstraction(implementactionB);
abstructionB.operation(); // ConcreteImplementationB: Here's the result on the platform B.

```

```typescript
// 例2：不同手機操作（ex：開機、關機、上網、打電話等等）
// interface
export interface IBrand {
  open: () => void;
  close: () => void;
  call: () => void;
}

// class
export abstract class Phone {
  private _brand: IBrand;

  constructor(brand: IBrand) {
    this._brand = brand;
  }

  protected open(): void {
    this._brand.open();
  }

  protected close(): void {
    this._brand.close();
  }

  protected call(): void {
    this._brand.call();
  }
}

export class XiaoMi implements IBrand {
  constructor() {}

  open(): void {
    console.log('小米手机开机了');
  }

  close(): void {
    console.log('小米手机关机了');
  }

  call(): void {
    console.log('小米手机打电话');
  }
}

export class IPhoneX implements IBrand {
  constructor() {}

  open(): void {
    console.log('IPhoneX手机开机了');
  }

  close(): void {
    console.log('IPhoneX手机关机了');
  }

  call(): void {
    console.log('IPhoneX手机打电话');
  }
}

export class Vivo implements IBrand {
  constructor() {}

  open(): void {
    console.log('Vivo手机开机了');
  }

  close(): void {
    console.log('Vivo手机关机了');
  }

  call(): void {
    console.log('Vivo手机打电话');
  }
}

export class FoldedPhone extends Phone {
  constructor(brand: IBrand) {
    super(brand);
  }

  override open(): void {
    super.open();
  }

  override close(): void {
    super.close();
  }

  override call(): void {
    super.call();
  }
}

export class UpRightPhone extends Phone {
  constructor(brand: IBrand) {
    super(brand);
  }

  override open(): void {
    super.open();
  }

  override close(): void {
    super.close();
  }

  override call(): void {
    super.call();
  }
}

// app.component.ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const miaoMi: FoldedPhone = new FoldedPhone(new XiaoMi());
    const vivo: UpRightPhone = new UpRightPhone(new Vivo());
    const iphoneX: FoldedPhone = new FoldedPhone(new IPhoneX());

    miaoMi.open();
    vivo.close();
    iphoneX.call();
  }
}
```



### 弊端

不要過度抽象，橋接模式是為了讓類別的職責更單一，維護更便捷，但如果只是小型項目，橋接模式會增加架構設計的複雜度，而且不正確的模組拆分，把本來關聯的邏輯強制解耦，在未來會導致更大的問題。

另外橋接模式也有簡單與複雜模式之分，只有一種實現的場景就不要用抽象工廠做過度封裝了。



### 總結

橋接模式讓我們重新審視類別的設計是否合理，把類別中不相關，或者說互相獨立的維度抽出去，由橋接模式做橋接的方式使用，這樣會使每個類別功能更內聚，程式碼量更少更清晰，組合能力更強大，更容易做拓展。