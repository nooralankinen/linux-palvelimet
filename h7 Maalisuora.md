# a) Kirjoita ja aja "Hei maailma" kolmella kielellä


(koneen tiedot)
Tehtävä aloitettu 6.3.2025 kotona klo 12.12.
Ensin avasin virtuaalikoneen, ja päivitin ohjelmat komennolla $sudo apt-get update. 
Kyetäkseni ajamaan eri ohjelmointikieliä, minun piti ensin asentaa ne Debianiin. Päätin testata alkuun tehtävän vinkkiosiossa annetun komennon, $ sudo apt-get install python3 gcc g++ openjdk-17-jdk golang-go ruby lua5.4.
Ja se näytti toimivan, koska asennus lähti pyörimään. 

(kuva asennus)

##Python

Asennuksen jälkeen kävin silmäilemässä Pythonin manuaalin läpi, koska se oli näistä ohjelmointikielistä minulle entuudestaan tuttu. Manuaalin saa terminaalissa näkyviin komennolla $ man python3.
Tämän jälkeen menin kirjoittamaan tiedostoa, joka sisältää näytettävän teksin "Hello world". Komento $ nano helloworld.py. 
Tiedoston sisältö:

(kuva python1)

Ja tämän jälkeen ajoin kyseisen tiedoston: 

(kuva python2)

Homma näytti toimivan kuten pitääkin. 

Seuraavaksi lähdin testaamaan samaa Bashillä. 

##Bash 

Aloitin taas tutustumalla ensiksi Bashin manuaaliin. 

/kuva bash1)

Äskeisen osion kaavaa toistaen, kirjoitin tiedoston helloworld.sh Nano Editorilla. 

(kuva bash2)

Tiedoston tallennuksen jälkeen ajoin tiedoston: 

(bash3)

Tämäkin toimi odotetusti, joten siirryin viimeisen kielen kimppuun. 

## C

C -kielestä minulla ei ole mitään aiempaa kokemusta, ja sen syntaksi näytti näinkin simppelille toiminnolle yllättävän monimutkaiselta. Kirjoitin kuitenkin Teron esimerkkiä (https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/)
noudattaen tiedoston helloworld.c, jonka sisältö näytti seuraavalta: 

kuva c1

Tämän jälkeen jännityksellä ajoin ensimmäisen ajokomennon, myös Teron ohjeen perusteella: 

kuva c2

No, se ei toiminut. Yritin avata C:n manuaalia komennolla $ man c, mutta sekään ei toiminut. Tutkiskelin hetken komennon rakennetta, ja päätin jättää siitä viimeisen 'helloworldc' -osion pois, ja katsoa mitä järjestelmä herjaa. Se kertoikin, että komennosta puuttuu nyt tiedoston nimi. 
Ajattelin, että koska tiedoston nimi oli pelkkä 'helloworld', testaan sillä tuota komentoa, eli jätän viimeisen c-kirjaimen pois tiedostonimestä. Ja sillä se näytti toimivan: 

kuva c3

Jostain syystä minulle jäi kuitenkin pieni epävarmuus siitä, lukiko ohjelma varmasti tämän helloworld.c -tiedoston, eikä esimerkiksi jotain muuta aiemmissa kohdissa luotua helloworld -tiedostoa, joten kävin vielä muokkaamassa helloworld.c -tiedoston tekstiä Nano editorilla, ja ajoin sen sitten uudestaan:

kuva c4

Eli kaikki näytti olevan kunnossa. 

# c) Uusi komento 

Tätä harjoiteltiin jo hieman tiistain oppitunnilla. Luin kuitenkin vielä läpi Teron ohjeen Shell Scriptin luomiseen (https://terokarvinen.com/2007/12/04/shell-scripting-4/). 
Sen jälkeen loin Nano editorilla tiedoston testikomento: $ nano testikomento, jonka sisältö oli seuraava:

kuva testikomento1

Kyseisen scriptin oli siis tarkoitus luoda uusi kansio nimeltä 'uusikansio', näyttää että kansio on luotu, vaihtaa kansion nimi 'roskakansio':ksi, näyttää että vaihto onnistui, jonka jälkeen poistaa kyseinen kansio, ja näyttää vielä lopuksi kansion sisältö.
Testasin scriptin komennolla $ bash testikomento :

kuva scripti2

Ja sehän ei toiminut, koska scriptin sisällä oli yksi komento väärin. Kävin korjaamassa rivin 'rm roskakansio' -> 'rm -r roskakansio'. 
Ja sitten tallennuksen jälkeen uudelleen scriptin testaus: 

kuva scripti3

Noh, ei se kaunis ole, mutta scripti periaatteessa kuitenkin toimii. Luettavuuden parantamiseksi rajasin kuvakaappauksessa eri ls -komentojen tulosteet omiksi laatikoikseen. Scriptin toimintaa hieman sekoitti jo edellisessä scriptin ajokomennossa luotu kansio 'roskakansio', jonka poisto kuitenkin epäonnistui, ja se näkyyedelleen ensimmäisen ajetun 'ls' -komennon jälkeen kansioiden joukossa. 
Seuraava komento 'mv uusikansio roskakansio' kuitenkin siirtää uusikansio:n tämän roskakansion sisään, ja lopulta poistaa roskakansion. Eli lopputulos on kuitenkin oikein, koska viimeisessä ls -tulosteessa ei näy enää uusikansio:ta tai roskakansio:ta. 
Kyseinen scripti toimii tällä hetkellä ollessamme kansiossa /home/nooral. Todentaakseni tämän siirryin toiseen kansioon, ja yritin ajaa komennon '$ bash testikomento', siinä kuitenkaan odotetusti onnistumatta: 

kuva scripti4

Menin seuraavaksi tarkastelemaan kansion käyttöoikeuksia. 

(kuva oikeudet)

Siellä näyttikin jo kaikilla olevan oikeus ajaa kyseinen tiedosto, joten sitä ei tarvinnut erikseen lisätä. Testasin vielä asian ajamalla '$ /home/nooral/testikomento':

kuva scripti5

Tehtävänannon mukaisesti, myös muille käyttäjille tuli antaa mahdollisuus ajaa kyseinen komento. 
Eli sitä varten, kyseinen scriptikansio tulee kopioida käyttäjän omasta kotihakemistosta kohteeseen /usr/local/bin, josta se on myös muiden käyttäjien saatavilla. 
Suoritetaan kopiointi komennolla '$ sudo cp testikomento /usr/local/bin/'. 
Tämä toiminto mahdollistaa myös komennon käytön ilman kansiopolkua tai bash -komentoa. Siksi testaamme siirron onnistumisen yksinkertaisesti komennolla '$ testikomento':

kuva scripti6

Ja sehän toimi sekin. 

Päätin tähän osioon klo 14.33.


## d) Ratkaise vanha arvioitava laboratorioharjoitus

Valitsin tehtäväksi edellisen vuoden kurssin lopputehtävän (https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/)












## Lähteet

Tero Karvinen, Arvioitava laboratorioharjoitus 2024: https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/
