# H2 System configuration management report - “Infra-As-Code”

Tämä harjoitustyö on osa Haaga-Helia ammattikorkeakoulun kurssia ”Palvelinten Hallinta”. 
Tehtävänannot löydät kurssin opettajan verkkosivustolta: https://terokarvinen.com/palvelinten-hallinta/

## Ympäristö, jossa harjoitukset on toteutettu:

Harjoitus on toteutettu kannettavalla koneella.

Malli: `Asus ZenBook UX325_UM325UA`

Prosessori: `AMD64 Family 23 Model 104 Stepping 1 AuthenticAMD ~1801 Mhz`

Prosessoriarkkitehtuuri: `x86-64`

Käyttöjärjestelmä: `Microsoft Windows 11 Home`

Harjoitukset on toteutettu Vagrantin avulla luoduilla virtuaalikoneilla. Seuraavassa kohdassa tietoa Vagrantin asentamisesta ja virtuaalikoneiden konfiguroinnista sekö käynnistämisestä.

# Tehtävä a)
##### ”Osoita jollain komennolla, että Vagrant on asennettu (esim tulostaa vagrantin versionumeron). Jos et ole vielä asentanut niitä, raportoi myös Vagrant ja VirtualBox asennukset. (Jos Vagrant ja VirtualBox on jo asennettu, niiden asennusta ei tarvitse tehdä eikä raportoida uudelleen.)”

![kuva](https://github.com/user-attachments/assets/ab71e721-e760-4cc0-a37a-da4f4233790d)

Asennettu isäntäkoneelle Vagrant. 
Asennuksen jälkeen uudelleenkäynnistetty kone

Tarkistettu vagrantin asentuminen:

![kuva](https://github.com/user-attachments/assets/b196e277-2927-4b0a-8cdb-d7d720c00b57)


# Tehtävä b)
##### "Linux Vagrant. Tee Vagrantilla uusi Linux-virtuaalikone."

Luotu työpöydälle kansio “VagrantProject1”

![kuva](https://github.com/user-attachments/assets/e9201dde-98f8-44cf-bf6b-963c152476c5)

luotu uusi virtuaalikone komennolla 'vagrant init Debian/bookworm64'

![kuva](https://github.com/user-attachments/assets/eeca5704-b262-4126-a4d9-4c767646084f)

Käynnistetty vagrant komennolla vagrant up

![kuva](https://github.com/user-attachments/assets/34aac0b9-5e31-4c8f-9dac-c8f994c28d84)

Yhdistetty koneeseen vagrant ssh ja suoritettu komento 'whoami'

![kuva](https://github.com/user-attachments/assets/44964ef6-6ab8-4537-a7a7-abc08b194ae9)

tarkistettu juuri asennetun koneen käyttöjärjestelmä komennolla 'cat'
![kuva](https://github.com/user-attachments/assets/bd80dfdb-cb02-4627-8bda-7ff54433932f)

kirjauduttu ulos virtuaalikoneelta ja pysäytetty vagrant

![kuva](https://github.com/user-attachments/assets/4eea8a1c-3db8-4fb3-aa42-d51f604475db)


# Tehtävä c)	
##### Kaksin kaunihimpi. Tee kahden Linux-tietokoneen verkko Vagrantilla. Osoita, että koneet voivat pingata toisiaan.

Avattu vagrantfile komentorivin kautta notepadiin

![kuva](https://github.com/user-attachments/assets/4122f37b-bc2f-4e24-b49a-31b92dc748b3)

Poistettu kaikki ylimääräinen teksti tiedostosta ja lisätty kaksi konetta staattisilla osotteilla. 
Selvitin vapaat staattiset osoitteet windows komentorivin ipconfig- ja ping -toimintojen avulla. 

![kuva](https://github.com/user-attachments/assets/0a860297-9f39-447e-9a4e-2cf463b7daf8)

Tämän jälkeen käynnistetty jälleen Vagrant

![kuva](https://github.com/user-attachments/assets/08be56e2-5588-417b-a9db-b4f122810b5e)

Kirjauduttu MASTERiin ja pingattu SLAVE

![kuva](https://github.com/user-attachments/assets/401dce72-6b09-4c82-99a6-f746a4794b15)

Huvin ja urheilun vuoksi kirjauduttu myös SLAVE ja pingattu MASTER

![kuva](https://github.com/user-attachments/assets/ba823647-be5f-466e-af46-a94803295ac4)


# Tehtävä d)	
##### "Herra-orja verkossa. Demonstroi Salt herra-orja arkkitehtuurin toimintaa kahden Linux-koneen verkossa, jonka teit Vagrantilla. Asenna toiselle koneelle salt-master, toiselle salt-minion. Laita orjan /etc/salt/minion -tiedostoon masterin osoite. Hyväksy avain ja osoita, että herra voi komentaa orjakonetta."

Aloitettu Saltin asennus 

![kuva](https://github.com/user-attachments/assets/b4b59128-1ad6-4e1c-94bf-f313db0ec4ab)

Odottamaton ongelma, ei saatu yhteyttä repo.saltproject.io, tarkistettu virtuaalikoneen yhteys internettiin pingaamalla google.com.
Tämän jälkeen kokeiltu isäntäkoneen selaimen kautta osoitetta, mutta yhteyttä ei saatu. 
Navigoitu salt.io -sivuille https://docs.saltproject.io/salt/install-guide/en/latest/ ja havaittu seuraava ilmoitus.

![kuva](https://github.com/user-attachments/assets/a9ff1e18-1789-42e8-8b45-0c7104ae6f15)

Toistettu komento uudella osoitteella, ja se toimi odotetusti. 

![kuva](https://github.com/user-attachments/assets/347e03b4-ac45-4ab7-b3bb-c0346d7e5a64)

![kuva](https://github.com/user-attachments/assets/d3515d16-16f7-441e-879a-94e2cb8de7e3)

Lisätty hakemistoon /etc/apt/preferences.d tiedosto nimeltä salt-pin-1001, joka sallii vain saltin long term supportin päivitykset. 
Komento rajoittaa Salt-pakettien päivitykset vain 3006.x LTS (long term support) -versioihin, 
estäen automaattiset päivitykset uusiin versioihin, jotka voisivat sisältää muutoksia tai uusia ominaisuuksia.

![kuva](https://github.com/user-attachments/assets/976020da-2410-462b-9f57-8225d2b88924)

Päivitetty apt ja asennettu salt-master

![kuva](https://github.com/user-attachments/assets/5d2c5db4-0d1e-4381-93c0-75bc58447806)

![kuva](https://github.com/user-attachments/assets/e1d975ac-1c56-4443-809d-dc130b8f1434)

Varmistettu vielä asentuminen:

![kuva](https://github.com/user-attachments/assets/cdf4c3be-4fa7-48d5-a016-9eb22322a29f)

Toistettu slavella samat asiat, mutta asennettu salt-masterin sijaan salt-minion.

![kuva](https://github.com/user-attachments/assets/641d67a0-d1f9-4535-80e6-ed8042a161ef)

Muokattu sudoedit /etc/salt/minion:

![kuva](https://github.com/user-attachments/assets/62e8ef48-1108-4821-afdb-31affaea61ed)

tarkistettu hyväksyntää odottavat avaimet

![kuva](https://github.com/user-attachments/assets/d7f90f1a-ce36-4ae6-9a38-a6e3ab4aba93)

hyväksytty avaimet

![kuva](https://github.com/user-attachments/assets/37d9e1bd-61f8-43cd-a6f3-e248bba4f02e)

tarkistettu toimivuus

![kuva](https://github.com/user-attachments/assets/464a32ac-a824-4df0-aa68-04c455377696)




# Tehtävä e)	
##### Hei infrakoodi! Kokeile paikallisesti (esim 'sudo salt-call --local') infraa koodina. Kirjota sls-tiedosto, joka tekee esimerkkitiedoston /tmp/ -kansioon.

Luotu tmp tiedosto

![kuva](https://github.com/user-attachments/assets/436ec30c-550b-4e6f-a26c-1cd08b0778a6)

sudo mkdir -p /srv/salt
cd /srv/salt
sudo mkdir trigger
cd trigger/
sudoedit init.sls

![kuva](https://github.com/user-attachments/assets/55c48a4d-0173-4087-808f-5c72d0f7938f)

# Tehtävä f) 
#### Aja esimerkki sls-tiedostosi verkon yli orjalla.

![kuva](https://github.com/user-attachments/assets/4212ed1a-956e-49e5-9a44-5c214ee0b867)

# Tehtävä g) 
#### Tee sls-tiedosto, joka käyttää vähintään kahta eri tilafunktiota näistä: package, file, service, user. Tarkista eri ohjelmalla, että lopputulos on oikea. Osoita useammalla ajolla, että sls-tiedostosi on idempotentti.

![kuva](https://github.com/user-attachments/assets/0de8323f-1cd5-47f7-b762-af28f7974e15)

vain 3 tilamuutosta, sillä Apache oli jo asennettu

![kuva](https://github.com/user-attachments/assets/d3885150-3c1f-4c7f-bf8a-de15f47f60c2)

Ajaessani komennon uudestaan 4 onnistui mutta vain 1 asia muuttui. 1 muuttunut asia oli komento hello world. Se on ainoa komento joka ei tallenna tietoa orjan konfiguraatioon. Tarkistin asian tulosteesta. Lukee, että apache2 service on jo käynnissä.  Tiedosto quux on jo olemassa ja apache2 on jo asennettu. Tämä osoittaa, että tilafunktiot 

"/tmp/quux:
  file.managed",
  
"install_apache:
  pkg.installed:
    - name: apache2"

    ja

"apache:
  service.running:
    - name: apache2
    - enable: true"
    
ovat idempotentteja.

![kuva](https://github.com/user-attachments/assets/2f01abfd-61eb-4299-b523-70c61647c60b)

![kuva](https://github.com/user-attachments/assets/1eb5c550-3c3c-46f9-b9bd-f0b260e31e58)



# Tehtävä h) 
#### Top file. Automatisoi vähintään kahden tilan / modulin ajaminen. Esim. komento 'sudo salt "*" state.apply' tai 'sudo salt-call --local state.apply' ajaa modulit "hello" ja "apache".

Muokattu kansion nimeä tehtävää varten (trigger -> hello)

![kuva](https://github.com/user-attachments/assets/7d264b7e-f327-4a2f-a9e4-216f9536d8fa

Luotu toinen kansio ”apache”  hakemistoon /srv/salt/

![kuva](https://github.com/user-attachments/assets/f143a1a1-b8bd-4355-9830-f18eab8c0779)

![kuva](https://github.com/user-attachments/assets/be57fcb8-ea8b-4394-8027-2bda0cceeae9)

![kuva](https://github.com/user-attachments/assets/65c8ac40-4f99-47c3-a7b9-f584e34ad0b0)

suoritettu top.sls ja molemmat modulit toimivat

![kuva](https://github.com/user-attachments/assets/1171d530-8631-4693-95ca-107920946528)

# Tehtävä x)
#### Tiivistelmä Tero Karvisen materiaaleista

-Vagrant luo virtuaalikoneita ja automatisoi SSH-kirjautumisen

-Ei graafista käyttöliittymää

-Tyypillinen käyttötarkoitus Saltille on useiden laitteiden hallinta verkon yli yhden laitteen avulla.

-Saltin avulla on mahdollista hallita tuhansia tietokoneita.

-Vain Master tarvitsee julkisen palvelimen ja tunnetun ip-osoitteen.

-Koneita voidaan hallita konfiguraatiotiedostoina (infraa koodina)

-top.sls -tiedoston avulla voidaan täsmentää, että mikä orja suorittaa minkäkin käskyn

-YAML eli Yet another markup language

-Käytössä Saltin konfiguraatiotiedoissa (.sls)

-Ei käytetä tabulaattoria vaan kahta välilyöntiä

-Perustuu avain-arvo -pareihin

-Tiedot voidaan tallentaa listoiksi ja sanakirjoiksi


## Lähteet

Karvinen, T 2021. Two Machine Virtual Network With Debian 11 Bullseye and Vagrant, luettavissa: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/
Karvinen, T. 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux, luettavissa: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/
Karvinen, T 2014. Hello Salt Infra-as-Code, luettavissa: https://terokarvinen.com/2024/hello-salt-infra-as-code/
Karvinen, T 2023. Salt Vagrant - automatically provision one master and two slaves, luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file
Salt Contributors. Salt overview, luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml