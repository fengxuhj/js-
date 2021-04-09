# 高级函数reduce方法



### 1.语法

+ `arr.reduce(callback, initial)`，reduce方法接收两个参数（回调函数，初始值）；
+ 在没有传递初始值的情况下，方法的索引会从 1 开始，把索引 0 对应的值当成初始值；

### 2.reduce的应用

+ 数组求和

+ ```js
  //没有传递初始值
  var arr = [1, 2, 3, 4];
  var sum = arr.reduce(function(returnVal, item, index, arr) {
      console.log(returnVal, item, index);
      return returnVal + item;
  })
  console.log(arr, sum);
  /*
  1 2 1
  3 3 2
  6 4 3
  [1, 2, 3, 4] 10
  */
  
  //传递初始值
  var arr = [1, 2, 3, 4];
  var sum = arr.reduce(function(returnVal, item, index, arr) {
      console.log(returnVal, item, index);
      return returnVal + item;
  }, 0)
  console.log(arr, sum);
  /*
  0 1 0
  1 2 1
  3 3 2
  6 4 3
  [1, 2, 3, 4] 10
  */
  ```

+ 统计数组中每个元素出现的次数

+ ```js
  let names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice', 'Bob', 'Alice'];
  
  let nameNum = names.reduce((returnVal, item)=>{
    if(returnVal in item){//returnVal.hasOwnProperty(item)
      returnVal[item]++;
    }else{
      returnVal[item] = 1;
    }
    return returnVal;
  }, {})
  console.log(nameNum); //{Alice: 3, Bob: 2, Tiff: 1, Bruce: 1}
  ```

+ 数组去重

+ ```js
  let arr = [1,2,3,4,4,1];
  let newArr = arr.reduce((returnVal, item)=>{
      if(!returnVal.includes(item)){
         return returnVal.concat(item);
      }else{
          return returnVal;
      }
  }, [])
  console.log(newArr); //[1,2,3,4]
  ```

+ 二维数组转一维数组

+ ```js
  let arr = [[0, 1], [2, 3], [4, 5]];
  const newArr = arr.reduce((returnVal, item)=>{
      return returnVal.concat(item);
  }, [])
  ```

+ 多维数组转一维数组

+ ```js
  let arr = [[0, 1], [2, 3], [4, [5, 6, 7]]];
  const newArr = function(arr){
     return arr.reduce((returnVal, item)=>{
         return returnVal.concat(Array.isArray(item) ? newArr(item) : item);
     }, [])
  }
  console.log(newArr(arr)); //[0, 1, 2, 3, 4, 5, 6, 7]
  ```

  