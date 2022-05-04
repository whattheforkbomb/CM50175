# What to Develop
## User Interface Adaptive to User Perspective
Essentially UI that changes what is shown by user perspective to screen.

**Possible Interpretations:**
- Interface is effectively a 3D space, and adjusting face / phone pose changes what is rendered
- Changing pose adjusts the UI, e.g. photos app, looking down from above reveals photo meta-data<br>
Presumably this is more a gesture, as maintaining a non-face-on perspective to review information on a screen is awkward, and reduces relative size of screen
- Bit of both, within apps you get more gesture focused adaptiveness, while the 'home' is 3D, and is more akin to looking around within a room (the screen being the window).

# Evaluation
## User Studies
### Preliminary review of gesture comfortableness
Prior to final prototypes, can send out some forms to get feedback on:
- Gestures based on perspective, what actions make sense (or UI changes) based on perspective<br>
Think which viewing angle makes sense for viewing photo metadata.
- Gesture confirmation delay (is it better to have an indicator to say what gesture it thinks you're doing, and a delay before actioning it to avoid accidental gestures)<br>
e.g. how long looking down from above to display the photo metadata information
- Whether perspective should be held to view certain information/screens, or if should just be used to toggle
- How easy is it to adapt to a 3D UI, such that you can move phone/face to see around the UI space, move towards/away from phone to adjust depth, etc...
- How often are they in a scenario where touch capabilities are reduced (e.g. only have one hand free, wearing gloves, etc...)

### Final Prototype Usability Study
Once final prototype exists, hand to users to perform certain tasks or to use for few days to provide feedback.
- Quantitative
    - How easy to adapt/how intuitive (maybe SAM)
    - Preference of touch vs face/gaze
    - Time taken to perform common tasks Vs touch input (what tasks?)
- Qualitative
    - limitations they faced
    - times it was better than touch (if any)
    - times better than single-touch (if any)
    - times out-performed by touch (if any)
- Compare evaluations with preliminary research (do they match, was any feedback contradictory?)

## Evaluation of Approach/Final Prototype
Compare with existing approaches (if available).

Evaluation of performance:
- Hardware Requirements
    - battery usage, or ideally computation requirements
    - time taken to process an image (e.g. frame-rate) and resolution used
- Accuracy
    - Correct head/phone pose
    - gesture recognition success rate

Limitations with system?

# Approach Notes
## Tracking phone orientation w/r/t user's face:
**How?**
- Motion Correlation of Front Facing Camera (FFC)?
    - Issue with this is that how to tell if movement or rotation?
    - Additionally when phone orbiting the user's face, no movement would be tracked, so either we don't accept that motion, or we infer from IMU, or we isolate user's face and perform motion correlation on background to determine movement?
        - Then do motion correlation on face to determine movement with respect to head?
- Tracking face via FFC (use existing ML models to identify region with user's face, or even just haar cascade), then inferring relative position of phone to user's face.
- AR methods (possibly using rear camera also)

Is it worth comparing tracking just face pose w/r/t phone (existing approaches) Vs tracking phone pose w/r/t face?

Can review orientation & position of face within phone camera view, but do we want a distinction between user moving their face vs the phone?<br>
Using the adaptive user interface (w/r/t user's perspective), do we want the position of the phone in 3D space to be distinct from the user's pose?<br>
e.g. moving the phone upwards reveals a different app, moving the head permits exploring within the app?

We can determine phone orientation via IMU and thus determine if phone being rotated vs user's head. Lateral movement is more difficult, though could use linear velocity/acceleration from IMU as indicator as to whether phone is moving or is just user's head moving in frame?

## Require gaze tracking:
Actions/gestures only make sense if user is viewing the phone screen while the phone or their face is moving.

Don't want to respond to potential 'gestures' that are result of user reacting to external stimuli, such as moving phone out of the way, or facing someone out-of-frame.

Gaze w/r/t specific regions of the screen can also be utilised to aid in some gestures potentially.

## Machine Learning:
Possible places it could be used:
- Face identification (probably just use existing implementation, no need to re-invent the wheel)
- Gesture recognition
- Noise removal/reduction (though may be able to filter this out or reduce with existing methods)
