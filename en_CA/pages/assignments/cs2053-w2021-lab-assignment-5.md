# 2053 Lab Assignment #5: Rotation and Lights 

In this lab assignment, you will gain experience with game object rotation in multiple axes, as well as using different light types.

## Due: 11:30pm, Monday., Feb. 22, 2021

## Video
[Example video of completed assignment](https://youtu.be/gLr3AsV9kjc)

## Background
Right-click all links and open in a new tab. Review the following materials (just for the specified parts of the larger document) before you get started:
 - [Step 8 of Lighting Tutorial: Ambient Lighting](https://learn.unity.com/tutorial/introduction-to-lighting-and-rendering#5c7f8528edbc2a002053b52f)
 - [Step 9 of Lighting Tutorial: Light Types](https://learn.unity.com/tutorial/introduction-to-lighting-and-rendering#5c7f8528edbc2a002053b530)
 - [LookAt](https://unity3d.com/learn/tutorials/topics/scripting/look)

## 1. Preliminary Steps
 - a) Your scene should start with a custom Skybox. You have been provided with a bunch of Skyboxes (see the folder in Assets). Choose a skybox you like and drag and drop it into your Scene.
 - b) Drag the JET 3D model from Models in Assets to the game scene. Change the name of the 3D game object to “Aircraft”. In inspector, reset the object’s position to <0,0,0> and rotate the object 90 degree about Y-Axis. 
 - c) Set the camera to be a child of the aircraft, and position it appropriately so it follows the aircraft.
 - d) Create a new script for “Aircraft” object with name "AircraftController”, and copy and paste the following code. Carefully read and understand the code.
 
```csharp
public class AircraftController : MonoBehaviour {
    float circleRadius;
    float circleSpeed;
    float circleAngle;
    float selfRotationSpeed;
    Vector3 lastDirection;

     void Start () {
        circleRadius = 10;
        circleSpeed = 0.5f;
        circleAngle = 0;
        selfRotationSpeed = 100;

        lastDirection = new Vector3(1, 0, 0);
        lastDirection.Normalize();
    }

    void Update() {
        circleAngle += circleSpeed * Time.deltaTime;

        circleAngle = (circleAngle + 360) % 360;

        float newPositionX = circleRadius * (float)Mathf.Cos(circleAngle);
        float newPositionZ = circleRadius * (float)Mathf.Sin(circleAngle);

        Vector3 newPosition = new Vector3(newPositionX, transform.position.y, newPositionZ);
        Vector3 newDirection = newPosition - transform.position;

        newDirection.Normalize();

        float rotationAngle = -Vector3.Angle(lastDirection, newDirection);
        transform.Rotate(Vector3.up, rotationAngle, Space.World);
        transform.Rotate(Vector3.forward, selfRotationSpeed * Time.deltaTime, Space.Self);

        transform.position = newPosition;
        lastDirection = newDirection;
    }
}
```
  - e) Run the program and observe how the Aircraft object moves along a circular path. It rotate around Space.World as it moves around the circle, and rotates around its own forward axis (Space.Self).
  - f) Finally, make some adjustments to the project's lighting setup:
    + Open the Lighting settings: Window -> Rendering ->Lighting Settings
    + Next change the Environment Lighting -> "Source" to "color"
    + And, set the Ambient Color to Black.
    + Run the program again to see the changes to the lighting.

## 2. Requirements
 - a) Make the following changes to the program to make the Aircraft object go around the circular path and self-rotate controlled by the keyboard keys:
    + Pressing the “up” key, the Aircraft will go forward along the circular path.
    + Pressing the “left” or “right” keys, the object will roll left or right.
    + Be sure to test your program after the change. 
    + Hint: Only one line of the code is responsible for rolling the aircraft, the other lines are for moving forward around the circle.
    + Hint: Once you do this change you will find your aircraft does not start on the circular path, but rather at (0,0,0). To get your aircraft in a valid position on it's path you will need to run the code for positioning it twice.

 - b) Add 3 obstacles on your aircraft's path. 
     + Each one should require your airplane to rotate differently.
     + Hint: A trick to find good positions for your obstacles is to do the following: 
       - run your game, get the aircraft on the circular path and then pause your game, at a position and rotation you would like for your obstacle.
       - Go to your aircraft and copy its transform values (In the Inspector, click the gear on Transform and "Copy Component"). 
       - Stop the game and go your Obstacle's Transform and paste in the values (click the gear on Transform and "Paste Component Values")
       - Now your Obstacle will start in this position. 
    
 - c) Add in collision handling.
     + When a collision happens (because the aircraft is not oriented properly to fit through the gap of the obstacle) the Aircraft should not be able to fly forward; however, it should be able to rotate. 
     + Add a BoxCollider to your Aircraft, increase the size of the Collider so that it is pretty close to the size of the Aircraft (it doesn't have to be perfect).
         * Remember to set the isTrigger check-box on your Aircraft.
         * Hint: use OnTriggerStay and OnTriggerExit, to set a flag noting whether forward movement is allowed.

 - d) There will be countdown time for going around the whole circle path. If the player takes more than the time limit, the player loses the game.
 - e) You can let the play go several rounds. Make each later round have shorter time limit to make it more difficult.
 - f) In addition to the above game play, you need to add and allow control over the following lighting options during game playing.
     + Pressing the “a” key to toggle the colors of the ambient light between red and black.
         * See the Unity Reference for: [ RenderSettings.ambientSkyColor](https://docs.unity3d.com/ScriptReference/RenderSettings-ambientSkyColor.html)
         *  ``` RenderSettings.ambientSkyColor = Color.black;```

     + Pressing the “d” key will toggle the directional light on or off. (enable/disable component)
         * Your scene should already have a directional light.
         * You may optionally change the settings and position of your directional light.
         
     + Create a new point light that has a green color. Pressing the “p” key to toggle it on or off.
         * Make sure the point light can reach 50% of game scene. 
         
     + Create a spot light with a blue color. Put it in the center of the game scene. Make the light rotate to always cover the moving aircraft object. Press "s" key to toggle the light on or off.
         * See: [Transform.LookAt](https://docs.unity3d.com/ScriptReference/Transform.LookAt.html)
     
     + Hint: For all of the above lights, you may need to adjust the parameters to make sure that they work appropriately. Remember to check the Scene view while your game is running to see the location and behaviour of your lights.   

 - g) Finish it all off
     + Most of the remaining code will be added to your GameController script
     + Add in Text elements (see Lab Assignment #1) for the time remaining display and to display a message when the game ends.
     + Remember to add in an 'r' to restart the game, which can be pressed at any time.
     + Make sure that you meet all other game requirements!
     + Test and submit!

## Test and Submit
### Build and Test on Your Platform
 - Remember to save your scene.
 - In GitHub Desktop remember to "commit" and then "push"
 - Remember you can be extra sure that everything works by cloning your completed code and opening it as a new project in Unity. Does it work the same as when you submitted it?