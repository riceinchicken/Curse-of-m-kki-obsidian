# Curse of Mökki – menujen muokkausohje

Päivitetty 2026-11-18 · Related: [[Claude log]] · [[Curse of the mökki GDD]]

Kaikki valikot ovat prefabeja kansiossa **`Assets/Art/Prefabs/UI/Menus/`**: `Menu_Button`, `Menu_MainMenu`, `Menu_PauseMenu`, `Menu_DeathEndScreen`, `Menu_Settings` ja `Menu_Credits`. Muokkaa niitä aina prefabin kautta:

1. Kaksoisklikkaa prefabia Project-ikkunassa, jolloin se aukeaa Prefab Modeen.
2. Tee muutokset. Ne tallentuvat automaattisesti, jos oikean yläkulman Auto Save on päällä. Muuten paina Save.
3. Jos muokkasit vahingossa scenessä olevaa kopiota, siirrä muutokset prefabiin valitsemalla Inspectorin yläosasta **Overrides > Apply All**.

## 1. Napit kaikki kerralla: Menu_Button

Jokainen nappi kaikissa valikoissa on tämän prefabin kopio, joten muutos tässä päivittyy kaikkiin nappeihin.

- **Taustakuva:** Image > **Source Image**. Venyvälle kuvalle valitse Image Type **Sliced** ja aseta reunat spriten Sprite Editorissa.
- **Värit:** Button > **Normal / Highlighted / Pressed / Selected / Disabled Color**. Nyt normaali on tummanharmaa ja hover tummanpunainen. Jos käytät värillistä spriteä, laita Normal Coloriksi valkoinen, niin kuva näkyy omissa väreissään.
    - Jos haluat eri kuvat eri tiloille, vaihda **Transition** asetukseen **Sprite Swap**.
- **Koko:** Layout Element > **Preferred Width / Height** (nyt 360 × 64).
- **Teksti:** valitse lapsiobjekti `Label` ja muuta siitä fontti, koko ja väri.
- **Yksittäisen napin teksti** (esim. "Play" → "Pelaa"): avaa valikon prefab, esim. `Menu_MainMenu`, valitse `Content/MainPanel/Buttons/PlayButton/Label` ja muuta teksti.

## 2. Päävalikko: Menu_MainMenu

Päävalikossa ovat napit Play, Settings, Credits ja Quit game.

- **Taustakuva:** `Content/Background` > Source Image. Vaihda samalla Color valkoiseksi, koska se on nyt lähes musta.
- **Otsikko:** `Content/MainPanel/Title` (TextMeshPro).
- **Logo tekstin tilalle:** poista `Title` käytöstä ja lisää `MainPanel`in alle Image, johon logo laitetaan. Anna sille Layout Element ja koko.
- **Välit:** `MainPanel` > Vertical Layout Group > **Spacing** (otsikon ja nappien väli, nyt 70). `Buttons` > Spacing (nappien väli, nyt 18).
- **Valikko vasempaan reunaan:** `MainPanel` > **Child Alignment** = Middle Left, ja lisää **Padding Left**.
- **Nappien järjestys:** raahaa napit Hierarchyssä `Buttons`-objektin alla haluamaasi järjestykseen.

## 3. Pause-valikko: Menu_PauseMenu

Pause-valikko aukeaa P:llä, ja siinä ovat Resume, Settings ja Quit to main menu.

- Valikon sisältö on piilossa. Ruksaa **`Content`** näkyviin muokkauksen ajaksi. Skripti piilottaa sen pelissä joka tapauksessa.
- **Taustan tummennus:** `Content/Dim`. Säädä Colorin alpha (nyt 65 %) tai lisää siihen kuva.
- **Otsikko:** `Content/MainPanel/Title` ("Paused"). Napit ja välit toimivat samoin kuin päävalikossa.

## 4. Death/end screen: Menu_DeathEndScreen

Sama ruutu toimii kuolemassa ("You died") ja pelin lopussa ("The End"), ja siinä ovat napit Continue ja Quit.

- Valikon sisältö on piilossa. Ruksaa **`Content`** näkyviin muokkauksen ajaksi.
- **Tausta:** `Content/Background` (nyt tummanpunainen, 85 %). Voit lisätä siihen kuvan.
- **Otsikon ulkoasu:** `Content/MainPanel/Title` (fontti, koko, väri).
- **Otsikon teksti:** juuriobjektin `DeathEndScreen`-komponentissa kentät **Death Title** ("You died") ja **End Title** ("The End"). Skripti vaihtaa otsikon näiden mukaan, joten älä kirjoita tekstiä suoraan Title-objektiin.
- **Credits:** end-ruudun Continue näyttää `Content/Credits`-paneelin. Se on sama `Menu_Credits`-prefab kuin päävalikossa, joten tekstit päivittyvät kaikkialle.

## 5. Settings: Menu_Settings

Settings-paneelia käytetään sekä päävalikossa että pause-valikossa, joten muutokset näkyvät molemmissa automaattisesti.

- **Paneelin tausta:** `Panel` > Image (sprite tai väri). Paneelin koko asettuu sisällön mukaan automaattisesti, joten reunusta säädetään kohdasta **Padding** (nyt 40).
- **Rivien tekstit:** `Row_*/Label`, esim. "Music volume" → "Musiikki".
- **Liukusäätimen ulkoasu:** `Slider`in osat `Background`, `Fill Area/Fill` ja `Handle Slide Area/Handle`. Jokaisessa on oma Image.
- **Valintaruudun ulkoasu:** `Toggle/Background` ja `Background/Checkmark`.

## 6. Credits: Menu_Credits

Credits-teksti on objektissa `Panel/Body`, ja nyt siinä on placeholderit, jotka vaihdetaan oikeisiin (artistit, äänilähteet).

- **Muotoilu:** tagit `<b>lihavoitu</b>`, `<size=36>iso</size>` ja `<color=#C03030>punainen</color>` toimivat tekstissä.
- **Leveys:** Layout Element > Preferred Width (nyt 700).
- Jos credits-teksti kasvaa pitkäksi, pyydä Claudea lisäämään vieritys.

## 7. HUD: HUD_Player ja HUD_ItemSlot

HUD on omassa kansiossaan **`Assets/Art/Prefabs/UI/HUD/`**, ja sitä muokataan samoin kuin menuja: avaa prefab ja tee muutokset. Pelaajan esineet näkyvät hetken jokaisen muutoksen jälkeen ja häipyvät sitten, mutta avaimet pysyvät näkyvissä koko ajan.

- **Sijainti:** `HUD_Player/BottomLeft` on ankkuroitu vasempaan alakulmaan, ja etäisyys reunoista on Pos X / Pos Y (nyt 40, 40). Jos haluat HUDin toiseen kulmaan, vaihda **Anchor** ja **Pivot** samaan kulmaan.
- **Rakenne:**
    - `BottomLeft/Fading` häipyy: `ChangeNote` (teksti, esim. "+1 Pills") ja `ItemRow` (lyhty, pillerit).
    - `BottomLeft/AlwaysVisibleRow` ei häivy: avaimet.
- **Välit:** riveissä Horizontal Layout Group > **Spacing** (nyt 16), `BottomLeft`issa ja `Fading`issa 6.
- **Esimerkkipaikat:** molemmissa riveissä on "Example slot (removed in game)", jotta asettelun näkee editorissa. Peli poistaa ne alussa, joten muuta esineen ulkoasua `HUD_ItemSlot`-prefabissa, älä niissä.
- **Yhden esineen ulkoasu: `HUD_ItemSlot`:**
    - `Icon` on ikonikuva. Koko asetetaan kohdasta Layout Element > Preferred Width / Height (nyt 64 × 64).
    - `Label` näyttää määrän (esim. "x2"). Jos esineellä ei ole vielä ikonia, se näyttää myös nimen.
    - `HUDItemSlot`-komponentissa: **Icon Color** = ikonien sävy (valkoinen = kuvan omat värit) ja **Placeholder Icon Color** = himmeä neliö esineille, joilla ei vielä ole ikonia.
- **Esineiden ikonit ja näkyvyys** asetetaan esineassettiin, ei HUDiin: `Assets/Data/Items/ITEM_*` > **Icon**, **Show In HUD** ja **Always Show While Carried**. Ruksaa viimeinen jokaiseen avaimeen.
- **Ajat:** `HUD_Player`in `PlayerHUD`-komponentissa **Show Seconds** (nyt 3), **Fade Seconds** (nyt 1) ja **Show Change Note**.
- **Älä poista** objekteja `Fading`, `ChangeNote`, `ItemRow` ja `AlwaysVisibleRow`. Jos `PlayerHUD`-komponentin kentässä lukee **None**, raahaa oikea objekti takaisin.
- **Testaus:** HUD näkyy vain, kun inventaario muuttuu. Vili scenessä testinäppäimet ovat 1 = lyhty, 2 = pilleri ja 3 = avain, ja Left Alt + numero poistaa.

## 8. Fontit ja kuvat

Fontti tuodaan TextMeshPron font assetiksi, ja kuvat nimetään GDD:n mukaan.

- **Fontti** (GDD: Google Fonts):
    1. Tuo .ttf esim. kansioon `Assets/Art/UI/Fonts/`.
    2. Klikkaa sitä oikealla ja valitse **Create > TextMeshPro > Font Asset**.
    3. Valitse syntynyt font asset tekstien kohtaan **Font Asset**. Aloita `Menu_Button`in `Label`ista, niin kaikki napit vaihtuvat kerralla.
    4. Jos otsikossa ö ei näy, varmista, että font asset on **Dynamic**.
- **UI-kuvat:**
    - Nimeä ne GDD:n mukaan `UI_ObjectName` ja laita kansioon `Assets/Art/UI/`.
    - Import-asetuksissa Texture Type on **Sprite (2D and UI)**.
    - Koot ovat GDD:n mukaan kahden potensseja.

## 9. Säännöt, ettei mikään hajoa

Ulkoasun muuttaminen, uudelleennimeäminen ja siirtäminen ovat turvallisia, mutta skriptien käyttämiä objekteja ei saa poistaa.

- **Älä poista** näitä objekteja:
    - `Content` ja `MainPanel`
    - napit
    - `Settings` ja `Credits`
    - death/end screenin `Title`
    - liukusäätimet, valintaruudut ja `BackButton`
- **Jos jokin nappi lakkaa toimimasta:**
    1. Valitse valikon juuriobjekti, esim. `Menu_MainMenu`.
    2. Katso komponentti `MainMenu`, `PauseMenu`, `DeathEndScreen` tai `SettingsMenu`.
    3. Jos jossain kentässä lukee **None**, raahaa oikea objekti takaisin siihen.
- **Koristekuvat nappien päällä:** poista niistä **Raycast Target**, muuten napit eivät reagoi klikkaukseen.
- **Testaa Game-ikkunassa** resoluutiolla **1920 × 1080**. Valikot skaalautuvat muihin resoluutioihin siitä.

## 10. Toiminta-asetukset juuriobjektien Inspectorissa

Toiminta säädetään valikoiden juuriobjektien Inspectorista. Oletuksena kaikki toimii testitilassa yhden scenen sisällä.

- **`Menu_MainMenu`:**
    - **Load Scene On Play:** pois = Play vain sulkee valikon (testaus). Päällä = Play lataa scenen kentästä **Play Scene Name** ("MAIN SCENE").
    - **Open On Start:** näytetäänkö valikko scenen alussa.
    - **Pause Game While Open:** pysäyttääkö valikko pelin ajan, kun se on auki.
- **`Menu_PauseMenu`:**
    - **Pause Key:** P.
    - **Load Scene On Quit** ja **Main Menu Scene Name:** mihin "Quit to main menu" vie.
- **`Menu_DeathEndScreen`:**
    - **Show On Death:** avautuuko ruutu automaattisesti pelaajan kuollessa.
    - **Death Delay:** viive kuolemasta ruutuun (nyt 1,5 s), jotta kuolemaefekti ehtii näkyä.
    - **Load Scene On Quit** ja **Main Menu Scene Name:** mihin Quit vie. Testitilassa Quit käynnistää scenen uudelleen päävalikko auki.
    - Death-tilan Continue käynnistää scenen toistaiseksi uudelleen. Kun Save Point System on tehty, se vie viimeisimpään tallennuspisteeseen.

Muista lopuksi commitoida prefabit ja uudet kuvat ja fontit .meta-tiedostoineen.
