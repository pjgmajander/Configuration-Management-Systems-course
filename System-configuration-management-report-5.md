# H5 System configuration management report - “Loppuhuipennus”

Tämä harjoitustyö on osa Haaga-Helia ammattikorkeakoulun kurssia ”Palvelinten Hallinta”. 
Tehtävänannot löydät kurssin opettajan verkkosivustolta: https://terokarvinen.com/palvelinten-hallinta/

## Ympäristö, jossa harjoitukset on toteutettu:

Harjoitus on toteutettu kannettavalla koneella.

Malli: `Asus ZenBook UX325_UM325UA`

Prosessori: `AMD64 Family 23 Model 104 Stepping 1 AuthenticAMD ~1801 Mhz`

Prosessoriarkkitehtuuri: `x86-64`

Käyttöjärjestelmä: `Microsoft Windows 11 Home`

Harjoitukset on toteutettu Vagrantin avulla luoduilla virtuaalikoneilla. Seuraavassa kohdassa tietoa Vagrantin asentamisesta ja virtuaalikoneiden konfiguroinnista sekä käynnistämisestä.

# Tehtävä a) Oma miniprojekti.
### "Tee oma miniprojektisi valmiiksi."

Projektini tarkoitus on automatisoida pelipalvelin. Kirjoitin usean YAML-konfiguraatiotiedoston, jotka suoritan SaltStackin avulla. 
Loin viisi moduulia, jotka orja-kone ajaa samanaikaisesti top.sls -tiedoston määrittelyn mukaisesti. 


## Ensimmäinen moduuli
#### APTupdate

Tämä moduuli käskee orja-koneen päivittää pakettien hallintajärjestelmän.

apt_update:
  cmd.run:
    - name: apt update

## Toinen moduuli
#### A2install

Tämä moduuli käskee orja-konetta asentamaan apache2:n ja käynnistämään sen.

install_apache:
  pkg.installed:
    - name: apache2

enable_apache:
  service.running:
    - name: apache2
    - enable: True
    - require:
      - pkg: install_apache

## Kolmas moduuli
#### A2CONF

Tämä moduuli käskee orjakoneen muokata apachen konfiguraatiotiedostoa, jotta viides moduuli mahdollistetaan. Muutos käyttää lähteenä käsin kirjoitettua konfiguraatiotiedostoa, joka on siirretty polkuun /srv/salt/files/apacheconf/. Konfiguraatiotiedostoon tehty seuraava muutos, joka koskee hakemistoa /var/www/: "AllowOverride All". Tämä mahdollistaa .htaccess -tiedoston käyttämisen kyseisessä hakemistossa. 

apache2_config:
  file.managed:
    - name: /etc/apache2/apache2.conf
    - source: salt://files/apacheconf/apache2.conf
    - user: root
    - group: root
    - mode: 644
    - require:
      - pkg: install_apache

apache2_service:
  service.running:
    - name: apache2
    - enable: True
    - require:
      - file: apache2_config


## Neljäs moduuli
#### UFW

Tämä moduuli avaa portteja ufw-palomuurissa tarpeen mukaan.

open_port_ssh:
  cmd.run:
    - name: ufw allow 22/tcp  # SSH
    - unless: ufw status | grep -q "22/tcp"
    - require:
      - cmd: enable_ufw

open_port_http:
  cmd.run:
    - name: ufw allow 80/tcp  # HTTP
    - unless: ufw status | grep -q "80/tcp"
    - require:
      - cmd: enable_ufw

open_port_https:
  cmd.run:
    - name: ufw allow 443/tcp # HTTPS
    - unless: ufw status | grep -q "443/tcp"
    - require:
      - cmd: enable_ufw

## Viides moduuli
#### NINDEX

Tämä moduuli vaihtaa apache2:n oletussivun ja uudelleenkäynnistää apachen, jotta muutos astuisi voimaan. Uuden sivun lähteeksi on määritelty kansion /files/html/nindex/ sisältö. Kansioon on lisätty .htaccess -tiedosto, joka määrittää mitä sivua käyttäjälle näytetään kun hän muodostaa yhteyden apache-palvelimeen. Ilman kolmannessa moduulissa tehtyä muutosta apachen konfiguraatiotiedostoon, .htaccess ei vaikuttaisi mitenkään. 

![kuva](https://github.com/user-attachments/assets/dc14819e-cf23-4c42-a3d0-8e13ac35b975)

![kuva](https://github.com/user-attachments/assets/0892ba54-6727-4658-87cb-653a0287aab8)

copy_html_files:
  file.recurse:
    - name: /var/www/html/
    - source: salt://files/html/nindex/

restart_apache:
  cmd.run:
    - name: systemctl restart apache2

## TOP.SLS

base:
  'epsilon':
    - APTupdate
    - A2install
    - A2conf
    - ufw
    - Nindex
