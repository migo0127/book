# 行為型模式 Behavioral Pattern

> 封裝變化。

行為型模式負則對象之間的溝通與職責劃分，其除了關注結構之外，更關注他們之間的溝通機制。透過行為型模式可以更清楚的劃分類別、物件之間的職責，展現實例物件之間的作用互動。



## 模版（範本）方法模式（Template Method）

### 意圖

> 定義一個操作中的演算法的骨架，而將一些步驟延遲到子類別。Template Method 使得子類別可以不改變一個演算法的結構即可重定義該演算法的某些特定步驟。

當要完成某個過程，該過程要執行一系列步驟，而這一系列步驟的基本邏輯相同但個別步驟在實現時可能不同的情況下，通常考慮使用模版方法模式來處理。

- 舉個例子

1. 模版文件：

   我們辦事列印的文件就是模版文件，只需要寫上個人基本資料再簽字就可以了，不需要做太多的重複勞動，因為某些場景下大部份內容是可以固化下來的。例如：買賣房屋，那大部份甲方乙方的條款是固定的，最大的變化是甲方與乙方的不同，我們在模版上簽字時，就是利用了模版式減少了大量的寫條款的時間。

   

2. 實例化：

   實例化也可以被認為是模版模式的某種表現形式，因為對於工廠方法，我們傳入不同的初始值可能給出不同結果，那麼實際上就是用很少的程式碼撬動了很大一塊功能，起到了抽象作用。

   

3. Angular 模版：

   Angular 模版更符合我們對模畚直覺的理解。在這個場景中，模版指的是 HTML 模版，只需要在模版中以 `{{}}` 形式描述一些變量，就可以生成一塊只有局部變量變化的模版 DOM，非常方便。



### 意圖解釋

> 定義一個操作中的演算法的骨架，而將一些步驟延遲到子類別。Template Method 使得子類別可以不改變一個演算法的結構，即可重定義該演算法的某些特定步驟。

這個設計模式初衷是用於物件導向的，所以考慮的是如何在類別中運用模版模式。先定義一個父類，實作一些演算法，再將需要被子類重載的方法提出來，子類重載這些部份方法後，即可利用父類實作好的演算法做一些功能。

比方說父類別方法 `function a(){ b() + c() }`，此時子類別只需要重定義 `b` 與 `c` 方法，即可重複使用 `a` 的演算法（ b 與 c 相加）。當然這個例子比較簡單，當演算法較為複雜時，模版模式的好處就會凸顯出來。

<img src="https://ask.qcloudimg.com/http-save/yehe-4823808/6617301d295f78807bfa851e46ab9693.png" alt="img" style="zoom:50%;" />

- `ConcreteClass`：具體的父類別。可以看到父類別中實作了 `TemplateMethod`，其呼叫了 `PrimitiveOperation1` 與 `PrimitiveOperation2`，所以子類別只需要重載這兩個方法，即可享用 `TemplateMethod` 提供的演算法。

  假設 `TemplateMethod` 是 `OpenDocument` 開啟文件的作用，那麼 `PrimitiveOperation1` 可能是 `CanOpen` 校驗，`PrimitiveOperation2` 可能是 `ReadDoucment` 讀取文件的方法。

  我們只要專心實現具體的細節方法，而不需要關心他們之間是如何互動的，父級會幫我們實現它。之後我們就可以呼叫子類別的 `OpenDocument` 實作開啟文件了。



#### 範例

```typescript
// 例1：
// 抽象類
export abstract class AbstractClass {
  constructor() {}

  // 模版方法
  template(): void {
    this.operation1();
    this.hookMethod() && this.operation2();
    this.operation3();
  }

  // 基本方法
  protected operation1(): void {
    console.log('使用了方法 operation1');
  }

  protected operation2(): void {
    console.log('使用了方法 operation2');
  }

  protected operation3(): void {
    console.log('使用了方法 operation3');
  }

  // 勾子方法
  protected hookMethod(): boolean {
    return true;
  }
}

// 具體類
// 具體類A
export class ConcreteClassA extends AbstractClass {
  // 覆蓋 protected2
  override operation2(): void {
    console.log('對該方法 operation2 進行了修改再使用');
  }

  // 覆蓋 protected3
  override operation3(): void {
    console.log('對該方法 operation3 進行了修改再使用');
  }
}

// 具體類B
export class ConcreteClassB extends AbstractClass {
  // 覆蓋勾子方法
  override hookMethod(): boolean {
    return false;
  }
}


export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const classA: AbstractClass = new ConcreteClassA();
    const classB: AbstractClass = new ConcreteClassB();

    console.log('==  classA.template ==');
    classA.template();
    console.log('==  classB.template ==');
    classB.template();
    
    // == classA.template ==
 		// 使用了方法 operation1
		// 對該方法 operation2 進行了修改再使用
		// 對該方法 operation3 進行了修改再使用
    
		// == classB.template ==
		// 使用了方法 operation1
		// 使用了方法 operation3
  }
}
```

```typescript
// 例2：
// 抽象類
export abstract class GameAbstract {
  abstract initialize(): void;
  abstract startPlay(): void;
  abstract endPlay(): void;

  // 模版
  play(): void {
    // 初始化游戲
    this.initialize();
    // 開始游戲
    this.startPlay();
    // 結束游戲
    this.endPlay();
  }
}

// 具體類
export class Basketball extends GameAbstract {
  override initialize(): void {
    console.log('Basketball Game Initialized! Start Playing.');
  }

  override startPlay(): void {
    console.log('Basketball Game Started. Enjoy the game.');
  }

  override endPlay(): void {
    console.log('Basketball Game Finished!');
  }
}

export class Soccer extends GameAbstract {
  override initialize(): void {
    console.log('Soccer Game Initialized! Start Playing.');
  }

  override startPlay(): void {
    console.log('Soccer Game Started. Enjoy the game.');
  }

  override endPlay(): void {
    console.log('Soccer Game Finished!');
  }
}

// component.ts
export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const basketball: GameAbstract = new Basketball();
    const soccer: GameAbstract = new Soccer();

    basketball.play();
    soccer.play();
  }
}
```

```typescript
// 例3： beforStageHooks
// 抽象類
export abstract class SystemAbstract {
  // 勾子函數列表，私有化，防止被賦值為空數組
  private beforeStage2Hooks: Array<() => void> = [];
  private afterStage2Hooks: Array<() => void> = [];
  
  beforeStage2(hook: () => void): void {
    this.beforeStage2Hooks.push(hook);
  }
  
  afterStage2(hook: () => void): void {
    this.afterStage2Hooks.push(hook);
  }
  
  private stage1(): void {
    console.log('stage 1');
  }
  
  private stage2(): void {
    console.log('stage 2');
  }
  
  // 階段 3 的具體實現是未知的
  abstract stage3(): void;
  
  private stage4(): void;
  
  // 模板
  start(): void {
    this.stage1();
    this.beforeStage2Hooks.forEach((hook: () => void) => hook());
    this.stage2();
    this.afterStage2Hook2.forEach((hook: () => void) => hook());
    this.stage3();
    this.stage4();
  }
}

// 具體類
export class SubSystem extends SystemAbstract {
  constructor() {
    super();
    this.beforeStage2(() => {
      console.log('before stage2');
    });
  }
  
  stage3(): void {
    console.log('subSystem stage3');
  }
}

export class AnotherSystem extends SystemAbstract {
  stage3(): void {
    console.log('anotherSystem stage3');
  }
}

// component.ts
const subSystem: SystemAbstract = new SubSystem();
const anotherSystem: SystemAbstract = new AnotherSystem();

anotherSystem.afterStage2(() => {
  console.log('another after stage2');
})

console.log('== subSystem.start ==');
subSystem.start();

console.log('== anotherSystem.start ==');
anotherSystem.start();

// == subSystem.start ==
// stage 1
// before stage2
// stage 2
// SubSystem stage3
// stage 4

// == antherSystem.start ==
// stage 1
// stage 2
// antherSystem after stage2
// after stage2-1
// AnthorSystem stage3
// stage 4
```



### 弊端

模版模式在類別中，本質上是固定不可變的結構，進逼步縮小重寫方法的範圍，重寫的範圍越小，程式碼可重覆使用度就越高，所以一定要在具有通用演算法可提取的情況下使用，不要為了節省程式碼行數而過度使用。

另外前端開發中，HTML 本身就很契合模版樣式，因為 HTML 中有大量標籤描述千變萬化的 UI 結構，可復用的地方實在太多太多，所以非常適合模板模式，所以不要認為模版模式僅能在類別中使用，模版模式還能在鷹架中使用呢，例如填入一些表單自動產生程式碼。

學習這個設計模式時，請注意不要固化思維在其定義的類這個框子中，因為設計模式寫於 1994 年，其中提到的模式已經被大量遷移使用，能否識別並做適當的知識遷移。是 20 多年後的今天學習設計模式的關鍵。



### 總結

模版模式與策略模式有一定相似處，模版模式是改變演算法的一部份，而策略模式是將策略完全提取出來，所以可以改變演算法的全部。

