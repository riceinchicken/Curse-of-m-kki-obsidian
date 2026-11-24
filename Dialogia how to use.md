3. Käyttöohje Unity Editorissa

4. **Luo kansio:** Tee Project-ikkunaan kansio **Assets/Data/Dialogues/** (vastaavasti kuin esineet ovat kansiossa `Assets/Data/Items/`).
5. **Luo dialogiassetti:** Klikkaa kansiossa hiiren oikealla > **Create > Curse of Mokki > Dialogue**. Nimeä se kuvaavasti, esim. `DLG_CottageIntro` tai `DLG_ForestCamp`.
6. **Lisää repliikit (****Lines****):**
    - Paina **+** lisätäksesi uuden repliikin.
    - **Speaker Name:** Kirjoita puhujan nimi (esim. `"Kaveri"`) tai jätä tyhjäksi pelaajan omia ajatuksia varten.
    - **Text:** Kirjoita teksti. `TextArea`-kentässä voit käyttää normaaleja rivinvaihtoja (`Enter`).
    - **Duration:** Jätä arvoon **0**, jos haluat keston laskettavan automaattisesti joko `Voice Clip` -tiedoston pituudesta tai sanojen määrästä. Aseta manuaalinen sekuntimäärä vain, jos haluat yliajaa automatiikan.


**Ääninäyttelyn kytkeminen (tärkeä huomio** **AudioManager****-skriptistä):**

- Nykyinen `AudioManager` lataa `Assets/Audio/Resources`-kansion klipit ja **poistaa lopusta pelkät numerot** alt-versioiden arpomista varten (esim. `SFX_WalkSnow2` tallentuu avaimella `"SFX_WalkSnow"`).
- Jos siis nimeät äänirepliikin GDD:n mukaan esimerkiksi `SFX_VACottage1` ja `SFX_VACottage2`, `AudioManager` arpoo niistä sattumanvaraisesti toisen, kun kutsutaan `"SFX_VACottage"`.
- **Suositus yksittäisille repliikeille:** Nimeä repliikkitiedostot niin, ettei viimeinen merkki ole pelkkä numero ilman erottavaa kirjainta (esim. `SFX_VACottage_Intro_a` tai `SFX_VACottage_Line1a`), **tai** raahaa klippi suoraan `Voice Clip` -kenttään, jolloin `DialogueManager` osaa sekä soittaa juuri oikean repliikin että lukea sen tarkan keston suoraan klipistä.