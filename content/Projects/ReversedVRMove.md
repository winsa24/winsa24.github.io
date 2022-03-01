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
What will happpen if you are moving in a reversed world ? Want to go left but actually go right ? Going backward to move forward ! Some experience you will NEVER gain in a physical world !  How will it impact on us ? Let's figure it out !


## Download from [github](https://github.com/winsa24/ReversedVRMove)
## Play instruction
![ins](/images/courses/igd301/P5/instruction.png)  
## Video
[![Watch the video](http://img.youtube.com/vi/wKmwVckSEc8/0.jpg)](https://www.youtube.com/watch?v=wKmwVckSEc8)



## Motivation
The goal of this project is to explore the impact of VR on people's activities in the physical world. I implemented a virtual environment (using Unity3D and the Oculus Quest) that can reverse a user's movement. This project was supervised by Jan Gugenheimer and Wen-Jie Tseng, and can be used to detect the potential physical harms of VR.

The application that I created is presented this way : there is a virtual room with colorful walls and two chairs are placed symmetrically in the room. Around the two chairs, there is a track of infinite shape with arrows on it, guiding the user to experience all kinds of movement(equally left and right turns). There is a button allows user to switch between virtual environment and physical environment. In the physical environment, there are the same box envirnoment with two chairs placed at the relatively same place. By switching to the physical environment, the user needs to remove the glasses from the eyes, but still needs to carry the glasses to record the movement trajectory.

![ins](/images/courses/igd301/P5/instruction.png) 

## VR potential impact to physical world

## Implementation
#### Reverse Movement

The first thing to do was to reverse the movement of user.   

My first idea was to composly reverse the position of camera. Since we know that the equipment will always override the position of VR camera according to its exact physical value, while in Unity3D, a GameObject always inherits the properties of its parent GameObject, maintaining an absolute relative position to it. Therefore, I tried to add an **Empty GameObject** as the parent of VR camera, and attach an script setting the opposite position to the empty GO. Unfortunately, it seems that the virtual camera device has the highest override permissions. So we have to find another way.   
![empt](/images/projects/ReverseVR/emptyGO.png)


The second idea was to use **Render texture**. Since we can't really reverse the users' movement (camera position), we tried to give them the sense of reversing : opposite the field of view. In this stragegy, we attached a render texture to our VR camera. The texture is rendering by another camera (we will call it view camera in the following) placed in the scene. The view camera initialized as the same view as the VR camera, yet it always takes the opposite position as our VR camera moved. From our headset, since we only see the render texture from the view camera, it will give us the sense that we move opposite than real.   
![rt](/images/projects/ReverseVR/renderTexture.gif) 
Although it tested great as the previous image, but when we real implemented it on our headset, it doesn't work properly. When we tile or yawn our head, the whole environment is turning with us. A possible reason could be we didn't properly coordinate the rotation among the two camera.
![rt](/images/projects/ReverseVR/rendertexture.mov) 


Therefore, we jump out of the box again. As clarified in the render texture strategy, we only need to provide the user a sense of reverse. So what about we **move the scene** ? While keeping the real position of virtual camera, we therefore need to move the entire scene double distance as the camera movement, in order to give user the sense of reversing. On the other hand, since we are only operating a normal scene, we won't rotate together with the enviroment.
![move2d](/images/projects/ReverseVR/move2d.png)

#### Redraw moving path


## Results
We don't have enough time to test this app with more external users. We only did the experience ourselves. We had a hard time when first adjusting to the reversed movement, but we surprisely found that we could adjust it really fast. After moving in the reversed world, when we took off the headset, we would feel confused by the starting 3 seconds, but after we can adjust back. It’s interesting to see that people can be easily controlled by what they see.