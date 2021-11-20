---
title: "Introduction to Alarm Management"
date: 2021-11-15 10:00:00 +0000
categories: ot
folder: "2021-11-15-introduction-to-alarm-management"
tn_ref: TN2153001
tn_rev: "01"
abstract: >
  Introduction to Alarm Management
---
## 1 INTRODUCTION

This document describes the basic functionalities associated with how the
alarms are managed on an Alarm System. At the time of writing, most of the
active and recent projects were in the marine and offshore environment. For
that reason, the information presented in this note is influenced by the
standards applied in such environments.

## 2 DEFINITIONS

The table below lists the terms used in this document and the respective
definitions according to the IEC standard[^1].

<table class="definitions">
    <tr>
      <th class="definitions" width="150px">Term</th>
      <th class="definitions">Definition</th>
    </tr>
    {% for entry in site.data.2021-11-15-post-data1 %}
      <tr class="definitions">
        <td><i>{{ entry.term }}</i></td>
        <td>{{ entry.desc }}</td>
      </tr>
    {% endfor %}
</table>

Legacy alarm systems, particularly those lacking the alarm acknowledgement
functionality, use the term **alarm accept**. However, this term is not
mentioned in the IEC standard [^1] nor any marine classification society rules
listed in the References section. Therefore, the term alarm accept will be
omitted in this document.

The **reset** functionality will also be left out of the scope of this document.
Except for latching alarms, the reset functionality is associated with the
process, not alarm management. A **latching alarm** remains in the active state
(see Alarm States section) even after the condition that triggered it returns
to normal. Only the operator reset will transition the alarm state to normal.
Note that a latching alarm is not a common functionality.

The terms **acknowledge** and **silence** often cause confusion. Due to the
importance of these terms in an Alarm System and their consequence in the
control of a process, these functionalities will be detailed below. First,
however, an understanding of the alarm states is important.

## 3 ALARM STATES ##

The figure below presents a diagram showing the alarm state transition. The
diagram shows the four main alarm states and omits the following states,
usually implemented in more complex alarm systems:

- **Shelved**: Temporarily suppress an alarm initiated by the operator, with
engineering controls to unsuppress the alarm.
- **Suppress by design**: Alarm annunciation to the operator is prevented based
on plant state or other conditions.
- **Out of Service**: Alarm indication is suppressed, typically manually, for
reasons such as maintenance.

Each alarm state has two characteristics: the *active state*, which is directly
related to the state of the process, and the *acknowledge state*. These two
characteristics are completely independent; one is associated with the state of
a process, the other is associated with an operator action.

![states](/assets/images/posts/2021-11-15-introduction-to-alarm-management/img0301.png)

An Alarm System reacts to those transitions, also called events. An alarm list
displays or changes the information to the operator whenever one of those
transitions, or events, occur. Each event has a timestamp associated, and that
is the timestamp shown to the user on any alarm list.

### 3.1 Active State

As mentioned, the alarm state is directly related to the state of the process,
i.e., to the condition (e.g., temperature, pressure, contact state, etc.) that
activates the alarm. Therefore, the alarm will be active only when the process condition is abnormal.

An exception to the above is the latched alarm. As the Definitions section
mentions, a latched alarm becomes active when the process becomes abnormal but
only returns to the not active state after an operator reset.

### 3.2 Acknowledge State

The acknowledge state of an alarm is purely dependent on the operator. The
acknowledge can be seen as a notification from the operator to the Alarm System
that a specific alarm was seen. Therefore, the alarm system must allow the
operator to acknowledge only the alarms presented to the operator. DNV
Rules[^3] explicitly state in paragraph 1.5.6 that “*only visible alarms may be
acknowledged.*”

Note that some alarms might be deemed not required to be
acknowledged due to their lower severity. In that case, the alarm will become
NORMAL as soon as the process condition is no longer abnormal.

## 4 ACKNOWLEDGE AND SILENCE

The functionalities associated with Acknowledge and Silence are different.
However, in specific circumstances, these functions can be performed
simultaneously, as described in the following sections.

### 4.1. Acknowledge
The acknowledge functionality was already presented in the Acknowledge State
section above.

The requirement that the acknowledge functionality must only acknowledge the
alarms presented to the operator is simpler to comply with if the acknowledge
function is performed where the alarms are presented, i.e., the computer or
graphical console. Having a physical button to acknowledge the alarms usually
implies an alarm list (typically an alarm banner 5 ) to be permanently
presented to the operator, and only the alarms visible on that list or the top
one will be acknowledged.

After the alarm is acknowledged, it must be still presented to the operator
whilst the associated alarm condition is active.

The acknowledge function might also silence the audible signal.

### 4.2 Silence

The only purpose of the silence functionality is to silence the audible
signals.

When sounders or buzzers are installed in multiple locations, usually a silence
button exists on each location. The scope of the silence functionality
associated with each location, i.e., if it only silences the audible signal on
that location or in all locations, will depend on the Client requirements or
how far those locations are from each other. However, in paragraph 2.3.8 of
Lloyd's Rukes[^4], is stated “*Acknowledgement of alarms at positions outside a
machinery space is not to silence the audible signal or extinguish the visual
indication in that machinery space.*”

Visual alarm indications must not be affected by the silence functionality.

## 5 ALARM DISPLAY

The alarms are usually presented to the operator using two separate lists:
*pending alarms* and *alarm historic*.

The term *pending* is associated with all the states other than NORMAL
presented in figure above. If the alarm state is not NORMAL, it implies that it
is pending on the process to become normal or on the operator to acknowledge
it. The pending alarms list shows the last of the blue transitions shown in the
figure above. Once the alarm state is NORMAL, it is no longer shown in the
pending alarms list.

The alarm historic shows all transitions shown in the figure above.

So that the operator can distinguish between the different alarm states, each
state must be presented differently. Marine classification societies have this
as a requirement for Alarm systems, and Lloyd’s Register even goes a step
further by stating in paragraph 2.3.9 of the rules[^4] “*Unacknowledged alarms
on monitors are to be distinguished by either flashing text or a flashing
marker adjacent to the text. A change of colour will not in itself be
sufficient to distinguish between acknowledged and unacknowledged alarms.*”

Below is show a suggestion of implementation for alarm lists. None of the
references listed in the References section specify how alarms must be
displayed.

![display](/assets/images/posts/2021-11-15-introduction-to-alarm-management/img0501.png)

## ALARM PRIORITIES

The alarm priority is an indication of the importance of an alarm and its
consequence on the process.

When multiple alarms are presented simultaneously to the operator, the alarm
priority allows the operator to select which ones to address based on the
respective severity.

As an exanmple, IMO[^2] specifies priorities the four alarm priorities listed
in table below.

<table class="post">
    <tr>
      <th class="post" width="130px">Priority</th>
      <th class="post">Definition</th>
    </tr>
    {% for entry in site.data.2021-11-15-post-data2 %}
      <tr>
        <td class="post">{{ entry.term }}</td>
        <td class="post">{{ entry.desc }}</td>
      </tr>
    {% endfor %}
</table>

Graphical Consoles or software packages used to implement Alarm Systems usually
have a numerical value associated with the alarm priority. However, no standard
or rules specify how the numerical value is related to the alarm severity.

The table below  presents an implementation suggestion.

<table class="post">
    <tr>
      <th class="post" width="80px">Priority</th>
      <th class="post">Definition</th>
    </tr>
    {% for entry in site.data.2021-11-15-post-data3 %}
      <tr>
        <td class="post">{{ entry.term }}</td>
        <td class="post">{{ entry.desc }}</td>
      </tr>
    {% endfor %}
</table>

---
[^1]: Management of alarms systems for the process industries, IEC 62682

[^2]: Codes on Alerts and Indications, International Maritime Organization, December 2009

[^3]: Rules for Classification – Ships, Part 4. Chapter 9, Section 3, DNV, January 2018

[^4]: Rules and Regulations for the Classification of Ships, Lloyd’s Register, July 2021

