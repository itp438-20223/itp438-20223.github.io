---
title: Midterm
has_children: false
nav_order: 3
has_toc: false
---

# Midterm Exam

The Midterm Exam is due at ***11:59PM*** on ***Tuesday, October 11***. This deadline will be strictly enforced. Late submissions will not be accepted.

If you would like clarification on something, please make a ***private*** post on Piazza with your question.

## Exam Honor Statement

The USC SCampus Student Conduct Code states in section 11.13.A:

> Any use or attempted use of external assistance in the completion of an academic assignment and/or during an examination, or any behavior that defeats the intent of an examination or other classwork or assignment, shall be considered academically dishonest unless expressly permitted by the instructor. The following are examples of unacceptable behaviors: communicating with fellow students during an exam, copying or attempting to copy material from another student’s exam; allowing another student to copy from an exam or assignment; possession or use of unauthorized notes, calculator, or other materials during exams and/or unauthorized removal of exam materials.

### Exam Rules

You ***MAY NOT*** collaborate with anyone else on this exam. This means you cannot talk to anyone else about the content of this exam until the exam period is over. This is an ***individual*** take home exam, and you are expected to work on this individually. The goal of this exam is to see what you could do on your own, not what you could do with the help of other parties.

You ***ARE*** allowed to consult the following resources while working on the exam:

- Notes including any slides from the class
- The flipped classroom and lecture recordings
- Your own code/blueprints from the programming assignments and flipped classrooms
- The Unreal Engine documentation ([https://docs.unrealengine.com/5.0/en-US/](https://docs.unrealengine.com/5.0/en-US/))
- The course Piazza for the sole purpose of getting clarification on the exam

You ***MAY NOT*** consult other online resources (such as blogs, Discord channels, tutorials, etc) while working on the exam.

### Affirmation

By submitting this exam, you affirm you have read the above excerpt from the Student Conduct Code as well as the exam rules and you will follow them. If you fail to abide by these rules, you will be reported to SJACS with a recommended sanction of an F in the course.

## Part 1 - 40 points

In this part, you will add a "prone" state to the character in your TopDown game.

The requirements are:

- The `X` key should toggle the character between prone and standing. So for example, if the player is standing and you press `X`, the player will switch to prone. Then if you press `X` again, the player will go back to standing.
- When prone, the player should not be able to move. Also, if the player is moving when they switch to prone, they should stop moving.
- For the purposes of the exam, you do not have to worry about transitions between prone and crouching.
- For the prone animations, you'll use three animations from the AnimStarterPack which should already be retargetted to the new mannequin and in your project's Content folder:
  - When transitioning from standing to prone, use `Stand_To_Prone` (Retargeted)
  - While prone, use `Prone_Idle` (Retargeted)
  - When transitioning from prone to standing, use `Prone_To_Stand` (Retargeted)
- Because the transition animations are fairly short, you may have to adjust the condition for when they'll transition so that the animations don't noticeably pop

*Hint*: There is no existing logic for a "prone" state in Character/CMC. You don't have to implement a custom CMC, and instead can just add necessary logic/variables to `ATopDownCharacter`.

Once implemented, switching to/from prone should look like this:

<video style="display:block; margin: 0 auto;" width="640" height="360" controls>
  <source src="assets/Midterm1.mp4" type="video/mp4">
</video>

### Submission Requirements

In your grading `midterm` folder make a `part1` sub-folder. Please submit the following:

- A video demonstrating that:
  - The player can transition to and from prone
  - Make sure at least once you show the player walking before switching to prone (to demonstrate the player stops moving)
  - Make sure you show the player facing in different directions when they go prone, don't always just show one
  - (Your video can be similar to the one above)
- A screenshot of the default animation graph, showing the high-level view of any new states added
- ***Any source files*** you modified to implement the prone logic. You don't need to submit any modified ini files, we just want the .h and .cpp files you changed.

Please double-check that you submitted the required files!

## Part 2 - 60 points

This part cannot be fully completed without implementing Part 1, although though you can complete _most_ of it even if you don't implement Part 1.

In this part, you will add a stealth indicator to the UMG HUD which gives feedback to the player as to how stealthy they are. This indicator will show at the top middle of the screen as an asterisk `*` which will change colors based on your threat level.

The threat levels are from highest to lowest:

- Purple
- Red
- Yellow
- Green

You need to update the threat level every frame. The threat level should be computed as follows:

- By default, the character's threat level is red
- If the character is currently moving ***OR*** moved within the last 0.5 seconds, the threat level should increase by one tier
- If the character is currently crouching, the threat level should decrease by one tier
- If the character is currently prone, the threat level should decrease by one tier
- If the character is currently inside ***one or more*** stealth trigger volume, the threat level should decrease by one tier. *Note that you will have to implement these stealth trigger volumes and place them in the level!*

This means that, for example, if the player is just walking around their threat level would be purple (+1 threat). On the other hand, if the player were crouched and standing still inside a stealth trigger, their threat level would be green (-2 threat).

You ***must*** implement this indicator using UMG (you can add it to the existing `HUDWidget`).

*Hint*: The stealth trigger volume can be implemented in a manner very similar to how we did the blue color triggers earlier in the semester.

*Hint 2*: You can represent the threat level with an integer (although an `enum` is probably better quality code, you aren't required to use an `enum`).

*Hint 3*: Remember that if you set a gameplay timer with the same handle as a previously set one, it will reset the timer to the full duration.

Once implemented, this will look something like this:

<video style="display:block; margin: 0 auto;" width="640" height="360" controls>
  <source src="assets/Midterm2.mp4" type="video/mp4">
</video>

{:.note}
In my video, I rotated the "Light Source" so that the shadows are cast to the right, and then I placed a stealth trigger box where the shadow was to make it cooler. You aren't required to do this as long as we can understand where the stealth trigger is (for example you can mark it in some other way in the level).

### Submission Requirements

In your grading `midterm` folder make a `part2` sub-folder. Please submit the following:

- A video demonstrating that the threat level on the stealth indicator correctly updates. Specifically, you should show that:
  - When ***NOT*** in a stealth trigger:
    - Standing still, the indicator is red
    - Moving while standing, and 0.5s after stopping moves, the indicator is purple
    - Crouching while still, the indicator is yellow
    - Moving while crouching, and 0.5s after stopping movement, the indicator is red
    - When prone, the indicator is yellow
  - When ***IN*** a stealth trigger (try to make it clear in the level where the stealth trigger is):
    - Standing still, the indicator is yellow
    - Moving while standing, and 0.5s after stopping movement, the indicator is red
    - Crouching while still, the indicator is green
    - Moving while crouching, and 0.5s after stopping movement, the indicator is yellow
    - When prone, the indicator is green
- ***Any source files*** you modified to implement the prone logic. You don't need to submit any screenshots of blueprints, we just want the .h and .cpp files you changed.

Please double-check that you submitted the required files!

## Grading

There are no points for code quality, so you don't have to worry about matching Unreal Style exactly. The points breakdown is as follows:

### Part 1 (40 points)

- 10 points for basic changes to state machine
- 15 points for hooking up X to toggle on/off
- 10 points for making sure the player can't move when prone
- 5 points for making sure the animations don't pop when switching between standing/prone

### Part 2 (60 points)

- 20 points for setting up changes to HUD widget to show and update the indicator
- 20 points for calculating the correct threat level when not in a stealth trigger volume
- 20 points for adding the stealth trigger volumes, and calculating the correct threat level when in a stealth trigger

Remember that the deadline is strictly enforced, and there are no resubmissions once the deadline is over. There will only be regrading in the event of a grading error.
