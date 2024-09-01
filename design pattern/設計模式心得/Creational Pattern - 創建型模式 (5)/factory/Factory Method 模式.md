# 創建型模式（Creational Pattern）

> 用於管理物件的建立，實例化物件。

創建型模式提供創建物件的機制，抽象化了實例化的過程，將物件的創建和創建的過程細節進行了分離。使結構更加清晰，並符合單一職責原則（SRP）的設計思路。



## 工廠方法模式（Factory Method）

<img src="https://img-blog.csdnimg.cn/555a057e40144086bc58939bdcca94aa.png" alt="img" style="zoom:60%;" />

### 意圖

> 定義一個用於建立物件的接口，讓子類別決定實例化哪一個類別。Factory Method 使一個類別的實例化延遲到其子類別。



舉例子

1. 換燈泡

我自己在家換過燈泡，以前我家裡燈壞掉的時候，我看著這個奇形怪狀的燈管，心裡想，這種燈泡和這個燈座應該是一體的，市場上估計很難買到適配我這個燈座的燈泡了。結果等我把燈泡擰下來，跑到門口的五金店去換的時候，店員隨便給了我一個燈泡，我回去隨便擰了一下居然就能用了。

我買這個燈泡的過程就用到了工廠模式，而正是得益於這種模式，讓我可以方便在家門口就買到可以用的燈泡。



2. 卡牌對戰遊戲

卡牌對戰中，卡牌有一些基本屬性，例如攻防、生命值，也符合一些通用約定，例如一回合出擊一起等等，那麼對於戰鬥系統來說，應該怎樣實例化卡牌呢？如何批次操作卡牌，而不是通用功能也要拿到每個卡牌的實例才能呼叫？另外每張卡牌有特殊能力，這些特殊能力又該如何拓展呢？



3. 實現任意圖形拖曳系統

一個可以被交互操作的圖形，它可以用滑鼠進行拉伸、旋轉或移動，不同圖形實現這些操作可能並不相同，要存儲的數據也不一樣，這些數據應該獨立於圖形存儲，我們的系統如果要對接任意多的圖形，具備強大拓展能力，物件關係該如何設計？



### 意圖解釋

> 定義一個用於建立物件的接口，讓子類別決定實例化哪一個類別。Factory Method 使一個類別的實例化延遲到其子類別。

在使用工廠方法之前，我們就要闖建一個用於創建對象的接口，這個接口具備通用性，所以我們可以忽略不同的實現來做一些通用的事情。

依換燈泡的例子來說，去門口五金店買燈泡，而不是拿到燈泡材料自己 New 一個出來，就是因為五金店這個 "工廠" 提供的燈泡符合國家接口標準，而家裡的燈座也符合這個標準，所以燈座不需要知道對接的燈泡是巨體哪個實例，什麼顏色、什麼形狀，這些都無所謂，只要燈泡符合國家標準接口，就可以對接上。

依卡牌對戰的系統來說，所有卡牌都應該實現同一種接口，所以卡牌對戰系統拿到的卡牌應該就是簡單的 Card 類型，這種類型具備基本的卡片操作交互能力，系統就調用這些能力完成基本流程就好了，如果系統直接實例化具體的卡片，那麼不同的卡片類型會導致系統難以維護，卡片間操作也無法抽象化。正式這鞏模式，使得我們可以在卡牌的具體實現上做一些特殊功能，例如修改卡片攻擊時效果，修改卡牌銷毀時效果。

依圖形拖曳系統來說，用到了「連接平行的類層次」這個特幸，所謂連接平行的類層次，就是指一個圖形與其對應的操作類是一個平行抽象類，而一個具體的圖形與具體的操作類別則是另一個平行關係，系統只要專注於最抽象的 "通用操作類別" 即可，操作時，底層可能是某個具體的 "圓類" 與 "圓操作類" 結合使用，具體的類別有不同的實現，但都符合同一種接口，因此操作系統才可以把它們一視同仁，統一操作。

所以介面是非常重要的，工廠方法第一句話就是 "定義一個用於創建物件的介面"，這個介面就是 `Creator`，讓子類，也就是具體的創建類別 `ConcreteCreator` 決定要實例化哪個類別 `ConcreteProduct` 

所謂使一個類別的實例化延遲到其子類，是因為抽象類別不知道要實例化哪個具體類別，所以實例化動作只能由具體的子類別去做，這樣繞一圈的好處是，可以將任意多物件看作是同一類事物，做統一的處理，比如無論何種燈泡實例都滿足通用的燈座接口，所有工廠實例化的卡牌都具備玩一局卡牌遊戲的基本功能任何圖形與互動類別都滿足特定功能關係，這種想法讓生活和設計得到了大幅簡化。

<img src="/Users/migo.weng/Library/Application Support/typora-user-images/image-20240409163122965.png" alt="image-20240409163122965" style="zoom:50%;" />

- Factory Method 角色：

  - `Creator`：抽象工廠類。

  - `ConcreteCreator`：具體工廠類。

  - `Product`：抽象產品類。

  - `ConcreteProduct`：具體產品類。

    `Creator` 就是工廠方法，`ConcreteCreator` 是實現了 `Creator` 的具體工廠方法，每一個具體工廠方法生產一個具體的產品 `ConcreteProduct`，每個具體的產品都實現通用性產品的特性 `Product`。

#### 範例

```typescript
// 例1：工廠方法模式
// interface
// 產品接口
export interface Product {
  save: () => void;
}

// 工廠接口
export interface Creator {
  createProduct: () => Product;
}

// class
// 具體產品
export class ConcreteProduct implements Product {
  save(): void {
   	console.log('ConcreteProduct');
  }
}

// 具體工廠
export class ConcreteCreator implements Creator {
  createProduct(): Product {
    return new ConcreteProduct();
  }
}

// component.ts
export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const factory: ICreator = new ConcreteCreator();
    const product: IProduct = factory.createProduct();
    product.save();
  }
}
```

```typescript
// 例2：生成不同圖形 - 圓形 or 正方形
// interface
// 產品接口
export interface IShape {
  draw: () => void;
}

// 工廠接口
export interface IShapeFactory {
  createShape: (shape: string) => IShape;
}

// class
// 具體形狀類
export class Circle implements IShape {
  draw(): void {
    console.log('draw a circle.');
  }
}

// 具體形狀類
export class Square implements IShape {
  draw(): void {
    console.log('draw a squre.');
  }
}

// 具體工廠類
export class ShapeFacroty implements IShapeFactory {
  createShape(shape: string): IShape {
    switch (shape) {
      case 'square':
        return new Square();
      default:
        return new Circle();
    }
  }
}

// component.ts
export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const shapeFacroty: IShapeFactory = new ShapeFacroty();
    const circle: IShape = shapeFacroty.createShape('circle');
    const square: IShape = shapeFacroty.createShape('square');

    circle.draw();
    square.draw();
  }
}
```

```typescript
// 例3: 不同產品的車
// interface 
// 接口車
export interface ICar {
  showInfo: () => void;
}

// 接口車工廠
export interface ICarFactory {
  createCar: (name: string) => ICar;
}

// class
// 通用 car 具體類
export class Car implements ICar {
  private _name: string;

  constructor(name: string) {
    this._name = `${name} - ${new Date()
      .toLocaleDateString('zh-TW', {
        year: 'numeric',
        month: '2-digit',
        day: '2-digit',
      })
      .replace(/\//g, '')}`;
  }

  showInfo(): void {
    const time: string = new Date(Date.now()).toLocaleTimeString('zh-TW', {
      hour12: false,
    });
    console.log(`This is ${this._name} ${time}`);
  }
}

// 具體車類：繼承通用車具體類
export class Audi extends Car {
  constructor(name: string) {
    super(name);
  }
}

// 具體車類：繼承通用車具體類
export class BMW extends Car {
  constructor(name: string) {
    super(name);
  }
}

// 具體車類：繼承通用車具體類
export class Nissan extends Car {
  constructor(name: string) {
    super(name);
  }
}

// 具體車工廠
export class CarFactory implements ICarFactory {
  createCar(name: string): ICar {
    switch (name) {
      case 'Audi':
        return new Audi(name);
      case 'Nissan':
        return new Nissan(name);
      default:
        return new BMW(name);
    }
  }
}

// component.ts
export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const carFactory: ICarFactory = new CarFactory();
    const audi: ICar = carFactory.createCar('Audi');
    const nissan: ICar = carFactory.createCar('Nissan');
    const bmw: ICar = carFactory.createCar('BMW');

    setTimeout(() => {
      audi.showInfo();
    }, 0);

    setTimeout(() => {
      nissan.showInfo();
    }, 1000);

    setTimeout(() => {
      bmw.showInfo();
    }, 2000);
  }
}
```



### 弊端

工廠方法中，每創建一種具體的子類，就要寫一個對應的 `ConcreteCreate`，這相對比較笨重，如果將創建多個對象放到一個 `ConcreteCreate` 中，就變成了簡單工廠模式，新增捵品要修改已有類不符合開閉模式。

彼之毒藥吾之蜜糖，要知道沒有一種設計模式解決所有問題，沒有一種設計模式沒有弊端，而這個弊端不代表這個設計模式不好，一個弊端的出現可能是為了解決另一個痛點。要接受不完美的存在，這麼多設計模式就是對應了不同的業務場景，為合適的場景選擇一種能將優勢發揚光大，以至於能掩蓋弊端，就算進行了合理的架構設計。



### 總結

工廠方法並不是簡單把 new 的過程換成了函數，而是抽象出一套面向介面的設計模式：

<img src="https://ask.qcloudimg.com/http-save/yehe-4823808/1a72f50af9bc4708c5a1eba47a8232d4.png" alt="img" style="zoom:80%;" />

要做燈泡，可以直接做具體的燈泡，也可以定義一個燈泡接口，透過燈泡工廠拿到具體燈泡，燈泡工廠對待所有燈泡的製作流程都是一樣的，不管是中世紀風燈泡，還是復古風燈泡，亦或是普通白織燈，都是一模一樣的製作流程，具體怎麼做由具體的子類去實現，這樣我們可以統一管理 "燈泡" 這一個通用概念，而忽略不同燈泡之間不太重要的差別，程式的可維護性大幅提升。