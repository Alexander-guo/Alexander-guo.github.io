---
layout: page
title:  Events Replay Buffer 
description: Replay Events Stream into Super Slow Motion
img: assets/img/Event_rply/cover.gif
importance: 6
category: academic 
---

Event cameras, namely Dynamic Vision Sensor (DVS), are bio-inspired vision sensors that output pixel-level brightness changes (`events`) instead of standard intensity frames. They offer significant advantages over standard cameras, namely a very high dynamic range, no motion blur and extremely low latency in the order of microseconds. This toy project exploits the high temporal resolution of events data. The buffer is able to replay live events stream up to $$ 10^6 \times$$  slower to better study high speed motion. [[code](https://github.com/Alexander-guo/events-replay-buffer)]

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include figure.html path="assets/img/Event_rply/cover.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    10x Slow Motion Replay of Events Stream
</div>
