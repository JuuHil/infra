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

## d) Installed. Asenna Windowsille ohjelma Saltilla. (Voit käyttää eri vaihtoehtoja: kopioida binäärin suoraan sopivaan kansioon, pkg.installed ja choco, pkg.installed ja salt winrepo). 13:30

Aloitin asentamalla Windows virtuaalikoneen. Latauslinkki: https://aka.ms/windev_VM_virtualbox Lähde: https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/

Tiedosto oli suuri (21.1GB) joten latauksessa kesti pitkään (15min, 200Mbit/s).

Käytän H01 tehtävässä tehtyä `tmaster ` konetta herrana uudelle Windows orjalle.

VirtualBoxin Import appliance ei löytänyt zipattuna tiedostoa joten jouduin vielä purkamaan sen. (noin 10min)

Ova tiedoston import vaiheessa tuli uusi ongelma

![image](https://user-images.githubusercontent.com/122887067/236446737-694e2e97-4fc1-4a99-b126-469283632391.png)

Kokeilin kaksi kertaa ja molemmilla kerroilla noin 10minuutin (35-40%) kohdalla tuli sama error.

## Lähteet
https://terokarvinen.com/2018/control-windows-with-salt/

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html

https://salthomework.wordpress.com/h5/
