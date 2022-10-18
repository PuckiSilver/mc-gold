# **mc-gold**
With this project you can add loot to other loot tables without overwriting them.

## **Overview**
This pack **only works if a player is opening the container**, so breaking the container or placing a hopper underneath it only gives items of the original loot table.
Loot will be inserted into **both chest types, barrels and all shulker boxes**.

## **Configure**
At the top of the [main.bolt](src/data/gold/modules/main.bolt) file, you can see the global variables `lootTables`, `insertTables` and `yourNamespace`.
Fill these in with your details.
```python
lootTables = [ # add a loot table to add items to
    "minecraft:chests/simple_dungeon",
    "minecraft:chests/bastion_treasure",
]

insertTables = [ # add the loot tables that will be added to the loot table with the same index
    [ "myCoolPack:artifact", "myCoolPack:weapon" ],
    [ "minecraft:chests/buried_treasure", "minecraft:blocks/diamond_block" ],
]

yourNamespace = "myCoolPack" # the namespace the insersion functions can be found under for compatibilty reasons
```
In this example the loot tables `myCoolPack:artifact` and `myCoolPack:weapon` will be added to normal dungeons using the `minecraft:chests/simple_dungeon` loot table and the `minecraft:chests/buried_treasure` and `minecraft:blocks/diamond_block` loot tables can also be found in chests using the `minecraft:chests/bastion_treasure` loot table.

The pack also uses the specified `myCoolPack` namespace to insert the loot

## **Technical Stuff**
Why do I see other Items flashing when opening a chest?
- The pack has to wait for the loot table to generate before modifying the chests contents

## **Dependencies**
This project uses [beet](https://github.com/mcbeet/beet), [bolt](https://github.com/mcbeet/bolt) and [mecha](https://github.com/mcbeet/mecha).
