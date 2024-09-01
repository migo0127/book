# 結構型模式 Structural Pattern

結構型模式是介紹如何將「類別」或「物件」結合在一起，組合成功能更強大的結構，從而實現一定的設計目標。類別、物件之間怎樣設計繼承、依賴、組合關係直接影響到整體程式的健壯性、可維護性。



## Facade（外觀模式）

Facade（外觀模式）屬於 **結構型模式**，是日常開發中常被使用的設計模式。

<img src="https://ithelp.ithome.com.tw/upload/images/20181029/20112528NteGp12xG0.png" alt=" Facade Pattern" style="zoom:75%;" />

### 意圖

為子系統中的一組接口提供一個一致的界面，Facade 模式定義了一個高層接口，這個接口使得這一子系統更容易使用。

為降低一個擁有多個接口的子系統內部複雜性，我們需要一個外觀來屏蔽內部的複雜性，因此外觀模式就是定義一個高層接口，這個接口直連子系統的內部實現，但掉用這個高層接口的人不需要關心子系統內部的實現，這樣，對於不想了解子系統內部實現的人來說，提高了易用度。

當然如果想要深度定制，就可以繞過外觀模式，直接使用子系統提供的類，所以說並不是有了外觀模式就必需通過外觀調用，而是根據實際需要判斷使用哪種調用方式。

### 意圖解釋

圖書館是一個非常複雜的系統，雖然圖書按照一定規則擺放，但也只有內部人員比較清楚，作為一位初次來的訪客，想要快速找到一本書，最好的辦法是直接問圖書管理員，而不是先了解這個圖書館的設計，因為你可能要來回在各個樓宇間奔走，借書的流程可能也比較長。

圖書館員就起到了簡化圖書館子系統複雜度的作用，我們只要凡事詢問圖書館員即可，而不需要關心他是如何與圖書館內部系統打交道的。

#### 範例：

```typescript
// 例1：假設一個子系統是三個類結合使用的，為了抽象而解藕開了
class A {
  b: B;
  constructor(b: B){
    this.b = b;
  }
}

class B {
  c: C;
  constructor(c: C){
    this.c = c;
  }
}

class C {
  constructor(){}
}

// 它們組合成了一種常用功能，我們可以使用外觀模式屏蔽子類別的細節直接使用
class Compile {
 	constructor() {}
  run(): void {
    const parse = new A(new B(new C));
    parse.run();
  }
}

// 這樣只需要知道 Compile 類就可以了，不需要了解後的 A、B、C 類及其組合關係
const compile = new Compile();
compile.run();


// 例2: https://stackblitz.com/edit/angular14-sharebuttons-fbjssa?file=src%2Fapp%2Fapp.component.ts,src%2Fapp%2Fapp.component.html

// 例3:
class SubClassOne {
  methodOne(): void {
    console.log(1);
  }
}

class SubClassTwo {
  methodTwo(): void {
    console.log(2);
  }
}

class SubClassThree {
  methodThree(): void {
    console.log(3);
  }
}

class SubClassfour {
  methodFour(): void {
    console.log(4);
  }
}

class Facade {
  private _one: SubClassOne;
  private _two: SubClassTwo;
  private _three: SubClassThree;
  private _four: SubClassfour;
  
  constructor() {
    this._one = new SubClassOne();
    this._two = new SubClassTwo();
    this._three = new SubClassThree();
    this._four = new SubClassfour();
  }
  
  // constructor(one: SubClassOne, two: SubClassTwo, three: SubClassThree, four: SubClassfour) {
  //  this._one = one;
  //  this._two = two;
  //  this._three = three;
  //  this._four = four;
  // }
  
  methodA(): void {
    this._one.methodOne();
    this._four.methodFour();
  }
  
  methodB(): void {
    this._two.methodTwo();
    this._three.SubClassThree();
  }
}

const facade = new Facade();
// const facade = new Facade(new SubClassOne(), new SubClassTwo(), new SubClassThree(), new SubClassfour());
facade.mothodA(); // 1 4
facade.mothodB(); // 2 3
```



### 弊端

外觀模式並不適用所有場景，當子系統夠易用時，再使用外觀模式就是畫蛇添足。另外，當系統難以抽象出通用功能時，外觀模式的設計可能也無所適從，因為設計的高層介面可能適用範圍很窄，此時外觀模式的意義就比較小。



### 總結

其實抽象工廠模式也可以取代外觀模式，來實現隱藏子類別具體實現的效果，但外觀模式描述更具通用性。