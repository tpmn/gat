# TPMN AdTag for app(GAT)

*참고 : GAT는 TPMN AdTag for app의 약어입니다.

## About?

앱 매체사 또는 개발자에게 TPMN Exchange를 연결하여 광고를 송출할 수 있도록 설계되었습니다.<br>
AdTag를 활용하므로 별도 광고 SDK 설치 없이 쉬운 설정을 통해 자유로운 광고 거래가 가능합니다.

**지원하는 광고 유형**
- Banner

**현재 지원하지 않는 광고 유형**
- Native
- Video

## 시작하기

### 연동 순서

- TPMN AdTag for app InventoryId 발급
- AD SDK에 Tag 등록
- JsTag의 CallBackFunction 정의
- 테스트 진행


### TPMN AdTag for app InventoryId 발급

- Ad Tag를 사용하려면 InventoryId 발급이 선행되어야 합니다.
- 광고지면의 InventoryId 발급문의는 TPMN 매니저 또는 대표메일(info@tpmn.io)로 문의 주시기 바랍니다.
> 소속 및 이름 : 
> 
> AppBundle Id : 
>
> 연락처(Phone) :
> 
> 내용 : 
> 
> Applobin SDK 사용 여부 :



### AdTag 요소

`<div>` : 광고가 나가는 영역입니다. 
- 광고 요청시(loadAd)시 ***해당 div객체 ID를 동일하게 설정*** 하는 것을 ***권장***합니다.
- "div_" + InventoryId
<pre>
#Inventory ID가 1234인 경우 예시 :
< div id="<b>div_1234</b>" sytle="..." />
GAT.loadAd({
    divid : "<b>div_1234</b>",
    inventoryid: "<b>1234</b>",
    ....
</pre>

`<script>` : https://static.tpmn.io/gat/ads.js 파일을 통해 광고처리를 수행합니다.

`tagCallbackFunction()` : 광고수신 정상유무를 처리하기위한 Callback 함수입니다.

```html
<div id="div_InventoryId" style="width:100%;text-align:center;margin:0 auto;padding:0;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function tagCallbackFunction (status)
{
    if (status == "OK"){
        console.log("Success");
        //TO-DO Custom Action..
    } else {
        console.log("Fail");
        //TO-DO Custom Action..
    }
};
GAT.loadAd({
      divid : "div_InventoryId",
      inventoryid: "${TAG_INVENTORY_ID}",
      adverid: "${DEVICE_IDFA}"
}, tagCallbackFunction);
</script>
```


### AdTag Paramter 설명

| parameter | type   | required | 설명                   | Escaped |
|-----------|--------|----------|----------------------|---------|
| `divid`     | string | Yes      | 광고영역id (랜더링영역)   |         |
| `inventoryid`        | string | Yes      | GAT Inventory Id     |         |
| `adverid`   | string | Yes      | idfa                 |         |
| `dnt`       | string |          | Do Not Track         |         |
| `ipv4`      | string |          | IP Address           |         |
| `latitude`  | string |          | Latitude             |         |
| `longitude` | string |          | Longitude            |         |
| `requestid` | string |          | Request ID           |         |
| `useragent` | string |          | User Agent           | Yes     |


### Response status CallbackFunction 설명

- TPMN AdTag for app 통한 광고 응답 상태 따른 SDK에서 처리해야할 Action을 수행하면 됩니다.
- CallbackFunction 이름은 수정이 가능합니다.

### Response CallbackFunction status parameter

| status  | type   | 상태    |
|---------|--------|-------|
| `OK`      | string | 광고수신  |
| `NOBID`   | string | 광고없음  |
| `INVALID` | string | 잘못된요청 |
| `ERROR`   | string | 오류    |

**OK**

- 정상적으로 응답을 받았으며 광고 HTML이 포함된 경우입니다.


**NOBID**

- 광고가 없는 상태 입니다. 


**INVALID**

- 잘못된 요청인 경우입니다.
- 필수 파라미터가 누락이 또는 공백일 경우에 발생합니다.
- 잘못된 InventoryId를 입력하였을때 발생합니다.


**ERROR**

- 요청을 처리 중 오류가 발생한 경우입니다.
- 지원하지 않는 형식인 경우 발생합니다.


----

# Example

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
<meta http-equiv="Cache-control" content="no-cache">
<meta http-equiv="imagetoolbar" content="no"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no"/>
<style type="text/css">
body {
    margin: 0;
    padding: 0;
}
</style>
</head>
<body>
<div id="div_1234" style="width:100%;text-align:center;margin:0 auto;padding:0;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function tagCbStatus (status)
{
if (status == "OK"){console.log("Success!");}
else {console.log("Fail.");}
};
GAT.loadAd({
      divid : "div_1234",
      inventoryid: "1234",
      adverid: "ec6eb127-a34b-4e7f-a623-78bd6205cfd2"
}, tagCbStatus);
</script>
</body>
</html>
```
