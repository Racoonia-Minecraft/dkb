# Datapack Knowledge Base

A collection of useful information and guidelines for datapack developement.

## Guidelines

### 1. Style Guidelines

#### 1.1 Files and Folder Structure

Files and folders should be named in **snake case** (all lowercase seperated by underscores).

Functions should be split into **internal functions**, located inside the namespace `_datapackname` and **external functions**, meaning functions that are intended to be used by a player or other datapack, located inside the namespace `datapackname`.

The **load** function and **main** loop should both be located in the **internal namespace** and should be named `load.mcfunction` and either `tick.mcfunction` in case of the main loop being triggered every tick (not recommended see [2.1 Scheduled Main Loop](#2.1-Scheduled-Main-Loop)) or `main.mcfunction` in every other case.

Functions should be grouped in folders named after their **common functionality**. For example getter functions for various items could be grouped inside a folder named `get` not only organizing them but also shortening the individual file names by removing the `get` prefix.

#### 1.2 Code Style

For naming **storage paths, scoreboards, custom nbt tags and fake entities** should **snake case** (all lowercase seperated by underscores) be used.

The **storage namespace** should be the name of the **datapack creator** or in case of a group of datapacks belonging together a **group name**. Under no circumstances should the `minecraft` namespace be used.

Fake entities should be prefixed with a `#` (This also prevents them from showing up in the scoreboard sidebar).

### 2. Performance

#### 2.1 Scheduled Main Loop

In most cases the execution of the main loop every tick is unnecessary, therefore the `schedule function` command should be used at the end of the code to create a loop that is executed in a **regular time interval**. For example `schedule function _datapack:main 10t`. This loop has to be **started** by calling the function once during the **initial load**.

#### 2.2 Filtering @e

To prevent code from being executed for every loaded entity. The usage of the `@e` selector should always be **filtered** using a **entity type** and if possible also a **tag**.

#### 2.3 Marker
