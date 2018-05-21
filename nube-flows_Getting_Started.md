
## Node-red

## Getting Started

Website 
https://nodered.org/

Forum
https://groups.google.com/forum/#!forum/node-red


### Default Port for node-red
The default port is 1880, If you're using the USB host the open chrome and go to 192.168.7.2:1880

### Basics in node-red



#### Create a flow


#### View Debug Messages


#### Deploy a flow


#### Export and Import a flow


#### Install a new node-red node

https://flows.nodered.org/


## Javascript Functions

### Boolean Functions
AND, OR, NOT

Logical Operators

#### Logical operators are used to determine the logic between variables or values.

Given that x = 6 and y = 3, the table below explains the logical operators:

##### AND
```
(x < 10 && y > 1) is true
```
##### OR
```
||
```
```
(x == 5 || y == 5) is false
```
##### NOT
```
!
```
```
!(x == y) is true
```

### Comperserson 

https://www.w3schools.com/js/js_comparisons.asp


#### equal to

x == 8


#### greater than
x > 8


#### less than
x < 8


#### greater than or equal to

x >= 8

#### less than or equal to

x <= 8

## Functions

```

function myFunction(p1, p2) {
    return p1 * p2;
}
msg.payload = myFunction(4, 4);

return msg;

```



#### Time functions
```

// this is a on delay

if (msg.payload === true){
    setTimeout(function(){ 
        node.send(msg);
    }, 3000);
} else {
    msg.payload = false;
    node.send(msg);
}



// Off delay

if (msg.payload === false){
    setTimeout(function(){ 
        node.send(msg);
    }, 3000);
} else {
    msg.payload = true;
    node.send(msg);
}

```


### Arithmetic Operators

Operator
Description

Addition +

Subtraction -

Multiplication *

Division 

Modulus %

Increment ++

Decrement --

### Math Functions
https://www.w3schools.com/js/js_math.asp

#### Math.random()

Example  
1 is the start number
6 is the number of possible results (1 + start (6) - end (1))

msg.payload = (Math.floor(Math.random() * 6) + 1  ); //(1 + start (6) - end (1))
return msg;




#### Math.round()

```

Math.round(x) returns the value of x rounded to its nearest integer:
Math.round(4.7);    // returns 5
Math.round(4.4);    // returns 4

```

## Conversions

### Converting Strings to Numbers 
```
Method
Description
parseFloat()
Parses a string and returns a floating point number
parseInt()
Parses a string and returns an integer
```


## Node Red Functions

### Basic function without context

```
function GT(a, b, m) {
    if (a > b) {
        m.payload = true;
    } else {
        m.payload = false;
    }
    return m;
}
msg1 = GT(aa, bb, msg1);
return [msg1];

```

### Basic function with storeding context

### With mulitable outputs

```
msg1={};
msg2={};
if (msg.payload=="Lamp,1"){
  msg1.payload = 1;
  msg2.payload = 0;
  return [msg1,msg2]}
else if (msg.payload=="Lamp,0"){
  msg1.payload = 0;
  msg2.payload = 1; 
  return [msg1,msg2]}
```



### Store msg history in context

```

//This will store the topics and payloads
topic = msg.topic;
curVal = context.get('curVal') || {
    "a": { topic: "a", default: 0}, // set default values.
    "b": { topic: "b", default: 0},
    "c": { topic: "c", default: 0},
    "d": { topic: "d", default: 0},
};

if(!(topic in curVal)) {
    node.error(topic + " has no defined mapping!");
    return null;
}
curVal[topic].default = msg.payload;

// set topics names
var aa = curVal.a.default;
var bb = curVal.b.default;
var cc = curVal.c.default;


msg1={};
msg2={};


function add(a, b, m) {
    if (a > b) {
        m.payload = true;
    } else {
        m.payload = false;
    }
    return m;
}

function myFunc2(m) {
    ret = m + 5;
    return ret;
}

msg1 = add(aa, bb, msg1);
msg2.payload = myFunc2(aa);

context.set('curVal', curVal);

return [[msg1],[msg2]];

Store a timestamp, msg.payload and topic in context for a small history

// initialize the text to an empty array if it doesn't exist already in context

if(msg.topic == '_returnval') {
    msg.payload = context.get('text')|| [];
    return msg;
} else if(msg.topic == '_clearval') {
    context.set('text',undefined);
} else {
    msg.payload = {x: new Date(), y: msg.payload};
    var text = context.get('text')|| [];
    text.push(msg.payload);
    if (text.length > 5){
       text.shift();
       text.length = 5;
    }

    msg.payload = text;
    context.set('text',text);
}

return null;

```

```


[{"id":"4140d11e.46913","type":"debug","z":"f52515c2.592508","name":"","active":true,"console":"false","complete":"false","x":693.017333984375,"y":1653.34375,"wires":[]},{"id":"cf516ed0.d559a","type":"inject","z":"f52515c2.592508","name":"Test Data","topic":"","payload":"22","payloadType":"num","repeat":"","crontab":"","once":false,"x":176.017333984375,"y":1613.3437232971191,"wires":[["76ed390f.d370c8"]]},{"id":"870b1ee4.358ad","type":"inject","z":"f52515c2.592508","name":"Return Stored Data","topic":"_returnval","payload":"","payloadType":"str","repeat":"","crontab":"","once":false,"x":193.017333984375,"y":1753.34375,"wires":[["76ed390f.d370c8"]]},{"id":"78f5747d.a34c2c","type":"inject","z":"f52515c2.592508","name":"Clear Stored Data","topic":"_clearval","payload":"","payloadType":"str","repeat":"","crontab":"","once":false,"x":194.017333984375,"y":1833.343729019165,"wires":[["76ed390f.d370c8"]]},{"id":"76ed390f.d370c8","type":"function","z":"f52515c2.592508","name":"","func":"// initialize the text to an empty array if it doesn't exist already in context\n\nif(msg.topic == '_returnval') {\n    msg.payload = context.get('text')|| [];\n    return msg;\n} else if(msg.topic == '_clearval') {\n    context.set('text',undefined);\n} else {\n    msg.payload = {x: new Date(), y: msg.payload};\n    var text = context.get('text')|| [];\n    text.push(msg.payload);\n    if (text.length > 5){\n       text.shift();\n       text.length = 5;\n    }\n\n    msg.payload = text;\n    context.set('text',text);\n}\n\nreturn null;\n// make it part of the outgoing msg object\n//msg = {};\n//msg.payload = text;\n\n//node.status({fill:\"green\",shape:\"ring\",text:text});\n\n//return msg;","outputs":1,"noerr":0,"x":533.017333984375,"y":1653.34375,"wires":[["4140d11e.46913"]]},{"id":"18a10012.e134e","type":"inject","z":"f52515c2.592508","name":"Test Data","topic":"","payload":"224","payloadType":"num","repeat":"","crontab":"","once":false,"x":167.01734924316406,"y":1651.34375,"wires":[["76ed390f.d370c8"]]}]


```


### Function in a function node

```

[{"id":"e831f9a6.eb6ef8","type":"inject","z":"83aa3763.91baf8","name":"","topic":"a","payload":"22","payloadType":"num","repeat":"","crontab":"","once":true,"x":133,"y":824,"wires":[["4fe8888e.c1bc38"]]},{"id":"7177c02d.b5356","type":"inject","z":"83aa3763.91baf8","name":"","topic":"b","payload":"2","payloadType":"num","repeat":"","crontab":"","once":true,"x":122,"y":941,"wires":[["4fe8888e.c1bc38"]]},{"id":"8b5f4003.7a7a4","type":"inject","z":"83aa3763.91baf8","name":"","topic":"b","payload":"1100","payloadType":"num","repeat":"","crontab":"","once":true,"x":131,"y":986,"wires":[["4fe8888e.c1bc38"]]},{"id":"f5ef8c22.aef65","type":"inject","z":"83aa3763.91baf8","name":"","topic":"c","payload":"5","payloadType":"num","repeat":"","crontab":"","once":true,"x":106,"y":1074,"wires":[["4fe8888e.c1bc38"]]},{"id":"443ea591.09583c","type":"inject","z":"83aa3763.91baf8","name":"","topic":"c","payload":"10","payloadType":"num","repeat":"","crontab":"","once":true,"x":103,"y":1122,"wires":[["4fe8888e.c1bc38"]]},{"id":"20824c1e.49f634","type":"inject","z":"83aa3763.91baf8","name":"","topic":"a","payload":"2","payloadType":"num","repeat":"","crontab":"","once":true,"x":123,"y":874,"wires":[["4fe8888e.c1bc38"]]},{"id":"4fe8888e.c1bc38","type":"function","z":"83aa3763.91baf8","name":"","func":"context.data = context.data || {} ;\nswitch (msg.topic) {\ncase \"a\" :\ncontext.data.a = msg.payload;\nmsg = null ;\nbreak;\ncase \"b\" :\ncontext.data.b = msg.payload;\nmsg = null ;\nbreak;\ncase \"c\" :\ncontext.data.c = msg.payload;\nmsg = null ;\nbreak;\ndefault:\nmsg = null ;\nbreak;\n}\n\nvar aa = context.data.a;\nvar bb = context.data.b;\nvar cc = context.data.c;\n\n\n\n//function example 1\n   \nfunction myFunc1(aa, bb) {\n  return aa * bb;\n}\n\n//---------------------\n\n//function example 2\n\n\nfunction myFunc2(cc) {\n  return cc;\n}\n\n// //---------------------\n\n//function example 3\n\nvar out = out;\n//out = myFunc3;\n\nfunction myFunc3(aa, bb) {\n   return (aa > bb);\n}\n    \n//---------------------\n\n\nvar out1 =  myFunc1(aa, bb) + myFunc2(cc);\nvar out2 =  myFunc3(aa, bb);\n\n\nmsg= {\n    \n    Out1:out1,\n    Out2:out2\n};\nreturn msg;\n\n","outputs":1,"noerr":0,"x":523,"y":948,"wires":[["e18b0247.21b96"]]},{"id":"e18b0247.21b96","type":"debug","z":"83aa3763.91baf8","name":"","active":true,"console":"false","complete":"true","x":806,"y":947,"wires":[]}]

```

3.6 Array 

```

var MyArray = ["key1","value1","key2","value2","key3","value3"];
msg1 = {topic:MyArray[0],payload:MyArray[1]};
msg2 = {topic:MyArray[2],payload:MyArray[3]};
msg3 = {topic:MyArray[4],payload:MyArray[5]};
return [msg1,msg2,msg3];

```




## BMS applications Examples

4.1 Staging of Compressors
Inputs: Setpoint, Zone Temp, Htg Offset, Clg Offset, Number of Stages, Interstage Delay.
Clg Mode: When Zone Temp > (Setpoint + Clg Offset) start Clg Mode, start Clg Stage 1.  If Zone Temp > (Setpoint + Clg Offset) for Interstage Delay then start next stage.  If Cooling Mode and Zone Temp < (Zone Temp + ((Clg Offset / Num of Stages) * (Current Stage - 1))) then stop Current Stage.  If Zone Temp < (Zone Temp - Htg Offset) stop Clg Mode, start Htg Mode. 
Htg Mode: When Zone Temp < (Setpoint - Htg Offset) start Htg Mode, start Htg Stage 1.  If Zone Temp < (Setpoint - Htg Offset) for Interstage Delay then start next stage.  If Htg Mode and Zone Temp > (Zone Temp - ((Htg Offset / Num of Stages) * (Current Stage - 1))) then stop Current Stage.  If Zone Temp > (Zone Temp + Clg Offset) stop Htg Mode, start Clg Mode. 


### PID Loop

#### Porp Band
#### Direct acting
When porb only is used with direct acting the output will modulate between 0% and 100% between SP and the SP+the PB


#### Reversing acting
When porb only is used with revers acting the output will modulate between 0% and 100% between SP and the SP-the PB

#### PI control
When PI control is used the output will be the the combination of the porp +int reset value. The reset value will change based up/down on the reset time and how far +/- from setpoint

PI control with DB
If the DB is used no integral action will occur when the measure value is in the db range +/- and the pi loop will hold the current value

#### Loop disable
If the loop is disabled
The int action will be reset and to output will be 0

### On/Off Delay 

```

// On delay
msg.payload = msg.payload;

var timeoutFunc = context.get('timeoutFunc') || null;
var turningOn = context.get('turningOn') || false;
var turningOff = context.get('turningOff') || false;
var isOn = context.get('isOn') || false;

if(msg.payload === true) {
   if(turningOff) {
       clearTimeout(timeoutFunc);
       turningOff = false;
       isOn = true;
       context.set('turningOff', turningOff);
       context.set('isOn', isOn);
       node.status({fill:"green",shape:"dot",text:msg.payload});
       node.send(msg);
   } else if(!turningOn && !isOn) {
       turningOn = true;
       context.set('turningOn', turningOn);
       node.status({fill:"yellow",shape:"dot",text:"Busy"});
       timeoutFunc = setTimeout(function(){
           isOn = true;
           turningOn = false;
           context.set('isOn', isOn);
           context.set('turningOn', turningOn);
           node.status({fill:"green",shape:"dot",text:msg.payload});
           node.send(msg);
       }, 2000);
 // Set On delay in ms      
       context.set('timeoutFunc', timeoutFunc);
   } else if(turningOn && !isOn) {
       msg.payload = false;
       node.send(msg);
   } else {
       msg.payload = true;
       node.send(msg);
   }
   
// Off delay
   
} else if(msg.payload === false) {
   if(turningOn) {
       clearTimeout(timeoutFunc);
       turningOn = false;
       isOn = false;
       context.set('turningOn', turningOn);
       context.set('isOn', isOn);
       node.status({fill:"red",shape:"dot",text:msg.payload});
       node.send(msg);
   } else if(!turningOff && isOn) {
       turningOff = true;
       context.set('turningOff', turningOff);
       node.status({fill:"yellow",shape:"dot",text:"Busy"});
       timeoutFunc = setTimeout(function(){
           isOn = false;
           turningOff = false;
           context.set('isOn', isOn);
           context.set('turningOff', turningOff);
           node.status({fill:"red",shape:"dot",text:msg.payload});
           node.send(msg);
       }, 2000);
 // Set Off delay in ms         
       context.set('timeoutFunc', timeoutFunc);
   } else if(!turningOff && isOn) {
       msg.payload = false;
       node.send(msg);
   } else {
       msg.payload = false;
       node.send(msg);
   }
}


```

## Energy and Water

### Pulse water meter

```

[{"id":"3e242fe5.0540e","type":"inject","z":"29ae3e4d.0373f2","name":"","topic":"","payload":"true","payloadType":"bool","repeat":"1","crontab":"","once":false,"x":123,"y":395,"wires":[["7a022bc9.a1ffa4"]]},{"id":"f437e6ce.bfe2e8","type":"function","z":"29ae3e4d.0373f2","name":"Pulse counter to accumulated val","func":"/*\n    Accepts pulses from meters and maintains an accumulated value\n    using.\n*/\n\nif(msg.val===true) {\n    value = msg.payload!==undefined && !isNaN(msg.payload)? parseFloat(msg.payload) : 0;\n    value += msg.scale;\n    msg.payload = value;\n    \n    node.status({fill:\"green\",shape:\"ring\",text:\"Cur: \" + value});\n    \n    return msg;\n}\n\nreturn null;","outputs":1,"noerr":0,"x":562,"y":315,"wires":[["bec8da10.965988","89423c86.08eaa"]]},{"id":"bec8da10.965988","type":"debug","z":"29ae3e4d.0373f2","name":"","active":false,"console":"false","complete":"true","x":868,"y":425,"wires":[]},{"id":"26370b3f.7c9214","type":"inject","z":"29ae3e4d.0373f2","name":"","topic":"","payload":"false","payloadType":"bool","repeat":"1","crontab":"","once":false,"x":121,"y":466,"wires":[["7a022bc9.a1ffa4"]]},{"id":"97bb3c5c.0d62c","type":"file in","z":"29ae3e4d.0373f2","name":"","filename":"water1.txt","format":"utf8","sendError":true,"x":540,"y":445,"wires":[["f437e6ce.bfe2e8"]]},{"id":"7a022bc9.a1ffa4","type":"change","z":"29ae3e4d.0373f2","name":"set value file","rules":[{"t":"set","p":"filename","pt":"msg","to":"_val_water_volume.txt","tot":"str"},{"t":"set","p":"val","pt":"msg","to":"payload","tot":"msg"},{"t":"set","p":"scale","pt":"msg","to":"0.5","tot":"num"}],"action":"","property":"","from":"","to":"","reg":false,"x":354,"y":418,"wires":[["97bb3c5c.0d62c"]]},{"id":"89423c86.08eaa","type":"file","z":"29ae3e4d.0373f2","name":"","filename":"","appendNewline":false,"createDir":false,"overwriteFile":"true","x":868,"y":485,"wires":[]},{"id":"314f691d.9581d6","type":"comment","z":"29ae3e4d.0373f2","name":"README","info":"* Use change node to set **filename** of file use to store the most recent value\n* Use change node to set the **scale** to increment the accumuated value","x":344,"y":458,"wires":[]}]

```

## Use Node red create a web API
### Example use

#### The flow

```

[{"id":"fb7a05cc.a83b68","type":"http in","z":"194736c8.a2c219","name":"","url":"/api","method":"get","upload":false,"swaggerDoc":"","x":340,"y":1340,"wires":[["955a9dd9.bf83e"]]},{"id":"ae5168b.8beb498","type":"http response","z":"194736c8.a2c219","name":"","statusCode":"","headers":{},"x":830,"y":1240,"wires":[],"inputLabels":["in"]},{"id":"f64cdadc.f92dc8","type":"debug","z":"194736c8.a2c219","name":"","active":false,"console":"false","complete":"true","x":690,"y":1140,"wires":[]},{"id":"8753d1f7.144cc","type":"function","z":"194736c8.a2c219","name":"","func":"key = 'api';\n\nif(msg.topic == '_clear') {\n    context.flow.set(key,undefined);\n} else {\n    topics = context.flow.get(key) || {};\n    topics[msg.topic] = msg.payload;\n    context.flow.set(key,topics);\n}\n\nreturn msg;","outputs":1,"noerr":0,"x":430,"y":1140,"wires":[["f64cdadc.f92dc8","955a9dd9.bf83e"]]},{"id":"ba67ce26.9e60d","type":"debug","z":"194736c8.a2c219","name":"","active":false,"console":"false","complete":"true","x":635.017333984375,"y":1414.7882080078125,"wires":[]},{"id":"955a9dd9.bf83e","type":"change","z":"194736c8.a2c219","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"api","tot":"flow"}],"action":"","property":"","from":"","to":"","reg":false,"x":450,"y":1240,"wires":[["ba67ce26.9e60d","b5ce40d2.117e5"]]},{"id":"b5ce40d2.117e5","type":"function","z":"194736c8.a2c219","name":"","func":"if('req' in msg)\n    return msg;\n\nreturn null;","outputs":1,"noerr":0,"x":670,"y":1240,"wires":[["ae5168b.8beb498"]]},{"id":"5c81227f.31c72c","type":"inject","z":"194736c8.a2c219","name":"Clear Old Data","topic":"_clear","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":280,"y":1140,"wires":[["8753d1f7.144cc"]]},{"id":"f151c8f1.e99278","type":"inject","z":"194736c8.a2c219","name":"Temp 1","topic":"Temp 1","payload":"22","payloadType":"num","repeat":"","crontab":"","once":false,"x":173.01734924316406,"y":999.8992919921875,"wires":[["8753d1f7.144cc"]]},{"id":"ff8710aa.f684e","type":"inject","z":"194736c8.a2c219","name":"Temp 2","topic":"Temp 2","payload":"2222","payloadType":"num","repeat":"","crontab":"","once":false,"x":170,"y":1040,"wires":[["8753d1f7.144cc"]]}]

```

# Email

```

[{"id":"d4b1e322.8e35d","type":"function","z":"b1fbb48a.f20568","name":"Flow 1 : Setup mail content","func":"msg.topic = \"Result flow 1\";\n\nmsg.payload = \"Dear,<br><br>Your PIR detector has detected movement in the living room.<br>In attachment you can find a camera snapshot.<br><br>Kind regards,<br>Your Node-Red flow\";\n\nmsg.attachments = [{\n    filename: 'camera_image.jpg',\n    path: '/tmp/local_image.jpg',\n    content: msg.payload\n}];\n        \nreturn msg;","outputs":1,"noerr":0,"x":571.017333984375,"y":1149.7882080078125,"wires":[["5d2563e7.7237cc","3966686e.9f9908"]]},{"id":"b0db1b7d.321258","type":"inject","z":"b1fbb48a.f20568","name":"Start","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":160.77513313293457,"y":1151.1904258728027,"wires":[["ba563e7b.c2ed3"]]},{"id":"3966686e.9f9908","type":"e-mail","z":"b1fbb48a.f20568","server":"smtp.gmail.com","port":"465","secure":true,"name":"aidan@enviroservices.com.au","dname":"Send mail","x":828.7752227783203,"y":1168.1904516220093,"wires":[]},{"id":"ba563e7b.c2ed3","type":"http request","z":"b1fbb48a.f20568","name":"Get image","method":"GET","ret":"bin","url":"https://storage.googleapis.com/relevant-magazine/2017/06/burglar-alamy.jpg","tls":"","x":317.0212173461914,"y":1150.4794883728027,"wires":[["b290bb2d.d9d688","d4b1e322.8e35d","57d93b3b.7b8ae4","1a95b316.51603d","fd19eec6.32d28"]]},{"id":"1a95b316.51603d","type":"file","z":"b1fbb48a.f20568","name":"Save image file","filename":"/tmp/local_image.jpg","appendNewline":false,"createDir":false,"overwriteFile":"true","x":515.0133895874023,"y":1095.2258052825928,"wires":[]},{"id":"5d2563e7.7237cc","type":"debug","z":"b1fbb48a.f20568","name":"","active":true,"console":"false","complete":"true","x":833.017333984375,"y":1117.8992919921875,"wires":[]}]

```

# Jsonata

Test data


```

{
    "orderItems": [
        {
            "conditionId": "New",
            "promotionDiscount": {
                "amount": 1.99,
                "currencyCode": "USD"
            },
            "giftWrapPrice": {
                "amount": 0.1,
                "currencyCode": "USD"
            },
            "shippingPrice": {
                "amount": 4.49,
                "currencyCode": "USD"
            },
            "itemPrice": {
                "amount": 25.44,
                "currencyCode": "USD"
            },
            "quantityShipped": 2,
            "title": "my life in kenya"
        }
    ]
}

```

```

Math
msg.payload{
   "shipment title" : orderItems.title,
   "shipment price" : orderItems.(
       quantityShipped*(
           itemPrice.amount + shippingPrice.amount
       ) - promotionDiscount.amount
   )

}


```

```

Reverse an array 
$reverse(
   [
       orderItems.promotionDiscount.amount,
       orderItems.promotionDiscount.currencyCode
   ]
)

```


Simple examples

$round((msg.payload), 2)

