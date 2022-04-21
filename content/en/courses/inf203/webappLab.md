---
date: 2021-04-11
description: ""
tags: ["Lecture", "HCI"]
title: "Web Application (webappLab)"
---
# Lab: Web Application

## Questions 1 to 5: 
> Client-side: create a button bar with “show txt”, “add element”, “remove element”, “clear” and ”restore”. The actions of these buttons are:

1. show txt: shows the text of the current JSON
2. add element: adds in the current JSON an element whose information is a number (value), a text (title) and a color in CSS format (color name or hex) (color). The new element is at the end of the list.
3. remove element: deletes an element in the running with his index finger
4. clear: deletes all elements in the current JSON, and replaces the whole storage.json content with [{"title": "empty", "color": "red", "value": 1}]
5. restore: restores a storage.json file with 3 slices, this last action is intended to simplify the automatic grading.
Put the buttons at the top of the space as a menu bar and the display space below. Each button clears the display space and shows what is necessary. For each button, there is something to do in the editing page and something to do in the server.

In order to allow auto-grading, please put ids on the buttons in the toolbar:

“SHOW” for the button to show the text of the current JSON below the menu bar,
“ADDBUTTON” for the button to show the form to add a new element to the JSON,
“REMOVE” for the button to show the form to remove an existing element from the JSON,
“CLEAR” for the button to clear the existing JSON,

>> html:
```
<div id="buttonbar">
            <button id="SHOW" onclick="show()" >show text</button>
            <button id="ADDBUTTON" onclick="add()">add element</button>
            <button id="REMOVE" onclick="remove()">remove element</button>
            <button id="CLEAR" onclick="clearf()">clear</button>
            <button id="RESTORE" onclick="restore()">restore</button>
</div>
<div id="MAINSHOW"></div>
```
>> js:
```
let mainshow = document.getElementById("MAINSHOW");

//Question1: “SHOW” for the button to show the text of the current JSON below the menu bar,
function show(){
    mainshow.innerHTML = ''; //clear
    //read json file
    var url = "../../show";
    readurl(url); //a function use AJAX to connect to the server
}

//Question2: “ADDBUTTON” for the button to show the form to add a new element to the JSON,
function add(){
    mainshow.innerHTML = ''; //clear
    let addform = document.getElementById("addform");
    addform.style.display = 'block';
}

//Question3: “REMOVE” for the button to show the form to remove an existing element from the JSON,
function remove(){
    mainshow.innerHTML = ''; //clear
    let delform = document.getElementById("delform");
    delform.style.display = 'block';
}

//Question4: “CLEAR” for the button to clear the existing JSON,
function clearf(){
    // console.log("enter clear");// can't set name as clear()
    mainshow.innerHTML = ''; //clear
    //read json file
    var url = "../../clear";
    readurl(url);
}

function restore(){
    mainshow.innerHTML = ''; //clear
    //read json file
    var url = "../../restore";
    readurl(url);
}
```
You also need textfields for the index to remove and the title, color and value to add, use “indexTF”, “titleTF”, “colorTF” and “valueTF” for them. The buttons to validate addition and removal shall have “VALIDADD” and “SENDREM” as ids. The element in which you insert the result of show shall have the id “MAINSHOW”.

>> html:
```
<div id="delform" style="display: none;">
    <input id="indexTF" placeholder="index" type="number" name="index" />
    <button id="VALIDADD" onclick="validdel()">valid delete</button>
</div>
        
<div id="addform" style="display: none;">
    <input id="titleTF" placeholder="title" type="text" name="title" />
    <input id="colorTF" placeholder="color" type="text" name="color" />
    <input id="valueTF" placeholder="value" type="number" name="value" />
    <button id="VALIDADD" onclick="validadd()">valid add</button>
</div>
```
>> js:
```
function validadd(){
    mainshow.innerHTML = ''; //clear

    var Ftitle = document.getElementById("titleTF").value;
    var Fcolor = document.getElementById("colorTF").value
    var Fvalue = document.getElementById("valueTF").value;
    console.log("Fcolor:" + Fcolor);
    console.log(typeof Fcolor);
    Fcolor = Fcolor.replace("#", "%23");
    console.log("Fcolor2:" + Fcolor);
    var url = "../../add?title=" + Ftitle + "&value=" + Fvalue + "&color=" + encodeURI(Fcolor); //encodeURI doesn't work
    console.log(url);
    readurl(url);
}

function validdel(){
    mainshow.innerHTML = ''; //clear

    var Findex = document.getElementById("indexTF").value;

    var url = "../../remove?index=" + Findex;
    readurl(url);
}
```

> Server: Make sure that the JSON on the server is changed. URLs querying the server will be:

1. http://localhost:8000/show, called by the button show txt
2. http://localhost:8000/add?title=*&value=**&color=*** where * is a string, ** a number and *** a CSS color, called by the button add element
3. http://localhost:8000/remove?index=* where * is a number, called by the button remove element
4. http://localhost:8000/clear, called by the button clear
5. http://localhost:8000/restore, called by the button restore
 
```
http.createServer(function(req, res){
    
    // console.log(req.url);
    if(req.url == "/"){
        res.writeHead(200,{
            "content-type":"text/html"
        });
        res.write("hello world123");
        res.end();
    }
    else if(req.url == "/kill"){
        res.writeHead(200,{
            "content-type":"text/html"
        });
        res.write("The server will stop now.");
        res.end();
        process.exit(0)
    }
    else if(req.url == "/show"){
        let json = fs.readFileSync("./client/storage.json", 'utf-8');
        // json  = JSON.parse(json);
        res.writeHeader(200,{
            'Content-Type' : 'application/json'
        });
        res.end(json);
        // return
    }
    else if(req.url == "/clear"){
        let tmp = [{"title": "empty", "color": "red", "value": 1}];
        // let cleardata = JSON.stringify(tmp);
        fs.writeFileSync("./client/storage.json", JSON.stringify(tmp));

        res.writeHeader(200,{
            'Content-Type' : 'application/json'
        });
        res.end(JSON.stringify(tmp));
    }
    else if(req.url == "/restore"){
        let tmp = [{"title": "foo", "color": "red", "value": 20}, {"title": "bar", "color": "ivory", "value": 50}, {"title": "test", "color": "yellow", "value": 30}];
        // let cleardata = JSON.stringify(tmp);
        fs.writeFileSync("./client/storage.json", JSON.stringify(tmp));

        res.writeHeader(200,{
            'Content-Type' : 'application/json'
        });
        res.end(JSON.stringify(tmp));
    }
    else if(req.url.includes("?")){
        //get json to modify
        let data = JSON.parse(fs.readFileSync("./client/storage.json", 'utf-8'));
        let arr = [];
        //get info from url
        console.log("req.url: "+ req.url);
        var url = qs.unescape(req.url);
        console.log("url unescape: "+ url);
        url = url.split("?");
        var getinfo = qs.parse(url[1]);
        console.log(getinfo);

        if(getinfo.index){    // delete
            for(var i = 0; i < data.length ; i++){
                if(i != getinfo.index){
                    let tmp = new Object();
                    tmp.title = data[i].title;
                    tmp.color = data[i].color;
                    tmp.value = data[i].value; 
                    arr.push(tmp);
                }               
            }
            fs.writeFileSync("./client/storage.json", JSON.stringify(arr));
        }else{ // add
            let newObj = new Object();
            newObj.title =  getinfo.title;
            newObj.color = getinfo.color;
            newObj.value =  parseInt(getinfo.value);        
          
            for(var i = 0; i < data.length ; i++){
                let tmp = new Object();
                tmp.title = data[i].title;
                tmp.color = data[i].color;
                tmp.value = data[i].value; 
                arr.push(tmp);
            }
            arr.push(newObj);
            fs.writeFileSync("./client/storage.json", JSON.stringify(arr));
        }

        let json = fs.readFileSync("./client/storage.json", 'utf-8');
        res.writeHeader(200,{
            'Content-Type' : 'application/json'
        });
        res.end(json);
    }
    ...
 
```

## Question 6: 
> Client-side: add the show piechart button in test2.html. In this exercise you will create the SVG on the server and then display it in an image in the HTML on the client. The media type of SVG is “image/svg+xml”. The SVG namespace is “http://www.w3.org/2000/svg”. To make pie wedges, use path objects, the fill attribute for the color, the d field for the plot, and in the plot use M to move, L to make a line, A to create an arc and Z to close the path.
> Per wedge, an SVG text displays the field “title”. The text appears on the colorful area of ​​the wedge.
```
function draw(){
    var t = [], c = [], v = [];
    var url = "storage.json"/*json文件url，本地的就写本地的位置，如果是服务器的就写服务器的路径*/
    var request = new XMLHttpRequest();
    request.open("GET", url);/*设置请求方法与路径*/
    request.send(null);/*不发送数据到服务器*/
    request.onload = function () {/*XHR对象获取到返回信息后执行*/
        if (request.status == 200) {/*返回状态为200，即为数据获取成功*/
            var json = JSON.parse(request.responseText);
            for(var i = 0; i < json.length; i ++){
                v.push(json[i].value);
                t.push(json[i].title);
                c.push(json[i].color);
            }
        }
        console.log("v:" + v);
        console.log("t:" + t);
        console.log("c:" + c);
        // let data = [140, 100, 110, 90, 170,125];//要画出扇形的数据
        // drawPie(v);
        drawPie(v, t, c);
    }
    
}
function drawPie(data, text, color) {
    let oSvg = document.getElementById('s1');
    oSvg.style.display = 'block';
    while (oSvg.lastChild) {
        oSvg.removeChild(oSvg.lastChild);
    }//clear svg

    let cx = 400, cy = 300, r = 200,  sum = 0;//初始圆心半径
    data.forEach(item => {//求数据总和
        sum += item; 
    })
    let now = 0;
    let i = 0;
    data.forEach(item => {//画圆
        let ang = 360 * item / sum;
        if(ang == 360){
            let oPath = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
            oPath.setAttribute('cx', cx);
            oPath.setAttribute('cy', cy);
            oPath.setAttribute('r', r);
            oPath.setAttribute('fill', color[i]);
            oSvg.appendChild(oPath);
            var oText = document.createElementNS('http://www.w3.org/2000/svg', 'text');
            oText.innerHTML = text[i];
            oText.setAttribute('font-size', 30);
            oText.setAttribute('fill', invertColor(color[i]));
            oText.setAttribute('x', cx);
            oText.setAttribute('y', cy);
            oSvg.appendChild(oText);
        }else{
            pie(now, now + ang, text[i], color[i]);
        }
        now += ang;
        i ++;
    })
    function pie(ang1, ang2, text, color) {
        let oPath = document.createElementNS('http://www.w3.org/2000/svg', 'path');
        // oPath.setAttribute('stroke', 'white');
        //oPath.setAttribute('fill', `rgb(${Math.floor(Math.random() * 256)},${Math.floor(Math.random() * 256)},${Math.floor(Math.random() * 256)})`);
        oPath.setAttribute('fill', color);
        // oPath.setAttribute('stroke-width', 2);
            function d2a(ang) {//角度转弧度
                return ang * Math.PI / 180;
            }
            function point(ang) {//根据角度求坐标
                return {
                    x: cx + r * Math.sin(d2a(ang)),
                    y: cy - r * Math.cos(d2a(ang))
                }
            }
            //画扇形的三个步骤                   
            let arr = [];
            //第一步
            let { x: x1, y: y1 } = point(ang1);
            arr.push(`M ${cx} ${cy} L ${x1} ${y1}`);//画第一条线
            //第二步
            let { x: x2, y: y2 } = point(ang2);
            arr.push(`A ${r} ${r} 0 ${ang2 - ang1 > 180 ? 1 : 0} 1 ${x2} ${y2}`);//画弧
            //第三步
            arr.push('Z');//闭合
    
            oPath.setAttribute('d', arr.join(' '));//拼接字符串，执行绘画命令
            oSvg.appendChild(oPath);//添加到svg中
    
            //Add text
            let { x: textx, y: texty } = point(ang1 + (ang2 - ang1)/2);
            textx = (textx - cx) * 0.7 + cx;
            texty = (texty - cy) * 0.7 + cy;
            // var textx = ang2 - ang1 > 180 ? cx - Math.abs(x1 - x2)/2 : cx + Math.abs(x1 - x2)/2;
            // var texty = ang2 - ang1 > 180 ? cy - Math.abs(y1 - y2)/2 : Math.abs(y1 - y2)/2;
            var oText = document.createElementNS('http://www.w3.org/2000/svg', 'text');
            oText.innerHTML = text;
            oText.setAttribute('font-size', 30);
            oText.setAttribute('fill', invertColor(color));
            oText.setAttribute('x', textx);
            oText.setAttribute('y', texty);
    
            oSvg.appendChild(oText);
    }
}

```


> Find a way to adjust the color of the text so that the text is always visible, regardless of the color choice of the wedges.  Main idea:  
```
function invertChar(c) {
    switch (c.toLowerCase()) {
      case '0': return 'f';
      case '1': return 'e';
      case '2': return 'd';
      case '3': return 'c';
      case '4': return 'b';
      case '5': return 'a';
      case '6': return '9';
      case '7': return '8';
      case '8': return '7';
      case '9': return '6';
      case 'a': return '5';
      case 'b': return '4';
      case 'c': return '3';
      case 'd': return '2';
      case 'e': return '1';
      case 'f': return '0';
      case "#": return '#';
    }
}
```

## Question 7: 
In this question, you will see one of the choices of any web developer: do the work on the server as you have done in question 2b or do the work in the browser. Add a show local piechart button that will use the relative URL ../../show to get the JSON, then create the SVG directly into the local browser and then insert it into the HTML to display it.
>> js
```
function piech(){  
    var url = "../../piech";
    // self.location.href= url;
    let oSvg = document.getElementById('s1');
    oSvg.parentNode.removeChild(oSvg);

    var request = new XMLHttpRequest();
    request.open("GET", url);/*设置请求方法与路径*/
    request.send(null);/*不发送数据到服务器*/
    request.onload = function () {/*XHR对象获取到返回信息后执行*/
        if (request.status == 200) {/*返回状态为200，即为数据获取成功*/            
            let tmp = new DOMParser().parseFromString(request.responseText,'text/html').body.childNodes[0];
            document.body.appendChild(tmp);
        }
    }
}
```
>> server
```
function drawPie(data, text, color) {
    let htmlstr = "<svg xmlns='http://www.w3.org/2000/svg' id='s1' width=800 height=600>";
    let cx = 400, cy = 300, r = 200,  sum = 0;//初始圆心半径
    data.forEach(item => {//求数据总和
        sum += item; 
    })
    let now = 0;
    let i = 0;
    data.forEach(item => {//画圆
        let ang = 360 * item / sum;
        if(ang == 360){
            htmlstr += "<circle cx='" + cx + "' cy='" + cy + "' r = '" + r + "' fill = '" + color[i] + "'/>";
            htmlstr += "<text x='" + cx + "' y='" + cy + "' fill='" + invertColor(color[i]) + "' font-size='30'>" + text[i] + "</text>";
        }else{
            htmlstr += pie(now, now + ang, text[i], color[i]);
        }
        now += ang;
        i ++;
    })
    function pie(ang1, ang2, text, color) {
        let oPath = "";
        let oText = "";
            function d2a(ang) {//角度转弧度
                return ang * Math.PI / 180;
            }
            function point(ang) {//根据角度求坐标
                return {
                    x: cx + r * Math.sin(d2a(ang)),
                    y: cy - r * Math.cos(d2a(ang))
                }
            }
            let { x: x1, y: y1 } = point(ang1);
            let { x: x2, y: y2 } = point(ang2);
            let path = `M ${cx} ${cy} L ${x1} ${y1} A ${r} ${r} 0 ${ang2 - ang1 > 180 ? 1 : 0} 1 ${x2} ${y2} Z`

            oPath = "<path d='" + path + "' fill='" + color + "'></path>";

            //Add text
            let { x: textx, y: texty } = point(ang1 + (ang2 - ang1)/2);
            textx = (textx - cx) * 0.7 + cx;
            texty = (texty - cy) * 0.7 + cy;
            oText = "<text x='" + textx + "' y='" + texty + "' fill='" + invertColor(color) + "' font-size='30'>" + text + "</text>";            
        
        return oPath+oText;
    }
    htmlstr += "</svg>"
    return htmlstr;
}

```
```
...
    else if(req.url == "/piech"){
        //get data 
        let json = fs.readFileSync("./client/storage.json", 'utf-8');
        json  = JSON.parse(json);
        let t = [], c = [], v =[];
        for(var i = 0; i < json.length ; i++){
            t.push(json[i].title);
            c.push(json[i].color);
            v.push(json[i].value);            
        }
        let htmlstr = "";
        htmlstr = drawPie(v, t, c);
        
        res.writeHeader(200,{
            'content-type' : 'image/svg+xml'
        });
        res.write(htmlstr);
        res.end();
    }
...

```
