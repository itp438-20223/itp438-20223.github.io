---
title: Final
has_children: false
nav_order: 4
has_toc: false
---

# Final Exam

The Final Exam is due at ***11:59PM*** on ***Friday, December 9***. This deadline will be strictly enforced. Late submissions will not be accepted.

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

## Instructions

For this exam, you will implement an "alternate fire" grenade launcher for the Multi game you've been working on this semester. The basic functionality is as follows:

- Clicking the right mouse button will fire a grenade
- Firing the grenade should play the same sound effects/animations as firing the regular projectile
- The grenade will explode 3 seconds after created. When the grenade explodes, it will play a sound effect and particle effect, and kill ***any*** characters within a certain radius
- You have a "cooldown" where if you fire a grenade, you can't fire another grenade for 5 seconds
- When the grenade is ***not*** on cooldown, the HUD should have a "GRENADE" text that's visible. When it's on cooldown, the text should be hidden
- Everything must work networked. You can test everything in PIE in Listen Server mode, but you must demonstrate that all functionality works correctly on both the listen server and the client

When done, the functionality will look like this:

<video style="display:block; margin: 0 auto;" width="640" height="360" controls>
  <source src="assets/Final.mp4" type="video/mp4">
</video>

Here are some implementation details and notes:

- *Hint*: You can leverage much of the existing firing code to get the grenade to fire, so try to think about how you can do that
- *Hint*: Before implementing this I suggest sketching out and thinking what should occur on the server and what will need to be replicated and/or use RPCs
- You ***MUST*** create a Actor subclass in C++ for the grenade called `AGrenade`
- *Hint*: I strongly recommend you use the MultiProjectile constructor as reference for how to setup the grenade class
- The `AGrenade` C++ class should have:
  - A `UCapsuleComponent` as the `RootComponent` with:
    - Set the Capsule Size to `(5.0f, 10.0f)`
    - Set the Body Instance's Collision Profile Name to `"Projectile"`
  - A `UProjectileMovementComponent` with:
    - `UpdatedComponent` is the root component
    - `InitialSpeed` = 2000
    - `MaxSpeed` = 2000
    - `bRotationFollowsVelocity` = false
    - `bShouldBounce` = true
    - `Bounciness` = 0.3f
    - `ProjectileGravityScale` = 1.5f
    - Needs to be setup to replicate and replicate movement
- For the blueprint subclass of your grenade, add a Cylinder static mesh as a child of the root component and set its scale to `(0.1, 0.1, 0.15)`
- When spawning the grenade, you should roll the grenade 90 degrees so it fires on its side
- When the grenade is fired, there's a 5 second cooldown that prevents the grenade from being fired again, and the HUD should update with the "Grenade" indicator as noted
- For the explosion that occurs 3 seconds after the grenade is spawned, all clients need to play the sound and particle effect:
  - For the sound effect, you can just use the same sound used for firing
  - For the explosion particle, add the free "M5 VFX Vol2. Fire and Flame" from the Marketplace through the Epic Games Launcher, and use the Fire_Exp_00 particle effect from that
  - *Hint*: If you don't remember how to play sound effects and particle effects from C++, take a look at how you did it in FireComponent back in TopDown
- When the explosion occurs, any MultiCharacters within 250.0f units of the center of the grenade should die (you can just do a simple radius check), this includes teammates and the instigator
  - Each enemy killed by an instigator's grenade still awards 1 point
  - Each teammate killed by an instigator's grenade awards 0 points
  - You don't have to worry about kill streaks for this
- 0.2 seconds after starting the explosion, destroy the grenade actor
- If you have any further questions or need clarification, please make a ***private*** post on Piazza.

### Submission Requirements / Grading

Please submit the following in your `final` grading folder:

- A video demonstrating the following:
  - When the listen server fires a grenade:
    - Show that you see the grenade on both the listen server and client (10 points)
    - Show that you see/hear the explosion on both the listen server and client (10 points)
    - Show that the listen server "GRENADE" HUD element disappears when the grenade is on cooldown (10 points)
    - Show that if the grenade explosion hits an enemy, that enemy dies and it adds 1 point to the score (10 points)
    - Show that if both yourself and the enemy are within the explosion range, both players die (and in total, 1 point gets added to the score) (10 points)

  - When the client fires a grenade:
    - Show that you see the grenade on both the listen server and client (10 points)
    - Show that you see/hear the explosion on both the listen server and client (10 points)
    - Show that the listen server "GRENADE" HUD element disappears when the grenade is on cooldown (10 points)
    - Show that if the grenade explosion hits an enemy, that enemy dies and it adds 1 point to the score (10 points)
    - Show that if both yourself and the enemy are within the explosion range, both players die (and in total, 1 point gets added to the score) (10 points)

- Your Grenade.h/cpp files
- ***Any source files*** modified to implement the grenade. You don't need to submit any content, we just want the .h and .cpp files you changed. You can consult Perforce if you're not sure which files you changed

Please double-check that you submitted the required files!

There are no points for code quality.

Remember that the deadline is strictly enforced, and there are no resubmissions once the deadline is over. There will only be regrading in the event of a grading error.
