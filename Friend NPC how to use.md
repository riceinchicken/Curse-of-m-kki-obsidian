# Friend NPC how to use

Kaveri (GDD "Kaveri"): juttelee pelin alussa mökissä, kidnapataan (cutscene 2), löytyy lopussa alttarilta kuolleena, ja lopetuksessa (cutscene 5) on taas elossa ja osoittaa ikkunasta ulos. Tehnyt Vili's Claude 2026-11-26. Koodi: `Assets/Scripts/NPC/`. Lisätietoa: [[Claude log]].

## Prefabit (`Assets/Art/Prefabs/NPC/`)

| Prefab | Mihin |
| --- | --- |
| **Friend** | Elävä kaveri. Vedä sceneen. Sininen viiva pään korkeudella osoittaa katsomissuunnan. |
| **FriendTalkZone** | Näkymätön laatikko, josta juttelu alkaa, kun pelaaja kävelee siihen. Scene-näkymässä se on oranssi. |
| **Friend_Dead** | Kuollut kaveri alttarille. Pelkkä malli makuuasennossa, ei skriptiä. |
| Friend_Model | Yhteinen malli. Kun oikea kaverin malli on valmis, vaihda se tähän, niin muutos näkyy kaikissa kolmessa. |

Malli on väliaikainen: kultistin malli ilman vihreitä mörkö-osia. Animaatiot (Idle, Walk) tulevat controllerista `AC_Friend`, joka on kopio kultistin controllerista. Kultistin omaan controlleriin ei siis kosketa.

## Juttelun lisääminen

1. Vedä **Friend** sceneen ja käännä se haluamaasi suuntaan.
2. Laita **Default Dialogue** -kenttään dialogi. Valmiina on `DLG_CottageIntro`.
3. Vedä **FriendTalkZone** kohtaan, johon pelaajan pitää kävellä. Säädä laatikon koko BoxColliderin *Size*-arvosta.
4. Laatikossa:
   - **Friend**: vedä kaveri tähän. Jos kenttä on tyhjä, laatikko käyttää ensimmäistä kaveria scenessä.
   - **Dialogue**: tyhjänä laatikko käyttää kaverin Default Dialoguea. Jos laatikkoon laittaa oman dialogin, samalla kaverilla voi olla eri juttuja eri paikoissa.
   - **Only Once**: juttelu käynnistyy vain kerran. Jos asetus on pois päältä, laatikko voi käynnistää juttelun uudestaan *Cooldown*-ajan jälkeen.
5. Juttelun aikana kaveri kääntyy pelaajaan päin. Dialogin loputtua se palaa paikalleen seisomaan.

Scene-näkymässä laatikon yläpuolella lukee, mikä dialogi siitä alkaa, ja viiva näyttää, mihin kaveriin laatikko on kytketty. Myöhemmin juttelun voi aloittaa myös E-napilla, kun Oton interaktio on valmis.

## Asetukset (FriendNPC)

| Asetus | Mitä tekee |
| --- | --- |
| Default Dialogue | Dialogi, jota käytetään, jos laatikolla ei ole omaa. |
| Face Player While Talking | Kaveri katsoo pelaajaa juttelun ajan. |
| Face Player When Near | Kaveri kääntyy pelaajaan päin, kun tämä tulee näin lähelle (metreinä, 0 = pois päältä). |
| Walk Speed / Turn Speed | Kävelynopeus (m/s) ja kääntymisnopeus (astetta sekunnissa). |
| Idle / Walk / Talk / Point State | Animator-tilojen nimet. Tyhjä Talk State tai Point State käyttää Idle-animaatiota. |
| Reset On Respawn | Pois päältä (oletus): kaveri pysyy ennallaan, kun pelaaja herää tallennuspisteeltä, eli kidnapattu kaveri ei palaa takaisin. |

## Cutsceneille (Otto)

| Kutsu | Mitä tekee |
| --- | --- |
| `friend.Talk(dialogi)` | Aloittaa dialogin. Ilman dialogia käytetään Default Dialoguea. |
| `friend.WalkTo(piste)` | Kävelee pisteeseen. Jos piste on objekti, kaveri kääntyy lopuksi samaan suuntaan kuin se. |
| `friend.WalkPath(reitti)` | Kävelee `PatrolPath`-reitin lapsipisteet järjestyksessä. |
| `OnArrived` | Tapahtuma, kun kaveri on perillä. |
| `friend.LookAt(kohde)` / `friend.PointAt(kohde)` | Kääntyy kohteeseen ja katsoo sitä tai osoittaa sitä (lopetuksessa ikkunaan). |
| `friend.StopAndIdle()` | Lopettaa kaiken ja jää seisomaan. |
| `friend.Disappear()` / `friend.Appear(paikka)` | Kaveri katoaa (kidnappaus) tai ilmestyy paikkaan. |

## Testaus Vili scenessä

`Friend_Test` sisältää:
- kaverin pelaajan aloituspaikan lähellä pelaajaan päin
- juttelulaatikon kaverin ympärillä
- kuolleen kaverin kohdassa x 8, z −6

Kävele kaverin luo, niin aloitusdialogi alkaa. Vanha testilaatikko `DialugueZone_testi` on sammutettu mutta ei poistettu.
