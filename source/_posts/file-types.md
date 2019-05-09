---
title: File types
date: 2019-05-05T13:53:05.597Z
updated: 2019-05-05T13:53:07.012Z
toc: true
category:
  - 'CD:RE SDK'
  - Overview
---
Crashday has a lot of different files used for different things. Generally there are two types: text and binary ones. Binary files can only be edited using special programs, plugins or the game itself. Text files can be opened with any text editor.

All the text files are based on the same format:

Every line is important. The game will look for a certain variable on a certain line position, so don't add any additional lines etc.

After every line you can add a comment using **\#** symbol. Everything after it on the same line will be ignored by the game.

CD uses a dot to represent decimal numbers e.g.  `1.23`

Colors are represented as RGB, channels are separated with a space e.g. `255 255 255` is white color.

# Text based files

## .amb Ambience definition file

path: `content/ambience/`

Ambience files are used by the game to change weather. In general it is a way to change how light, reflections and sounds work in the level.
Here is a cleaned up version of CD's `day.amb`

```
PropsFX-Ambience-File   # defines a start of an .amb file
-                       # name of ambience (overwritten by localized data for default ambiences)
ambience/day		# texture set for the ambience (lut, background, skybox, sun). First part is the folder name inside textures folder, second is the start of the file name the game will look for(day_bg, day_lut etc.)
219 186 149 		# sun color
1			# maximum sunlight strength
2            		# power exponent of sun light
97 111 131 		# ambience sky color
45 49 54         	# ambience ground color
0 0 0                   # absolute ambience minimum
255 217 175 		# specular reflection color
170 170 170	        # darker dense vegetation render color (e.g. treetops)
190 190 190		# brighter vegetation render color (e.g. grass)
0 -0.6 -0.8 		# sun light vector
90			# additional rotation of sun vector
1.0 1.0 1.0 1.0  	# screen gamma
1			# apply gamma to sky?
55 55 55 	    	# texture color filter
0.0		 	# strength of color filter
0.1			# strength of color filter on car (defines how much the car's color is "greyed out", to not oversaturate)
use_colormap
3			# number of ramp points
0->0 127->128 255->255	# ramp point 1
0->0 127->128 255->255
0->0 127->128 255->255
69 78 91 		# fog color for looking into normal scenery
0 0.5 		        # fog amount (min,max) looking into normal scenery
93 103 115 		# fog color for looking into sun haze
0 0.5			# fog amount (min,max) looking into sun haze
ambience/day.wav        # environment sound in sounds folder
0 0.2 0.8		# sunflare vector
255 228 173		# sunflare color
300 300 		# sun corona width/height
5 5 5			# object shadow color
0.4			# max shadow alpha density
1.0 1.0 1.0 		# reflections: min/max/fresnel scaling coefficients
0		        # turn on headlights for cars
255 255 255		# color tint for the lensflares/coronas in this ambience. 255,255,255 means no tint. 0,0,0 makes flares invisible
70 78 78		# the color for the bottom of the scene background
0               	# rotational angle to correct the envmapping directions
0                	# [optional] force music off (useful when hacking envsound with a soundtrack)
```

## ladder.lst Career opponent list file

path: `content/career/ladder.lst`

Ladder list defines the names of opponents in career mode. Opponent names in quickplay can be found in translations files.

```
Crashday-CareerLadder   # defines a start of ladder.lst
21			# number of opponents
1                       # Randomize opponent names?
James|Hamilton amateurs # Name|Surname league
George|Rush amateurs
Emmett|Brown amateurs
Kevin|Hale amateurs
Donald|Spikes amateurs
Mia|Tsukamoto amateurs
Steve|Jarvik amateurs
Percy|Kay amateurs
Waldo|Silver professionals
Isaac|N. professionals
John|Williams professionals
Jules|Ash professionals
Robert|Patch professionals
Joe|Eastman professionals
Kanzan|Tsukamoto professionals
Vincent|Dust final
Maurice|Kane final
Lance|Cartwright  final
Ryan|Whitney final
Tyreece|Moore final
\1THE|INCUBATOR lastmission
```

## .lst Shop list file

Shop lists are used to display what can be bought in the shop. There are three main lists: 

* `content/shop/cars/`**`carname`**`/` items will show up for the given car. In these files you want to specify vinyls, car perf upgrades etc.
* `content/shop/cars/`**`weapons.lst`**is a list of available weapons in game and will show up for every car under weapons category. Usually you don't need to edit this, unless you want to change some weapon's shop stat.
* `content/shop/` items will show up for all cars. Generally the only thing you want there are wheels

Every .lst files consists of a first line and then any amount of entry blocks separated by an empty line.

```
Crashday-ShopDataList #defines a start of an .lst file
```

an example of an entry block:

```
Next:  #---------------------------------------------
crsh1			# identifier name
crashpower		# group (car, fbumper, rbumper, sidekit, hood, rwing, wheels, performance, weapon, skin, cassis, armour, crashpower)
NONE			# parent object (e.g. car folder) or NONE
$ID trkdata/cars/apachee/shop.lst crsh1.Name		# Article name (English)
DEPRECATED         	# UNUSED article description (English)
aftercarbought    	# availability condition
0			# UNUSED show media _before_ available
NONE		        # UNUSED Media showed when available (can be NONE)
8000			# cost to buy in shop
NONE	        	# UNUSED shop picture
0			# Is this a stock car part?
0                       # !IMPORTANT! - Is mod content? This must be set to 1 if you are creating additional items for the shop. Used to properly save kits - Outblast.
```

If you want to add any part to the car, not only you have to have it in the model and listed in carinfo.cca, you will also need to create an .lst entry for that part. For every new car you also have to create a car entry, usually looking like this:

```
Next:  #---------------------------------------------
*currentcar*		# identifier name
car			# group (car, fbumper, rbumper, sidekit, hood, rwing, wheels, performance, weapon, skin, cassis, armour, crashpower)
NONE			# parent object (e.g. car folder) or NONE
$ID trkdata/cars/apachee/shop.lst apachee.Name		# Article name (English)
DEPRECATED         	# UNUSED article description (English)
withrespect 1370	# availability condition
0			# UNUSED show media _before_ available
NONE		        # UNUSED Media showed when available (can be NONE)
152000			# cost to buy in shop
NONE	        	# UNUSED shop picture
0			# Is this a stock car part?
0                       # !IMPORTANT! - Is mod content? This must be set to 1 if you are creating additional items for the shop. Used to properly save kits - Outblast.
```

In every block, if the .lst file was loaded in the car folder you can use _\*`currentcar*`_ text. When the game process the file it will automatically replace it with the cars identifier.

There are three possible availability conditions

* **withrespect \[number]** This item will unlock after **number** amount of respect is gained.
* **aftercarbought** This item will be unlocked when you buy a car. Can only be used for items with specified parent object.
* **always** This item will by always unlocked.

> nowhere it is specified what car is used, so i guess the parent object is used for **aftercarbought** condition. But seeing how its named parent object originally and not car parent, makes me think we could define another item or something else as parent? Stackable objects or smth?

## .tex Texture shader file

path: `found in the same folder as any texture you use. Has the same name as the given texture, but the .tex format.`

These files define how textures are rendered in game.

```
has_alpha                #defines if the texture has opaque places. If it does not, comment out that line or leave it blank. !The line still should be there!
# disable_mipmapping     #enable or disable mipmaps?
default			 # ingame material type. Options: default, grass, tree, standard or STANDARD. If you choose standard, the file ends here.
diffuseenvmap            # shader type
0.3			 # minimum reflection
0.7			 # maximum reflection
3			 # fresnel exponent
```

There are multiple possible shader types. Each of them defines how the following lines are read. Next up you will see a list of shader types followed by their used variables.

```
default        # simple diffuse lighting (default shading type)
```
```
additiveblend  # additively blended textures
```
```
specularvertex # diffuse lighting with specular reflection map
ultra          # minimum graphics quality to activate shader(if user has lower setting, diffuse will be used) Possible options: "ultra", "high", "medium" or "low"
_self_         # specular map file. _self_ to use alpha channel of the texture, or name of another texture. e.g. colwhite.tga
0.7            # specular overall strength
2              # specular power exponent
0.9            # diffuse sun light strength
```
```
diffuseenvmap  # diffuse lighting with environment mapping
0.3            # minimum reflection strength
0.7            # maximum reflection strength
3              # fresnel exponent
```
```
chrome         # chrome shader(great job explaining what it actually is)
1              # amount of chrome blended against diffuse
```
```
use_shaderparams_from # reference to other .tex file to pick shader specified there
[filename].tex        # tex file we copy the shader settings from
```

> These were taken from the old sdk, but looking at the reversed Crashday source code, these were also found: diffuse(possibly one parameter, should be the same as default), specularmapping(first variable is graphics setting, then something else?), alphatest, alphatestdoubleside, alphadoubleside