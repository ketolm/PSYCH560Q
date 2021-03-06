# Demo ACT-R: Building a device and a model for the Stroop task

This is some demo code for the ACT-R class; it shows how to build a
simple, interactive device for an ACT-R model, and it provides
a skeleton for a model.

## Device

The device is written in Python using the new, JSON-RPC based
interface for ACT-R 9 (currently in beta, coded as 7.6 or 7.x).

The device implements the version of the Stroop task used by Tim
Verstynen in his 2014 paper. In this version of the Stroop, words
appear in one of three colors (red, blue, and green), and the
responses are given by pressing one of three corresponding keys with
the right hand (index = red, middle = blue, ring = green). The task is
made of 120 trials: 42 congruent, 42 neutral, and 36 incongruent.

### Device implementation

The device is implemented as a Python object (`StroopTask`) that
controls an ACT-R experimental window. The experimental window is
ACT-R's predefined proxy for a screen, and comes with native support
to translated GUI elements (text, images, buttons, and lines) into a
predefined set of ACT-R chunks.

If ACT-R's Environment has been started, the window will be physically
visible, with the stimuli appearing in order. If ACT-R is running
without the Environment, the window will be hidden from the user (but
still visible for the model).


## Model

The model is a "response monkey", a basic model that simply clicks
at random when it sees a stimulus. The model continuously performs 
the following actions:

1. If the model is not looking at anything, it looks for a object 
   on the screen.
   
2. If the model is looking at a fixation cross, it does nothing and
   keeps looking.
   
3. If the model is looking at a stimulus (a word in red, green, or
   blue color), then it randomly responds with the index ("j"), the
   middle ("k") or the ring finger ("l").
   
4. If the model is looking at a black word that reads "done", it 
   stops ACT-R using the `!stop!` command.

## How to run the code

Simply type the following:

```
> import stroop
ACT-R connection has been started.
> stroop.run_experiment(time = 200)
     0.000   PROCEDURAL             CONFLICT-RESOLUTION
     0.050   PROCEDURAL             PRODUCTION-FIRED CHECK-THE-SCREEN
     0.050   PROCEDURAL             CLEAR-BUFFER VISUAL-LOCATION
     0.050   VISION                 Find-location
     0.050   VISION                 FIND-LOC-FAILURE
   ...	 
   163.905   VISION                 Encoding-complete VISUAL-LOCATION599-1 NIL
   163.905   VISION                 SET-BUFFER-CHUNK VISUAL TEXT784
   163.905   PROCEDURAL             PRODUCTION-FIRED DONE
   163.905   PROCEDURAL             CLEAR-BUFFER VISUAL
   163.905   ------                 BREAK-EVENT Stopped by !stop!
--------------------------------------------------------------------------------
congruent (N=42): Accuracy = 0.33, Response Times = 329.88 ms
neutral (N=42): Accuracy = 0.34, Response Times = 417.32 ms
incongruent (N=36): Accuracy = 0.31, Response Times = 329.31 ms
```
