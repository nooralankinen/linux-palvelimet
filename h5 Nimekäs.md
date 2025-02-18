# a) Nimi


tehtävä aloitettu 12.2 klo 8.30.
Lähdin hankkimaan domainia tunnilla demotulta namecheap.com -sivustolta. 
Etusivun hakupalkkiin kirjoitin haluamani domainin, 'lankinen.com'. Ja yllätyksekseni se oli vielä vapaana! Myös hinta oli opiskelijabudjetille varsin kohtuullinen, vaivaiset 11,362.40e. 
Kävin tässä välissä luomassa tilin palveluun, jotta pääsin suorittamaan ostostani loppuun.

(kuva domain-haku)

Tarkkaavaisuushäiriöisenä katsoin siis hinnan olevan vajaa 12e. Maksuvaiheessa kuitenkin tuli vastaan ongelma. Järjestelmä pyysi nimittäin tarkistamaan tilini saldon, sillä maksua ei jostain syystä oltu voitu sieltä veloittaa. Tässä vaiheessa keskityin katsomaan pilkkujen ja pisteen paikan hinnassa uudestaan.
Ja ihmekös tuo ettei maksu mennyt läpi, kun saldosta uupui noin kymppitonnin verran. Epäilin, että kyseessä täytyy olla jokin järjestelmän pilkkuvirhe, ja laitoin sivuston chattiin viestin hinnasta. Sieltä vastattiin, että hinta on ihan oikea. Kyseessä on premium -domain, jonka hinta tosiaan on tuo reilu 11 000e.
Asia harvinaisen selvä.

Mietin hetken, kannattaako tällaista kömmähdystä edes kirjoittaa tänne, mutta toisaalta tällaista on elämä adhd:n kanssa, joten antaa mennä. 

Palasin siis takaisin etusivulle, ja hain seuraavaksi domainin 'nooralankinen.com.' Sekin oli vapaana, ja hinnalla 6,46e / vuosi. Se kuulosti jo paljon paremmalta. 

(kuva domain-2)

(kuva speksit)

Domainin kylkeen tarjolla olisi ollut myös jos jonkinlaista palvelua, mutta päätin pysytellä pelkässä domainin hankinnassa.

(kuva ekstrat)

Maksoin ostokseni PayPalilla.

(kuva ostos)

Tämän jälkeen palasin taas etusivulle, ja lähdin etsimään paikkaa josta tehdä muutokset DNS -asetuksiin. Tämä siksi, että sivu olisi mahdollista löytää netistä domainin perusteella. 
Sivun yläreunassa, käyttäjätunnuksen vieressä olevasta nuolesta avautui valikko, ja sieltä kohta 'Dashboard'. Avautuvalta sivulta valittiin vielä sivun keskeltä ylävalikosta tuo 'Advanced DNS', ja sen jälkeen syötettiin tiedot klikkaamalla '+ Add new record' -painiketta.

(kuva dns)

Ensimmäisten tietojen tallennuksen jälkeen sivusto poisti automaattisesti siellä aiemmin olleet recordit. 
Tässä kohtaa tunnilla ohjeistettiin, että kannattaa hetki odottaa (n. 30min) ennen domainin testaamista, jotta tietueet ehtivät varmasti päivittyä. 

Odottelun jälkeen, menin selaimella osoitteeseen nooralankinen.com:

(kuva nooralankinen.com)


# b) Based

Tämä kohta oli jo toteutettu, koska sivuilla näkyi jo aiemmin luotu Name Based Virtual Host. (kts. kohta a.) Nimi)
Sivun index.html -tiedostoa muokkaamalla saadaan muutettua sivun sisältöä. 


# c) Kotisivu

Tätä kohtaa aloitin 12.2 klo 12.05
Siitä on jo tovi aikaa, kun olen viimeksi tehnyt kotisivuja, joten päätin kerrata asioita ensin hieman W3 Schoolsin avulla. Tarkoitus olisi jossain kohtaa rakentaa tälle ostamalleni domainille omat kotisivut, mutta tätä tehtävää varten loin kuitenkin vain pyydetyt rakenteet. 
Lähdin rakentamaan sivua Visual Studio Coden avulla, joka minulta löytyy jo valmiiksi koneelle asennettuna. Kopioin alkuun sivun koodiksi Teron (https://terokarvinen.com/2012/short-html5-page/) kirjoittaman simppelin html-koodin.

Jatkoin tehtävää 13.2 klo 10.50
Täydensin sivun koodia W3 Schoolsin ohjeiden perusteella. 
Kun olin saanut sivun rakenteet kasaan, avasin virtuaalikoneen ja loin komennolla '$ sudoedit /etc/apache2/sites-available/nooralankinen.com.conf' konfiguraatiotiedoston sivulle. 

(nooralankinen.com-conf)

Otin konfiguraation käyttöön ja restarttasin Apachen, jotta muutokset otetaan käyttöön. 
Seuraavaksi loin kansion '$ mkdir -p /home/noora/publicsites/nooralankinen.com/', ja '$ echo kukkuu > /home/noora/publicsites/nooralankinen.com/index.html'. 
Sitten testasin komennolla 'curl localhost':

/(kuva curl-localhost)

(curl-host)

Menin katsomaan, mitä error.log :ista näkyy. Ja viimeisimpänä näkyi seuraava viesti:

(errorlog)

Eli ongelma oli ilmeisesti jonkin polun kansion käyttöoikeuksissa. Tarkistin käyttöoikeudet:

(kuva kaytto-oikeudet)

Ja näytti, että kolmannella, eli 'muut'-ryhmällä ei näyttänyt olevan kuin read -oikeudet. Yritin muistella, mitä tunnilla mahdettiin sanoa käyttöoikeuksista, joita tarvitaan jotta sivut saadaan luettua. Googlesta löysin Linux Foundationin sivun (https://www.linuxfoundation.org/blog/blog/classic-sysadmin-understanding-linux-file-permissions), jolla asiaa selitettiin. Järkeilin sen perusteella, että käyttöoikeus execute tulisi olla myös others-ryhmällä. Päätin kuitenkin vielä kysyä asiaa ChatGPT:ltä, ja tilanteen selittämisen jälkeen se veikkasi ongelmaksi sitä, ettei Apachella ole käyttöoikeuksia kaikkiin polun kansioihin. Ongelman ratkaisuksi se käski antaa x-oikeuden hakemistopolun eri osiin seuraavilla komennoilla:

    $ sudo chmod +x /home
    $ sudo chmod +x /home/noora
    $ sudo chmod +x /home/noora/publicsites
    $ sudo chmod +x /home/noora/publicsites/nooralankinen.com

Tein työtä käskettyä. Tämän jälkeen päivitin selaimen, ja kas kummaa, sivu tuli näkyviin. Tässä kohtaa tosin harmitti, etten ollut käynyt itse tarkastamassa kaikki hakemistopolun kansioiden oikeuksia, koska olisin saattanut hoksata asian sieltä myös itse. Tai sitten en. Tavallaan olin oikeilla jäljillä, mutta aiempi päätelmäni ei kuitenkaan pitänyt täysin paikkaansa, koska ilmeisesti index.html -tiedostolle Apachelle riittää oikeus 'read', ja vika oli ylempänä hakemistopolussa. Loppu hyvin, kaikki hyvin kuitenkin. 
Loin vielä samaiseen nooralankinen.com -kansioon tiedostot 'art.html', 'contact.html' ja 'about.html', alasivuja varten, ja lisäsin sinne haluamani koodin. 

# d) Alidomain

Tätä tehtävän osaa aloin tekemään 18.2.2025 klo 11.40

Alidomainien luomiseksi menin ensimmäiseksi domainini palveluntarjoajan sivulle, https://www.namecheap.com/ ja kirjauduin sisään. Dashboard -näkymän kautta, josta aiemmassa tehtävässä lisättiin DNS -tietueet, mennään tällä kertaa lisäämään myös uudet alidomainit. 

kuva new-dns1

Tehtävänannon mukaisesti päätin tehdä ensimmäisen alidomainin A-tietueella. Hetken aikaa pohdin, kuinka tämä alidomain kuuluu määritellä, mutta koska kenttiä ei ollut paljon valittavana, päättelin, että 'host' kenttään tulee kirjoittaa haluttu alidomain. Heti en pysty toimivuutta testaamaan, joten tehdään se myöhemmin. 
Päätin tehdä vapaaehtoisen bonustehtävän, eli tehdä toisen alidomainin CNAME -tietueella. En kuitenkaan tunne tätä tietueasiaa kovin hyvin, niin päätin ensin hieman perehtyä, mistä näissä eri tietuetyypeissä on oikeastaan kyse. Lueskelin ensin NameCheapin oman artikkelin eri tietueista (https://www.namecheap.com/support/knowledgebase/article.aspx/579/2237/which-record-type-option-should-i-choose-for-the-information-im-about-to-enter/), ja vielä videon aiheesta YouTubesta (https://www.youtube.com/watch?v=ZXCQwdVgDno).
Tämän jälkeen, aloin tehdä toista alidomainia. Tietueeksi valitsin siis CNAME record, 'Host' kohtaan kirjoitin 'blogi' ja 'Value'-riville IP-osoitteen 94.234.37.216, TTL 5min. Tämän jälkeen klikkasin rivin hyväksytyksi. Sivusto kuitenkin antoi seuraavan virheilmoituksen:

kuva error-dns

Eli syöttämäni tiedot eivät olleet oikein. Luin vielä uudestaan aiempia ohjesivuja läpi. Niiden perusteella, jos olen ymmärtänyt asian oikein, CNAME record voi ainoastaan ohjata toiseen domainnimeen, ei IP-osoitteeseen. Eli vaihdoin nyt 'Value' -riville osoitteeksi linuxkurssi.nooralankinen.com. Sillä muutoksella, rivi hyväksyttiin. Tämän vuoksi, osoitteeseen mennessä tulee nyt ainakin yksi ylimääräinen DNS-kysely tuon linuxkurssi. mutkan vuoksi, ja sen välttämiseksi olisi ollut fiksumpaa laittaa osoitteeksi pelkkä nooralankinen.com. En tiedä, voiko siitä olla jotain muuta haittaa, että CNAME ohjaa nyt toiselle alidomainille. Ehkäpä asia selviää minulle kurssin edetessä.  

kuva dns-setup

# e) 'host' ja 'dig'

18.2.2025 klo 13.04.

Avasin tehtävää varten virtuaalikoneen, ja sieltä terminaalin. Ajattelin ensin tutkia ohjeen mukaisesti, mitä host ja dig -komennoilla tehdään. Jostain syystä manuaali ei kuitenkaan näyttänyt näistä mitään tietoja 

kuva man

Piti siis ensin asentaa kyseinen manuaali komennolla 

        $ sudo apt install man-db

Asennuksen jälkeen pääsin selaamaan manuaali. Aloitin ensin 'host' komennosta. 

kuva man-host

kuva man-dig

Luettuani manuaalit, menin katsomaan ensin, mitä tietoja molemmilla komennoilla on saatavilla osoitteesta nooralankinen.com

kuva nooralankinen.com-hostdig

Tarkastellaan ensin host-komennon tarjoamia tietoja DNS-tietueista. Tässä tietoja on huomattavasti vähemmän, mutta
ensimmäisellä rivillä lukee kyseisen domainin IP-osoite 94.237.37.216. 
Tämän jälkeen on tietoja sähköpostipalvelimista ja niiden prioriteeteista. 

Dig -komennolla tietoja onkin saatavilla jo enemmän. Oleellisimpia tässä kohtaa ovat kuitenkin QUESTION SECTION, ja ANSWER SECTION. Question section kertoo, mitä on kysytty. Eli tässä tapauksessa osoitetta nooralankinen.com. Ja Answer section taas, mitä palvelin on vastannut kyselyyn. Vastauksesta selviää, että kyseisen vastauksen TTL on 261. Asetuksissa säädimme TTL:n viiteen minuuttiin, eli vastaa asetettua. Vastauksessa kerrotaan myös, että kyseessä on A-tietue, jonka IP-osoite on 94.237.37.216. Lisäksi siitä selviää vastausaika (4 ms), nimipalvelimen IP (94.237.127.9) ja portti (53). DNS käyttää oletusasetuksena porttia 53, eli se on kunnossa, ja sivustolta whatsmyip.com haettaessa (https://www.whatismyip.com/ip/94.237.127.9/) sivu näyttää, että kyseinen IP-osoite kuuluu upcloudille, jossa virtuaaliserverini sijaitsee. Lopussa on vielä hakuajankohta, sekä vastauksen koko tavuina. 

kuva whatsmyip

Seuraavaksi hain domainin gilda.fi -tiedot samoilla komennoilla:

kuva gilda.fi

Tässä host- komento näyttää IPv4 -osoitteen lisäksi myös kyseisen domainin IPv6 -osoitteen. Dig -toiminto taas ei automaattisesti näytä sitä, mutta sekin on saatavissa näkyville komennolla 'dig gilda.fi AAAA'

kuva gildaAAAA

Kyselyn käsitellyt nimipalvelin on sama, koska haku tehtiin samasta osoitteesta. 

Viimeisenä päätin hakea google.com -tiedot. 

kuva google.com 

kuva google AAAA



##Lähteet

Clouds.net, Linux Host command, troubleshot your DNS: https://www.cloudns.net/blog/linux-host-command-troubleshot-dns/

Geeks for geeks, dig Command in Linux with Examples: https://www.geeksforgeeks.org/dig-command-in-linux-with-examples/

Namecheap: https://www.namecheap.com/ 

Namecheap, All Types of DNS Records Explained: https://www.namecheap.com/support/knowledgebase/article.aspx/10594/10/all-types-of-dns-records-explained/

Namecheap, Which record type option should I choose for the information I’m about to enter?: https://www.namecheap.com/support/knowledgebase/article.aspx/579/2237/which-record-type-option-should-i-choose-for-the-information-im-about-to-enter/

Tero Karvinen, Name Based Virtual Hosts on Apache– Multiple Websites to Single IP Address: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/

Tero Karvinen, Short HTML5 page: https://terokarvinen.com/2012/short-html5-page/

Tony Teaches Tech, What are CNAME records? (and how they compare to DNS A records): https://www.youtube.com/watch?v=ZXCQwdVgDno

W3 Schools, HTML Tutorial: https://www.w3schools.com/html/default.asp
