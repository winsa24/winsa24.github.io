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
![ins](/images/projects/ReverseVR/instruction.png)  
#### Video
![demo](/images/projects/ReverseVR/demo(all).mp4)  
#### Presentation  
https://docs.google.com/presentation/d/1TV_MF5ZaYTl9RdQCPSqY2aA-RQWA4oex2c2gDJIY_eo/edit?usp=sharing



## Motivation
The goal of this project is to explore the impact of VR on people's activities in the physical world. I implemented a virtual environment (using Unity3D and the Oculus Quest) that can reverse a user's movement. It is an extreme situation that we are expecting to see more obvious effect. Reverse movement is just a starting point, since walk is the most basic behavoir we have. While later, we could also create reverse jump, reverse picking, reverse sitting etc. We are expecting to see how these behavoir will impact on us and compare the result we had with simple walking. This project was supervised by Jan Gugenheimer and Wen-Jie Tseng, and can be used to detect the potential physical harms of VR. 


The application that I created is presented this way : there is a virtual room with colorful walls and two chairs are placed symmetrically in the room. Around the two chairs, there is a track of infinite shape with arrows on it, guiding the user walk around them. There is a button allows user to select different mode. In different reversed mode, based on the initial view, movement along the corresponding axis will be reversed. User is invited to walk 3 rounds and by wearing the headset, it automatically record the path. There is another button allows user to switch between virtual environment and physical environment. In the physical environment, there are the same box envirnoment with two chairs placed at the relatively same place. By switching to the physical environment, the user needs to remove the glasses from the eyes, but still needs to carry the glasses to record the movement trajectory. We are expecting to see will the movement cause any effect on user's same behavoir in physical world.

![demo](/images/projects/ReverseVR/demo(all).mp4)  

## VR potential impact to physical world
Evaluate VR impact on physical world is not a new topic. Many studies have run on this topic.  

Stanford has run an experiments [1](https://brgcommunications.com/virtual-reality-impact-behavior-change/) that creating situations such as putting participants, who are not colorblind, through colorblindness VR color-matching exercises.  The scientists measure the likelihood of understanding the perspective of others before and after the exercise, noting positive change from participants after the exercise. That empathy is also being applied by the participants into future, separate scenarios, demonstrating the opportunity for behavior change learned in a scenario to have long-term impacts on an individual. 
![colorblindnessEXP](/images/projects/ReverseVR/colorblindness.jpg)

Many people also experience different degrees of cybersickness when using VR. Cybersickness is a form of motion sickness that occurs as a result of exposure to immersive eXtended Reality (XR) environments. Depending on the immersive content and virtual movement required, 20%-95% of users typically experience some form of cybersickness, ranging from a slight headache to an emetic response. The most common symptoms include general discomfort, headache, eyestrain, stomach awareness, nausea, sweating, sopite syndrome (a.k.a. drowsiness), and disorientation. Only on rare occasions (~1%), an emetic response is experienced. [2](https://www.frontiersin.org/research-topics/30494/cybersickness-in-vr-applications)

Either it's possitive or negative effect, we can see that VR does bring impact to human in physical world. So we are curious about how those impacts are and how do people react to adjust to these impacts.

## Implementation
#### Reverse Movement

The first thing to do was to reverse the movement of the user.   

My first idea was to composly reverse the position of camera. Since we know that the equipment will always override the position of VR camera according to its exact physical value, while in Unity3D, a GameObject always inherits the properties of its parent GameObject, maintaining a editable relative position to it. Therefore, I tried to add an **Empty GameObject** as the parent of VR camera, and attach an script setting the opposite position to the empty GO. Unfortunately, it seems that the virtual camera device has the highest override permissions. So we have to find another way.   
![empt](/images/projects/ReverseVR/emptyGO.png)


The second idea was to use **Render texture**. Since we can't really reverse the users' movement (camera position), we tried to give them the sense of reversing : opposite the field of view. In this stragegy, we attached a render texture to our VR camera. The texture is rendering by another camera (we will call it view camera in the following) placed in the scene. The view camera initialized as the same view as the VR camera, yet it always takes the opposite position as our VR camera moved. From our headset, since we only see the render texture from the view camera, it will give us the sense that we move opposite than real.   
![rt](/images/projects/ReverseVR/renderTexture.gif) 
Although it tested great as the previous image, but when we real implemented it on our headset, it doesn't work properly. When we tile or yawn our head, the whole environment is turning with us. A possible reason could be we didn't properly coordinate the rotation among the two camera.
![rt](/images/projects/ReverseVR/rendertexture.mp4) 


Inspired from the render texture strategy, since we only need to provide the user a sense of reverse, we can **move the scene** instead of move the camera. While keeping the real position of virtual camera, we therefore need to move the entire scene double distance as the camera movement, in order to give user the sense of reversing. On the other hand, since we are only operating a normal scene, we won't rotate together with the enviroment.
![move2d](/images/projects/ReverseVR/move2d.png)

#### Setting Up Scene

There are two basic goals on setting up the scene. First is to help users locate themselves, to enhance the sense of reverse. Second is to set a clear task, to guide users' movement.

Therefore, I set colorful grid pattern to the walls. So when user move, they can better notice to which direction that they are moving towards to. 
For the task, I set an infinite shape track to achieve all direction movement and equally left and right turns as well. 
![scene](/images/projects/ReverseVR/VRScene.png)

#### Analyse
There is a script attached to the VR camera recording the position of Oculus headset each 50 frames. It will also record the changes of reverse mode or environment. Based on the gathered data, I created an web page to reproduce the motion trajectory. Different environment will change the line dash and different mode will change the line color. 
![drawPath](/images/projects/ReverseVR/drawpath.gif)



## Findings
We don't have enough time to test this app with more external users. We only did the experience ourselves. We had a hard time when first adjusting to the reversed movement, but we surprisely found that we could adjust it really fast. After moving in the reversed world, when we took off the headset, we would feel confused by the starting 3 seconds, but after we can adjust back. It’s interesting to see that people can be easily controlled by what they see.