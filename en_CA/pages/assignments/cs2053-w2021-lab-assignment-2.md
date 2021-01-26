# Lab Assignment #2: Build PacMan Tag
In this assignment you will gain some skills with 2D game development by creating a PacMan Tag game. In PacMan Tag, the player controls PacMan. PacMan must avoid ghosts as long as possible with out getting "tagged" by a ghost. The game starts with just one ghost that PacMan must avoid, but every five seconds another Ghost appears. Here is a demo video so you can see how the game should behave: [PacMan TagDemo](https://www.youtube.com/watch?v=dYqCVp5PObo)

## Requirements
 - PacMan is controlled by the arrow keys, can a move up, down, left, or right
 - When PacMan reaches the edge of the screen, he stops moving
 - A timer is displayed that counts up each second that PacMan survives, and is displayed in the top right corner of the screen
 - A new Ghost appears around the center of the screen every 5 seconds
 - Ghosts (can be all of the same color) and move up, down, left or right randomly
 - When Ghosts reach the edge of the screen they automatically start moving in the opposite direction
 - When a Ghost collides with PacMan the game is over
 - The game can start immediately when launched
 - Pressing 'R' should restart the game, and can happen at anytime
 - Both PacMan and the ghosts are animated based on the provided sprite sheet

## Resources
You should likely start by watching the following tutorial videos (best done before the lab). Together they contain much of the information you need for completing this assignment.

 - [2D Mode](https://www.youtube.com/watch?v=L3vLnEc7HTg)
 - [The Sprite Type](https://www.youtube.com/watch?v=R0J1fNSoxcI)
 - [Sprite Renderer](https://www.youtube.com/watch?v=VfAYSWpf7gg)
 - [The Sprite Editor](https://www.youtube.com/watch?v=gbgIA3pwpHc)
 - [2D Physics](https://www.youtube.com/watch?v=xEVXE7_YILk)
 - [Rigid Body 2D](https://www.youtube.com/watch?v=rq6c2B_socs)
 - [Collider 2D](https://www.youtube.com/watch?v=YQ7Umjp6R10)
 - [Colliders as Triggers](https://www.youtube.com/watch?v=m0fjrQkaES4)

### Helpful Hints
#### Accessing Components
Unity applications are made up of different components. If you need access to a component from a Script the easiest thing to do is to:

 1. Add a public variable of the components type to your script
 2. Drag the component to the script in the Unity Inspector
 3. Use the GetComponent function (see below) to access the object
 4. See [here](http://web.archive.org/web/20190308220127/https://unitylore.com/articles/script-communication) for more info.
 
```csharp

using UnityEngine;
 
public class GameManager : MonoBehaviour
{
    Player playerInstance;
    
    void Start()
    {
        playerInstance = GetComponent<Player>();
        
        print ("The Player's health is: " + playerInstance.health);
    }
}
```

## Specific Steps to Take Note of When Starting
 
- Note: you would be able to drag and drop new images/audio/etc. to your project and it would just work.

### 1. Set up your project

- To keep things neat create some directories in your project folder
- Rename your scene PacManTag (right-click on the SampleScene and click rename)
- Create folders called "Prefabs", "Scripts", and "Sprites"
- Create a subfolder in Sprites called "Animations"
- Download the two sprite files (right click these links and Save As)
 - [Sprite Sheet](Pacman-sprite.png)
 - [Background](https://cs-2053-winter-2021.github.io/en_CA/pages/assignments/as2-files/back.png)
- Drag these downloaded files into your Sprites directory

### 2. Create PacMan

Let's start by creating a moving PacMan with the animation.

- Add a new Sprite object for PacMan (click the '+' in the Hierarchy and choose )
- Rename the sprite PacMan
- Add a Script component to PacMan called PacManController
- To add in the animation for your PacMan
   + Click the Sprite sheet (Pacman-sprite) you have already downloaded and put in the Sprites directory
   + In the Inspector window, find "Sprite Mode" and set it to "multiple"
   + Next, click the "Sprite Editor" button in the Inspector
   + Use the Slice button (you might need to resize your window to see the Slice button) and automatically slice the sprite sheet, click "Apply"
   + You should now be able to find the individual sprites sliced up from your PacMan (you might have to click on the right arrow next to the sprite sheet in your "Project" window)
   + Use Shift+Select (or Ctrl+Select) to choose each animation frame (left, right, up, down), there should be three frames for each of the four animations
   + Drag and drop each of your animations on the PacMan Sprite object 
       * This will popup and dialog to name a new animation, give each of your animations something like "PacManLeft", "PacManUp", etc.
   + You can save each of the animations in an animations folder you created. 
- to get your PacMan to show up in the Scene view, in the Hierarchy choose "PacMan" and in the Inspector open the Sprite Renderer, and click the dot next to Sprite. 
- Choose a sprite for PacMan's default display, maybe Pacman-sprite_15
- PacMan will likely be very small right now, so make him larger change PacMan's Transform Scale line to 5 for the x and y.
- Now move PacMan so he starts towards the top left-hand corner of the game

**Initial PacMan Control**

- Use the code below to control your PacMan (copy and paste the code below into your PacManController script), be sure to read it carefully, so that you understand it.
- Test your game to make sure PacMan behaves correctly. You should be able to control him and he won't go outside the borders of the window.

```csharp

using UnityEngine;

public class PacManController : MonoBehaviour
{
    private Vector3 velocity;

    private SpriteRenderer rend;
    private Animator anim;
    public float speed = 2.0f;

    // Use this for initialization
    void Start()
    {
        velocity = new Vector3(0f, 0f, 0f);
        rend = GetComponent<SpriteRenderer> ();
        anim = GetComponent<Animator>();
    }

    // Update is called once per frame
    void Update()
    {

       // calculate location of screen borders
        // this will make more sense after we discuss vectors and 3D
        var dist = (transform.position - Camera.main.transform.position).z;
        var leftBorder = Camera.main.ViewportToWorldPoint(new Vector3(0, 0, dist)).x;
        var rightBorder = Camera.main.ViewportToWorldPoint(new Vector3(1, 0, dist)).x;
        var bottomBorder = Camera.main.ViewportToWorldPoint(new Vector3(0, 0, dist)).y;
        var topBorder = Camera.main.ViewportToWorldPoint(new Vector3(0, 1, dist)).y;
        
        //get the width of the object
        float width = rend.bounds.size.x;
        float height = rend.bounds.size.y;

        //set the direction based on  input
        //note this is a simplified version //in assignment 1 we used a the Input Handler System
        if (Input.GetKey("left"))
        {
            velocity = new Vector3(-1f, 0f, 0f);  
            anim.Play("PacManLeft");    
        }
        if (Input.GetKey("right"))
        {
            velocity = new Vector3(1f, 0f, 0f);   
            anim.Play("PacManRight");    
        }
        if (Input.GetKey("down"))
        {
            velocity = new Vector3(0f, -1f, 0f); 
            anim.Play("PacManDown");    
        }
        if (Input.GetKey("up"))
        {
            velocity = new Vector3(0f, 1f, 0f);
            anim.Play("PacManUp");    
        }

        //make sure the obect is inside the borders... if edge is hit reverse direction
        if((transform.position.x <= leftBorder + width/2.0) && velocity.x < 0f){
            velocity = new Vector3(0f, 0f, 0f);
        }
        if((transform.position.x >= rightBorder - width/2.0) && velocity.x > 0f){
            velocity = new Vector3(0f, 0f, 0f);
        }
        if((transform.position.y <= bottomBorder + height/2.0) && velocity.y < 0f){
                velocity = new Vector3(0f, 0f, 0f);
        }
        if((transform.position.y >= topBorder - height/2.0) && velocity.y > 0f){
            velocity = new Vector3(0f, 0f, 0f);
        }
        transform.position = transform.position + velocity * Time.deltaTime * speed;
    }
}
```

**Add a Background**

- Drag and drop the Background sprite (back) from your Sprites Directory on to the PacManTag scene in the hierarchy
- rename the sprite Background in the Hiearchy
- The background may now cover your PacMan, so in the Inspector for the Background, open the Sprite Renderer section
- Click Sorting Layer Default and "Add Sorting Layer..."
- Under Sorting Layers, click the + to add a new layer... name it Background
- Then Drag Background so that it is above Default
- Click on Background in the Hierarchy and change the Sorting Layer to the newly created Background layer.
- Run your game and confirm that PacMan is on top of the background. 
- You may need to stretch and resize your background so that it covers the entire background of your game scene.
  

### 3. Create Your Ghost

 - Repeat the steps above for creating a Ghost.
 - Make your Ghost which ever color you like, and note that there are only 2 images for each ghost direction animation.
 - Use the same code above for your GhostController script, but switch out the code for moving the player with the arrows with the random movement code below
 - Test your program to see your ghost in action


```csharp

//add this import statement for Random number generation
using Random = UnityEngine.Random;

//1% of the time, switch the direction: 
int change = Random.Range(0,100);
if (change == 0)
{
    velocity = new Vector3(-velocity.x, -velocity.y, 0);
}
//1% of the time, switch from horizontal to vertical, or vice-versa: 
else if (change == 1)
{
    if (velocity.x != 0f)
        velocity = new Vector3(0f, velocity.x, 0f);
    else
        velocity = new Vector3(velocity.y, 0f, 0f);
} 
```

### 4. Create a GameController Script and Multiple Ghosts

To control the state of your game we will create a third script (you should already have scripts for PacMan and for the Ghost).

 - First create an "Empty Game Object" and name it GameController
 - Add a new Script to your GameController object name your script "GameController"
 - Drag and drop your GameController script to the GameController game object (this will cause the script to run while the game is running).
 -  Next drag and drop your Ghost Game Object from the Hiearchy to your "Prefabs" folder. Once you are sure that your ghost is safely stored in the Prefabs folder you can delete the Game Object from your project hierarchy
     +  This will allow you to start the game without any ghosts initially, we will create ghosts from the GameController script.
 - Create a public GameObject variable in your GameController called Ghost
- The last step will give you a public Ghost variable on your GameController script, drag the Ghost prefab to the public variable in the Inspector of the GameController
 -  In your GameController script try instantiating multiple Ghosts in your Start function using the code below

```csharp

//note red ghost was the name of my public variable that 
//contains the Ghost prefab
Instantiate(redGhost, new Vector3 (0f, 0f, 0f), Quaternion.identity);

```

### 5. Detecting Collisions and notifying the GameContorller

- to detect collisions add Box Collider 2D to both your PacMan and Ghost
  + Select the objects, in the Inspector -> "Add Component" -> "Physics 2D" -> "Box Collider 2D"
- Make sure that both colliders have the "Is Trigger" box checked (we will discuss this option when we do more with collisions)
- Your Ghosts and PacMan can now detect collisions, but you need to add a new method to your objects... however, only one of these objects will need to detect the collision so add the method below to your PacManController script
- Afterwards test your code to make sure everything works and when you collide with a ghost that your PacMan disappears
- If you find that your OnTrigger code is not being called automatically, try adding a RigidBody2D component to your Pacman and your Ghost.

```csharp

void OnTriggerEnter2D(Collider2D other) {
    
    //uncomment this in the next step
    //to notify the GameController that the game is over
    //gameController.GameOver();
    
    //this get rid of your PacMan
    Destroy(gameObject);    
}
```

### 6. Finish it all off
- Most of the remaining code will be added to the GameController script
- Add in TextMeshPro elements ([see Lab Assignment #1](https://cs-2053-winter-2021.github.io/en_CA/#!pages/assignments/cs2053-w2021-lab-assignment-1.md)) for the time and to display a message when the game ends
- Add in updates to the time in your GameController
- Add a public GameOver method to your GameController, this method should set the state of your game to be completed (remember to uncomment the GameOver function call in your PacManController script)
- Add in the code to generate a new ghost every 5 seconds
- Use this simple line to check if the R key is being pressed to restart: 

```csharp

   //in GameController
   if (Input.GetKeyDown(KeyCode.R)) {
       //restart
   }

```

- Make sure that you meet all other game requirements!
- Test and submit!

## Test and Submit

- Remember to save your scene and from GitHub you can commit and push your code. 
- See the [instructions on the course website](https://cs-2053-winter-2021.github.io/en_CA/#!pages/CS2053-working-with-git.md)
