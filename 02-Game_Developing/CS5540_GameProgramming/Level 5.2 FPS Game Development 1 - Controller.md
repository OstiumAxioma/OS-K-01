---
date: 2024-05-06
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet:
  - "[[Level 2.1 Math and Vector in Unity]]"
knowledgeLink:
  - "[[Quaternion 四元数]]"
  - "[[Vector向量]]"
status: true
---
# FPS Development
## Development RoadMap
- FPS Character Controller
- FPS Camera Controller (Mouse Look)
- FPS Shooting
- Player
	- Health and UI
	- Shield System
- Enemy
	- Enemy Behavior
	- Spawning Enemies
- Interactable Objects
- VFX
	- Trail Renderer
	- Particle System

## Development Procedure
### FPS Controller
Add an empty game object to your scene and name it as “Player”. Add a capsule collider to it and set the height of the collider to represent a human. Then add a rigid body component to the player.

Adjust the position of the Player game object so that it looks like the player is standing on the ground. There should be a small gap between the capsule collider and the ground. Otherwise, the player may fall through the ground on game start.

Adjust the camera height, so that it’s little above the ground level and set the camera as the child of the Player game object. The camera position should be, where the head of the player would be. Here is how the player should look like.

Two scripts components is used:
1. Mouse Look (attached to the camera)
2. Player Controller (attached to the capsule)
#### Mouse Look
We need mouse look to control two different dimensions:
1. Pitch:
		Look up and down. Attached to the camera
2. Yaw:
		Look left and right. Attached to the parent GameObject
We can find these two concepts familiar to what we've seen in [[Level 5.1 Describe Rotation#transform.rotation|quaternion]]. 

To get mouse input, we need to again use Input method. And we need to associate the increments with deltaTime for smoothness:

```csharp
float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity * Time.deltaTime;
float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity * Time.deltaTime;
```

Both name of the GetAxis can be found inside the <mark class="hltr-blue">Input Manager</mark> under <mark class="hltr-orange">Project Settings</mark>.

![[Pasted image 20240520234917.png|center|500]]

```csharp
        float moveX = Input.GetAxis("Mouse X") * mouseSensitivity * Time.deltaTime;
        float moveY = Input.GetAxis("Mouse Y") * mouseSensitivity * Time.deltaTime;
		//Yaw axis
        playerBody.Rotate(Vector3.up * moveX);
	    //Pitch axis
	    pitch -= moveY
        transform.localRotation = Quaternion.Euler(pitch, 0, 0);  
```

> [!warning]
> Be careful that you don't need to rotate the parent object for pitch

> [!important]
> Also notify that a float variable pitch is used to store current rotation amount. If you remember from Euler angle, every rotation is a description of transformation, meaning if you don't store this value, the computer will have to **calculate the rotation in each frame separately all start from initial status**.

To avoid overflow of mouse movement, we also need to limit the value from user input. Using a `Mathf.Clamp` method can help this out.

```csharp
pitch = Mathf.Clamp(pitch, -90f, 90f);
```

Also, we might like to have the cursor locked inside the screen center.

```csharp
        Cursor.visible = false;
        Cursor.lockState = CursorLockMode.Locked;
```
### Character Controller
#### Moving Behavior
Character controller is a unique component that can be applied to an object in unity.

![[Pasted image 20240521035409.png|center]]

We need character controller for both TPS and FPS, or any game that required single character control. 

The traditional Doom-style first person controls are not physically realistic. The character runs 90 miles per hour, comes to a halt immediately and turns on a dime. Because it is so unrealistic, use of Rigidbodies and physics to create this behavior is impractical and will feel wrong. The solution is the specialized Character Controller. It is simply a capsule shaped Collider
 which can be told to move in some direction from a script. The Controller will then carry out the movement but be constrained by collisions. It will slide along walls, walk up stairs (if they are lower than the Step Offset) and walk on slopes within the Slope Limit.

> [!NOTE]
> The Controller does not react to forces on its own and it does not automatically push Rigidbodies away.

If you want to push Rigidbodies or objects with the Character Controller, you can apply forces to any object that it collides with via the `OnControllerColliderHit()` function through scripting.

On the other hand, if you **want** your player character to be **affected** by physics then you might be better off using a Rigidbody instead of the Character Controller.

> [!tip]
> **Don’t get stuck**
> The Skin Width is one of the most critical properties to get right when tuning your Character Controller. If your character gets stuck it is most likely because your Skin Width is too small. The Skin Width will let objects slightly penetrate the Controller but it removes jitter and prevents it from getting stuck.

It’s good practice to keep your Skin Width at least greater than 0.01 and more than 10% of the Radius. Recommend keeping Min Move Distance at 0.

Following properties are critical for setting character controller:
- **Slope Limit**
	- Limits the collider to only climb slopes that are less steep (in degrees) than the indicated value.

- **Step Offset**
	- The character will step up a stair only if it is closer to the ground than the indicated value

- **Skin Width**
	- Two colliders can penetrate each other as deep as their Skin Width Larger Skin Widths reduce jitter

- **Min Move Distance**
	- If the character tries to move below the indicated value, it will not move at all. (use 0)

We can break down the controller script into following parts:
1. Obtain user input to horizontal and vertical variables
2. Generate a `Vector3` variable, and calculate this vector based on user inputs and objects location at <font color="#c0504d">current frame</font>
3. <font color="#c0504d">Normalized</font> `Vector3` variable to make it a unit vector to designated position, this makes speed **consistent on all direction**.
4. Applied the `Vector3` value to `controller.Move` 

```csharp
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");

        Vector3 input = transform.right * moveHorizontal + transform.forward * moveVertical;

        controller.Move(input.normalized * Time.deltaTime * speed);
```
#### Jumping Behavior
Like mentioned above, physical forces is not applicable for player controller. Therefore, we need other ways to simulate gravity that is essential for jumping behavior. 

We can use `isGrounded` method to determine the location of the player, to determine if jumping feature is applicable for player or not.

To manually simulate gravity force, we need formulas that help us out on this:
$$
h_{jump}=\frac12\frac{v(0)^2}g
$$
We may use this to calculate the upward movement of the character.

$$
d=\frac12gt^2
$$
And use this distance formular to help simulating falling behavior due to gravity.