# ECO CRAFTING
## FiveM QBCore Crafting script

![eco_crafting gallery](https://github.com/Ekhion76/eco_crafting/blob/main/previews/eco_crafting6.jpg)
![eco_crafting gallery](https://github.com/Ekhion76/eco_crafting/blob/main/previews/eco_crafting7.jpg)
![eco_crafting gallery](https://github.com/Ekhion76/eco_crafting/blob/main/previews/eco_crafting8.jpg)

### Features
- Search in recipes
- Easy to understand, recipes are built on eachother, you can go step by step
- Recipe book

- Labor point system. Each time when you craft you will need labor points.
- The used labor points will increase your proficiency level in the category you crafted items.
- "Labor point" item. (An item what gives you labor points, can be given as a present to your players or can be bought)

- Proficiency system
- Proficiency statistics panel
- "Proficiency point" item. (An item what gives you proficiency points, can be given as a present to your players or can be bought)

- You can move the whole UI around your screen
- The recipes can be adjusted to your needs. (jobs can have their on recipes and other players won't be able to see them, you can also block recipes, you can have "special recipes")
- Workstands can be given separately to jobs or players, you can even set the owner as a single player.
- Effects, markers (cp), animations, tables (objects) can be set up for individual tables. You can set them up one by one

- Target system or the regular distance settings are interchangable as a function.
- Multilanguage (hu, en)
- Discord log included

## How does it work?
In order to craft an item you will need ingredients, labor points and the neccesary proficiency level.
Every profession has its own workstand/workstands.
The recipes are categorized as professions. Eg.:
- chemist
- cooking
- weaponry
- foundry
- handicraft(mechanic)
 
The used labor points increases the proficiency level in the profession where you use it and with that you can craft higher level items.
For example you have two restaurants, restaurant1 and restaurant2. If you want restaurant1 to craft items which restaurant2 should not be able to craft then you can set recipes
to be only visible to restaurant1(as a job) or you can create special workbenches.


Here is an example how you can set recipes for two different restaurants in cooking profession:

```lua
Config.craftData = {
    cooking = {
        tosti = { -- everybody can craft it on your server, at every 'cooking' workstand
            labor = 10,
            ingredients = { item1 = 1, item2 = 1 },
            time = 5,
            amount = 1,
            proficiency = 0,
            price = 0
        },
        twerks_candy = { -- everybody can craft it on your server, but only in a speacial 'tacoBomb' workplace
            labor = 10,
            ingredients = { item1 = 1, item2 = 1 },
            time = 5,
            amount = 1,
            proficiency = 0,
            price = 0,
            special = 'tacoBomb'
        },
        snikkel_candy = { -- only the defined job and on a special 'hookies' workplace 
            labor = 10,
            ingredients = { item1 = 1, item2 = 1 },
            time = 5,
            amount = 1,
            proficiency = 0,
            price = 0,
            exclusive = {
                mechanic = { 0, 1 }, -- selected ranks in a job (optional)
                vagos = {}, -- all ranks in a job
                'crips', -- all ranks in a job
            },
            special = 'hookies'
        },
        sandwich = { -- everybody can craft it except police and ambulance
            labor = 10,
            ingredients = { item1 = 1, item2 = 1 },
            time = 5,
            amount = 1,
            proficiency = 0,
            price = 0,
            excluding = {
                "police",
                "ambulance"
            }
        },
    }
}
```

### Recipe structure
```lua
Config.craftData = {
    foundry = {
    
        -- Basic recipe. Yes, that's all, the rest of the default gets value!
        
        aluminum = { -- item what you can create
            ingredients = { aluminumoxide = 3 } -- required
        },
        
        -- Fully optimized recipe
        
        steel = { 
            labor = 5, -- labor point (optional)
            ingredients = { iron = 5 }, -- ingredients = piece !required
            time = 3,   -- needed time to create the item (optional)
            amount = 2, -- ammount of items you will get (optional)
            proficiency = 3000, -- min proficiency level needed (optional)
            price = 0, -- price to craft (optional)
            exclusive = {}, -- jobs where the recipe will be visible / can create the item (optional)
            excluding = {}, -- list of excluded jobs, gangs. If you have the "exclusive" list this part won't work (optional)
            special = 'vehicleParts' -- items can be crafted on a special workplace (optional)
        }
    }
}
```

Default recipe values:

    labor = 0
    time = 10
    price = 0
    amount = 1
    proficiency = 0
    special = nil
    exclusive = nil
    excluding = nil
 


### Exclusive settings
- profession(job) and group (gang) you can add all as list
- if you have them in tables, you need to add the number of the ranks Eg.: mechanic = { 0, 1 }
- If you have an empty table all ranks will work. Eg.: mechanic = {}
- If you have it in strings then all the ranks will work Eg.: 'mechanic'
- The jobs which are not used here will not see the recipes in the recipe book

```lua
Config.craftData = {
    foundry = { 
         steel = {
             -- ...
             -- ...
             -- ...
             -- ...
             exclusive = {
                 mechanic = { 0, 1 }, -- rank list (optional)
                 vagos = {}, -- all ranks accepted
                 'crips', -- all ranks accepted
             }
         }
     }
 }
```

### Excluding 
- only in string! profesions(job) and groups(gang), ranks are not needed here.
- The listed professions and gangs won't be able to see the recipes in the recipe book.
```lua
Config.craftData = {
    foundry = { 
         steel = {
             -- ...
             -- ...
             -- ...
             -- ...
             excluding = {
                 'mechanic', 'vagos', 'crips',
             }
         }
     }
 }
```

### Workstands
Workstands like recipes can be exclusive for different jobs/gangs.
You can create as much workstands as you like. Every profession has its own workstand/workstands.
For every workstand you can define an own marker, animation, object, spcialization and owner.
You can craft all the non spcialized items in the spcialized workstands.

Guide for the plascement of the objects:
https://github.com/Ekhion76/eco_placement

Guide for creating effect markers:
https://github.com/Ekhion76/eco_effect

```lua
Config.workstations = {

    -- Basic workstand
    {
        workstation = 'cooking', -- profession (this is defined at the recipes)
        pos = vector4(226.98, -889.95, 29.7, 70.16),
    },
    
    -- Fully optimized workstand
    {
        workstation = 'cooking', -- profession (this is defined at the recipes)
        pos = vector4(216.98, -889.95, 29.7, 70.16),
        animation = { -- (optional)
            dict = 'anim@amb@clubhouse@tutorial@bkr_tut_ig3@',
            anim = 'machinic_loop_mechandplayer',
            flag = 16
        },
        fx = { -- (optional)
            dict = 'cut_family5',
            name = 'cs_alien_hand_light',
            offset = vector3(0, 0, 1.1),
            loopedAtCoord = { 0.0, 0.0, 0.0, 1.0 },
            loopedColour = { 8.0, 2.0, 2.0 },
        },
        object = 'v_ret_fh_kitchtable', -- (optional)
        special = '', -- (optional)
        exclusive = { "ballas", "vagos" }, -- (optional)
        excluding = { "police", "ambulance" } -- (optional, n.a. if exclusive is used)
    }
}
```

**Side note:** 
All players will see the object (e.g. worKstand) even if the workstand is exclusive, but the marker won't be visible and the player won't be able to interact wtih the workstand.


### Blips
The blips can be set manually in the configuration file because:

- the workstand can be secret
- workstand groups can use only one blip
```lua
Config.blips = {
    {
        name = 'Foundry',
        coord = vector3(1110.3, -2008.43, 30.71);
        sprite = 537,
        color = 42,
        scale = 0.9
    },
    {
        name = 'Street food',
        coord = vector3(-1249.14, -1473.74, 3.28);
        sprite = 537,
        color = 42,
        scale = 0.9
    },
}
```

### Needed extensions, dependencies
 - **The script only uses QBCore 1.1 standard packages, no additional downloads needed**
 - DrawText (standard from qb-core 1.1)
 - qb-target + PolyZone (optional standard)
 - qb-inventory (also tested with lj-inventory)

Always uses the actual pictures from the inventory. The path can be set in the config file.
```lua
Config.imagePath = "https://cfx-nui-qb-inventory/html/images/"
```

### Server exports
 The workstand properties and the profession level can be adjusted and monitored with external scripts.
```lua
exports['eco_crafting']:addLabor((xPlayer or serverId), amount)
exports['eco_crafting']:removeLabor((xPlayer or serverId), amount)

exports['eco_crafting']:addProficiency((xPlayer or serverId), 'proficiency', amount)
exports['eco_crafting']:removeProficiency((xPlayer or serverId), 'proficiency', amount)

exports['eco_crafting']:getLabor((xPlayer or serverId))
exports['eco_crafting']:getProficiency((xPlayer or serverId))
```

### Install
- copy to the resource folder
- refresh
- start eco_crafting
- the script do not use databases (the script uses meta data)

### Target system:
**turning on qb-target:**
```lua
Config.useTarget = GetConvar('UseTarget', 'false') == 'true' -- Uses the server config file values 
```
1. If the workstand has an object allocated then the script will attach the polyBox. 
2. If there is no object attached then the script will search in a 1 meter area from the workstand and if it is possible attaches the polyZone. 
3. If it does not find then it will create a polyBox around the coordinate of the workstand.

For checking it please use:
```lua
Config.debugPoly = true
```

### Attached useful tools
See the QBCore_addition folder. Icons and readme attached.
To make them work create the items in the  **/qb-core/shared/items.lua** file
Copy the icons to the inventory folder **qb-inventory/html/images/** 

- Recipe book (recipe_collection)
- Labor point increase +1000 point (labor_enhancer)
- Proficiency point increase +1000 point (weaponry_enhancer) 

They are introduced in the **server/usableitem.lua** file as an example.
