# 行為型模式 Behavioral Pattern

> 封裝變化。

行為模式負責對象之間的溝通與職責劃分，其除了關注結構以外，更關注他門之間的溝通機制。透過行為型模式可以更清楚的劃分類別、物件之間的職責，展現實例物件之間的作用互動。



## Observer（觀察者模式）

### 意圖

> 定義物件間的一種一對多的依賴關係，當一個物件的狀態改變時，所有依賴它的物件都會被通知並被自動更新。

例1：npm 依賴

npm 套件與專案是一對多的關係（一個 npm 套件被多個專案使用），當 npm 套件發布ㄒ新版本時，如果所有依賴它的專案都能得到通知，並自動更新這個套件的版本號，那麼就解決了套件版本更新的問題，這就是觀察者模式要解決的基本問題。



例2：物件與視圖雙向綁定

觀察者模式在最初提出的時候，就舉了資料與 UI 相互綁定的例子。即同一份資料可以同時渲染為表格與長條圖，那麼當操作表格更新資料時，如何讓長條圖的資料也刷新呢？從這個場景引出了對觀察者模式的定義，即「數據」與「UI」是一對多的關係，我們需要一種設計模式實現當「數據」變化時，所有依賴於它的「UI」都得到通知並自動更新。



例3：拍賣

拍賣由一個拍賣員與多位拍賣者組成。排賣時，由 a 喊出競價（出100）就是觀察者向目標發出的 `setStatus`，同時，此時拍賣員喊出（有人出價100，還有更高的嗎？）就是一個 `notify` 通知行為，拍賣員通知了現場競價全員，刷新了他們對當前最高價的信息。



例4：聊天室

聊天室由一個中央伺服器與多個客戶端組成。客戶端發送訊息後，就是向中央伺服器發送了 `setStatus` 更新請求，此時中央伺服器通知所有處於同一聊天室的客戶端，更新他們的訊息 `notify`，從而完成一次訊息的發送。



### 意圖解釋

Observer Pattern 實作的原理就是把獲取資料的部份抽離出來，並在資料改變時，同步發送給所有的觀察者。且觀察者可以在任何時候決定是否要繼續接收資料。

當多個 class 都需要接收同一種資料的變化時，就適合使用 Observer Pattern。

- 多個 class => 觀察者 Observer。
- 同一種資料 => 主題 Subject。

![img](https://fullstackladder.dev/assets/blog/mastering-rxjs-06-observer-pattern/03.png)

- 觀察者模式基本上只有兩個角色：
  - 觀察者（Observer）：
    - `notify`：當目標資料變更，會呼叫觀察者的 nodify 方法，告知資料變更了。
  - 目標（Subject）：
    - `notifyObservers`：用來通知所有目前的觀察者資料變更了，也就是呼叫所有觀察者物件的 `notify` 方法。
    - `addObserver`：將某個物件加入觀察者清單。
    - `deleteObserver`：將某個物件從觀察者清單移除。



#### 範例

##### 設計觀察者的 interface

這裡先設計 interface 有幾個原因：

1. 因為觀察者不只一個，為了讓所有的觀察者都能夠被主題「加入訂閱」和「移除訂閱」，它們就得擁有同一個 interfce，當執行事件時，就只需要以 interface 做共同型別代替 class 傳入。
2. 觀察者需要被通知，用 interface 可以確保所有 class 都有接受通知的行為存在。



```typescript
// 例1：youtube - 加入訂閱、移除訂閱、通知訂閱的人
// interface
export interface IObserver {
  nodify(videoName: string): void;
}

export interface ISubject {
  addObserver: (obs: IObserver) => void;
  deleteObserver: (obs: IObserver) => void;
  notifyObserver: () => void;

  // 模擬有新影片發佈，不存在observer模式中
  publishVideo: () => void;
}


// class
export class Youtuber implements ISubject {
  private _observersSet: Set<IObserver> = new Set();
  private _name!: string;

  constructor(name: string) {
    this._name = name;
  }

  addObserver(obs: IObserver): void {
    this._observersSet.add(obs);
  }

  deleteObserver(obs: IObserver): void {
    this._observersSet.delete(obs);
  }

  notifyObserver(): void {
    this._observersSet.forEach((obs: IObserver) =>
      obs.notify(`${this._name} 有新影片啦！！`)
    );
  }

  // 模擬有新影片發佈，不存在observer模式中
  publishVideo(): void {
    this.notifyObserver();
  }
}

export class Observer implements IObserver {
  notify(content: string): void {
    console.log('Observer: ', content);
  }
}

// component.ts
// 建立 Youtuber 的實例
export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    // 建立 Youtuber 的實例
    const gao: ISubject = new Youtuber('老高');
    const ndnz: ISubject = new Youtuber('明星大偵探');

    // 建立 Observer 的實例
    const observer: IObserver = new Observer();

    console.log('== 訂閱老高 ＆ 大偵探 ==');
    gao.addObserver(observer);
    ndnz.addObserver(observer);

    console.log('== 當老高和大偵探發佈新影片 ==');
    gao.publishVideo();
    ndnz.publishVideo();

    console.log('== 取消訂閱老高 ==');
    gao.deleteObserver(observer);

    console.log('== 取消訂閱老高後，老高發佈新影片，不會收到通知 ==');
    gao.publishVideo();

    console.log('== 當大偵探發佈新影片 ==');
    ndnz.publishVideo();

    console.log('== 再次訂閱老高 ==');
    gao.addObserver(observer);

    console.log('== 當老高發佈新影片 ==');
    gao.publishVideo();
  }
}

/*
	結論：
		1. interface 真的很偉大。
		2. 將負責處理資料的部份抽到「Youtube」，當有新影片就主動通知所有訂閱的人，
			 而不是由訂閱人一直問說有沒有新影片。
    3. 耦合度很低，因為只要是實作了「IObserver」接口都可以接收新影片，而不是特
    	 定某個 class 或是 object 做型別參數，傳入了才發現沒有「notify」的行為。
    4.「Youtuber」不管誰收到通知後要幹嘛，因為它只負責通知，上方是以觀察者的角度
    	 接收通知，但其實不只觀影者，就算是官方負責統計資料的 class，只要它有實作
    	 「IObserver」接口，那就能和觀影者同步揪收到新影片的通知，各自處理接到資料
    	 後的邏輯。
*/
```

```typescript
// 例2：顧客點餐後，服務員記下顧客的信息，菜做好後廣播通知顧客領取
export class abstructor Observer {
  abstructor take(): void;
}

export class Subject {
  
  private set: Set<Observer> = new Set();
  
  constructor(){}
  
  add(obs: Observer): void {
    this.set.add(obs);
  }
  
  delete(obs: Observer): void {
    this.set.delete(obs)
  }
  
  notify(msg: string): void {
    this.set.forEach((obs: Observer) => obs.take(msg));
  }
  
}

export class Waiter extends Subject {
  constructor(){
    super();
  }
  
  ready(msg: string): void {
    super.notify(msg);
  }
}

export class Clienter implements Observer {
  
  private _name!: string = name;
  
  constructor(name: string, waiter: Waiter){
    this._name = name;
    waiter.add(this);
  }
  
 	take(msg: string): void {
    console.log(
      `顧客 ${this._name} 收到了消息顯示狀態是<${msg}>， 到吧台領取了菜`
    );
  }
}

export class AppComponent {
  constructor() {}

  ngOnInit(): void {
    const waiter: Waiter = new Waiter();
    const bob: Clienter = new Clienter('Bob', waiter);
    const mick: Clienter = new Clienter('Mick', waiter);

    waiter.ready();
  }
}
```



### 弊端

不要拘泥於實現形式，一對多的關係不一定要用這種代碼組織形式來實現觀察者效果。也可以利用 proxy 模式輕鬆實作。



### 總結

觀察者模式是非常常用的設計模式，它描述了物件一對多依賴關係下，如何通知並更新的機制，這種機制可以用在前端的 UI 與資料映射、後端的請求與控制器映射，平台間的訊息通知等大部份場景，無論實現或程式中，存在依賴且需要通知的場景非常普遍。

