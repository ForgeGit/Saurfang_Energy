# Deathbringer Saurfang energy generation mechanics - World of Warcraft: Classic WoTLK

**Tl;dr:** 
`2,500 damage taken by the raid = ~1 Blood Power`, which can be decimal (e.g. 2.495 energy), and accumulates over time.

In this document I show my approach works better than other alternatives proposed to measure energy generation on Saurfang, and suggest energy generation is entirely damage based<sup>1</sup>, rather than "tick" or "cast" based<sup>2</sup>.

[1] Damage Taken by raiders, including pets and summons such as Army of the Death or Warlock pets. This includes Absorbs and Overkill values.

[2] Valid sources of damage are Boiling Blood, Rune of Blood, Blood Nova, and Beast melee damage.

## Preamble

We lack a precise, catch-all method to explain and measure energy generation on Saurfang (final raid boss of the Lower Spire at Icecrown Citadel). 

We have a general idea of how the mechanic works, but we cannot explain it or measure it in detail.

Here I describe how using the formula `2,500 damage taken = ~1 Blood Power` explains very well the energy generation mechanics of Deathbringer Saurfang.

This approach can be used in logs uploaded to Warcraft Logs Classic to measure which abilities contributed the most towards BP generation in a single pull.

Note: From now on, for all intents and purposes, I will refer to Energy on Saurfang as Blood Power or BP.
(A.K.A Runic Power, Blood Energy, Blood Points). 

## Introduction

Common third-hand sources of information such as [WoWhead Comments](https://www.wowhead.com/wotlk/npc=37813/deathbringer-saurfang#comments), [WowPedia](https://wowpedia.fandom.com/wiki/Deathbringer_Saurfang), and [WoW-Wiki](https://wowwiki-archive.fandom.com/wiki/Deathbringer_Saurfang), plus "common knowledge" from Class Discords, [Blizzard Forums from 2010](https://web.archive.org/web/20100213045756/http://forums.worldofwarcraft.com/thread.html?topicId=22749002374&sid=1&pageNo=1) and PTR experience, all coincide more or less on the following:

- Only spell-related damage should contribute towards BP generation
    - Boiling Blood, Rune of Blood, Blood Nova, and Blood Beast melee damage all count towards BP generation.
        - They are all "spells casted by" Saurfang that deal physical type damage.
    - Melee damage from the boss on the tanks should not contribute towards BP generation. 
        - Unless it is a Rune of Blood hit, which is different than the standard melee hit.
        <img src="_img/BloodRune_damage.png" />
        
        *Figure 1: Rune of Blood and a melee hit.*


- In Heroic mode BP generation seems much faster compared to normal mode.

There are, however, conflicting or vague attempts at explaining the underlying mechanics of BP generation. For instance: 

- Does "Absorbed" damage count towards BP generation?
    - An old comment suggests Absorbed damage would not count towards BP generation
    
        - A wowhead comment from the 18th December 2009 by 7103 on 2009/12/19 (Patch 3.3.0) says "he doesn't gain BP unless he actually deals damage, meaning a disc priest perma-bubbling our tank and our add kiters doing a stellar job resulted in him only having one mark up when he died."
        - Patch 3.3.2 (2010-02-01): Mitigation abilities such as Spell holy powerwordshield [Power Word: Shield] will no longer prevent blood power generation.
    
- Does "Mark" damage count towards BP generation?
    - It is suggested mark damage increases BP generation over time.
        - Mark should not generate BP. Or does it? (Spoiler: It should not)
        - Would it add 1 extra BP once or is it 1 extra BP per tick? 
        - Is it because "more marks" are out? or because the longer the fight the more BP is passively generated?
             - (Is there even such a thing as "passive BP" generation?)
    - Old patch notes suggest this should not be the case
        - Patch 3.3.2 (2010-02-01): Deathbringer Saurfang will no longer gain blood power from Mark of the Fallen Champion.
         

- Exactly how much Blood Energy does a Boiling Blood tick contribute? 
    - It is said boiling blood generates somewhere around 2 and 4 BP. 
        - [Old websites](https://typehforheals.com/raid-strategies/wrath-of-the-lich-king/icecrown-citadel/deathbringer-saurfang/#:~:text=Besides%20Blood%20Nova%2C%20Saurfang%20will%20be%20casting%20Boiling,or%20Divine%20Shield%20this%20should%20be%20done%20immediately), [wordpress pages](https://dontstandinthefire.wordpress.com/tactics/icecrown-citadel/deathbringer-saurfang-10-man/) and even WoWHead comments from the past give us some numbers that usually disagree with each other.
    - It is also said it generates more BP based on how much damage each tick hits for, however, we know from looking at videos + log simultaneously [2] that the same boiling blood ticking for the same amount of damage went from giving 2 BP, to giving 3 BP. 
          - (E.g. Two consecutive ticks of 6,500 damage each giving 2 and 3 BP respectively)
      
<img src="_img/Forum_comment_2010.png" />

*Figure 2: On the following document, this guy argument from 2010 will be utterly destroyed.*

## Current Understanding

It is usually agreed upon that out of all the 4 damaging abilities in the encounter that generate BP, Blood Beast melees are usually the biggest source of BP.

In order of "most to least BP generation", the list would go like this:

  - Blood Beast melee > Blood Nova >= Blood Boiling tick >= Rune of Blood 

A common and simple method used to quickly give each ability a value or "weighting" is to add up the times each ability hit an unit and assume each of those hits gave a "fixed" amount of BP.

<img src="_img/Saurfang_Energy_1.jpg" />

*Figure 3: Illustrated example of how many times each ability hit a player over a period of time. From here you would usually assume they all add up to at least 100 BP*

This method would normally value Boiling Blood at around 2-3 BP, Blood Nova at 2-3 BP, and Rune of Blood at around 1-3 BP. Blood Beast melee BP generation being an outlier at 2-18 BP, with varying explanation and causes for it. 

This method however tends to not flawlessly work with all logs, and relies in adjusting your values to make them add up as close as possible to 100, with no clear rule or explanation that can be extrapolated to other logs.

This very basic method tends to either fall short of estimating 100 BP, or going well past 100 BP, by the time of mark first being casted by Saurfang.

A more accurate analysis of energy generation relies on visualizing the boss energy bar in-game, which updates roughly every 3s, and can be used as our real value of reference.

<img src="_img/Saurfang_energy_bar_ingame.jpg" />

*Figure 3: In yellow, the energy bar (BP) of Saurfang seen in-game from different UIs. The image on the right is Fojji's Weakaura displaying the Energy value separately from the Boss Frame.*

Using this method, two Saurfang Heroic (25 man and 10 man) logs were analyzed next to their recordings by `overrated_` and `oozeness`.
    - Joardee <Fusion> Saurfang Heroic 25 man:
        - Log 1: https://classic.warcraftlogs.com/reports/4aGYdP3kyBNchRQf#fight=10&type=damage-taken&options=0&by=ability
        - Vod 1: https://youtu.be/eVM0n_3IUAQ?t=894
        
    - Oozeness Saurfang Heroic 10 man: 
        - Log 2: https://classic.warcraftlogs.com/reports/ZWbAJC2nLcHRkhdG#translate=true
        - Vod 2: https://www.youtube.com/watch?v=BYIly4KoEqY&ab_channel=Oozeness 

<img src="_img/Fightclub_cooking.jpg" />

However, attempts at assigning a BP value to each damage taken from Saurfang have always resulted in inconsistent, non-preproduceable weigthings, that rely in a lot of caveats and conditions for one spell to fit the energy gains we see in-game. 



