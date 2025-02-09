# h4 Maailma Kuulee

## x) Lue ja tiivistä

## Teoriasta käytäntöön pilvipalvelimen avulla, Susanna Lehto
- Pilvipalvelintarjoajia on useita, joista valita
- Hinta vaihtelee sen mukaan, mitä ominaisuuksia serveriltä halutaan.
- Datakeskus kannattaa valita mahdollisimman läheltä käyttäjiä, sekä Euroopan sisältä.

## First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS, Tero Karvinen
-kirjaudu sisään ssh:n avulla root:ina, muotoa 'ssh root@ip-osoite'.
-tee reiät palomuuriin porteille 80 ja 22.
-lisää itsesi käyttäjäksi, sekä sudo että admin -ryhmiin. 
-lukitse root -login
-päivitä ohjelmistot. 


## a) Vuokraa oma virtuaalipalvelin

Fyysinen kone: Suoritin 11th Gen Intel(R) Core(TM) i5-11300H @ 3.10GHz 3.11 GHz Asennettu RAM 8,00 Gt (7,70 Gt käytettävissä) Käyttöjärjestelmä Windows 11 Home, 23H2 Näytönohjain: Intel Iris Xe Graphics

Oracle Virtualbox Version 7.1.4 r165100 (Qt6.5.3) Debian Live 12.9.0, amd64-xfce 

Tehtävä aloitettu 7.2 klo 10.03, kotikoneella jonka tiedot näkyvät yllä.
Valitsin palveluntarjoajaksi virtuaalipalvelimelleni Upcloudin (https://upcloud.com/). 
Heidän sivulleen piti ihan alkuun rekisteröityä, ja luoda tunnukset. Tein tämän, jonka jälkeen aloin valita itselleni serveriä. Otin käyttöön heidän Free trial -kokeilujakson, mutta koska se oli voimassa vain kymmenen päivää, päädyin saman tien päivittämään varsinaiseen versioon. 
Vasemman reunan valikosta menin kohtaan 'Servers', jonka välilehdellä luki, ettei minulla vielä ole käytössäni yhtään serveriä. Klikkasin teksin alta violettia painiketta '+ Deploy Server'. 

(kuva upcloud-serverin-luonti)

Sen jälkeen pääsin tekemään ensimmäisiä valintoja serverilleni:

(kuva server-valinnat1)

Näiden jälkeen pitikin valita sisäänkirjautumismetodi, jolle ei tässä edes annettumuita vaihtoehtoja kuin SSH-Key. Se täytyy luoda virtuaalikoneella, joten avasin virtuaalikoneeni ja sieltä terminaalin. 
Päivitin alkuun ohjelmat tutulla komennolla $ sudo apt-get update. 
Debian 12 -käyttöjärjestelmässä ei automaattisesti ole OpenSSH:ta, joten asensin myös sen ja loin SSH Keyn komennoilla:

    $ sudo apt-get -y install openssh-client
    $ ssh keygen

(kuva sshkey-luonti)

Menin hakemaan public keyn hakemistopolusta /home/nooral/.ssh/id_rsa.pub, jonne se tallennettiin,
ja liitin sen virtuaaliserverin tietoihin. 
Tämän jälkeen sivu loi hetken aikaa serveriä minulle. 

(kuva serveria-luodaan)

Kun serveri oli valmis, sivu tarjosi myös ohjetta sen käyttöönottoon.

(serveri-yhdistysohje)


## b) Tee alkutoimet omalla virtuaalipalvelimellasi

Toimin tämän UpCloudin ohjeen mukaan, ja serveri otettiin käyttöön. Teron ohjeesta (https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/) poiketen, järjestelmä ei kuitenkaan jostain syystä pyytänyt luomaan salasanaa root -käyttäjälle. 
Yritin päätellä, voiko siitä olla jotain haittaa tässä. Toisaalta, oman käyttäjän lisäämisen jälkeen, root -tunnukset oli määrä lukita ulos joka tapauksessa, joten ajattelin, että ehkei tästä salasanan puuttumisesta synny suurta vahinkoa. 
Lisäsin itseni uudeksi käyttäjäksi, sekä sudo -ryhmään komennoilla 

  $sudo adduser noora 
  $sudo adduser noora sudo
  $sudo adduser noora adm

  (kuva adduser)

  Tämän jälkeen avasin uuden terminaalin testatakseni tunnuksieni toimivuutta. Tunnukset eivät kuitenkaan toimineet, vaan antoivat seuraavan virheen:

  (kuva permission-denied)

  Stackoverflow -sinulta löysin jonkinlaisen ohjeistuksen, jolla yrittää korjata tilanne Public Keyn lanssa (https://stackoverflow.com/questions/2643502/git-how-to-solve-permission-denied-publickey-error-when-using-git)
  Joten annoin komennoiksi 

      $ eval $(ssh-agent -s)
      $ ssh-add /home/nooral/.ssh/id_rsa.pub

  (kuva privatekey-ignored)

  Mutta se ei toiminut, koska private key oli luotu toiselle käyttäjälle.
  Kyselin neuvoa kurssikavereilta, ja sieltä sainkin vinkiksi siirtää ssh:n tiedoston tälle uudelle käyttäjälleni. Googlailin siihen ohjeet, ja löysinkin (https://askubuntu.com/questions/1218023/copying-ssh-key-from-root-to-another-user-on-same-machine). 
  Ohjeen perusteella sain kopioitua Authorized Key Filen tälle uudelle käyttäjälle, sekä siirrettyä tiedoston omistuksen. Sitten testasin kirjautumista uudelleen, ja nyt se toimi. 

  Seuraavaksi päivitin ohjelmat, ja lähdin luomaan reikiä porteille 22 ja 80 ssh:lle enne tulimuurin asennusta. Asensin ufw:n komennolla $sudo apt-get -y install ufw 
  Reiät puolestaan komennolla $sudo ufw allow 22/tcp, $sudo ufw allow 80/tcp ja $sudo ufw enable. Tarkistin vielä lopuksi toimivuuden komennolla $sudo ufw status verbose. 

  (kuva ufw-status)

  Sitten piti vielä lukita root -käyttäjä. Annoin komennon $ sudo usermod --lock root , jolla lukittiin salasanakirjautuminen (salasanaa tosin en ollut edes asettanut). Komennolla $ sudo rm /root/.ssh -R puolestaan estin kirjautumisen SSH-avaimella. 

  Lopuksi vielä softien päivitys komennoilla $ sudo apt-get update ja $ sudo apt-get upgrade. 


## c) Asenna weppipalvelin

Päätin asentaa weppipalvelimeksi tutun ja turvallisen Apache2:n. Asennus komennolla $ sudo apt-get -y install apache2. 
Ja tämän jälkeen tarkistus $ systemctl status apache2

(kuva apache-asennus)

Lähdin muokkaamaan näkyvää oletussivua. 
Tein sovelletusti Teron ohjeen mukaan (https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/), eli komennolla $ echo "Testisivu"|sudo tee /var/www/html/index.html. 

(kuva testisivu1)

Ja sivu näytti siltä miltä pitikin.


## d) Vapaaehtoinen:

Päätin luoda uuden Name Based Virtual Hostin nimeltä kissa.example.com. 

Noudatin samaa, edellisessä kohdassa mainittua Teron ohjetta, ja loin sivulle uuden konfiguraatiotiedoston komennolla '$ sudoedit /etc/apache2/sites-available/kissa.example.com.conf', tiedoston sisältö:
<VirtualHost *:80>
 ServerName kissa.example.com
 ServerAlias www.kissa.example.com
 DocumentRoot /home/nooral/publicsites/kissa.example.com
 <Directory /home/nooral/publicsites/kissa.example.com>
   Require all granted
 </Directory>
</VirtualHost>

ja otin vielä konfiguraation käyttöön. Sen jälkeen loin tekstitiedoston index.html polkuun,  /home/nooral/publicsites/kissa.example.com/, ja sinne sivulla näkyvän sisällön. 
Testasin sivujen toimivuuden ensin terminaalissa curl localhost, se oli ok. Sitten selaimessa, ja sekin ok. Puhelimella en ensin saanut sivua näkyviin, koska en tajunnut laittaa eteen http://, mutta kysyin neuvoa chatGPT:ltä, ja se ohjeisti tämän. Sen jälkeen sivut toimivat myös puhelimella. 

(kuva testisivu)


# Lähteet

Tero Karvinen,Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address:  https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/

