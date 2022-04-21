---
title: ReversedVRMove
date: 2022-01-26

description: 
categories: [VR Develop]
tags: [Unity, VR, IP Paris]

draft: false
enableDisqus : true
enableMathJax: false
disableToC: false
disableAutoCollapse: true

---
# Reversed VR Move
What will happpen if you are moving in a reversed world ?  
Want to go left but actually go right ? Going backward to move forward !  
Some experience you will NEVER gain in our real physical world ! 
How will you adapt to it ?    
How will it impact on us ? 


#### Download from [github](https://github.com/winsa24/ReversedVRMove)
#### Play instruction
Please check inside the game ^ ^
<!-- ![ins](/images/projects/ReverseVR/instruction.png)   -->
#### Video
[![Watch the video](http://img.youtube.com/vi/9q9WuNcCT7I/0.jpg)](https://www.youtube.com/watch?v=9q9WuNcCT7I)
<!-- #### Presentation  
https://docs.google.com/presentation/d/1TV_MF5ZaYTl9RdQCPSqY2aA-RQWA4oex2c2gDJIY_eo/edit?usp=sharing -->



## Motivation
The goal of this project is to explore the impact of VR on people's activities in the physical world. I implemented a virtual environment (using Unity3D and the Oculus Quest) that can reverse user movement. Users will get the opposite visual experience, when they move around the virtual environment. This is an extreme case setup and we expect to see more pronounced effects. Reverse movement is just a starting point, since walking is the most basic behavoir we have. While later, we could also create reverse jumps, reverse pickups, reverse sits, etc. We are expecting to see how will the virtual experience leave impact on us. This project was supervised by Jan Gugenheimer and Wen-Jie Tseng. 
![demoshort](/images/projects/ReverseVR/demo.gif)  

The application that I created is presented in this way : there is a virtual room with colorful walls and two chairs are placed symmetrically in the room. Around the two chairs, there is a track of infinite shape with arrows on it, guiding the user walk around them. Based on different selected reversed mode and the initial position, the view changes in the opposite direction of the axis of user movement. The headset will automatically record its position (user's position). There are serveral assigned buttons for user to specify their situation. It will help us to category the data and present it in better style. We also created a similary environment in the physical world, unless the colorful walls and ground. While moving in the physical environment, the user still needs to carry the headset, in order to record the movement trajectory data. Last, a web application is developed to read the input data and draw it as lines on canvas. 

## VR potential impact to physical world
Evaluate VR impact on physical world is not a new topic. Many studies have run on this topic.  

Stanford has run an experiments [[1]](https://brgcommunications.com/virtual-reality-impact-behavior-change/) that creating situations such as putting participants, who are not colorblind, through colorblindness VR color-matching exercises.  The scientists measure the likelihood of understanding the perspective of others before and after the exercise, noting positive change from participants after the exercise. That empathy is also being applied by the participants into future, separate scenarios, demonstrating the opportunity for behavior change learned in a scenario to have long-term impacts on an individual. 
![colorblindnessEXP](/images/projects/ReverseVR/colorblindness.jpg)

Many people also experience different degrees of cybersickness when using VR. Cybersickness is a form of motion sickness that occurs as a result of exposure to immersive eXtended Reality (XR) environments. Depending on the immersive content and virtual movement required, 20%-95% of users typically experience some form of cybersickness, ranging from a slight headache to an emetic response. The most common symptoms include general discomfort, headache, eyestrain, stomach awareness, nausea, sweating, sopite syndrome (a.k.a. drowsiness), and disorientation. [[2]](https://www.frontiersin.org/research-topics/30494/cybersickness-in-vr-applications)

Either it's possitive or negative effect, we can see that VR does bring impact to human in physical world. So we are curious about how those impacts are and how do people react to adjust to these impacts.

## Implementation
### Reverse Movement

The first thing to do was to reverse the movement of the user.   

My first idea was to composly reverse the position of camera. Since we know that the equipment will always override the position of VR camera according to its exact physical value, while in Unity3D, a GameObject always inherits the properties of its parent GameObject, maintaining a editable relative position to it. Therefore, I tried to add an **Empty GameObject** as the parent of VR camera, and attach an script setting the opposite position to the empty GO. Unfortunately, it seems that the virtual camera device has the highest override permissions. So we have to find another way.   
![empt](/images/projects/ReverseVR/emptyGO.png)


The second idea was to use **Render Texture**. Since we can't really reverse the users' movement (camera position), we tried to give them the sense of reversing : opposite the field of view. In this stragegy, we attached a render texture to our VR camera. The texture is rendering by another camera (we will call it view camera in the following) placed in the scene. The view camera initialized as the same view as the VR camera, yet it always takes the opposite position as our VR camera moved. From our headset, since we only see the render texture from the view camera, it will give us the sense that we move opposite than real.   
![rt](/images/projects/ReverseVR/renderTexture.gif) 
Although it tested great as the above gif, but when we real implemented it on our headset, it doesn't work properly. When we tile or yawn our head, the whole environment is turning with us. A possible reason could be we didn't properly coordinate the rotation among the two camera.
![rt](/images/projects/ReverseVR/rendertextureScene.gif) 


Inspired from the render texture strategy, since we only need to provide the user a sense of reverse, we can **move the scene** instead of move the camera. While keeping the real position of virtual camera, we therefore need to move the entire scene double distance as the camera movement, in order to give user the sense of reversing. On the other hand, since we are only operating a normal scene, we won't rotate together with the enviroment.
![move2d](/images/projects/ReverseVR/move2d.png)

### Setting Up Scene

There are two basic goals on setting up the scene. First is to help users locate themselves, to enhance the sense of reverse. Second is to set a clear task, to guide users' movement.

Therefore, I set colorful grid pattern to the walls. So when user move, they can better notice to which direction that they are moving towards to. 
For the task, I set an infinite shape track to achieve all direction movement and equally left and right turns as well. 
![scene](/images/projects/ReverseVR/VRScene.png)

### Analyse
There is a script attached to the VR camera recording the position of Oculus headset each 50 frames. It will also record the changes of settings (include reverse mode, environment, track visibility etc). 
With all this collected data, I can reproduce them by drawing the user's actions on the canvas. When plotting the data, we need to distinguish between the data collected at different settings. We used colors and dash lines and created a legend. We also implemented a filter to avoid dirty data. We calculated the distance between two neighboring points and once a certain threshold was exceeded, we did not show it on the canvas.


## Findings
To test our application, I designed a user experiment :   
3 rounds of walk before using VR, then 3 rounds in the virtual world, and the last 3 rounds of walk after experiencing VR.  


After several attempts by myself, I became kind of an expert in the reverse world. So my tracing track is like :  
![drawPath](/images/projects/ReverseVR/worldDP2.gif)
So I challenged myself into different reversed mode. And it does took me longer time to adjust and achieve the goal. Under these two modes, I extended myself with an extra task : reproduce the path I went through in vr world in physical world. Normally we should get infinite shape in two direction under these two modes. And when I tried to reproduce, I completed a good shape in the end while not in the correct order. In real is, I actually lost my sense of direction in the reversed virtual world.
![drawPath](/images/projects/ReverseVR/moveDP.gif)

I also invited a friend to test, but only the reverse all mode. He really struggled, but he managed to complete the task. 
![demoF](/images/projects/ReverseVR/userTest.gif)
While form his path review, we can tell that the yellow line takes up more space than the other two and contains a large jaggedness. That exactly reflects the struggling he had in VR world. On the other hand, by comparing the auqa and purple line, we are not concerned about their offset, we are more concerned about their fluency (level of jaggedness), and completion time as well. The duration shows here is not real time duration in real world, but we can get the approximate duration by calculate with frame rate. We are not concerned about the value itself neither, we are more concerned about comparing the difference between before and after using VR. From these two judging criteria, we can measure the impact that vr brings to us.
![drawPath](/images/projects/ReverseVR/worldDP.gif)


Despite the varying degrees of torture we suffered in the virtual world (expert users and novices), surprisingly we didn't bring any impact back to the real world.  In a casual conversation after the test, he said the task was so hard that he couldn't really walk, it was more like stumbling around in the virtual world. So when he came back to the real thing, he just felt like he got his ability to move back.We speculate that the reason for not leaving any impact may be that a very difficult task is very difficult to change a behavior that we are so used to.  Also because humans rely most on what they see.


An bonus finding during the talk was that if I asked him to reproduce the path he had taken in the VR world, he said it was impossible.  He was completely disoriented in the virtual reality world. As an expert user, I had similar feedback. In the video, I tried to reproduce the path and I succeed in the reverse all mode but failed in the other two modes. A reason to explain the difference could be I did more 360-degree turns in the two failed modes and it made me quickly lose my positioning capability. But the collision with two chairs somehow contribute a bit to give me a little perception of my position in the real world. **One potential way to conduct this project is to study how users orient themselves in the real world while wearing a VR headset.**