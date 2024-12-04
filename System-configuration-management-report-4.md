
# H4 System configuration management report - “Puolikas”

Tämä harjoitustyö on osa Haaga-Helia ammattikorkeakoulun kurssia ”Palvelinten Hallinta”. 
Tehtävänannot löydät kurssin opettajan verkkosivustolta: https://terokarvinen.com/palvelinten-hallinta/

## Ympäristö, jossa harjoitukset on toteutettu:

Harjoitus on toteutettu kannettavalla koneella.

Malli: `Asus ZenBook UX325_UM325UA`

Prosessori: `AMD64 Family 23 Model 104 Stepping 1 AuthenticAMD ~1801 Mhz`

Prosessoriarkkitehtuuri: `x86-64`

Käyttöjärjestelmä: `Microsoft Windows 11 Home`

Harjoitukset on toteutettu Vagrantin avulla luoduilla virtuaalikoneilla. Lisätietoa löydät aiemmista raporteistani.

# Tehtävä a) Puolikas
### "Tee ensimmäinen vedos omasta modulista. Tätä jatketaan vielä, eli valmista ei tarvitse tulla. Keskeneräisen modulin pohjalta saat vinkkejä haastaviin kohtiin ja oikeaan kehityssuuntaan. Lopullisen modulin tulee olla modernia keskitettyä hallintaa, eli idempotentti ja infraa koodina."

Projektini tarkoitus on automatisoida yksinkertaisen pelipalvelimen rakentaminen ja käynnistäminen. Tässä kohtaa keskityn varsinaisen pelin ohjelmointiin. Seuraavassa vaiheessa tulen kirjoittamaan usean YAML-konfiguraatiotiedoston, jotka suoritan SaltStackin avulla. Luon viisi moduulia, jotka orja-kone ajaa samanaikaisesti top.sls -tiedoston määrittelyn mukaisesti. Lopputuloksena on infrastruktuuria koodina, jossa vaihtamalla kansion "srv/salt/files/html/nindex/" sisältöä, voidaan käynnistää automaattisesti uusia variaatioita pelipalvelimesta. Varsinaisen lopputuloksen (ohjelmoimani pelin) esittelen vain kurssin jäsenille eksklusiivisessa live-demonstraatiossa.

