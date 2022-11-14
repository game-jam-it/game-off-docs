---
label: Graphics
order: 400
---

# Graphical Overview

### Overview Map

A 2D map showing roads, points of interest, and region indicators. The center of the town is more urban areas, more to the outside it gets rural, and on the outskirts, there may be more abandoned/unused areas. The map is managed as a hexagonal tilemap. This means around 42 tiles to map all possible edges and transitions between types.

Requires all tilesets needed to cover regions, roads, paths, building types, and the surrounding forest.


### Expedition Map

As with the town map, the base is a hex map that is more detailed than the town map and has more 2.5D visual aspects. The base textures also need 42 tiles to map all possible edges and transitions between types. Exceptions are that some of the detail or building types may only have edges on top the or bottom of a hex.

Several tilesets are required per type of setting and region on the map.


#### Map Objects

* Obsticals:
	
	Defined by one or more points that make it dificult for actors to enter the hexes under the defined points.

* Hard Obsticals:

	Defined by one or more points that fully block actors from entering the hexes under the defined points.

* Walls and Fencing:

	Defined by a path going from point A to point B, all actors are blocked from entering hexes along the path.


---

**Ref.** Tilemap for hex-based grids
**Note:** To reduce the workload layered hard edges can be used where suited. Meaning that an edge tile covers another tile to create the transition. The result is that the top edge tile texture can be used to define multiple transitions without having to manually create all maps.
![](/static/technical-art/ref-hex-tilemap.png)
