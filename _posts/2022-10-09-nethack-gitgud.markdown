---
layout: post
title: "NetHack Git-Gud: a Tutorial for Complete Beginners"
data: 2022-10-09 22:00:00 +800
comments: true
categories: [Tutorial]
---

*This article was first released on [LUG Planet](https://lug.ustc.edu.cn/planet/2021/09/nethack-gitgud/). Here is the English version.*

#### About the tutorial

This tutorial will only introduce the most basic game operation and very-early-stage strategies, in order to help new players survive the first dungeon level. Quoted contents are supplementaries or fun facts and can be ignored. All game "screenshots" are just direct copy of NetHack's character interface -- I think this way is better despite the loss of colors and bolds. 

#### About the game

NetHack is a classical roguelike game with long history, with Dungeon & Dragon rules and classical roguelike characteristics: random map, permanent death, and insanely high difficulty/complexity. Meanwhile the game includes elements from various fields and cultures, as well as multiple funny messages and ways to die. 

> There're always old players claiming NetHack is not hard after decades of effort -- maybe it's true compared with other roguelike games, but does this matter to you?

The game uses text interface by default, here's a in-game "picture", for you to feel: 

```
This kobold corpse tastes terrible!--More--

                                                       --------
                      ----------                       |......-
                      |........|`######################-......|
                      |........|     -----------       |<.....|
                      |........|     |.........|  #####.....{.|
                      |........-#####|.........-###    --------
                      |........|    #..........|
                      --------|-     -------.---
                             ##
                              ##
                              #
                             #
                     --------.--
                     |$........|
                     |.........|
    ------------    #..........|
    |.....@.d..|    #|..........
    |.....^.....#####|....>....|
    ------------     -----------

Petergu the Stripling          St:15 Dx:14 Co:18 In:9 Wi:7 Ch:10 Lawful
Dlvl:1 $:11 HP:7(16) Pw:1(2) AC:6 Xp:1
```

> There *are* texture modes but not as classic

#### Installation and setup

If you use Linux, probably NetHack can be installed from package sources directly as `nethack` or `nethack-console`. You can download from [official website](https://nethack.org/v366/ports/download-win.html) if on Windows. 

You may want to add these into your `~/.nethackrc` (This is also the default config on Ubuntu. Don't be scared though, it's safe to skip this part). 

```
#
# System-wide NetHack configuration file for tty-based NetHack.
#

OPTIONS=windowtype:tty,toptenwin,hilite_pet
OPTIONS=fixinv,safe_pet,sortpack,tombstone,color
OPTIONS=verbose,news,fruit:potato
OPTIONS=dogname:Slinky
OPTIONS=catname:Rex
OPTIONS=pickup_types:$
OPTIONS=nomail

# Enable this if you want to see your inventory sorted in alphabetical
# order by item instead of by index letter:
# OPTIONS=sortloot:full
# or if you just want containers sorted:
# OPTIONS=sortloot:loot

#
# Some sane menucolor defaults
#

OPTIONS=menucolors
MENUCOLOR=" blessed "=green
MENUCOLOR=" holy "=green
MENUCOLOR=" uncursed "=yellow
MENUCOLOR=" cursed "=red
MENUCOLOR=" unholy "=red
MENUCOLOR=" cursed .* (being worn)"=orange&underline
```

#### Gamestart

You see these after start NetHack: 

```

NetHack, Copyright 1985-2020
         By Stichting Mathematisch Centrum and M. Stephenson.
         Version 3.6.6 Unix, built Mar 18 18:21:43 2020.
         See license for details.


Shall I pick character's race, role, gender and alignment for you? [ynaq] 
```

You can press `n` then choose character's race(human, dwarf, elf, gnome, orc), role(archeologist, knight, and 11 more), gender(male, female), and alignment(lawful, neutral, chaotic), or press `y` to choose a random set. For beginners, Valkyrie, Barbarian, and Samurai are good. Though I think Wizard has more fun. 

> Gender and alignment can sometimes get changed in-game

> Each role in each alignment has a different god. Monk's god are "characters" in Chinese myths: Shan Lai Ching(山海经), Chih Sung-tzu(赤松子), and Huan Ti(黄帝). 

Press `y` to start after selection. If you see `--More--`, which means there's more prompts to be displayed, press space to continue reading. 

> If you see `--More--` after being hit by a moster，the next message will always be `You die...`

```
Hello petergu, welcome to NetHack!  You are a neutral male human Wizard.
--More--




  -----
  |..@|
  |.f(|
  |x..|
  |...|
  |?..+
  -----









Petergu the Evoker             St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:1 $:0 HP:12(12) Pw:7(7) AC:9 Xp:1
```

In the "picture", the `@` sign is you. The `f`(white background in game) is your pet cat. `x` is a monster(grid bug). `+` is a shut door. You'll pick up the meaning of all characters by time, or you can press `/`(read the instructions!) then `/` again, and move cursor to show what's on the map(ESC t o end). 

> Read prompts carefully no matter when. It's the same as when learning Linux command line. 

The lower column contains your stats. For newbies you'll only need to pay attention to HP aka life and AC, Armor Class, which means defense(the lower the better). Temporary stats will also occur there, for example, Blind. You can look up what other stats mean on Wiki. 

> HP falling to zero is only the most boring way to die

Press `i` to open equipment column and see what you're carrying. Space to return. 

```
 Weapons
 a - a blessed +1 quarterstaff (weapon in hands)
 Armor
 b - an uncursed +0 cloak of magic resistance (being worn)
 Scrolls
 i - an uncursed scroll of magic mapping
 j - an uncursed scroll of enchant weapon
 k - an uncursed scroll of remove curse
 Spellbooks
 l - a blessed spellbook of force bolt
 m - an uncursed spellbook of create monster
 Potions
 f - an uncursed potion of gain ability
 g - an uncursed potion of monster detection
 h - an uncursed potion of healing
 Rings
 d - an uncursed ring of fire resistance
 e - an uncursed ring of invisibility
 Wands
 c - a wand of magic missile (0:4)
 Tools
 n - a magic marker (0:55)
 o - an uncursed blindfold
 (end) 
```

Press `Ctrl-X` to see you own attributes that you are self-aware of. Space to page down and return. 

> You are not aware of more other intrinsic attributes and characteristics, which will only be shown under certain events

```
 Petergu the Wizard's attributes:
 
 Background:
  You are an Evoker, a level 1 male human Wizard.
  You are neutral, on a mission for Thoth
  who is opposed by Ptah (lawful) and Anhur (chaotic).
  You are in the Dungeons of Doom, on level 1.
  You entered the dungeon 9 turns ago.
  There is a full moon in effect.
  You have 0 experience points.
 
 Basics:
  You have all 12 hit points.
  You have all 7 energy points (spell power).
  Your armor class is 9.
  Your wallet is empty.
  Autopickup is on for '$' plus thrown.
 
 Current Characteristics:
  Your strength is 9.
  Your dexterity is 8.
  Your constitution is 16.
  Your intelligence is 17.
 (1 of 2)
```

OK, these are "static" operations. Because NetHack is a turn-based game, you can view these infomation at any time and think carefully before performing any actions. 

Then, the task is to live on. Because anyway, the aim of the game is to recover the Amulet of Yendor from dungeons deep and sacrifice it to your own god, instead of standing still. 

#### Real game begins

First, **move**. Recommended is Vim-like 8-direction `hjklyubn`. As new player using 4 arrow keys are... not impossible. 

> Actually, using 4 arrow keys to move is as good as dead. Holding a key down also commonly results in death. 
>
> If want to move long distance in one direction, use capital `HJKLYUBN` instead. 

Like, press arrow down or `j` to move next to door. When we are moving, `x` was killed by cat. 

> `Rex misses the grid bug.  Rex bites the grid bug.`
>
> `Rex bites the grid bug.  The grid bug is killed!`


> Press `Ctrl-P` to re-view previous messages. 


> It's not uncommon for your pets be stronger than you in early game. 

```
 -----
 |..<|
 |..(|
 |..f|
 |...|
 |..@+
 -----
```

**Open** the door to go out. Press `ol` to open door on the right(`l` direction). Then we can explore to the right. Walking along passage(`#`) until there's no path. 

```
-----
|..<|
|..(|
|...|
|...|
|...-#
-----#
     ###
       #
       #
       ###f
          @
```

This is also common(show-stopper for newcomers): we need to search to find path. Press `s` to **search**, or, `10s` to search 10 times. If still no path we search another 10 times. Found hidden door and get into new room. 

```
You find a hidden door.





  -----
  |..<|
  |..(|
  |...|
  |...|
  |...-#
  -----#
       ###
         #
         #
         ###f
            @+
```

There's a `[` in the room, walk onto the item and `,` to **pick up**. `p - a splint mail.`. It's an armor, press `w` to **wear**, then `?` to select what to wear, `p` for the new armor. `You cannot wear armor over a cloak.`, first should **take off** cloak. Press `T` and cloak's taken off, wear armor again, then `W` to put on the cloak back. Our AC now lowered to 3 -- nice armor. 

> We are lucky. If you find AC rising, probably the armor is cursed. And you can't take off cursed stuff. 

```
                                             ----------------
                                            #@d.............|
  -----                                     f|...............           ....
  |..<|                                  #### ..............|
  |..(|                                  ####+ .............+
  |...|                                  ##    --------- ----
  |...|                                  ##
  |...-#                                 ##
  -----#     ---------                   ##
       ###   |.......|      -----+----  ###
         #   |.......|      |.........###
         #   |.......|      |.......{|####
         ####|........######-........|
            #-.......|      |........|
             |.......|      |..>.....|
             ---------      |........|
                            ----------

Petergu the Evoker             St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:1 $:0 HP:10(12) Pw:7(7) AC:3 Xp:1
```

Continue adventure, door opened and seen a jackal. Directly press right-moving key `l` to **attack** it. After a few move jackal's killed and we are at only 8 HP. 

> Meanwhile of course, hitting any encountered monster without thinking is not good strategy. 

> If you hit a frozen eye face to face, you'll be frozen for a few turns and killed by passer-by trivial monsters. 

> Press `.` to stay still and **rest**, and slowly recovery lost HP. 


```
You kill the jackal!  You hear water falling on coins.



                                             ----------------
                                            f@%.............|
  -----                                     #|...............           ....
  |..<|                                  #### ..............|
  |..(|                                  ####+ .............+
  |...|                                  ##    --------- ----
  |...|                                  ##
  |...-#                                 ##
  -----#     ---------                   ##
       ###   |.......|      -----+----  ###
         #   |.......|      |.........###
         #   |.......|      |.......{|####
         ####|........######-........|
            #-.......|      |........|
             |.......|      |..>.....|
             ---------      |........|
                            ----------

Petergu the Evoker             St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:1 $:0 HP:8(12) Pw:7(7) AC:3 Xp:1
```

Message said the jackal has been killed. Meanwhile you heard some noise. Yeah, you can say there's audio in NetHack. 

> `You hear water falling on coins.` means there's fountain on this level(`{` in the middle). 

The `%` on the ground is jackal's corpse. You may want to **eat** it because you'll starve to death without things to eat. Move onto `%`, press `e` then choose `y` to eat. 

>`You see here a jackal corpse.`
>
>`There is a jackal corpse here; eat it? [ynq] (n) y`
>
>`This jackal corpse tastes okay.  You finish eating the jackal corpse.`

> Of course you can't eat everything. Some body poison you. If you vomit because of this, you'll be more hungry. While some body make you gain some abilities(poision resistance, cold resistance). If you touch the corpse of cockatrace, you'll turn into stone. 

Now as shown in the status, we are Satiated. 

> Continue eating a lot at this time may chock us to death. 

Meanwhile we find there's a locked door. We don't have tools for this, so have to **kick** it open. Press `Ctrl-D` and direction `h` to kick, several times. 

```
This door is locked.



                                             ----------------
                                            #-..............|            -----
  -----                                     #|.f.............           .....|
  |..<|                                  ####|..............|
  |..(|                                  ####+@.............+
  |...|                                  ##  ----------- ----
  |...|                                  ##
  |...-#                                 ##
  -----#     ---------                   ##
       ###   |.......|      -----+----  ###
         #   |.......|      |.........###
         #   |.......|      |.......{|####
         ####|........######-........|
            #-.......|      |........|
             |.......|      |..>.....|
             ---------      |........|
                            ----------

Petergu the Evoker             St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:1 $:0 HP:9(12) Pw:7(7) AC:3 Xp:1
```

We kicked a lot of times to get the door open. Meanwhile two wolves came, the first was one-shot by our cat, then second was also one-shot by our cat. 

>`This door is locked.`
>
>`WHAMMM!!!  Rex misses the jackal.`
>
>`WHAMMM!!!  Rex bites the jackal.  The jackal is killed!`
>
>`WHAMMM!!!`
>
>`WHAMMM!!!`
>
>`WHAMMM!!!`
>
>`As you kick the door, it crashes open!  The jackal bites!`
>
>`Rex misses the jackal.`
>
>`You miss the jackal.  The jackal bites!  Rex bites the jackal.`
>
>`The jackal is killed!`

>Noticed the partially displayed room on the left of the picture? Because we just passed by the door from a room next to it. Thus due to optics, we can only see part of the room. A character-based interface doesn't mean there's no ray tracing. 

After exploring the 1st floor, we got some other amulets and rings. We are at **Burdened** state because we picked too much things up(and the heavy armor). Go on to the `>` tiled, press `>` key to **go downstairs** into the next layer of dungeon. 

> Be burdened is really suboptimal. But I, a collector, just don't want to throw anything away. 

> `You fall down the stairs.`

> If your pet is within one block to you when getting down, it will go down will you. Otherwise it'll be left on the previous floor, and gradually turns wild. 

At this stage, there's already something unknown in our bag. 

>`r - a sapphire ring`
>
>`t - a pyramidal amulet`

**Identify** things is important and hard in NetHack. The naive method is try that yourself: `P` - `?` - `t` to put on the amulet. Nothing happened... Don't know what that is. Press `R` to remove it. So is the ring. Got nothing identified, have to go on. 

> There's many advanced tricks to identify things, like observing the price shopkeepers offer at shops. 

>In NetHack, every ring, amulet and scroll has different appearance. In every game, the appearance ~ function relationship remains the same. But between different games, this correspondence is randomized. This is not trivial. Because, for example, the material of rings can be different(metal, stone, etc), and this decides in each game, what kind of permanent attribute you can gain from polymorphing and eating rings. Yeah, it's as complex as this. 

Same adventure continues. Saw **shop**, but we don't have money and have to leave. 

> `"Hello, petergu!  Welcome to Akhalataki's used armor dealership!"`

> Attacking the shopkeeper is a very unwise move. But, if there's a Wand of death in the shop, you can use it to kill the keeper and acquire all the things. If there's Wand of wishing, you can wish for a Wand of death. Both of the wands are easily recognizable for their base price of 1200. 

> If you are a Tourist wearing a Hawaiian shirt, the shopkeeper will raise the price. 

>Your pet can pick up things inside the shop and drop them outside -- shoplifting. 
>
>`You hear someone cursing shoplifters.`

```
         #  
        ##  
        #   
        #   
--------|-- 
|.......@.| 
|[[[[[[f@[| 
|[[[[[)[[)| 
|))[[.[[[[|
-----------
```

After picking up and putting on a ruby ring, we found it **cursed** -- cannot take off! Don't know what that is yet, but probably nothing good. 

> `E - a cursed ruby ring (on right hand)`

```
You see here a blindfold.  You faint from lack of food.--More--

                                       ------
                                       |....|
                                      #......###
                           ------######|....|  ##     ------
      ----------------     |....-######|....|   ###   |....|
      |..............-     |....|#     |....|     ####....F|
      .......@..F....|     |....|      |....|         |....-##
      |......f.......|     |..^..      --)---         ------ ###  ------------
      ---.------------     -|-.--        #                     ###..{........|
         ###                #            #####                    |..........|
           ####             #                ##                   |........>.|
             -.----      ---.-      ----------.--                 ------------
             |....|      |...|      ............|
             |....|      |...|      |...........|
             |....|      |...|      |.<.........+
             |.....######-...|      |...........|
             ------      -----      -------------



Petergu the Evoker             St:8 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:3 $:28 HP:15(15) Pw:23(23) AC:1 Xp:2 Fainted Burdened Deaf
```

Soon later, we fainted in hunger. We probably are wearing a Ring of hunger. Press `e` to eat an egg, and gain back conscience. Luckily there's curse-removing scroll in bag, read it: press `r` - `?` - `k`. 

> `As you read the scroll, it disappears.--More--`
>
> `You feel like someone is helping you.  Rex misses the lichen.--More--`

Then, the ring can be taken off. 

Hungry again, ate an egg but it has rotted. 

> `Blecch!  Rotten food!  The world spins and goes dark.`

After another walk, we have nothing to eat(all corpses ate by the cat). We have fainted, what to do now? Only god can save us now. 

>  `Wizard Needs Food, Badly!`

`#pray`

> `You begin praying to Thoth.  You are surrounded by a shimmering light.--More--`
>
> `You finish your prayer.  You feel that Thoth is well-pleased.--More--`
>
> `Your stomach feels content.`

**Pray** is really strong -- look, we just turned full! But our god will be angry at us if we pray too often. 

> Pray thrice in a row, and you'll probably be struck by thunder. 

```


                                                 --------
                            ---------------######........#
           --------         |..@>.........|#     |......|#     -----
           |......|         |.............|#    #-..B...|#     |...|
           |...:..|         ---|-----------#    #|......|#     |...|
           |......|            #   ##   ####    #|.......##    |...|
           |......|         ##*##########       #.......| #####....|
           |......|        ##  #    #####     ###-------- #    |...|
           -----|--       ##   #        #     # #         #    -|---
                #         #  ###        ##    ##          #     #
                #        ##  #           #    #           #     #
                #        #---.------     #    #           #     #
                #        #|........|    -|----#           ##    ##
                #       ##|........|    |..|>|#            ####--|---------
               ##     ### |........|    |....|#               #...........|
           ----.-     #   |<.......|    |....-#                |..........|
           |.....######   |........|    ------                 |..........|
           |...{| ######  .........|                           ------------
           ------         ----------

Petergu the Evoker             St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:4 $:62 HP:15(15) Pw:23(23) AC:1 Xp:2 Burdened
```

Found that there's two downstairs. This is because one of them is into a branch line called the Gnomish Mines. It's difficult, and usually we shouldn't trespass except Valkyrie-like strong characters. The style is like below, full of gnomes `G`. Be surrounded after getting in. Luckily our strong cat cleared some enemies. We escaped after having 7 HP left. Of course, there's also many equipments in the Mines -- we changed shoes. Now our `AC` is 0, while the price is heavy armor and cumbersome movement. 

>  `b - an uncursed +0 cloak of magic resistance (being worn)`
>  `p - a +0 splint mail (being worn)`
>  `B - a +0 iron skull cap (being worn)`
>  `I - a +0 pair of hard shoes (being worn)`

```
   ---
   |..--
 ---....- -
 |.......|.
  .........-
   -.......|
    -......|
     -..f..--
     --.....|
      -..G..|
   |....|...| ---
   | ----.......|
       -.@......|
      |.........|
      |.......---
      ..*.G.....
     ------------
```

Next layer there's a box. We can **loot** it using `#loot`. It's locked, and we have to kick it open. 

> The downside of kicking is potions and fragile wands inside will be broken. 
>
> `THUD!  You break open the lock!`

```
You see here a large box.


                                                           -----
                  -----------     #########################....|
                  |.........|    ###########        #      |...|
                  |.........|    #------      ------       .....
                  |.........|    #.....| #####...<.|       |...|
                #`|..f..>...|    #|.....##    |...)|       --.--
                 #-...@.....+     |....|      |...)|         #
                 #-----------    #-.----      |....|         #
                 #####        ##  ##          |....|###      #
                   # ################        #---.--##############
                              #              `############ --.-+-|----
                   .                               ###     ..........|
                  |.                                       |.........|
                  |.                                       |...... ..|
                  |.                                       |.........|
                  ---                                      |.........|
                                                           -----------


Petergu the Conjurer           St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:5 $:75 HP:25(25) Pw:31(31) AC:0 Xp:3 Burdened
```

But it's empty. 

> You can also put something inside. 

Next layer there's some plaza-like things. The oracle is at middle, we can spend 50 to have a inquery(`#chat`). 

```
You see here a statue of a plains centaur.


                                                   -------
             ------------           ###############-....*|
             |..........|        ####              |.....|
             ...........| ############        #####-.^....
             |.....<....|##     #-----|-------#    -------
             -|----------#       |C.........C.#
              ############       |.....C.....|#
                                 |...-----...|
                                 |...|.{.|...|
                                 |..@|{.{|C..|
                                 |.....{.|...|
                                 |..f-----...|
                                 |.....C.....|
                                 |C.........C|
                                 -------------




Petergu the Conjurer           St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:6 $:75 HP:25(25) Pw:31(31) AC:0 Xp:3 Burdened
```

On the next layer, we met a Giant bat. We fled after being seriously bite. But it chases on, and encountered us in thin corridor. 

```
The giant bat bites!--More--

                                          -----                       -----
                                          |...|                #######-...|
                                         #|....#################      |...|
                                         #-...|#                      |...|
                                         #|...|                       -...|
                                         #|...|                       |.>.|
                                         #-----                       ---.-
                                         ##                              #
                                         #                               *
                                        ###                              #
                                         # #                             #
                                         ###                             #
                                           #                          -|-|-
                                           #                          |...|
                                           #                          |...|
                                          +######B@######`            ...<|
                                         .|#                          -----
                                         .-#
                                         --#

Petergu the Conjurer           St:9 Dx:8 Co:16 In:17 Wi:14 Ch:11 Neutral
Dlvl:7 $:88 HP:5(29) Pw:43(43) AC:0 Xp:4 Burdened
```

> `You die...--More--`
>
> `Do you want your possessions identified? [ynq] (n) `

And here, we **die**. 

The game will ask if you'd like to have a look what you really have. Doesn't hurt to have a glimpse. 

```
 L - 3 uncursed scrolls of blank paper
 R - an uncursed scroll of fire
 Spellbooks
 l - a blessed spellbook of force bolt
 m - an uncursed spellbook of create monster
 x - an uncursed spellbook of slow monster
 Potions
 f - an uncursed potion of gain ability
 g - an uncursed potion of monster detection
 h - an uncursed potion of healing
 G - an uncursed potion of sickness
 O - an uncursed potion of water
 S - a blessed potion of sleeping
 Rings
 d - an uncursed ring of fire resistance
 e - an uncursed ring of invisibility
 r - an uncursed ring of cold resistance
 E - an uncursed ring of aggravate monster
 H - an uncursed ring of sustain ability
 Wands
 c - a wand of magic missile (0:4)
 Tools
 n - a magic marker (0:55)
 (2 of 3)
```

Usually, if you have a look, you can find there's at least one object that can prevent the death(like the potion of healing can probably save us, or the magic missle looks promising). But YOU ONLY LIVE ONCE, and this is Roguelike. 

```

                       ----------
                      /          \
                     /    REST    \
                    /      IN      \
                   /     PEACE      \
                  /                  \
                  |     petergu      |
                  |      88 Au       |
                  |   killed by a    |
                  |    giant bat     |
                  |                  |
                  |                  |
                  |       2021       |
                 *|     *  *  *      | *
        _________)/\\_//(\/(/\)/\//\/|_)_______


Goodbye petergu the Wizard...

You died in The Dungeons of Doom on dungeon level 7 with 704 points,
and 88 pieces of gold, after 3310 moves.
You were level 4 with a maximum of 29 hit points when you died.
```

#### Epilogue

I hope you have understood the basics of NetHack after my typical game walkthrough as a noob, and are able to survive inside the dungeon for a finite amount of time. There are still a ton of mechanisms and skills unmentioned(not even BUC status), and they're up to you to find. For ordinary players, to beat a game like this, cheating, spoiling, or even reading the source code may all be necessary, and they are certainly not shameful things -- knowing this is enough. 

#### Reference & further reading

https://nethackwiki.com

https://www.zhihu.com/question/40177337 (Chinese)

https://www.melankolia.net/nethack/nethack.guide.html

https://nethack.org/

https://github.com/regymm/nethackassistant

https://alt.org/nethack/



Original by regymm 2021.9