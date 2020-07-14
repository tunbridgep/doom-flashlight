# Flashlight Mod #

## About ##

This is designed to be an "all in one" solution for those who may have used [DarkDoom](https://www.moddb.com/games/doom/addons/dark-doom) and other mods in the past, who want an easy but configurable solution to playing a darker, more spooky version of Doom, or who want a new way of playing a mapset they are very familiar with. This mod relies heavily on zscript and is designed exclusively for GZDoom. Zandronum support will never be implemented.

## Feature Overview ##

This mod has the following features, all of which are designed to be optional and fully customisable:

- A fully-functional handheld flashlight, using GZdoom's Spotlights. This doesn't simply place a dynamic light on the ground where you're looking, it has a proper beam. The flashlight uses a battery which automatically recharges, and can have it's capacity upgraded through dynamically-placed pickups.
- Light-Level alteration, using various algorithms for different effects. This can make the map very dark or give more contrasting light levels in maps. The algorithms used are thoroughly explained through the lighting document available in the docs directory, however more documentation is needed.
- Profiles allowing for completely changing mod functionality with the press of a button.
- Usable flares that light up the environment and burn-out after a configurable time.
- Full customisation of all features including flashlight and flare colors, gameplay effects, light levels, and other preferences.
- "Ambushes", allowing monsters in dark areas to be replaced with stealth variants (many people hate stealth monsters, keep in mind this is completely optional)
- Uses ZScript event handlers and modifies existing actors dynamically based on parameters. No actors are replaced. Should be inherently compatible with most mods. Thoroughly tested with Brutal Doom and Project Brutality, both of which work perfectly. It is designed to be as compatible as possible - any undesired interaction with any other mod, no matter how trivial, should be considered a bug.

## Why Does This Exist? Doom is an Action Game! Making it Dark Destroys the Balance! ##

Maybe. This mod is configurable for a reason. Still, I am open to balance suggestions and changes.

This started as an exploration of zscript and it's features, especially with dynamically modifying actors through script, such as applying the bStealth flag. It eventually became something viable, without me even realising it.

## Installation ##

Premade release versions are ready to go. The repository doesn't contain anything other than the core source files, with all generated/compiled files not included.

If you wish to build it yourself you can do so using the following steps:

1. Compile ACS Scripts. This can be done using [acc](https://zdoom.org/wiki/ACC)
2. Create an include file for all files in the zsc directory. Zscript includes are discussed in detail [here](https://zdoom.org/wiki/ZScript)
3. [Zip the contents of the src directory as a pk3](https://zdoom.org/wiki/Using_ZIPs_as_WAD_replacement), then run in GZDoom using the -file parameter or using another method.

Alternatively, for a much quicker build experience, a config file is included for use with my [doom_quickbuild](https://github.com/tunbridgep/doom_quickbuild) utility.

## Detailed Feature Breakdown ##

TBD. This section should include a thorough description of every option and console command. Contributions are welcome. Of special importance is documenting the darkening algorithms and the compatibility options for glowing textures, as these are not self-explanatory and require some detail.

## Missing/Incomplete Features ##

- The battery display kind of sucks. The numbers are supposed to show flares, battery upgrades, and battery recharges, but it's not clear what everything is. The entire battery display probably needs a redesign. I am open to ideas.
- The mod needs thorough testing. There is no guarantee it will work properly out of the box. Some of the default values probably need some tweaking, and some descriptions updated. This is an alpha for a reason.
- Sometimes, seemingly at random, performance will drop in some areas if the beam is on. This is rare and doesn't tank performance all that badly, so it's not a major issue, but I would still like to resolve it. It may be a gzdoom issue though.

## Planned Features ##

- Support for other iwads, such as Heretic/Hexen. The mod technically already works with them, however no items are dropped/replaced, and a flashlight doesn't really fit into the style of the game. Planned solution is to create a "medieval themed" flashlight, such as a magic wand.
- A headlamp, effectively a flashlight that lets you continue to use your weapons while it's on. Likely a more permanent replacement for the night-vision goggles.
- A thorough code rework as a lot of it needs cleaning up, especially relating to console variables and profiles.

## Contributing ##

There are many ways to contribute to the project. The easiest way is by filing a bug report or feature request, however code and documentation contributions are welcome. Documentation of the various features and lighting modes is especially important. All pull requests are welcome, however code should generally follow these rules:

1. ZScript should always be preferred over DECORATE and ACS. This mod generally tries to only use ACS in situations where it makes sense or where functionality is undocumented/impossible in zscript. A good example of this is replacing textures, which is very easy in ACS and virtually undocumented in zscript.
2. All console variable interactions should have an associated function in their respective Config class. See the existing examples for more information. Currently there are only a handful of these, as most of the mod was written before this idea was finalised. Routing all console interactions through these objects has a few benefits, namely that console command names can be relatively easily changed and things like range checking can be enforced.
3. All features should, wherever possible, try to do things in ways that do not involve any existing actor. Doing things like replacing an enemy with a stealth variant is done by applying the STEALTH flag to the monster directly, not by replacing them with a separate actor. CheckReplacement is used to ensure that when item replacements occur, they affect the correct items. Most other mods implement features by implementing new actors that use the "replaces" keyword or by other hacky means, and that should be avoided here as compatibility is important. Actor class checks for actors from other mods should use the isClass function inside ActorUtil, rather than using the '[is](https://zdoom.org/wiki/ZScript_special_words)' keyword, as this mod will not load if the 'is' keyword is used for an actor in a mod which is not loaded.
4. Almost no thought has been put into multiplayer. This mod has not been tested at all with multiplayer. It is not required for contributions to consider multiplayer, however some consideration would be appreciated. A good example of small considerations is considering whether a console command should exist within the user or server space, and whether or not ACS scripts should have the "net" keyword.
5. Consideration should be made for hubs. There is not a significant number of popular hub maps for Doom, however this mod is intended to eventually support Hexen, at which point hub map support will be required. A new event handler called FlashlightEventHandler was made specifically to handle this situation, which implements it's own UniqueWorldLoaded function, designed to be a hub-friendly version of WorldLoaded. You should use this as a base for all EventHandlers that need to do something when a map is loaded. This is important to prevent hub maps from being darkened multiple times as players enter and leave them, for example.

## Credits ##

Credits are available via the in-game menu. Detailed credits are available in CREDITS.TXT. Below is a brief list of the assets used and their authors

- [Flashlight by Steve](https://forum.zdoom.org/viewtopic.php?t=59429)
- [Flare by scalliano](https://www.realm667.com/index.php/en/item-store-mainmenu-169/others-mainmenu-172/1001-flares#credits)
- [Flashlight by idGamer](https://realm667.com/index.php/en/armory-mainmenu-157-97317/doom-style-mainmenu-158-94349?start=30#description-2)
