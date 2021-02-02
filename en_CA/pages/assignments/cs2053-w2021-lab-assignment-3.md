# Lab Assignment #3: Tank Shooter
In this assignment you will practice 2D game development further, but now gaining some experience with the tilemaps and vector functionalities. Tank Shooter is a top down game, where a player controls a tank on the bottom of the map. A competing tank is on the other side of the map. 

Here is a demo video so you can see how the game should behave: [Tank Shooter Demo](https://www.youtube.com/watch?v=1gH_oYgkud0)

## Requirements
- Two tanks are across from one another, separated by some physical barrier (e.g., water separates one tank from the other).
- The player's tank can be controlled using either arrow keys or WASD, and the tank can only move left or right.
- When the player's tank reaches the edge of the screen, it stops moving.
- The player can use the mouse to shoot at the computer's tank. When the mouse is clicked the tank fires a bullet straight across.
- It takes 3 seconds to reload a tank after it has been fired, during this time the tank is unable to shoot.
- The computer's tank moves left and right changing direction randomly.
- When it hits the wall, it changes direction. 
- The computer's tank always shoots as soon as it can.
- When the computer's tank is hit, a red X is displayed in the top right hand corner, representing the number of times the player has been hit.
- When the player's tank is hit, a red X is displayed in the bottom right hand corner.
- After being hit three times, either the player tank wins or the computer opponent wins. A message is displayed and the losing tank disappears.
- The game can start immediately when launched.
- Pressing 'R' should restart the game, and can happen at anytime.
 
## Resources
You should likely start by watching the following tutorial videos and reading the following documents. Together they contain much of the information you need for completing this assignment.

- <a href="https://unity3d.com/learn/tutorials/topics/scripting/vector-maths" target="_top">Vector Math</a>
- <a href="https://www.youtube.com/watch?v=fmNtibNWPhc" target="_top">TileMap Introduction</a>
- <a href="https://docs.unity3d.com/ScriptReference/WaitForSecondsRealtime.html" target="_top">WaitForSecondsRealtime</a>
- <a href="https://unity3d.com/learn/tutorials/topics/physics/colliders-triggers" target="_top">Colliders as Triggers</a>
 

### Helpful Hints

This tutorial builds heavily on what you have done in previous tutorials. Consult your code from previous tutorials to get started.


### 1. Setting Up Sprites and Create Palette

- Find the Sprite Sheet for the Tiles for your game in your Assets/Sprites folder of your Project.
- Click on the sprite sheet of the tiles, and change its Texture Type to Multiple. Set its Pixels per Unit to be 32. 
- Set the Filter Mode to Point (no filter)
- Use the Sprite Editor to Slice the sprite sheet automatically as in the past tutorial. 
- If you are having trouble getting them sliced up automatically, you can also slice by cell (use 32x32px for the cell size, and set 1x1px padding and offset)

- To "paint" the background on your scene, you must first add a new Tilemap (Create -> 2D Game Object -> 2D TileMap). This will provide a Grid object in your hierarchy with a Tilemap as the child.
- Open your "Tile Palette" (Menu: Window->2D->Tile Palette)
- Drag whichever sprites you would like to use into the tile palette or the entire sprite sheet (my solution uses just three different sprite tiles). **Warning if you drag the entire Sprite Sheet you will get 700 different tiles on your sprite sheet, which takes a few minutes to generate. Alternatively, you can open the sprite sheet and select individual tiles you would like to use.**
- You can use the provided World Palette folder to store the create tile and palette assets.

- Now when you select the Grid in your hierarchy you can paint on tiles using your tile palette tool
- Use the "Paint Brush" or "Filled Box" tool in your tile palette to fill in the map
- You can use whatever sprites you like, but be sure to stick to the assignment requirements


### 2. Create Sprites and Add Scripts for Your Tanks

- Find the sprite sheet for the tanks as above in your Assets/Sprites folder of your Project.
- Click on the sprite sheet of the tiles, and change its Texture Type to Multiple. Set its Pixels per Unit to be 32.  
- Use the Sprite Editor to again automatically slice the sprite sheet to create individual sprites for the game.
- For your tanks you will need to use the tanks that are broken into two parts.

- First, create a sprite that are the base and treads of the player's tank by dragging the sprite onto the Scene. You will need to rotate the sprite (using the rotation values in the Inspector) so that the tanks base is oriented correctly (so that the left right movement makes sense)
   + Note: this rotation will have implications for your tanks movements. So you may need to come back to this step.
- Next create a sprite for the tank's turret (the round part on top). In your Hierarchy drag the turret on top of the tank's base so that it becomes a child element of the base
   + This will mean that when you move the base it will also move the turret, but it can have a different orientation (it can point across the water at the opponent)
- Next repeat the steps so that you also have the computer's tank created (at the top of the screen).
- Run your game to test it (nothing should happen yet, but everything should look OK).

### 3. Add scripts to the tanks to control their movement

- Add a new script component to both tanks (PlayerTankController and ComputerTankController)
- Make use of the folders to keep your project tidy
- Adjust the logic from the code provide from the previous lab to control your tanks
- See the code snippets below for new, additional ways to control your game objects
- Test and run your game.

```csharp
    //previously we had updated GameObject positions using position, like this:
    transform.position = translate.position + someOffsetVector;

    //however, you can also simply translate objects
    transform.Translate(someOffsetVector);

    //your tank left-right tank control might consist of a single line
    //the code below allows you to control the right left movement of a 
    //rotated tank using the arrow left and right arrow keys or A or D 
    //if your tank is moving up and down, how do you think you can fix vector below?
    velocity = new Vector3(0f, Input.GetAxis("Horizontal") * -1f, 0f);  

    //using the code above is also an easy way to stop the tank when 
    //a player is not pushing a key... Input.GetAxis will return 0 
```

### 4. Add a bullet

- Create another sprite GameObject that will act as your bullet (a separate bullet sprite has been provided)
- Similar to the Ghosts from the PacMan activity you will want to instantiate your bullet whenever a tank fires (when the mouse button is clicked for your player)
- Add a script for your bullet, the bullet should move in one direction only, either up or down.
- You can use the script below for your bullet
- Once you have created your bullet, remember to move it to you Prefabs folder and delete from the game objects.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BulletController : MonoBehaviour {

    public float speed = 2f;

    public Vector3 velocity; 

    // Use this for initialization
    void Start () {
    }
    
    // Update is called once per frame
    void Update () {
        if (velocity != null)
            transform.Translate(velocity * Time.deltaTime * speed);

        // this will make more sense after we discuss vectors and 3D
        float dist = (transform.position - Camera.main.transform.position).z;
        float leftBorder = Camera.main.ViewportToWorldPoint(new Vector3(0, 0, dist)).x;
        float rightBorder = Camera.main.ViewportToWorldPoint(new Vector3(1, 0, dist)).x;
        float bottomBorder = Camera.main.ViewportToWorldPoint(new Vector3(0, 0, dist)).y;
        float topBorder = Camera.main.ViewportToWorldPoint(new Vector3(0, 1, dist)).y;

        //destroy bullet when it leaves the screen
        if (transform.position.y > topBorder ||
            transform.position.y < bottomBorder ||
            transform.position.x > rightBorder ||
            transform.position.x < leftBorder)
            {
                Destroy(gameObject);
            }
        
    }

    public void InitPosition(Vector3 p, Vector3 v) {
        transform.position = p; 
        velocity = v;
    }   

    void OnTriggerEnter2D(Collider2D other) {
        Destroy(gameObject);    
    }
}

```

### 5. Shoot Bullets

- Bullet shooting will be handled by your tank controllers
- see the code below for an example of how to shoot the bullet and to create a timeout


```csharp

    void Update() {

        //... steps for controlling tank
        //shooting the bullet from the player
        if (Input.GetButtonDown("Fire1") && canFire)
        {
            //the offset 
            Vector3 offset = new Vector3(0f, 2f, 0f);

            //create a bullet pointing in its natural direction 
            GameObject b = Instantiate(bullet, new Vector3 (0f, 0f, 0f), Quaternion.identity);

            //creates a bullet that is pointing in the opposite direction... pick the one you need  
            GameObject b = Instantiate(bullet, new Vector3 (0f, 0f, 0f), Quaternion.AngleAxis(180, Vector2.left));

            b.GetComponent<BulletController>().InitPosition(transform.position + offset, new Vector3(0f,2f,0f));
            canFire = false;

            //this starts a coroutine... a non-blocking function
            StartCoroutine(PlayerCanFireAgain());
        }

        //... more stuff  as needed goes down here  
    }

    //will wait 3 seconds and then will reset the flag
    IEnumerator PlayerCanFireAgain()
    {
        //this will pause the execution of this method for 3 seconds without blocking
        yield return new WaitForSecondsRealtime(3);
        canFire = true;
    }
```

### 6. Add colliders
- Add in 2DBoxCollider and RigidBody2D components for your tanks and bullets (see Lab Assignment #2)
- Remember to set all BoxColliders (tanks and bullet) to have the "Is Trigger" checkbox option set
- Remember to set the Body Type of all RigidBody components to Kinematic.
- Note: if you are having trouble getting the triggers to happen:
   + Remember to double check the signature of your "OnTriggerEnter2D" function
   + Make sure you have a RigidBody2D component on your bullet and your tanks
   + When in doubt try deleting and re-adding all BoxColliders and RigidBody components

### 7. Finish it all off

- Most of the remaining code will be added to the GameController script
- Add in Text elements (see Lab Assignment #1) for the score (the XXX's to display the status) and to display a message when the game ends.
- Remember to add in an 'R' to restart the game
- Make sure that you meet all other game requirements!
- Test and submit by committing your code in GitHub desktop and pushing.


