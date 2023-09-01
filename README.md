# ROBLOX gravity system (CURRENTLY INDEV)
## Roadmap and design plan

This originally started as a project developed by myself for a smaller development group that required "slope physics" that replicate those seen in many of the popular "Sonic The Hedgehog" titles,
as well as the gravity physics seen inside of Super Mario Galaxy.

I took on the challenge myself, using various methods and decided to provide a solution to the public, that they can open source and use. 
Anyone who uses this gravity controller/system must credit me, Krish "worhas". Access to the system can be restricted if being misused or resold and or publicised as a holders own work. 

**I will be updating this system every now and then, fixing bugs and making things more efficient.**

### General plan

- Step 1: Figure out how to rewrite ROBLOX's gravity as a whole, and prevent the player from being affected by ROBLOX's workspace gravity. *In basic terms, we set the Gravity property to 0*
- Step 2: Enforce custom velocity handling, such that the player can return to a grounded state after a jump or whenever they touch the ground. In addition, this acts as the new gravity.
- Step 3: Figure out a method that detects when a player should have their velocity forced down to the perpendicular vector of the normal of the surface that the player finds themselves above, and orient the player accordingly
- Step 4: Make everything efficient, and work such that the player sees no visible change in performance
  

### References

*Sonic Physics Example:*
![image](https://github.com/worhasdev/rogravityRewritten/assets/131204733/875fae55-1223-42e9-91d0-4dcd635a3824)


*Super Mario Galaxy Physics Example:*
![838187-915692_20071113_005](https://github.com/worhasdev/rogravityRewritten/assets/131204733/6b80c027-3501-4ae1-97d8-f2a71ba0c3ac)


## Theory and design plan
*This will explain in-depth, everything that allows the system to function and the physics, math and logic behind it all.*
*Disclaimer: This is purely for those who have the intent of editing this system in order to achieve radically different outcomes from the system, or for those who are simply just interested.*

### Vectors, Velocities and Gravity

Gravity:

In order to overwrite ROBLOX's default set gravity, we find the property "Gravity" under the workspace in the explorer. Set this property to 0. Now there will be no gravity to affect the players movement. We can set this either manually or have it referenced by a line in the system as a precautionary setup. This has already been done for you.
```lua
game.Workspace.Gravity = 0
```
![image](https://github.com/worhasdev/rogravityRewritten/assets/131204733/20cc1546-bba1-4b62-b59b-6c55bfd7c6c6)

Following on from this, the player is still able to jump and never return back to the ground. Once the "Jump" property, under the players humanoid is set to "true", a jumping force is applied to the player which sends them upwards. We cannot directly access this force, and we would rather have more customisability with the entire gravity system; we resort to ultimately writing a custom "ControlScript" seperate to the one ROBLOX provides that allows for basic movement and controls that are standard to the ROBLOX player.

Keep in mind the process of making this control script, in my case will allow for any other local or serverscripts to interact with the player as they would usually, without any side-effects.

In order to create a control script, and keep things organised I used an OOP approach. OOP is object - oriented programming. I will not elaborate further onto it, but if you do want to understand more about it please see https://www.lua.org/pil/16.html

### Control Script, Including Vector and Velocity handling:

*TBD*

### Rays


### Detection (falls under rays)



## Documentation

*TBD*
