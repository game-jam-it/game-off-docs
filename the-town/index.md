## Intro

The game takes place in a fictional town in the United States. A small suburb with a school, some stores, a summer camp, and other amenities, it even has a few abandoned environments such as a sawmill, or dock.

## Setup

At the beginning of every run, a map is generated for the player to explore. The town is split up into three typical regions, an urban center, with suburb housing around it, and rural locations outside of those areas.

For every region, the town has multiple sectors/nodes for the player to explore. Most of these sectors are random nodes, while some are quest-specific nodes that can be substantially larger.

![](/static/refrence-art/TheTown-hexmap-concept.png)

---

## Events

Most nodes on a town map is randomly selected out of a set of events for the region the node is in, while some of them are fully handcrafted others can have randomized elements in them. At the same time, some nodes are just preparation nodes used to travel, stock up or learn info, others are expedition nodes where the player has to explore the sector to find items or defeat a specific threat.

Finally, some nodes have both, which may or may not be available depending on the state of the game.

*Based on the state of development the game has a list of available events to pick from.*

#### MVP Version

* Start in the Haunted House (Intro)
* Next to the Cemetary (Tutorial)
* The final Girl (End Boss)

#### Demo Version

* Start in the Haunted House (Intro)
  - -> Outer
* Next to the Cemetary (Tutorial)
  - -> Suburb
* Pick your path: (Town Map)
  * Outer:
    - Wood(s)
  * Center:
    - Shop(s)
    - Hospital
  * Suburb:
    - Home(s)
    - School(s)
* The final Girl (End Boss)
  - -> Suburb

#### Final Version

* Start in the Haunted House (Intro)
  - -> Outer
* Next to the Cemetary (Tutorial)
  - -> Suburb / Outer
* Pick your path: (Town Map)
  * Outer:
    - Camp(s)
    - Farm(s)
    - Mall
    - Mill(s)
    - Mine(s)
    - Wood(s)
    - University
  * Center:
    - Shop(s)
    - Townhall
    - Hospital
    - Restaurant(s)
    - Apartment(s)
  * Suburb:
    - Park(s)
    - Home(s)
    - Store(s)
    - School(s)
* The final Girl (End Boss)
  - -> Suburb / Outer
