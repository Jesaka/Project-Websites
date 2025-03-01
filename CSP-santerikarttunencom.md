# Content security policy lisääminen sivustolle santerikarttunen.com 1.3.2025 10:45

Tässä tekstissä käsitellään sivuston https://santerikarttunen.com sivun tietoturvan parantamista ja CSP lisäämistä sivulle tietoturvan parnatamiseksi. Tavoitellussa loppuasetelmassa sivusto ilmoittaa HTTP observatory tarkastuksessa korkeamman pistemäärän sivun turvallisuudelle.

## Content Security Policy (CSP) – Mitä se tarkoittaa?

Content Security Policy (CSP) on HTTP-otsikko, joka auttaa estämään haitallisia hyökkäyksiä, kuten Cross-Site Scripting (XSS) ja data injection -hyökkäyksiä. CSP rajoittaa, mistä lähteistä sivusto saa ladata resursseja (esim. skriptejä, tyylejä, kuvia ja fontteja).
Ilman CSP:tä haitalliset skriptit voivat suorittaa koodia käyttäjän selaimessa, mikä voi johtaa tietovarkauksiin, käyttöoikeuksien väärinkäyttöön ja muuhun vahingolliseen toimintaan. (CHATGPT)

## Lähtötilanne 

Aloitin skannaamalla oman sivuni  https://developer.mozilla.org/en-US/observatory

CSP osalta tulos näytti tältä (D arvosana on sivun kokonaisarvosana, joka koostuu muistakin osa-alueista sivun turvallisuudessa)

<img width="610" alt="image" src="https://github.com/user-attachments/assets/05c9eb72-bf47-478b-9198-b06bed005eda" />

## CSP Asettaminen sivulle

CSP määriteltiin tässä lisäämällä Content-Security-Policy -otsikko palvelimen vastaukseen. Sen avulla voi sallia tai estää eri sisältölähteitä.

https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP

Aloitin kirjautumalla palvelimelleni ja loin sivun hakemistopolkuun (samaan mistä index.html löytyy .htaccess tiedoston komennolla `micro .htaccess`. Käytin tiedoston luomisessa apuna ChatGPT:tä joka tarjosi valmiin tiedoston.

<IfModule mod_headers.c>
Header set Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self'; img-src 'self'"
</IfModule>

Tämän jälkeen kysyin vielä, onko tiedostoon tarpeellista lisätä muita tietoturvaa parantavia asetuksia ja sain seuraavat ehdotukset.

- 1. HTTP Strict Transport Security (HSTS)
Varmistaa, että verkkosivustosi toimii vain HTTPS-yhteyden kautta.
- 2. X-Frame-Options (Clickjackingin esto)
Estää sivuston upottamisen iframe-elementtiin, mikä suojaa "clickjacking"-hyökkäyksiltä.
- 3. X-Content-Type-Options (MIME-sniffing esto)
Estää selaimia arvaamasta tiedostotyyppiä, mikä voi estää tiettyjä hyökkäyksiä.
- 4. 4. Referrer Policy (Tietovuotojen esto)
Määrittää, miten selaimen tulisi lähettää Referer-otsikkoa muille sivustoille.

Lisäykset kuulostivat fiksuila, joten lisäsin ne tiedostoon, loppulinen asetustiedosto näyttää tältä

<img width="800" alt="image" src="https://github.com/user-attachments/assets/b8bdd0a9-59cc-498a-9f20-5764538c53cb" />

Tekeminen keskeytetty 11:39 




## Lähteet: 

https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
