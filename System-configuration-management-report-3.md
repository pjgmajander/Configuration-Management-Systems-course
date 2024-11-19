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

Asennettu ensin manuaalisesti ssh MASTER-koneelle:

![kuva](https://github.com/user-attachments/assets/88d6d433-05a5-47ec-9f77-ad3c493e10cf)

Muokattu ssh-demonin konfiguraatiotiedostoa "sshd_config":

![kuva](https://github.com/user-attachments/assets/77c21622-a85d-41b1-bbe2-180b0d2eb142)

Lisätty portit 22 sekä 8888

![kuva](https://github.com/user-attachments/assets/40e5557c-97c9-43a1-8eed-1c87dacc858b)

Uudelleenkäynnistetty ssh ja kokeiltu kirjautua komennolla ssh

![kuva](https://github.com/user-attachments/assets/57303a9f-0d8b-49d8-9fa5-0818cfadabee)

Avattu portti 8888 ssh-yhteyksille:

![kuva](https://github.com/user-attachments/assets/f4fa5bd9-5634-42e7-a67d-66bf966e1377)

Pyritty kirjautumaan tuloksetta:

![kuva](https://github.com/user-attachments/assets/8bfeea10-01df-4e64-a585-1e490bdc866c)

Varmistettu kuitenkin, että portti kuuntelee. Saatu positiivinen tulos:

![kuva](https://github.com/user-attachments/assets/afd98e3b-63cc-4cac-9b08-ea2b84e3cf5e)

Siirrytty automatisoimaan edeltävä asetelma.
Ensin päivitetty sshd_config -tiedostoon uusi porttikonfiguraatio:

![kuva](https://github.com/user-attachments/assets/4db5a410-08da-454a-994d-33de6c4e39dd)

Avattu kyseinen portti SLAVE-koneella:

![kuva](https://github.com/user-attachments/assets/64a597bf-459a-482a-b3ac-568baf7d7902)

Luotu /srv/salt/files/sshdconf/ polku, ja kopioitu siihen juuri muokattu sshd_config -tiedosto:

´sudo cp /etc/ssh/sshd_config /srv/salt/files/sshdconf/´

Tämän jälkeen luotu uusi ssh-moduuli, ja lisätty siihen .sls -tiedosto:

![kuva](https://github.com/user-attachments/assets/b44bf761-2252-46f8-a13c-8e724e3ed280)

![kuva](https://github.com/user-attachments/assets/99b21eb8-fa13-43b9-b5d6-5dbc9fe45c00)

Suoritettu ssh-moduuli SLAVE-koneella Saltin avulla:

![kuva](https://github.com/user-attachments/assets/589824e6-a251-4b63-a003-69eb35bbf3c2)

Varmistettu, että portti aukesi kuuntelemaan ssh-yhteyksiä (+ havainnollistava esimerkki eli esim. portti 5558 ei ole auki automaattisesti):

![kuva](https://github.com/user-attachments/assets/b454ca0a-ccc1-4c99-b711-73c538cb3378)

# Tehtävä c) Oma moduli
### Valitse aihe omalle modulille.

Pyrin toteuttamaan variaation Cookie Clicker -pelistä web-palvelimella.

# Tehtävä d) VirtualHost
### "Asenna Apache tarjoilemaan weppisivua. Weppisivun tulee näkyä palvelimen etusivulla (localhost). HTML:n tulee olla jonkun käyttäjän kotihakemistossa, ja olla muokattavissa normaalin käyttäjän oikeuksin, ilman sudoa."

# Tehtävä x)
### Tiivistelmä Tero Karvisen materiaaleista

### Tiivistelmä SaltStackin materiaaleista

## Lähteet

Karvinen, T. 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port, luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh

Salt Contributors. Salt states, luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html

