
# TPMN AdTag for app(GAT) - Applovin MAX Integration English Version

*FYI : GAT is the shorthand for TPMN AdTag for app

**Ad Type Applovin MAX support**

| TYPE         | SUPPORT(Y/N)     |
|--------------|----------|
| `Banner`       | Yes      |
| `Interstitial` | Yes      |
| `MREC`         | Yes      |
| `Native`       | Not yet  |
| `Rewarded`     | Not yet  |


## Let's start

**What We Are Trying To Do**

- We connect TPMN AdTag for app through Custom JS Tag Network method provided by Applovin Max.
- Below are check point and procedure for setting.


**How to use GAT through Custom JS Tag Network Method**

- You need to get GAT Inventory ID From us
- Register MAX Custom JS Tag Network (You must Define CallBack Function For Dealing With Events Related to Ad Request and Response)
- Add Custom Netowork Inventory into MAX Ad Units and Activate it.
- Do a Test


**TPMN AdTag**

- [x] We Support Applovin SDK Macro
- [x] We Support Applovin SDK Process through CallBack Function


**Applovin MAX**

- [x] Manage → Network → Register New Custom Network
- [x] Manage → Ad Units → Custom Network(TPMN_GAT Network) → Enter Setting values(PlacementID,CPM Price,Country Targeting) → Activate Status
- reference : [Applovin Max Setting Manual(Notion)](https://tpmnkorea.notion.site/Applovin-MAX-6c6e8097c92f41ab8a5d8bb4933354a6)
- reference : [Custom JS Tag Network Integration Guide Ref.](https://dash.applovin.com/documentation/mediation/android/mediation-setup/jstag)


**APP**

- [x] Check If Ad Is Impressed Well in the Area through Applovin Max SDK.


### Max - TPMN AdTag for app Template

`<div>` : This tag define the area where ad is impressed. 
- When you request ad with GAT, we ***recomand*** for you to ***set the above div tag Id same with the 'divid' parameter value in script*** 
- "div_" + inventory ID
<pre>
#For example, let the inventory ID is 1234 :
< div id="<b>div_1234</b>" sytle="..." />
GAT.loadAd({
    divid : "<b>div_1234</b>",
    inventoryid: "<b>1234</b>",
    ....
</pre>

`<script>` : https://static.tpmn.io/gat/ads.js This is the key file URL for the ad deal process.

`tagCallbackFunction()` : This is the callback function to deal with event after the process is finished and response is returned.(Response status parameter is explained at the bottom.)

```html
<div id="div_adInventory" style="width:100%;text-align:center;margin:0 auto;padding:0;"></div>
<script type="text/javascript" src="https://static.tpmn.io/gat/ads.js"></script> 
<script>
function tagCallbackFunction (status)
{
    if (status == "OK"){
        <!-- Do Not Delete This. We need this event code to finally impress the ad which is normally received from Max.-->
        loaded=true; window.location="applovin://load";
    }else {
        <!-- Do Not Delete This. We need this event code to take an appropriate action after receiving 'NOBID' response from Max.-->
        loaded=true; window.location="applovin://failLoad";
    }
};
GAT.loadAd({
      divid : "div_1234",
      inventoryid: "%%PLACEMENTID%%",
      adverid: "%%ADVERTISING_ID%%",
      dnt : "%%DNT%%",
      ipv4 : "%%IPADDRESS%%",
      latitude : "%%LATITUDE%%",
      longitude : "%%LONGITUDE%%",
      requestid : "%%REQUESTID%%"
}, tagCallbackFunction);
</script>
```


### JsTag Paramter detail

| parameter | type   | required | explanation          | Escaped | Macro               |
|-----------|--------|----------|----------------------|---------|---------------------|
| `divid`     | string | Yes      | Ad Slot Div Tag Id   |         |                     |
| `ii`        | string | Yes      | GAT Inventory Tag Id |         | %%PLACEMENTID%%     |
| `adverid`   | string | Yes      | idfa                 |         | %%ADVERTISING_ID%%  |
| `dnt`       | string |          | Do Not Track         |         | %%DNT%%             |
| `ipv4`      | string |          | IP Address           |         | %%IPADDRESS%%       |
| `latitude`  | string |          | Latitude             |         | %%LONGITUDE%%       |
| `longitude` | string |          | Longitude            |         | %%LATITUDE%%        |
| `requestid` | string |          | Request ID           |         | %%REQUESTID%%       |
| `useragent` | string |          | User Agent           | Yes     | %%USERAGENT%%       |


### About Response status Callback Function

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
function tagCallbackAd (status)
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
}, tagCallbackAd);
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
function tagCallbackAd (status)
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
}, tagCallbackAd);
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
function tagCallbackAd (status)
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
}, tagCallbackAd);
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
