Päivitetty 2026-11-20 · Related: [[Claude log]] · [[Curse of the mökki GDD]] · [[Curse of Mökki – menujen muokkausohje]]

Viholliset kävelevät partioreittiä, joka on scenessä tavallinen objekti. Reitin pisteet ovat sen lapsiobjekteja. Reittiä muokataan siirtämällä, lisäämällä tai poistamalla pisteitä, eikä koodiin tarvitse koskea.

## Mistä löytyy

| Mitä | Missä |
|---|---|
| Reitin skripti | `Assets/Scripts/Enemies/PatrolPath.cs` |
| Kultistin aivot (partiointi, näkeminen, jahtaaminen, lyönti) | `Assets/Scripts/Enemies/CultistAI.cs` |
| Madness-aura | `Assets/Scripts/Enemies/EnemyMadnessAura.cs` |
| Kultistin animaattori (Idle, Walk, Attack) | `Assets/Art/FBX/AC_Cultist.controller` |
| Kultisti MAIN SCENEssä | Hierarchy: **`cultist`** |
| Kultistin reitti MAIN SCENEssä | Hierarchy: **`Cultist_PatrolPath`** (lapset `Point 1` … `Point 7`) |

Kultistin `CultistAI`-komponentin kentässä **Path** näkyy, mitä reittiä se kävelee.

**Scene-näkymässä** (Gizmos-painike päällä):
- **oranssi viiva ja pallot** = reitti ja sen pisteet
- **violetti pallo** = madness-aura
- kun kultisti on valittuna: **keltainen kartio** = mihin se näkee, **punainen ympyrä** = lyöntietäisyys, **oranssi ympyrä** = etäisyys, jolta se huomaa pelaajan mistä suunnasta tahansa

### Esimerkki: kultistin reitin jatkaminen

1. Valitse Hierarchystä `Cultist_PatrolPath` > `Point 7` (reitin viimeinen piste).
2. Paina **Ctrl+D**. Syntyy `Point 7 (1)`, joka on Hierarchyssä listan viimeisenä, eli siitä tulee reitin uusi päätepiste.
3. Siirrä uusi piste Scene-näkymässä sinne, mihin reitin pitää jatkua. Pidä se maan pinnalla, esimerkiksi raahaamalla sitä ylhäältäpäin katsottuna ja korjaamalla korkeutta (Y) niin, että piste on maassa.
4. Tarkista oranssista viivasta, ettei suora viiva pisteiden välillä kulje kiven tai seinän läpi (katso kohta *Rajoitukset*).
5. Paina **Play** ja katso, että kultisti kävelee reitin loppuun ja kääntyy takaisin.
6. Tallenna scene (**Ctrl+S**) ja commitoi GitHub Desktopissa `MAIN SCENE.unity`.

## 1. Reitin muokkaus

- **Pisteen siirto:** valitse piste Hierarchystä ja siirrä sitä Scene-näkymässä (W-työkalu).
- **Uusi piste:** valitse piste ja paina **Ctrl+D**. Järjestys Hierarchyssä on kävelyjärjestys, joten vedä uusi piste oikeaan kohtaan listassa.
- **Pisteen poisto:** valitse ja paina Delete. Reitti toimii, kun siinä on vähintään 2 pistettä.
- **Koko reitin siirto:** siirrä `Cultist_PatrolPath`-objektia, jolloin kaikki pisteet siirtyvät mukana.

Pisteiden nimillä ei ole väliä, vain järjestyksellä.

## 2. Uusi reitti uudelle viholliselle

1. Hierarchyssä **oikea klikkaus > Create Empty**. Nimeä se, esimerkiksi `Cultist2_PatrolPath`.
2. Inspectorissa **Add Component > Patrol Path**.
3. Lisää pisteet: oikea klikkaus reittiobjektin päällä > **Create Empty**, jolloin piste syntyy lapseksi. Tee niitä niin monta kuin tarvitset ja siirrä ne paikoilleen.
4. Valitse vihollinen ja vedä reittiobjekti Hierarchystä sen `CultistAI`-komponentin kenttään **Path**.

Uusi kultisti tehdään helpoimmin kopioimalla nykyinen: valitse `cultist`, paina Ctrl+D ja vaihda kopion **Path**-kenttään oma reitti. Kopiossa ovat valmiina materiaalit, animaattori, aura ja kaikki asetukset.

Jos **Path** on tyhjä, vihollinen seisoo paikallaan, mutta näkee, jahtaa ja lyö silti. Sopii vartijaksi.

Sama `PatrolPath` käy myöhemmin myös muille vihollisille (esim. susi).

## 3. Asetukset

### Patrol Path (reittiobjektissa)

| Asetus | Oletus | Mitä tekee |
|---|---|---|
| **Mode** | Back And Forth | *Back And Forth* = 1-2-3-2-1… *Loop* = 1-2-3-1-2-3… |
| Gizmo Color | oranssi | Reitin väri Scene-näkymässä |

### Cultist AI: partiointi

| Asetus | Oletus | Mitä tekee |
|---|---|---|
| **Path** | – | Reitti, jota kävellään |
| **Walk Speed** | 1,4 | Kävelynopeus (m/s) |
| **Wait At Point** | 2 | Sekuntia, jotka seistään pysähtyessä |
| **Wait At Every Point** | pois | Pois = pysähtyy vain reitin päissä. Päällä = jokaisessa pisteessä |
| Point Reach Distance | 0,4 | Kuinka läheltä piste lasketaan saavutetuksi (m) |
| Turn Speed | 300 | Kääntymisnopeus (astetta/s) |
| **Model Yaw Offset** | 180 | Mallin suunnan korjaus. Kultistin malli katsoo väärään suuntaan, siksi 180. Jos uusi malli kävelee takaperin, vaihda 0 ↔ 180; jos sivuttain, kokeile 90 tai -90 |

### Cultist AI: näkeminen

| Asetus | Oletus | Mitä tekee |
|---|---|---|
| **View Distance** | 15 | Kuinka kauas näkee (m) |
| **View Angle** | 120 | Näkökartion leveys (astetta) |
| Notice Distance | 2,5 | Tätä lähempänä huomaa pelaajan mistä suunnasta tahansa |
| Eye Height | 1,6 | Silmien korkeus |
| Sight Blockers | Everything | Mitkä layerit estävät näkemisen (seinät, maasto) |
| Lose Sight Seconds | 3 | Kun pelaaja on näin kauan poissa näkyvistä, jahti loppuu |
| Search Seconds | 3 | Kuinka kauan etsii paikassa, jossa näki pelaajan viimeksi, ennen kuin palaa reitille |

Piiloutunutta pelaajaa ei nähdä. Etsimisen jälkeen vihollinen palaa reitin lähimpään pisteeseen ja jatkaa partiointia.

### Cultist AI: jahti ja lyönti

| Asetus | Oletus | Mitä tekee |
|---|---|---|
| **Chase Speed** | 3,5 | Nopeus jahdatessa (m/s) |
| **Attack Range** | 1,6 | Etäisyys, jolta hyökkäys alkaa (m) |
| **Attack Windup** | 0,45 | Sekuntia hyökkäyksen alusta siihen, kun lyönti osuu. 0,45 = kädet täysin ojennettuina animaatiossa. Pelaaja ehtii vielä juosta karkuun |
| Hit Madness | 0 | Lyönnin lisäämä madness. 0 = pelaajan oletus (25). Vaara-alueella lyönti tappaa |
| **Disappear After Hit** | päällä | GDD: vihollinen katoaa, jos lyönti ei tappanut |
| Disappear Delay | 0,8 | Sekuntia lyönnistä katoamiseen |
| Attack Finish Seconds | 0,8 | Kuinka kauan hyökkäysanimaatio saa jatkua lyönnin jälkeen |

### Cultist AI: maa ja animaatio

| Asetus | Oletus | Mitä tekee |
|---|---|---|
| Snap To Ground | päällä | Pitää jalat maassa |
| Ground Layers | Everything | Mitä maana pidetään |
| Animator | – | Tyhjä = käyttää omaa Animatoria |
| Idle / Walk / Attack State | Idle / Walk / Attack | Animaattorin tilojen nimet. Jos Attack-tilaa ei ole, käytetään idleä |
| Walk Anim Speed | 1 | Kävelyanimaation nopeus partioidessa |
| Chase Anim Speed | 2 | Kävelyanimaation nopeus jahdatessa (juoksuanimaatiota ei vielä ole) |

### Enemy Madness Aura

| Asetus | Oletus | Mitä tekee |
|---|---|---|
| **Aura Enabled** | päällä | Aura päälle tai pois |
| **Radius** | 8 | Auran säde (m) |
| **Drain Multiplier** | 1 | Kuinka nopeasti madness nousee auran sisällä. 1 = 5 / s, 0,5 = puolet. Ei koskaan tapa |
| Needs Line Of Sight | pois | Päällä = toimii vain, kun välissä ei ole seinää |
| Centre Height | 1,2 | Auran keskipisteen korkeus |

## 4. Rajoitukset

- **Ei NavMeshiä:** vihollinen kävelee suoraan pisteestä toiseen ja jahdatessa suoraan pelaajaa kohti. Se kulkee puiden läpi (niissä ei ole collidereita) ja kivien tai seinien läpi, jos suora viiva kulkee niiden kautta. Laita reitille tarpeeksi pisteitä, että viivat kiertävät esteet. Luolaa varten NavMesh kannattaa ehkä lisätä myöhemmin.
- **Ei juoksuanimaatiota:** jahdatessa kävelyanimaatio pyörii nopeammin.

## 5. Ongelmia

| Ongelma | Korjaus |
|---|---|
| Vihollinen seisoo eikä kävele | Onko **Path**-kentässä reitti? Onko reitillä vähintään 2 pistettä? |
| Kävelee takaperin tai sivuttain | Muuta **Model Yaw Offset** (0 ↔ 180, tai 90 / -90) |
| Leijuu tai uppoaa maahan | Siirrä reitin pisteet maan pinnalle. Tarkista, että **Snap To Ground** on päällä ja maastossa on collider |
| Ei näe pelaajaa | Kasvata **View Distance** tai **View Angle**. Tarkista, ettei jokin ylimääräinen collider ole välissä (Sight Blockers) |
| Näkee seinien läpi | Seinässä ei ole collideria. Lisää se, tai rajaa **Sight Blockers** oikeisiin layereihin |
| Animaatio ei pyöri | Onko Animatorissa controller `AC_Cultist`? Onko klipin import-asetuksissa **Loop Time** päällä (kävely ja idle)? |
| Katoaa lyötyään | GDD:n mukaista. Jos otat **Disappear After Hit** pois päältä, vihollinen jää lyönnin jälkeen seisomaan paikalleen |

Muista tallentaa scene (**Ctrl+S**) ja commitoida muutokset GitHub Desktopissa.
