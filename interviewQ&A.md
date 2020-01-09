## == 與 === 的差異
   * == 為一般相等→會先將比較值轉換成同一型別，再使用嚴格相等。
   * === 為嚴格相等→型別相等且值相等，效率比使用一般相等好。  
   <b>Notes</b>：  
   ```txt
   false == "0"       // true 這兩個人的值是一樣的！！  
   0 == ""            // true  
   null == undefined  // true  
   null === undefined // false
   ```
## Describe 'this' works in JavaScript
   * 影響<b> this </b>是函式的呼叫，並非宣告的時機
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
## `.call`、`.apply`、`.bind`
   * .call 等同於 `()`，使用方法`fn.call(this, arg1, arg2..., argn)`，第二個以後的參數會作為參數傳進目標函式中。
   * .apply 與.call功能一樣，使用方法`fn.apply(this, [arg1, arg2..., argn])`，只會傳入兩個參數，第二個參數必須為<b>陣列</b>。
   * .bind 可以把先前傳入 bind 的參數一並帶進目標函式中，使用方法`fn.bind(this, arg1, arg2..., argn)`，第二個以後的參數會作為往後傳進目標函式的參數。
## `var`、`let`、`const`
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
