# Datapack Knowledge Base

A collection of useful information and guidelines for datapack development.

## Guidelines

### 1 Style Guidelines

#### 1.1 Files and Folder Structure

Files and folders should be named in **snake case** (all lowercase separated by underscores).

Functions should be split into **internal functions**, located inside the namespace `_datapackname` and **external functions**, meaning functions that are intended to be used by a player or other datapack, located inside the namespace `datapackname`.

The **load** function and **main** loop should both be located in the **internal namespace** and should be named `load.mcfunction` and either `tick.mcfunction` in case of the main loop being triggered every tick (not recommended see [2.1 Scheduled Main Loop](#21-Scheduled-Main-Loop)) or `main.mcfunction` in every other case.

Functions should be grouped in folders named after their **common functionality**. For example getter functions for various items could be grouped inside a folder named `get` not only organizing them but also shortening the individual file names by removing the `get` prefix.

#### 1.2 Code Style

For naming **storage paths, scoreboards, custom nbt tags and fake entities** should **snake case** (all lowercase separated by underscores) be used.

The **storage namespace** should be named `creator:datapack_name` in case of groups of datapacks belonging together you can also use `creator:group_name`. Under no circumstances should the `minecraft` namespace be used.

Fake entities should be prefixed with a `#` (This also prevents them from showing up in the scoreboard sidebar) and named in the pattern `namespace.entity_name`, `author.entity_name` or `datapack_group.entity_name`.

**Tags** and **scoreboards** should be prefixed with the **creator**, **collection name** or **namespace** like this `creator.tag_name`.

**Custom data** for items and markers should be grouped inside a **creator dict** containing a **namespace dict**. For example `give @s diamond[minecraft:custom_data={creator:{datapack:{data:1b}}}]` and `summon marker ~ ~ ~ {data:{creator:{datapack:{data:1b}}}}`

### 2 Performance

#### 2.1 Scheduled Main Loop

In most cases the execution of the main loop every tick is unnecessary, therefore the `schedule function` command should be used at the end of the code to create a loop that is executed in a **regular time interval**. For example `schedule function _datapack:main 10t`. This loop has to be **started** by calling the function once during the **initial load**.

#### 2.2 Filtering selectors

To prevent code from being executed for every loaded entity. The usage of the `@e` selector should always be **filtered** using a **entity type** and if possible also a **tag**. Narrowing down the selector to a **dimension** and a smaller number of **chunks** also greatly improves performance. This can be done by using the **spacial filters** like `distance` or even better `dx`, `dy` and `dz`.

##### 2.2.1 NBT Checks

NBT checks are one of the most **expensive operations** in datapacks. Use them carefully and prevent repeating them by **grouping** the actions into a **function** called with the selector. Using [predicates](#222-Predicates) also improves the performance of nbt checks so consider using them instead.

##### 2.2.2 Predicates

In case of using the same or similar selectors repeatedly consider using predicates. They offer a more filtering possibilities than ordinary selectors and have better performance. Using a [generator](https://misode.github.io/predicate/) makes them only a little bit more work.

### 3 Datapack Registration

A datapack should register themselves at two places. One is a [datapack advancement](#31-Datapack-Advancement) which lets **users** see which datapacks are installed. The other is the [datapack storage](#32-Datapack-Storage) offering **other datapacks** the possibility to check for installed datapacks and their version.

#### 3.1 Datapack Advancement

For the datapack advancement you need three files in your datapacks `/data/global/advancements/` directory.

##### 3.1.1 Root Advancement

First is the **root advancement** at `/data/global/advancements/root.json` with the following content:

```json
{
    "display": {
        "title": "Installed Datapacks",
        "description": "",
        "icon": {
            "item": "minecraft:knowledge_book"
        },
        "background": "minecraft:textures/block/gray_concrete.png",
        "show_toast": false,
        "announce_to_chat": false
    },
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

##### 3.1.2 Creator Advancement

Second is the **creator advancement** at `/data/global/advancements/<creator>.json` used for grouping your datapacks.

This can also be altered using a **team name and icon** for datapacks made by **multiple people**.

```json
{
    "display": {
        "title": "<Your name>",
        "description": "",
        "icon": {
            "item": "minecraft:player_head",
            "nbt": "{SkullOwner: '<your_minecraft_name>'}"
        },
        "show_toast": false,
        "announce_to_chat": false
    },
    "parent": "global:root",
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

##### 3.1.3 Datapack Advancement

Third is the **datapack advancement** at `/data/global/advancements/<creator>/<datapack>.json` holding the information about your datapack.

```json
{
    "display": {
        "title": "<datapack name>",
        "description": "<datapack description>",
        "icon": {
            "item": "<item>"
        },
        "announce_to_chat": false,
        "show_toast": false
    },
    "parent": "global:<creator>",
    "criteria": {
        "trigger": {
            "trigger": "minecraft:tick"
        }
    }
}
```

##### 3.1.4 Datapack Collections

For multiple datapacks belonging together you can also create subfolders to the `/data/global/advancements/<creator>/` directory. Creating a new **creator advancement** with a different team name is also a possibility.

##### 3.1.5 MC Datapacks

[Section 3.1](#31-Datapack-Advancement) is heavily inspired and also compatible with the [MC Datapacks - Datapack Advancement Convention](https://mc-datapacks.github.io/en/conventions/datapack_advancement.html)

#### 3.2 Datapack Storage

_TODO_

### 4 Datapack Uninstallation

_TODO_
