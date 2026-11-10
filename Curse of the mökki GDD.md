# Curse of the mökki GDD

**Curse of The Mökki/Mountain/Cave** — Game Design Document, 2026

> Imported from the [Google Doc](https://docs.google.com/document/d/1CPt07hfyMdiQCxjAZefjJKTiYywah9qw3gqGsOCxWa8/edit) on 2026-11-10. Images (level design sketches, theme colors, reference pictures) were not imported — see the original doc for those.

## Overview

### High Concept

3D Horror survival peli. Menet kaverisi kanssa to your cottage ja kaverisi katoaa keskellä yötä, but everything is not as it seems. You find yourself dreaming and wandering. Löydät kaverisi kuolleena, something in the Luola is altering your perception of reality.

### Features

- 3D First person
- Stealth
- Madness
- Lantern (for forest part)
- Cutscenes
- Dialogue
- Player movement
- Enemy movement
    - Enemies kill you, if they get you at full madness, otherwise they cause madness
    - Enemies disappear after hitting you if they don't kill you
    - Enemies have sanity losing aura
    - Obvious sound effects on monster movement
- Scavenging from corpses
- Puzzles (locked doors)
- Stone throwing distraction
- Seamless transition from forest to cave (from one scene to other)
- Hiding places
    - Lose sanity if you stay in hiding place, maybe slight recovery after leaving
- Wolf chase
- Slow passive sanity recovery

### Player Motivation

### Design Goals

- **Exploration and puzzles with a forced path.** LINEAR game with clear areas of gameplay and exploration. Puzzles will be simple door key combos and more lurking and sneaking past enemies.
- **Visuals first.** Design strives for better visuals than anything and is meant more for the visual experience, detailed textures and surfaces.
- **Map design.** Claustrophobic rooms full of grime, water, wooden structures and dim
- **Jännitys ja pelko.** Vuorella tapahtuu yliluonnollisia asioita jotka luovat pelkoa äänen ja grafiikan avulla

### Setting & Narrative

1. Start in cottage with friend
2. Friend gets gotten
3. Player follows the tracks of missing friend in forest
4. Ends up in a cave after getting chased by wolves and following tracks
5. Advances through different rooms in the cave made by some cultist
6. Has to avoid some kind of monstrous cultist
7. Finds friend in the end of cave with the "fish"
8. Wakes up and some mystery element

### Genre

first person horror stealth, eldritch

### Target Audience

### Development Software

| Purpose | Tool | Details |
| --- | --- | --- |
| Game Engine | Unity | 6000.3.24f1 |
| Version control | Git | GitHub |
| Graphics | Blender | |
| Textures | Krita | |
| Task Management | HacknPlan | Hack'n'Plan |
| Platform | PC | |

## Gameplay Details

### Primary Mechanics

| Verb | Controls | Description | Muokattavat arvot |
| --- | --- | --- | --- |
| Move | WASD | Walking | |
| Interact | E | interact with world / Start dialogue | |
| Sneaking | Ctrl/C | move almost silently to avoid enemies. Doesn't help if fully insane | |
| Run | Shift | Move faster | |
| Use pills | ?Q | recover 30-50 sanity over 5-10 seconds (overdose?) | |
| Stone throw | Mouse Left | distract enemies by throwing a stone (pick up stone by interacting with one) | |
| Madness | | Health like mechanic, where you die if you get hit while madness is 80% or higher | |
| Passive sanity recovery | | recover sanity passively | |

### Secondary Mechanics

#### Enemies

| Enemy | Description | Muokattavat arvot |
| --- | --- | --- |
| Cultist | Scary enemies that patrol and attack when they see the player. They disappear if hit doesn't kill the player. Can be found in 3rd "Cave" sector of the game | Different behaviour, patrolling |
| Wolf | Chasing enemy will show up at the end of 2nd sector "Forest" and force the player forward. If player moves too slow they get killed | |

#### Save points

Get activated by progressing and respawn on death. Forest only has one before wolf chase. Cave has one at the start of every "room"

### User Interface

#### Camera

Low FOV view

#### HUD

- items
    - lantern
- Madness
    - no "real" ui element, afflicts camera view with disorienting effects

#### Menus

**Main menu:**
- Play
- Settings
- Credits
- Quit game

**Pause menu:**
- Resume
- Settings
- Quit to main menu

**Settings:**
- Musiikki ja SFX säätö
- Musiikki ja SFX pois/päälle

**Death/end screen menu:**
- Continue
- Quit

## Level Design

### Cottage/Mökki

### Forest: Alustava simppeli level design

### Cave

## Graphics

### Overview

### Technical Aspects

HD resoluutio, retro aspect ratio. Universal Render Pipeline (URP).

Textures and sprites in powers of two (128x128, 256x256, 512x512, etc.) and .png format.

Meshes in .fbx format.

Materiaalit URP/Lit → smoothness 0, specular highlight ja environment reflections pois päältä.

#### Exporting from Blender to Unity

**In Blender:**

- Transform values: Location: (0, 0, 0) | Rotation: (0, 0, 0) | Scale: 1, 1, 1 (shortcut Ctrl + A → All transforms)
- Export FBX settings
    - Transform panel:
        - Apply Scalings: FBX All
        - Forward: -Y Forward
        - Up: Z Up
        - Use Space Transform: unchecked
    - Armature panel (if the mesh is rigged):
        - Primary Bone Axis: set to the axis the root bone's local Y axis points to
        - Secondary Bone Axis: set to the axis the root bone's local X axis points to

**In Unity:**

- In the Model Importer Inspector window: Bake Axis Conversion: checked
- Remove unnecessary imports (for example Materials)

More info: https://polynook.com/learn/lesson/how-to-export-models-from-blender-to-unity

#### Naming Conventions

`TypeAbbreviation_BaseName_AdditionalInfo`

An entity should have the same BaseName for all of its assets.

| Asset Type | Abbreviation | Example |
| --- | --- | --- |
| Static Mesh | SM | SM_ObjectName |
| Skeletal Mesh | SKM | SKM_CharacterName |
| Texture | T | T_ObjectName_BC |
| Material | M | M_ObjectName |
| Collision Mesh | CM | CM_ObjectName |
| UI sprite | UI | UI_ObjectName |
| animation | ANIM | ANIM_AnimationName |

| Texture Type | Abbreviation | Example |
| --- | --- | --- |
| Base Color (albedo/diffuse) | BC | T_ObjectName_BC |
| Luminosity | L | T_ObjectName_L |

#### Shaders

Sanity shader?

### Style Guide

#### Modeling

Simple low poly models

#### Texturing

https://fawkek.itch.io/psx-retro-tools

Dithered textures using this free tool

Theme colors:

#### List of VFX

| Name | Description |
| --- | --- |
| VFX_Sanity | Silmät joita näkyy ympäristössä kun sanity laskee/ joku muu sekava efekti |
| VFX_Snowstorm | Lumimyrsky efekti |
| VFX_Lantern | Lyhdyssä oleva efekti |
| VFX_Candlelight | Kynttilöissä oleva tuliefekti |
| VFX_Smoke | Nuotiosta tuleva savu ja ehkä eri versio tupakanpoltosta tulevalle savulle |
| VFX_BlackMist | Vihollisista tuleva musta usva niiden ympärille |
| VFX_Rockthrow | Kiven heitosta tuleva efekti kun kivi osuu johonkin |

### Characters

#### Pelaaja

List of animations

| Name | Description |
| --- | --- |
| Player_LanternSway | Lyhdyn heiluminen kädessä liikkuessa |
| Player_Throw | Pelaaja heittää kiven |
| Player_Pills | Pelaaja syö pillereitä |

#### Kaveri

Juttelee pelaajalle pelin alussa. Kidnapataan mökistä. Löytyy kuolleena pelin lopussa

List of animations

| Name | Description |
| --- | --- |
| Friend_Idle | |
| Friend_Walk | |

#### Zoth Ommog

ui pintaan

#### Cultist and wolf

List of animations

| Name | Description |
| --- | --- |
| Enemy_Idle | |
| Enemy_Walk | |
| Enemy_Run | |
| Enemy_Attack | |
| Enemy_PlayerKill | |
| Wolf_Run | |
| Wolf_PlayerKill | |

### Environment

#### Setting

Mökki

Metsä:

Luola:

#### Asset List

**Metsä**
- Koivu puut
- kivet
- pusikko
- järvi
- hylätty telttapaikka
- emblemeitä puihin
- pilarit
- sudet

**Luola**
- kalteri ovet
- sammal
- kynttilät
- lyhtyjä
- avaimet
- kaappeja
- pöytä
- arkku
- puulaatikko
- soppa
- ämpäreitä
- ruumiita
- alttari
- Zoth Ommog
- tukipilareita
- puuseinä
- symboleita seiniin
- iso kivi ovi
- kivisilta/riippusilta
- vesi
- hirviöpatsas

**Mökki**
- Mökki
- tuoli
- pöytä
- sänky
- kahvinkeitin
- kamina
- ikkuna
- ovi
- viina
- kalja
- makkara
- paistinpannu
- ulkohuussi

**Muu roina**
- Kädet
- lyhty
- pillerit
- notes
- Cultist
- Kaveri

#### Lighting

Night time low lighting forest. Natural moonlight/snow storm(?) and lanterns light up the area

Near pitch black cave. Lit by ambient lighting from candles and lanterns with greenish hue

### UI

### Itch Page

The itch submission requires a thumbnail (630x500 px), a banner (should include logo/game title), and screenshots. Optional background image. The page should include a brief game description and a how-to-play section that explains controls and the goal. GIFs are supported. The maximum file size for images is 3 MB; optimising for browsers is recommended.

Fonts from Google Fonts:

## Audio

### Overview

### Sound Effects and Music

| Name | Description | Prio |
| --- | --- | --- |
| SFX_WalkFloor | player/human walk in cottage | 1 |
| SFX_WalkSnow | player/human walk in snow | 1 |
| SFX_RunSnow | player/human run in snow | 1 |
| SFX_SneakSnow | player sneaking in snow | 2 |
| SFX_WalkCave | player/human walk in cave (echo) | 1 |
| SFX_RunCave | player/human run in cave (echo) | 1 |
| SFX_SneakCave | player sneaking in cave | 1 |
| SFX_WalkEnemy | enemy walk in cave (echo) | 1 |
| SFX_RunEnemy | enemy chase sound (echo) | 1 |
| SFX_WalkEnemyAlt | enemy walk in cave, but moist | 2 |
| SFX_AMBCottage | ambience of old cottage, with some wind whistling | 2 |
| SFX_AMBForest | forest ambience with snowstorm and high wind | 1 |
| SFX_AMBCave | cave ambience with dripping water and breath like wind, moving back and forth | 1 |
| SFX_AMBChamber | chamber of the old god statue that feels impossibly large | 3 |
| SFX_WolfHowling | howling of the wolves while they track player | 1 |
| SFX_WolfChase | growling of the wolves while they chase player | 1 |
| SFX_WolfAttack | wolf chomping at person | 1 |
| SFX_InteractionPaper | sound of paper crumble | 1 |
| SFX_InteractionKey | sound for picking up key | 2 |
| SFX_InteractionLock | klik kläk sound of lock | 1 |
| SFX_InteractionDoor | sound of door opening (maybe some echo) | 2 |
| SFX_Madness | whispering… effect when madness is high | 1 |
| SFX_MadnessAlt | whining mosquito like sound. effect when madness is high | 1 |
| SFX_HidingPlaceClimb | player climbs in hiding place | 2 |
| SFX_DeepBreathing | player deep breathing while hiding | 2 |
| SFX_AMBCultChant | cult chant when closing in the gods chamber | 2 |
| SFX_StoneThrow | when thrown stone hits (echo) | 1 |
| SFX_Pills | player eats pills to recover sanity | 1 |
| SFX_LootingCorpse | rifling through corpses pockets | 1 |
| SFX_TwigCrack | twig cracking in forest | 2 |
| SFX_Death | player death | 1 |
| SFX_Enemy | gurgling and other odd mouth sounds cave enemy makes (echo) | 1 |
| SFX_EnemyAttack | cave enemy attacking (echo) | 1 |
| SFX_SplashLarge | sounds of something large moving in the waters behind the gods statue | 2 |
| SFX_UIClick | UI clicking sound | 2 |
| SFX_MUSMainMenu ? | music for main menu | 3 |
| SFX_MUSMadness ? | music for madness (may not be necessary) | 4 |
| SFX_MUSChase ? | music for chase | 3 |
| SFX_MUSEnding ? | music for ending | 3 |
| SFX_VACottage (nth) | voice acting for intro, between player and his friend | 3 |
| SFX_VAForest (nth) | voice acting for player reactions in forest | 3 |
| SFX_VACave (nth) | voice acting for player reactions in cave (possibly conversations with dead bodies) | 3 |
| SFX_VAEnding (nth) | voice acting for ending, between player and his friend | 3 |

### List of sound effects

**SFX:**
- Named as "sfx_soundname"
- All sounds have the "SFX_" tag. After, comes the group, ex. AMB = ambient and finally the object/type.

**UI SFX:**
- Named as "SFX_UI+Action"

**Music:**
- Named as "SFX_MUS + location"

**Voice acting:**
- Named as "SFX_VA + location + [context +] number"

**Audio Format:**
- audio/ogg (audio/vorbis) @ 128+ kbp/s (using Vorbis 1.3 codec)

Unity seems to be quite unclear about what Vorbis features it actually supports - Unity AudioClip Documentation

## Changelog

Any notable changes, what was there originally, and why it was changed.

## REFERENCES
