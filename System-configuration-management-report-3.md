# H3 System configuration management report - “Demoni”

Tämä harjoitustyö on osa Haaga-Helia ammattikorkeakoulun kurssia ”Palvelinten Hallinta”. 
Tehtävänannot löydät kurssin opettajan verkkosivustolta: https://terokarvinen.com/palvelinten-hallinta/

## Ympäristö, jossa harjoitukset on toteutettu:

Harjoitus on toteutettu kannettavalla koneella.

Malli: `Asus ZenBook UX325_UM325UA`

Prosessori: `AMD64 Family 23 Model 104 Stepping 1 AuthenticAMD ~1801 Mhz`

Prosessoriarkkitehtuuri: `x86-64`

Käyttöjärjestelmä: `Microsoft Windows 11 Home`

Harjoitukset on toteutettu Vagrantin avulla luoduilla virtuaalikoneilla. Seuraavassa kohdassa tietoa Vagrantin asentamisesta ja virtuaalikoneiden konfiguroinnista sekä käynnistämisestä.

# Tehtävä a) Apache easy mode
### "Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy."

Aloitettu asentamalla Apache paikallisesti MASTER-koneella.
Päivitetty ensin apt:

![kuva](https://github.com/user-attachments/assets/8263e805-7231-465a-abeb-0148b609e902)

Tämän jälkeen asennettu Apache:

![kuva](https://github.com/user-attachments/assets/2ae1bfb8-d214-4ccf-a01b-ec06929f8633)

Käynnistin Apachen ja tarkastelin oletussivua komentorivissä:

![kuva](https://github.com/user-attachments/assets/ff8d67f2-ba29-47b4-a49e-efde3f281549)

Etsin oletussivun tiedostojärjestelmästä:

![kuva](https://github.com/user-attachments/assets/456cc3e4-8bc9-423c-be0a-2b8aab78befa)

Poistin oletussivun ja korvasin sen uudella versiolla:

![kuva](https://github.com/user-attachments/assets/a64fc5ed-f72d-4783-a2e1-4b2b5b9a8525)

Avasin isäntäkoneella Vagrantfilen ja konfiguroin porttien uudelleenohjauksen:

![kuva](https://github.com/user-attachments/assets/95900b54-1592-4fae-9b16-c7a08fe133bb)

Käynnistin vagrantin uudestaan ja siirryin MASTER-koneelle. Tämän jälkeen asensin ufw:n ja käynnistin sen. Avasin juuri Vagrantfileen määrittelemäni portit, sekä SaltStackin edellyttämät portit 4505 ja 4506:

![kuva](https://github.com/user-attachments/assets/6e08327e-eddc-4c58-b9f6-c4147dfb8776)

Loin hakemistpolkuun /srv/salt/ uuden kansion nimellä "ufw" ja kirjoitin siihen init.sls -tiedoston, joka automatisoi juuri paikallisesti laatimani ufw-konfiguraation:

![kuva](https://github.com/user-attachments/assets/44ff3dc6-a098-4f56-a942-c1a06922fc57)

Käskin SLAVE-koneen suorittaa juuri kirjoittamani tiedoston komennolla sudo salt epsilon state.apply ufw:

![kuva](https://github.com/user-attachments/assets/c4f8cc3e-3e93-4998-8637-21548364bb25)

Uudelleenkäynnistin myös salt-minionin:

![kuva](https://github.com/user-attachments/assets/730a42a1-0918-4057-8fb8-2ca638bc3994)

Seuraavaksi loin /srv/salt/ hakemistoon kansion "apache", johon kirjoitin jälleen uuden .sls -tiedoston:

![kuva](https://github.com/user-attachments/assets/074fe856-b4b9-45c7-8b58-1d05f793df8e)

Käskin SLAVE-koneen suorittaa juuri kirjoittamani tiedoston sudo salt epsilon state.apply apache:

![kuva](https://github.com/user-attachments/assets/020ed4a3-8fbf-4f77-bafa-1d2406856d6b)

Loin uuden hakemistopolun /srv/salt/files/html

![kuva](https://github.com/user-attachments/assets/746314a3-a96a-48a4-8c0e-d54cbf4c7955)

Lisäsin uuden html-tiedoston:

![kuva](https://github.com/user-attachments/assets/b5e15890-7717-4c79-be16-54343323d32f)

![kuva](https://github.com/user-attachments/assets/2cdc7ca6-73f4-45c5-aea4-3dcc26a8d5ff)

Loin jälleen uuden konfiguraatiomoduulin /srv/salt/nindex, johon lisäsin .sls -tiedoston:

![kuva](https://github.com/user-attachments/assets/e66db1f4-e499-437e-8904-df420c0d7b9b)

Komennettu SLAVE-kone suorittamaan nindex-moduuli:

![kuva](https://github.com/user-attachments/assets/62bce508-e7dc-4fc4-b2a4-dd56bfcde330)

Nyt oli aika tarkistaa, että toimiko suunnitelmani. Siirryin isäntäkoneelle ja yhdistin Firefox-selaimella vagrantfileen määrittelemieni uudelleenohjattavien porttien kautta vagrant-koneiden apache-demoneille. Lopputulos oli toivottu, pystyin tarkastelemaan molempien eri virtuaalikoneiden demonien tarjoilemia html-tiedostoja:

![kuva](https://github.com/user-attachments/assets/a09c9aa7-634e-4a87-98d8-911ebf65a6bb)


# Tehtävä b) Linux Vagrant
### "Lisää uusi portti, jossa SSHd kuuntelee."


# Tehtävä c) Oma moduli
### Valitse aihe omalle modulille.


# Tehtävä d) VirtualHost
### "Asenna Apache tarjoilemaan weppisivua. Weppisivun tulee näkyä palvelimen etusivulla (localhost). HTML:n tulee olla jonkun käyttäjän kotihakemistossa, ja olla muokattavissa normaalin käyttäjän oikeuksin, ilman sudoa."

# Tehtävä x)
### Tiivistelmä Tero Karvisen materiaaleista

### Tiivistelmä SaltStackin materiaaleista

## Lähteet

Karvinen, T. 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port, luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh

Salt Contributors. Salt states, luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html
