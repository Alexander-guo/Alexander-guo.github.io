---
layout: page
title: Grand Theft Auto Robot
description:  Self-Designed Differential Drive Robot 
img: assets/img/GTA_robot/cover.jpg
importance: 5
category: academic 
---

<!-- Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    --- -->

This is a differential drive robot designed for collecting beacons and soda cans in the game rule and also with a variety of other functionalities. The MCU is ESP32 and programmed by [Arduino C++](https://www.arduino.cc/) on this robot. [[code]](https://github.com/Alexander-guo/MEAM510_GTA_final_project)

<div class="container">
    <div class="row justify-content-center">
        <div class="col-sm-6">
            {% include figure.html path="assets/img/GTA_robot/cover.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>


### Functionalities

This is a self-designed differential drive robot featuring following functionalities:
* Wall Following
* [VIVE](https://www.vive.com/us/accessory/base-station/) Localizing and ID Broadcasting with UDP
* Autonomous Navigation to Given Position
* Beacon Tracking
* Can Gripping
* Manual Wireless Control through Web-based Joystick

__Wall Following__:

Two lateral and one front [ultrosonic sensors](https://maxbotix.com/blogs/blog/how-ultrasonic-sensors-work) are mounted on the chassis of the robot to detect obstacles. Ultrasonic sensors are able to measure distance through sound waves. The layout avoids the crosstalk between multiple sensors. Although the readings fluctrate most time, EMA (Exponential Moving Average) is applied to smoothing the measurement. A PID controller is also designed to keep the lateral distance to the wall. The turing logics in wall following is set as follows:
* If both the front sensor and one lateral sensor detect the wall, turn 90 degrees to the other direction.
* If only the front sensor detects the wall, turn 90 degrees to either direction.

__VIVE Localizing and ID Broadcasting__:

[VIVE](https://www.vive.com/us/accessory/base-station/) base stations beam both horizontal and vertical sweeping infrared laser lines alternately to trigger IR sensors on top of the robot, and by comparing the triggering time difference, the position and orientation of the robot can be caluculated. The ID is setup with a dip switch. Then the position and ID of the robot is broadcasted via UDP. The circuits design of IR laser detection in shown [here](#vive).  

__Autonomous Navigation__:

With the assistance of VIVE system, the robot is always able to determine its heading and position. Since soda cans' positions are also broadcasted, it becomes quite easy to drive the robot autonomously to any can's or given position by sending orders to the robot.

__Beacon Tracking__:

The beacons are setup to emitting 23Hz and 700Hz IR rays. Two [photosensitive circuits](#beacon) are designed to detect the presence of both frequency IR rays.

__Can Gripping__:

The contact of the gripper to alumnium cans is easily detected through a capacitive touch sensor, which is an one-wire circuit by reading sudden voltage drop with ADC on ESP32.

__Manual Wireless Control__:

The manual control is realized through a webpage joystick. The horizontal and vertical position of the pushbutton is mapped to the left and right motor rotation speed for differential maneuvers. The transmission is via UDP.

### Circuit Design

The specific design of beacon sensing, VIVE detection and motor driving circuits:
<div class="container">
    <div class="row justify-content-center">
        <div class="col-sm-10">
            {% include figure.html path="assets/img/GTA_robot/Beacon_Circuit.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
    <div class="caption">
        <a name="beacon">Beacon Sensing Circuit</a>
    </div>
    <div class="row justify-content-center">
        <div class="col-sm-10 mt-3 mt-md-0">
            {% include figure.html path="assets/img/GTA_robot/Vive_Circuit.png" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
    <div class="caption">
        <a name="vive">VIVE Detection Circuit</a>
    </div>
    <div class="row justify-content-center">
        <div class="col-sm-8 mt-3 mt-md-0">
            {% include figure.html path="assets/img/GTA_robot/Motor_Circuit.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
    <div class="caption">
        Motor Driving Circuit
    </div>
</div>

### Videos

__Autonomous Navigation__:

<!-- [![Auto_Navig](http://img.youtube.com/vi/HcDnAdSXqKk/0.jpg)](https://www.youtube.com/watch?v=HcDnAdSXqKk) -->

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include video.html path="https://www.youtube.com/embed/HcDnAdSXqKk?si=-Hiypdr2dh2Spgal" class="img-fluid rounded z-depth-1" 
        width=900 %}
    </div>
</div>

__Wall Following__:

<!-- [![Wall_Follow](http://img.youtube.com/vi/rx5FApxXP7M/0.jpg)](https://www.youtube.com/watch?v=rx5FApxXP7M) -->

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include video.html path="https://www.youtube.com/embed/rx5FApxXP7M?si=ogZ9--m--0_bwJwY" class="img-fluid rounded z-depth-1" 
        width=900 %}
    </div>
</div>

__Beacon Tracking__:

<!-- [![Beacon](http://img.youtube.com/vi/L0Ju4nM8VtI/0.jpg)](https://www.youtube.com/watch?v=L0Ju4nM8VtI) -->

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include video.html path="https://www.youtube.com/embed/L0Ju4nM8VtI?si=dwA_xqm16WCEG14I" class="img-fluid rounded z-depth-1" 
        width=900 %}
    </div>
</div>

<!-- You can also put regular text between your rows of images.
Say you wanted to write a little bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, *bled* for your project, and then... you reveal its glory in the next row of images. -->

<!-- The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above: -->


