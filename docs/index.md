Code RPG
=============

Getting Started
---------------

Welcome to Code RPG, an RPG that gives you full creative control over your players actions. 

Code RPG uses C# code compiled written by you, compiled at runtime, to access details about the games characters, and to instruct the player. 

Docs will help with that!

Quick Start
----------------

### First Steps

I'm going to assume the game is running! 

By now, you will be able to see the 'Core.cs' tab at the top of the page. Click on it to open the code input. 

In here, you will see a basic script already in there, that looks like this: 

```cs
public class DynamicPlayer : RuntimePlayer
{
	public CharacterRuntimeBridge target;

	public void Setup () 
	{
		UpdateTarget();
		player.MoveTo(target.Position);
	}

	public void Act () 
	{
		// player.Log.Add("Act Complete");
	}

	void UpdateTarget () 
	{
		string id = "Training Dummy 01";
		target = game.FindCharacter(id);
	}
}
```

Lets go through this step by step. 

There are three functions in this script, Setup(), Act() and UpdateTarget(). 

Two of these, Setup and Act are referred to as 'inbuilt' functions. These are run by the game, not by you. You just specify what they do! 

#### Setup

Setup is run when the code is first compiled (after you click the run button). It's useful for making sure your code has all the things it needs to perform.  
In this example, Setup is running the UpdateTarget function, and then telling the player to move to the targets position. 

#### Act

Act is run once every frame. It's useful for things that need to happen again and again. 

#### Update Target

UpdateTarget is a function that has been made inside of this piece of code! It is there to show you not to be afraid to make your own methods, and to experiment. 

For now, what UpdateTarget does is to search the game for a character named "Test 01", and save that to a variable named 'target'. 

### Running a Script

This code is very basic, but it should work!  
To have a go, click the 'Run' button up the top right of the code window, then click on the 'Core.cs' text to minimise the code window. 

For your immersion, we made it take a while to compile the code :) 

But no, really, the more complicated your code is, the longer compilation will take, welcome to being a programmer! 

Once your code has finished compiling, you should see your player start moving toward another character! 

### Making Changes

As a starting script, thats not bad, but let's get you going with some 'advanced' behaviour. 

What if we want the player to attack the dummy? 

It feels like we could expand start to say...

```
player.MoveTo(target.position);
player.Attack(target.ID);
```

but unfortunately, this wouldn't work. 

Why? 

#### * Moving to a target (attackRange, while & Wait())

First off, your player can't teleport! So we have given them a target to move towards, but they will take time to reach the position you gave them. 
So we need to Wait() for them to reach the target. 

To do this, we use a 'while' loop. This is IMPORTANT! Never use a while loop without a Wait() in it, or you will crash your game! 

In your Setup function, add this: 
```cs
public void Setup () 
{
    UpdateTarget();
    player.MoveTo(target.Position);
    while (Vector3.Distance(player.Position, target.Position) > player.attackRange)
    {
      Wait();
    }
}
```

We've introduced a few new things here! 

Vector3.Distance is a function that will tell you how far it is between two positions. Here, we are seeing how far it is from the player to the target.  
We can access the position of the player and the target with .position because they are both 'Characters'. You can access the position of any Character you reference!

We are comparing the distance from the player to the target with another variable frome the player: player.attackRange.  
This variable is how far away a target can be before the player can't reach it.  

To state it concisely: we are waiting until the enemy is in range for us to hit! 

This takes time though, and we want to keep checking as time passes. This is what the Wait() function does.  
This function will make the player wait a frame until checking again.  
If we forgot Wait(), it would make the player try to check again and again inside the same frame, crashing the game. 

This is generally considered bad. 

#### * Attacking a target (Attack(), Cooling and attackCooldown)

Now we are in range, we want to attack our target! 

We can do this with 
```cs
player.Attack(target.ID);
```

But we are going to run into another problem.  
The player can't attack instantaneously, so we need to wait for them to recover from their attack before we attack again. 

Luckily, the player has all the information we need to make this work. 

There are two variables we need to check.  
player.Cooling is a boolean variable (true/false) that tells us whether the player is currently cooling down from their strike.  
player.attackCooldown is a float variable (decimal number) that tells us how long the player needs to cool down after they finish their strike. 

So there are actually two ways you could make this work! 

Option 1: This option will wait for the player to stop cooling before striking again

```cs
public void Setup () 
{
    UpdateTarget();
    player.MoveTo(target.Position);
    while (Vector3.Distance(player.Position, target.Position) > player.attackRange)
    {
      Wait();
    }
    
    player.Attack(target.ID);
    while (player.Cooling)
    {
    	Wait();
    }
    player.Attack(target.ID);
}
```

Option 2: This option tells the game to Wait() for a specific amount of time (the cooldown time) before trying again. 
```cs
public void Setup () 
{
    UpdateTarget();
    player.MoveTo(target.Position);
    while (Vector3.Distance(player.Position, target.Position) > player.attackRange)
    {
      Wait();
    }
    
    player.Attack(target.ID);
    Wait(player.attackCooldown);
    player.Attack(target.ID);
}
```

### Good job! 

You are become death, he who strikes twice and then goes to sleep! 

I am going to write more tutorials for this program, but later. For now, enjoy using the documentation to figure out how to do cool stuff!
