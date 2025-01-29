# x) Lue ja tiivistä

## Apache Name-based Virtual Host Support

# a) Apache2 -asennus

Päivitys $sudo apt-get update:lla. 
Apache 2 asensin jo tunnilla, komennoilla  

    $ sudo apt-get -y install apache2
    $ echo "Default"|sudo tee /var/www/html/index.html

Testasin, että default page toimii selaimessa

(kuva default-page)


# b. Loki

(kuva loki2901)

Ensimmäisenä lokimerkinnässä näkyy clientin IP-osoite 127.0.0.1. , ja sen jälkeen kyseisen tapahtumahetken aikaleima. "GET / HTTP/1.1" 304 247 on komento, jota palvelin lähtee suorittamaan. GET toiminto kertoo, että kyseessä on HTTP, ja 1.1 on protokollaversio. 
304 247



Seuraavaksi loin uuden Name Based Virtual Hostin, 

    $ sudoedit /etc/apache2/sites-available/juukelispuukelis.example.com.conf
    
... ja konfiguroin sille kuvan mukaiset asetukset.

(kuva conf-dns)

Enablasin sivun ja uudelleenkäynnistin virtuaalikoneen

    $ sudo a2ensite pyora.example.com
    $ sudo systemctl restart apache2

Seuraavaksi loin uudella etusivulla näytettävän tiedoston 

    $ mkdir -p /home/nooral/publicsites/juukelispuukelis.example.com/
    $ echo juukelispuukelis > /home/nooral/publicsites/juukelispuukelis.example.com/index.html

  Tämän jälkeen testasin toimivuutta esin terminaalissa

  (kuva juukelispuukelis-testing)

  sekä selaimessa. Selaimessa localhost -sivu näkyi edelleen, mutta sivu http://juukelispuukelis.example.com antoi seuraavan virheilmoituksen: 

  /kuva virhe-juukelis)

  Avasin 












  # Lähteet:

  Loggly, Linux Logging Basics: https://www.loggly.com/ultimate-guide/linux-logging-basics/
