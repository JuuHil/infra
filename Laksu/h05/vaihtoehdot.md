# Vaihtoehdot

## Tehtävät

![image](https://user-images.githubusercontent.com/122887067/235122055-9e9c4404-a538-4912-a65f-bb69dd3a71b2.png)

https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/
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
    
## x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Huomaa, että osa artikkeleista on luonteeltaan raportteja tai muistiinpanoja, eli vähemmän valmiita kuin testatut ja siivotut ohjeet. Keskity kohtiin, joita voisit itse kokeilla tai hyödyntää) 18:40-18:55

https://terokarvinen.com/2018/control-windows-with-salt/

- Artikkelissa näytettiin Windowsin hallintaa Saltin avulla.
- Kuinka ohjelmistot asennetaan automaattisesti Windowsille
- Kuinka ajetaan etänä PowerShell komentoja

https://salthomework.wordpress.com/h5/

- Toni Sepän 2019 Salt Windows tehtävä
- Seppä hallitsi masterina windows orjaa

## a) Asenna Salt Windowsille 17:00-17:50

Latasin `Salt-Minion-3006.0-Py3-AMD64-Setup` täältä ja ajoin sen järjestelmänvalvojana. 
https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html

Avasin Powershellin ja menin polkuun `C:\Program Files\Salt Project\Salt` 

Kokeilin toimiiko salt-call --local test.ping

    PS C:\Program Files\Salt Project\Salt> .\salt-call --local test.ping
    local:
        True
        
Toimihan se!

## b) Ei voi kalastaa. Käytä Windowsilla Salttia paikallisesti ilman verkkoa (Ruma-X, powershell as admin, salt-call --local state.single ...) 17:50-17:55

Ajoin komennon `.\salt-call.exe --local state.single cmd.run 'whoami'`

    PS C:\Program Files\Salt Project\Salt> .\salt-call.exe --local state.single cmd.run 'whoami'
    local:
    ----------
              ID: whoami
        Function: cmd.run
          Result: True
         Comment: Command "whoami" run
         Started: 17:52:33.649821
        Duration: 20.368 ms
         Changes:
                  ----------
                  pid:
                      17920
                  retcode:
                      0
                  stderr:
                  stdout:
                      syre-pc23\xhild

    Summary for local
    ------------
    Succeeded: 1 (changed=1)
    Failed:    0
    ------------
    Total states run:     1
    Total run time:  20.368 ms
    
## c) Hei ikkuna! Tee hei maailma Windowsin Saltille. Voit vaikkapa tehdä tyhjän tiedoston johonkin väliaikaistiedostojen kansioon. Käytä idempotentteja komentoja, esim file.managed. 13:00-13:30

Tavoitteena on kopioida tyhjä tekstitiedosto toiselle levylle.

Aloitin tekemällä tyhjän teksti tiedoston `C:\Users\xhild\tekstit\empty.txt`

Tein käyttäjälle myös kansion nimeltä suola `C:\Users\xhild\suola`

Suola kansioon tein kansion ´maailma´ ja sen sisälle ´init.sls´

init.sls sisältö

    E:/Heisuola/empty.txt:
      file.managed:
      - source: "C:\Users\xhild\tekstit\empty.txt"
      
Ajoin komennon `salt-call --file-root=C:\Users\xhild\suola --local state.apply 'maailma'`

Mutta sain vastaukseksi 
    PS C:\Program Files\Salt Project\Salt> salt-call --file-root=C:\Users\xhild\suola --local state.apply 'maailma'
    [CRITICAL] Rendering SLS 'base:maailma' failed: expected escape sequence of 8 hexdecimal numbers, but found 's'; line 3

    ---
    E:/Heisuola/empty.txt:
      file.managed:
      - source: "C:\Users\xhild\tekstit\empty.txt"     <======================
    ---
    local:
        Data failed to compile:
    ----------
        Rendering SLS 'base:maailma' failed: expected escape sequence of 8 hexdecimal numbers, but found 's'; line 3

Hetken ihmeteltyäni kopion ChatGPT ylemmän viestin. ChatGPT osasi kertoa, "Tämä virhe johtuu siitä, että käytetyssä tiedostopolussa on merkkejä, joita ei ole asianmukaisesti pakattu tai käsitelty. Merkit, kuten kenoviiva tai kaksoislainausmerkit voivat aiheuttaa tämän virheen." Joten vaihdoin kenoviivat sourcesta kauttaviivoiksi.

Toimiva init.sls

    E:/Heisuola/empty.txt:
      file.managed:
      - source: "C:/Users/xhild/tekstit/empty.txt"

Ajoin salt-callin uudestaan ja se toimii

    PS C:\Program Files\Salt Project\Salt> salt-call --file-root=C:\Users\xhild\suola --local state.apply 'maailma'
    local:
    ----------
              ID: E:/Heisuola/empty.txt
        Function: file.managed
          Result: True
         Comment: File E:/Heisuola/empty.txt updated
         Started: 13:24:38.844252
        Duration: 17.613 ms
         Changes:
                  ----------
                  diff:
                      New file

    Summary for local
    ------------
    Succeeded: 1 (changed=1)
    Failed:    0
    ------------
    Total states run:     1
    Total run time:  17.613 ms
    
Empty.txt oli luotu 13:24 

![image](https://user-images.githubusercontent.com/122887067/236435687-ecd07e67-8087-41ab-9fb5-ca22c39e2743.png)

## d) Installed. Asenna Windowsille ohjelma Saltilla. (Voit käyttää eri vaihtoehtoja: kopioida binäärin suoraan sopivaan kansioon, pkg.installed ja choco, pkg.installed ja salt winrepo). 13:30-16:00

Aloitin asentamalla Windows virtuaalikoneen. Latauslinkki: https://aka.ms/windev_VM_virtualbox Lähde: https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/

Tiedosto oli suuri (21.1GB) joten latauksessa kesti pitkään (15min, 200Mbit/s).

Käytän H01 tehtävässä tehtyä `tmaster ` konetta herrana uudelle Windows orjalle.

VirtualBoxin Import appliance ei löytänyt zipattuna tiedostoa joten jouduin vielä purkamaan sen. (noin 10min)

Ova tiedoston import vaiheessa tuli uusi ongelma

![image](https://user-images.githubusercontent.com/122887067/236446737-694e2e97-4fc1-4a99-b126-469283632391.png)

Kokeilin kaksi kertaa ja molemmilla kerroilla noin 10minuutin (35-40%) kohdalla tuli sama error.

Selvittelyn jälkeen vapaata levytilaa piti olla enemmän kuin minulla oli, joten tyhjensin levyä ja sain import vaiheen hoidettua.

Virtuaali koneen käynnistämisen jälkeen tarkastin mestari koneen saltin version.

    vagrant@tmaster:~$ salt --version
    salt 3002.6
    
Latasin windows koneelle saman version saltista. 

Lataus: https://archive.repo.saltproject.io/windows/Salt-Minion-3002.6-1-Py3-AMD64.msi
Lähde: https://archive.repo.saltproject.io/windows/

Kun yritin avata cmd/powershell työpöytä alkoi vilkkumaan ja jäätyi, kun kokeilin käynnistää virtuaalikoneen uudestaan se ei enää käynnistynyt.

![image](https://user-images.githubusercontent.com/122887067/236451858-500bbfa9-0eac-4d99-b625-05ad54dc691a.png)

Poistin virtuaalikoneen ja importasin uuden. 15:10...

Windowsini oli vieläkin jotenkin korruptoitunut, mutta sain powershellin auki adminina Win+R ja kirjoitin powershell.exe ja painoin Ctrl+Shift+Enter, jotta PowerShell avautuu adminin oikeuksilla.

Tarkistin herra koneen ip:n `ip a`

![image](https://user-images.githubusercontent.com/122887067/236456838-de09a49c-d444-441d-a37c-816a1af5b582.png)

ja asensin saltin loppuun windowsilla.

Asennuksen jälkeen orja on kutsunut herraa.

![image](https://user-images.githubusercontent.com/122887067/236457509-a153941f-4d41-4631-98e6-685220bd9a49.png)

Hyväksyin orjan 

    vagrant@tmaster:~$ sudo salt-key -A
    The following keys are going to be accepted:
    Unaccepted Keys:
    windowsorja
    Proceed? [n/Y] y
    Key for minion windowsorja accepted.
    
Orja toimii.

    vagrant@tmaster:~$ sudo salt 'windowsorja' test.ping
    windowsorja:
        True


Latasin micron Windows version herra koneelle. Lataus: https://github.com/zyedidia/micro/releases/download/v2.0.11/micro-2.0.11-win64.zip

En saanut asennettua unzippiä herra koneelle. Ajoin sudo apt-get update ennen tätä.

    vagrant@tmaster:/usr/bin$ sudo apt-get install unzip
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    Suggested packages:
      zip
    The following NEW packages will be installed:
      unzip
    0 upgraded, 1 newly installed, 0 to remove and 1 not upgraded.
    Need to get 172 kB of archives.
    After this operation, 393 kB of additional disk space will be used.
    Ign:1 https://deb.debian.org/debian bullseye/main amd64 unzip amd64 6.0-26+deb11u1
    Err:1 https://deb.debian.org/debian bullseye/main amd64 unzip amd64 6.0-26+deb11u1
      Certificate verification failed: The certificate is NOT trusted. The certificate chain uses not yet valid certificate.  Could not handshake: Error in the certificate verification. [IP: 151.101.246.132 443]
    E: Failed to fetch https://security.debian.org/debian-security/pool/updates/main/u/unzip/unzip_6.0-26%2bdeb11u1_amd64.deb  Certificate verification failed: The certificate is NOT trusted. The certificate chain uses not yet valid certificate.  Could not handshake: Error in the certificate verification. [IP: 151.101.246.132 443]
    E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?
    
Koska en saanut purettua windows versiota päätin, että siirrän linux version micron Windows orjalle.

Kopioin linux version micron /srv/salt/winukka kansioon.

Init.sls tiedosto

    vagrant@tmaster:/srv/salt/winukka$ cat init.sls
    C:\Windows\system32\micro.exe:
      file.managed:
        - source: "salt://winukka/micro"

Kokeilin ajaa väärän micro version windowsille.

    vagrant@tmaster:/srv/salt/winukka$ sudo salt 'windowsorja' state.apply 'winukka'
    windowsorja:
    ----------
              ID: C:\Windows\system32\micro.exe
        Function: file.managed
          Result: True
         Comment: File C:\Windows\system32\micro.exe updated
         Started: 05:57:54.855667
        Duration: 397.182 ms
         Changes:
                  ----------
                  diff:
                      New file

    Summary for windowsorja
    ------------
    Succeeded: 1 (changed=1)
    Failed:    0
    ------------
    Total states run:     1
    Total run time: 397.182 ms
    
Perille tuli vaikka väärä versio onkin...

![image](https://user-images.githubusercontent.com/122887067/236464054-fc282110-b560-4769-9a00-ac8554f24aa2.png)

## Lähteet
https://terokarvinen.com/2018/control-windows-with-salt/

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html

https://salthomework.wordpress.com/h5/

https://github.com/FredrikAkerlund/InfraAsCode/blob/main/H5%20Vaihtoehtoisuus.md d) Virtuaali Windowsin asennus 
