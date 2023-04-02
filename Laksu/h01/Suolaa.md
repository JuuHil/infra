# Suolaa

## Tehtävät
![image](https://user-images.githubusercontent.com/122887067/229345542-d77c7c75-b7e4-48d7-81e0-93007b3e2231.png)


## Rauta

    Koneen rauta ja käyttöjärjestelmä
    CPU:  i7-13700K 5,4GHz
    RAM:  32GB DDR5 5200Mhz
    GPU:  RTX 3080 OC 10G
    OS:   Windows 11 Pro, Versio 22H2
    
    Versiot. 
    VirtualBox -7.0.6-155176.
    Vagrant 2.3.4
    debian-live -11.6.0
    
## x) Lue ja tiivistä
#### Create a Web Page Using Github 

- Kuinka tehdä sivu käyttämällä GitHubia.
- Helpot ohjeet aloittaa GitHubin käyttö.
- Kuinka kirjoittaa "MarkDown"
- Käytä lisenssejä.

#### Salt Vagrant - automatically provision one master and two slaves 

- Ohjeet kuinka hallita satoja tietokoneita.
- Saltin käyttöä.
- Yleisiä komentoja.
- Ja lopussa vagrantilla koneiden tyhjentäminen

## a) Asenna Debian 11 Vagrantilla. 13:00-13:30

![image](https://user-images.githubusercontent.com/122887067/229347598-82d82432-87a3-49d3-a036-d19cb0426abb.png)

Latasin ja asensin Vagrantin version 2.3.4 (https://developer.hashicorp.com/vagrant/downloads)

Suoritin ohjatun asennuksen oletusasetuksilla ja käynnistin tietokoneen uudelleen.

Käynnistin Windows Powershellin järjestelmänvalvojana ja tein polun C:\Users\xhild\vagrant\debian> `mkdir` avulla.

    vagrant init debian/bullseye64
    
![image](https://user-images.githubusercontent.com/122887067/229347776-4a622b37-2497-4d39-89cc-87b98aa24100.png)

    vagrant up
    
![image](https://user-images.githubusercontent.com/122887067/229347506-198741a0-9f92-4497-805c-ee610fa80747.png)

    vagrant ssh

![image](https://user-images.githubusercontent.com/122887067/229347545-fef87fd2-3227-4490-8ee8-e1ae85dad045.png)

Sain yhteydeyn virtuaalikoneeseen SSH:n kautta.

## b) Asenna artikkelissa (Karvinen 2023) kuvattu kolmen koneen verkko 13:30-13:45

Muokkasin polussa C:\Users\xhild\vagrant\debian olevaa VagrantFile:ä

![image](https://user-images.githubusercontent.com/122887067/229350528-4648b22b-800e-49fe-a6d2-7910f0bc6095.png)

Ajoin komennon `vagrant up` ja odotin muutaman minuutin.

Kirjauduin mestari koneelle.

    vagrant ssh tmaster 

## c) Hyväksy orjat (t001 ja t002) ohjattavaksi ja testaa yhteys 13:45-13:50

Hyväksyin orjien avaimet ja kokeilin, että yhteys toimii.

    sudo salt-key -A
    sudo salt '*' test.ping

![image](https://user-images.githubusercontent.com/122887067/229350794-96e7bf2b-1dc3-40a5-96cf-3e749a5ec079.png)

## d) Näytä esimerkit seuraavista tiloista: package, file, service, user, cmd.run. (voit käyttää state.single) 14:30-15:15

#### Esimerkki package
Asensin apache2.

![image](https://user-images.githubusercontent.com/122887067/229351184-2c532461-0128-485c-a750-9645552a11a8.png)

#### Esimerkki file
Tein kansion.

![image](https://user-images.githubusercontent.com/122887067/229350999-ef798ffa-bc55-4642-9baa-724aed629582.png)

#### Esimerkki service
Laitoin apachen päälle

![image](https://user-images.githubusercontent.com/122887067/229351231-3a71050c-c4ac-4747-9bfc-2e8b4aa0ea56.png)

#### Esimerkki user

Tein käyttäjän.

![image](https://user-images.githubusercontent.com/122887067/229351311-cc2bda5f-e25e-4cdc-acdf-d4ea0132e131.png)

#### Esimerkki cmd.run

Ajoin vielä whoami ja hostnamen orjilla

![image](https://user-images.githubusercontent.com/122887067/229351356-7a177ab2-1a29-4d29-bac4-2133efe28752.png)

## e) Tee infraa koodina, esim oma hei maailma.

## Lähteet

Create a Web Page Using Github (https://terokarvinen.com/2023/create-a-web-page-using-github/)

Salt Vagrant - automatically provision one master and two slaves (https://terokarvinen.com/2023/salt-vagrant/)
