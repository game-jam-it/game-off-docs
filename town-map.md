# Small Town Map

As the player is selecting a character to play a town map is generated for the game.

## Game Design

### Prepphase

> MVP: Skips this phase: The player is presented with the town map and possible expeditions to choose from.

The player uses downtime to move across the map and or gather resources.

### Expedition

The player starts an expedition by selecting the location on the town map.

---
## Technical

Two map types get generated procedurally where the town map contains multiple event maps. While there is some overlap there is a split between the required functionality and the visual representation of the underlying information.

When a new game starts the town map is generated and populated with a pseudo-random selection of event nodes. When a play selects an event node on the town map and starts an expedition the map zooms in to the node as the details get generated and drawn onto the screen.

High-level object overview:
```mermaid
classDiagram
TheTown --> TownMap
TheTown --> TownGrid
TheTown --> EventGrid
TheTown --> EventMap

TownMap *-- TownNode
TownGrid *-- TownHex

EventMap *-- EventNode
EventGrid *-- EventHex

TownMap <--> TownCreator
TheTown --o TownCreator
TownGrid <-- TownCreator

EventGrid <-- EventCreator
TheTown --o EventCreator
EventMap <--> EventCreator

TownMap : path []
TownMap : road []
TownMap : nodes {}
TownMap : get_node(vec2) 
TownMap : set_node(vec2)

TownGrid : path {}
TownGrid : road {}
TownGrid : grid HexGrid
TownGrid : get_hex(vec2)
TownGrid : get_path(from, to)

EventMap : path []
EventMap : road []
EventMap : nodes {}
EventMap : get_node(vec2)
EventMap : set_node(vec2) 

EventGrid : path {}
EventGrid : road {}
EventGrid : grid HexGrid
EventGrid : get_hex(vec2)
EventGrid : get_path(from, to)

TownHex : type
TownHex : barrier
TownHex : obstacle
TownHex : coords
TownHex : position

TownNode : size
TownNode : links
TownNode : region
TownNode : coords
TownNode : position

EventHex : type
EventHex : barrier
EventHex : obstacle
EventHex : coords
EventHex : position

EventNode : size
EventNode : type
EventNode : group
EventNode : coords
EventNode : position
```

---
## Visual

### The Town Map

A 2D map showing roads, points of interest, and region indicators. The center of the town is more urban areas, more to the outside it gets rural, and on the outskirts, there may be more abandoned/unused areas. The map is managed as a hexagonal tilemap. This means around 42 tiles to map all possible edges and transitions between types.

Requires all tilesets needed to cover regions, roads, paths, building types, and the surrounding forest.

### The Expedition Map

As with the town map, the base is a hex map that is more detailed than the town map and has more 2.5D visual aspects. The base textures also need 42 tiles to map all possible edges and transitions between types. Exceptions are that some of the detail or building types may only have edges on top the or bottom of a hex.

Several tilesets are required per type of setting and region on the map.

---
**Ref.** Tilemap for hex based grids
**Note:** To reduce the workload layered hard edges can be used where suited. Meaning that an edge tile covers another tile to create the transition. The result is that the top edge tile texture can be used to define multiple transitions without having to manually create all maps.
![](/static/ref-hex-tilemap.png)