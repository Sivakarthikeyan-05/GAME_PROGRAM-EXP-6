# GAME_PROGRAM-EXP-6

### NAME    : Sivakarthikeyan V

### REG. NO.: 212225220098

---

# EXP: 6

## AI Random Roam with Chase – Unreal Engine

## Aim

To create an AI character in Unreal Engine that randomly roams within a defined **NavMesh** area and chases the player when the player comes within a specified detection range, using **Behavior Trees, Blackboard, and AI Perception**.

---

## Procedure

### 1. Setup Navigation

* Add a `NavMeshBoundsVolume` to the level.
* Scale the volume to cover the area where the AI should roam.
* Press **P** to visualize the navigation area.
* The navigable area should appear in green.

---

### 2. Create AI Character

* Create a Blueprint Character, for example:

`BP_AIEnemy`

* Assign a suitable **Skeletal Mesh** to the character.
* Create an AI Controller Blueprint:

`BP_AIController`

* Assign `BP_AIController` as the AI Controller Class for `BP_AIEnemy`.

---

### 3. Enable AI Perception

In `BP_AIController`:

* Add an **AI Perception** component.
* Add and configure the **Sight** sense.
* Set appropriate values for:

  * Detection Range
  * Lose Sight Range
  * Peripheral Vision Angle

Use the `OnPerceptionUpdated` event to detect the player and update Blackboard values such as:

* `CanSeePlayer`
* `PlayerActor`

---

### 4. Set Up Blackboard

Create a Blackboard named:

`BB_AI`

Add the following keys:

| Key              | Type    | Purpose                                  |
| ---------------- | ------- | ---------------------------------------- |
| `TargetLocation` | Vector  | Stores the random destination            |
| `PlayerActor`    | Object  | Stores the detected player               |
| `CanSeePlayer`   | Boolean | Determines whether the player is visible |

---

### 5. Create Behavior Tree

Create a Behavior Tree named:

`BT_AI`

Use the following structure:

```text
Root
└── Selector
    ├── Sequence (Chase Player)
    │   ├── Blackboard Check: CanSeePlayer == true
    │   └── Move To: PlayerActor
    │
    └── Sequence (Random Roam)
        ├── Task: Find Random Location → TargetLocation
        └── Move To: TargetLocation
```

The **Selector** gives priority to chasing the player whenever the `CanSeePlayer` condition becomes true. Otherwise, the AI performs random roaming.

---

### 6. Create Custom Task: Find Random Location

Create a new **BTTask_BlueprintBase** task.

The task should:

1. Get the AI character's current location.
2. Generate a random reachable point within a specified radius.
3. Store the resulting location in the `TargetLocation` Blackboard key.

The Unreal Engine navigation system can be used with:

```cpp
UNavigationSystemV1::GetRandomReachablePointInRadius()
```

The Behavior Tree then uses `TargetLocation` with the **Move To** node to move the AI to the randomly selected position.

---

### 7. Test the AI

* Add a player character to the level.
* Place the AI enemy in the game world.
* Assign the appropriate AI Controller.
* Assign the Behavior Tree to the AI Controller.
* Ensure the `NavMeshBoundsVolume` covers the required area.
* Press **Play** to test the AI behavior.

### Expected Behavior

```text
AI Starts
   ↓
Random Roaming
   ↓
Player Enters Sight Range
   ↓
AI Detects Player
   ↓
Chase Player
   ↓
Player Leaves Sight Range
   ↓
Resume Random Roaming
```

---

## Output

### AI Random Roaming

![AI Random Roam](https://github.com/user-attachments/assets/8cc1b432-1809-4fb2-b82c-842d16b2f651)

<br>

### AI Behavior Tree / Blackboard

![AI Behavior Tree](https://github.com/user-attachments/assets/aac9fada-353e-4369-a7a1-8037059117b9)

<br>

### AI Chase Behavior

![AI Chase Behavior](https://github.com/user-attachments/assets/0017652a-93f4-4168-b375-6389bb48b189)

---

## Result

The AI character was successfully implemented to **roam randomly within a defined NavMesh area**.

When the player enters the AI's sight range, the AI detects the player using **AI Perception**, stops random roaming, and begins chasing the player.

When the player moves out of the AI's sight range, the AI resumes its **random roaming behavior**.

Thus, the **AI Random Roam with Chase system using NavMesh, AI Perception, Blackboard, and Behavior Tree** was successfully implemented in Unreal Engine.
