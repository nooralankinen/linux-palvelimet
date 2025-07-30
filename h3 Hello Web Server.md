# x) Lue ja tiivistä

## Apache Name-based Virtual Host Support

- Name-based hosting perustuu asiakkaan pyytämään HTTP -osoitteeseen, ei IP-osoitteeseen.
  
- mahdollistaa sen, että useat verkkosivustot voivat käyttää samaa IP-osoitetta.
  
- myös name-based hosting aloitetaan ensin IP-resoluutiolla, jossa määritetään parhaiten pyyntöä vastaava virtuaalihost.
 
- Käyttöönotto: Määritä ensin Virtual Host jokaiselle käyttäjälle, jota haluat palvella. Virtual Host -blokin sisään määritetään vähintäänkin server name, sekä document root, josta sivun tiedot haetaan.
  
- Samaan konfigurointitiedostoon voidaan lisätä useampi domain.
  
- ServerAlias -määrittelyllä voidaan lisätä vaihtoehtoisia kirjoitustapoja osoitteelle.

  

## Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address

-Asenna ja konfiguro web serveri: $ sudo apt-get -y install apache2
$ echo "Default"|sudo tee /var/www/html/index.html

-lisää uusi virtual host: $ sudoedit /etc/apache2/sites-available/pyora.example.com.conf
<VirtualHost *:80>
 ServerName pyora.example.com
 ServerAlias www.pyora.example.com
 DocumentRoot /home/xubuntu/publicsites/pyora.example.com
 <Directory /home/xubuntu/publicsites/pyora.example.com>
   Require all granted
 </Directory>
</VirtualHost>

-ota uusi sivu käyttöön: $ sudo a2ensite pyora.example.com

-restarttaa Apache

-Luo webbisivun root -tiedosto ja sen sisältö:
$ mkdir -p /home/xubuntu/publicsites/pyora.example.com/
$ echo pyora > /home/xubuntu/publicsites/pyora.example.com/index.html

-Testaa: 
$ curl -H 'Host: pyora.example.com' localhost
$ curl localhost



# a) Apache2 -asennus

Fyysinen kone: Suoritin 11th Gen Intel(R) Core(TM) i5-11300H @ 3.10GHz 3.11 GHz
Asennettu RAM 8,00 Gt (7,70 Gt käytettävissä) 
Käyttöjärjestelmä Windows 11 Home, 23H2 Näytönohjain: Intel Iris Xe Graphics

Oracle Virtualbox Version 7.1.4 r165100 (Qt6.5.3)
Debian Live 12.9.0, amd64-xfce

Päivitys $sudo apt-get update:lla. 
Apache 2 asensin jo tunnilla 28.1 klo 17.40 alkaen, komennoilla  

    $ sudo apt-get -y install apache2
    $ echo "Default"|sudo tee /var/www/html/index.html

Testasin, että default page toimii selaimessa

![Alt](images/images/Default-page.png)



# b. Loki

Tein tämän osion tehtävästä 29.1 klo 12.40 

![Alt](images/images/loki2901.png)


Ensimmäisenä lokimerkinnässä näkyy nk. "loopback" IP-osoite 127.0.0.1., koska pyyntö tehtiin samalta koneelta. Sitä seuraa kyseisen tapahtumahetken aikaleima. "GET /favicon.ico HTTP/1.1"  on komento, jota palvelin lähtee suorittamaan. GET toiminto kertoo, että kyseessä on HTTP ja 1.1 on protokollaversio, favicon.ico on sivu jota pyydettiin. 404 on HTTP statuskoodi (tiedostoa ei löydy), ja 487 on vastauksen koko bitteinä. Pyyntö tuli sivulta http://localhost, ja Linux x86_64 koneelta jolla käytössä Firefox 128.0 -selain. 


# c) Etusivu uusiksi

Seuraavaksi loin uuden Name Based Virtual Hostin, 

    $ sudoedit /etc/apache2/sites-available/hattu.example.com.conf
    
... ja konfiguroin sille kuvan mukaiset asetukset.

![Alt](images/images/conf-dns.png)


Enablasin sivun ja uudelleenkäynnistin virtuaalikoneen

    $ sudo a2ensite hattu.example.com
    $ sudo systemctl restart apache2

Seuraavaksi loin uudella etusivulla näytettävän tiedoston 

    $ mkdir -p /home/nooral/publicsites/hattu.example.com/
    $ echo hattu > /home/nooral/publicsites/hattu.example.com/index.html

  Tämän jälkeen testasin toimivuutta esin terminaalissa

  ![Alt](images/images/hattu-testing.png)


  sekä selaimessa. Selaimessa localhost -sivu näkyi edelleen. 

  # e) Validi HTML5 sivu

  Seuraavaksi hain sivun index.html -tiedoston, jotta pääsin muokkaamaan sivun sisältöä. 

          $ nano /home/xubuntu/publicsites/pyora.example.com/index.html


Ja sisältö on seuraava:

    <!doctype html>
    <html lang="fi">
    <head>
    <title>hattu.example.com</title>
    <meta charset="utf-8">
    </head>
    <body>
    <h1>Nooran oma sivu</h1>
    <p>Tervetuloa osoitteeseen hattu.example.com. Tämän sivun näkymisen eteen on vuodatettu lukuisia tunteja verta, hikeä ja kyyneleitä. >
    </body>
    </html>

Ja näkymä sivulla on: 

![Alt](images/images/hattu-final.png)



# f) Anna esimerkit 'curl -I' ja 'curl' -komennoista

cURL -toiminto hakee dataa verkosta serverin kautta.
Ensin hain tietoja komennolla '$ culr localhost'

![Alt](images/images/curl-localhost.png)


komento 'curl -I (verkkosivu)' puolestaan näyttää haetun sivun serveriltä vastauksena tullutta sivun metadataa, kuten vaikkapa statuskoodin, sisällön tyypin, välimuistin ja evästeet. 
Ensiksi tein haun osoitteella www.haaga-helia.fi:

![Alt](images/images/curl-I1.png)


Ja tiedoista näemme, että yhteytenä käytetään HTTP -protokollan versiota 1.1. Statuskoodi on 301, eli sivu on siirtynyt pysyvästi. URL uudelleenohjaa sivun oikeaan osoitteeseen, mutta pelkkä cURL -toiminto ei pysty automaattisesti seuraamaan tätä uudelleenohjausta. 
Seuraavaksi haku osoitteella www.hs.fi:

![Alt](images/images/curl-I2.png)


Ja itseasiassa tämän sivun kanssa on melko sama alku, eli tämäkin sivu oodelleenohautuu, eikä cURL -komento ole pystynyt seuraamaan sitä uuteen osoitteeseen. 


# m) Hanki GitHub Education -paketti.

En tiennyt tarkalleen, mitä kyseinen paketti sisältää, eli googlasin ensiksi kyseisen paketin. 
Hakutuloksissa heti toisena olikin GitHubin oma sivu, jossa asiaa esiteltiin (https://github.com/education). 
Etusivulta klikkasin kuvaketta 'Join GitHub Education', ja päädyin sivulle https://education.github.com/discount_requests/application .
Valitsin rooliksi 'Student', ja kävin sivulla olevan tarkistuslistan läpi. 
Lisäsin koulun sähköpostiosoitteen olemassaolevalle GitHub -profiililleni, jonka jälkeen osoite piti vielä vahvistaa sähköpostiin saapuneen viestin linkin kautta. 

# o) Vapaaehtoinen, kaksi web-sivua. 

Lähdin ensin luomaan root -tiedostoja verkkosivuille, ja niihin molempiin index.html -tekstitiedostot. 

       $ sudo mkdir -p /var/www/foo.example.com
       $ sudo mkdir -p /var/www/bar.example.com

Sen jälkeen loin uuden virtual host -määrityksen näille molemmille:

        $ sudoedit /etc/apache2/sites-available/foo.example.com.conf


![Alt](images/images/virtual-hosts.png)


Tämän jälkeen testasin komennolla curl -H 'Host: foo.example.com' localhost, mutta näkyviin tulivat vanhan sivun tiedot. Sen jälkeen menin tarkistamaan sites-enabled -tiedoston, ja siellä tosiaan oli vain vanhan sivun konfiguraatiotiedosto. 

![Alt](images/images/error-vhosts.png)


En tiedä, mitä aiemmin luoduille tiedostoille oli tapahtunut, mutta tein nyt vielä uudet komennoilla 

$ mkdir -p /home/nooral/publicsites/foo.example.com/
$ mkdir -p /home/nooral/publicsites/bar.example.com/

Mutta tämän jälkeen edelleenkin kansiossa näkyi vain aiemmin luomani hattu.example.com.conf. 
Menin seuraavaksi kansioon sites-available, ja siellä näkyi uusimmat konfiguraatiot. Yritin saada hattu.com -sivun pois päältä komennolla $ sudo a2dissite hattu.ecample.com.conf. 
Sain sen pois päältä, ja järjestelmä muistutti että se piti uudelleenkäynnistää. Tein sen. 
En muistanut, olinko jo ottanut uudet sivut käyttöön, joten tein sen nyt komennolla $ sudo a2ensite foo.example.com. 
Yrittäessäni käynnistää apachen uudelleen, tuli seuraava virheilmoitus: 

![Alt](images/images/error-toinen.png)


Menin katsomaan tuon viimeisimmän lokin tiedot, ja sieltä löytyi seuraavat: 

![Alt](images/images/viimeinen-virhe.png)


Näille en enää osannut tehdä mitään, joten luovutin tämän lisätehtävän kanssa. 









  # Lähteet:

  Apache, Name-based Virtual Host Support: https://httpd.apache.org/docs/2.4/vhosts/name-based.html

  Tero Karvinen, Linux Palvelimet 2025 alkukevät: https://terokarvinen.com/linux-palvelimet/

  Tero Karvinen, Name-Based Virtual Hosts On Apache - Multiple Websites to Single IP Address: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/

  Loggly, Linux Logging Basics: https://www.loggly.com/ultimate-guide/linux-logging-basics/

  Hostinger, What is the cURL command? Understanding the syntax, options, and examples: https://www.hostinger.com/tutorials/curl-command#:~:text=The%20cURL%20command%20lets%20you%20send%20or%20fetch,4%20DELETE%20%E2%80%93%20removes%20data%20from%20the%20endpoint.

  Signoz, Complete Guide To Apache Logs: https://signoz.io/guides/apache-log/

  Simplified Guide, How to show HTTP response headers in cURL: https://www.simplified.guide/curl/headers-response-show

  GitHub Education, https://github.com/education
