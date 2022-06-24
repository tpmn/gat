
# GAT - Applovin MAX Integration

**Applovin MAX 광고 지원 여부**

| TYPE         | 지원여부     |
|--------------|----------|
| `Banner`       | Yes      |
| `Interstitial` | Yes      |
| `MREC`         | Yes      |
| `Native`       | Not yet  |
| `Rewarded`     | Not yet  |


## 시작하기

**사전정보**

- Applovin MAX의 Custom JS Tag Network방식을 활용하여 GAT Tag와 연동을 합니다.
- 이를 위해 아래 사항을 확인하고 설정이 필요합니다.


**연동 순서**

- GAT 인벤토리 ID 발급
- MAX Custom JS Tag Network 등록(광고 송수신을 위한 CallBackFunction 필수 정의)
- MAX Ad Units에 MAX GAT Custom Netowork Inventory 설정후 활성화
- 테스트 진행


**GAT JsTag**

- [x] Applovin SDK 매크로 지원
- [x] Callback을 통한 Applovin SDK 처리 지원


**Applovin MAX**

- [x] Manage → Network → 신규 Custom Network 등록
- [x] Manage → Ad Units → Custom Network(GAT Network) → 설정값 입력(PlacementID,CPM Price,Country Targeting) → Status 활성화
- 참고 : [Applovin Max 설정방법(Notion)](https://www.notion.so/tpmnkorea/Applovin-MAX-6c6e8097c92f41ab8a5d8bb4933354a6)
- 참고 : [Custom JS Tag Network Integration Guide Ref.](https://dash.applovin.com/documentation/mediation/android/mediation-setup/jstag)


**APP**

- [x] 광고영역을 Applovin Max SDK로 연동하여 광고가 나오는지 확인 합니다.


### Max GAT Tag Template

- `<div>` : 광고가 나가는 영역입니다. 해당 div객체 ID를 GAT는 인자로 받습니다.
- `<script>` : https://static.tpmn.io/gat/ads.js 파일을 통해 광고처리를 수행합니다.
- `GatCallbackFunction()` : 광고수신 정상유무를 처리하기위한 Callback 함수입니다.

```html
<div id="div_adInventory" style="width:100%;text-align:center;margin:0 auto;padding:0;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function GatCallbackFunction (status)
{
    if (status == "OK"){
        <!-- Max에서 정상 광고 수신시 노출하기 위해 해당 이벤트가 필요합니다.삭제하지 마세요.-->
        loaded=true; window.location="applovin://load";
    }else {
        <!-- Max에서 광고 없음을 수신하고 다음 동작을 하기 위해 해당 이벤트가 필요합니다.삭제하지 마세요.-->
        loaded=true; window.location="applovin://failLoad";
    }
};
GAT.loadAd({
      divid : "div_adInventory",
      inventoryid: "%%PLACEMENTID%%",
      adverid: "%%ADVERTISING_ID%%",
      dnt : "%%DNT%%",
      ipv4 : "%%IPADDRESS%%",
      latitude : "%%LATITUDE%%",
      longitude : "%%LONGITUDE%%",
      requestid : "%%REQUESTID%%"
}, GatCallbackFunction);
</script>
```


### JsTag Paramter 설명

| parameter | type   | required | 설명                   | Escaped | Macro               |
|-----------|--------|----------|----------------------|---------|---------------------|
| `divid`     | string | Yes      | 광고영역id (랜더링영역)   |         |                     |
| `ii`        | string | Yes      | GAT Inventory Tag Id |         | %%PLACEMENTID%%     |
| `adverid`   | string | Yes      | idfa                 |         | %%ADVERTISING_ID%%  |
| `dnt`       | string |          | Do Not Track         |         | %%DNT%%             |
| `ipv4`      | string |          | IP Address           |         | %%IPADDRESS%%       |
| `latitude`  | string |          | Latitude             |         | %%LONGITUDE%%       |
| `longitude` | string |          | Longitude            |         | %%LATITUDE%%        |
| `requestid` | string |          | Request ID           |         | %%REQUESTID%%       |
| `useragent` | string |          | User Agent           | Yes     | %%USERAGENT%%       |


### Response status CallbackFunction 설명

- 추가 구현을 통해 광고 응답 상태 따른 Action을 수행할 수 있습니다.(MAX 플랫폼에서 필요한 사항은 제거하지 말아주세요!)
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
- SDK에 광고 응답 및 노출 집계 처리를 수행하면 됩니다.


**NOBID**

- 광고가 없는 상태 입니다.
- SDK에서 광고가 없는 상태를 집계 및 처리를 수행하면 됩니다.


**INVALID**

- 잘못된 요청인 경우입니다.
- 필수 파라미터가 누락이 또는 공백일 경우에 발생합니다.
- 잘못된 Inverntory ID를 입력하였을때 발생합니다.


**ERROR**

- 요청을 처리 중 오류가 발생한 경우입니다.
- 지원하지 않는 형식인 경우 발생합니다.


----

# Example

## Banner

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
<div id="div_max" style="width:100%;text-align:center;margin:0 auto;padding:0;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function GatCallbackAd (status)
{
if (status == "OK"){loaded=true; window.location="applovin://load";}
else {loaded=true; window.location="applovin://failLoad";}
};
GAT.loadAd({
      divid : "div_max",
      inventoryid: "%%PLACEMENTID%%",
      adverid: "%%ADVERTISING_ID%%",
      dnt : "%%DNT%%",
      ipv4 : "%%IPADDRESS%%",
      latitude : "%%LATITUDE%%",
      longitude : "%%LONGITUDE%%",
      requestid : "%%REQUESTID%%"
}, GatCallbackAd);
</script>
</body>
</html>
```


## Interstitial

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
<div id="div_max_interstitial" style="width:100%;height: 100%;display: flex; justify-content: center;align-items : center;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function GatCallbackAd (status)
{
if (status == "OK"){loaded=true; window.location="applovin://load";}
else {loaded=true; window.location="applovin://failLoad";}
};
GAT.loadAd({
      divid : "div_max_interstitial",
      inventoryid: "%%PLACEMENTID%%",
      adverid: "%%ADVERTISING_ID%%",
      dnt : "%%DNT%%",
      ipv4 : "%%IPADDRESS%%",
      latitude : "%%LATITUDE%%",
      longitude : "%%LONGITUDE%%",
      requestid : "%%REQUESTID%%"
}, GatCallbackAd);
</script>
</body>
</html>
```


## MREC

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
<div id="div_max" style="width:100%;text-align:center;margin:0 auto;padding:0;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function GatCallbackAd (status)
{
if (status == "OK"){loaded=true; window.location="applovin://load";}
else {loaded=true; window.location="applovin://failLoad";}
};
GAT.loadAd({
      divid : "div_max",
      inventoryid : "%%PLACEMENTID%%",
      adverid : "%%ADVERTISING_ID_IFA%%",
      dnt : "%%DNT%%",
      ipv4 : "%%IPADDRESS%%",
      latitude : "%%LATITUDE%%",
      longitude : "%%LONGITUDE%%",
      requestid : "%%REQUESTID%%",
      useragent : "%%USERAGENT%%"
}, GatCallbackAd);
</script>
</body>
</html>
```

## Native

```html
Support Not yet.
```

## Rewarded

```html
Support Not yet.
```
