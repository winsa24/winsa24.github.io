---
date: 2021-04-11
description: ""
tags: ["Lecture", "HCI"]
title: "Simple Server (ServerLab)"
---
# Lab: Server

## Question 1: 
Using only the http module, in a file called server.js, write a JavaScript program that implements a web server on port 8000. The server shall return an HTML string confirming that the server works when you access the root of the server, that is, “http://localhost:8000/”.   
 
```
var http = require("http");

http.createServer(function(req, res){
    if(req.url == "/"){
        res.writeHead(200,{
            "content-type":"text/html"
        });
        res.write("hello world123");
        res.end();
    }
}).listen(process.argv[2])
```
Type << node server.js 8000 >> in the console.  

## Question 2: 
Modify the previous server so that receiving the URL “http://localhost:8000/kill” will stop the server.   

```
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
```

## Question 3: 
Using the fs module, modify the web server so that it serves the files present in the server directory and all subdirectories. Use the same server.js file.  

```
var fs = require("fs");

...
    if(...){
        ...
    }
    else{
        var url = "." + req.url;
        // console.log(url)
    
        fs.readFile(url , function(err,data){
        /*
            一参为文件路径
            二参为回调函数
                回调函数的一参为读取错误返回的信息，返回空就没有错误
                二参为读取成功返回的文本内容
        */
            if(err){
                res.writeHeader(404,{
                    'content-type' : 'text/html;charset="utf-8"'
                });
                res.write('<h1>404</h1><p>file does not exist</p>');
                res.end();
            }else{
                
                if(url.includes("jpg")){
                    res.writeHeader(200,{
                        'Content-Type' : 'image/jpeg'
                    });
                    
                    // var imgUrl = "/" + url.split("/").pop();

                    // data = "<img src=" + imgrl + "/>";
                }else if(url.includes("css")){
                    res.writeHeader(200,{
                        'Content-Type' : 'text/css'
                    });
                }else if(url.includes("js")){
                    res.writeHeader(200,{
                        'Content-Type' : 'text/javascript'
                    });
                }else{
                    res.writeHeader(200,{
                        'content-type' : 'text/html'
                    });
                }
                res.write(data);
                res.end();    
            }
    
    });
    }

```

## Question 4: 
Modify the previous web server so that it processes GET requests for the address http://localhost:8000/hi?nom=xxxx where xxxx is a string of characters. Continue to use the server.js file. The server will respond with HTML code saying hi xxxx. It should be OK for xxxx to contain accents, spaces … You can use the unescape method of the module querystring.  

## Question 5: 
Modify the previous server to process requests to http://localhost:8000/salut?user=xxxx. The server will save in memory all received user values. The server will respond, at each new request, with an HTML response of the type:   "salut xxxx, the following users have already visited this page: yyyy, zzzz, yyyy, bbbb"  
Check the XSS vulnerability. Vulnerability is more serious in this case. Correct this vulnerability, especially for user=<b>Toto</b> and user=<script>alert('hello');</script>.

```
var qs = require("querystring");
let salut_arr = [];
...
    else if(req.url.includes("?")){
        // let url = new URL(req.url);
        // let name = url.searchParams[“name”]
        var url = qs.unescape(req.url);
        console.log("url unescape:"+ req.url);
        url = url.split("?");
        var getinfo = qs.parse(url[1]);
        var xxxx;
        if(getinfo.nom){
            xxxx = getinfo.nom;
            res.writeHead(200,{
                "content-type":"text/html"
            });
            res.write("hi " + xxxx);
            res.end();
        }else if(getinfo.user){
            //xxxx = xss(getinfo.user); //only work with <script>
            res.writeHead(200,{
                //"content-type":"text/plain" //grader says no
                "content-type":"text/html"
            });
            //xxxx = getinfo.user; //change write content-type to plain works
            xxxx =  getinfo.user;
            xxxx = xxxx.replace(/</g,"&lt;");
            xxxx = xxxx.replace(/>/g,"&gt;");
            salut_arr.push(xxxx);
            res.write("salut " + xxxx + ", the following users have already visited this page: " );
            for(var i = 0; i < salut_arr.length - 1; i++){
                res.write(" " + salut_arr[i] + ",");
            }            
            res.end();
        }

    }
```

> The code does work. But it can be more readable if I separate it with
```
    else if(req.url == "/hi?"){
        ...
    }
    else if(req.url == "/salut?"){
        ...
    }
```

## Question 6: 
Modify the previous server to process requests to http://localhost:8000/clear by clearing the memory of past salut requests. After a clear, a request to http://localhost:8000/salut?user=xxxx will produce the answer: "salut xxxx, the following users have already visited this page: "  

```
    else if(req.url == "/clear"){
        salut_arr = [];
        res.writeHead(200,{
            "content-type":"text/html"
        });
        res.write("Clear salut request.");
        res.end();
    }
    
```

