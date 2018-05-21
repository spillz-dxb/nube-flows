```
[{"id":"73328d3c.1bb024","type":"function","z":"121f9671.73e58a","name":"","func":"var nDate = new Date().toLocaleString('en-UK', {\n    timeZone: 'Australia/Sydney'\n});\n\ntimeAu = (nDate);\n\nnode.status({fill:\"blue\",shape:\"dot\",text:\"Message recieved:  @\" +\" \"+ timeAu});\nreturn msg;","outputs":1,"noerr":0,"x":470,"y":1300,"wires":[["a7e1d68e.7c5d98","5cf33265.571dcc"]]}]

```
