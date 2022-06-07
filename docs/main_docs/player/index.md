---
permalink: /main_docs/player/
---
# Player Documentation

## Character : CharacterRuntimeBridge

Variables:
```cs
get {} string ID; // Unique identifier for all characters

get {} string Log.Value; // Retrieve the current Log output
get {} set {} int Log.Size; // Retrieve the current capacity of the log

get {} Vector3 Position; // Current world position of the character

get {} float CurrentHealth;
get {} float MaxHealth;
get {} float CurrentEnergy;
get {} float MaxEnergy;

get {} float AttackPower; // Damage output
get {} bool Cooling; // Is the attack cooling down?
get {} float AttackCooldown; // Attack cooldown length
get {} float AttackRange; // Maximum attack range in meters
```

Methods: 

```cs
void Log.Add(string message); // Register a message with the characters log. Messages over Log.Size will be discarded
void Log.Clear(); // Remove all messages from the Log
```
## Player : Character

Variables:

```cs
get {} int Level;
get {} float CurrentExp;
get {} float MaxExp;
```

Methods:

```cs
void MoveTo (Vector3 position); // Attempt to move to the specified position
void Attack (CharacterRuntimeBridge target); // Attempt to attack the specified target
void Attack (string _ID); // Attempt to attack the target with specified ID
```
