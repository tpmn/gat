# TPMN AdTag for app(GAT) English Version

*FYI : GAT is the shorthand for TPMN AdTag for app

## About?

This tag is for app publishers and developers. They can connect TPMN Exchange and impress our ad through this tag. <br>
You don't need to install addtionanl AD SDK using this AdTag(GAT). Once you set the condition easily, you can make participants deal ad freely.

**Ad Type We Support**
- Banner

**Ad Type We Do Not Support**
- Native
- Video

## Let's Start

### How To Use GAT

- You need to get GAT Inventory ID From us (See below for detail.) 
- Register the tag into AD SDK.
- Define a callBack function For JsTag.
- Do a test


### How to Get TPMN AdTag for app(GAT) Inventory ID

- You need be provided with inventory Id From us before you use GAT.
- You can contact TPMN Manager or send a mail to info@tpmn.io(CEO e-mail) to get inventory Id for each slot. Below is the format example.
> Name and Position : 
> 
> AppBundle Id : 
>
> Contact(Phone) :
> 
> Contents : 
> 
> Do you use Applobin SDK? (Y/N) :



### AdTag Detail

`<div>` : This tag define the area where ad is impressed.
- When you request ad with GAT, we ***recomand*** for you to ***set the above div tag Id same with the divid parameter value in script*** 
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
        console.log("Success");
        //TO-DO Custom Action..
    } else {
        console.log("Fail");
        //TO-DO Custom Action..
    }
};
GAT.loadAd({
      divid : "div_adInventory",
      inventoryid: "${TAG_INVENTORY_ID}",
      adverid: "${DEVICE_IDFA}"
}, tagCallbackFunction);
</script>
```


### AdTag Paramter detail

| parameter | type   | required | explanation          | Escaped |
|-----------|--------|----------|----------------------|---------|
| `divid`     | string | Yes      | Ad Slot Div Tag Id   |         |
| `ii`        | string | Yes      | GAT Inventory Id     |         |
| `adverid`   | string | Yes      | idfa                 |         |
| `dnt`       | string |          | Do Not Track         |         |
| `ipv4`      | string |          | IP Address           |         |
| `latitude`  | string |          | Latitude             |         |
| `longitude` | string |          | Longitude            |         |
| `requestid` | string |          | Request ID           |         |
| `useragent` | string |          | User Agent           | Yes     |


### About Response status CallbackFunction

- This function need to define addtional action for SDK to process according to response status
- The name of callbackFunction can be changed freely.

### Response CallbackFunction status parameter

| status  | type   | status    |
|---------|--------|-------|
| `OK`      | string | Normal  |
| `NOBID`   | string | No Bid  |
| `INVALID` | string | Bad Request |
| `ERROR`   | string | Error    |

**OK**

- It means you get response normally and the ad HTML tag is included in your ad slot succesfully.


**NOBID**

- There is no ad to impress.


**INVALID**

- It means bad request. Below are possible factors.
- Check if you missed required parameter.
- Check if the inventory Id is valid.


**ERROR**

- It means there was an error during the process.
- Check if the request format is right.


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
