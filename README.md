# Introduction

Who likes music? I like music, you sure must like music if you clicked this! Or maybe you're just someone whos my title interested?...

Either way! We all know music is an essential component to every Roblox experience. Audio can shift in many ways how a player reacts to a situation, area or in general how much they like the game because of it!

Lately I've been working again on an old project of mine where I have used [ZonePlus](https://devforum.roblox.com/t/zoneplus-v320-construct-dynamic-zones-and-effectively-determine-players-and-parts-within-their-boundaries/1017701) and some mathematical calculations to bring a module that everyone can use to various needs. This was a more of a heavy extension to [Kitsune's](https://devforum.roblox.com/t/area-music-player-script-open-source/656954) already existing region music module however I noticed that it does not really support multiple parts or any complex shapes. I as well improved some elements on how detection is handled.

## Usage
This module allows users to customize areas bounded by points or individual blocks, however some things must be understood before using it to make sure you are using it correctly.

**THIS IS SO FAR ONLY FIT FOR CLIENT USAGE, PREFERABLE IF YOU PLACE IT INSIDE `StarterPlayerScripts`**

**Modes**
* Multiple - Using hand placed points to simulate an area like a trapezium, square, smooth square etc..
* Object - Using a whole part similar to how Region3 works but you can you a part that you created already.

Here's a simple breakdown of how to call it simply from the module:

```
local EasySound = require(script.Parent:WaitForChild('EasySound')).new()
local Music = script.Parent:WaitForChild('EasySound').SoundGroup

-- EXAMPLE [EasySound:AddSound(Your sound object, Mode as a string, Volume, BindableEvent as an Instance, Angle of accuracy as an integer, Y position as an integer, Direction as a string]
EasySound:AddSound(Music.CoolSound, 'Multiple', 1, game.ReplicatedStorage.Event, 300, nil, 'Up')

-- EXAMPLE 2 [EasySound:AddSound(Your sound object, Mode as a string, Volume, BindableEvent as an Instance]
EasySound:AddSound(Music.CoolSound, 'Object', 1, nil)
```

- If you are using Object mode, you just need a single part under the sound that will play, it can be scaled as high or wide as you want and placed where you need it to be.

- **If you are using Multiple mode, you will need to use small part nodes to create your shape, you then need to label every part with a number from 1 - How much parts there are in the created area.**

## More in depth usage

**BindableEvents**
- You can use `BindableEvents` to connect with entering any zone, this can be used to change atmospheres, send rendering signals for the area or anything that you desire!

**Angle, Ypos, Direction** -- MULTIPLE MODE EXCLUSIVE
- Due to how the multiple mode operates and how buggy it can be at times I decided to let users have feature to specify their wanted accuracy of detection for it.

- In more easy to say term, the script calculates your character's angle between points and if its more or is the specified angle the script counts you as inside a shape, for example a square shape would work best if given angle of accuracy was somewhere around 300 or so degrees.

- Because multiple mode doesn't care at what Y position you are at, it will detect you inside even if you are lets say 300 feet above your so thought zone. Because of this Ypos is sort of the Y position you have to be at for it to detect you as inside. 

- Lastly Direction heavily links with Ypos parameter. If Direction is specified as `Up` the zone will detect anything above the given Y position, similarly if you specify `Down` for the direction it will detect anything thats below the certain Y point. 

- **Summary**
Angle: Can be any positive integer value, lower it is the less degree of accuracy is used to see if you're inside.

  Ypos: Min Y world co-ordinate needed to check if you are within the wanted Y position, if left as nil it will be set as 0

  Direction: Links to Ypos, if direction is `Up` anything above the Y point within bounded angle of accuracy area will be detected. If direction is `Down` anything below the Y point within bounded angle of accuracy area will be counted, **if left nil it will detect you from below and above of the given area**.

## Some examples

Lets say I want to use the `multiple` mode for an area, I would set it out like this:

```
local EasySound = require(script.Parent:WaitForChild('EasySound')).new()
local Music = script.Parent:WaitForChild('EasySound').SoundGroup

EasySound:AddSound(Music.CoolSound, 'Multiple', 1, game.ReplicatedStorage.Event, 300, nil, 'Up')
```
Lets go in parameter by parameter.

1st parameter is my sound Instance I used
2nd parameter is my selected mode, `Multiple` in my case.
3rd parameter is the volume I want to play it at when I enter the zone.
4th parameter is optional, it can be left as nil but I binded a BindableEvent to entering and leaving the zone. 
5th parameter is my angle of accuracy
6th parameter is my Ypos, in this case because It's nil it will be set as Y point of 0.
7th parameter is direction I want it to detect anything in, it being anything above Y point of 0.

Lets try the object mode:

```
local EasySound = require(script.Parent:WaitForChild('EasySound')).new()
local Music = script.Parent:WaitForChild('EasySound').SoundGroup

EasySound:AddSound(Music.CoolSound, 'Object', 1)
```

Like before lets go into it bit by bit.

1st parameter is my sound instance
2nd Parameter is my mode, `Object` in this case.
3rd Parameter is my wanted volume when I enter the zone for music to play at.

Because my mode is Object, all other parameters don't need to be set as they aren't gonna be needed, I decided to not bind a BindableEvent to this sound class so I didn't specify a 4th parameter which would be a BindableEvent if you did want to bind one for when you enter/leave the area.
