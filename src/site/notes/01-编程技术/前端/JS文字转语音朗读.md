---
{"dg-publish":true,"permalink":"/01-编程技术/前端/JS文字转语音朗读/","dgPassFrontmatter":true,"created":"2023-10-27T09:00:35.316+08:00","updated":"2024-01-19T08:47:56.000+08:00"}
---


```javascript
  var u = new SpeechSynthesisUtterance();
  u.text = "我爱中国";
  u.lang = "zh";
  u.rate = 0.7;
  speechSynthesis.speak(u);
```


### 按位朗读数字
在朗读数字的时候，默认会按照数学的方式读出来，比如“100”会读成“一百”而不是“一零零”，但有时候需要按位挨个朗读，这里采取将数字转中文的办法
```javascript
numberToZhCN(number) {
    let zh = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九'];
    let array = number.toString().split("");
    let result = "";
    for (let i = 0; i < array.length; i++) {
      switch (array[i]) {
        case "+":
          result += "正";
          break;
        case "-":
          result += "负";
          break;
        case ".":
          result += "点";
          break;
        default:
          {
            //是数字
              if(!isNaN(array[i]))
              {
                result += zh[array[i]];
              }
              else{
                result += array[i];
              }
          }break;
      }
    }
    return result;
  }
```
