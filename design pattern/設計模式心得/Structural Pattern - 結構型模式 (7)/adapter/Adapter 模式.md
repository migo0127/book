# 結構型模式 Structural Pattern

結構型模式是介紹如何將「類別」或「物件」結合在一起，組合成功能更強大的結構，從而實現一定的設計目標。類別、物件之間怎樣設計繼承、依賴、組合關係直接影響到整體程式的健壯性、可維護性。



## 適配器模式（Adapter）

Adapter（適配器模式）屬於 **結構型模式**，是日常開發中常被使用的設計模式。

<img src="/Users/migo.weng/Library/Application Support/typora-user-images/image-20240222180358825.png" alt="image-20240222180358825" style="zoom:50%;" />

<img src="/Users/migo.weng/Library/Application Support/typora-user-images/image-20240222174249241.png" alt="image-20240222174249241" style="zoom:50%;" />

- Target（轉接的介面）：

  定義著所需要的方法，可以是抽象類別或是介面。如：國外的插座。（白話文：想要變成的對象）

- Adapter（介面的實作）：

  為呼叫和配接的元件中的组件接口。如：身上的電器插頭。（白話文：想變身的對象）

- Adaptee（提供的類別）:

  為一個轉換器，通過繼承或是合成的方式，將 Adaptee 的接口轉換成 Adapter 接口，Client 直接呼叫Adapter 接口就可以執行 Adaptee 的方法。如：轉接頭。（白話文：把想變身的對象變成他想變成的對象）

  

### 意圖

適配器模式通常用來：

- 介面不相容（Interface Incomplatibility）：

  當有一個現有的類別，它提供了一個介面，但這個介面與你需要的介面不相容時，可以使用 Adapter 模式。Adapter 模式將現有類別的介面轉換為符合你需求的新介面。

  

- 重用現有類別（reusing Existing Classes）：

  當想要重用一個已經存在的類別，但它的介面不符合你的需求時，Adapter 模式允許你將這個類別包裝起來，使其能夠與其他代碼協同工作，而不需要修改原有的類別。

  

- 將多個類別整合為一個介面（intergrating Multiple Classes into One Interface）：

  有時你可能需要將多個不同的類別整合成一個統一的介面，以簡化客戶端代碼。Adapter 模式可以實現這種整合。

  

總之，Adapter 模式的主要目的是使不相容的介面能夠共同工作，同時還可以重用現有的程式碼，提高程式碼的可維護性和擴展性。



### 意圖解釋

介面轉換器的概念，譬如插座的種類很多，我們都用過許多轉接頭，將不同的插頭轉換，可以在不替換插座的情況下正常使用。

USB 介面轉換也同樣精彩，有將 TypeC 介面轉換成 TypeA 的，也有將 TypeA 介面轉換成 TypeC 的，支援雙向轉換。

介面轉換器就是我們在生活種使用到的適配器模式，因為廠商並沒有生產一個新的插座，我們也沒有因為接口不適配而換一個手機，一切只需要一個接口轉換器（接頭轉換器）即可。



因此適配器模式需滿足下面兩個條件：

- 介面不相容：插座與插頭不相容。
- 但能力已支援：插座都擁有充電或讀取能力。

#### 範例：

```typescript
// 例：為都有「顯示」行為的點、線、正方行分別建立類別，客戶物件不必知道是擁有點、線還是正方形，它們只需知道擁有這些形狀

interface Shape {
  display: () => void;
  fill: () => void;
  undisplay: () => void;
}

class Point implements Shape {
  constructor() {}
  
  display(): void {
    console.log('Point display');
  }
  
  fill(): void {
    console.log('Point fill');
  }
  
  undisplay(): void {
    console.log('Point undisplay');
  }
}

class Line implements Shape {
  constructor() {}
  
  display(): void {
    console.log('Line display');
  }
  
  fill(): void {
    console.log('Line fill');
  }
  
  undisplay(): void {
    console.log('Line undisplay');
  }
}

class Square implements Shape {
  constructor() {}
  
  display(): void {
    console.log('Square display');
  }
  
  fill(): void {
    console.log('Square fill');
  }
  
  undisplay(): void {
    console.log('Square undisplay');
  }
}

// 客戶追加一個圓形
// 發現已有一個現有類別 XXXCircle 可以使用，故透過適配器轉接
interface IXXXCircle {
  displayIt: () => void;
  fillIt: () => void;
  undisplay: () => void;
}

class XXXCircle implements IXXXCircle {
  // XXXCircle 的實際實現
  displayIt(): void {
    console.log('XXXCircle display');
  }

  fillIt(): void {
    console.log('XXXCircle fill');
  }

  undisplay(): void {
    console.log('XXXCircle undisplay');
  }
}

class CircleAdapter implements Shape {
  
  private _XXXCircle: XXXCircle;
  
  constructor() {
    this._XXXCircle = new XXXCircle();
  }
  	
  display(): void {
    this._XXXCircle.displayIt();
  }
  
  fill(): void {
    this._XXXCircle.fillIt();
  }
  
  undisplay(): void {
    this._XXXCircle.undisplay();
  }
  
}
```

```typescript
interface ITarget {
  request: () => void
}

export interface IAdaptee {
  specificRequest: () => void;
}

class Adaptee implements IAdaptee {
  
  constructor(){}
  
  specificRequest(): void {
    console.log('run specificRequest in Adaptee class');
  }
}

class Adapter extends Adaptee implements ITarget {
  
  constructor(){
    super();
  }
  
  request(): void {
    super.specificRequest();
  }
}

const adapter: ITarget = new Adapter();
adapter.request();
```



### 弊端

使用適配器模式本身就可能是個問題，因為一個好的系統內部不應該做任何橋界，模型應該保持一致性。只有在如下情況才考慮使用適配器模式：

1. 新舊系統接替，改造成本非常高。

2. 三方包適配。

3. 新舊 API 相容。

4. 統一多個類別的介面，一般可以結合工廠方法使用。

   

### 總結

適配器模式也符合開放封閉原則，在不對原有物件改造的前提下，建構一個適配器就能完成模組銜接。適配器模式的實作分為類別與物件模式：

- 類別模式：繼承。
- 物件模式：組合。