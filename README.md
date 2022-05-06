# MS_source_code_learning
In order to learn source code of pre bigbang ms, I'm a fan of MS through out these years

I'm just a big fan of old MS and TODAY(2022.5.6) I want to dig into the source code of old MS and I found this [GIT][git-source] , I'm quit thrilled and buckle up to learn through it ,in on way, I can improve my coding skills, in another, maybe(thank god if I can) I can found some "bugs" and to improve them!

## Day01:
### Start from "gambling"!

What's the most exciting thing in MS? OFC gachapon! According to my recent private old maple story playing(no advertising), Like xxroyals, xxlengends, xxtruely, the mushroom shrine and new leaf city contains chaos scorlls and white scrolls and have `profitable` values, so I decide dig into 

#### IN
```sh
src/server/gachapon/MushroomShrine.java
```
and I found this class extends `GachaponItems` in GachaponItems.java. This abstract class has three abstract methods and one final methods call `public final int[] getItems(int tier) {}`,so the next step is related to find the var `tier` because is related to what kind of items we can get from gacha! so I found another .java in the same repo.

#### IN
```sh
src/server/gachapon/MapleGachapon.java
```

The code 
```sh
private static final MapleGachapon instance = new MapleGachapon();
```
and
```sh
public static MapleGachapon getInstance() {
		return instance;
	}
```
is a lazy creation if I make no mistakes, so in multihreads, is safe but is waste of memory(maybe I'm wrong, please to point out if I was wrong!)

The class contains another constants class `Gachapon`, lists out 13 kinds of constants gachapon(according to the name, it definetly relating to the place), the constructor need npcid, common, uncommon, rare, and gachaponItems, here we found the 3 class(common, uncommon, rare) are all int.

Another method is `private int getTier()` , and inside it user `Randomizer.nextInt()` method, and in tools/Randomizer, the class is base on java.util.random, at mean time, the 3 vars in above constructor is the percentage, the common is 90%, uncommom is 8%, and rare is 2%(wtf this rate, no wonder I usually get the junk!). 

Look back to the mushroomshrine.java, we can see the `getRareItems()` method returns int [] {1102084, 3010019}, and I found out they are  `1102084 - Pink Gaia Cape - (no description)` in `/handbook/cape.txt` and `3010019 - Kadomatsu - A specially-made chair that's only available in Mushroom Shrine. Recover 60 MP every 10 seconds.` in /handbook/chair.txt (no chao scrolls when I searched, no gamble game...really? :D).

```sh
public int [] getItems(int tier)
```
This method is also related to tier, and return the true item(finally) we gacha. But pay attention ,this method return the array, which means the two items if u get rare(very lucky!), so how to decide which exact item you get? 

```sh
public int getItem(int tier){}
```
In this method, we can see that var gacha is array, global is a array too. When i look into the global, 
`2049100 - Chaos Scroll 60% - Alters the equipment for better or worse. Not available on Cash Items.\nSuccess rate:60%`
BINGO!!! The final item you get is localize gacha and global gacha! Wait, which means that all the place you can get chaos rather than just one specific place?

finally, the method
```sh
public MapleGachaponItem process(int npcId) 
```
is to process and really return the luck(the item) you gacha(MapleGachaponItem),this class contains the id and the tier, both int.

In next day, I want to explore how the player move and contact with the npc(ofc gacha machine first :D).

[git-source]: <https://github.com/ronancpl/HeavenMS>
