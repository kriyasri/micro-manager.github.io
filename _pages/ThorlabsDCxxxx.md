---
autogenerated: true
title: ThorlabsDCxxxx
layout: page
---

## Thorlabs DCxxxx adapter

<table cellspacing=3>
<tr>
<td markdown="1">

**Summary:**

</td>
<td markdown="1" valign="top">

Interface to Thorlabs DCxxxx LED driver series (DC2100, DC2010, DC3100,
DC4100, DC2200)

</td>
</tr>
<tr>
<td markdown="1">

**Author:**

</td>
<td markdown="1">

Olaf Wohlmann

</td>
</tr>
<tr>
<td markdown="1">

**License:**

</td>
<td markdown="1">

LGPL

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

**Available since version:**

</td>
<td markdown="1">

1.3.41

</td>
<tr>
<td markdown="1" valign=top>

**Default serial port settings:**

</td>
<td markdown="1" valign=top>

|                     |        |
|---------------------|--------|
| AnswerTimeout       | 500    |
| BaudRate            | 115200 |
| DelayBetweenCharsMs | 0      |
| Handshaking         | Off    |
| Parity              | None   |
| StopBits            | 1      |

</td>
</tr>
</table>

The Thorlabs DCxxxx adapter operates the [DC2100 - High Power LED
Driver](http://www.thorlabs.de/NewGroupPage9.cfm?ObjectGroup_ID=4003&pn=DC2100&CFID=709850&CFTOKEN=40518309),
the DC2010 - Universal LED Driver, the DC3100 - FLIM LED Driver, the
[DC4100 - 4 Channel LED
Driver](https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=3832)
and the [DC2200 - High Power LED
Driver](https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=9117).

This adapter makes the LED driver usable as a shutter in Micro-Manager.
On startup, the adapter will switch the LED off. The adapter has several
properties that can be used to control the LED driver.

To operate the DC2200 or DC4100, the [Thorlabs DC2200 software
package](https://www.thorlabs.com/software_pages/ViewSoftwarePage.cfm?Code=DC2200)
has to be installed. In general, if this adapter is greyed out in the
Hardware Configuration Wizard, try installing the DC2200 software.

{% include Note text="The above mentioned serial port settings are the default, but they are recommended to communicate with the device." %}

{% include Devices_Sidebar text="" %}