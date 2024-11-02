# H1 System configuration management report - “Viisikko”

Tämä harjoitustyö on osa Haaga-Helia ammattikorkeakoulun kurssia ”Palvelinten Hallinta”. 
Tehtävänannot löydät kurssin opettajan verkkosivustolta: https://terokarvinen.com/palvelinten-hallinta/

## Mikä ihmeen Salt?

Salt Project on pythonilla toteutettu avoimen lähdekoodin konfiguraationhallintajärjestelmä, joka mahdollistaa 
komentojen ja kyselyjen suorittamisen etänä. Sen avulla voidaan automatisoida yleisiä tietotekniseen infrastruktuuriin 
kuuluvia ylläpitotehtäviä, kuten käyttöjärjestelmien ja ohjelmistojen asennuksia, konfigurointia ja hallinnointia. 
Myös palvelinten, virtuaalikoneiden, konttien, tietokantojen, web-palvelimien ja erilaisten verkkolaitteiden hallinnointi on mahdollista Saltin avulla. 
(VMware, inc. 2024).

## Ympäristö, jossa harjoitukset on toteutettu:

Harjoitus on toteutettu kannettavalla koneella.

Malli: `Asus ZenBook UX325_UM325UA`

Prosessori: `AMD64 Family 23 Model 104 Stepping 1 AuthenticAMD ~1801 Mhz`

Prosessoriarkkitehtuuri: `x86-64`

Käyttöjärjestelmä: `Microsoft Windows 11 Home`

Asensin Debian 12.7.0 jakelun virtuaalikoneelleni Oracle VM VirtualBox managerissa. 
Annoin virtuaalikoneelleni 4 prosessoriydintä ja 4 096Mb RAM-muistia. 
Dynaamisesti allokoitavaa levytilaa virtuaalikone sai 10,45Gb. 

Annoin käyttäjälleni pääkäyttäjän oikeudet kirjautumalla ensin pääkäyttäjäksi.

![kuva](https://github.com/user-attachments/assets/e63a9a89-a0aa-4e46-8a1e-44e540c5fe10)
 
Seuraavaksi muokkasin hakemistossa /etc sijaitsevaa tiedostoa ”sudoers”. Tämä onnistuu näppärästi komennolla ~#sudo visudo. 
Lisäsin tiedostoon käyttäjänimeni ja kopioin perään pääkäyttäjää koskevat oikeudet ”ALL=(ALL:ALL) ALL”.

![kuva](https://github.com/user-attachments/assets/00c0e0ef-1b01-42cc-97fe-f5bb951664c3)

Asensin UFW-palomuurin komennolla: 
`sudo apt update && sudo apt install -y ufw`

ja avasin portit 4505 sekä 4506 Salt Stackia varten komennolla: 
`sudo ufw allow 4505/tcp && sudo ufw allow 4506/tcp`

![kuva](https://github.com/user-attachments/assets/67e815d6-7a96-4803-b4bd-6a3c59311286)

![kuva](https://github.com/user-attachments/assets/361e4693-1434-4eaf-9d21-8203a7e1b16f)

![kuva](https://github.com/user-attachments/assets/eb68947d-5325-45d3-a692-1f1ac8d99a34)

Asennettu myös curl komennolla `sudo apt install curl`

![kuva](https://github.com/user-attachments/assets/e0caede4-812b-4ae2-aa4d-5d9e1ef7f00d)

Käyttääkseni Saltia, asensin myös uuden pakettivaraston toistamalla nämä komennot virtuaalikoneeni komentorivillä: 
1.	`$ sudo mkdir -p /etc/apt/keyrings`
2.	`$ sudo curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring-2023.gpg https://repo.saltproject.io/salt/py3/debian/12/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg`
3.	`$ echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.gpg arch=amd64] https://repo.saltproject.io/salt/py3/debian/12/amd64/latest bookworm main" | sudo tee /etc/apt/sources.list.d/salt.list`

Yllä olevien komentojen lyhytmuotoinen selitys:
1.	Komento luo root-oikeuksilla “keyrings” nimisen hakemiston polkuun root/etc/apt. Etc-hakemisto sisältää järjestelmän konfiguraatiotiedostot, ja sen apt-alihakemisto perustuu Debian-pohjaisten Linux jakeluiden pakettienhallintajärjestelmään (APT). Toisin sanoen /etc/apt sisältää APT:n konfigurointitiedostot.
2.	Komento lataa tiedoston Salt Projectin GPG-avaintiedoston ja tallentaa sen edeltävässä komennossa luotuun hakemistoon nimellä ”salt-archive-keyring-2023.gpg”. 
3.	komento lisää uuden ohjelmistolähteen Salt Projectin paketeille APT:n lähdeluetteloon (/etc/apt/sources.list.d/salt.list), mikä mahdollistaa Salt Projectin ohjelmistojen asentamisen ja päivittämisen osaksi järjestelmää.

# Tehtävä X) 
##### Tiivistelmä Tero Karvisen materiaaleista.

-Salt-komentoja voidaan suorittaa paikallisesti ilman Master-Minion -toteutusta.

-Salt-funktiot toimivat samalla tavalla sekä Windowsissa, että Linuxissa.

-Tärkeitä mekanismeja ovat ’pkg’, ’file’, ’service’, ’user’ & ’cmd’.

-Tyypillinen käyttötarkoitus Saltille on useiden laitteiden hallinta verkon yli yhden laitteen avulla.

-Saltin avulla on mahdollista hallita tuhansia tietokoneita.

-Vain Master tarvitsee julkisen palvelimen ja tunnetun ip-osoitteen.

-Hyvä harjoitusraportti on mahdollisimman täsmällinen, helppolukuinen ja toistettava. Se sisältää myös lähdeviittauksia tiedolle.

# Tehtävä a)
##### ”Asenna Debian 12-Bookworm virtuaalikoneeseen. (Poikkeuksellisesti tätä alakohtaa ei tarvitse raportoida, jos siinä ei ole mitään ongelmia…”

# Tehtävä b)
##### "Asenna Salt (salt-minion) Linuxille (uuteen virtuaalikoneeseesi)."

Aloitin asentamalla salt-masterin komennolla sudo apt-get -y install salt-master (Karvinen 2018)
![kuva](https://github.com/user-attachments/assets/9c89d8c1-3c84-4f83-b268-8b09c66add31)

Tämän jälkeen asensin salt-minionin vastaavalla tavalla sudo apt install salt-minion (Karvinen 2018)
![kuva](https://github.com/user-attachments/assets/15fd6788-7b6c-415b-aa90-327ebf5cfde1)

Varmistin Salt -asennusten onnistuneen komennolla sudo salt-call –version (Karvinen 2021)
![kuva](https://github.com/user-attachments/assets/7137c51a-dff7-4c39-b45c-344bfb955fdc)

# Tehtävä c)	
##### Viisi tärkeintä. Näytä Linuxissa esimerkit viidestä tärkeimmästä Saltin tilafunktiosta: pkg, file, service, user, cmd. Analysoi ja selitä tulokset.

Seuraavaksi kokeilin viittä erilaista toiminnallisuutta paikallisesti. Kaikkia komentoja yhdistävät osat ”salt-call” ja lippu ”- -local” mahdollistavat Saltin paikallisen suorittamisen. Suoritin komennot lähes täysin samalla tavalla kuin T. Karvisen materiaaleissa (Karvinen 2021).  

## 1.	PKG

Aloitin komennolla ”pkg”:
`$ sudo salt-call --local -l info state.single pkg.installed tree`
Tämä komento asentaa tree paketin.

![kuva](https://github.com/user-attachments/assets/c7174578-e2a8-4265-822e-64efa5eaf8bf)

`$ sudo salt-call --local -l info state.single pkg.removed tree`

Tämä komento kumosi edellisen. Tulosteen kohdassa ”Comment:” näkyy parhaiten merkittävä ero eli aiemmin asetettu paketti poistettiin.

![kuva](https://github.com/user-attachments/assets/396e2753-3f5e-4513-9666-82164edbd677)

Edellä olevat komennot voisi toteuttaa kaikilla minioneilla: `sudo salt '*' pkg.install tree` tai vain spesifisti määritetyillä minioneilla: `sudo salt 'alfa' pkg.remove tree.`

## 2.	FILE

Seuraava käsittelemäni komento oli ”file”:
`$ sudo salt-call --local -l info state.single file.managed /tmp/temporaryfile`
Tämä komento loi _“temporaryfile”_ -nimisen tiedoston tmp-hakemistoon. Tiedoston nimi `<temporaryfile>` on käyttäjän määrittelemä muuttuja.

![kuva](https://github.com/user-attachments/assets/3b1052fa-23be-4346-b0e5-a6a1d6fadfb8)

`$ sudo salt-call --local -l info state.single file.managed /tmp/temporaryfilewithcontent contents="NTesla > TEdison"` 
Tämä seuraava komento loi samaan hakemistoon toisen tiedoston nimellä _”temporaryfilewithcontent”_, ja lisäsi tiedostoon määrittelemäni sisällön ”NTesla > TEdison”. 
_(Huom. Tiedostoon lisäämäni sisältö ei liity Haaga-Heliaan tai Palvelinten Hallinta -kurssiin mitenkään, vaan se edustaa omia subjektiivisia näkemyksiäni historiallisten henkilöiden paremmuusjärjestyksestä.)_

![kuva](https://github.com/user-attachments/assets/cd183926-98b4-46c4-ad71-d5d24f3a243b)

Seuraavaksi tarkistin, että toimiko suorittamani komento odotetulla tavalla. Navigoin tieni /tmp -hakemistoon, ja tarkastin ”temporaryfilewithcontent” -tiedoston sisällön `cat` -komennolla.

![kuva](https://github.com/user-attachments/assets/8e426dad-dc67-4368-825f-f82296fef4be)

Tämän jälkeen kokeilin vielä ensimmäisen luomani tyhjän tiedoston poistamista komennolla:
`$ sudo salt-call --local -l info state.single file.absent /tmp/temporaryfile`
Oheisesta kuvakaappauksesta selviää, että komento onnistui. 

![kuva](https://github.com/user-attachments/assets/b92678b6-f0e8-451d-88a8-b6eb74f5187f)

## 3.	SERVICE

Seuraava kokeilemani komento oli “service”:
Service komennolla voidaan hallita demoneja. Esimerkissä apache2 web-palvelin.
`$ sudo salt-call --local -l info state.single service.running apache2 enable=True`

![kuva](https://github.com/user-attachments/assets/146510f5-9ad7-4e95-8a6f-2c50f87a4c86)

`$ sudo salt-call --local -l info state.single service.dead apache2 enable=False`

![kuva](https://github.com/user-attachments/assets/c759286b-b4fa-403b-a5de-842cbd70e16c)

Edeltävät komennot siis käynnistivät ja sammuttivat apache2 -palvelun.

## 4.	USER

Seuraava kokeilemani komento oli “user”:
`$ sudo salt-call --local -l info state.single user.present nikolatesla963`
Komento loi uuden käyttäjän nimellä _nikolatesla963._

![kuva](https://github.com/user-attachments/assets/12e9309f-62a9-454c-902d-04036e96f527)

Tämän jälkeen suoritin komennon, joka poisti juuri luomani käyttäjän.
$ sudo salt-call --local -l info state.single user.absent nikolatesla963

![kuva](https://github.com/user-attachments/assets/27bf2703-8754-4a4c-842a-b8c12279ea9a)

## 5.	CMD

Tämä oli viides ja viimeinen komento “cmd”:
`$ sudo salt-call --local -l info state.single cmd.run 'touch /tmp/temporaryitem' creates="/tmp/temporaryitem"`
Tämä komento antoi käskyn luoda /tmp -hakemistoon tiedosto `<temporaryitem>`. 
Komennon suoritus onnistui.

![kuva](https://github.com/user-attachments/assets/27994600-ab04-4f05-baef-5dbd3d57bbb3)

# Tehtävä d)	
##### "Idempotentti. Anna esimerkki idempotenssista. Aja 'salt-call --local' komentoja, analysoi tulokset, selitä miten idempotenssi ilmenee."

Useiden samanlaisten idempotenttien komentojen suorittaminen vaikuttaa palvelimen tilaan samalla tavalla, kuin ensimmäinen suoritettu komento. 
Toisin sanoen Idempotentti komento vaikuttaa palvelimen tilaan vain kerran, eikä toistoilla ole merkitystä. 
Matemaattisesti idempotenssi voidaan esittää konjunktiossa **P ∧ P ≡ P** ja disjunktiossa **P ∨ P ≡ P** (Kärkkäinen 2003).
Jos P vastaisi samaa asiaa kuin apache2-palvelimen asennus, tarkottaisi **P ∧ P ≡ P** sitä, että apache2-palvelimen asennus ja apache2-palvelimen asennus on sama kuin apache2-palvelimen asennus. 
Toisin sanoen peräkkäiset asennukset eivät muuta idempotentilla komennolla laitteen tilaa mitenkään. 
Vaikka apache2 asennettaisiin idempotentin komennon avulla 10 000 kertaa, olisi laitteella silti vain yksi apache2-palvelin.

Hyvä esimerkki tästä on tämän harjoitustyön kohdassa c) suorittamani viimeinen komento:
`$ sudo salt-call --local -l info state.single cmd.run 'touch /tmp/temporaryitem' creates="/tmp/temporaryitem"`
Kun komento suoritetaan uudestaan, näkyy komennon lokitiedoissa kommentti *”/tmp/temporaryitem exists”*. 
Tämä kertoo, että tiedosto on jo olemassa. Toisin sanoen komento on idempotentti, koska sen toistaminen ei muuta alkuperäistä tulosta. 
Uutta kopiota tiedostosta ei luotu. Tämä on toteutettu komennossa ehtolauseella `creates="/tmp/temporaryitem"`

![kuva](https://github.com/user-attachments/assets/c3f3104f-6159-47c3-9863-b7ae55a08eeb)

# Tehtävä e)	
##### Herra-orja. Kokeile herra-orja arkkitehtuuria niin, että herra ja orja ovat samalla koneella.

Tehtävä on käytännössä suoraa jatkoa tämän harjoitustyön tehtävälle b, jossa tarkoituksena oli asentaa salt-minion. 
Saadakseni herra-orja -arkkitehtuurin toimimaan, muokkasin ensin ensin minion -tiedostoa:
`sudoedit /etc/salt/minion`
Määrittelin tiedostoon Master -koneelle ip-osoitteen `127.0.0.1` sekä minion -koneen identiteetiksi *”alfa”*.

![kuva](https://github.com/user-attachments/assets/3cd5d87d-8db6-4acc-9b7a-afbf9caa5e51)

Tämän jälkeen uudelleenkäynnistin palvelun
`sydo systemctl restart salt.minion` 
ja tarkastelin hyväksyntää odottavat avaimet komennolla
`sudo salt-key -L`

![kuva](https://github.com/user-attachments/assets/20f6c870-cd17-41c4-844c-fdc3d26366c4)

Kuvakaappauksesta voi huomata, että hyväksyntää odotti yksi avain, jolla oli sama nimi kuin aiemmin konfiguroimani minionin id *”alfa”*. Hyväksyin avaimen komennolla
`sudo salt-key -A`
ja suoritin prosessin loppuun painamalla *”y”*.

![kuva](https://github.com/user-attachments/assets/0dcb443d-f62a-4311-8cf3-097927ece9f8)

Nyt yhteys Herran ja orjan välille oli syntynyt. Kokeilin välittömästi lähettää käskyn orjalle
`sudo salt ’*’ cmd.run ’hostname -I’` 
ja se todellakin toimi. "Alfa" -orja vastasi suorittamalla antamani käskyn.

![kuva](https://github.com/user-attachments/assets/d891e785-8063-4470-8970-a14c7868cf30)

## Lähteet
Karvinen, T. 2021. Run Salt Command Locally, luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/

Karvinen, T. 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux, luettavissa: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

Karvinen, T. 2006. Raportin kirjoittaminen, luettavissa: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/

Kärkkäinen, T. 2003. Jyväskylän yliopiston luentokalvot, Formaalit menetelmät – Propositiolaskennan kertausta, luettavissa: http://users.jyu.fi/~tka/opetus/kevat03/kalvo7.pdf. Luettu 30.10.2024. 

VMware, Inc. 2024. SaltStack Salt Documentation (Release 3007.01), s. 2-5, luettavissa: https://docs.saltproject.io/en/pdf/Salt-master.pdf. Luettu: 29.10.2024. 
