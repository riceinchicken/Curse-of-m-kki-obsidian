 
Päivitetty 2026-11-18 · Related: [[Claude log]] · [[Curse of the mökki GDD]]

Kaikki valikot ovat prefabeja kansiossa **`Assets/Art/Prefabs/UI/Menus/`**: `Menu_Button`, `Menu_MainMenu`, `Menu_PauseMenu`, `Menu_DeathEndScreen`, `Menu_Settings` ja `Menu_Credits`. Muokkaa niitä aina prefabin kautta:

1. Kaksoisklikkaa prefabia Project-ikkunassa, jolloin se aukeaa Prefab Modeen.
2. Tee muutokset. Ne tallentuvat automaattisesti, jos oikean yläkulman Auto Save on päällä. Muuten paina Save.
3. Jos muokkasit vahingossa scenessä olevaa kopiota, siirrä muutokset prefabiin valitsemalla Inspectorin yläosasta **Overrides > Apply All**.

### Esimerkki: pillerit saavat oman ikonin

Ikoni vaihdetaan esineeseen (`ITEM_Pills`), ei HUDiin, joten sama kuva näkyy kaikkialla, missä pillerit näytetään.

1. **Tee kuva:** PNG, läpinäkyvä tausta, koko kahden potenssi (esim. 128 × 128). Nimeä se GDD:n mukaan: `UI_Pills.png`.
2. **Tuo kuva Unityyn:** raahaa tiedosto Project-ikkunaan kansioon `Assets/Art/UI/`.
3. **Tee siitä sprite:** valitse `UI_Pills` Project-ikkunassa. Vaihda Inspectorissa **Texture Type** = **Sprite (2D and UI)** ja paina **Apply**.
4. **Avaa esine:** valitse Project-ikkunassa `Assets/Data/Items/ITEM_Pills`, jolloin sen asetukset näkyvät Inspectorissa.
5. **Liitä ikoni:** raahaa `UI_Pills` Project-ikkunasta `ITEM_Pills`in kenttään **Icon**.
6. **Testaa:** avaa Vili scene, paina Play, klikkaa Game-ikkunaa ja paina **2**. Vasempaan alakulmaan tulee pilleripurkin kuva ja teksti "x1". Nimi "Pills" ei enää näy, koska esineellä on nyt ikoni.
7. **Commitoi** GitHub Desktopissa `UI_Pills.png`, sen `.meta` ja `ITEM_Pills.asset`.

Jos kuva on liian iso tai pieni, muuta kokoa `HUD_ItemSlot`-prefabissa: `Icon` > Layout Element > Preferred Width / Height. Jos kuva näyttää sumealta, vaihda kuvan import-asetuksista **Filter Mode** = **Point** ja **Compression** = **None**, ja paina **Apply**.

## 1. Napit kaikki kerralla: Menu_Button

Jokainen nappi kaikissa valikoissa on tämän prefabin kopio, joten muutos tässä päivittyy kaikkiin nappeihin.

- **Taustakuva:** Image > **Source Image**. Venyvälle kuvalle valitse Image Type **Sliced** ja aseta reunat spriten Sprite Editorissa.
- **Värit:** Button > **Normal / Highlighted / Pressed / Selected / Disabled Color**. Nyt normaali on tummanharmaa ja hover tummanpunainen. Jos käytät värillistä spriteä, laita Normal Coloriksi valkoinen, niin kuva näkyy omissa väreissään.
    - Jos haluat eri kuvat eri tiloille, vaihda **Transition** asetukseen **Sprite Swap**.
- **Koko:** Layout Element > **Preferred Width / Height** (nyt 360 × 64).
- **Teksti:** valitse lapsiobjekti `Label` ja muuta siitä fontti, koko ja väri.
- **Yksittäisen napin teksti** (esim. "Play" → "Pelaa"): avaa valikon prefab, esim. `Menu_MainMenu`, valitse `Content/MainPanel/Buttons/PlayButton/Label` ja muuta teksti.

### Esimerkki: kaikki napit saavat oman kuvan ja punaisen hover-värin

1. Tee nappikuva `UI_Button.png` (esim. 512 × 128): koristeelliset reunat ja tasainen keskiosa.
2. Raahaa kuva kansioon `Assets/Art/UI/`, valitse se ja vaihda Inspectorissa **Texture Type** = **Sprite (2D and UI)**. Paina **Apply**.
3. Paina samassa Inspectorissa **Sprite Editor** ja vedä vihreät reunaviivat koristereunojen sisäpuolelle, jolloin keskiosa venyy mutta reunat eivät. Paina **Apply**.
4. Kaksoisklikkaa `Menu_Button`-prefabia ja valitse juuriobjekti `Menu_Button`.
5. Valitse Image-komponentissa **Source Image** = `UI_Button` ja **Image Type** = **Sliced**.
6. Valitse Button-komponentissa **Normal Color** = valkoinen (kuva omissa väreissään) ja **Highlighted Color** = punertava (väri, kun hiiri on napin päällä).
7. Tallenna ja testaa: Vili scene, Play ja vie hiiri napin päälle. Kaikkien valikoiden napit ovat muuttuneet.

## 2. Päävalikko: Menu_MainMenu

Päävalikossa ovat napit Play, Settings, Credits ja Quit game.

- **Taustakuva:** `Content/Background` > Source Image. Vaihda samalla Color valkoiseksi, koska se on nyt lähes musta.
- **Otsikko:** `Content/MainPanel/Title` (TextMeshPro).
- **Logo tekstin tilalle:** poista `Title` käytöstä ja lisää `MainPanel`in alle Image, johon logo laitetaan. Anna sille Layout Element ja koko.
- **Välit:** `MainPanel` > Vertical Layout Group > **Spacing** (otsikon ja nappien väli, nyt 70). `Buttons` > Spacing (nappien väli, nyt 18).
- **Valikko vasempaan reunaan:** `MainPanel` > **Child Alignment** = Middle Left, ja lisää **Padding Left**.
- **Nappien järjestys:** raahaa napit Hierarchyssä `Buttons`-objektin alla haluamaasi järjestykseen.

### Esimerkki: päävalikkoon taustakuva

1. Tee kuva `UI_MainMenuBackground.png` (esim. 2048 × 1024) ja raahaa se kansioon `Assets/Art/UI/`. Vaihda **Texture Type** = **Sprite (2D and UI)** ja paina **Apply**.
2. Kaksoisklikkaa `Menu_MainMenu`-prefabia ja valitse `Content/Background`.
3. Valitse Image-komponentissa **Source Image** = `UI_MainMenuBackground` ja vaihda **Color** valkoiseksi.
4. Jos otsikko ei erotu kuvasta, valitse `Content/MainPanel/Title` ja vaihda tekstin väri kohdasta **Vertex Color**.
5. Tallenna ja testaa: Play. Päävalikko näkyy taustakuvan päällä.

## 3. Pause-valikko: Menu_PauseMenu

Pause-valikko aukeaa P:llä, ja siinä ovat Resume, Settings ja Quit to main menu.

- Valikon sisältö on piilossa. Ruksaa **`Content`** näkyviin muokkauksen ajaksi. Skripti piilottaa sen pelissä joka tapauksessa.
- **Taustan tummennus:** `Content/Dim`. Säädä Colorin alpha (nyt 65 %) tai lisää siihen kuva.
- **Otsikko:** `Content/MainPanel/Title` ("Paused"). Napit ja välit toimivat samoin kuin päävalikossa.

### Esimerkki: pause-valikko suomeksi ja kevyempi tummennus

1. Kaksoisklikkaa `Menu_PauseMenu`-prefabia ja ruksaa `Content` näkyviin.
2. Valitse `Content/MainPanel/Title` ja vaihda teksti: "Tauko".
3. Vaihda nappien tekstit: `Content/MainPanel/Buttons/ResumeButton/Label` = "Jatka", `SettingsButton/Label` = "Asetukset" ja `QuitToMainMenuButton/Label` = "Päävalikkoon".
4. Valitse `Content/Dim`, avaa **Color** ja laske **A** (läpinäkyvyys) esim. arvosta 165 arvoon 100.
5. Poista `Content`-ruksi ja tallenna.
6. Testaa: Play, paina päävalikon Play-nappia ja sitten **P**.

## 4. Death/end screen: Menu_DeathEndScreen

Sama ruutu toimii kuolemassa ("You died") ja pelin lopussa ("The End"), ja siinä ovat napit Continue ja Quit.

- Valikon sisältö on piilossa. Ruksaa **`Content`** näkyviin muokkauksen ajaksi.
- **Tausta:** `Content/Background` (nyt tummanpunainen, 85 %). Voit lisätä siihen kuvan.
- **Otsikon ulkoasu:** `Content/MainPanel/Title` (fontti, koko, väri).
- **Otsikon teksti:** juuriobjektin `DeathEndScreen`-komponentissa kentät **Death Title** ("You died") ja **End Title** ("The End"). Skripti vaihtaa otsikon näiden mukaan, joten älä kirjoita tekstiä suoraan Title-objektiin.
- **Credits:** end-ruudun Continue näyttää `Content/Credits`-paneelin. Se on sama `Menu_Credits`-prefab kuin päävalikossa, joten tekstit päivittyvät kaikkialle.

### Esimerkki: kuolemaruutu suomeksi

1. Kaksoisklikkaa `Menu_DeathEndScreen`-prefabia ja valitse juuriobjekti.
2. Vaihda `DeathEndScreen`-komponentissa **Death Title** = "Kuolit" ja **End Title** = "Loppu".
3. Ruksaa `Content` näkyviin ja vaihda nappien tekstit: `Content/MainPanel/Buttons/ContinueButton/Label` = "Jatka" ja `QuitButton/Label` = "Lopeta".
4. Halutessasi vaihda `Content/Background`in **Color**, esim. kirkkaammaksi punaiseksi.
5. Poista `Content`-ruksi ja tallenna.
6. Testaa: Play, päävalikon Play-nappi, paina **J** ja odota, että palkki on punaisen viivan (80 %) yli. Paina sitten **J** uudelleen ja **H**. Kuolemaruutu tulee 1,5 sekunnin päästä.

## 5. Settings: Menu_Settings

Settings-paneelia käytetään sekä päävalikossa että pause-valikossa, joten muutokset näkyvät molemmissa automaattisesti.

- **Paneelin tausta:** `Panel` > Image (sprite tai väri). Paneelin koko asettuu sisällön mukaan automaattisesti, joten reunusta säädetään kohdasta **Padding** (nyt 40).
- **Rivien tekstit:** `Row_*/Label`, esim. "Music volume" → "Musiikki".
- **Liukusäätimen ulkoasu:** `Slider`in osat `Background`, `Fill Area/Fill` ja `Handle Slide Area/Handle`. Jokaisessa on oma Image.
- **Valintaruudun ulkoasu:** `Toggle/Background` ja `Background/Checkmark`.

### Esimerkki: asetukset suomeksi

1. Kaksoisklikkaa `Menu_Settings`-prefabia.
2. Valitse `Panel/Title` ja vaihda teksti: "Asetukset".
3. Vaihda rivien tekstit: `Panel/Row_MusicVolume/Label` = "Musiikki", `Row_MusicOn/Label` = "Musiikki päällä", `Row_SfxVolume/Label` = "Äänitehosteet" ja `Row_SfxOn/Label` = "Äänet päällä".
4. Valitse `Panel/BackButton/Label` ja vaihda teksti: "Takaisin".
5. Jos jokin teksti ei mahdu, suurenna sen `Label`in Layout Element > **Preferred Width** (nyt 300).
6. Tallenna. Muutos näkyy sekä päävalikon että pause-valikon asetuksissa.

## 6. Credits: Menu_Credits

Credits-teksti on objektissa `Panel/Body`, ja nyt siinä on placeholderit, jotka vaihdetaan oikeisiin (artistit, äänilähteet).

- **Muotoilu:** tagit `<b>lihavoitu</b>`, `<size=36>iso</size>` ja `<color=#C03030>punainen</color>` toimivat tekstissä.
- **Leveys:** Layout Element > Preferred Width (nyt 700).
- Jos credits-teksti kasvaa pitkäksi, pyydä Claudea lisäämään vieritys.

### Esimerkki: artistien nimet creditseihin

1. Kaksoisklikkaa `Menu_Credits`-prefabia ja valitse `Panel/Body`.
2. Etsi Inspectorin TextMeshPro-komponentin **Text Input** -kentästä kohta "(artists)" ja kirjoita nimet sen tilalle, jokainen omalle rivilleen (Enter).
3. Kirjoita kohtaan "(sources)" äänien lähteet, esim. "Askeleet – freesound.org / käyttäjänimi (CC BY 4.0)".
4. Tallenna. Credits näkyy päävalikossa ja pelin lopussa.

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

### Esimerkki: Google-fontti nappeihin ja otsikkoon

1. Lataa fontti osoitteesta fonts.google.com (**Get font > Download**) ja pura zip.
2. Raahaa .ttf-tiedosto kansioon `Assets/Art/UI/Fonts/`.
3. Klikkaa .ttf:ää oikealla ja valitse **Create > TextMeshPro > Font Asset** (valitse **SDF**, jos kysytään).
4. Valitse syntynyt font asset ja tarkista, että **Atlas Population Mode** on **Dynamic**, jotta ä ja ö toimivat.
5. Kaksoisklikkaa `Menu_Button`-prefabia, valitse `Label` ja vaihda **Font Asset** uuteen fonttiin. Kaikki napit vaihtuvat.
6. Kaksoisklikkaa `Menu_MainMenu`-prefabia, valitse `Content/MainPanel/Title` ja vaihda samoin **Font Asset**.
7. Tallenna ja testaa: Play. Commitoi .ttf, font asset ja niiden .meta-tiedostot.

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

### Esimerkki: Credits-nappi lakkasi toimimasta

Tilanne: poistit vahingossa päävalikon `CreditsButton`in ja teit tilalle uuden.

1. Jos poisto oli juuri, kokeile ensin **Ctrl+Z**.
2. Muuten avaa `Menu_MainMenu`-prefab ja valitse juuriobjekti. `MainMenu`-komponentin kentässä **Credits Button** lukee **None**.
3. Raahaa uusi nappi Hierarchysta kenttään **Credits Button**.
4. Tallenna ja testaa: Play ja Credits.

Uuden napin saa helpoiten kopioimalla toisen napin (**Ctrl+D**) ja vaihtamalla sen `Label`in tekstin.

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

### Esimerkki: Play vie MAIN SCENEen

1. Avaa Vili scene ja valitse Hierarchysta `Menu_MainMenu`.
2. Ruksaa `MainMenu`-komponentissa **Load Scene On Play**. Kenttään **Play Scene Name** on jo valmiiksi kirjoitettu "MAIN SCENE".
3. Testaa: Play ja päävalikon Play-nappi. MAIN SCENE latautuu.
4. Muutos koskee vain tämän scenen kopiota (asetus näkyy lihavoituna). Jos haluat sen kaikkiin, valitse Inspectorin yläosasta **Overrides > Apply All**.

Ennen oikeaa buildia MAIN SCENE pitää lisätä Build Profilesin scene-listaan. Se sovitaan Oton kanssa (Claude log).

Muista lopuksi commitoida prefabit ja uudet kuvat ja fontit .meta-tiedostoineen.
