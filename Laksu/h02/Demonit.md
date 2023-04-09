# Demonit

## Tehtävät
![image](https://user-images.githubusercontent.com/122887067/230770599-c966d491-e390-4e9c-b1ca-0acbd7fe8ef3.png)


## Rauta

    Koneen rauta ja käyttöjärjestelmä
    CPU:  i7-13700K 5,4GHz
    RAM:  32GB DDR5 5200Mhz
    GPU:  RTX 3080 OC 10G
    OS:   Windows 11 Pro, Versio 22H2
    
    Versiot. 
    
    Vagrant 2.3.4
    debian 5.10.158-2
    bullseye 5.10.0-20-amd64
    
## x) Lue ja tiivistä

### Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port 

## a) Asenna OpenSSH-palvelin käsin. Laita se kuuntelemaan oletusportin lisäksi jotain toista porttia. Testaa lopputulos.

## b) Automatisoi äsken tekemäsi SSH-konfiguraatio Saltilla.

## c) Tee jokin muu asetus äsken tekemääsi SSH-palveluun. Osoita testein, että Salt käynnistää demonin uudelleen, kun asetustiedosto on muuttunut (jolloin uudet asetukset tulevat voimaan). Osoita, että Saltin ajaminen ei käynnistä demonia uudelleen, jos asetukset eivät ole muuttuneet. (Helpoin asetus on lisätä kolmas portti mukaan, haastavampia löytyy esim 'man sshd_config').


## Lähteet
https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh
