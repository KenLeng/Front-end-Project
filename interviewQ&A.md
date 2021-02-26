## <a name='toc'>目錄</a>
  1. [callback、promise](#a)
  1. [== 與 === 的差異](#b)
  1. [Describe 'this' works in JavaScript](#c)
  1. [.call、.apply、.bind](#d)
  1. [var、let、const](#e)
  1. [捕獲與冒泡](#f)
  1. [Vue Lifecycle](#g)
  1. [null、undefined、undeclared](#h)
  1. [Cookie、Session、SessionStorage、LocalStorage](#i)
  1. [CSR、SSR、SPA](#j)
  1. [事件委派](#k)

## <a name='a'>callback、promise</a>
   * callback意指「回調」，用另一個函式作為函式中的傳入參數的樣式來撰寫程式，然後將本來應該要回傳的值(不限定只有一個)，傳給下一個延續函式，繼續下個函式的執行
   ```javascript
   doA(function() {
    doB();

    doC(function() {
      doD();
    });

    doE();
  });
  doF();
  
  // doA() -> doF() -> doB() -> doC() -> doE() -> doD()
   ```
## <a name='b'>== 與 === 的差異</a>
   * == 為一般相等→會先將比較值轉換成同一型別，再使用嚴格相等。
   * === 為嚴格相等→型別相等且值相等，效率比使用一般相等好。  
   <b>Notes</b>：  
   ```txt
   false == "0"       // true 這兩個人的值是一樣的！！  
   0 == ""            // true  
   null == undefined  // true  
   null === undefined // false
   ```
## <a name='c'>Describe 'this' works in JavaScript</a>
   * 影響<b> this </b>是函式的呼叫，並非宣告的時機。箭頭函式並沒有 this
   ```javascript
   function callName() {
      console.log(this.name);
   }
   let name = 'Kobe bryant';
   let player = {
      name: 'Allen iverson',
      callName: callName  // 這裡的 function 指向全域的 function，但不重要
   }
   let callThisPlayer = player.callName;
   
   callName();        // 'Kobe bryant'
   player.callName(); // 'Allen iverson'，呼叫是在物件下調用，那麼 this 則是該物件
   callThisPlayer();  // 'Kobe bryant'
   ```
   * <b> this </b>的重新指向  
   立即函式 (IIFE) 或是非同步的事件 (setTimeout) 大多都會指向全域，如果需調用物件本身的話，可以先用一個變數(vm, self)指向 this。
   ```javascript
   function callName() {
     console.log('區域 1:'+ this.name);
     var vm = this;
     setTimeout(function () {
       console.log('全域:'+ this.name);
       console.log('區域:'+ vm.name);
     }, 3000);
     console.log('區域 2'+ vm.name);
   }

   var name = 'Kobe bryant';
   var auntie = {
     name: 'Allen iverson',
     callName: callName 
   }

   auntie.callName();   //區域 1:Allen iverson
                        //區域 2:Allen iverson
                        //全域:Kobe bryant
                        //區域:Allen iverson
   ```
## <a name='d'>`.call`、`.apply`、`.bind`</a>
   * .call 等同於 `()`，使用方法`fn.call(this, arg1, arg2..., argn)`，第二個以後的參數會作為參數傳進目標函式中。
   * .apply 與.call功能一樣，使用方法`fn.apply(this, [arg1, arg2..., argn])`，只會傳入兩個參數，第二個參數必須為<b>陣列</b>。
   * .bind 可以把先前傳入 bind 的參數一並帶進目標函式中，使用方法`fn.bind(this, arg1, arg2..., argn)`，第二個以後的參數會作為往後傳進目標函式的參數。
## <a name='e'>`var`、`let`、`const`</a>
   * var變數範圍在function內；而let是在block<b>(block意指｛｝)</b>，除了 function 以外 if、for 的 {} 都屬於 let 的作用範圍。
   ```javascript
   function varMing () {
     var ming = '小明';
     if (true) {
       var ming = '憲哥';  // 這裡的 ming 依然是外層的小明，所以小明即將被取代
       console.log(ming);  // '憲哥'
     }
     console.log(ming);    // '憲哥'
   }

   function letMing () {
     let ming = '小明';
     if (true) {
       let ming = '憲哥';  // 這裡的 ming 是不同的，只有在這個 if block 才有作用
       console.log(ming);  // '憲哥'
     }
     console.log(ming);    // '小明'
   }
   varMing();
   letMing();
   ```
## <a name='f'>[DOM事件](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)：捕獲(event capturing)與冒泡(event bubbling)</a>
   ### 原則-  
   1.先捕獲，在冒泡。  
   2.當事件傳到 target 本身，沒有分捕獲跟冒泡。  
   ### 概念-  
   DOM樹在傳遞時，會先從根節點(window)往下傳到target，這邊加上事件的話，就會處於`CAPTURING_PHASE(捕獲階段)`。  
   target就是你所點擊的那個`目標`。  
   最後，事件再往上從子節點一路逆向傳回去根節點，這時候就叫做`BUBBLING_PHASE(冒泡階段)`。  
   事件傳遞圖→
   <img src="https://www.w3.org/TR/DOM-Level-3-Events/images/eventflow.svg" width="400" height="400"></img>
   * `stopPropagation()`與`preventDefault()`  
   ```txt
   stopPropagation - 中斷事件。  
     加在哪邊，事件的傳遞就斷在哪裡，不會繼續往下傳遞，但若是你在同一個節點上不只有一個 listener，還是會被執行到。  
     因為事件傳遞被停止，所以剩下的 listener 都不會再收到任何事件。
   preventDefault - 取消瀏覽器的預設行為。  
     preventDefault 跟 JavaScript 的事件傳遞「一點關係都沒有」，加上這一行之後，preventDefault事件還是會繼續往下傳遞。
   ```
## <a name='g'>Vue Lifecycle</a>
   <img src="https://miro.medium.com/max/1200/1*byyX8EW6mIhRsCBWwByNYg.png" width="350" height="800"></img>
   
   `beforeCreated` 什麼事都還沒做，只跟大家說我要開始囉。  
   `created` 所有屬性都已經綁定囉，但是$el還沒建立，DOM也還沒生成。在此階段，包含data observation，computed屬性，methods，watch/event callbacks等項目都已設定完成。  
   `beforeMounted` 要開始產virtual DOM。  
   `mounted` el被新建的vm.$el替換，並掛到instance後調用的hook。  
   `beforeUpdate` data收到更新異動，在更新前調用它。  
   `updated` DOM已經完成更新，盡量在此階段不要更動狀態，如果有要做狀態相對應的改變，應該用computed和watch取代。  
   `beforeDestroy` Vue instance被銷毀前調用，這時候大家功能都還能使用。  
   `destroy` Vue instance 所有的東西都會解除綁定，event listener也會被移除。最好使用v-if & v-for以data控制component的生命週期
## <a name='h'>`null`、`undefined`、`undeclared`</a>
  * `undeclared` variables don’t even exist - 未宣告
  * `undefined` variables exist, but don’t have anything assigned to them - 有宣告但未給值
  * `null` variables exist and have null assigned to them - 有宣告卻給null
## <a name='i'>`Cookie`、`Session`、`SessionStorage`、`LocalStorage`</a>
  |特性|Cookie|Session|SessionStorage|LocalStorage|
  |----|------|-------|--------------|-----------|
  |數據有效期|一般由服務器(server)生成，可設置失效時間。如果在瀏覽器生成，默認是關閉瀏覽器後失效|僅在當前瀏覽器(含新視窗、分頁)關閉前有效|僅在當前瀏覽器窗口關閉前有效|除非被清除，否則永久保存|
  |存放數據大小|4K左右|與cookie共用|5MB|5MB|
  |與server通信|存放於client端，在所有同源瀏覽器窗口中都是共享的，每次都會攜帶在HTTP表頭中|本身就存放在server端|僅在瀏覽器(client)中保存，不參與服務器通信|僅在瀏覽器(client)中保存，不參與服務器通信|
## <a name='j'>[CSR、SSR、SPA](https://blog.techbridge.cc/2017/09/16/frontend-backend-mvc/)</a>
   CSR(client side render) - 畫面在前端渲染，瀏覽器傳送頁面請求，server返回的是一個模板頁面  
   SSR(server side render) - 畫面在後端渲染，渲染完後即回傳給瀏覽器  
   SPA(single page application) - 頁面不轉換，由前端內部透過router和ajax去後端撈資料，client在重新渲染  
  * CSR與SSR的差異就在於，有了資料後，畫面在哪邊渲染。
## <a name='k'>事件委派</a>
  * 是一種受惠於 Event Bubbling 而能減少監聽器數目的方法。
  * 優點 - 可減少監聽器的數目。
  * 缺點 - 由於需要判斷哪些 child node 是我們有興趣的項目，而必須多寫一些程式碼做判斷。
  ```javascript
  <div class="parent" onclick="clickEvent">
    <div class="child" data-name="a"></div>
    <div class="child" data-name="b"></div>
    <div class="child" data-name="c"></div>
    <div class="subitem" data-name="d"></div>
  </div>
  ```
