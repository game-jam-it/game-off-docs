---
label: Technicals
order: 100
---

# Technical Overview

Two map types get generated procedurally where the town map contains multiple event maps. While there is some overlap there is a split between the required functionality and the visual representation of the underlying information.

When a new game starts the town map is generated and populated with a pseudo-random selection of event nodes. When a play selects an event node on the town map and starts an expedition the map zooms in to the node as the details get generated and drawn onto the screen.

### Expedition

The player starts an expedition by selecting the location on the town map.

### Preparation

> MVP: Skips this phase: The player is presented with the town map and possible expeditions to choose from.

The player uses downtime to move across the map and or gather resources.

---

High-level object overview:
```mermaid
%%{init: { 'theme': 'forest', 'darkMode': true } }%%
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
### Select an Event

In the preparation phase, the user is presented with a town map and has a certain amount of time to complete the daily tasks.

> For the MVP the only available progress is expedition events.

```mermaid
%%{init: { 'theme': 'forest', 'darkMode': true } }%%
sequenceDiagram
	participant Mouse
	participant TownEvents
	Mouse-->>TownEvents: enters event node
	TownEvents-->>TheTown:  focus event (coords)
	TheTown-->>Overlay:  focus event (coords)
	Overlay->>Overlay:  display event info
	Mouse-->>TownEvents: leaves event node
	TownEvents-->>TheTown:  clear event (coords)
	TheTown-->>Overlay:  clear event (coords)
	Overlay->>Overlay:  clear event info
	opt when focused
		Mouse-->>TownEvents: click event node
		TownEvents-->>TheTown:  select event (coords)
		TheTown-->>Overlay:  select event (coords)
		Overlay->>Overlay:  open confirm dialog
		alt when confirmed
			Overlay->>Overlay: close confirm dialog
			Overlay->>TheTown:  start select event (coords)
		else when canceled
			Overlay->>Overlay: close confirm dialog
			Overlay->>TheTown:  cancel select event (coords) 
		end
	end
```
---
### Shopping Event
> TODO Implementm Not part of the MVP

### Research Event
> TODO Implementm Not part of the MVP

### Expedition Event

Expedition events start with the player hovering the mouse over an event on the town map and then deciding to select the event. Once the event is started the town map fades out while zooming in and enters the expedition mode.

Entering the expedition mode means the start of the expedition loop. **Where the goal is?**

```mermaid
%%{init: { 'theme': 'forest', 'darkMode': true } }%%
sequenceDiagram
		participant EventsMap
		participant TownEvents
		participant TheTown
		participant Overlay
		Overlay-->>TheTown:  set select event (coords)
		Overlay-->>Overlay:  hide TownHud
		Overlay-->>Overlay:  show EventHud
		TheTown-->>TownEvents:  start event (coords)
		TownEvents-->TownEvents: Hide Town Grid & Gfx
		TheTown-->>EventsMap:  start event (coords)
		EventsMap-->EventsMap: Show Event Grid & Gfx
```

```mermaid
%%{init: { 'theme': 'forest', 'darkMode': true } }%%
sequenceDiagram
		participant EventsMap
		participant TheTown
		participant Overlay
		TheTown-->>EventsMap: start event (coords)
		opt pause menu
			EventsMap-->>Overlay: pause expedition
			Overlay->>Overlay:  open pause dialog
			alt when continue
				Overlay->>Overlay: close pause dialog
				Overlay->>TheTown:  resume expedition
			else when run to safety
				Overlay->>Overlay: close pause dialog
				Overlay->>TheTown:  escape expedition
			end
		end
```


---
#### Generator Configs

###### Config v1
Big Map (+-90 Nodes)
```
var cfg = {
	"zoom": 16,
	"nodes": 192,
	"culler": 0.35,
	"spread": Vector2(620.0, 40.0),
	"type_a_dist": 7680000,
	"type_a_offset": 384,
	"type_b_dist": 25600000,
	"type_b_offset": 512,
	"type_c_dist": 76800000,
	"type_c_offset": 768,
}
```

Small Map (+-60 Nodes)
```
var cfg = {
	"zoom": 14,
	"nodes": 96,
	"culler": 0.25,
	"spread": Vector2(160.0, 20.0),
	"type_a_dist": 5120000,
	"type_a_offset": 384,
	"type_b_dist": 12800000,
	"type_b_offset": 512,
	"type_c_dist": 51200000,
	"type_c_offset": 768,
}
```

###### Config v2
Big Map (+-60 Nodes)
```
const BIG_MAP = {
	"zoom": 16,
	"nodes": 128,
	"culler": 0.35,
	"spread": Vector2(620.0, 40.0),
	"grid_size": Vector2(1280.0, 720.0),
	"center": 7680000,
	"center_offset": 384,
	"outer": 25600000,
	"outer_offset": 512,
	"edge": 51200000,
	"edge_offset": 768,
}
```

Small Map (+-30 Nodes)
```
const SMALL_MAP = {
	"zoom": 14,
	"nodes": 56,
	"culler": 0.25,
	"spread": Vector2(160.0, 20.0),
	"grid_size": Vector2(1024.0, 576.0),
	"center": 2560000,
	"center_offset": 128,
	"outer": 7680000,
	"outer_offset": 256,
	"edge": 25600000,
	"edge_offset": 512,
}
```
