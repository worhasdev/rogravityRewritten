# ROBLOX's default gravity system, rewritten
## Roadmap and design plan

This originally started as a project developed by myself for a smaller development group that required "slope physics" that replicate those seen in many of the popular "Sonic The Hedgehog" titles,
as well as the gravity physics seen inside of Super Mario Galaxy.

I took on the challenge myself, using various methods and decided to provide a solution to the public, that they can open source and use. 
Anyone who uses this gravity controller/system must credit me, Krish "worhas".

**I will be updating this system every now and then, fixing bugs and making things more efficient.**

### General plan

- Step 1: Figure out how to rewrite ROBLOX's gravity as a whole, and prevent the player from being affected by ROBLOX's workspace gravity. *In basic terms, we set the Gravity property to 0*
- Step 2: Enforce custom velocity handling, such that the player can return to a grounded state after a jump or whenever they touch the ground. In addition, this acts as the new gravity.
- Step 3: Figure out a method that detects when a player should have their velocity forced down to the perpendicular vector of the normal of the surface that the player finds themselves above, and orient the player accordingly
- Step 4: Make everything efficient, and work smoothly
  

### References

*Sonic Physics Example*
![sonic06glitch](https://github.com/worhasdev/rogravityRewritten/assets/131204733/31741f2b-ec40-4a56-a20b-17235b9b8ddd)

*Super Mario Galaxy Physics Example*
![838187-915692_20071113_005](https://github.com/worhasdev/rogravityRewritten/assets/131204733/6b80c027-3501-4ae1-97d8-f2a71ba0c3ac)


## Theory and design plan
*This will go over pretty much everything that makes the system tick*



### Vectors, Velocities and Gravity


### Rays


### Detection (falls under rays)



## Documentation

*TBD*
