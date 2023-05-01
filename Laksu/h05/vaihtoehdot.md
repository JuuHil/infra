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
    
## c) Hei ikkuna! Tee hei maailma Windowsin Saltille. Voit vaikkapa tehdä tyhjän tiedoston johonkin väliaikaistiedostojen kansioon. Käytä idempotentteja komentoja, esim file.managed. 17:55-18:15

Tein E: levylle tyhjän tiedoston nimeltä heimaailma komennolla ` .\salt-call.exe --local state.single file.managed E:\heimaailma`

    PS C:\Program Files\Salt Project\Salt> .\salt-call.exe --local state.single file.managed E:\heimaailma
    [WARNING ] State for file: E:\heimaailma - Neither 'source' nor 'contents' nor 'contents_pillar' nor 'contents_grains' was defined, yet 'replace' was set to 'True'. As there is no source to replace the file with, 'replace' has been set to 'False' to avoid reading the file unnecessarily.
    local:
    ----------
              ID: E:\heimaailma
        Function: file.managed
          Result: True
         Comment: Empty file
         Started: 17:57:30.104524
        Duration: 13.671 ms
         Changes:
                  ----------
                  new:
                      file E:\heimaailma created

    Summary for local
    ------------
    Succeeded: 1 (changed=1)
    Failed:    0
    ------------
    Total states run:     1
    Total run time:  13.671 ms
   
## d) Installed. Asenna Windowsille ohjelma Saltilla. (Voit käyttää eri vaihtoehtoja: kopioida binäärin suoraan sopivaan kansioon, pkg.installed ja choco, pkg.installed ja salt winrepo). 18:15-19:00

Micron asennus.

Ensin etsin binääri tiedoston microlle. Se löytyy GitHubista.

https://github.com/zyedidia/micro/releases/tag/v2.0.11

![image](https://user-images.githubusercontent.com/122887067/235475212-6690eeb3-979e-44f2-bb1f-9c518e21d9e1.png)

asensin `micro-2.0.11-amd64.deb`  C:\Program Files\Salt Project\Salt\ alle.

En saanut purettua .deb tiedostoa.

Kokeilin luoda init.sls tiedoston. 

![image](https://user-images.githubusercontent.com/122887067/235479485-a3a75934-bc08-4170-92f3-62d97bcfedd7.png)

Mutta siinä meni jotain pieleen. 

![image](https://user-images.githubusercontent.com/122887067/235479686-3e31a498-2188-4d0b-9827-b9ce7a6c4295.png)



## Lähteet
https://terokarvinen.com/2018/control-windows-with-salt/

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html
