---
date: 2021-04-11
description: ""
tags: ["Lecture", "HCI"]
title: "Gestural Interfaces"
---

# To design a gestural interfaces 
## Creating a gesture set and defining a gesture-command mapping

### Laban feature
Laban Movement Analysis (LMA)
> The LMA system provides models for the interpretation of movement, its function and its expression through 4 components:
− Body (what)
− Effort (how)
− Space (where)
− Shape (relation with the environment)
> Effort has 4 Factors thought as a continuum with 2 opposite ends:
− Weight : strong, light
− Time : quick, substained
− Space : flexible, direct
− Flow : bound , free  

## Choosing the device
### Structured light
> Depth from focus: the further the blurrer
> Depth from stereo: view different from different angle  

### Time of Flight (TOF) imaging:   
Use phase difference between the radiated and reflected IR wavesis to compute the distance of objects.   


## Building a recognizer

### Rubine algorithm 
There is a set of C gesture classes (from 0 to C-1). Each class is specified by example gestures. Given an input gesture g, determine the class to which g belongs.   
Statistical gesture recognition is done in 2 steps: first a vector of features f is extracted from the input gesture. Then, f is classified as one of the C possible gestures.   
**Recognition of 2D single-stroke gestures with a clear start and a clear end**  

###  $ Family (Template-based) 
$1 one stroke recoginzer   
对输入的手势轨迹进行重新采样，将采样轨迹中心点与x轴的连线旋转到0度，将旋转后的手势缩放到标准的正方形大小并进行手势平移。然后，对处理过的手势和搜索匹配模板使用“黄金选择搜索”(GSS)，得到每个模板的最佳匹配得分。得分最高的手势是最终识别的手势
1. Resampling the point path (gesture trajectory)   
2. Rotate once based on the « indicative angle » (parallel to x axis)   
3. Scale and traslate  
4. Recognition  ( Golden Section Search (GSS))  
Limitations:
1. No way to distinguish a rectangle by a square
2. No way to distinguish an ellipse by a circle
3. No way to distinguish the orientation of an arrow

### DTW (Template-based)

### Machine Learning approaches: kNN, SVM...   


## Providing a teaching method
### When  
>Feedback vs feedforward information:
− Feedback: informs about the effects of the actions already performed  
− Feedforward: provides information prior to any action, i.e. showing or anticipating the possible future actions.  


## Evaluating your gestural interface



