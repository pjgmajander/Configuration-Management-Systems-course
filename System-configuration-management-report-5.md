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



#### APTupdate

apt_update:
  cmd.run:
    - name: apt update

#### A2install

install_apache:
  pkg.installed:
    - name: apache2

enable_apache:
  service.running:
    - name: apache2
    - enable: True
    - require:
      - pkg: install_apache

#### UFW

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

#### A2CONF

`apache2_config:
  file.managed:
    - name: /etc/apache2/apache2.conf
    - source: salt://files/apacheconf/apache2.conf
    - user: root
    - group: root
    - mode: 644
    - require:
      - pkg: install_apache`


apache2_service:
  service.running:
    - name: apache2
    - enable: True
    - require:
      - file: apache2_config

#### NINDEX

copy_html_files:
  file.recurse:
    - name: /var/www/html/
    - source: salt://files/html/nindex/




