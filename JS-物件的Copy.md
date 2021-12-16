# JS的深拷貝(Shallow copy)與淺拷貝(Deep copy)
* <b>物件</b>跟<b>基本型別</b>最大的不同就在於他們的傳值方式。  
  ### 基本型別： Ex. int、String、boolean
  ```javascript
  let a = 1;
  let b = a;
  b = 5;
  console.log(a);  //1
  console.log(b);  //5
  ```
  ### 物件：  
  淺拷貝
  ```javascript
  let ori_obj = { a: 1, b: 3, c: 5};
  let copy_obj = ori_obj;
  copy_obj.c = 7;
  console.log(ori_obj);  //{ a: 1, b: 3, c: 7 }
  console.log(copy_obj); //{ a: 1, b: 3, c: 7 }
  ```
  深拷貝
  ```javascript
  let ori_obj = { a: 1, b: 3, c: 5};
  let copy_obj = { a: ori_obj.a, b: ori_obj.b, c: ori_obj.c };
  copy_obj.c = 7;
  console.log(ori_obj);  //{ a: 1, b: 3, c: 5 }
  console.log(copy_obj); //{ a: 1, b: 3, c: 7 }
  ```
* 淺拷貝-複製物件的指標，指向同一塊記憶體，而不是複製物件本身。
* 深拷貝-複製物件本身，存於不同的記憶體上，修改新物件不會改到舊物件。

淺拷貝的方法如下：
1. Obeject.assign
   * 只能複製一層，第二層開始 改新物件也會改到舊物件
2. es6 解構

深拷貝的方法如下：
1. JSON.parse(JSON.stringify({ 被複製的物件 }))
   * funtion沒辦法複製
2. $.extend(true, {}, { 被複製的物件 })
   * jquery的用法
3. lodash.cloneDeep({ 被複製的物件 })
   * 函式庫lodash，require('lodash')
