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
*Disclaimer: This is purely for those who have the intent of editing this system in order to achieve radically different outcomes from the systems main intentions, or for those who are simply just interested.*

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

### Control Script



### Rays, Velocities and Vectors

Since the main development plan was to follow OOP principles in order to organise the code, such that it has better expandability and better understandablilty, I created a utility module script, that contains more than enough functions to have the physics achieve the primary goal, orientation and stick. I will elaborate further on why we have more functions than what is needed later on.

See https://create.roblox.com/docs/reference/engine/classes/ModuleScript for more information on modulescripts.

I first began, with Vectors in mind. Primarily, you'd want to be able to manipulate vectors by performing basic arithmetic on them; hence we create methods that handle basic arithmetic for the vectors that will be used in order to manipulate the position or velocity of the player.

```lua
utils.Vector3 = {}
utils.Vector3.new = function(x, y, z) --primarily used for diagnostics
	return Vector3.new(x, y, z)
end

utils.Vector3.addX = function(vec, x)
	return Vector3.new(vec.x + x, vec.y, vec.z)
end

utils.Vector3.addY = function(vec, y)
	return Vector3.new(vec.x, vec.y + y, vec.z)
end

utils.Vector3.addZ = function(vec, z)
	return Vector3.new(vec.x, vec.y, vec.z + z)
end

utils.Vector3.subtractX = function(vec, x)
	return Vector3.new(vec.x - x, vec.y, vec.z)
end

utils.Vector3.subtractY = function(vec, y)
	return Vector3.new(vec.x, vec.y - y, vec.z)
end

utils.Vector3.subtractZ = function(vec, z)
	return Vector3.new(vec.x, vec.y, vec.z - z)
end

utils.Vector3.multiplyX = function(vec, x)
	return Vector3.new(vec.x * x, vec.y, vec.z)
end

utils.Vector3.multiplyY = function(vec, y)
	return Vector3.new(vec.x, vec.y * y, vec.z)
end

utils.Vector3.multiplyZ = function(vec, z)
	return Vector3.new(vec.x, vec.y, vec.z * z)
end

utils.Vector3.divideX = function(vec, x)
	if x == 0 then
		return Vector3.new(0, vec.y, vec.z)
    error("Cannot return vector value when divided by nil") --throw an error for dividing a vector by zero
	end
	return Vector3.new(vec.x / x, vec.y, vec.z)
end

utils.Vector3.divideY = function(vec, y)
	if y == 0 then
		return Vector3.new(vec.x, 0, vec.z)
    error("Cannot return vector value when divided by nil") --throw an error for dividing a vector by zero
	end
	return Vector3.new(vec.x, vec.y / y, vec.z)
end

utils.Vector3.divideZ = function(vec, z)
	if z == 0 then
		return Vector3.new(vec.x, vec.y, 0)
    error("Cannot return vector value when divided by nil") --throw an error for dividing a vector by zero
	end
	return Vector3.new(vec.x, vec.y, vec.z / z)
end
```

Now that the Vector methods are defined under their own construct table named "Vector3", we can move onto the math methods, which in themselves are pretty extraneous, except for one which is inadvertently required. (The reason I say it is inadvertently required is because it is not expected to be used anywhere but is vital to the system.)

```lua
utils.Math = {}
utils.Math.clamp = function(value, min, max)
	return math.max(min, math.min(max, value))
end

utils.Math.lerp = function(start, finish, alpha)
	return start + (finish - start) * utils.Math.clamp(alpha, 0, 1)
end
```
EDIT: As of 01/09/2023 the math functions have been stripped down to only what is necessary for the functions of the system.

As of the removal of any unnecessary bulk in the math construct table, the only thing that remains is the lerp. Lerping or a "lerp", is an abbreviation for Linear Interpolation, which simply put is the process of smoothly transitioning between one point to another, based on an "interpolant" which is a constant clamped between 0 and 1. The constant acts as a fractional value that finds that specific point between the two **start** and **finish** values. To learn more about linear interpolation, and the math behind it: https://en.wikipedia.org/wiki/Linear_interpolation

To conclude with the utilities script, I had to add some form of raycast handling, in order to detect and change player states, among other things.

```lua
utils.Raycast = {}
utils.Raycast.raycast = function(point, direction, whitelist, water)
	local ray = Ray.new(point, direction)
	local hit, pos, nor, mat = workspace:FindPartOnRayWithWhitelist(ray, whitelist, water)
	return hit, pos, nor, mat
end

utils.Raycast.raycastPoints = function(point, to, whitelist, water)
	local ray = Ray.new(point, to - point)
	local hit, pos, nor, mat = workspace:FindPartOnRayWithWhitelist(ray, whitelist, water)
	return hit, pos, nor, mat
end

utils.Raycast.raycastAll = function(point, direction, whitelist, water)
	local ray = Ray.new(point, direction)
	local hits = workspace:FindPartsInRayWithWhitelist(ray, whitelist, water)
	return hits
end

utils.Raycast.raycastAllPoints = function(point, to, whitelist, water)
	local ray = Ray.new(point, to - point)
	local hits = workspace:FindPartsInRayWithWhitelist(ray, whitelist, water)
	return hits
end
```

Under the Raycast construct table, there are methods that conduct raycasts in different ways. These will all be useful in handling the detection for the physics.

*There are string manipulation methods present in the utilities module script, but they aren't worth mentioning nor explaining since they serve no real purpose as of now.*

**Next, the explanation of the theory behind the physics, how they work, and how they are expansive.**

When it comes to vectors and forces, we must understand the basic logic behind how the system would work.
Applying a downwards force on the player towards the floor, as they orient based on the surface they stand on generally would be the best way to go about solving the gravity.

Establishing public player variables that can be manipulated by any script, which will then effect the player --> wip



*TBD*

## Documentation

*TBD*
