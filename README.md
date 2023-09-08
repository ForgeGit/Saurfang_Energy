# 🩸Deathbringer Saurfang🩸<br/>Blood Power generation mechanics<br/>

_Vivax (Pagle-US) -_ `Discfordge` _(Discord)_

The following document goes in-depth describing how Blood Power generation works during the encounter with "Deathbringer Saurfang", final raid boss of the Lower Spire at Icecrown Citadel, in the Classic version of World of Warcraft: Wrath of the Lich King (2023).

## **SUMMARY**
### **Tl;dr**
`2,500 damage taken = ~1 Blood Power` in Heroic Mode<br />
Blood Power can be decimal (e.g 7.495 BP), and it accumulates over time. This formula explains Saurfang energy generation mechanics.

In this document I explain why this approach works better than other alternatives proposed to measure energy generation on Saurfang, and suggest energy generation is entirely damage based<sup>1</sup>, rather than "tick" or "cast" based<sup>2</sup>.

[1] *Damage Taken by friendly units.* <br />
        - *Friendly units include "pets" such as Army of the Death or Warlock summons.*<br />
        - *Damage taken includes Absorbs and Overkill values.* <br />
[2] *Valid sources of damage are Boiling Blood, Rune of Blood, Blood Nova, and Beast melee damage.*

# Table of Contents 📜

1. [Preamble](#preamble)<br>
2. [Introduction](#munching-the-dps-missing-from-your-ignite)<br>
3. [Previous Understanding](#previous-understanding)<br>
    3.1. [Basic Log Measurement](#a-basic-log-measurement)<br>
    3.2. [In-game Energy Bar Measurement](#b-in-game-energy-bar-measurement)<br>
4. [The 2,500 damage methodology](#the-2500-damage-methodology)<br>
    4.1. [How can I check this by myself?](#how-can-i-check-this-by-myself)<br>
5. [Results (WIP)](#results)<br>
6. [FAQ](#frequently-asked-questions)<br>
7. [Fact Sheet](#fact-sheet)<br>

## 🌤Preamble

We lack a precise, catch-all method to explain and measure Blood Power (energy) generation on Saurfang. It is not recorded in logs and previous attempts at measuring it have failed.

We have a general idea of how the mechanic works based on its description and feelscraft, but we could not precisely explain it or measure it in detail until now.

<img src="_img/Ability_Descr.jpg" /> <br />

Here I describe how using the formula `2,500 damage taken = ~1 Blood Power` we can explain very well the Blood Power generation mechanics of Deathbringer Saurfang in Heroic Mode. For Normal Mode, one can use 5,000 damage taken.

This approach can be used in logs uploaded to Warcraft Logs Classic to analyze which abilities contributed the most towards BP generation.

Note: From now on, for all intents and purposes, I will refer to Energy on Saurfang as Blood Power or BP.
(A.K.A Runic Power, Blood Energy, Blood Points). 

## 🐗Introduction

### **Introduction Tl,dr:** <br />
> Four abilities generate BP. Heroic mode Saurfang generates more BP (or faster BP) than normal mode Saurfang.<br />
> The specifics are beyond us.

Commonly what we know about WoW game mechanics (in this case, Saurfang BP generation) comes from sites such as [WoWhead Comments](https://www.wowhead.com/wotlk/npc=37813/deathbringer-saurfang#comments), [WowPedia](https://wowpedia.fandom.com/wiki/Deathbringer_Saurfang), and [WoW-Wiki](https://wowwiki-archive.fandom.com/wiki/Deathbringer_Saurfang), with some of this information coming from "OG" [Blizzard Forum posts from 2010](https://web.archive.org/web/20100213045756/http://forums.worldofwarcraft.com/thread.html?topicId=22749002374&sid=1&pageNo=1), Elitist Jerks archives, or "common knowledge" shared in WoW communities such as Class Discords. 

More recently, with the first PTR test of ICC, we can now use detailed logs and video recordings to look into the specifics of BP generation.

All of the sources previously mentioned coincide more or less on the following:

  1. Only spell-related damage should contribute towards BP generation
      - These four (4) sources are all "spells casted by" Saurfang that deal physical type damage.
          - Boiling Blood (ID: 72385)
          - Rune of Blood (ID: 72409)
          - Blood Nova (ID: 72380)
          - Blood Beast melee hits (ID: 1)
      - Melee damage from the boss on the tanks should not contribute towards BP generation. 
          - Unless it is a Rune of Blood hit, which is different than the standard melee hit.
        <img src="_img/BloodRune_damage.png" /> <br />
          *Figure 1: Rune of Blood and a melee hit - they are different*

  2. In Heroic mode BP generation seems much faster compared to normal mode.
  
  3. At 100 BP generated, Saurfang casts "Mark of the Fallen Champion" (ID: 72255), which you want to delay as much as possible by getting hit less by the four (4) abilities previously mention. 

There are, however, conflicting or vague attempts at explaining the underlying mechanics of BP generation. For instance: 

- Does "Absorbed" damage count towards BP generation?
    - An old comment suggests Absorbed damage would not count towards BP generation
    
        - A wowhead comment from the 18th December 2009 by 7103 on 2009/12/19 (Patch 3.3.0) says "he doesn't gain BP unless he actually deals damage, meaning a disc priest perma-bubbling our tank and our add kiters doing a stellar job resulted in him only having one mark up when he died."
        - However, in a later patch, it reads: Patch 3.3.2 (2010-02-01): Mitigation abilities such as Spell holy powerwordshield [Power Word: Shield] will no longer prevent blood power generation.
    
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
    - It is also said it generates more BP based on how much damage each tick hit for. 
        - However, we know from looking at videos + log simultaneously [1] that the same boiling blood ticking for the same amount of damage can give both 2 BP and 3 BP. 
        - i.e. Two consecutive ticks of 6,500 damage each giving 2 and 3 BP respectively.
      
<img src="_img/Forum_comment_2010.png" /> <br />
*Figure 2: On the following document, this guy argument from 2010 stating BP gains are "consistent" will be utterly destroyed.*

With the upcoming ICC re-release, having a better understanding of how BP generation works could help optimize better the encounter, which is specially relevant during the first few weeks with [potentially tight DPS and healing checks for some raid teams.](https://github.com/ForgeGit/ICC_PTR/blob/main/_img/final_image_25_10_wing_title.jpg)

## 📅Previous Understanding

### **Previous Understanding Tl,dr:** <br />
> BP generation goes like: <br />
> Blood Beast melee > Blood Nova >= Blood Boiling tick >= Rune of Blood <br /> 
> (All previously reported BP values in the past for each ability are a lie.)

It is usually agreed upon that from the four (4) abilities in the encounter that generate BP, Blood Beast melee hits are usually the biggest source of BP.

In order of "most to least BP generation", the list would go like this:

  - Blood Beast melee > Blood Nova >= Blood Boiling tick >= Rune of Blood 

We can sort of confirm this by doing one of the following measurements.

### A. Basic log measurement 

A common and simple method used to quickly give each ability a value or "weighting" is to add up the times each ability dealt damage and assume each of those hits gave a "fixed" amount of BP.

<img src="_img/Saurfang_Energy_1.jpg" /> <br />
*Figure 3: Illustrated example of how many times each ability hit a player over a period of time. From here you would usually assume they all add up to at least 100 BP. <br />https://classic.warcraftlogs.com/reports/a:4VrdMJn8ypFjC2zv#fight=3&type=damage-taken&start=784192&end=889217&options=0&by=ability*

This method would normally place the value of abilities as:

- Boiling Blood 2-3 BP
- Blood Nova at 2-3 BP
- Rune of Blood at 1-3 BP
- Blood Beast melee at 2-18 BP (the wide range has varying explanations and causes for it, apparently explained by how "big" or "hard" the hit was).

<img src="_img/Saurfang_Energy_2_2.png" /> <br />
*Figure 4: It works! Sort of. Now try it with other logs, lmao.*

The issue with this approach is that it won't work with all logs, and relies in adjusting <sup>(read: optimizing)</sup> your values to make them add up as close as possible to 100, with no clear rule or explanation. <br />

This will usually result in the estimation either falling short of reaching 100 BP, or going well past 100 BP by the time the first mark goes out.

We can use the "weighted values" from the Table above (Figure 4) on the 1,227 PTR logs from the 1st round of PTR testing and "estimate" Saurfang BP at the time of 1st mark going out on Figure 5. 

Figure 5 shows the estimated BP (Y-axis) at the time the first mark was cast for each encounter analyzed [one (1) log can have several encounters]

<img src="_img/plot_saurfang_example1.png" /> <br />
*Figure 5: Estimated BP at the moment of first mark cast for all PTR logs with Saurfang encounters using the weights on the table of Figure 4. <br />"Beast" numbers specify which value was used for Blood Beast melee hits weightings.<br />Shaded red area is the range from 90 to 110 BP estimated at first mark cast.<br />Blue dotted lines indicate the  150 BP and 75 BP marks.* 

### B. In-Game Energy Bar Measurement

A more accurate analysis of BP generation relies on visualizing the boss energy bar in-game, which updates roughly every 3s.<br />
This value directly provided by the game (but not registered in logs) can be used as our real value of reference.

<img src="_img/Saurfang_energy_bar_ingame.jpg" /> <br />
*Figure 6: In yellow, the energy bar (BP) of Saurfang seen in-game from different UIs. The image on the right is a Weakaura/Addon displaying the Energy value separately from the Boss Frame.*

Using this method, two Saurfang Heroic logs (25 man and 10 man) were analyzed along with to their recordings by `overrated_` and `oozeness`.

  - **LOG #1** Joardee <Fusion> Saurfang Heroic 25 man:
      - Log #1: https://classic.warcraftlogs.com/reports/4aGYdP3kyBNchRQf#fight=10&type=damage-taken&options=0&by=ability
      - Vod #1: https://youtu.be/eVM0n_3IUAQ?t=894
        
  - **LOG #2** Oozeness Saurfang Heroic 10 man: 
      - Log #2: https://classic.warcraftlogs.com/reports/ZWbAJC2nLcHRkhdG#translate=true
      - Vod #2: https://www.youtube.com/watch?v=BYIly4KoEqY&ab_channel=Oozeness 

<img src="_img/Fightclub_cooking.jpg" /> <br />
*Figure 7: Two Fight Club discord members cooking.<br /> On the left, Naz with LOG #1. On the right, Oozeness with LOG #2.*

However, attempts at assigning a BP value to each ability results in inconsistent, non-preproduceable weightings, that rely in several of caveats and conditions that do not always apply to all cases.

For instance, using log #2 Wipe #3 (Oozeness 10 man) the weightings [1] would look like this:

  - Boiling Blood tick: 2-3-4 BP
  - Blood Rune application: 1 BP (inconsistent)
  - Blood Rune damage: 2 BP
  - Blood Nova: 3 BP (possibly misses/immune hits counting)
  - Blood Beast melee: 1-3-4-8-9 (damage dealt + Scent of Blood buff presence seem to play a factor) 

The weightings proposed from using video recordings (which include Blood Nova misses as sometimes generating BP) would "overestimate" our previous log estimation of 104 BP on Figure 4. <br />
In other words, going off the values recorded in-game, Saurfang should have casted Mark several seconds earlier than what he actually did.

<img src="_img/Saurfang_Energy_3_2.png" /><br />
*Figure 8: The estimation that works on one log, no longer works on the previous log. Sadge*

## 🔨The 2,500 damage methodology

### **The 2,500 damage methodology Tl,dr:** <br />
> 2,500 dmg = 1 BP, a number sort of pulled out of my ass, works better than numbers pulled out of other assess.

After sharing notes and ideas with `oozeness`, we came up with the following more generalized approach at estimating BP generation:


Lets assume that BP is accumulated over time and it is not a rounded number as shown by the energy bar.<br />
(e.g. 0.53 energy, 3.10 energy, 7.9048 energy)

Using logs and the in-game energy bar from Log #1 and Log #2, we know how much energy was generated every 3s, and we can check how much damage was taken by friendly units during that period of time.

<img src="_img/Saurfang_Energy_4.jpg" /> <br />
*Figure 9: Illustrated example of how damage taken was estimated every time the energy bar updated (3s intervals).*

If we continue to do this for several points using Log #1 (Joardee) and Log #2 (Oozeness) as reference, we get a number that approximates ~2,500 damage per BP.

If we assume every `2,500 damage` equals `1 BP`, we can accurately match the values on both logs, and in every other Heroic log almost perfectly. 

<img src="_img/plot_saurfang_example_2.png" /> <br />
*Figure 10: Estimated BP at the moment of first mark cast on all PTR logs comparing different approaches at estimating BP. <br /> "Beast" numbers specify which value was used for Blood Beast melee hits weightings.<br />Shaded red area is the range from 90 to 110 BP estimated at first mark cast.<br />Blue dotted lines indicate the  150 BP and 75 BP marks.* 

### How can I check this by myself?

You should be able to check for your own logs with the following options. (Highlighted in blue) <br />
The damage taken before the 1st mark goes out, and between marks, should add up to at least 249k-ish.

<img src="_img/energy_yourself_2.png" /> <br />
*Figure 11: Example of how to filter and see the values that generate BP in logs* 

## 🔮Results 

[WORK IN PROGRESS - The result is summarized in Figure 10. However, I want to better describe the results here, eventually. Maybe?]

The data used to generate the analysis and results can be found [in this repository](https://github.com/ForgeGit/Saurfang_Energy/blob/main/Saurfang_BP_Data.csv).

In figure 10, we estimated the energy at the time of the 1st mark cast by Saurfang (Y-axis) for 3 scenarios:

- 2,500 damage (damage after mitigation/block + absorbed damage + overkill) = 1 energy
- Static weights being Nova=3, Boiling Blood=3, Rune Blood hit = 1, Melee Beast = 3
- Static weights being Nova=3, Boiling Blood=3, Rune Blood hit = 1, Melee Beast = 5

The 2,500 damage model is able to explain extreme log examples with mark going out extremely early (20s into the fight; one of the fastest mark recorded in PTR): <br/>
https://classic.warcraftlogs.com/reports/7Bv1rbpYxzCmPDKV#fight=27&type=damage-done&hostility=1&source=101&start=6260520&end=6278616&options=256&ability=72380

It also explains pets such as Army of the Death getting hit by Blood Beasts, and other logs within a margin of +/- 1 BP. 

More importantly, this method can be easily extrapolated to almost all logs to precisely understand what contributed to Blood Energy generation between marks.

<img src="_img/Blood_Power_example.png" /> <br />
*Figure 12: In this log, mark went out at 1:36. <br /> 7.44 BP from Rune of Blood and 32.28 BP from Blood Beasts should be preventable. <br /> Potentially more from Boiling Blood if defensives and cooldowns such as BoPs are optimized.* 

For normal mode, using `5,000 damage` works also fine.

<img src="_img/plot_saurfang_example_5.png" /> <br />
*Figure 13: Normal mode estimation of BP at moment of 1st mark cast for Normal Mode.*

## ❓Frequently Asked Questions

- According to this, what generates Blood Power/Energy?

  - Any `damage taken` by friendly units (character, pet, summon) + `Absorbed damage` + `Overkill` from one (1) of the following four (4) spells:
  
      - Boiling Blood (ID: 72385)
      - Rune of Blood (ID: 72409)
      - Blood Nova (ID: 72380)
      - Blood Beast melee hits (ID: 1)
  - For Heroic Mode, `2,500 damage taken` is equal to more or less `1 Blood Power/Energy`
  - For Normal Mode, `5,000 damage taken` is equal to more or less `1 Blood Power/Energy`
  

<img src="_img/Blood_Power_example_2.png" /> <br />
*Figure 14: Example of damage that counts toward BP generation.* 


## 🩸Fact Sheet<br />
*By "Naz"* `overrated_` *(Discord) - Fight Club*

🩸 Saurfang gains 1 Blood for every 2500 damage dealt by Saurfang's spells, or Blood Beast melee hits. 1 Blood for every 5000 damage in Normal modes.<br />
🩸 Damage dealt by Mark of the Fallen Champion does not generate Blood.<br />
🩸 Damage dealt to pets counts - Army can be used >5s before the pull, so it disappears at 40s into the fight, when first beasts are summoned. Otherwise avoid using Army.<br />
🩸 2500 damage is calculated after mitigation (e.g. Block a beast melee, BoP a Boiling Blood debuff, DSac the DoT/Nova, PS, etc).<br />
🩸 2500 damage includes damage done to shields - PW:S does not reduce Blood generation (this is intended, noted in 2010 patch notes).<br />
🩸 2500 damage includes Overkill - clothies get doinked by beasts -> 20+ Blood generated on boss.<br />
🩸 Saurfang's melee hits do not increase his Blood, but the Rune of Blood ability used when hitting tank-debuffed targets does generate Blood. <br />
🩸 His damage increases with his current Blood level (+0% -> +100%), which increases his own Blood generation in turn. <br />

Slowing the pace of blood generation with mitigation, means slowing the rate at which Marks come out.<br />
Mark of the Fallen Champion doesn't generate Blood, but Mark's damage does increase with current Blood level. Important to note this when balancing CDs between preventing Blood generation, and handling the increased Mark damage at high Blood levels.
