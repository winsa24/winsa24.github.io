---
date: 2021-04-11
description: ""
tags: ["Lecture", "HCI"]
title: "JavaScript AJAX (js2Lab)"
---
# Lab: Javascript in the browser

## Exercise 1 - Get a file with AJAX and add its contents in the current HTML page 
> Question 1a: Write a function named loadDoc that loads the file text.txt and includes it in the page when you click a button.   
```
function loadDoc(){
    var texta = document.getElementById('texta');
    var xmlHttp = new XMLHttpRequest(); 

    if(xmlHttp != null){
        xmlHttp.open("get","text.txt", true);// 3 parameters (method, url, async)
        xmlHttp.send();
        xmlHttp.onreadystatechange = function () {
            if (xmlHttp.readyState===4 && xmlHttp.status===200) //check status
            {
                texta.value = xmlHttp.responseText;
            }
        }
    }

}
```
> Question 1b: Write a function named loadDoc2 that inserts the text no longer in a textarea, but with each line in a p element with a style attribute that assigns **different colors** to each line of inserted text. The id of the button shall be b2 and the id of the div containing your p elements shall be texta2.  

Write a function to generate random colors.
```
function randomColor() {
    var randomColor = Math.floor(Math.random()*16777215).toString(16);
    return "#" + randomColor;
}
```
Dynamic create <p> element.
```
    if (xmlHttp.readyState===4 && xmlHttp.status===200){  
        str_arr = xmlHttp.responseText.split(/[\n]/);
        for(var i = 0; i < str_arr.length; i ++){
            var p  = document.createElement('p');
            p.innerHTML = str_arr[i];
            p.style.color = randomColor();
            div.appendChild(p);
        }
    }
```

## Exercise 2 - Single Chat  
>Create the chat.php file in your personal pages from these lines:  
```
<?php
$chaine = gethostbyname($_SERVER['REMOTE_ADDR']) ;
$chaine .=  " - " . $_GET['phrase'] . "\n";
$fp = fopen("chatlog.txt","a");
if ($fp == false) {
  echo "Permission error on chatlog.txt: do 'chmod a+w chatlog.txt'";
} else {
  fwrite($fp, $chaine);
  fclose($fp);
  echo "Success";
}
?>
```
> Question 2.1: Create files ex2.html and exercise2.js that contain:
A text field to enter the new sentence, with the id textedit.  
A send button to send the new sentence with chat.php and delete the text field, with the id sendbutton.  
```
function send() {
    var textedit = document.getElementById("textedit");
    var text = textedit.value;
    textedit.value = "";

    var xmlHttp = new XMLHttpRequest();
    var url = "chat.php";
    url += "?phrase=" + text;

    if(xmlHttp != null){
        xmlHttp.open("get",url, true);
        xmlHttp.send();
    }

}
```
A div for the content of the chat with the id texta; NOTE: do not put the chat text directly into the div with br tags but structure this chat into paragraphs using p elements. There should be **no extra empty p elements** or text nodes in that div. Display requirement: **last on the top** & **max 10 messages**
```
function loadDoc2(){
    const div = document.getElementById("texta");
    console.log(div);
    div.innerHTML = "";

    var str_arr;
    var xmlHttp = new XMLHttpRequest();

    if(xmlHttp != null){
        xmlHttp.open("get","chatlog.txt", true);
        xmlHttp.send();
        xmlHttp.onreadystatechange = function () {
            if (xmlHttp.readyState===4 && xmlHttp.status===200)
            {
                str_arr = xmlHttp.responseText.split(/[\n]/);
                str_arr.pop(); //**no extra empty element**
                str_arr.reverse(); //**last on the top**
                console.log(str_arr);
                for(var i = 0; i < ((str_arr.length < 10)? str_arr.length : 10 ); i ++){ 
                //**max 10 messages**
                    var p  = document.createElement('p');
                    p.innerHTML = str_arr[i];
                    p.style.color = randomColor();
                    div.appendChild(p); //**last on the top**
                }
            }
        }
    }

}
```
A loop that reloads the contents of the chatlog.txt file (= the chat itself). Display the text in p elements. Make sure you do not not have empty p elements at the beginning or end. Redisplay the text every second, no more.
```
window.setInterval(loadDoc2, 1000);
```
**Attention:** setInterval(loadDoc2(), 1000) doesn't work   


## Exercise 3 - Slides in JSON  
> Put the slides.json file on your personal space from the content below.  
```
{
  "slides": [
    {
      "time": 0,
      "url": "https://perso.telecom-paris.fr/dufourd/git/"
    },
    {
      "time": 2,
      "url": "https://perso.telecom-paris.fr/dufourd/cours/inf203/js.html#/functions"
    },
    {
      "time": 4,
      "url": "https://perso.telecom-paris.fr/dufourd/git/slr201.htm#/git"
    },
    {
      "time": 6,
      "url": "https://perso.telecom-paris.fr/dufourd/cours/inf203/svg.html#/svg-curves"
    },
    {
      "time": 8,
      "url": "https://perso.telecom-paris.fr/bellot/CoursJava/JavaClassesObjets.html#slide21"
    },
    {
      "time": 10,
      "url": ""
    }
  ]
}
```
>Question 3.1: Create files ex3.html and exercise3.js that contain:

An empty div with id="SLSH" to receive the slides
A script to load exercise3.js
A function that **loads the slides.json** file with AJAX and renders the object described in the file
A function that plays the **slideshow**: at the time indicated by time, empty the div with id="SLSH" and add in this div an iframe pointing to the given URL. The id of the play button shall be PLAY.
```
function loadJson(){
    var xmlHttp = new XMLHttpRequest();
    if(xmlHttp != null){
        xmlHttp.open("get","slides.json", true);   // **change url**
        xmlHttp.send();
        xmlHttp.onreadystatechange = function () {
            if (xmlHttp.readyState===4 && xmlHttp.status===200)
            {
                var arr = JSON.parse(xmlHttp.responseText).slides;
                console.log(arr);
                var div = document.getElementById('SLSH');
                for(let i = 0; i < arr.length; i++){
                    let p  = document.createElement('iframe');
                    p.src = arr[i].url;
                    
                    //**slide show**
                    setTimeout(function () {        
                        div.removeChild(div.lastChild);
                        div.appendChild(p);
                    }, arr[i].time*1000);

                }
            }
        }
    }

}
```

>Question 3.2: create an ex4.html file and exercise4.js that contain the previous features plus:

A “pause / continue” button with id=“pauseButton”  
A “next slide” button with id=“suivant” and a “previous slide” button with id=“PREVIOUS”, which interrupts the playing of the slide show. These buttons should work even if the slideshow was not played.  
In this version, you have to change the algorithm substantially from the simple version that worked for the previous question.  

**Keys**: 1.load json file to global 2.have a global index 3. set a global timer  
```
let json_arr;
let i = -1; //index
let timer;
```
```
function startSlide(){
    i = i + 1;
    console.log(i);
    if(i < 6) {
        let div = document.getElementById('SLSH');
        let p = document.createElement('iframe');
        p.src = json_arr[i].url? json_arr[i].url : json_arr[i-1].url;
        console.log(p.src);
        div.removeChild(div.lastChild);
        div.appendChild(p);
        if (i === 5) {
            clearTimeout(timer);
        } else {
            timer = setTimeout("startSlide ()", (json_arr[i + 1].time - json_arr[i].time) * 1000);
        }

    }
}
```
```
function preSlide(){
    clearTimeout(timer);
    i = i > 0 ? i - 1 : i;
    console.log(i);
    let div = document.getElementById('SLSH');
    let p  = document.createElement('iframe');
    p.src = json_arr[i].url? json_arr[i].url : json_arr[i-1].url;
    div.removeChild(div.lastChild);
    div.appendChild(p);
}
```



