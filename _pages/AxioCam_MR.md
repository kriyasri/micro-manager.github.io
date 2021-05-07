---
autogenerated: true
title: AxioCam MR
layout: page
---

## Zeiss AxioCam MR/HR Adapter

<table>
<tr>
<td markdown="1">

**Summary:**

</td>
<td markdown="1">

Interfaces Zeiss AxioCam medium resolution and high resolution color and
monochrome cameras

</td>
</tr>
<tr>
<td markdown="1">

**Author:**

</td>
<td markdown="1">

Henry Pinkard

</td>
</tr>
<tr>
<td markdown="1">

**License:**

</td>
<td markdown="1">

Source code cannot be made available

</td>
</tr>
<tr>
<td markdown="1">

**Platforms:**

</td>
<td markdown="1">

Windows

</td>
</tr>
<tr>
<td markdown="1">

**Devices:**

</td>
<td markdown="1">

AxioCam MRm/MRc/HRm/HRc, MRm/MRc/HRm/HRc Rev. 2, and MRm/MRc/HRm/HRc
Rev. 3 (only tested with Rev. 3 cameras)

</td>
</tr>
</table>

Because of built in delays in the AxioCam's image acquisition, there is
a delay of approximately 200ms between when Micro-Manager sends the
command to open the shutter and when the AxioCam's exposure begins. If
over exposure of the sample is a concern, it is highly recommended that
hardware triggering is used with these cameras. Refer to the below
descriptions of hardware triggering to do so (All images and text below
©2010 Carl Zeiss MicroImaging GmbH. All rights reserved.).

<h2>

Trigger-Out

</h2>

Via a supplementary cable the AxioCam has the capability of sending out
a TTL trigger pulse to an external device, for example a mechanical
shutter. This trigger pulse can be used to synchronize the exposure of
the camera and the task of the external device, as you can specify the
delay between sending out the trigger signal and actually starting the
acquisition.

To meet the requirements of the device you are using, you can invert the
polarity that you want the signal to have.

<h2>

**Trigger-In**

</h2>

The exact time of exposure can be triggered from outside the camera.
When the trigger-in is enabled and an acquisition function is called,
the camera will delay the acquisition and wait for a trigger-in signal
to be sent from an external device until it invokes the exposure. The
following graphic demonstrates the two camera models' reaction to the
trigger signal when performing a triggered acquisition. An acquisition
request is sent out at each **T**, the acquisition takes place at an
**A**: ![](media/AxioCamFig1a.png "fig:media/AxioCamFig1a.png")

![](media/AxioCamFig1b.png "media/AxioCamFig1b.png")

NOTE: The trigger OUT of the AxioCam HR / MR expects a TTL signal.

<h2>

Trigger Timing

</h2>

Due to different hardware and trigger mechanisms, the precise
calculation of the camera's timing when involvong the trigger IN / OUT
is a bit difficult and depends on the camera model's internal design.

The following charts demonstrate, how an example acquisition using both
trigger IN and trigger OUT could look like and explain how to precisely
calculate the timing.

<table border=1 bordercolor=#999999 cellpadding=5 cellspacing=0
       style='margin:10pt 20pt 10pt; font-family:sans-serif; font-size:9pt'>
<tr>
<td markdown="1" bgcolor=#CCCCCC>

<b>AxioCam HR / MR</b>

</td>
</tr>
<tr>
<td markdown="1">

The AxioCam HR / MR accepts only positive values for the trigger OUT
delay, ranging from 0 to 255 row cyles, where 1 cycle lasts 154 µs
(AxioCam HR) respectively 99 µs (AxioCam MR).  
When the delay is set, the trigger OUT signal is sent prior to exposing
the CCD sensor. The camera takes the following steps when performing an
acquisition using trigger IN and OUT:  

-   receive acquisition call (snap)
-   wait for trigger IN signal to arrive
-   send trigger OUT signal
-   delay the exposition (0-255 row cycles)
-   acquire

</td>
</tr>
</table>

![](media/AxioCamFig2.png "media/AxioCamFig2.png")

<table border=1 bordercolor=#999999 cellpadding=5 cellspacing=0
       style='margin:10pt 20pt 10pt; font-family:sans-serif; font-size:9pt'>
<tr>
<td markdown="1" bgcolor=#CCCCCC>

<b>AxioCam MRc5 / MR Rev.3 / HS</b>

</td>
</tr>
<tr>
<td markdown="1">

The AxioCam MRc5 / MR Rev.3 / HS allows both positive and negative
values for the trigger OUT delay, ranging from +4095 row cycles to -4095
row cyles, where 1 cycle lasts 244 µs.  
   
A <b>negative</b> delay means sending the trigger OUT signal prior to
exposing the sensor (like AxioCam HR/MR).  
A <b>positive</b> delay means sending the trigger OUT signal after
starting the exposition of the CCD sensor.  
   
<u>Sensor cleanout:</u>  
   
Before the AxioCam MRc5 / MR Rev.3 / HS is able to acquire an image, a
certain 'cleanout time' is required to erase any existing data from the
CCD sensor. When an acquisition call has been sent by the user / program
and the trigger IN signal is being received, the camera needs about 6.5
ms of processing time. Now the initialization takes place, including a
fixed 1ms delay (4 idle rows) and the sensor cleanout time, which
depends on the current exposure time. A simplified formula for the
cleanout time is:  
   
cleanout time = ( 1003 - (exposure time - 1) \* binning factor ) /
23    <em>\[ unit: row cycles = 250 µs \]</em>  
   
<u>Note:</u> when the exposure time is set to 1004 row cycles ( approx.
251ms ) or more, the cleanout time will be zero.

</td>
</tr>
<tr>
<td markdown="1" bgcolor=#CCCCCC>

\- negative delay  

</tr>
<tr>
<td markdown="1">

If the specified trigger OUT delay is longer than the interval for the
fixed idle rows and clearing the sensor, the trigger OUT is sent
directly after the initialization has been completed. The camera
performs the cleanout and then waits an appropriate amount of time until
the exposition is invoked.  

-   receive acquisition call (snap)
-   waits for trigger IN signal to arrive
-   approx. 6.5 ms processing time
-   send trigger OUT signal
-   finish idle rows and sensor cleanout
-   wait delta( trigger OUT delay - idle rows - cleanout time )
-   start acquisition

</td>
</tr>
</table>

![](media/AxioCamFig3.png "media/AxioCamFig3.png")

<table border=1 bordercolor=#999999 cellpadding=5 cellspacing=0
       style='margin:10pt 20pt 10pt; font-family:sans-serif; font-size:9pt'>
<tr>
<td markdown="1">

If the specified trigger OUT delay is shorter than the interval for the
fixed idle rows and clearing the sensor, the camera starts the sensor
cleanout right after the initialization was finished and delays sending
the trigger OUT signal to achieve the desired trigger OUT delay.  

-   receive acquisition call (snap)
-   waits for trigger IN signal to arrive
-   approx. 6.5 ms processing time
-   start idle rows and sensor cleanout
-   wait delta( idle rows + cleanout time - trigger OUT delay ), then
    send trigger OUT
-   start acquisition

</td>
</tr>
</table>

![](media/AxioCamFig4.png "media/AxioCamFig4.png")

<table border=1 bordercolor=#999999 cellpadding=5 cellspacing=0
       style='margin:10pt 20pt 10pt; font-family:sans-serif; font-size:9pt'>
<tr>
<td markdown="1" bgcolor=#CCCCCC>

\- positive delay

</td>
</tr>
<tr>
<td markdown="1">

-   receive acquisition call (snap)
-   waits for trigger IN signal to arrive
-   approx. 6.5 ms processing time
-   sensor cleanout
-   start acquisition
-   delay the trigger OUT and send it

</td>
</tr>
</table>

![](media/AxioCamFig5.png "media/AxioCamFig5.png")

--[Henry Pinkard](User:Henry_Pinkard "wikilink") 10:58, 1st October 2012
(PDT)

{% include Listserv_Search text="AxioCam" %}

{% include Devices_Sidebar text="" %}