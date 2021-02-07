# Lab Assignment #4: 
In this assignment you will practice 3D game development.

## Preliminary Steps
In this assignment you will follow the steps below to first create a base system. We will then make some changes to it, based on the items listed in the Requirements section. The final lab project can be viewed here: [Lab 4 - CS2053](https://www.youtube.com/watch?v=Vddt4VShKNo).

### 1. Setting up game scene
- Similar to the lab 1 tutorial, create a new 3D object “Plane”. Change its name to “Ground”. Change Ground to a desirable size (scaling it by 2 should work well). Do the following to change the color of Ground to whatever color you like:
    + Create a new material.
    + In the Inspector for the Material, click Base Map to select a color.
    + Drag the new material to Ground the object.

- In Models you will find a Ship model... drag it into your Scene.
- Scale it to be 10% of it's normal size.
- Rename the 3D game object to “FlyingCraft”.
- Run the program to make sure it is working. 

### 2. Rotate 3D Model Around the Y-Axis
- Create a new script for “FlyingCraft” with name “FlyingCraftController”.
- Add “using System;” in the script.
- Add the following instance variables:

```csharp
private float rotationAngleDegree;
private float rotationSpeedDegree;
private int rotationDirection;
```

- In the Start() method, add the following code:

```csharp
rotationAngleDegree = 0;
rotationSpeedDegree = 10;
rotationDirection = 0;
```

- Define a new ```Rotate()``` method as follows. Carefully read the code to understand the code. 
```csharp
private void Rotate()
{
    float rotationVelocityDegree = rotationSpeedDegree * rotationDirection;
    rotationAngleDegree += rotationVelocityDegree * Time.deltaTime;

    //make sure that rotationAngleDegree within 0-360
    rotationAngleDegree = (rotationAngleDegree + 360) % 360; 
    transform.Rotate(Vector3.up, rotationVelocityDegree * Time.deltaTime); //Vector.up is Y-Axis
}
```

- In ```Update()``` method, add the following code to control rotation of FlyingCraft by keyboard “left arrow” (clock-wise) and “right arrow” (counter-clock-wise)

```csharp
if (Input.GetKey("right"))
{
    rotationDirection = 1;
    Rotate();
}

else if (Input.GetKey("left"))
{
    rotationDirection = -1;
    Rotate();
}
```
    
- Run the program. Press “left arrow” key or “right arrow” key to observe how the 3D model rotates around Y-Axis. Change the value of rotationSpeedDegree to make rotation slower or faster.

### 3. Move the 3D model forward and backward
Now you will move the 3D model forwards and backwards along the orientation/direction of the model in parallel with X-Z plane.

- Add the following instance variables to FlyingCraftController script

```csharp
 public Vector3 motionVelocity;
 public float motionSpeedXZ;   
 private int motionDirectionXZ;
```

- In Start() method, add the following code:
```
motionVelocity = Vector3.zero;
motionSpeedXZ = 1;
motionDirectionXZ = 0;
```
 
- Define a new “Move()” function with the following code. Carefully read the code to understand the code.

```csharp
private void Move()
{
    //convert degree to radian
    double rotationAngleRadian = ((float)rotationAngleDegree / 360.0) * (Math.PI * 2.0); 
    float motionX = (float)Math.Sin(rotationAngleRadian) * motionDirectionXZ;
    float motionZ = (float)Math.Cos(rotationAngleRadian) * motionDirectionXZ;

    motionVelocity = new Vector3(motionX, 0, motionZ);

    //nomoralized vector to represent the directions of motionVelocity
    motionVelocity.Normalize(); 

    motionVelocity = new Vector3(motionVelocity.x * motionSpeedXZ, 0, motionVelocity.z * motionSpeedXZ);
    transform.position += motionVelocity * Time.deltaTime;

    rotationDirection = 0;
    motionDirectionXZ = 0;
}
```

- In ```Update()``` method, add the following code to control motion of FlyingCraft object going forward to backward in the object’s current orientation/direction by keyboard “up arrow” or “down arrow” keys.

```csharp
if (Input.GetKey("up"))
{
    motionDirectionXZ = 1;
    Move();
}

else if (Input.GetKey("down"))
{
    motionDirectionXZ = -1;
    Move();
}
```
- Run the program. Press “up arrow” or “down arrow” keys to observe motion of the 3D object. Change the value of motionSpeedXZ variable to make the object move slower or faster. Note that in case that the motion does not align with the object’s direction, in FlyingCraft’s inspector, change Y value of Transform/Rotation, to pre-rotate the object around Y-Axis in desired degree, and/or change the project axis, i.e.  switch Math.Sin() and Math.Cos() for X and Z.

### 3. Move 3D model up and down in Y-axis

- Add the following instance variables in FlyingCraftController script

```csharp
public float motionSpeedY;
private int motionDirectionY;
```

- In “Start(), add the following code. Note that you can set different speed for Y-Axis motion that X-Z motion.

```cs
motionSpeedY = 0.5f;
motionDirectionY = 0;
```

- In move() method, modify the code as follows:

```csharp
//Add the following
float motionY = motionDirectionY;

//Change the following 
motionVelocity = new Vector3(motionVelocity.x * motionSpeedXZ, 0, motionVelocity.z * motionSpeedXZ);
//should become: 
motionVelocity = new Vector3(motionVelocity.x * motionSpeedXZ, motionY * motionSpeedY, motionVelocity.z * motionSpeedXZ);
```

- In Update() method, add the following code control motion of the FlyingCraft object in Y-Axis by keyboard keys ‘u’ (up) and ‘d’ (down).

```csharp
if (Input.GetKey("u"))
{
    motionDirectionY = 1;
    Move();
}

else if (Input.GetKey("d"))
{
    motionDirectionY = -1;
    Move();
}
```

- Run the program. Press ‘u’ key or ‘d’ key to observe how the FlyingCraft object moves in Y-Axis. Change the value of motionSpeedY to make the object move slower or faster along Y-Axis.

## Requirements
Based on the tutorial above you will now update your game to meet the following requirements. 

- Update the spaceship so that it always flies forwards at a constant speed.
- Update the ship's controller, so that the up and down arrows allow you fly up higher or lower.
   + No forward or backwards controls are needed.
- You will create a 3D obstacle that the player can fly through.
   + For example, using cubes create a structure that the player can fly through
- You will create at least 3 obstacle placed over the "ground" that the players must fly through, without touching the sides of the obstacle.
   + The three obstacles should be distributed in a challenging way and have different rotations
- To win the game players must fly through all three obstacles.
   + Note flying through the same obstacle 3 times, should not work.
- The number of obstacles remaining is displayed in the top right-hand corner of the screen.
- If the player hits the ground or makes contact with any of the obstacles sides, the ship stops moving and responding to input, and "you lose" message is displayed.
- If the player wins, the ship stops moving and a "you win" messages is displayed.
- The game can be restarted at any time using the 'r' key, and quit using the 'esc' key.


## Specific Steps to Help

### 1. Create an Obstacle
- Use 4 separate cubes to create an obstacle.
   + Scale and stretch them as appropriate.
   + For each cube, "Add a Box Collider" component.
- In the interior (the gap) of your obstacle create another cube. This cube will fill up the entire empty space.
   + Again, remember to "Add a Box Collider" to this cube.
   + Make this cube transparent, by simply turning off the "Renderer" from your gap cube from its inspector. Just uncheck "Mesh Renderer".
- Once you have your five cubes arranged, "Create an Empty" game object, and place your five cubes as children of this GameObject.
   + Rename your GameObject: "Obstacle"
 
### 2. Add a Collider to Your Ship
- For your ship, "Add a Mesh Collider" (under Physics). 
   + This creates a collider based on the 3D model and will create a collider that is really accurate (but computationally much more expensive)
- Also add a "Rigid body" to your ship and check "Is Kinematic" and uncheck "Use Gravity"
- Make sure the Collider is marked as "Is Trigger" 
- Add in the code below to process the collision events.
   + Notice the use of "tags", we will set these up in the next step

```csharp
void OnTriggerEnter(Collider c)
{
    //you will update these in a later step
    if (c.gameObject.tag == "Obstacle") {
        Debug.Log("Game Over: hit obstacle");
    } else if (c.gameObject.tag == "Ground") {
        Debug.Log("Game Over: hit ground");
    }
    if (c.gameObject.tag == "Gap") {
        Debug.Log("Passed through a gap!");
    }
}

void OnTriggerExit(Collider c){
    if (c.gameObject.tag == "Gap") {
        Debug.Log("Exited a gap!");
    }
}
```

### 3. Add in Tags to the Different Game Elements
To help with detecting the source of a collider, create new tags for the appropriate game objects. This will allow our ship to know what it collided with, so it can react accordingly. The alternative approach would be to add collisions processing functions on each of the objects that can be collided with (you can hopefully see why using tags is a better approach).

- For each of the elements (ground, each part of the obstacle, and the gap) add in the appropriate tags, and make sure each has its collider to be set as a Trigger.
  - Note for the Ground, you will first have to to Check "Convex" before you can check "Trigger"
- Tags can be added by selecting one of the GameObjects, clicking "Tag > Add Tag..."
   + Note your tags should match the tag names reference in the code of the previous step


### 4. Update the flight behaviour of your ship and update the camera
- You should now be comfortable updating the flight behaviour of your ship, based on the requirements, the provided code and previous lab assignments.
- To get your camera to follow the ship, simply drag your Main Camera in the Hierarchy and make it a child of the FlyingCraft.
- Adjust the camera's position, so it is behind the ship and is a good distance away, so it can see the world comfortably.
- At this point you should be able to test all of your work so far. 
  + Your ship should fly up and down with the up and down arrows.
  + Should rotate with left or right arrows.
  + The camera should follow your ship.
  + And you should be able to get appropriate messages in your console based on what collisions occur.
- Make adjustments to your camera's position and the speed of your ships movement and rotation, until you are satisfied with its behaviour.
- Test your code extensively. You may notice that when you fly through the gap, that multiple events/collisions are triggered.
   + There is some assistance with how to deal with this below.
 
### 5. Create a GameController and make an obstacle prefab
- As in previous lab assignments, create a GameController for controlling your game's state, displaying win and loss messages, updating the score, and creating Prefab game objects.
- First make your obstacle a Prefab object.
   + See previous lab assignments.
- Create your game controller script and instantiate your prefab obstacles.
   + Here it will be helpful to have a data structure to keep track of the obstacles, and which ones have been flown through.
- The partial code below can help you get started with this

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;
public class GameController : MonoBehaviour {
    public GameObject obstacle;

    private ArrayList obstacles;

    public Text scoreText;
    public Text gameOverText;

    // Use this for initialization
    void Start () {
        obstacles = new ArrayList();
        CreateObstacles();
    }
    
    // Update is called once per frame
    void Update () {
        scoreText.text = "Obstacles remaining: "+ obstacles.Count;

        //to be completed
    }

    public void CreateObstacles()
    {
        //you should create at least 3 obstacles with different rotations 
        //and in different positions
        //note: we are adding them to the ArrayList created above

        //Instantiate can take the transform (position vector) 
        //and rotation quaternion as parameters

        obstacles.Add(
            Instantiate(obstacle, new Vector3(4,4,4), Quaternion.identity)
        );

        //Quaternion.AngleAxis creates a Quaternion object that is defined by 
        //the number of degrees of rotation around a provided axis. 
        //Below we provide the up axis (or y-axis)
        obstacles.Add(
            Instantiate(obstacle, new Vector3(-10,8,6), Quaternion.AngleAxis(45f, Vector3.up))
        ); 
    }

    //This method is to be called by the FlyingCraft when it successfully
    //makes it through a gap. The first parameter is the obstacle,
    //the second parameter is the FlyingCraft game object.
    public void Score(GameObject obstacle, GameObject sender)
    {
        //check if the obstacle is in the list
        //i.e., has not yet been passed through
        if (obstacles.Contains(obstacle)){
            obstacles.Remove(obstacle);
        }

        if (obstacles.Count == 0)
        {
            Win();
            sender.SendMessage("Stop");
        }
    }
}

```

- The code below is provided to assist with updating your Collision handlers

```csharp
void OnTriggerEnter(Collider c)
{
    if (c.gameObject.tag == "Obstacle") {
        gameOn = false;
        gameController.GameOver();
    } else if (c.gameObject.tag == "Ground") {
        gameOn = false; 
        gameController.GameOver();  
    }
    if (c.gameObject.tag == "Gap") {
        inGap = true;
    }
}

void OnTriggerExit(Collider c){
    if (inGap)
    {
        inGap = false;

        //note the line below is necessary to access the whole Prefab
        //obstacle object. The collider that is return only refer to the 
        //the gap cube
        GameObject parentObject = c.gameObject.transform.parent.gameObject;
        
        //call the score method with the correct obstacle and
        //a reference to this FlyingCraft object
        gameController.Score(parentObject, gameObject);
    }
}
```

### 6. Finish it all off
- Most of the remaining code will be added to the GameController script
- Add in Text elements (see Lab Assignment #1) for the obstacles remaining display and to display a message when the game ends.
- Remember to add in an 'r' to restart the game and an 'esc' to quit.
- Make sure that you meet all other game requirements!
- Test and submit!

## Submitting
- Remember to Commit and Push your project in Github Desktop when you are ready to submit.