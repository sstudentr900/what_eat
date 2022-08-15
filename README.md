# 現在要去哪裡??
## Google Maps Embed API
```
  //查詢並變更地圖
  map.src = 'https://www.google.com/maps/embed/v1/place?key=keyID&q=查詢文字';
  map.addEventListener('load', () => {}, false);
```

## apps script SpreadSheet 程式碼.gs
```
  function doGet(e) {
    //打開google excel,URL:https://../d/【excel_ID】】/edit.. 
    var SpreadSheet = SpreadsheetApp.openById("excel_ID");

    //取得 第一個表單
    var Sheet = SpreadSheet.getSheets()[0];

    //取得ajax的data 
    var params = e.parameter; 
    var action  = params.action;
    // Logger.log("action: %s", action);

    //預設返回值
    var result= [];

    //判斷動作
    if (action === 'queryData') {
      result = queryData(Sheet, params);
    } 
    if (action === 'updateData'){
      result = updateData(Sheet, params);
    } 

    //回傳json
    return ContentService.createTextOutput(JSON.stringify(result)).setMimeType(ContentService.MimeType.JSON);
  }

  function queryData(Sheet,params) {

    //取得最後一列值的索引
    //var LastRow = Sheet.getLastRow();

    //取得excel
    var array = Sheet.getDataRange().getValues(); //[[1],[2]]

    //二維轉一維
    var data = [].concat.apply([],array);//[1,2]
    // Logger.log("data: %s", data);
    
    return data;
  }

  function updateData(Sheet,params) {

    //清除全部Sheet
    Sheet.clear();

    //字串轉數組
    var array = params.array.split(',');

    //存入sheet
    array.forEach((e,i)=>{
      Logger.log("e: %s", e);
      Logger.log("i: %s", i);
      Sheet.getRange(i+1, 1).setValue(e)
    });

    return [true];
  }
```
## apps script SpreadSheet debug.gs
```
  function debug() {
    //執行doGet
    var Result = doGet({
      //parameter預設,裡面放ajax,data的值
      parameter:  {
        action: "updateData",
        array:'11,22,33,44,55'
      },
    });
    Logger.log("Result: %s", Result);
  }
```
## 選轉
```
  .box{
    width: 520px;
    height: 710px;
    margin: auto;
    position: relative;
    /* 翻轉時的深度,值越大效果越小 */
    perspective: 4000px;
  }
  .box>div{
    position: absolute;
    width: 100%;
    height: 100%;
    padding: 30px;
    background-color: #fff;
    border-radius: 12px;
    box-shadow: 0 0 30px rgb(0 0 0 / 10%);
    /* 翻轉背面隱藏 */
    backface-visibility: hidden;
    transition: all 0.3s ease-in-out;
  }
  .box_front{
    /* 正面 */
    transform: rotateY(0deg);
  }
  #sample:checked+.pageBg .box_front{
    /* 翻轉 */
    transform: rotateY(180deg);
  }
  .box_back{
     /* 背面 */
    transform: rotateY(-180deg);
  }
  #sample:checked+.pageBg .box_back{
    /* 翻轉 */
    transform: rotateY(0deg);
  }
```    
## 連結
[該網頁連結](https://sstudentr900.github.io/what_eat/){:target="_blank"}

