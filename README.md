Flowchain App 入門極簡教材

# 1. Flowchain App 簡介

如果要「簡單介紹」這是什麼技術，還真的不簡單。所以，不如根據這份中文教材，體驗 Flowchain App 在物聯網開發上的創新。

## Flowchain App 起源

Flowchain App 發源自 [WoT.City](https://wotcity.com) 計畫，WoT 是 Web of Things 的縮寫。要把 Flowchain App 說清楚，就必須把 Web of Things 講明白。你可以在[這裡](https://www.w3.org/WoT/IG/wiki/Implementations)找到一份 Web of Things Implementations 的清單，這份清單整理 W3C Web of Things 的相關計畫。

也就是說，WoT.City 是 W3C Web of Things (WoT) 的一個計畫，它也是一個開源項目，也是 WoT 的技術。這就是故事的開始。

WoT.City 主要在處理 *Connectivity*，例如：WebSocket、CoAP 與 Thing Description 等。WoT.City 是 Connecitivity Layer，目前它已經打包成  [wotcity.io](https://www.npmjs.com/package/wotcity.io) 模組，但是你不需要直接使用 wotcity.io 模組。

wotcity.io 被封裝為 [devify-server](https://github.com/DevifyPlatform/devify-server) 模組，devify-server 是一份 IoT server boilerplate，這是什麼呢？簡單說，就是一套給 IoT 用的伺服器程式模板，你可以用 devify-server 很容易寫出一個「負責收集感測器資料」的 IoT server。這套 IoT server 可以執行在 Desktop、Server 或 IoT device 上，它使用的是 Nodoe.js 技術，這表示只要有 Node.js 執行環境的硬體，都可以成為 devify-server 的執行環境。

架構上，devify-server 封裝了 wotcity.io，並且負責 Event Handling、Broker Server 與 Serverless 的角色。不過，你也不需要學習 devify-server 的程式設計。devify-server 再被封裝了，成為 *Flowchain* 技術。

Flowchain App 帶來一些好消息，其中之一就是採用 Flow-Based Programming 的物聯網開發模式。

## Flowchain 設計思路

Flowchain App 的名稱來自 _Flow-Based Programming_，故名思義，這是一個 Flow-Based Programming (FBP) 的技術。Flow-Based Programming 並不是新鮮事，非常知名的 [Flux](https://facebook.github.io/flux/docs/overview.html) 就是一個 Flow-Based Programming 的模式。

實際上，Flowchain App 的設計模式與 Flux 模式是相同的。這讓 Flowchain App 與 React 也能良好地結合。在未來的進階教材中，你將能學會 Flowchain App 如何與 React 結合，應用在物聯網的 Fullstack 開發中。

Flowchain App 採用 Flow-Based Programming 的觀念，這一點都不難，請見這個例子：

```
{
    "author": "jollen",
    "type": "coapBroker",
    "connections": [
        {
            "upproc": "io.flowchain.console",
            "upport": "out",
            "downproc": "io.flowchain.fs",
            "downport": "in"
        }
    ]
}
```

這就是一個 Flowchain 的應用程式。

Flowchain App 大幅簡化了典型的 Flow-Based Programming 模式，這讓 Flowchain App 更適用於 IoT 場景，而且執行 Flowchain App 應用程式的 Runtime 也只有大約 300+ 行左右的程式碼實作。

Flowchain App 架構設計包含幾個特色：

* 單一資料方向流（single data direction）：與 Flux 相同的是，Flowchain App 的設計是單一資料方向流，它不採用雙向資料流（two-way）的設計
* Component-based 設計：組件（component）透過 inPort 與 outPort 來輸入與輸出資料，組件可以直接重用或程式碼重用（Code Reuse），而且組件之間沒有耦合性
* Decoupled Architecture：因為 Flowchain App 的 component 間是完全沒有耦合的，所以 Flowchain App 的應用程式，是高度 decoupled 架構，這點與 Flux 相同

## Flowchain App 程式設計

將上述例子，以視覺化圖形（Graph）的方式表示。

![Flowchain Graph](https://cloud.githubusercontent.com/assets/1126021/17215664/409fd6ec-5510-11e6-80fb-371b6c3a724e.png)

這裡有二個 Component：

* *io.flowchain.console*：這個元件會把輸入的資料列印在 console
* *io.flowchain.fs*：這個元件會把輸入的資料儲存為文字檔

你只需要實作以上這二個元件，然後把它們「連接」起來，就能達到：將資料印在 console 後，再儲存為文字檔。上述的 Flowchain App，就是在描述這個「連接關係」。

以這個例為例，資料的「流向」是從 *io.flowchain.console* 到 *io.flowchain.fs*。如果你想讓資料的流向是：從 *io.flowchain.fs* 到 *io.flowchain.console*，那要如何撰寫 Flowchain app 呢？答案如下：

```
{
    "author": "jollen",
    "type": "coapBroker",
    "connections": [
        {
            "upproc": "io.flowchain.fs",
            "upport": "out",
            "downproc": "io.flowchain.console",
            "downport": "in"
        }
    ]
}
```

是不是很簡單呢。元件可以重用與共享，意思是，*io.flowchain.console* 與 *io.flowchain.fs* 二個元件，你都能直接拿去使用。這是 Flowchain pattern 的設計思想。

# 2. Flowchain App Pattern

如果你已經熟悉 Flux 模式，要了解 Flowchain App 的設計思路，對你來說已經是輕而易舉的事情了。

![Flowchain](https://cloud.githubusercontent.com/assets/1126021/17242135/190517ee-55a8-11e6-8207-a936a29fb8f6.png)

Flowchain App 的目標，並不是教你理解並撰寫 Flowchain App，而是將這個模式「封裝」為 Software Framework 後，讓你很容易撰寫 Flowchain App。使用 Flowchain App，你唯一要學習的是如何撰寫 *Flowchain Applications*.

上圖是「基本款」的 Flowchain App Pattern。眼尖的你應該已經發現，這就是 Flux Pattern。是的，基本的 Flowchain App Pattern 與 Flux Pattern 是相同的。二者之間有諸多相通的設計，例如：single dispatcher、single data flow 等。

在 Flowchain App 模式裡：

* *Actions* 主要負責將數據傳送給 Dispatcher，actions（例如：數據）的來源是 Websocket Server 或是 CoAP Server
* *Dispatcher* 主要負責將數據分發給 Stores
* *Stores* 就是 Flowchain 的 Component。另外，Flowchain 的底層，有一個稱為 Devify 的 Store，它負責 emit 事件。但是你不需要了解 Devify 這個部份，只要學習如何撰寫 Component 與 Graph 就可以了

Dispatcher 還有一個特別的角色，它也做為「Broker」，這部份現階段你也不必了解。這有什麼用途呢？一個例子是：讓你完全不用寫程式，就可連到 Dashboard（Web Frontend）。

另外，附帶一提：Actions 也扮演 URL Routing 的角色。

# 3. Flowchain App 快速入門

下載 [flowchain-app](https://github.com/flowchain/flowchain-app) 後，在 *graphs/* 目錄下可以找到幾個 _Graph_ 的範例，其中一個 Graph 稱為 *console.json*，請直接執行這個 Graph：

```
$ git clone https://github.com/flowchain/flowchain.git
$ npm install
$ ./bin/init.js start graphs/console.json 
[devify] Starting coapBroker server…
WoT/CoAP server is listening at coap://localhost:8000
```

現在，你已經啟動第一個 Flowchain App 了。Flowchain App 的檔案名稱是 _*.json_。Flowchain 應用程式是以 JSON 格式撰寫，這份 JSON 文件就是在描述「Component 連接」關係（請見上文說明），也就是說，這份 JSON 文件就是在描述 Graph（圖形）。

是的，用 JSON 來描述 Graph，就是 Flowchain App 的 Programming Model。

現在，你只需要將 Sensor Data 傳送到 *coap://localhost:8000* 伺服器即可。從這個示範來看，Flowchain App 是以 Server 形式存在，並且是 REST 架構。

更進一步說，所以你只要透過以下的 URI 來傳送資料給 Flowchain App 即可：

```
coap://localhost:8000/object/12345678/send
```

你可以使用這個 Test Case 來測試：

```
var coap = require(‘coap’);

var sendNumber = function() {
    // 1 to 100
    var number = Math.round(Math.random() * 100 + 1);
    var obj = {temperature: number, temp: number};
    var data = JSON.stringify(obj);

    console.log(‘Pushing: ‘ + data);

    var clientWriable = coap.request(‘coap://localhost:8000/object/55548dd35200c3917f000159/send');
    clientWriable.end(new Buffer(data));

    setTimeout(sendNumber, 1000);
};

sendNumber();
```

請將這個測試式儲存為 *test.js*，然後安裝 coap 模組後直接執行即可：

```
$ npm i coap
$ node test.js
```

## IoT Protocol

Flowchain App 執行後，成為一個 *Server*，IoT 裝置可以使用 URI 將資料傳送給 Flowchain App。

問題來了，那 Flowchain App 啟動後，要怎麼指定通訊協定呢？非常簡單，只要修改 ```type``` 欄位就可以了：

* ```coapBroker```：使用 CoAP 協定
* ```websocketBroker```：使用 WebSocket 協定

## Data Flow 的開始

整個 Graph 的資料從何而來呢？Flowchain App 的第 1 個元件稱為 *Root Component*，資料會從 Root Component 流入。上述範例的 *io.devify.fs* 就是 Root Component。意思是說：*io.devify.fs* 的 inPort 是資料流入的起點。

根據上述範例的 connections 設計，這筆資料會從 *io.devify.fs* 的 outPort 流出，並流入 *io.devify.console* 的 *inPort*。

# 4. Flowchain Component

上述 Flowchain App 範例，包含 2 個 Flowchain App Component，其中一個稱為 *io.devify.console*。練習撰寫 Flowchain App Component 是學習 Flowchain 的重要課程。底下是 *io.devify.console* 的實作解說。

```
//
// 1. 引用 wotcity.io 模組
//     * Flowchain Component 與 Flowchain Runtime 都打包在 wotcity.io 裡面
var wotcity = require('wotcity.io');

//
// 2. 匯出 getComponent() 函數
//     * 每個 Flowchain Component 都要有這個 public method 的實作
//
exports.getComponent = function() {
//
// 3. 取得 wotcity.Component 的實例化
//     * 意思等同於建立新的 Flowchain 元件
//
  var component = new wotcity.Component;

//
// 4. 給你的元件一個名稱
//     * 命令規範與 Java package 相同
//
  component.name = "io.devify.console";
  
//
// 5. 元件的用途說明
//
  component.description = "This component prints the received data on the console.";

//
// 6. Flowchain 元件的 inPort。元件的 inPort 如果有資料流入，callback function 就會被呼叫。
//
  component.inPorts.add('in', function(event, payload) {
    switch (event) {
//
// 7. 你只需要實作 'data' 事件就可以了
//    
      case 'data':
//
// 8. 由 payload 取得流入的資料。資料格式都是用 JSON。
//   
        console.log(JSON.stringify(payload));
//
// 9. 成功請回傳 '0'
//          
        return 0;
      case 'disconnect':
        // Input port disconnects
        return 0;
    }
  });

//
// 10. 宣告元件的 outPort。這裡不需要修改。
//  
  component.outPorts.add('out');

//
// 收工。
//  
  return component;
};
```

## 出版 Component

如果你開發了一個 Flowchain App Component，可以利用 npm 將這個元件出版。以下是 Flowchain App Component 的交付流程。

1. 參考 [io.flowchain.console](https://github.com/flowchain/io.flowchain.console) 的專案結構，將你的元件打成標準的 npm 包
2. 使用 `npm publish` 出版到 npm 社群
3. 在 [github.com/flowchain/flowchain.github.io](https://github.com/flowchain/flowchain.github.io/issues) 開 Issue，提交你的 github 網址
4. 你的元件會被收錄到 [flowchain.io](http://flowchain.io) 上

Flowchain App 元件套件的命名，採用與 Java 套件相同的規範。例如，你的專案網域名為 ```flowchain.io```、元件名為 ```sms``` 時，請使用倒轉的網域名做為 naming space，完整的套件應命名為 ```io.flowchain.sms```。



# 結語

Flowchain App 的設計思想，深受 Flux 模式與 Flow-Based Programming 觀念的影響，整體架構也完全採取 Decoupled 方式設計。這讓 *Flowchain App* 能與這些技術整合：

* 實作前端或移動應用，使用 ```Flowchain + React```
* 實作 IoT 雲平台，使用 ```Flowchain + Microservice```

最後，還有一個重要的事情：整個 Flowchain App 從上到下（當然）都是 JavaScript :-)
