# 移动端touch事件



### 1.1 click事件300毫秒延迟的问题

+ 原因：和双击事件产生出冲突，判定双击事件的原理是第一次点击后300毫秒内再次点击

+ 解决办法，移动端新增了touch触摸事件

  + `touchstart`：手指按下
  + `touchmove`：手指滑动
  + `touchend`：手指抬起
  + `touchcancel`：手机弹出框处理后

  

### 1.2 移动端event事件

+ type：当前触发的触摸事件类型
+ `e.target`：手指按下的元素本身，在手指滑动，抬起事件中`e.target`，都表示手指按下时的元素
+ `touches`：获取屏幕上手指的所有触摸点
+ `changedTouches`：获取和当前事件相关联的触摸点
+ `targetTouches`：获取当前元素上面的所有触摸点
  + `identifier`：触点的标识



### 1.3  touch事件封装

+ 单击事件

+ 长按事件

+ ```js
  //封装移动端单点和长按事件
  ;(function(doc){
      var Touch = function(sele){
          return Touch.prototype.init(sele)
      }
  
      Touch.prototype = {
          init: function(sele){
              if(typeof(sele) === 'string'){
                  this.ele = doc.querySelector(sele);
                  //console.log(this);
                  return this;
              }
          },
  
          //点击事件
          tap: function(callback){
              var sTime,
                  eTime;
              this.ele.addEventListener('touchstart', fn, false);
              this.ele.addEventListener('touchend', fn, false);
  
              function fn(e){
                  //阻止click事件
                  e.preventDefault();
                  switch(e.type){
                      case 'touchstart':
                          sTime = new Date().getTime();
                          break;
                      case 'touchend':
                          eTime = new Date().getTime();
                          if(eTime - sTime < 500){
                              callback.call(this, e);
                          }
                          break;
                  }
              }
          },
  
          //长按事件
          longtap: function(callback){
              var t = null,
                  seft = this;
              this.ele.addEventListener('touchstart', fn, false);
              this.ele.addEventListener('touchmove', fn, false);
              this.ele.addEventListener('touchend', fn, false);
  
              function fn(e){
                  //阻止click事件
                  e.preventDefault();
  
                  switch(e.type){
                      case 'touchstart':
                          t = setTimeout(function(){
                              callback.call(seft, e)
                          },500)
                          break;
                      case 'touchmove':
                          clearTimeout(t);
                          t = null;
                          break;
                      case 'touchend':
                          clearTimeout(t);
                          t = null;
                          break;
                  }
              }
          }
      }
  
      window.Touch = Touch
  })(document);
```
  
  

### 1.4 touch事件穿透问题

+ 产生原因：是由于click的300毫秒延迟问题
+ 解决方案
  + 利用封装好的touch事件（如上边）
  + 利用`fastclick.js`插件
    + 引入之后，初始化`FastClick.attach(document.body)`，之后直接使用`click`事件，不会出现延迟和穿透的问题

