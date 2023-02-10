# RealInternet

## Lukuläksy
Lukuläksynä oli lukea ja tiivistää Tero Karvisen kirjoittama blogi nimeltä First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS (https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/)

- Käytä AINA vahvoja ja hyviä salasanoja
- Palveluntarjoajia on lukuisia
- VPS ja Domain nimi liiketoiminta on erittäin kilpailtua
- Opiskelijoille löytyy ilmainen ratkaisu pilvipalvelimia varten GitHub Education paketista
- Muista seuraavat asiat virtuaalipalvelinta luodessa: Palomuuri, Oma järjestelmänvalvojatili, sulje Root tili & päivitä ohjelmistot


## Ympäristö

Hyper-V kotikoneella (Host):

- CPU: i5-9600K
- RAM: 16Gb
- HDD: 120Gb
- Win 11 Pro x64

Virtuaalikoneen speksit:

- 2 x CPU
- 4 Gb RAM
- 50 Gb HDD
- Generation 2 (Hyper-V pyytää määrittelemään)

## Aloitus 
Tehtävän tarkoituksena oli jatkaa tunnilla käytyyn virtuaalipalvelimiin suunnattuja tehtäviä.
Aloitin tehtävät 7.2.2023 klo 17:02.

## a) Vuokraa oma virtuaalipalvelin haluamaltasi palveluntarjoajalta
Olin jo tunnin aikana rekisteröitynyt onnistuneesti Linode.com palveluun, joten jätänkin tässä kohtaa rekisteröimisen vaiheet käymättä läpi ja siirryn suoraan virtuaalipalvelimen luontiin ja konfigurointiin. Esivalmisteluna loin ihan ensimmäiseksi oman SSH avaimen virtuaalikoneellani:</br>
![Kuva2](https://user-images.githubusercontent.com/122887740/217283182-24ba894f-36eb-4a0a-9856-d787adca18a6.png)</br>


SSH avaimen luonnin jälkeen siirryin Linoden palveluun ja aloitin painamalla Create Linode:</br>
![Kuva1](https://user-images.githubusercontent.com/122887740/217281963-fc5ae696-fc16-4be3-ba87-0a4b53ac6945.png)


Loin virtuaalikoneen seuraavilla määritteillä:</br>
- Image: Debian 11
- Region: Frankfurt, DE (eu-central)
- Linode Plan: Shared CPU - Nanode 1Gb (1 CPU, 25Gb Storage, 1Gb RAM)
- Linode Label: Testipannu
- Root Password: vahva salasana
- SSH Keys: itse luotu SSH

Jätin valinnaiset kohdat tyhjiksi, koska ne toisivat lisähintaa palvelimen vuokraukselle. Painoin seuraavaksi Create:


Palvelin onkin jo käynnistymässä: </br>
![Kuva3](https://user-images.githubusercontent.com/122887740/217284064-0a1aac42-8605-44f5-abf5-1da9aea3e1c1.png)

Palvelimen luontiin ei mennyt pitkään, eikä sen provisiointikaan kestänyt palvelulla kauan, ehkä vain joitain minuutteja.


## b) Tee alkutoimet omalla virtuaalipalvelimellasi
Klo 17:14

Seuraavaksi oli aika siirtyä kokeilemaan yhteyttä uuteen palvelimeen sekä siirtyä sen konfigurointiin. Aloitin luomalla SSH-yhteyden palvelimeen käyttämällä komentoa ```ssh root@143.42.18.59```. Tämän jälkeen yhteys pyysi koneeseen linkitetyn SSH-avaimen salasanaa, jotta se voitiin aktvoida: </br>
![Kuva 4](https://user-images.githubusercontent.com/122887740/217285528-b99bfe45-2cfe-4e59-a35a-96d3f13c628e.png)


Oli aika siirtyä konfiguroimaan uutta palvelinta. Aloitin tekemällä päivitykset käyttämällä komentoja ```apt-get update``` sekä ```apt-get upgrade``` (HUOM. olin jo root oikeuksilla, sen takia en käyttänyt ```sudo``` etuliitettä komennossa: </br>
![Kuva5](https://user-images.githubusercontent.com/122887740/217286773-9f07c291-4129-45ea-8038-76d1bbccea3f.png)


![Kuva6](https://user-images.githubusercontent.com/122887740/217286785-3337dd0c-e736-4007-b10e-9aea23148a26.png)


Tämän jälkeen siirryin konfiguroimaan palomuuria käyttämällä komentoa ```sudo ufw allow 22```. Törmäsin heti ongelmaan, jossa terminaali ilmoitti, että se ei löydä komentoa nimeltä UFW, joten siirryin asentamaan sen käyttämällä komentoa ```apt-get install ufw```.

Komento ei löydy: </br>
![Kuva7](https://user-images.githubusercontent.com/122887740/217287890-ae2d57d3-77f3-4f8f-8be6-47a73b1a7a99.png)


Asennuksen jälkeen sain onnistuneesti muokattua palomuurisääntöjä käyttämällä komentoja ```sudo ufw allow 22/tcp``` ja ```sudo ufw enable```:</br>
![Kuva 8](https://user-images.githubusercontent.com/122887740/217290060-1e675b8d-1557-42f3-be4f-002e07500995.png)



Seuraavaksi vuorossa oli luoda itselle oma admin tunnus ja sulkea root-tunnukset SSH-palvelulta. Ensimmäisenä vuorossa oli omat järjestelmänvalvoja tunnukset: </br>

Syötin peräkanaa seuraavat komennot:
- ```sudo adduser matti```
- ```sudo adduser matti sudo```
- ```sudo adduser matti adm```
- ```sudo adduser matti admin```</br>


![Kuva9](https://user-images.githubusercontent.com/122887740/217290188-ccaa8f3f-4daa-4162-bff1-7547cc7e496f.png)</br>
![Kuva10](https://user-images.githubusercontent.com/122887740/217506745-4c54f668-2a4b-4eb7-be01-2b6c3ba10d33.png)</br>
![Kuva11](https://user-images.githubusercontent.com/122887740/217290210-213abbc3-b788-4763-9d1f-7beb71160ce4.png)</br>
![Kuva12](https://user-images.githubusercontent.com/122887740/217290218-53144c15-5a3d-4036-b8a3-164b19f58510.png)</br>

Luonnin jälkeen oli aika siirtyä käyttämään käyttäjää matti, jotta pääsin samalla sulkemaan rootin SSH:lta. Sammutin SSH-yhteyden komennolla ```exit``` ja aloitin uuden session kirjautumalla ```ssh matti@143.42.18.59```:</br>
![Kuva13](https://user-images.githubusercontent.com/122887740/217290955-45720ea1-099b-4bf8-b107-e0bbe7460642.png)


Käyttäjänä matti aloitin rootin sulkemisen SSH:ssa käyttämällä komentoa ```sudoedit /etc/ssh/sshd_config```, tämä avasi seuraavan näkymän:</br>
![Kuva14](https://user-images.githubusercontent.com/122887740/217291505-3345307f-c8d7-44e1-9b7e-bf3572ecfdca.png)

Näkymässä tuli vaihtaa kohta: PermitRootLogin yes -> no


Tämän jälkeen käynnistin vielä uudelleen SSH palvelun komennolla ```sudo service ssh restart``` ja testasin vielä root kirjautumisen: </br>
![Kuva15](https://user-images.githubusercontent.com/122887740/217292584-5ce0fe1d-a57c-4605-8fa7-01b53bc936ff.png)


Asetuksen konfigurointi toimi niin kuin pitikin. Asennus ja konfigurointi näytti menevän mallikkaasti ja oli aika siirtyä seuraavaan haasteeseen -> Apache.


## c) Asenna weppipalvelin omalle virtuaalipalvelimellesi
Klo 17:50


Asennusten jälkeen siirryin asentamaan vielä Apachen web-palvelimen uudelle virtuaalipalvelimelle. Asennus alkoi niin kuin aiemminkin, eli komenolla: ```sudo apt-get install apache2```. Asennuksen jälkeen siirryin luomaan testisivua, tarkastin ensin olevani käyttäjän ```matti``` kotikansiossa, jonka alle loin uuden alikansion komennolla ```mkdir public_site```, luonnin jälkeen siirryin kansioon. Kansiossa aloin luomaan Micro-työkalulla index.html tiedostoa käyttämällä komentoa ```micro index.html```, mutta huomasinkin, että Micro ei ollutkaan esiasennettuna koneelle, joten asensin sen komennolla: ```sudo apt-get install micro```. Asennuksen jälkeen pääsin luomaan sivua index.html, lainasin pohjaa, jonka opettajani Tero Karvinen on luonut ja julkaissut sivuillaan "Short HTML5 page": </br>
![Kuva16](https://user-images.githubusercontent.com/122887740/217295478-70416aee-1ebc-4792-9cb4-76ef5ded5360.png)


Sitten oli vuorossa navigoida Apachen kansioon tekemään muokkaukset omaa sivua varten, jotta liikenne ohjautuisi oikeaan osoitteeseen palvelimelle navigoitaessa.
Navigoin ihan ensimmäiseksi oikeaan osoitteeseen komennolla: ```cd /etc/apache2/sites-available```, tämän jälkeen aktivoin Apachessa käyttäjien kansiot komennoilla ```sudo a2enmod userdir``` sekä ```sudo systemctl restart apache2```: </br>
![Kuva17](https://user-images.githubusercontent.com/122887740/217297128-e6bd5143-f940-4c07-bb04-9cc01bd4a0dc.png)



Aktivoinnin jälkeen siirryin luomaan uutta sivustomääritelmää komennolla ```sudoedit /etc/apache2/sites-available/frontpage.conf```: </br>
![Kuva18](https://user-images.githubusercontent.com/122887740/217297750-08c1c73d-64da-41f7-82db-ad6e9191821d.png)


Konfiguraatiotiedoston jälkeen oli vuorossa poistaa oletussivumääritys 000-default.conf ja korvata se uudella frontpage.conf - tiedostolla. Homma eteni seuraavasti: </br>
- 1: ```sudo a2dissite 000-default.conf```
- 2: ```sudo a2ensite frontpage.conf```
- 3: ```systemctl restart apache2```.


Lopulta vielä avasin palomuurista portin 80, jotta HTTP-liikenne sallitaan palvelimelle, etenin komennolla ```sudo ufw allow 80/tcp```.

Testasin miltä näyttää ja tuloshan ei ollut lupaava: </br>
![Kuva19](https://user-images.githubusercontent.com/122887740/217300149-2b9a49ca-8acc-4195-9597-ab00349197b1.png)


Käytin vianmääritykseen komentoa: ```sudo apache2ctl configtest```:</br>
![Kuva20](https://user-images.githubusercontent.com/122887740/217300694-ef1c495a-8e52-44f5-9371-fad8cc17c9a0.png)


Ongelma oli siis väärinkirjoitetuissa määritteissä, korjasin virheen ja käynnistin Apachen palvelut uudelleen. Niinhän se olikin, että pieni näppäilyvirhe määritteissä aiheutti ongelman </br>
![Kuva21](https://user-images.githubusercontent.com/122887740/217301147-85412b4b-6c94-4b3d-b728-511d3ebb9d24.png) </br>
![Kuva22](https://user-images.githubusercontent.com/122887740/217301172-19fd41c0-ab4e-4fa1-ab9e-b95cfb5b9940.png) </br>


Palvelin on nyt konfiguroitu valmiiksi.

## d) Etsi merkkejä murtautumisyrityksistä
Klo 18:18

Lopulta oli vuorossa tutkia palvelimelta murtautumisyrityksiä. Navigoin Teron vinkkaamaan lokikansioon tutkimaan mahdollisia murtautumisyrityksiä.
Kävin tutkimassa ```auth.log``` tiedostoa kansiossa ```/var/log``` ja sain seuraavat tulokset: </br>
![Kuva23](https://user-images.githubusercontent.com/122887740/217303287-1374f80b-518a-45a3-83ed-5fa4e3bcd55b.png)


Lokia tulkittaessa sinne on jo yritetty päästä sisälle, mutta järjestelmä on evännyt pääsyn. Toista lokitiedostoa ``` /var/log/apache2/access.log``` tutkiessa ei tullut tuloksia vastaan lainkaan.


## Lopetus
Lopetin tehtävien teon klo 18:25. Kyseinen tehtävä avasi hyvin virtuaalipalvelimen isännöinnistä, Linuxin palomuuria sekä murtautumisyritysten tarkastelua. SSH oli entuudestaan itselleni tuttu, joten se ei tuonut mitään uutta. Töihin meni tällä erää n. 1,5h.

## Lähteet:
Karvinen, Tero, 7.2.2023 - h6 Real Internet(tm) (https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/)

Karvinen, Tero, 19.9.2017 - First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS (https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/)

Karvinen, Tero, 12.2.2012 - Short HTML5 page (https://terokarvinen.com/2012/short-html5-page/)

