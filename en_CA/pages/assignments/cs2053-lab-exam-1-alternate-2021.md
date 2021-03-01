# CS 2053 Lab Exam 1 - Alternate - Winter 2021

The requirements in this lab have been ordered in suggested order of completion.

## Overview of the Lab Exam
Your goal is to create a simple game, and is designed for you to demonstrate your ability in creating the basic elements of a game using Unity.

While the activity is not particularly "difficult", you are free to use whatever resources you would like including your previous labs to assist you. Including the [Unity Documentation](https://docs.unity3d.com/2019.1/Documentation/Manual/index.html).

You have 1.5 hours (10am-11:30am) to complete the lab and push it back to GitHub. So, it would be best to allocate some time test your code and commit and push it.

## Submission
You need to complete and submit your code before the 1.5 hour deadline. To submit your code you will make sure your project and scenes are saved. 

You must be in the Teams call during the lab exam, which is mandatory. If you are not in attendance then your lab exam will not be graded.

## Requirements
Note the requirements below have been organized according to a suggested logical flow for implementation. You may want to review them all before you start, however. NOTE: It would be good practice to make sure that all of the objects you create have a RigidBody component added to them.

- DEMO OF THE COMPLETED GAME: https://www.youtube.com/watch?v=lCgSLSlXJUQ

 1. *3 points* - The game takes place on a board, a simple plane that contains three cylinders as computer controlled players placed on the top of the plane. You can place the three cylinders in predetermined positions. The game board should have a color, while the cylinders can remain white. The game uses a fixed camera, positioned appropriately to show the game board.  
 2. *6 points* - Create a rectangular cube to represent the player, placed on the board. You control the player by using the arrow keys (which calculates a new position for the rectangular cube). The arrow keys will stop responding when the player gets close to the edge of the board. 
 3. *3 points* - The game contains a ball, placed somewhere on the board (it can start at the same place). The ball will move when bumped (remember to add a RigidBody to all objects in the game). The game also contains a goal (a large rectangular cube), placed just off the edge on one side of the board. The ball can remain white, but the goal should be colored red.
 4. *3 points* - The game can be restarted at any time by pressing 'r' on the keyboard. Although there does NOT need to be a message that states this.
 4. *7 points* - When the ball falls off (or hits the edge of) the game board, it immediately disappears and "You Lose!" is displayed on the screen. The game should prevent the player from moving at this point.
 6. *7 points* - When the ball is pushed into the goal it immediately disappears and "You Win!" is displayed on the screen.
 7. *6 points* - The computer players (cylinders) move in random directions around the game board (either forward, backwards, left or right), but they stay on the game board. Their movement does not have to be perfect, but should be reasonable and allow the game to be played (i.e., minor glitches are OK and might arise from changing the position of the object and it colliding with other objects)
  
**Total Points: 35**
 
 

## Helpful Code 
 ```cs
  // generate a random float between -4.0 and 4.0
  Random.Range(-4.0f, 4.0f);
```