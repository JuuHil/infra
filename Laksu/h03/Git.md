# Git

## Tehtävät
![image](https://user-images.githubusercontent.com/122887067/232315676-abcf6364-e8dd-4b27-905a-c72fdef45e26.png)
(https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/)



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
    
## a) Online. Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun). Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "summer". Aiemmin tehty varasto ei kelpaa. (Muista tehdä varastoon tiedostoja luomisvaiheessa, esim README.md ja GNU General Public License 3) 16:30-16:45

Tein uuden varaston GitHubiin nimeltä "summertime".
Loin siihen tiedostot README.md ja LICENSE
Käytin GNU3.0 lisenssiä.

![image](https://user-images.githubusercontent.com/122887067/232316091-1f7c0ead-0b9c-4922-819e-e3693b8f26c8.png)

https://github.com/JuuHil/summertime

## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään. 16:45-17:20
Teen harjoituksen Windows 11 Pro (Versio 22H2)

Aloitin avaamalla Windows PowerShellin ja antamalla komennon

    ssh-keygen.exe
  
jonka jälkeen painoin pari kertaa enteriä ja ajoin komennon joka kopioi avaimen.

    Get-Content -Path ~/.ssh/id_rsa.pub | Clip
  
Kävin lisäämässä julkisen avaimen GitHubiin.

Settings > SSH and GPG keys > New SSH key

![image](https://user-images.githubusercontent.com/122887067/232316465-22f5ef87-96a2-4977-b421-3d220516474e.png)

Kopioin GitHubista SSH osoitteen.

![image](https://user-images.githubusercontent.com/122887067/232317372-d8f379f9-a27c-4334-9e55-f4c7c82b90ea.png)

Asensin Gitin komennolla

    winget install --id Git.Git -e --source winget
  
Tein kansion nimeltä gitit ja komennolla 

    git clone git@github.com:JuuHil/summertime.git
  
kloonasin varaston koneelleni.

![image](https://user-images.githubusercontent.com/122887067/232318947-44c5de54-6521-48cc-a5af-caa6d41edad3.png)

![image](https://user-images.githubusercontent.com/122887067/232318956-49522bdd-ce06-44e5-b3c6-533f7c8297f5.png)

Tein uuden tekstitiedoston 

![image](https://user-images.githubusercontent.com/122887067/232319098-1edc1989-e821-4a9b-92b3-4824076b1eca.png)

Ajoin git add, commit, pull ja push

      PS C:\Users\xhild\gitit\summertime> git add .
      PS C:\Users\xhild\gitit\summertime> git commit
      Author identity unknown

      *** Please tell me who you are.

      Run

        git config --global user.email "you@example.com"
        git config --global user.name "Your Name"

      to set your account's default identity.
      Omit --global to set the identity only in this repository.

      fatal: unable to auto-detect email address (got 'xhild@SYRE-PC23.(none)')
      PS C:\Users\xhild\gitit\summertime> git config --global user.email "01juusohiltunen@gmail.com"
      PS C:\Users\xhild\gitit\summertime> git config --global user.name "Juuso Hiltunen"
      PS C:\Users\xhild\gitit\summertime> git commit
      [main 84a2389] First Commit
       1 file changed, 1 insertion(+)
       create mode 100644 tiedosto.txt
      PS C:\Users\xhild\gitit\summertime> git pull
      Already up to date.
      PS C:\Users\xhild\gitit\summertime> git push
      Enumerating objects: 4, done.
      Counting objects: 100% (4/4), done.
      Delta compression using up to 24 threads
      Compressing objects: 100% (2/2), done.
      Writing objects: 100% (3/3), 333 bytes | 333.00 KiB/s, done.
      Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
      To github.com:JuuHil/summertime.git
         a419719..84a2389  main -> main

Uusi tekstitiedosto tuli GitHubiin.

![image](https://user-images.githubusercontent.com/122887067/232319407-7191e0a3-b45d-4927-a88a-3ee778755773.png)


## c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia. 17:20

Kävin lisäämässä b) kohdassa luotuun tekstitiedostoon tekstin "tyhmä muutos"

![image](https://user-images.githubusercontent.com/122887067/232319577-e9ee86ab-26f1-4137-8aba-d3ec33f05fe9.png)

Ajoin komennot 

    git add . && git status
    
jonka jälkeen tein hard resetin ja katsoin catilla tiedoston sisään.

![image](https://user-images.githubusercontent.com/122887067/232319736-9bce5be3-b168-4aed-921a-c24872f1dabd.png)

## d) Tukki. Tarkastele ja selitä varastosi lokia. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.

Lokissa on nimi ja sähköpostiosoite oikein. Kellokin on oikeassa.

![image](https://user-images.githubusercontent.com/122887067/232319872-3e916abb-3731-41ae-9ad3-08486ca8a337.png)

Ajoin vielä komennon `git log --patch`

![image](https://user-images.githubusercontent.com/122887067/232320243-2f351b5b-cf4a-4971-bff4-9d91425bf34f.png)

Lokista selvenee 

commit: numerokirjain sarja

Author: kuka muokkasi

Date: päivämäärä ja kellonaika

diff --git: mikä muuttui

+commit push pull : tiedostoon lisätty teksti.



## Lähteet 
https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/
