---
{"dg-publish":true,"permalink":"/01-编程技术/前端/JSUtil.js/","dgPassFrontmatter":true,"created":"2023-10-27T09:00:35.318+08:00","updated":"2024-01-19T08:47:56.000+08:00"}
---


### JSON对象数组排序
```javascript
//倒排  
this.gasInfos.sort((v1,v2)=>{
  return v2.dateTime<v1.dateTime?-1:1;
});
//正排  
this.gasInfos.sort((v1,v2)=>{
  return v1.dateTime<v2.dateTime?-1:1;
});
```


### 时间格式化
```javascript
//H和h都以24小时记，不支持12小时模式
Date.prototype.format = function (format) {
  var o = {
      "M+": this.getMonth() + 1, //month
      "d+": this.getDate(),    //day
      "h+": this.getHours(),   //hour
      "H+": this.getHours(),   //hour
      "m+": this.getMinutes(), //minute
      "s+": this.getSeconds(), //second
      "q+": Math.floor((this.getMonth() + 3) / 3),  //quarter
      "S": this.getMilliseconds() //millisecond
  }
  if (/(y+)/.test(format)) format = format.replace(RegExp.$1,
  (this.getFullYear() + "").substr(4 - RegExp.$1.length));
  for (var k in o) if (new RegExp("(" + k + ")").test(format))
      format = format.replace(RegExp.$1,
      RegExp.$1.length == 1 ? o[k] :
      ("00" + o[k]).substr(("" + o[k]).length));
  return format;
}
```
使用方法
```javascript
new Date().format("yyyy-MM-dd")
//输出
'2022-04-20'
```


### 纯JS复制内容到剪切板
```javascript
copyText(text) {
  if (navigator.clipboard) {
      // clipboard api 复制
      navigator.clipboard.writeText(text);
  } else {
      var textarea = document.createElement('textarea');
      document.body.appendChild(textarea);
      // 隐藏此输入框(不调整样式的话会，增加删除元素会导致滚动条异常)
      textarea.style.position = 'fixed';
      textarea.style.clip = 'rect(0 0 0 0)';
      textarea.style.top = '10px';
      // 赋值
      textarea.value = text;
      // 选中
      textarea.select();
      // 复制
      document.execCommand('copy', true);
      // 移除输入框
      document.body.removeChild(textarea);
 }
```


### 判断字符串是否无效
```javascript
isBlank(text){
  if(text===""||text===null||text===undefined)
  {
    return true;
  }
  else{
    text = text.replaceAll(" ","");
    if(text==="")
    {
      return true;
    }
  }
  return false;
}
```

### 日期增减
```javascript
/**
* 今日时间 加x天 , 减x天后的日期
* @param count 正数加 或者 负数减
* */
function rangeDate(count) {
  var dd = new Date();
  dd.setDate(dd.getDate() + count);
  var y = dd.getFullYear();
  var m = dd.getMonth() + 1;
  var d = dd.getDate();
  return y + "-" + m + "-" + d;
 }
```
