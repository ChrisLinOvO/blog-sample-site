---
title: "Lodash Introduction"
date: 2023-11-08T15:00:00+08:00
draft: false

# post thumb
image: "images/post/lodash-logo.jpg"

# meta description
description: "紀錄一些 lodash 筆記"

# taxonomies
categories:
  - "Notes"
tags:
  - "Lodash"

# post type
type: "post"
---

### 前言

---

{{< target-blank url="https://lodash.com/" >}}Lodash{{< /target-blank >}} 是一個 JavaScript library，提供了很多常用的函式，處理資料可以省去很多時間，有時候也會比原生 JS 的效能還要好。

### 安裝

---

透過 npm

```
npm i --save lodash
```

### 使用

---

```
import _ from 'lodash'
```

### 筆記紀錄

---

介紹幾個比較常用方法 {{< target-blank url="https://lodash.com/docs/4.17.15" >}}參考 Document{{< /target-blank >}}

#### 陣列

##### 1. \_.chunk(array, [size=1])

參數

- array (Array) : 需要處理的陣列
- [size=1] (number) : 每個數組區塊的長度

返回

- (Array) : 傳回分割區塊的新陣列

範例

```
_.chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]
```

##### 2. \_.compact(array)

參數

- array (Array) : 需要處理的陣列

返回

- (Array) : 傳回過濾掉「假值」的新陣列(`false`、`null`、`0`、`""`、`undefined`和`NaN`都是被認為是「假值」。)

範例

```
_.compact([0, 1, false, 2, '', 3]);
// => [1, 2, 3]
```

##### 3. \_.concat(array, [values])

參數

- array (Array) : 被連接的陣列
- [values] (...\*) : 連接的值

返回

- (Array) : 回傳數組連接後新陣列

範例

```
// 合併多個數組

const array1 = [1, 2];
const array2 = [3, 4];
const array3 = [5, 6];

const result = _.concat(array1, array2, array3);

console.log(result); // => [1, 2, 3, 4, 5, 6]
```

```
// 合併數組和其他值

const array = [1, 2];
const other = _.concat(array, 3, [4], 5);

console.log(other); // => [1, 2, 3, 4, 5]
```

```
// 合併多維數組

const array1 = [1, [2, 3]];
const array2 = [4, [5, 6]];

const result = _.concat(array1, array2);

console.log(result); // => [1, [2, 3], 4, [5, 6]]
```

```
// 合併空數組

const array = [1];
const emptyArray = [];
const result = _.concat(array, emptyArray);

console.log(result); // => [1]
```

##### 4. \_.difference(array, [values])

參數

- array (Array) : 要檢查的陣列
- [values] (...Array) : 排除的值

返回

- (Array) : 傳回一個計算數組之間差異後的新陣列

範例

```
_.difference([3, 2, 1], [4, 2]);
// => [3, 1]
```

##### 5. _.differenceBy(array, [values], [iteratee=_.identity])

參數

- array (Array) : 要檢查的陣列
- [values] (...Array) : 排除的值
- [iteratee=_.identity] (Array|Function|Object|string) : iteratee 呼叫每個元素

返回

- (Array) : 傳回一個計算數組之間差異後的新陣列

範例

```
const array1 = [{ 'x': 1 }, { 'x': 2 }, { 'x': 3 }];
const array2 = [{ 'x': 3 }, { 'x': 4 }, { 'x': 5 }];

// 使用 'x' 屬性來屬性來確定對象唯一性
const difference = _.differenceBy(array1, array2, 'x');

console.log(difference); // => [{ 'x': 1 }, { 'x': 2 }]
```

##### 6. _.findIndex(array, [predicate=_.identity], [fromIndex=0])

參數

- array (Array) : 要搜尋的陣列
- [predicate=_.identity] (Array|Function|Object|string) : 這個函數會在每一次迭代呼叫
- [fromIndex=0] (number) : The index to search from

返回

- (number) : 傳回找到元素的索引值（index），否則傳回-1

範例

```
// 找大於30歲索引
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 },
  { name: 'David', age: 40 }
];

const index = _.findIndex(users, (user) => user.age > 30);

console.log(index); // => 2
```

```
// 從索引1找大於30歲索引
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 },
  { name: 'David', age: 40 }
];

const index = _.findIndex(users, (user) => user.age > 30, 1);

console.log(index); // => 2
```

##### 7. _.findLastIndex(array, [predicate=_.identity], [fromIndex=array.length-1])

參數

- array (Array) : 要搜尋的陣列
- [predicate=_.identity] (Array|Function|Object|string) : 這個函數會在每一次迭代呼叫
- [fromIndex=array.length-1] (number) : The index to search from

返回

- (number) : 傳回找到元素的索引值（index），否則傳回-1

範例

```
// 找等於30歲最後一筆索引
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 },
  { name: 'David', age: 30 }
];

const index = _.findLastIndex(users, (user) => user.age === 30);

console.log(index); // => 3
```

```
// 從索引1找等於30歲最後一筆索引
const users = [
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
  { name: 'Charlie', age: 35 },
  { name: 'David', age: 30 }
];

const index = _.findLastIndex(users, (user) => user.age === 30, 1);

console.log(index); // => 3
```

##### 8. \_.head(array)

參數

- array (Array) : 要查詢的陣列

返回

- (\*) : 傳回陣列 array 的第一個元素

範例

```
_.head([1, 2, 3]);
// => 1

_.head([]);
// => undefined
```

##### 9. \_.flatten(array)

參數

- array (Array) : 需要減少巢狀層級的陣列

返回

- (Array) : 傳回減少巢狀層級後的新陣列

範例

```
const nestedArray = [1, [2, [3, [4]], 5]];

const flatArray = _.flatten(nestedArray);

console.log(flatArray); // => [1, 2, [3, [4]], 5]
```

##### 10. \_.flattenDeep(array)

參數

- array (Array) : 需要處理的陣列

返回

- (Array) : 傳回一個的新一維數組

範例

```
const deeplyNestedArray = [1, [2, [3, [4]], 5]];

const fullyFlatArray = _.flattenDeep(deeplyNestedArray);

console.log(fullyFlatArray); // => [1, 2, 3, 4, 5]
```

##### 11. \_.intersection([arrays])

參數

- [arrays] (...Array) : 待檢查的陣列

返回

- (Array) : 傳回一個包含所有傳入陣列交集元素的新陣列

範例

```
_.intersection([2, 1], [4, 2], [1, 2]);
// => [2]
```

##### 12. \_.join(array, [separator=','])

參數

- array (Array) : 要轉換的陣列
- [separator=','] (string) : 分隔元素

返回

- (string) : 傳回連接字串

範例

```
_.join(['a', 'b', 'c'], '~');
// => 'a~b~c'
```

##### 13. \_.last(array)

參數

- array (Array) : 要檢索的陣列

返回

- (\*) : 返回 array 中的最後一個元素

範例

```
_.last([1, 2, 3]);
// => 3
```

##### 14. \_.pull(array, [values])

參數

- array (Array) : 要修改的陣列
- [values] (...\*) : 要刪除的值

返回

- (Array) : 返回 array

範例

```
const array = [1, 2, 3, 1, 2, 3];

_.pull(array, 2, 3);
console.log(array);
// => [1, 1]
```

##### 15. _.pullAllBy(array, values, [iteratee=_.identity])

參數

- array (Array) : 要修改的陣列
- values (Array) : 要移除值的陣列
- [iteratee=_.identity] (Array|Function|Object|string) : iteratee（迭代器）呼叫每個元素

返回

- (Array) : 返回 array

範例

```
const array = [{ 'x': 1 }, { 'x': 2 }, { 'x': 3 }, { 'x': 1 }];

_.pullAllBy(array, [{ 'x': 1 }, { 'x': 3 }], 'x');
console.log(array);
// => [{ 'x': 2 }]
```

##### 16. _.remove(array, [predicate=_.identity])

參數

- array (Array) : 要修改的陣列
- [predicate=_.identity] (Array|Function|Object|string) : 每次迭代呼叫的函數

返回

- (Array) : 傳回移除元素組成的新陣列

範例

```
const array = [1, 2, 3, 4];
const evens = _.remove(array, function(n) {
  return n % 2 == 0;
});

console.log(array);
// => [1, 3]

console.log(evens);
// => [2, 4]
```

##### 17. \_.slice(array, [start=0], [end=array.length])

參數

- array (Array) : 要裁剪數組
- [start=0] (number) : 開始位置
- [end=array.length] (number) : 結束位置

返回

- (Array) : 傳回陣列 array 裁剪部分的新陣列

範例

```
const array = [1, 2, 3, 4, 5];

// 從索引 1 到索引 3 之間 （不包括索引 3）
const slicedArray = _.slice(array, 1, 3);

console.log(slicedArray); // => [2, 3]
```

```
const array = [1, 2, 3, 4, 5];

// 從索引 2 到最後
const slicedArray = _.slice(array, 2);

console.log(slicedArray); // => [3, 4, 5]
```

##### 18. \_.take(array, [n=1])

參數

- array (Array) : 要檢索的陣列
- [n=1] (number) : 要提取的元素個數

返回

- (Array) : 傳回 array 陣列的切片（從起始元素開始 n 個元素）

範例

```
_.take([1, 2, 3]);
// => [1]

_.take([1, 2, 3], 2);
// => [1, 2]

_.take([1, 2, 3], 5);
// => [1, 2, 3]

_.take([1, 2, 3], 0);
// => []
```

##### 19. \_.uniq(array)

參數

- array (Array) : 要檢查的陣列

返回

- (Array) : 傳回新的去重後的陣列

範例

```
const array = [1, 2, 2, 3, 4, 4, 5];

const uniqueArray = _.uniq(array);

console.log(uniqueArray); // => [1, 2, 3, 4, 5]
```

##### 20. _.uniqBy(array, [iteratee=_.identity])

參數

- array (Array) : 要檢查的陣列
- [iteratee=_.identity] (Array|Function|Object|string) : 迭代函數，呼叫每個元素

返回

- (Array) : 傳回新的去重後的陣列

範例

```
const objects = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
  { id: 1, name: 'Charlie' },
  { id: 3, name: 'David' },
];

// 基于 'id' 属性來确定元素的唯一性
const uniqueObjects = _.uniqBy(objects, 'id');

console.log(uniqueObjects);
// => [ { id: 1, name: 'Alice' }, { id: 2, name: 'Bob' }, { id: 3, name: 'David' } ]
```

#### 集合 Collection

##### 1. _.countBy(collection, [iteratee=_.identity])

參數

- collection (Array|Object) : 一個用來迭代的集合
- [iteratee=_.identity] (Array|Function|Object|string) : 一個迭代函數，用來轉換 key（鍵）

返回

- (Object) : 傳回一個組成集合物件

範例

```
const numbers = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4];

// 將元素的值分組，，並計算次數
const countByResult = _.countBy(numbers);

console.log(countByResult);
// => { '1': 1, '2': 2, '3': 3, '4': 4 }
```

##### 2. _.forEach(collection, [iteratee=_.identity])

參數

- collection (Array|Object) : 一個用來迭代的集合
- [iteratee=_.identity] (Function) : 每次迭代呼叫的函數

返回

- (\*) : 傳回集合 collection

範例

```
const array = [1, 2, 3, 4, 5];

_.forEach(array, (element) => {
  console.log(element);
});
```

```
const object = { a: 1, b: 2, c: 3 };

_.forEach(object, (value, key) => {
  console.log(key, value);
});
```

##### 3. _.every(collection, [predicate=_.identity])

參數

- collection (Array|Object) : 一個用來迭代的集合
- [predicate=_.identity] (Array|Function|Object|string) : 每次迭代呼叫的函數

返回

- (boolean) : 如果所有元素經 predicate（斷言函數） 檢查後都返回真值，那麼就返回 true，否則返回 false

範例

```
const numbers = [1, 2, 3, 4, 5];

// 使用 _.every 檢查陣列中的所有元素是否都大於 0
const allPositive = _.every(numbers, (num) => num > 0);

console.log(allPositive); // => true
```

```
const students = {
  Alice: { grade: 85 },
  Bob: { grade: 92 },
  Charlie: { grade: 78 },
};

// 使用 _.every 檢查所有學生的成績是否都大於等於 80
const allGoodGrades = _.every(students, (student) => student.grade >= 80);

console.log(allGoodGrades); // => false
```

##### 4. _.filter(collection, [predicate=_.identity])

參數

- collection (Array|Object) : 一個用來迭代的集合
- [predicate=_.identity] (Array|Function|Object|string) : 每次迭代呼叫的函數

返回

- (Array) : 傳回一個新的過濾後的陣列

範例

```
const numbers = [1, 2, 3, 4, 5];

// 使用 _.filter 篩選陣列中的所有偶數
const evenNumbers = _.filter(numbers, (num) => num % 2 === 0);

console.log(evenNumbers); // => [2, 4]
```

##### 5. _.find(collection, [predicate=_.identity], [fromIndex=0])

參數

- collection (Array|Object) : 一個用來迭代的集合
- [predicate=_.identity] (Array|Function|Object|string) : 每次迭代呼叫的函數
- [fromIndex=0] (number) : 開始搜尋的索引位置

返回

- (\*) : 傳回符合元素，否則回傳 undefined

範例

```
const numbers = [1, 2, 3, 4, 5];

// 使用 _.find 找數組第一個偶數
const firstEvenNumber = _.find(numbers, (num) => num % 2 === 0);

console.log(firstEvenNumber); // => 2
```

##### 6. _.flatMap(collection, [iteratee=_.identity])

參數

- collection (Array|Object) : 一個用來迭代遍歷的集合
- [iteratee=_.identity] (Array|Function|Object|string) : 每次迭代呼叫的函數

返回

- (Array) : 傳回新扁平化陣列

範例

```
function duplicate(n) {
  return [n, n];
}

_.flatMap([1, 2], duplicate);
// => [1, 1, 2, 2]
```

##### 7. _.groupBy(collection, [iteratee=_.identity])

參數

- collection (Array|Object) : 一個用來迭代的集合
- [iteratee=_.identity] (Array|Function|Object|string) : 這個迭代函數用來轉換 key

返回

- (Object) : 傳回一個組成聚合的物件

範例

```
const numbers = [1, 2, 3, 4, 5, 6];

// 使用 _.groupBy 根據元素的奇偶性分組
const groupedNumbers = _.groupBy(numbers, (num) => (num % 2 === 0) ? '偶數' : '奇數');

console.log(groupedNumbers);
/* 輸出:
{
  '奇數': [1, 3, 5],
  '偶數': [2, 4, 6]
}
*/
```

```
const students = {
  Alice: { grade: 'A' },
  Bob: { grade: 'B' },
  Charlie: { grade: 'A' },
  David: { grade: 'C' },
};

// 使用 _.groupBy 根據學生成績分組
const groupedStudents = _.groupBy(students, (student) => student.grade);

console.log(groupedStudents);
/* 輸出:
{
  'A': [ { grade: 'A' }, { grade: 'A' } ],
  'B': [ { grade: 'B' } ],
  'C': [ { grade: 'C' } ]
}
*/
```

##### 8. \_.includes(collection, value, [fromIndex=0])

參數

- collection (Array|Object|string) : 要檢索的集合
- value (\*) : 要檢索的值
- [fromIndex=0] (number) : 要檢索的索引位置

返回

- (boolean) : 如果找到 value 返回 true， 否則返回 false

範例

```
_.includes([1, 2, 3], 1);
// => true

_.includes([1, 2, 3], 1, 2);
// => false

_.includes({ 'user': 'fred', 'age': 40 }, 'fred');
// => true

_.includes('pebbles', 'eb');
// => true
```

##### 9. _.keyBy(collection, [iteratee=_.identity])

參數

- collection (Array|Object) : 用來迭代的集合
- [iteratee=_.identity] (Array|Function|Object|string) : 這個迭代函數用來轉換 key

返回

- (Object) : 傳回一個組成聚合的物件

範例

```
const students = {
  { id: 1, name: 'Alice', grade: 'A' },
  { id: 2, name: 'Bob', grade: 'B' },
  { id: 3, name: 'Charlie', grade: 'A' },
  { id: 4, name: 'David', grade: 'C' },
};

// 使用 _.keyBy 根據學生的 id 屬性創建物件
const studentsById = _.keyBy(students, 'id');

console.log(studentsById);
/* 輸出:
{
  '1': { id: 1, name: 'Alice', grade: 'A' },
  '2': { id: 2, name: 'Bob', grade: 'B' },
  '3': { id: 3, name: 'Charlie', grade: 'A' },
  '4': { id: 4, name: 'David', grade: 'C' }
}
*/
```

##### 10. _.map(collection, [iteratee=_.identity])

參數

- collection (Array|Object) : 用來迭代的集合
- [iteratee=_.identity] (Array|Function|Object|string) : 每次迭代呼叫的函數

返回

- (Array) : 傳回新的映射後陣列

範例

```
const numbers = [1, 2, 3, 4, 5];

const doubledNumbers = _.map(numbers, (num) => num * 2);

console.log(doubledNumbers); // => [2, 4, 6, 8, 10]
```

##### 11. _.orderBy(collection, [iteratees=[_.identity]], [orders])

參數

- collection (Array|Object) : 用來迭代的集合
- [iteratees=[_.identity]] (Array[]|Function[]|Object[]|string[]) : 排序的迭代函數
- [orders] (string[]) :iteratees 迭代函數的排序順序

返回

- (Array) : 排序排序後的新陣列

範例

```
const users = [
  { 'user': 'fred',   'age': 48 },
  { 'user': 'barney', 'age': 34 },
  { 'user': 'fred',   'age': 40 },
  { 'user': 'barney', 'age': 36 }
];

// 以 `user` 升序排序 再  `age` 以降序排序。
_.orderBy(users, ['user', 'age'], ['asc', 'desc']);
// => objects for [['barney', 36], ['barney', 34], ['fred', 48], ['fred', 40]]
```

##### 12. _.reduce(collection, [iteratee=_.identity], [accumulator])

參數

- collection (Array|Object) : 用來迭代的集合
- [iteratee=_.identity] (Function) : 每次迭代呼叫的函數
- [accumulator] (\*) : 初始值

返回

- (\*) : 傳回累加後的值

範例

```
_.reduce([1, 2], function(sum, n) {
  return sum + n;
}, 0);
// => 3
```

##### 13. \_.size(collection)

參數

- collection (Array|Object) : 要檢查的集合

返回

- (number) : 傳回集合的長度

範例

```
_.size([1, 2, 3]);
// => 3

_.size({ 'a': 1, 'b': 2 });
// => 2

_.size('pebbles');
// => 7
```

##### 14. _.some(collection, [predicate=_.identity])

參數

- collection (Array|Object) : 用來迭代的集合
- [predicate=_.identity] (Array|Function|Object|string) : 每次迭代呼叫的函數

返回

- (boolean) : 如果任意元素經 predicate 檢查都為 truthy（真值），返回 true，否則返回 false

範例

```
_.some([null, 0, 'yes', false], Boolean);
// => true

var users = [
  { 'user': 'barney', 'active': true },
  { 'user': 'fred',   'active': false }
];

// The `_.matches` iteratee shorthand.
_.some(users, { 'user': 'barney', 'active': false });
// => false

// The `_.matchesProperty` iteratee shorthand.
_.some(users, ['active', false]);
// => true

// The `_.property` iteratee shorthand.
_.some(users, 'active');
// => true
```

##### 15. _.sortBy(collection, [iteratees=[_.identity]])

參數

- collection (Array|Object) : 用來迭代的集合
- [iteratees=[_.identity]] (...(Array|Array[]|Function|Function[]|Object|Object[]|string|string[])) : 這個函數決定排序

返回

- (Array) : 傳回排序後的陣列

範例

```
const products = [
  { name: 'Apple', price: 2.5 },
  { name: 'Banana', price: 1.2 },
  { name: 'Orange', price: 3.0 },
  { name: 'Mango', price: 2.8 },
];

const sortedProducts = _.sortBy(products, (product) => product.price);

console.log(sortedProducts);
/* 輸出:
[
  { name: 'Banana', price: 1.2 },
  { name: 'Apple', price: 2.5 },
  { name: 'Mango', price: 2.8 },
  { name: 'Orange', price: 3.0 }
]
*/
```

#### 函數

##### 1. \_.debounce(func, [wait=0], [options=])

參數

- func (Function) : 要防抖動的函數
- [wait=0] (number) : 需要延遲的毫秒數
- [options=] (Object) : 選項物件
- [options.leading=false] (boolean) : 指定在延遲開始前呼叫
- [options.maxWait] (number) : 設定 func 允許被延遲的最大值
- [options.trailing=true] (boolean) : 指定在延遲結束後呼叫

返回

- (Function) : 傳回新的 debounced（防抖動）函數

範例

```
// 使用 Lodash 的 _.debounce 創建一個延遲執行的函數
const debouncedFunction = _.debounce((text) => {
  console.log(`用戶輸入: ${text}`);
}, 500);

// 模擬用戶輸入
debouncedFunction('A');
debouncedFunction('B');

// 500 毫秒後只執行一次回調
// 輸出: 用戶輸入: B
```

#### 語言 Lang

##### 1. \_.clone(value)

參數

- value (\*) : 要拷貝的值

返回

- (\*) : 傳回拷貝後的值

範例

```
const objects = [{ 'a': 1 }, { 'b': 2 }];

const shallow = _.clone(objects);
console.log(shallow[0] === objects[0]);
// => true
```

##### 2. \_.cloneDeep(value)

參數

- value (\*) : 要深拷貝的值

返回

- (\*) : 傳回拷貝後的值

範例

```
const objects = [{ 'a': 1 }, { 'b': 2 }];

const deep = _.cloneDeep(objects);
console.log(deep[0] === objects[0]);
// => false
```

##### 3. \_.isArray(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 是一個陣列返回 true，否則返回 false

範例

```
_.isArray([1, 2, 3]);
// => true

_.isArray('abc');
// => false
```

##### 4. \_.isBoolean(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 是一個布林值，那麼返回 true，否則返回 false

範例

```
_.isBoolean(false);
// => true

_.isBoolean(null);
// => false
```

##### 5. \_.isEmpty(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 為空，則返回 true，否則返回 false

範例

```
_.isEmpty(null);
// => true

_.isEmpty(true);
// => true

_.isEmpty(1);
// => true

_.isEmpty([1, 2, 3]);
// => false

_.isEmpty({ 'a': 1 });
// => false
```

##### 6. \_.isEqual(value, other)

參數

- value (\*) : 用來比較的值
- other (\*) : 另一個用來比較的值

返回

- (boolean) : 如果兩個值完全相同，那麼返回 true，否則返回 false

範例

```
const object = { 'a': 1 };
const other = { 'a': 1 };

_.isEqual(object, other);
// => true

object === other;
// => false
```

##### 7. \_.isError(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 是一個錯誤（Error）對象，那麼返回 true，否則返回 false

範例

```
_.isError(new Error);
// => true

_.isError(Error);
// => false
```

##### 8. \_.isFunction(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 是一個函數，那麼返回 true，否則返回 false

範例

```
_.isFunction(_);
// => true

_.isFunction(/abc/);
// => false
```

##### 9. \_.isNaN(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 是一個 NaN，那麼返回 true，否則返回 false

範例

```
_.isNaN(NaN);
// => true

_.isNaN(new Number(NaN));
// => true

isNaN(undefined);
// => true

_.isNaN(undefined);
// => false
```

##### 10. \_.isNil(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 為 null 或 undefined，那麼返回 true，否則返回 false

範例

```
_.isNil(null);
// => true

_.isNil(void 0);
// => true

_.isNil(NaN);
// => false
```

##### 11. \_.isNull(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 為 null，那麼返回 true，否則返回 false

範例

```
_.isNull(null);
// => true

_.isNull(void 0);
// => false
```

##### 12. \_.isNumber(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 為一個數值，則返回 true，否則返回 false

範例

```
_.isNumber(3);
// => true

_.isNumber(Number.MIN_VALUE);
// => true

_.isNumber(Infinity);
// => true

_.isNumber('3');
// => false
```

##### 13. \_.isObject(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 為一個對象，那麼返回 true，否則返回 false

範例

```
_.isObject({});
// => true

_.isObject([1, 2, 3]);
// => true

_.isObject(_.noop);
// => true

_.isObject(null);
// => false
```

##### 14. \_.isString(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 為一個字串，那麼返回 true，否則返回 false

範例

```
_.isString('abc');
// => true

_.isString(1);
// => false
```

##### 15. \_.isUndefined(value)

參數

- value (\*) : 要檢查的值

返回

- (boolean) : 如果 value 是 undefined，那麼返回 true，否則返回 false

範例

```
_.isUndefined(void 0);
// => true

_.isUndefined(null);
// => false
```

##### 16. \_.toArray(value)

參數

- value (\*) : 要轉換的值

返回

- (Array) : 傳回轉換後的陣列

範例

```
_.toArray({ 'a': 1, 'b': 2 });
// => [1, 2]

_.toArray('abc');
// => ['a', 'b', 'c']

_.toArray(1);
// => []

_.toArray(null);
// => []
```

##### 17. \_.toNumber(value)

參數

- value (\*) : 要處理的值

返回

- (number) : 回傳數字

範例

```
_.toNumber(3.2);
// => 3.2

_.toNumber(Number.MIN_VALUE);
// => 5e-324

_.toNumber(Infinity);
// => Infinity

_.toNumber('3.2');
// => 3.2
```

##### 18. \_.toString(value)

參數

- value (\*) : 要處理的值

返回

- (string) : 傳回字串

範例

```
_.toString(null);
// => ''

_.toString(-0);
// => '-0'

_.toString([1, 2, 3]);
// => '1,2,3'
```

#### 數學

##### 1. \_.floor(number, [precision=0])

參數

- number (number) : 要向下捨入的值
- [precision=0] (number) : 向下舍入的精度

返回

- (number) : 傳迴向下舍入的值

範例

```
_.floor(4.006);
// => 4

_.floor(0.046, 2);
// => 0.04

_.floor(4060, -2);
// => 4000
```

##### 2. \_.max(array)

參數

- array (Array) : 要迭代的陣列

返回

- (\*) : 傳回最大的值

範例

```
_.max([4, 2, 8, 6]);
// => 8

_.max([]);
// => undefined
```

##### 3. \_.min(array)

參數

- array (Array) : 要迭代的陣列

返回

- (\*) : 傳回最小的值

範例

```
_.min([4, 2, 8, 6]);
// => 2

_.min([]);
// => undefined
```

##### 4. \_.round(number, [precision=0])

參數

- number (number) : 要四捨五入的數字
- [precision=0] (number) : 四捨五入的精度

返回

- (number) : 傳回四捨五入的數字

範例

```
_.round(4.006);
// => 4

_.round(4.006, 2);
// => 4.01

_.round(4060, -2);
// => 4100
```

##### 5. \_.sum(array)

參數

- array (Array) : 要迭代的陣列

返回

- (number) : 傳回總和

範例

```
_.sum([4, 2, 8, 6]);
// => 20
```

#### 數位

##### 1. \_.random([lower=0], [upper=1], [floating])

參數

- [lower=0] (number) : 下限
- [upper=1] (number) : 上限
- [floating] (boolean) : 指定是否傳回浮點數

返回

- (number) : 傳回隨機數

範例

```
_.random(0, 5);
// => an integer between 0 and 5

_.random(5);
// => also an integer between 0 and 5

_.random(5, true);
// => a floating-point number between 0 and 5

_.random(1.2, 5.2);
// => a floating-point number between 1.2 and 5.2
```

#### 物件

##### 1. \_.assign(object, [sources])

參數

- object (Object) : 目標物件
- [sources] (...Object) : 來源物件

返回

- (Object) : 返回 object

範例

```
const target = { a: 1, b: 2 };
const source = { b: 3, c: 4 };

// 使用 _.assign 將 source 對象的屬性複製到 target 對象
const mergedObject = _.assign(target, source);

console.log(mergedObject);
/* 輸出:
{
  a: 1,
  b: 3,
  c: 4
}
*/
```

##### 2. \_.get(object, path, [defaultValue])

參數

- object (Object) : 要檢索的物件
- path (Array|string) : 要取得屬性的路徑
- [defaultValue] (\*) : 如果解析值是 undefined，這值會被回傳

返回

- (\*) : 傳回解析的值

範例

```
const object = { 'a': [{ 'b': { 'c': 3 } }] };

_.get(object, 'a[0].b.c');
// => 3

_.get(object, ['a', '0', 'b', 'c']);
// => 3

_.get(object, 'a.b.c', 'default');
// => 'default'
```

##### 3. \_.has(object, path)

參數

- object (Object) : 要檢索的物件
- path (Array|string) : 要檢查的路徑 path

返回

- (boolean) : 如果 path 存在，那麼返回 true，否則返回 false

範例

```
const object = { a: 1, b: 2, c: 3 };

const hasPropertyB = _.has(object, 'b');
console.log(hasPropertyB); // => true
```

##### 4. \_.keys(object)

參數

- object (Object) : 要檢索的物件

返回

- (Array) : 傳回包含屬性名的陣列

範例

```
const object = { a: 1, b: 2, c: 3 };

const keys = _.keys(object);

console.log(keys); // => ['a', 'b', 'c']
```

##### 5. _.mapKeys(object, [iteratee=_.identity])

參數

- object (Object) : 要遍歷的物件
- [iteratee=_.identity] (Function) : 每次迭代時呼叫的函數

返回

- (Object) : 傳回映射後的新物件

範例

```
const originalObject = { firstName: 'Alice', lastName: 'Johnson' };

const newObject = _.mapKeys(originalObject, (value, key) => {
  // 轉換大寫
  return key.toUpperCase();
});

console.log(newObject);
/* 輸出:
{
  FIRSTNAME: 'Alice',
  LASTNAME: 'Johnson'
}
*/
```

##### 6. _.mapValues(object, [iteratee=_.identity])

參數

- object (Object) : 要遍歷的物件
- [iteratee=_.identity] (Function) : 每次迭代時呼叫的函數

返回

- (Object) : 傳回映射後的新物件

範例

```
const originalObject = { a: 1, b: 2, c: 3 };

const newObject = _.mapValues(originalObject, (value) => {
  return value * 2;
});

console.log(newObject);
/* 輸出:
{
  a: 2,
  b: 4,
  c: 6
}
*/
```

##### 7. \_.merge(object, [sources])

參數

- object (Object) : 目標物件
- [sources] (...Object) : 來源物件

返回

- (Object) : 返回 object

範例

```
const object = {
  'a': [{ 'b': 2 }, { 'd': 4 }]
};

const other = {
  'a': [{ 'c': 3 }, { 'e': 5 }]
};

_.merge(object, other);
// => { 'a': [{ 'b': 2, 'c': 3 }, { 'd': 4, 'e': 5 }] }
```

##### 8. \_.omit(object, [props])

參數

- object (Object) : 來源物件
- [props] (...(string|string[])) : 要被忽略的屬性。（註：單獨指定或指定在數組中。）

返回

- (Object) : 傳回新物件

範例

```
const object = { 'a': 1, 'b': '2', 'c': 3 };

_.omit(object, ['a', 'c']);
// => { 'b': '2' }
```

##### 9. _.omitBy(object, [predicate=_.identity])

參數

- object (Object) : 來源物件
- [predicate=_.identity] (Function) : 呼叫每一個屬性的函數

返回

- (Object) : 傳回新物件

範例

```
const object = { 'a': 1, 'b': '2', 'c': 3 };

_.omitBy(object, _.isNumber);
// => { 'b': '2' }
```

##### 10. \_.pick(object, [props])

參數

- object (Object) : 來源物件
- [props] (...(string|string[])) : 要被忽略的屬性。（註：單獨指定或指定在數組中。）

返回

- (Object) : 傳回新物件

範例

```
const object = { 'a': 1, 'b': '2', 'c': 3 };

_.pick(object, ['a', 'c']);
// => { 'a': 1, 'c': 3 }
```

##### 11. _.pickBy(object, [predicate=_.identity])

參數

- object (Object) : 來源物件
- [predicate=_.identity] (Function) : 呼叫每一個屬性的函數

返回

- (Object) : 傳回新物件

範例

```
const object = { 'a': 1, 'b': '2', 'c': 3 };

_.pickBy(object, _.isNumber);
// => { 'a': 1, 'c': 3 }
```

##### 12. \_.set(object, path, value)

參數

- object (Object) : 要修改的物件
- path (Array|string) : 要設定的物件路徑
- value (\*) : 要設定的值

返回

- (Object) : 返回 object

範例

```
const object = { 'a': [{ 'b': { 'c': 3 } }] };

_.set(object, 'a[0].b.c', 4);
console.log(object.a[0].b.c);
// => 4

_.set(object, ['x', '0', 'y', 'z'], 5);
console.log(object.x[0].y.z);
// => 5
```

##### 13. _.transform(object, [iteratee=_.identity], [accumulator])

參數

- object (Object) : 要遍歷的對象
- [iteratee=_.identity] (Function) : 每次迭代時呼叫的函數
- [accumulator] (\*) : 定制疊加的值

返回

- (\*) : 傳回疊加後的值

範例

```
const numbers = [1, 2, 3, 4, 5];

const sum = _.transform(numbers, (result, num) => {
  result.sum += num;
}, { sum: 0 });

console.log(sum); // => { sum: 15 }
```

##### 14. \_.unset(object, path)

參數

- object (Object) : 要修改的物件
- path (Array|string) : 要移除的物件路徑

返回

- (boolean) : 如果移除成功，那麼返回 true，否則返回 false

範例

```
const object = { 'a': [{ 'b': { 'c': 7 } }] };
_.unset(object, 'a[0].b.c');
// => true

console.log(object);
// => { 'a': [{ 'b': {} }] };

_.unset(object, ['a', '0', 'b', 'c']);
// => true

console.log(object);
// => { 'a': [{ 'b': {} }] };
```

##### 15. \_.values(object)

參數

- object (Object) : 要檢索的物件

返回

- (Array) : 傳回物件屬性的值的陣列

範例

```
const object = { a: 1, b: 2, c: 3 };

const values = _.values(object);
console.log(values); // => [1, 2, 3]
```

#### 字串

##### 1. \_.replace([string=''], pattern, replacement)

參數

- [string=''] (string) : 待替換的字串
- pattern (RegExp|string) : 要符合的內容
- replacement (Function|string) : 替換的內容

返回

- (string) : 傳回替換後的字串

範例

```
_.replace('Hi Fred', 'Fred', 'Barney');
// => 'Hi Barney'
```

##### 2. \_.split([string=''], separator, [limit])

參數

- [string=''] (string) : 要拆分的字串
- separator (RegExp|string) : 拆分的分隔符號
- [limit] (number) : 限制結果的數量

返回

- (Array) : 傳回拆分部分的字串的陣列

範例

```
_.split('a-b-c', '-', 2);
// => ['a', 'b']
```

##### 3. \_.trim([string=''], [chars=whitespace])

參數

- [string=''] (string) : 要處理的字串
- [chars=whitespace] (string) : 要移除的字元

返回

- (string) : 傳回處理後的字串

範例

```
_.trim('  abc  ');
// => 'abc'

_.trim('-_-abc-_-', '_-');
// => 'abc'

_.map(['  foo  ', '  bar  '], _.trim);
// => ['foo', 'bar']
```

#### 實用函數

##### 1. _.flow([funcs])

參數

- [funcs] (...(Function|Function[])) : 要呼叫的函數

返回

- (Function) : 傳回新的函數

範例

```
const idsStr = _.flow(
  () => _.map(listings, 'id'),
  ids => _.join(ids, '，')
)()
```

##### 2. _.range([start=0], end, [step=1])

參數

- [start=0] (number) : 開始的範圍
- end (number) : 結束的範圍
- [step=1] (number) : 範圍的增量或減量

返回

- (Array) : 傳回範圍內數字組成的新陣列

範例

```
_.range(4);
// => [0, 1, 2, 3]
 
_.range(-4);
// => [0, -1, -2, -3]
 
_.range(1, 5);
// => [1, 2, 3, 4]
 
_.range(0, 20, 5);
// => [0, 5, 10, 15]
 
_.range(0, -4, -1);
// => [0, -1, -2, -3]
 
_.range(1, 4, 0);
// => [1, 1, 1]
 
_.range(0);
// => []
```

##### 3. _.times(n, [iteratee=_.identity])

參數

- n (number) : 呼叫iteratee的次數
- [iteratee=_.identity] (Function) : 每次迭代呼叫的函數

返回

- (Array) : 傳回呼叫結果的陣列

範例

```
_.times(3, String);
// => ['0', '1', '2']
 
 _.times(4, _.constant(0));
// => [0, 0, 0, 0]
```

### 結語

---

關於 Lodash 筆記會持續更新～
