
# Applovin MAX Integration

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

- Applovin MAX에 GAT tag를 통해 NON-RTB요청을 RTB 광고로 보내기위해서 몇가지 체크사항이 필요합니다.

**Applovin MAX**

- 참고 : [Custom JS Tag Network Integration Guide Ref.](https://dash.applovin.com/documentation/mediation/android/mediation-setup/jstag)
- Manage → Network → Custom Network 등록 → GAT 태그를 광고 유형별 등록
- Manage → Ad Units → 인벤토리 등록 or 수정 → Custom Network → GAT 네트워크 등록
- SDK가 설치된 APP 등록

**APP**

- Applovin Max SDK 연동이 되어있어야 합니다.
- 광고영역을 Applovin Max SDK로 연동하여 광고가 나오는지 확인 합니다.

**GAT JsTag**

- Applovin SDK 메크로 지원
- Callback을 통한 Applovin SDK 처리 지원

**연동 순서**

- App에 Applobin SDK 연동 완료
- GAT 인벤토리 ID 발급
- MAX Custom Netowork 등록
- MAX Ad Units에 Inventory 등록 or 추가
- JsTag의 CallBackFunction 정의
- 테스트 진행


### 인벤토리ID 발급

- GAT Tag를 사용하려면 Inventory ID 발급이 선행되어야 합니다.
- 광고지면의 Inventory ID 발급문의는 TPMN 매니저 또는 대표메일(info@tpmn.io)로 문의 주시기 바랍니다.
> 소속 및 이름 :
>
> AppBundle Id :
>
> 연락처(Phone) :
>
> 내용 :
>
> Applobin SDK 사용 여부 :



### JsTag

- `<div>` : 광고가 나가는 영역입니다. 해당 div객체 ID를 GAT는 인자로 받습니다.
- `<script>` : https://static.tpmn.io/gat/ads.js 파일을 통해 광고처리를 수행합니다.
- `GatCallbackFunction()` : 광고수신 정상유무를 처리하기위한 Callback 함수입니다.

````renderscript
<div id="div_adInventory" style="width:100%;text-align:center;margin:0 auto;padding:0;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function GatCallbackFunction (status)
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
      divid : "div_adInventory",
      inventoryid: "{발급받은 GAT InventoryId}",
      adverid: "%%ADVERTISING_ID%%",
      dnt : "%%DNT%%",
      ipv4 : "%%IPADDRESS%%",
      latitude : "%%LATITUDE%%",
      longitude : "%%LONGITUDE%%",
      requestid : "%%REQUESTID%%"
}, GatCallbackFunction);
</script>
````


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


### CallbackFunction 설명

- GAT를 통한 광고 여부에 따른 SDK에서 처리해야할 Action을 수행하면 됩니다.
- CallbackFunction 이름은 수정이 가능합니다.

### CallbackFunction status parameter

| status  | type   | 상태    |
|---------|--------|-------|
| OK      | string | 광고수신  |
| NOBID   | string | 광고없음  |
| INVALID | string | 잘못된요청 |
| ERROR   | string | 오류    |

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
- SDK에서 User-Agent를 Header 정보를 포함하지 않을 경우 useragent 파라미터에 기기의 User-Agent를 Escaped 처리하여 전달 하시기 바랍니다.


**ERROR**

- 요청을 처리 중 오류가 발생한 경우입니다.
- 지원하지 않는 형식인 경우 발생합니다.


----


### Response

- 광고가 있을 경우.

> JsGatCB3758575419({"result":"OK","scripts":"","htmp":"<광고응답HTML>","callback":"JsGatCB3758575419"})

- 광고가 없을 경우.
> JsGatCB3758575419({"result":"NOBID","scripts":"","callback":"JsGatCB3758575419"})


----

# Example

## Banner

```renderscript
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

```renderscript
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

```renderscript
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

```renderscript
Support Not yet.
```

## Rewarded

```renderscript
Support Not yet.
```
