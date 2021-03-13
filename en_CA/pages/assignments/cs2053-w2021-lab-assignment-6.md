# 2053 Lab Assignment #6: Physics, Cameras, Sounds and Scenes

In this lab assignment, you will gain experience with physics, cameras, sounds and working with multiple scenes.

This is a longer tutorial that has been designed to walk you through the steps. This assignment will be made easier if you follow them closely, step by step.

## Due: 11:30pm, Wed., Mar. 17, 2021

## 1. Setup the Basic Game

 1. In the game scene, similar to Lab 1 tutorial, use 3D cubes to build a closed polygon, preferably more complex than rectangle or square, such as a hexagon. 
 2. Lay the cubes (that have been stretched to make walls) on X-Z plane with y=0. Notice that for each cube, in the Inspector, a Box Collider is created (if not add one).
 3. And put ground (using a 3D plane) under the cubes. 
 4. Create a “Physic Material” with name “BounceMaterial” in Assets folder. Notice the 5 configurable properties of the “BounceMaterial”. Drag “BounceMaterial” to “Material” entry of the Box Collider component of each cube.
 5. Create a 3D Sphere object with the name Ball in the center (0, 0, 0) of the polygon. Notice that, in the Inspector, a Sphere Collider is created (if not add one).
    - you should adjust the walls for your shape so that the "Ball" is approximately in the center of the enclosed area. 
 6. Drag “BounceMaterial” to “Material” entry of the Sphere Collider for the "Ball".
 7. In the Inspector of Ball, add a “Rigidbody” component by selecting “Add Component”, “Physics”, then “Rigidbody”. Uncheck “Is Kinematic” property in “RigidBody” component. Note the “Mass” property.
 8. Create a new script with name “BallController” for the Ball object. Add the following code to “BallController”:

```cs
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class BallController : MonoBehaviour {

        Rigidbody rb;
        Vector3 initForce;

        // Use this for initialization
        void Start () {
            rb = GetComponent<Rigidbody>();

            Vector3 forceDirection = new Vector3(Random.Range(-1f, -.5f), 0, Random.Range(-1f, -.5f));
            forceDirection.Normalize();

            float forcePower = 15;
            initForce = forcePower * forceDirection;

            //initial push onto the ball in the forceDirection with forcePower
            rb.AddForce(initForce, ForceMode.Impulse); 
        }

        void OnCollisionEnter(Collision collision)
        {
        }
    }
```

 9. Run the program. Observe that the ball object’s movement by the initial force and bounces from the collided cubes. Note that the collision detections between the ball object and a cube are detected by the cube’s Box Collider and the ball’s Sphere Collider.
 10. Change the friction and bounciness property values in “BounceMaterial” to see how the ball’s movement changes. Also change the “Mass” property of the ball’s “Rigidbody” to see how the ball’s movement changes. The more mass the more force needed.
 11. You will now add Audio to the balls in the game, so that they will make a realistic sound when they hit something. Here we will provide two ways to do this. 

*Importing Sound into a Project:* Add an “Audio Source” component to Ball object. Download the sound file [here](http://hcidev.cs.unb.ca/Ball_Bounce.wav) into the Assets folder. Drag the wave file to the AudioClip entry of “Audio Source” component. Add the following code in “BallController”
- a) Define a variable ```public AudioSource myAudio;```
- b) In Start(), add ```myAudio = GetComponent<AudioSource>();```
- c) In OnCollisionEnter(), add ```myAudio.Play();```
- Run the program and hear bouncing sound on each collision.
- e) In OnCollisionEnter(), add the following. Notice the checks that are performed to make sure the object is initialized:

```cs
    if (myAudio != null && !myAudio.isPlaying &&
        myAudio.clip != null && myAudio.clip.loadState == AudioDataLoadState.Loaded
        )
    {
        myAudio.Play();
    }
```

    - Run the program and hear bouncing sound on each collision of the ball.

 1.  Duplicate the Ball object to create several more ball object and scatter them inside the polygon on the ground. Run the program and observe collisions between balls which are detected by two colliding balls’ Sphere Colliders.
 2.  Note that if you want give some constant force during ball’s movement in the same move direction after initial force, you can add the following code to FixedUpdate() of BallController, which may result in acceleration of the movement:

 ```cs 
    Vector3 myVelocity = rb.velocity;
    myVelocity.Normalize();
    float forcePower = 2;
    rb.AddForce(forcePower * myVelocity);
 ```

 1.  Create a material for your ground (Assets->Create->Material), change its color, and drag it from your assets folder to the ground plane in your scene view.
 2.  You might want to repeat this for the walls and balls, if you like.
 3.  Create another Sphere object, and call it "Player".
 4.  Provide a tag for your "Player" called "Player", and and create a distinct color using a material for the player.
 5.  Create a "PlayerController" script for the player and provide it with the following code. You will need to update the code to adopt the sound playing strategy that you used for the other balls, as described above. 
 ```cs
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class PlayerController : MonoBehaviour {
        public Vector3 direction;
        public float speed;
        Rigidbody rb;
        public AudioSource myAudio;

        // Use this for initialization
        void Start (){
            rb = GetComponent<Rigidbody>();
            myAudio = GetComponent<AudioSource>();
        }

        // Update is called once per frame
        void FixedUpdate () {
            float moveHorizontal = Input.GetAxis ("Horizontal");
            float moveVertical = Input.GetAxis ("Vertical");

            Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);

            rb.AddForce (movement * speed);
        }

        void OnCollisionEnter(Collision collision)
        {
            myAudio.Play();
        }
    }
 ```
 18. Don't forget to add a RigidBody component to your Player object.
 19. You may at this point want to create sub folders in your Assets folder to help you organize your Assets.

## 3. Add Cameras to your Game Scene

### 3.1 Static Camera and Camera Controller

 1. Rename the default “Main Camera” to “Static Camera”. Create 5 more cameras with names “Follow Camera”, “Spring Camera”, “Orbit Camera”, “First Person Camera”, and “Spline Camera”.
 2. Change the following properties of the static camera: 
    - Make its projection orthographic.
    - Set its transform (position and rotation) to be able to view the entire scene (a top-down view would be an easy way to achieve this). You will also need to make adjustments to the "size" to make everything visible.
 3. Create an empty game object with name “Camera Controller”, and create “CameraController” script with the following code:

```cs
public class CameraController : MonoBehaviour {
    public Camera staticCamera;
    public Camera followCamera;
    public Camera springCamera;
    public Camera orbitCamera;
    public Camera firstPersonCamera;
    public Camera splineCamera;

    public string choice = "static";

    // Use this for initialization
    void Start () {
        staticCamera.enabled = false;
        followCamera.enabled = false;
        springCamera.enabled = false;
        orbitCamera.enabled = false;
        firstPersonCamera.enabled = false;
        splineCamera.enabled = false;
   
        if (choice == "static") staticCamera.enabled = true;
        if (choice == "follow") followCamera.enabled = true;
        if (choice == "spring") springCamera.enabled = true;
        if (choice == "orbit") orbitCamera.enabled = true;
        if (choice == "fp") firstPersonCamera.enabled = true;
        if (choice == "spline") splineCamera.enabled = true;
    }
}
```
 

 4. In design scene, drag ‘Static Camera”, “Follow Camera”, “Spring Camera”, “Orbit Camera”, “First Person Camera” and “Spline Camera” objects to the corresponding entries of “CameraController”.

 5. In design scene, enter “static” in “choice” entry of the “Camera Controller”, then run the program and move the player object around to retest how static camera works.

### 3.2 Update the Follow Camera
 6. For “Follow Camera” object, create “FollowCameraController” script with the following code

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class FollowCameraController : MonoBehaviour {
    
    public GameObject target;

    Vector3 offset;

    // Use this for initialization
    void Start () {
        offset = transform.position - target.transform.position;
    }

    // Update is called once per frame
    void LateUpdate () {
        transform.position = target.transform.position + offset;
    }
}
```

 7. In the Inspector, play around with the Follow Camera’s transform so that it provides a 3rd person view. Starting values might be: position(-5,7,0) and rotation(45,90,0).  
 8. Drag the Player object to the “Target” entry of “Follow Camera”, set “Speed” entry of the player object to some positive value, and enter “follow” in the “Choice” entry of Camera Controller.
 9. Run the program. Move the player object and observe how the follow camera following the player object with fixed distance.

### 3.3 Update the Spring Camera

 10. For “Spring Camera” object, create “SpringCameraController” script with the following code:

```cs

using UnityEngine;
using System.Collections;
using System;

public class SpringCameraController : MonoBehaviour {
    
    public GameObject target;

    float springConstant = 9.0f;
    float dampConstant;

    Vector3 offset;
    Vector3 actualPosition;
    Vector3 velocity;

    // Use this for initialization
    void Start () {
        dampConstant = 2.0f * (float) Math.Sqrt(springConstant);
        offset = transform.position - target.transform.position;
        actualPosition = transform.position;
        velocity = Vector3.zero;
    }

    // Update is called once per frame
    void Update () {
        Vector3 idealPosition = target.transform.position + offset;
        Vector3 displacement = actualPosition - idealPosition;
        Vector3 springAccel = (-springConstant * displacement) - (dampConstant * velocity);
        velocity += springAccel * Time.deltaTime;
        actualPosition += velocity * Time.deltaTime;
        transform.position = actualPosition;
    }
}

```

11. Return to the Inspector of the Follow Camera, click on the gear icon -> "Copy Component". Then return to the transform of the Spring Camera. Click the gear icon -> "Paste Component Values". Your Follow Camera and Spring Camera should now have the same transform.
12. Drag player object to the “Target” entry of “Spring Camera”, set “Speed” entry of the player object to some positive value, and enter “spring” in the “Choice” entry of Camera Controller.
13. Run the program. Move the payer object and observe how the spring follow camera following the player object with variable distances.

### 3.4 Update the Orbit Camera
12. For “Orbit Camera” object, create “OrbitCameraController” script with the following code:

```cs
using UnityEngine;
using System.Collections;
using System;

public class OrbitCameraController : MonoBehaviour {
    
    public GameObject target;
    public float speed;

    // Use this for initialization
    void Start () {
    }

    // Update is called once per frame
    void Update () {
        transform.RotateAround(target.transform.position, Vector3.up, speed * Time.deltaTime);
        transform.LookAt(target.transform);
    }
}
```

13. Copy and paste the transform from either the Follow or Spring camera.
14. Drag player object to the “Target” entry of “Orbit Camera”, set “Speed” entry of the player object to 0 or some small positive value, set “Speed” entry of Orbit Camera to some positive value, and enter “orbit” in the “Choice” entry of Camera Controller.
15. Run the program. Observe how the orbit camera orbits around the player object with fixed distance.

### 3.5 Update the First-Person Camera

16. For “First Person Camera” object, create “FirstPersonCameraController” script and copy the same code from “FollowCameraController”.
17. Set the First Person Camera’s transform as position(0,1,0) and rotation as appropriate (you probably don't need to do anything).
18. Drag player object to the “Target” entry of “First Person Camera”, set “Speed” entry of the player object to some positive value, and enter “fp” in the “Choice” entry of Camera Controller.
19. Run the program. Move the player object and observe how the first person camera has the same view as the player object.

### 3.6 Update Spline Camera 

20.  For ‘Spline Camera” object, create “SplineCameraController” script with the following code:

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SplineCameraController : MonoBehaviour {

    public GameObject target;

    CRSpline path = new CRSpline();
    int index = 1;
    float t = 0.0f;
    public float speed;

    // Use this for initialization
    void Start () {
        
    }

    // Update is called once per frame

    void Update () {
        
        t += speed * Time.deltaTime;
        if (t >= 1.0f)
        {
            index++;
            t = t - 1.0f;
        }

        if (index + 2 < path.getSize ()) {
            transform.position = path.compute (index, t);
        } else {
            index = 1;
        }
        transform.LookAt(target.transform);

    }
}

class CRSpline
{
    Vector3[] controlPoints = {new Vector3(-10.0f, 3.0f, -6.0f),
        new Vector3(-8.0f, 4.0f, -2.0f),
        new Vector3(-6.0f, 5.0f, 2.0f),
        new Vector3(-4.0f, 6.0f, 6.0f),
        new Vector3(-6.0f, 5.0f, 8.0f),
        new Vector3(-8.0f, 4.0f, 2.0f),
        new Vector3(-10.0f, 4.0f, -2.0f)
    };

    public Vector3 compute(int start, float t)
    {
        Vector3 p0 = controlPoints[start - 1];
        Vector3 p1 = controlPoints[start];
        Vector3 p2 = controlPoints[start + 1];
        Vector3 p3 = controlPoints[start + 2];

        Vector3 position = 0.5f * ((2 * p1) + (-p0 + p2) * t + (2 * p0 - 5 * p1 + 4 * p2 - p3) * t * t + (-p0 + 3 * p1 - 3 * p2 + p3) * t * t * t);

        return position;
    }
    public int getSize() { return controlPoints.Length; }
}
```
 
21. Drag the player object to the “Target” entry of “Spline Camera”, set “Speed” entry of the player object to 0 or some small positive value, and enter “spline” in the “Choice” entry of Camera Controller.

22. Run the program. Observe Spline Camera’s movement along the spline path. You can add more control points and modify existing control points’ position in CRSpline class to observe different camera paths.

## 4. Create a Menu Screen/Scene

1. Create a new Scene, and name your scene "MainMenu"
2. Add two buttons to your scene ("Create->UI->Button"). Position your buttons, so that they are centered in the window (remember to anchor them).
3. Change the name of the first button to "StartButton" and set its text to "Start Game".
4. Change the name of the second button to "QuitButton" and set its text to "Quit Game".
5. Create a Text UI component and name it "Title" and add it to your scene. Make the text large and visible and give your game a name, e.g., "Ball Escape".
6. Create an empty game object called "GameController" and add a script to it called GameController, paste in the following code: 

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class GameController : MonoBehaviour {
    public enum GameState {BEGIN, PLAYING, WIN, LOSE};

    public static GameState gameState = GameController.GameState.BEGIN;

    public Text title;

    void Start()
    {
        title = GameObject.Find("Title").GetComponent<Text>();
        if (title != null) {
            if (gameState == GameController.GameState.BEGIN)
                title.text = "Ball Escape!!";
            else if (gameState == GameController.GameState.WIN)
                title.text = "You Win!";
            else if (gameState == GameController.GameState.LOSE)
                title.text = "You Lose!";
        }
    }

    public void LoadGame()
    {
        gameState = GameController.GameState.PLAYING;
        SceneManager.LoadScene("Game");
        SceneManager.UnloadSceneAsync("GameController");
    }

    //This will not use in the editor... it will only work if you build your game... not required for this assignment... just here, so you know
    public void QuitGame()
    {
        Application.Quit();
    }
}

```

7. Tie together the behaviour for the Start and Quit buttons

 + In the inspector of a button, under "Script", "On Click" drag the GameController game object to the Object location ->it should say "None (Object)"
 + Click on the drop down to select a function (it should say "No function"). Choose GameController -> LoadGame() or QuitGame()
  
8. Look at the code above carefully. Note that the enumeration can be referenced in any scene or object as a type. 
9. Note the ```static GameState gameState``` variable. Because it is static it can be referenced from any state. So, this can be used to track and update the state of your game from scripts anywhere in your game.
 
## 5. Complete the Game (Requirements)
Once you have completed all of the steps above you are ready to complete the game. Your game must have all of the functionality outlined above, and additionally:

 - Winning the game will mean using your player ball to knock all other balls into the exit hole.
    + Upon winning the title screen is displayed with a "You Win!" message.
 - Losing the game happens if the player ball enters the hole
    + Upon losing the title screen is displayed with a "You Lose!" message.
 - Add an exit hole to the game. A ball should have to pass directly over the hole to count. Non-player balls will disappear, and the player ball will immediately trigger the end of the game.
    + Hint: a cylinder can be used as a hole. However, you may need a second (smaller and invisible) cylinder to get the correct collision behaviour with your code.
    + Recall that we are using collisions in this Lab and not Triggers (just be sure to implement the correct functions)
 - Pushing number keys will provide the swap to the corresponding camera:
    1. Static camera
    2. Follow camera
    3. Spring camera
    4. Orbit camera
    5. First-person camera
    6. Spline Camera

 - Add keys into the game that will cause the cylinder for the hole to either be larger or small (this is to make it easy for your grader to test your game)
    + By default and when the game starts the hole should be smaller (challenging to hit, but not impossible)
    + Pushing *9* Should cause the hole to grow to be three times as large (this should be the collider, and does not necessarily have to be the visual hole itself)
    + Pushing *0* should cause the hole to return to its original, smaller size.
    
### 6. Submitting
 - Test first and once you are happy, commit and push to GitHub
