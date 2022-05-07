# Day 02
Ola! Today, I gonna explore how we(player) move and interact with npc( looking forward to find gacha :P )

## All begin with Object

When I searched through the whole `server` file, I Found NPC or player should implements from `server/maps/MapleMapObject.java`...

### IN
```sh
server/maps/MapleMapObject.java
```
There are only one interface `MapleMapObject`, and another original package is Point from java.awt, this class extends from Point2D class and Serializable, So now I assumed that is a 2d(ofc MS is 2D) and can be transfered.

There are several methods like get and id, get `MaplerMapOBjectTye`, get position, send the spawn and destory data( maybe its related to the mobs?)

### IN
```sh
server/maps/MapleMapObjectType.java
```
I found that NPC, monster, player, item... are all constants in `MapleMapObjectType`, so my search instincts are right~! :D

## Decide starting from "static" and "easy" Object: NPC...

### IN
```sh
server/life/MapleNPC.java
```
this object extends `AbstractLoadedMapleLife` class, and this abstract class extends from `AbstractAnimatedMapleMapObject`, this class extends from `AbstractMapleMapObject`, and this class extends above `MapleMapObject`. Bluh bluh, in a word, it's also a object but can do some animations, which emplements `AnimatedMapleMapObject`

### IN
`AbstractAnimatedMapleMapObject` 
#### Additional
There is a tool call `MaplePacketLittleEndianWriter`, from the name we can see that this refers to the network transmission. Little endian vice verse Big endian, which is original from the two types of CPU, long time ago, there are just assembly language and machine code, so in order to run program in different type of CPU(intel and power PC), the data stores in different way.

Look into `MaplePacketLittleEndianWriter`, comment says `Writes a maplestory-packet little-endian stream of bytes`, and extends from `GenericLittleEndianWriter`, and this class implements the method from `LittleEndianWriter`

In these class, there is a method `public void write(int b) ` get my attention, while write `int` to the Byte, what if the 
int var is out of scope(-128-127)? I tried out on my computer, just get the lowest significant position value, for example:

int = 255 byte = -1
int = 256 byte = -128

This src code provider said is mostly based on java7, my computer is java14, so I test online, if int > 127 or int < -128, it just came out a error. But at least I found a little "BUG"(I just call it , not a really but :P )


### IN
```sh
server/maps/AbstractMapleMapObject.java
```
I saw that `MapleMapObject` or `MapleNPC` or extends from `AbstractMapleMapObject`, several methods we can set and get the object's id, position and `MapleMapObjectType`
