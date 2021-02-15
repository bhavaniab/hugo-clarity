+++
author = "Bhavani Anantapur Bache"
title = "Gesture events in BREWMP"
date = "2015-07-15"
description = "Gesture events in BREWMP"
featured = true
tags = [
    "BREWMP",
    "RTOS",
    "Embedded Systems",
]
categories = [
    "BREWMP",
    "RTOS",
    "Embedded Systems",
]
series = ["Embedded Systems"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/gesture.jpg"
+++

Smart phones and a few of the feature phones have the touch screen support. When we tap on the touch screen of mobile device, we don't realize the time and effort spent by the developer in building the amazing apps. The apps seem to be working so seamlessly. In this post, I would like to discuss in detail the complex state machine that our team had designed and implemented to differentiate various gesture events in BREW mobile devices.

The BREW mobile devices are sensitive to a wide range of touch events. They are tap, tap and hold, double tap, flick, panning, and double touch. There are certain events which get triggered when a user taps on the screen of the mobile device. For each AEEEvent, the wParam and dwParam parameters (if any) are passed to the applet or control.
Some of the platform events or are as follows:
`````C++
EVT_POINTER_DOWN
EVT_POINTER_MOVE
EVT_POINTER_STALE_MOVE
EVT_POINTER_UP
EVT_POINTER_MT_MOVE
EVT_POINTER_MT_STALE_MOVE
EVT_POINTER_MT_DOWN
EVT_POINTER_MT_UP
`````
These platform events, have their own limitations when they are being deployed on real time applications.For example, when a user moves his finger slightly on the screen of the BREW mobile, three events are triggered, EVT_POINTER_DOWN,EVT_POINTER_MOVE and EVT_POINTER_STALE_MOVE. Since the touch observer is very sensitive to the movement of the pointer, sometimes EVT_POINTER_MOVE is triggered in case of a tap. Double tap events also trigger tap events as a double tap is a combination of two successive single taps. It is highly challenging to differentiate between, tap, double tap, tab and hold,panning and flick events. Therefore, user defined events are declared for the gesture events, and a state machine is designed.Following user defined states are declared:
`````C++
GESTURE_INVALID
TAP_PRESS_EVENT
TAP_RELEASE_EVENT
DOUBLE_TAP_EVENT
`````
The user defined events are:
`````C++
TAP_EVENT
TAP_N_HOLD_EVENT
PANNING_EVENT
FLICK_EVENT
PANNING_EVENT
`````
The entire state machine for differentiation of these events is described in terms of the functionality that is executed at each state.

<h3>GESTURE_INVALID</h3>
This is the start state. When the event EVT_POINTER_DOWN is received, the present state is changed to TAP_PRESS_EVENT. In case of any other event received, the state remains unchanged.

<h3>TAP_PRESS_EVENT</h3>

In this state,the TAP_N_HOLD_EVENT is differentiated from rest of the events.When the timer callback is called (the timer has expired), and the pointer is down, then the event is TAP_N_HOLD_EVENT. If the timer does not expire, curr_x, curr_y and curr_time are saved and the timer is reset. If EVT_POINTER_UP is received, there is a state change to the TAP_RELEASE_EVENT.

<h3>TAP_RELEASE_EVENT</h3>
In the present state, if the timer expires, then it is a TAP_EVENT. If the timer does not expire, the event can be a DOUBLE_TAP_EVENT, PANNING_EVENT or FLICK_EVENT. If EVT_POINTER_DOWN is received, then the state is changed to DOUBLE_TAP_EVENT and DOUBLE_TAP_EVENT is registered. The curr_x, curr_y and curr_y are updated.

`````C++
curr_x = x;
curr_y = y;
curr_time = time;
`````

<h3>PANNING_EVENT and FLICK_EVENT</h3>
When there is a EVT_POINTER_MOVE or EVT_POINTER_STALE_MOVE,the current state is changed to PANNING_EVENT if the following condition is met:

`````C++
(curr_x -prev_x) >= PANNING_PIXEL_MARGIN && (curr_y-prev_y) >= PANNING_PIXEL_MARGIN
`````

The curr_x, curr_y, prev_x and prev_y are now updated with the latest values.

`````C++
prev_x = curr_x;
prev_y = curr_y;
curr_x = x;
curr_y = y;
`````

In case an EVT_POINTER_UP is received, check is made whether PANNING_EVENT is a FLICK_EVENT. The speed at which the pointer is moved can be determined by:

`````C++
IObserver_GetLLMSpeed(pMe->piTouchObserver, &speed);
`````
If speed < 1000 PANNING_END_EVENT is registered and the current state is reset to the start state GESTURE_INVALID. If speed > MAX_SPEED_FOR_PANNING , then FLICK_EVENT is registered, otherwise PANNING_END_EVENT is registered. The variables representing the x and y positions are again reset.

`````C++
prev_x = curr_x;
prev_y = curr_y;
`````
