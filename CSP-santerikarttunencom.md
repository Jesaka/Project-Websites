# Content security policy lisääminen sivustolle santerikarttunen.com 1.3.2025 10:45

Tässä tekstissä käsitellään sivuston https://santerikarttunen.com sivun tietoturvan parantamista ja CSP lisäämistä sivulle tietoturvan parnatamiseksi. Tavoitellussa loppuasetelmassa sivusto ilmoittaa HTTP observatory tarkastuksessa korkeamman pistemäärän sivun turvallisuudelle.

## Content Security Policy (CSP) – Mitä se tarkoittaa?

Content Security Policy (CSP) on HTTP-otsikko, joka auttaa estämään haitallisia hyökkäyksiä, kuten Cross-Site Scripting (XSS) ja data injection -hyökkäyksiä. CSP rajoittaa, mistä lähteistä sivusto saa ladata resursseja (esim. skriptejä, tyylejä, kuvia ja fontteja).
Ilman CSP:tä haitalliset skriptit voivat suorittaa koodia käyttäjän selaimessa, mikä voi johtaa tietovarkauksiin, käyttöoikeuksien väärinkäyttöön ja muuhun vahingolliseen toimintaan.

## Lähtötilanne 

Aloitin skannaamalla oman sivuni  https://developer.mozilla.org/en-US/observatory

CSP osalta tulos näytti tältä (D arvosana on sivun kokonaisarvosana, joka koostuu muistakin osa-alueista sivun turvallisuudessa)

<img width="610" alt="image" src="https://github.com/user-attachments/assets/05c9eb72-bf47-478b-9198-b06bed005eda" />
