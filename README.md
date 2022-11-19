# **mc-gold**
With this project you can add loot to other loot tables without overwriting them.


## **Overview**
This beet project gives you the opportunity to add to foreign loot tables without modifying them in any way.

A full data pack gets generated, that can directly be merged with your own project and your pack automatically will be compatible with all other packs using mc-gold!


## **Configure**
1. At the top of the [main.bolt](src/data/gold/modules/main.bolt) file, you can see the variables `lootTables`, `insertTables` and `yourNamespace`.
Adjust, remove and add to these to achive your desired result.

    The first loot table in the `lootTables` list corresponds to, and will generate, all the loot tables specified in the first row of the `insertTables` list.

    `yourNamespace` is the namespace of your own pack. This namespace is used for a single `.mcfunction` file to enable compatibility between different data packs using mc-gold.

    ```python
    lootTables = [ # add a loot table to add items to
        "minecraft:chests/simple_dungeon",
        "minecraft:chests/bastion_treasure",
    ]

    insertTables = [ # add the loot tables that will be added to the loot table with    the same index
        [ "myCoolPack:artifact", "myCoolPack:weapon" ],
        [ "minecraft:chests/buried_treasure", "minecraft:blocks/diamond_block" ],
    ]

    yourNamespace = "myCoolPack" # the namespace the insersion functions can be     found under for compatibilty reasons
    ```

    - In this example the loot tables `myCoolPack:artifact` and `myCoolPack:weapon` will be added to normal dungeons using the `minecraft:chests/simple_dungeon` loot table
    - And the `minecraft:chests/buried_treasure` and `minecraft:blocks/diamond_block` loot tables can also be found in chests using the `minecraft:chests/bastion_treasure` loot table.<br><br>

2. Once these variables are configured, **build the project** using [beet](https://github.com/mcbeet/beet).

3. After that, you should **copy the the folder** `gold` from `/build/mc-gold/data/` into the `.../data/` folder of your data pack.

4. You also have to **copy the folder** `gold` from `/buid/mc-gold/data/<yourNamespace>/functions/` into your pack to `.../data/<yourNamespace>/functions/`.

5. Lastly you must run the `gold:_technical/load` function when **loading your pack**.
This can be done by adding it as a new line into your `.../data/minecraft/tags/functions/load.json` file.

## **How it Works**

1. The pack checks when a **player opens a loot table** that should be modified.
    - For this a `player_generates_container_loot` loot table per unique loot table, that should be modified, is used.

2. When the pack detect a loot table that should be modified, it **adds two tags** to the player.
    - A **tag to identify**, that the player should check for a container and
    - The **name and namespace of the loot table** that should be replaced

3. Next an `item_used_on_block` advancement is used to check for the player **opening a chest while having the identifier tag**.
    - This executes right after the other advancement, but still in the same tick.
    - The next steps have to happen a little after the first advancement, because at that time, the chest content has not been generated yet.
    This leeds to items overwriting other items and other jank.

4. A **raycast** is used to locate the chest.

5. The **content of the chest gets removed**.
    - This is done, to stack the items of the original loot table as with this, there can be a lot more different items in the chest.

6. The original loot table and all additional **loot tables get inserted into the chest**.
    - The original loot table gets determined by the tag of the player and
    - Additional loot tables get inserted through a tag calling a function in the packs own namespace. This is done, to allow multiple packs to modify the same original loot table.

7. Lastly, all the chest content gets **shuffled**.
    - If the contents are not shuffled, all the loot would be in order of insertion.
    - This also insures the items to be spread in the chest, even if not all slots are filled.

## **Known Limitations**
This pack only works if a **player is opening the container**, so breaking the container or placing a hopper underneath it only gives items of the original loot table.

Loot will (only) be inserted into **both chest types, barrels and all shulker boxes**.

Once the chest is full, **additional loot tables won't be inserted**. So some loot may not generate if too many loot tables are added using this method.

## **Dependencies**
This project uses [beet](https://github.com/mcbeet/beet), [bolt](https://github.com/mcbeet/bolt) and [mecha](https://github.com/mcbeet/mecha).

---
<sub>check me out on [Planet Minecraft](https://www.planetminecraft.com/member/puckisilver/)
