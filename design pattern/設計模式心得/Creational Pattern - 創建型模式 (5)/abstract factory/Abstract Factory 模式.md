# 創建型模式 Creational Pattern

創建型模式提供創建物件的機制，抽象化了實例化的過程，將物件的創建和創建的過程細節進行了分離。使結構更加清晰，並符合單一職責原則（SRP）的設計思路。



## 抽象工廠模式（Abstract Factory）

![image-20240306115924235](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240306115924235.png)

- 抽象工廠核心結構有四個角色：

  1. 抽象工廠：

     擔任這個角色的是工廠方法模式的核心，它是與應用程式無關的。任何在模式中創建對象的工廠類必須實現這個接口。

  2. 具體工廠：

     擔任這個角色的是實現了抽象工廠接口的具體 class 類，具體工廠角色含有與應用密切相關的邏輯，並且受到應用程式的調用以創建產品對象。

  3. 抽象產品：

     工廠方法模式所創建的對象的超類型，也就是產品對象的共同父類或共同擁有的接口。

  4. 具體產品：

     這個角色實現了抽象產品角色所申明的接口。工廠方法模式所創建的每一個對象都是某個具體產品角色的實例。

     

### 意圖

為了更好理解抽象工廠模式，先引入兩個概念：

<img src="/Users/migo.weng/Library/Application Support/typora-user-images/image-20240306135053776.png" alt="image-20240306135053776" style="zoom:50%;" />

![image-20240322141919685](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240322141919685.png)

1. 產品等級結構：

   產品等級結構即產品的繼承結構，如一個抽象類別是電視機，其子類別有海爾電視機、海信電試機、TCL電視機，則抽象電視機與具體品牌的電視機之間構成了一個產品等級結構，抽象電視機是父類，而具體品牌的電視機是其子類。

2. 產品族：

   在抽象工廠模式中，產品族是指由同一個工廠生產的，位於不同產品等級結構中的一組產品，如海爾電器工廠生產的海爾電視、海爾電冰箱，海爾電視機位於電視機產品等級結構中，海爾電冰箱位於電冰箱產品等級結構中。
   
   

- 抽象工廠模式定義：

  > 提供一個介面以建立一系列相關或相互依賴的對象，而無須指定它們特別的類別。

![image-20240322142033639](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240322142033639.png)

- 抽象工廠模式結構與分析：

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/4/13/17170ec2577083b3~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)

- 抽象工廠模式包含以下角色：
  - AbstractFactory：抽象工廠。
  - ConcreteFactory：具體工廠。
  - AbstractProduct：抽象產品。
  - ConcreteProduct：具體產品。



- 抽象工廠模式分析：

![image-20240322142431309](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240322142431309.png)

![image-20240322142440368](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240322142440368.png)

![image-20240322142450784](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240322142450784.png)



### 意圖解釋

1. 情景 - 汽車工廠

我們都知道汽車有很多零件，隨著工業革命帶來的分工，很多零件都可以輕鬆取代。但實際生活中我們消費者不願意這樣，我們希望買來的寶馬車所包含的零件都是同一系列的，以保證最大的匹配度，從而帶來更好的性能與舒適度。

所以消費者不願意到輪胎工廠、方向盤工廠、車窗工廠去一個個採購，而是將需求提給了寶馬工廠這家抽象工廠，由這家工廠負責組裝。那你是這家工廠的老闆，已知汽車的組成部件是固定的，只是不同配件有不同的型號，分別來自不同的製造廠商，你需要推出幾款不同組合的車型來滿足不同價位的消費者，你會怎麼設計？

2. 情景 - 迷宮遊戲

你做一款迷宮遊戲，已知元素有房間、門、牆，他們之間的組合關係是固定的，你透過一套演算法產生隨機迷宮，這套演算法調用房間、門、牆的工廠生成對應的實例。但隨著新資料片的放出，你需要產生具有新功能的房間（可以回復體力）、新功能的門（需要魔法鑰匙才能打開）、新功能的牆（可以被炸彈破壞），但修改已有的迷宮生成演算法違背了開閉原則（需要在已有物件進行修改），如果你希望生成迷宮的演算法完全不感知新材料的存在，你會怎麼設計？

3. 情景 - 事件聯動

假設我們做一個前端搭建引擎，現在希望做一套關聯機制，以實現點擊表格元件單元格，可以彈出一個模態框，內部展示一個折線圖。已知業務方存在定製表格元件、模態框元件、折線圖元件的需求，但元件之間連動關係是確定的，你會怎麼設計？



在汽車工廠的例子中，我們已知車子的構成部件，為了組裝成一輛車子，需要以一定方式拼裝部件，而巨體用什麼部件是需要可拓展的。

在迷宮遊戲的例子中，我們已知迷宮的組成部分是房間、門、牆，為了產生迷宮，需要以某種演算法產生許多房間、門、牆的實例，而具體用哪種房間、哪一種門、哪一種牆是這個演算法不關心的，是需要可被拓展的。

在事件連動的例子中，我們已知這個表格彈出趨勢圖的互動場景基本上組成元素是表格元件、模態框元件、折線圖元件，需要以某種連動機制讓這三者間產生連動關係，而具體是什麼表格、什麼模態框元件、什麼折線圖元件是這個事件連動所不關心的，是需要可以被拓展的，表格可以被替換為任意業務方註冊的表格，只要滿足點擊`onClick` 機制就可以。

這三個例子不正是符合上面的意圖嗎？我們要設計的抽象工廠就是要創造一系列相關或相互依賴的對象，在上面的例子中分別是汽車的組成配件、迷宮遊戲的素材、事件連動的組件。而無須指定它們具體的類，也就說明了我們不關心車子方向盤用的是什麼牌子，迷宮的房間是不是普通房間，聯動機制的折線圖是不是用畫 `Echarts` 的，我們只要描述好他們之間的關係即可，這帶來的好處是，未來我們拓展新的方向盤、新的房間、新的折線圖時，不需要修改抽象工廠。

![image-20240306135835494](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240306135835494.png)

`AbstractFactory` 就是我們想要的抽象工廠，描述了創造產品的抽象關係，例如描述迷宮如何生成，表格和趨勢圖怎麼連結。至於具體用什麼方向盤、用什麼房間，是由 `ConcreteFactory` 實現的，所以我們可能會有多個 `ConcreteFactory`，比如 `ConcreteFactory1` 實例化牆壁是普通牆壁，`ConcreteFactory2` 實例化的牆壁是魔法牆壁，但其對 `AbstractFactory` 的接口是一致的，所以 `AbstractFactory` 不需要關心具體調用的是哪一個工廠。 

`AbstractProudct` 是產品抽象類，描述了例如方向盤、牆壁、折線圖的創建方法，而 `ConcreteProduct` 是具體實現產品的方法，例如 `ConcreteProduct1` 創建的表格是用 `Canvas` 畫的，折線圖是用 `G2` 畫的，而 `ConcreteProduct2` 創建的表格是用 `div` 畫的，折線圖是用 `Echarts` 畫的。

這樣，當我們要拓展一個用 `Rcharts` 畫的折線圖，用 `svg` 畫的表格，用 `div` 畫的模態框組成的事件機制時，只需要再創建一個 `ConcreteProduct3` 做相應的實現即可，再將這個 `ConcreteProduct` 傳遞給 `AbstractFactory`，並不需要修改 `AbstractFactroy` 方法本身。

#### 範例：

https://juejin.cn/post/6844904122710245383

```typescript
// 例1:
// interface
export interface IAbstractFactory {
  createProductA: () => IAbstractProductA;
  creatrProductB: () => IAbstractProductB;
}

export interface IAbstractProductA {
  useFulFunctionA: () => string;
}

export interface IAbstractProductB {
  useFulFunctionB: () => string;
  anotherUseFulFunctionB: (collaborator: IAbstractProductA) => string;
}

// class
export class ConcreteProductA1 implements IAbstractProductA {
  constructor() {}

  useFulFunctionA(): string {
    return 'The result of the product A1.';
  }
}

export class ConcreteProductA2 implements IAbstractProductA {
  constructor() {}

  useFulFunctionA(): string {
    return 'The result of the product A2.';
  }
}

export class ConcreteProductB1 implements IAbstractProductB {
  constructor() {}

  useFulFunctionB(): string {
    return 'The result of the product B1.';
  }

  anotherUseFulFunctionB(collaborator: IAbstractProductA): string {
    const result: string = collaborator.useFulFunctionA();
    return `The result of the B1 collaborating with the (${result})`;
  }
}

export class ConcreteProductB2 implements IAbstractProductB {
  constructor() {}

  useFulFunctionB(): string {
    return 'The result of the product B2.';
  }

  anotherUseFulFunctionB(collaborator: IAbstractProductA): string {
    const result: string = collaborator.useFulFunctionA();
    return `The result of the B2 collaborating with the (${result})`;
  }
}

export class ConcreteFactory1 implements IAbstractFactory {
  constructor() {}

  createProductA(): IAbstractProductA {
    return new ConcreteProductA1();
  }

  creatrProductB(): IAbstractProductB {
    return new ConcreteProductB1();
  }
}

export class ConcreteFactory2 implements IAbstractFactory {
  constructor() {}

  createProductA(): IAbstractProductA {
    return new ConcreteProductA2();
  }

  creatrProductB(): IAbstractProductB {
    return new ConcreteProductB2();
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
    const factory1: IAbstractFactory = new ConcreteFactory1();
    const prodcutA1: IAbstractProductA = factory1.createProductA();
    const productB1: IAbstractProductB = factory1.creatrProductB();

    console.log(`prodcutA1: ${prodcutA1.useFulFunctionA()}`);
    console.log(`productB1: ${productB1.anotherUseFulFunctionB(prodcutA1)}`);

    console.log('----------------');

    this.clientCode(new ConcreteFactory2());
  }

  clientCode(factory: IAbstractFactory): void {
    const productA: IAbstractProductA = factory.createProductA();
    const productB: IAbstractProductB = factory.creatrProductB();

    console.log(`clientCode: productA: ${productA.useFulFunctionA()}`);
    console.log(
      `clientCode: productB: ${productB.anotherUseFulFunctionB(productA)}`
    );
  }
}
```

 ```typescript
 // 例2： fast food factory
 // interface
 export interface IPizza {
   createPizza: () => void;
 }
 
 export interface IHamburger {
   createHamburger: () => void;
 }
 
 export interface IFastFoodFactory {
   createPizza: () => IPizza;
   createHamburger: () => IHamburger;
 }
 
 // class
 export class SinglePizza implements IPizza {
   constructor() {}
 
   createPizza(): void {
     console.log('單人披薩套餐');
   }
 }
 
 export class FamilyPizza implements IPizza {
   constructor() {}
 
   createPizza(): void {
     console.log('家庭披薩套餐');
   }
 }
 
 export class SingleHamburger implements IHamburger {
   constructor() {}
 
   createHamburger(): void {
     console.log('單人漢堡套餐');
   }
 }
 
 export class FamilyHamburger implements IHamburger {
   constructor() {}
 
   createHamburger(): void {
     console.log('家庭漢堡套餐');
   }
 }
 
 export class SingleSetFactory implements IFastFoodFactory {
   constructor() {}
 
   createPizza(): IPizza {
     return new SinglePizza();
   }
 
   createHamburger(): IHamburger {
     return new SingleHamburger();
   }
 }
 
 export class FamilySetFactory implements IFastFoodFactory {
   constructor() {}
 
   createPizza(): IPizza {
     return new FamilyPizza();
   }
 
   createHamburger(): IHamburger {
     return new FamilyHamburger();
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
     const sigleSetFactory: IFastFoodFactory = new SingleSetFactory();
     const familySetFacroty: IFastFoodFactory = new FamilySetFactory();
 
     console.log(
       this.useFacroty(
         Math.random() * 10 < 5 ? sigleSetFactory : familySetFacroty
       )
     );
   }
 
   useFacroty(facroty: IFastFoodFactory): void {
     const pizza: IPizza = facroty.createPizza();
     const hamburger: IHamburger = facroty.createHamburger();
     pizza.createPizza();
     hamburger.createHamburger();
   }
 }
 ```

```typescript
// 例3：形狀與顏色
// interface
export interface IShape {
  draw: () => string;
}

export interface IColor {
  fill: () => string;
}

export interface IPrintShapeFacroty {
  createShape: () => IShape;
  setColor: () => IColor;
}

// class
export class Circle implements IShape {
  constructor() {}

  draw(): string {
    return '畫一個圓形';
  }
}

export class Square implements IShape {
  constructor() {}

  draw(): string {
    return '畫一個正方形';
  }
}

export class Red implements IColor {
  constructor() {}

  fill(): string {
    return '塗上紅色';
  }
}

export class Blue implements IColor {
  constructor() {}

  fill(): string {
    return '塗上藍色';
  }
}

export class CircleFactory implements IPrintShapeFacroty {
  constructor() {}

  createShape(): IShape {
    return new Circle();
  }

  setColor(): IColor {
    return new Red();
  }
}

export class SquareFactory implements IPrintShapeFacroty {
  constructor() {}

  createShape(): IShape {
    return new Square();
  }

  setColor(): IColor {
    return new Blue();
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
    const circleFacroty: IPrintShapeFacroty = new CircleFactory();
    const squareFacroty: IPrintShapeFacroty = new SquareFactory();
    this.useFactory(squareFacroty);
  }

  useFactory(factory: IPrintShapeFacroty): void {
    const shape: IShape = factory.createShape();
    const color: IColor = factory.setColor();
    console.log(`先${shape.draw()}，在${color.fill()}`);
  }
}
```

```typescript
// 例4：迷宮
// interface 
export interface IDoor {
  createDoor: () => string;
}

export interface IRoom {
  createRoom: () => string;
}

export interface IWall {
  createWall: () => string;
}

export interface IMazeFactory {
  createDoor: () => IDoor;
  createRoom: () => IRoom;
  createWall: () => IWall;
}

// class
export class NormalDoor implements IDoor {
  constructor() {}

  createDoor(): string {
    return '創建了一個普通門';
  }
}

export class MagicDoor implements IDoor {
  constructor() {}

  createDoor(): string {
    return '創建了一個魔法門';
  }
}

export class LavaDoor implements IDoor {
  constructor() {}

  createDoor(): string {
    return '創建了一個熔岩門';
  }
}

export class NormalRoom implements IRoom {
  constructor() {}

  createRoom(): string {
    return '創建了一個普通房間';
  }
}

export class MagicRoom implements IRoom {
  constructor() {}

  createRoom(): string {
    return '創建了一個魔法房間';
  }
}

export class LavaRoom implements IRoom {
  constructor() {}

  createRoom(): string {
    return '創建了一個熔岩房間';
  }
}

export class NormalWall implements IWall {
  constructor() {}

  createWall(): string {
    return '創建了一個普通牆壁';
  }
}

export class MagicWall implements IWall {
  constructor() {}

  createWall(): string {
    return '創建了一個魔法牆壁';
  }
}

export class LavaWall implements IWall {
  constructor() {}

  createWall(): string {
    return '創建了一個熔岩牆壁';
  }
}

export abstract class MazeFactory implements IMazeFactory {
  abstract createDoor(): IDoor;
  abstract createRoom(): IRoom;
  abstract createWall(): IWall;
}

export class NormalMazeFactory extends MazeFactory {
  constructor() {
    super();
  }

  createDoor(): IDoor {
    return new NormalDoor();
  }

  createRoom(): IRoom {
    return new NormalRoom();
  }

  createWall(): IWall {
    return new NormalWall();
  }
}

export class MagicMazeFactory extends MazeFactory {
  constructor() {
    super();
  }

  createDoor(): IDoor {
    return new MagicDoor();
  }

  createRoom(): IRoom {
    return new MagicRoom();
  }

  createWall(): IWall {
    return new MagicWall();
  }
}

export class LavaMazeFactory extends MazeFactory {
  constructor() {
    super();
  }

  createDoor(): IDoor {
    return new LavaDoor();
  }

  createRoom(): IRoom {
    return new LavaRoom();
  }

  createWall(): IWall {
    return new LavaWall();
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
    const normalMazeFactory: MazeFactory = new NormalMazeFactory();
    const magicMazeFactory: MazeFactory = new MagicMazeFactory();
    const lavaMazeFactory: MazeFactory = new LavaMazeFactory();
    // this.useFactory(normalMazeFactory);
    this.useFactory(magicMazeFactory);
    // this.useFactory(lavaMazeFactory);
  }

  useFactory(factory: MazeFactory) {
    const door: IDoor = factory.createDoor();
    const room: IRoom = factory.createRoom();
    const wall: IWall = factory.createWall();
    console.log(
      `${door.createDoor()}、${room.createRoom()}、${wall.createWall()}`
    );
  }
}
```



### 弊端

任何設計模式都有其適用場景，反過來也說明了在某些場景下不適用。

根據上面的三個情景，如果我們的需求不是拓展一個新輪子、新牆壁、新折線圖，而是：

- 汽車工廠要為汽車增加一個新零件：自動架駛系統。
- 迷宮遊戲要新增一個功能材料：陷阱。
- 事件連動要新增一個連動物件：明細趨勢統計表。

像這種情況不是為已有元素新增一套實現，而是實現一些新元素，就會非常複雜，因為我們不僅要為所有 `ConcreteFactory` 新增每一個元素，還要修改抽象工廠，以將新元素與舊元素間建立聯繫，違反了開閉原則。

因此，對於已有元素固定的系統，適合使用抽象工廠，反之不然。



### 總結

抽象工廠對新增現有產品的實現適用，對新增一個產品的種類不適用，可以參考結合了情景的下圖加深理解：

![image-20240306174355336](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240306174355336.png)

拓展一個熔岩素材包是增加一種產品風格，適合使用抽象工廠設計模式 ; 拓展一個陷阱是增加一個產品種類，不適合使用抽象工廠設計模式。為什麼呢？看下圖：

![image-20240306175023031](/Users/migo.weng/Library/Application Support/typora-user-images/image-20240306175023031.png)

創建迷宮這個抽象工廠做的事情，是把已有的房間、門、牆壁建立關聯，因為操作的是抽象類，所以拓展一套具體實現（熔岩素材包）對這個抽象工廠沒有感知，這樣做很容易。

但如果新增一個產品種類 - 陷阱，可以看到，抽象工廠必須將陷阱與前三者重新建立關聯，這就要修改抽象工廠，不符合開放封閉原則。同時，如果已有素材包 1~999 個，就需要同時增加 1~ 999 個對應的陷阱實現（普通陷阱、魔法陷阱、熔岩陷阱），其工作量會非常大。

因此，只有產品種類穩定時，需要頻繁拓展產品風格時，才適合抽象工廠設計模式。

