# Content security policy lisääminen sivustolle santerikarttunen.com 2.3.2025

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

### apaxhe ei lue .htacces tiedostoa ja ratkaisu

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

Tämän jälkeen testasin `curl -I https://santerikarttunen.com` jonka pitäisi näyttää merkkejä että .htaccess tiedosto luetaan ja ehdot toteutuva. Näyttää kuitenkin siltä ettei .htaccess tiedostoa lueta. Syynä on todennäköisesti se, että AllowOverride on asetettu None palvelimen .conf-tiedostossa. En löytänyt tähän rakaisua, joka ei sisällä apache.conf tiedoston muuttamista, joten muokkasin sivun omaa .conf tiedostoa.

## HUOMIO 

Olen ottanut sertifikaatin sivulle käyttäen certbottia, joka on luonut oman tiedoston, joten conf, joka on käytössä ei ole aikaisemmin itse luomani .conf vaan mallia -le-ssl.conf'.  Tässä vaiheessa tein varmuuskopiot molemmista tiedostoista ennen muutosten tekemistä

Kun muutokset oli tehty sivun konfiguraatio tiedosto näytti tältä

<img width="608" alt="image" src="https://github.com/user-attachments/assets/8ad7c66d-fa4c-4342-841c-e6552ff3c9cb" />

Tämän jälkeen annoin komennon `sudo a2enmod headers` joka ottaa apachelle headerit käyttöön ja potkaisin palvelimen uudelleen käyntiin `sudo systemctl restart apache2`

## Toimivuuden tarkistaminen

Tarkistin että headerit ovat tulleet sivulle komennolla `curl -I https://santerikarttunen.com`

<img width="605" alt="image" src="https://github.com/user-attachments/assets/7a42b22a-c59a-4f51-833e-20866575903d" />

Ja skannasin sivun uudelleen https://developer.mozilla.org/en-US/observatory kautaa.

<img width="609" alt="image" src="https://github.com/user-attachments/assets/3b3ededa-a28c-47db-acb4-e940ed58e8cf" />

Lopuksi kävin vielä poistamassa turhan .htacces tiedoston sivun kansioista komennolla `rm "laita tähän tiedostopolku"` varmistin että tiedosto oli poistunut komennolla `ls -la "laita tähän tiedostopolku"` .htaccess ei vaikuta sivun toimintaan koska asetukset on määritelty aikaisemmin mainitussa sivun konfiguraatio tiedostossa. Otin myös aikaisemmin tekemäni .conf tiedoston pois päältä komennolla `sudo a2dissite santerikarttunen.com.conf` jolloin pelkkä santerikarttunen.com-le-ssl.conf jäi päälle.




## Lähteet: 

https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
Työn tukena on käytetty ChatGPT4.0 kielimallia
