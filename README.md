# MPC ONE Midi Protocol
Listed in this document are my finding for how to comunicate with the MPC ONE via MIDI while it is in the Control Mode.

## Activation - Ping/Pong
When activating control mode from the MPC, it presents a waiting screen that is waiting for Ableton live to communicate with the device using a special ping/pong type of SYSEX message. To activate the mode, a PING SYSEX message needs to be sent, and continually sent for the duration of the controller.

Here is a breakdown of SYSEX constants:
```python
SYSEX_MSG_HEADER = (240, 71, 0)
SYSEX_END_BYTE = 247
MPC_X_PRODUCT_ID = 58
MPC_LIVE_PRODUCT_ID = 59
FORCE_PRODUCT_ID = 64
SUPPORTED_PRODUCT_IDS = (MPC_LIVE_PRODUCT_ID, MPC_X_PRODUCT_ID, FORCE_PRODUCT_ID)
BROADCAST_ID = 127
PING_MSG_TYPE = 0
PONG_MSG_TYPE = 1
TEXT_MSG_TYPE = 16
COLOR_MSG_TYPE = 17
PING_MSG = SYSEX_MSG_HEADER + (BROADCAST_ID, PING_MSG_TYPE, SYSEX_END_BYTE)
INNER_MESSAGE_MAX_LENGTH = 251
NUM_MSG_IDENTIFYING_BYTES = 3
```
Note the PING_MSG. So the ping message would look like this in hex... *note the 3B in the message is the product ID. The correct product ID must be used for it to respond*
```
F0 47 00 3B 00 F7
```
To which the device should reply with the pong message:
```
F0 47 00 3B 01 F7
```

This constant ping/pong needs to be sent roughly every 1 second.

## Buttons
The buttons respond using Note On/Off MIDI messages *(Note Off is actually a NoteOn message with a velocity of 0).*

Here is a table of the available buttons on the MPC ONE. May very for MPC X, MPC LIVE, Akai Force. *note. values start from 0, ie channel are from 0-15*

| Name | Channel | Note | Note_HEX |
| ---  | ----   | ----| ----|
| REC  | 12 | 77 | 4D |
| OVER DUB  | 12 | 78 | 4E |
| STOP  | 12 | 79 | 4F |
| PLAY  | 12 | 80 | 50 |
| PLAY START  | 12 | 81 | 51 |
| PLAY START  | 12 | 81 | 51 |
| SHIFT | 12 | 72 | 48 |
| TAP TEMPO | 12 | 76 | 4C |
| UNDO | 12 | 74 | 4A |
| MINUS/PLUS/JOG | 12 | 6 | 06 |
| BANK A | 12 | 106 | 6A |
| BANK B | 12 | 107 | 6B |
| BANK C | 12 | 108 | 6C |
| BANK D | 12 | 109 | 6D |
| FULL LEVEL | 12 | 69 | 45 |
| 16 LEVELS | 12 | 70 | 46 |
| 16 LEVELS | 12 | 70 | 46 |
| COPY | 12 | 75 | 4B |
| ERASE | 12 | 71 | 47 |
| NOTE REPEAT | 12 | 68 | 44 |


## Pads
The Pads respond using Note On/Off MIDI messages *(Note Off is actually a NoteOn message with a velocity of 0).*

Here is a table of the available pads on the MPC ONE. May very for MPC X, MPC LIVE, Akai Force. *note. values start from 0, ie channel are from 0-15*

| Name | Channel | Note | Note_HEX |
| ---  | ----   | ----| ----|
| Pad1  | 12 | 56 | 38 |
| Pad2  | 12 | 57 | 39 |
| Pad3  | 12 | 58 | 3A |
| Pad4  | 12 | 59 | 3B |
| Pad5  | 12 | 48 | 30 |
| Pad6  | 12 | 49 | 31 |
| Pad7  | 12 | 50 | 32 |
| Pad8  | 12 | 51 | 33 |
| Pad9  | 12 | 40 | 28 |
| Pad10  | 12 | 41 | 29 |
| Pad11  | 12 | 42 | 2A |
| Pad12  | 12 | 43 | 2B |
| Pad13  | 12 | 32 | 20 |
| Pad14  | 12 | 33 | 21 |
| Pad15  | 12 | 34 | 22 |
| Pad16  | 12 | 35 | 23 |

## Qlink/Knobs
The Qlinks are touch sensitive, sending NoteOn/Off on touch and release, and sending signed CC values when turning. Clockwise is a value of 1, Counterclockwise is a value of 127. The QLink cycle button can be used to bank through a total of 16 qlinks.

Here is a table of the available Qlinks on the MPC ONE. May very for MPC X, MPC LIVE, Akai Force. *note. values start from 0, ie channel are from 0-15*

| Name | Channel | CC | CC_HEX |
| ---  | ----   | ----| ----|
| Qlink1  | 13 | 0 | 00 |
| Qlink2  | 13 | 1 | 01 |
| Qlink3  | 13 | 2 | 02 |
| Qlink4  | 13 | 3 | 03 |
| Qlink5  | 13 | 4 | 04 |
| Qlink6  | 13 | 5 | 05 |
| Qlink7  | 13 | 6 | 06 |
| Qlink8  | 13 | 7 | 07 |
| Qlink9  | 13 | 8 | 08 |
| Qlink10  | 13 | 9 | 09 |
| Qlink11  | 13 | 10 | 0A |
| Qlink12  | 13 | 11 | 0B |
| Qlink13  | 13 | 12 | 0C |
| Qlink14  | 13 | 13 | 0D |
| Qlink15  | 13 | 14 | 0E |
| Qlink16  | 13 | 15 | 0F |

# Touch User Interface(TUI) Screen Controls
The controls on the screen also send midi values. In the next section we'll cover the screen controls for each of the three screens available in control mode.

## Transport / Header
The top header has transport controls that send data. Here is a table of the available controls on the MPC ONE. May very for MPC X, MPC LIVE, Akai Force. *note. values start from 0, ie channel are from 0-15*

| Name | Channel | Note | Note_HEX |
| ---  | ----   | ----| ----|
| PhaseDown  | 10 | 12 | 0C |
| PhaseDown  | 10 | 13 | 0D |
| Metronome  | 10 | 0 | 00 |
| Follow  | 10 | 8 | 08 |
| Overdub  | 10 | 3 | 03 |
| AutomationArm  | 10 | 4 | 04 |
| Loop  | 10 | 5 | 05 |

## Session Controls

### Pad Matrix
The pad matrix is an 8x8 matrix of "Cells". They respond to touch using note on and note off values. 

Here is a table of the matrix. The values in the matrix are the midi note that the cells respond to. 


| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | Scenes |
| -- | -- | -- | -- | -- | -- | -- | -- | -- |
| 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 88 |
| 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 89 |
| 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 90 |
| 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 91 |
| 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 92 |
| 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 93 |
| 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 94 |
| 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 95 |
| -- | -- | -- | -- | -- | -- | -- | -- | -- |
| 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | Ch10 20 |


### Session Touch Buttons
The Session Touch buttons respond using Note On/Off MIDI messages *(Note Off is actually a NoteOn message with a velocity of 0).*

Here is a table of the available buttons on the MPC ONE. May very for MPC X, MPC LIVE, Akai Force. *note. values start from 0, ie channel are from 0-15*
 
| Name | Channel | Note | Note_HEX |
| ---  | ----   | ----| ----|
| Quantize  | 10 | 16 | 10 |
| Delete  | 10 | 14 | 0E |
| Insert Scene  | 10 | 21 | 15 |
| REC  | 10 | 22 | 16 |


## Mixer Controls
The mixer window shows eight channel strip. Each channel strip communicates on it's own channel and consist of the following controls along with their MIDI message types:

* Fader: CC 0
* Pan: CC 1
* Mute: NoteOn/Off: 1
* Solo: NoteOn/Off: 0
* RecArm: NoteOn/Off: 5
* Knob1: CC 3
* Knob2: CC 4
* Knob3: CC 5
* Knob4: CC 6
* A/B Mix: NoteOn/Off, 1 & 2

Each channel strip communicates on a different MIDI channel. So the first fader in the window will send a CC message on channel 2(1 in code), CC #0, 

Here are the channel strip to MIDI channel mapping

* Channel Strip 1: MIDI Ch 2(1 in code)
* Channel Strip 2: MIDI Ch 3(2 in code)
* Channel Strip 3: MIDI Ch 4(3 in code)
* Channel Strip 4: MIDI Ch 5(4 in code)
* Channel Strip 5: MIDI Ch 6(5 in code)
* Channel Strip 6: MIDI Ch 7(6 in code)
* Channel Strip 7: MIDI Ch 8(7 in code)
* Channel Strip 8: MIDI Ch 9(8 in code)

## Device Controls
Navigating to the device window gives you 8 slider controls, along with device, bank, and active buttons.

### Parameter Display Enabled
To enable one of the 8 parameters, a NoteOn/Off message is sent using the index of the param(0-7) + 112. Sending a note on value will show the parameter. Sending a note off message, or a note on message of zero will disable the parameter. When a parameter is enabled, it is shown, when it is disabled, it is not shown.

| Parameter | Channel | Note | Note_hex |
| -- | -- | -- | -- |
| Param1 | 9 | 112 | 70 |
| Param1 | 9 | 113 | 71 | 
| Param1 | 9 | 114 | 72 | 
| Param1 | 9 | 115 | 73 | 
| Param1 | 9 | 116 | 74 | 
| Param1 | 9 | 117 | 75 | 
| Param1 | 9 | 118 | 76 | 
| Param1 | 9 | 119 | 77 | 


### Parameter Slider Values.
The parameter slider values send data on CC data on channel 10(9 in code). The CC numbers start at zero and go up to 7. They also receive feedback on the same channel and CC numbers. Listed below is a table of the parameter and CC values.

Each slider sends CC messages and the buttons send NoteOn/Off messages

Here is a table representing the sliders

| Name | Channel | CC | CC_HEX |
| ---  | ----   | ----| ----|
| Slider1  | 9 | 0 | 00 |
| Slider2  | 9 | 1 | 01 |
| Slider3  | 9 | 2 | 02 |
| Slider4  | 9 | 3 | 03 |
| Slider5  | 9 | 4 | 04 |
| Slider6  | 9 | 5 | 05 |
| Slider7  | 9 | 6 | 06 |
| Slider8  | 9 | 7 | 07 |


### Device Buttons
Here is a table representing the Device Buttons
| Name | Channel | Note | Note_HEX |
| ---  | ----   | ----| ----|
| DeviceOn  | 9 | 0 | 00 |
| Device-  | 9 | 1 | 01 |
| Device+  | 9 | 2 | 02 |
| Bank-  | 9 | 3 | 03 |
| Bank+  | 9 | 4 | 04 |



# Display Feedback
Feedback can be sent to the display using MIDI SYSEX messages. Various text, colors, button type can be controlled visually this way. In this section we will explore how to send visual feedback to the display.

## Text
Text can be sent to the controls on the display using a sysex message. The SYSEX format to send text to the display is as follows

```python
HEADER + PRODUCT_ID + TEXT_MSG_TYPE + CONTROL_PAGE + CONTROL_ID + ASCII_DATA + END_BYTE
```
### Description
* Header: SYSEX HEADER. Starts with SYSEX start byte followed by manufacture ID and model (240, 71, 0)
* PRODUCT ID: This identifies the product. In this case it needs to match the product, Akai Force, MPC Live, MPC X
* TEXT_MSG_TYPE: This signifies you are sending a text message type of message.
* CONTROL_PAGE: This identifies the page the control resides. These correspond to the pages that can be selected via touch, Session, Mixer, Device, Transport.
* CONTROL_ID: This identifies which control to send text to.
* MSG_METADATA: This is MSB LSB of the message length. It is two bytes to potentially represent values larger than 127
* ASCII_DATA: This is the actual text characters encoded as ASCII
* END_BYTE: The universal sysex end BYTE F7

### Control Pages
Here is a list of the Pages with the corresponding numbers

| Name | ID |
| ---  | ----   | 
| Session  | 0 |
| Mixer  | 1 |
| Device  | 2 |
| Transport  | 2 |



An actual message (HEX format) to send hello would look like this

```
HEADER      PRODUCT_ID      TEXT_MSG_TYPE   CONTROL_PAGE   CONTROL_ID   MSG_METADATA      ASCII_DATA               END_BYTE
F0 47 00    3B              10              00             00           00 05            48 65 6C 6C 6F           F7 
```

### Message Metadata
Here are some functions to help generate the metadata in Python

```python
def clamp(val, minv, maxv):
    return max(minv, min(val, maxv))

def message_length(message) -> tuple:
    length = len(message)
    return (
     clamp(length // 128, 0, 127), length % 128)
```



### Message Text to Ascii
Here is a translation table from text to ASCII encoding in Python 

```python
ascii_translations = {'0':48, 
     '1':49, 
     '2':50, 
     '3':51, 
     '4':52, 
     '5':53, 
     '6':54, 
     '7':55, 
     '8':56, 
     '9':57, 
     'A':65, 
     'B':66, 
     'C':67, 
     'D':68, 
     'E':69, 
     'F':70, 
     'G':71, 
     'H':72, 
     'I':73, 
     'J':74, 
     'K':75, 
     'L':76, 
     'M':77, 
     'N':78, 
     'O':79, 
     'P':80, 
     'Q':81, 
     'R':82, 
     'S':83, 
     'T':84, 
     'U':85, 
     'V':86, 
     'W':87, 
     'X':88, 
     'Y':89, 
     'Z':90, 
     'a':97, 
     'b':98, 
     'c':99, 
     'd':100, 
     'e':101, 
     'f':102, 
     'g':103, 
     'h':104, 
     'i':105, 
     'j':106, 
     'k':107, 
     'l':108, 
     'm':109, 
     'n':110, 
     'o':111, 
     'p':112, 
     'q':113, 
     'r':114, 
     's':115, 
     't':116, 
     'u':117, 
     'v':118, 
     'w':119, 
     'x':120, 
     'y':121, 
     'z':122, 
     '@':64, 
     ' ':32, 
     '!':33, 
     '"':34, 
     '.':46, 
     ',':44, 
     ':':58, 
     ';':59, 
     '?':63, 
     '<':60, 
     '>':62, 
     '[':91, 
     ']':93, 
     '_':95, 
     '-':45, 
     '|':124, 
     '&':38, 
     '^':94, 
     '~':126, 
     '`':96, 
     "'":39, 
     '%':37, 
     '(':40, 
     ')':41, 
     '/':47, 
     '\\':92, 
     '*':42, 
     '+':43}

```

## TUI Track Buttons
The track buttons can display color, text, icon, and state.

The State of the Button is show using NoteOn/OFF
The Color of the Button is show using CC color indices
The Icon type is selected by NoteOn/Off.

### Track Button State
There are 8 Track buttons. Each has and index from 0-7. Send MIDI NoteOn/Off values to control state. The MIDI Note is the index of the Button + 8, the state to display is sent via velocity.

#### Track Button State Map
| State | Velocity |
| --    | ----     |
| Empty    | 0     |
| MIDI | 1 |
| Drum | 2 |
| Plain | 3 |
| Device | 4 |
| Audio | 6 |
| Folder | 7 |
| Crown | 9 |

## Track Select
To put a track in a selected state, A NoteOn/Off message is sent using the track index as the Midi note, and the selected state as the velocity

| State | Velocity |
| --    | ----     |
| Default   | 0     |
| Selected | 1 |


### Track Names
Track names are via the text protocol. Here is the table of the track names:

| Track | CONTROL_PAGE| CONTROL_ID |
| --- | --- | --- |
| Track1 | 0 | 0 |
| Track2 | 0 | 1 |
| Track3 | 0 | 2 |
| Track4 | 0 | 3 |
| Track5 | 0 | 4 |
| Track6 | 0 | 5 |
| Track7 | 0 | 6 |
| Track8 | 0 | 7 |


<!-- DEVICE PAGE -->

## TUI Device Page
The device page is used to display device parameters in a horizontal slider fashion with values and text labels. All this can be controled using MIDI.
The screen is able to show 8 parameters per page. 

| Slider #  | Channel | CC Number |
|--|--|--|
| Slider1 | 9 | 0 |
| Slider2 | 9 | 1 |
| Slider3 | 9 | 2 |
| Slider4 | 9 | 3 |
| Slider5 | 9 | 4 |
| Slider6 | 9 | 5 |
| Slider7 | 9 | 6 |
| Slider8 | 9 | 7 |

### Device Parameter Names
The parameter name can be set by using the above mentioned text formula. The Control_page is 2 and the Control_id is the index of the control + 16. Here is a table of the Device Parameter names.

| Param Label | Control_Page | Control_ID |
| -- | -- | -- |
| ParamLabel1 | 2 | 16 |
| ParamLabel2 | 2 | 17 |
| ParamLabel3 | 2 | 18 |
| ParamLabel4 | 2 | 19 |
| ParamLabel5 | 2 | 20 |
| ParamLabel6 | 2 | 21 |
| ParamLabel7 | 2 | 22 |
| ParamLabel8 | 2 | 23 |

### Device Parameter Values
The parameter name can be set by using the above mentioned text formula. The Control_page is 2 and the Control_id is the index of the control + 32. Here is a table of the Device Parameter names.

| Param Value | Control_Page | Control_ID |
| -- | -- | -- |
| ParamValue1 | 2 | 32 |
| ParamValue2 | 2 | 33 |
| ParamValue3 | 2 | 34 |
| ParamValue4 | 2 | 35 |
| ParamValue5 | 2 | 36 |
| ParamValue6 | 2 | 37 |
| ParamValue7 | 2 | 38 |
| ParamValue8 | 2 | 39 |

### Device Name
The device name can be controlled using sysex text protocol using a control_page of 2 and a control id of 1

### Device Bank Name
The device bank name can be controlled using sysex text protocol using a control_page of 2 and a control id of 0

### Device Number LEDs
At the bottom of the devices page are a series of white rectangles that show the amount of devices. This number can be controlled by send CC values on Channel 10(9 in code) on CC 16. The Currently selected rectangle is controlled using CC messages on Channel 10(9 in code) on CC 17.

### Device Active LED
On the device page, a yellow cirlce is displayed at the left of the device window. It can be controlled to show the active state of the device using NoteOn/Off # 0, Channel 10(9 in code). 

### Device Lock Icon
The device lock icon can be controlled using Midi NoteOn/Off Note#11(10 in code) on Channel 11(10 in code).
<!-- MIXER -->
## TUI Mixer
The mixer also has feedback controls.

### Meters / Sends
The meters for each channel can be controlled by sending MIDI CC value from 0-127 on CC 124/125 for left and right. The Channel Number determines the Channel strip meter that will be adjusted from channel. See the above channelstrip to channel mappings for reference

| Meter | CC Number |
| --    | ----     |
| Volume | 0 |
| Pan | 1 |
| Knob1 | 3 |
| Knob2 | 4 |
| Knob3 | 5 |
| Knob4 | 6 |
| Left RMS | 124 |
| Right RMS| 125 |
| Left Peak   | 126 |
| Left Peak   | 127 |

### Track Buttons
The track buttons also have state that can be manipulated by sending NoteOn/Off values. These buttons are bipolar representing only two states, on, and off. The Button to adjust is selected by MIDI Note and the state is selected by velocity.

| Button State | Value |
| --    | ----     |
| Off | 0 |
| Onn | 1 |

| Button Name | MIDI Note |
| --    | ----     |
| Solo | 0 |
| Muted | 1 |
| Disabled | 2 |
| RecArmed | 5|


## Clip

### Clip State
By default, the clips do not show. To control the clip state(ie the display of the clips), NoteOn/Off messages are sent to the device. Each clip is identified by an integer midi note, the note velocity selects the state. Here is a state to velocity mapping

* ClipEmpty = 0
* ClipEmptyWithStopButton = 1
* ClipStopped = 2
* ClipTriggeredPlay = 3
* ClipStarted = 4
* ClipTriggeredRecord = 6
* ClipRecording = 7
* ClipSelected = 127


### Clip Colors
A color can be assigned to a clip by sending CC values on channel one. The CC number is the same as the Clip ID and the color is selected by the CC value. The color is persistent.

Here is a mapping of the color indecies to RGB values
 
| Index  | R,G,B |
|--|--|
| 0  | 127, 74, 83 |
| 1  | 127, 82, 20 |
| 2  | 102, 76, 19 |
| 3  | 123, 122, 62 |
| 4  | 95, 125, 0 |
| 5  | 13, 127, 23 |
| 6  | 18, 127, 84 |
| 7  | 46, 127, 116 |
| 8  | 69, 98, 127 |
| 9  | 42, 64, 114 |
| 10 | 73, 83, 127 |
| 11 | 108, 54, 114 |
| 12 | 114, 41, 80 |
| 13 | 127, 127, 127 |
| 14 | 127, 27, 27 |
| 15 | 123, 54, 1 |
| 16 | 76, 57, 37 |
| 17 | 127, 120, 26 |
| 18 | 67, 127, 51 |
| 19 | 30, 97, 0 |
| 20 | 0, 95, 87 |
| 21 | 12, 116, 127 |
| 22 | 8, 82, 119 |
| 23 | 0, 62, 96 |
| 24 | 68, 54, 114 |
| 25 | 91, 59, 99 |
| 26 | 127, 28, 106 |
| 27 | 104, 104, 104 |
| 28 | 113, 51, 45 |
| 29 | 127, 81, 58 |
| 30 | 105, 86, 56 |
| 31 | 118, 127, 87 |
| 32 | 105, 114, 76 |
| 33 | 93, 104, 58 |
| 34 | 77, 98, 70 |
| 35 | 106, 126, 112 |
| 36 | 102, 120, 124 |
| 37 | 92, 96, 113 |
| 38 | 102, 93, 114 |
| 39 | 87, 76, 114 |
| 40 | 114, 110, 112 |
| 41 | 84, 84, 84 |
| 42 | 99, 73, 69 |
| 43 | 91, 65, 43 |
| 44 | 76, 65, 53 |
| 45 | 95, 93, 52 |
| 46 | 83, 95, 0 |
| 47 | 62, 88, 38 |
| 48 | 68, 97, 93 |
| 49 | 77, 89, 98 |
| 50 | 66, 82, 97 |
| 51 | 65, 73, 102 |
| 52 | 82, 74, 90 |
| 53 | 95, 79, 95 |
| 54 | 94, 56, 75 |
| 55 | 61, 61, 61 |
| 56 | 87, 25, 25 |
| 57 | 84, 40, 24 |
| 58 | 57, 39, 32 |
| 59 | 109, 97, 0 |
| 60 | 66, 75, 15 |
| 61 | 41, 79, 24 |
| 62 | 5, 78, 71 |
| 63 | 17, 49, 66 |
| 64 | 13, 23, 75 |
| 65 | 23, 41, 81 |
| 66 | 49, 37, 86 |
| 67 | 81, 37, 86 |
| 68 | 102, 23, 55 |
| 69 | 30, 30, 30 |

### Clip Names/Text
Clip names/text can be set using the text protocol.

The CONTROL_PAGE for the clips is 0.

The CONTROL_ID can be calculated by starting at 16. The grid is 8x8, and the CONTROL_ID increases by one to the next one in the row. 

Here is a table of the CONTROL_IDs

| -- | -- | -- | -- | -- | -- | -- | -- |
| -- | -- | -- | -- | -- | -- | -- | -- |
| 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 |
| 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
| 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 |
| 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 |
| 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 |
| 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 |
| 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 |


### Scene Names
Scene names can be set using the text Protocol. The CONTROL_PAGE is 0. Here is a table of the CONTROL_IDs

| Scene | CONTROL_ID |
| ----- | --- |
| Scene1 | 80 |
| Scene2 | 81 |
| Scene3 | 82 |
| Scene4 | 83 |
| Scene5 | 84 |
| Scene6 | 85 |
| Scene7 | 86 |
| Scene8 | 87 |


## Transport Feedback

### Transport Buttons
| Name | Channel | Note | Note_HEX | Notes |
| ---- | ------- | ---- | ---- | --- |
| Metronome | 10 | 0 | 00 |
| Overdub | 10 | 3 | 03 |
| Automation | 10 | 4 | 04 |
| LaunchQuantize | 10 | 6 | 06 | Velocity values determine bars amounts |
| Follow | 10 | 8 | 08 |
| PhaseDownButton | 10 | 12 | 0C |
| PhaseUpButton | 10 | 13 | 0D |
