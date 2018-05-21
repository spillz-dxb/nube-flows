# Nube-flows
Nube-iO Node-red Flows


## HVAC Control Logic

### [2 Stage Compressor Control ](https://github.com/NubeDev/nude-flows/blob/master/HVAC-2StageCompControl.md)

This is a flow for controlling compressor staging


### [Min and Max on/off time delay flow ](https://github.com/NubeDev/nude-flows/blob/master/MinMax%20Time%20Delay.md)

This is a flow for controlling compressor staging




### [Time Schedule Flow ](https://github.com/NubeDev/nube-flows/blob/master/timeScheduleFlow.md)

You need to install moment.js and enable in the settings.js file



### [Convert a timezone in node-red ](https://github.com/NubeDev/nube-flows/blob/master/convertTimeZoneFlow.md)

```
var nDate = new Date().toLocaleString('en-UK', {
    timeZone: 'Australia/Sydney'
});

timeAu = (nDate);

node.status({fill:"blue",shape:"dot",text:"Message recieved:  @" +" "+ timeAu});
return msg;

```




