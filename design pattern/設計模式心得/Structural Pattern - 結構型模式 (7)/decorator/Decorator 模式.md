# 結構型模式

結構型模式是介紹如何將「類別」與「物件」結合在一起，組合成功能更強大的結構，從而實現一定的設計目標。類別、物件之間怎樣設計繼承、依賴、組合關係直接影響到整體程式的健壯性、可維護性。



## 意圖

> 動態地為一個物件添加一些額外的職責。就增加功能來說，Decorator 模式相比產生子類別更為靈活。

說明例子：

1. 相框：

   照片＋相框 = 相框的照片，這背後就是一種裝飾器模式：照片具有看的功能，相框具有裝飾功能，在你看照片的基礎上，還能看到精心設計的相框，增加了美感，同時相框還可以增加照片的保存時間與安全性。

   

2. 帶有快取的文件讀寫：

   假設我們有一個類別 `FileI0` 用來讀寫文件，但是沒有快取功能，此時是新建一個 `CachedFileI0` 子類別好，還是創建一個 `CachedI0` ?

   一眼看上去好像 `CachedFileI0` 用起來更方便，而 `CacheI0` 的用法是 `new CachedI0(new FileI0())` 稍為麻煩一些，但如果我們增加一個網絡讀寫類 `NetworkI0`，一個數據庫讀寫類 `DBI0` 呢？

   顯然，繼承的方式會使子類別數量極速膨脹，而組合的方式則非常靈活，生成一個支援快取的網路讀寫器，只需要 `Nnew CahedI0(new NetwrokI0())` 即可，這就是組合靈活的地方。

   當然，為了實現這個能力，`CachedI0` 需要與 `FileI0`、`CachedFileI0`、`CahaedI0` 繼承自同一個類，具備相同的介面。

   

3. 搭建平台的元件 Wrapper：

   裝飾器模式別名也叫 `wrapper`，`wrapper` 也常在前端搭建場景中遇到，當搭建平台載入一個元件時，希望拓展其基礎能力，一般會使用 `wrapper` 層對元件進行嵌套，`wrapper` 層就是在不改變 API 的基礎上，對第三方組件進行增強。	



## 意圖解釋

> 動態地為一個物件添加一些額外的職責。就增加功能來說，Decorator 模式相比產生子類別更為靈活。

不同於繼承，組合可以在運行時進行，所以稱之為「動態添加」，這裡的「額外職責」泛指一切功能，比如在按鈕點擊時進行一些 log 日誌的打印，在繪製 text 文本框時，額外增加一個捲軸和邊框等等。

「就增加功能來說，Decorator 模式相比生成子類別更為靈活」這句話的含義是，組合比繼承靈活，當可拓展的功能很多時，繼承方案會產生大量的子類，而組合可以提前寫好處理函數，在需要時動態建構，顯然是更靈活的。

![image-20240325113721155](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240325113721155.png)

`ConcreteComponent` 指的是需要被裝飾的組件，可以看到，裝飾器 `Decorator` 與他都繼承同一個類，這樣能保證 API 的一致，才能保證無論裝飾多少層，始終符合 `Component` 類型。

裝飾器如果有多種，就要將 `Decorator` 申明為抽象類，`ConcreteDecoratorA`、`ConcreteDecoratorB` 分別實現它們，如果只有一種裝飾器，可以退化到 `Decorator` 自身就是一種實現。



### 範例：

```typescript
// 例1： oo 咖啡 => 純咖 vs 加奶 vs 加糖 => 成本
// class
// 咖啡抽象類
abstract class Coffee {
  abstract getDescription(): string;
  abstract getCost(): number;
}

// 咖啡抽象類的裝飾器：繼承咖啡抽象類，並聚合咖啡抽象類
abstract class CoffeeDecorator extends Coffee {
  // 被裝飾的咖啡
  beDecorated: Coffee;
  
  constructor(beDecorated: Coffee){
    super();
    this.beDecorated = beDecorated;
  }
}

// 美式咖啡
class AmericaCoffee extends Coffee {
  getDescription(): string {
    return 'America coffee';
  }
  
  getCost(): number {
    return 10;
  }
}

// 意式咖啡
class ItalyCoffee extends Coffee {
  getDescription(): string {
    return 'Italy coffee';
  }
  
  getCost(): number {
    return 20;
  }
}

// 加糖 oo 咖啡
class SugerCoffeeDecorator extends CoffeeDecorator {
  constructor(beDecorator: Coffee){
    super(beDecorator);
  }
  
  getDescription(): string {
    return this.beDecorator.getDesctiption() + ' with Suger';
  }
  
  getCost(): number {
    reutrn this.beDectorator.cost() + 1; // 裝飾後的價格是原價 + 1
  }
}

// 加奶 oo 咖啡
class MilkCoffeeDecorator extend CoffeeDecorator {
  consturctor(beDecorator: Coffee){
    super(beDecorator);
  }
  
  getDescription(): string {
    return this.beDecorator.getDescription() + ' with Milk';
  }
  
  getCost(): number {
    return this.beDecorator.getCost() + 5;
  }
}

// component.ts
// 純美式咖啡
const americaCoffee = new AmericaCoffee(); 
// 美式咖啡加糖
const americaCoffeeWithSugerDecorator = new SugerCoffeeDecorator(americaCoffee);
// 美式咖啡加奶
const americaCoffeeWithMilkDecorator = new MilkCoffeeDecorator(americaCoffee);

console.log(`${americaCoffee.getDescription()}: ${americaCoffee.getCost()}`);
console.log(`${americaCoffeeWithSugerDecorator.getDescription()}: ${americaCoffeeWithSugerDecorator.getCost()}`);
console.log(`${americaCoffeeWithMilkDecorator.getDescription()}: ${americaCoffeeWithMilkDecorator.getCost()}`);
```

```typescript
// 例2：圖形 => 畫圓 or 方 => 添加 o 色邊框的圖形
// interface
export interface IText {
  print: (text: string, style: string) => void;
}

// class
export class TextClass implements IText {
  print(text: string, style: string = '') {
    console.log(text);
  }
}

// decorator class
export class TextDecorator implements IText {
  textDecorator!: IText;

  constructor(textDecorator: IText) {
    this.textDecorator = textDecorator;
  }

  print(text: string, style: string = ''): void {
    console.log(text, style);
  }
}

export class YellowStyleDecorator extends TextDecorator {
  constructor(textDecorator: IText) {
    super(textDecorator);
  }

  override print(text: string, style: string = ''): void {
    super.print(`%c${text}`, `${style}color: yellow;`);
  }
}

export class BoldStyleDecorator extends TextDecorator {
  constructor(textDecorator: IText) {
    super(textDecorator);
  }

  override print(text: string, style: string = '') {
    super.print(`%c${text}`, `${style}font-weight: bold;`);
  }
}

export class BigSizeStyleDecorator extends TextDecorator {
  constructor(textDecorator: IText) {
    super(textDecorator);
  }

  override print(text: string, style: string = '') {
    super.print(`%c${text}`, `${style}font-size: 36px;`);
  }
}

// component.ts
export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const text: IText = new TextClass();
    const yellowTextDecorator: IText = new YellowStyleDecorator(text);
    const boldTextDecorator: IText = new BoldStyleDecorator(text);
    const bigSizeTextDecorator: IText = new BigSizeStyleDecorator(text);

    console.log(text.print('something', ''));
    console.log(yellowTextDecorator.print('something', ''));
    console.log(boldTextDecorator.print('something', ''));
    console.log(bigSizeTextDecorator.print('something', ''));
  }
}
```

其實方法很簡單，通過組合，我們得到了一個能力更強的元件，而實現的方式就是利用建構函式保存元件實例，並在重寫函數時，增加一些增強實作。

- 適用場景：

  - 想不修改原有代碼，並在運行時為對象添加額外的行為。

  - 難以通過繼承來擴展對象時。

  - ES7 新增的 decorator，通過 @decorator 類似的關鍵字和方法添加裝飾器。



- 分析：

  - 符合單一職責原則，額外添加的裝飾器都保持完成自身一項職責。

  - 符合開放封閉原則，無需修改原有代碼，增量增加裝飾器即可。

  - 可以用多個裝飾器實現鏈式調用，動態組合行為。

    


## 弊端

裝飾器的問題也是組合的問題，過多的組合會導致：

- 組合過程複雜，要產生過多的物件。
- 包裝器層次增多，會增加調試成本，比較難追溯一個 bug 是在哪一層包裝導致的。



## 總結

裝飾器模式是非常常用的模式，Decorator 是一個透明的包裝，只要確保包裝的透明性，就可以最大限度地發揮裝飾器模式的優勢。





