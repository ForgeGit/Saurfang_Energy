# Deathbringer Saurfang (World of Warcraft: Classic WoTLK) energy generation mechanics

We lack a precise, catch-all method to explain and measure energy generation on Saurfang (final raid boss of the Lower Spire at Icecrown Citadel). 
We have a general idea of how it works, but we cannot explain it or measure in detail.

For all intend and purposes of this document, I will refer to Energy on Saurfang as Blood Power or BP (A.K.A Runic Power, Blood Energy, Blood Points). 


## Introduction

Common thirdhand sources of information such as [WoWhead Comments](https://www.wowhead.com/wotlk/npc=37813/deathbringer-saurfang#comments), [WowPedia](https://wowpedia.fandom.com/wiki/Deathbringer_Saurfang), and [WoW-Wiki](https://wowwiki-archive.fandom.com/wiki/Deathbringer_Saurfang), plus "common knowledge" from Class Discords, [Blizzard Forums from 2010](https://web.archive.org/web/20100213045756/http://forums.worldofwarcraft.com/thread.html?topicId=22749002374&sid=1&pageNo=1) and PTR experience, all coincide more or less on the following:

- Only spell-related damage should count towards BP generation
    - Boiling Blood, Rune of Blood, Blood Nova, and Beast melee damage all count towards BP generation.
    - Melee damage from the boss on the tanks should not contribute towards BP generation (unless it is a Rune of Blood hit).
    
- In Heroic mode BP generation seems much faster compared to normal mode.

There are, however, conflicting or vague attempts at explaining the underlying mechanics of BP generation. Specifically: 

- Does "Absorbed" damage count towards BP generation?
    - An old comment suggests Absorbed damage would not count towards BP generation
    
          - A wowhead comment from the 18th December 2009 by 7103 on 2009/12/19 (Patch 3.3.0) says "he doesn't gain BP unless he actually deals damage, meaning a disc priest perma-bubbling our tank and our add kiters doing a stellar job resulted in him only having one mark up when he died."
          -  Patch 3.3.2 (2010-02-01): Mitigation abilities such as Spell holy powerwordshield [Power Word: Shield] will no longer prevent blood power generation.
    
- Does "Mark" damage count towards BP generation?
    - It is suggested mark damage (or the more "Mark" targets are out? subtle but important difference on tick damage vs pressence of the tick or debuff) increases BP generation over time.
    
        - Is this because damage "ramps up"? 
        - Does it add 1 extra BP or is it 1 extra BP per tick? 
    - Old patch notes suggest this should not be the case
        - Patch 3.3.2 (2010-02-01): Deathbringer Saurfang will no longer gain blood power from Mark of the Fallen Champion.
         

- Exactly how much Blood Energy does a Boiling Blood tick contribute? 
    - It is said boiling blood generates somewhere around 2 and 4 BP. 
    - [Old websites](https://typehforheals.com/raid-strategies/wrath-of-the-lich-king/icecrown-citadel/deathbringer-saurfang/#:~:text=Besides%20Blood%20Nova%2C%20Saurfang%20will%20be%20casting%20Boiling,or%20Divine%20Shield%20this%20should%20be%20done%20immediately), [wordpress pages](https://dontstandinthefire.wordpress.com/tactics/icecrown-citadel/deathbringer-saurfang-10-man/) and even WoWHead comments from the past give us some numbers that usually disagree with each other.
    - It is also said it generates more BP based on how much damage each tick hits for, however, we know from looking at videos + log simultaneously [2] that the same boiling blood ticking for the same amount of damage went from giving 2 BP, to giving 3 BP. 
          - (E.g. Two consecutive ticks of 6,500 damage each giving 2 and 3 BP respectively)
      
<img src="_img/Forum_comment_2010.png" />
Figure 1: On the following document, this guy argument from 2010 will be utterly destroyed. 

## Current Understanding

It is usually agreed that out of all the 4 damaging abilities in the encounter that generate BP, Beast melees are usually the biggest source of it, in an order that goes:

  - Blood Beast melee > Blood Nova >= Blood Boiling tick >= Rune of Blood 

Attempts at accurately measuring BP generation usually rely on visualizing the boss energy bar in-game, which updates roughly every 3s, and serves the purpose of ground truth or real value of reference.

<img src="_img/Saurfang_energy_bar_ingame.jpg" />
Figure 2: In yellow, in-game energy bar of Saurfang on different UIs. The image on the right is Fojji's Weakaura displaying the Energy value separately from the Boss Frame.

