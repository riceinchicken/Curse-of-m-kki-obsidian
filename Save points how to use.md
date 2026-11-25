# Save points how to use

Tallennuspisteet (GDD: aktivoituvat etenemällä, pelaaja herää niistä kuoleman jälkeen). Tehnyt Vili's Claude 2026-11-25. Koodi: `Assets/Scripts/SavePoints/`. Lisää tietoa: [[Claude log]].

## Lisääminen sceneen

1. Vedä **`Assets/Art/Prefabs/SavePoints/SavePoint.prefab`** sceneen kohtaan, jonka läpi pelaaja kulkee.
2. Säädä keltainen laatikko peittämään koko kulkureitti. Laatikkoa voi skaalata tai muuttaa sen kokoa BoxColliderin *Size*-arvosta. Kun pelaaja kävelee laatikkoon, tallennuspiste aktivoituu ja muuttuu vihreäksi.
3. Siirrä lapsiobjekti **`SpawnPoint`** maahan kohtaan, johon pelaaja ilmestyy. Sininen nuoli näyttää suunnan, johon pelaaja katsoo herätessään. Käännä nuolta Y-akselin ympäri. Pelaajan jalat asettuvat automaattisesti maahan.
4. Aseta **Order** juoksevana numerona kentän järjestyksessä: 0, 1, 2… Kun *Only Forward* on päällä, aiempaan tallennuspisteeseen palaaminen ei siirrä heräämispaikkaa taaksepäin.
5. Kentän ensimmäisessä tallennuspisteessä laita **Active At Start** päälle, jotta pelaaja herää siitä, vaikka ei olisi vielä kulkenut minkään tallennuspisteen läpi.

GDD:n mukaan metsässä on yksi tallennuspiste ennen suden jahtia, ja luolassa jokaisen "huoneen" alussa.

## Asetukset (Inspector)

| Asetus | Mitä tekee |
| --- | --- |
| Spawn Point | Paikka, johon pelaaja ilmestyy (lapsi `SpawnPoint`). Tyhjänä käytetään itse objektin paikkaa. |
| Respawn Madness | Madness heräämisen jälkeen (0 = täysin järjissään). |
| Snap To Ground | Asettaa pelaajan jalat maahan spawn-pisteen alapuolella. |
| Active At Start | Tallennuspiste on aktiivinen heti scenen alussa. |
| Order / Only Forward | Kentän järjestys. Aiempi tallennuspiste ei vie heräämispaikkaa takaisin. |
| Activate Sound | Valinnainen AudioManager-ääni, joka soi aktivoituessa. |
| On Activated / On Respawned | Valinnaiset tapahtumat, joihin voi kytkeä esimerkiksi valon tai efektin. |

## Mitä kuolemassa tapahtuu

- Kuolinruudun **Continue** siirtää pelaajan viimeisimpään aktivoituun tallennuspisteeseen. Madness palaa *Respawn Madness* -arvoon.
- Kultistit palaavat aloituspaikoilleen ja alkavat taas partioida. Susi palaa reitin alkuun ja odottaa uudestaan.
- Inventaario pysyy ennallaan. Dialogit ja Sanity lost -alueet, jotka toimivat vain kerran, eivät toistu.
- Jos mikään tallennuspiste ei ole aktiivinen, Continue aloittaa scenen alusta, kuten ennenkin.
- Tallennus on vain muistissa. Peli ei tallennu levylle, ja päävalikkoon poistuminen nollaa tallennuspisteet.

## Testaaminen

- Paina Playtä. Valitse tallennuspiste ja klikkaa hiiren oikealla SavePoint-komponentin otsikkoa. Valitse **Test: activate and respawn here**.
- Toinen tapa: kävele laatikon läpi ja kuole, esimerkiksi MadnessDebug-skriptillä: paina H, kunnes madness on vaara-alueella, ja sitten vielä kerran H. Paina sen jälkeen Continue.

Vili scenessä on valmiina kaksi testipistettä kansiossa `SavePoints`: `SavePoint_Start` pelaajan aloituspaikassa ja `SavePoint_BeforeWolf` ennen suden jahdin aloitusympyrää.

## Omat skriptit (ohjelmoijille)

Jos jonkin oman objektin pitää palata alkutilaan heräämisen yhteydessä, toteuta sille `IRespawnResettable` (metodi `ResetForRespawn()`). Kutsu `SavePointSystem.Register(this)` metodissa `Awake` ja `SavePointSystem.Unregister(this)` metodissa `OnDestroy`. Toinen vaihtoehto on kuunnella tapahtumaa `SavePointSystem.OnRespawned`.
