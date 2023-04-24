# Git

## Tehtävät

![image](https://user-images.githubusercontent.com/122887067/233955570-3f0a29fd-4705-4b90-a4c3-680b7523a432.png)

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
    
## a) Tee oma shell script (bash, sh...) ja laita se kaikille käyttäjille 12:15-12:45

Aloitin tekemällä uuden kansion

    mkdir scriptit
    
Tein kello.sh tiedoston jonka sisällä on

    #!/usr/bin/bash
    date --iso

Annoin execute oikeudet käyttäjille

    sudo cdmod ugo+x kello.sh
    
Siirsin kello.sh tiedoston /usr/local/bin

    sudo cp kello.sh /usr/local/bin
    
Testi,

![image](https://user-images.githubusercontent.com/122887067/233960186-b1b0dce4-0fcf-4afd-a170-c6b8a4a65fb2.png)

Toimii

## b) hello.py. Tee oma Python-skripti ja laita se kaikille käyttäjille 12:45-13:00

Tein alussa tehtyyn scriptit kansiion hello.py tiedoston

    #!/usr/bin/python3
    print("Hello pyy")

vagrant@tmaster:~/scriptit$ python3 hello.py

    Hello pyy

Kopioin hello.py /usr/local/biniin

    sudo cp hello.py /usr/local/bin/hellopy
   
Muokkasin execute oikeudet käyttäjille

    sudo chmod ugo+x /usr/local/bin/hellopy
    
vagrant@tmaster:~/scriptit$ ls -l /usr/local/bin/

    total 8
    -rwxr-xr-x 1 root root 38 Apr  9 20:50 hellopy
    -rwxr-xr-x 1 root root 27 Apr  9 20:35 kello.sh
    
Toimii

![image](https://user-images.githubusercontent.com/122887067/233964521-faf2d658-74c2-44cc-973f-78f7a69e9083.png)

## c) Automatisoi näiden skriptien asennus orjille Saltilla. 13:00-13:35

Aloitin tekemällä uuden kansion saltin alle

    mkdir /srv/salt/skripti

Kopioin kello.sh ja hello.py scriptit kansiosta saltin alle

    sudo cp kello.sh /srv/salt/skripti
    sudo cp hello.py /srv/salt/skripti

Tarkistin stat komennolla kello.sh "tilastot", jotta näen mitä laitan "- mode" kohtaan.

![image](https://user-images.githubusercontent.com/122887067/233970198-55422bf5-1265-4d56-b810-fd9a55560fa5.png)

Tein kello.sh init tiedoston

    micro init.sls

Initin sisältö

    vagrant@tmaster:/srv/salt/skripti$ cat init.sls
    /usr/local/bin/kello:
      file.managed:
        - source: "salt://skripti/kello.sh"
        - mode: "0755"
    /usr/local/bin/hellopy:
      file.managed:
        - source: "salt://skripti/hello.py"
        - mode: "0755"
        
Ajetaan
    
        vagrant@tmaster:/srv/salt$ sudo salt '*' state.apply skripti/
    t001:
    ----------
              ID: /usr/local/bin/kello
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/kello updated
         Started: 21:30:32.888179
        Duration: 23.59 ms
         Changes:
                  ----------
                  diff:
                      New file
                  mode:
                      0755
    ----------
              ID: /usr/local/bin/hellopy
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/hellopy updated
         Started: 21:30:32.911936
        Duration: 13.417 ms
         Changes:
                  ----------
                  diff:
                      New file
                  mode:
                      0755

    Summary for t001
    ------------
    Succeeded: 2 (changed=2)
    Failed:    0
    ------------
    Total states run:     2
    Total run time:  37.007 ms
    t002:
    ----------
              ID: /usr/local/bin/kello
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/kello updated
         Started: 21:29:50.917137
        Duration: 43.431 ms
         Changes:
                  ----------
                  diff:
                      New file
                  mode:
                      0755
    ----------
              ID: /usr/local/bin/hellopy
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/hellopy updated
         Started: 21:29:50.960666
        Duration: 16.552 ms
         Changes:
                  ----------
                  diff:
                      New file
                  mode:
                      0755

    Summary for t002
    ------------
    Succeeded: 2 (changed=2)
    Failed:    0
    ------------
    Total states run:     2
    Total run time:  59.983 ms

Testit.

![image](https://user-images.githubusercontent.com/122887067/233971722-8bcc9288-3e8b-48b7-8c65-37c6aa00e114.png)

## d) Asenna jokin yhden binäärin ohjelma Saltilla orjille. 13:35-

Orjilla ei ole vielä microa, joten asennan sen.

![image](https://user-images.githubusercontent.com/122887067/233972485-df68e631-61d6-4ff8-ae7c-be9541043cfb.png)

Ensin etsin binääri tiedoston microlle. Se löytyy GitHubista.

![image](https://user-images.githubusercontent.com/122887067/233973642-23391124-1489-4295-ae8f-4b0b4648cb08.png)

https://github.com/zyedidia/micro/releases

Latasin micron

    wget https://github.com/zyedidia/micro/releases/download/v2.0.11/micro-2.0.11-linux64.tar.gz

Purin .gz tiedoston

    tar -xf micro-2.0.11-linux64.tar.gz
    
Kopioin micro tiedoston saltin alle ja /usr/local/bin

    sudo cp micro /srv/salt/skripti
    sudo cp micro /usr/local/bin
Muokkasin init.sls tiedostoa.

![image](https://user-images.githubusercontent.com/122887067/233974960-62e9b4e6-26b8-4e0a-b865-bd895ce0ba9e.png)

Ajetaan micro orjakoneille

    vagrant@tmaster:/srv/salt$ sudo salt '*' state.apply skripti/
    t002:
    ----------
              ID: /usr/local/bin/kello
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/kello is in the correct state
         Started: 21:54:16.089324
        Duration: 18.118 ms
         Changes:
    ----------
              ID: /usr/local/bin/hellopy
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/hellopy is in the correct state
         Started: 21:54:16.107546
        Duration: 10.2 ms
         Changes:
    ----------
              ID: /usr/local/bin/micro
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/micro updated
         Started: 21:54:16.117830
        Duration: 323.856 ms
         Changes:
                  ----------
                  diff:
                      New file
                  mode:
                      0755

    Summary for t002
    ------------
    Succeeded: 3 (changed=1)
    Failed:    0
    ------------
    Total states run:     3
    Total run time: 352.174 ms
    t001:
    ----------
              ID: /usr/local/bin/kello
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/kello is in the correct state
         Started: 21:54:58.044685
        Duration: 20.306 ms
         Changes:
    ----------
              ID: /usr/local/bin/hellopy
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/hellopy is in the correct state
         Started: 21:54:58.065090
        Duration: 7.954 ms
         Changes:
    ----------
              ID: /usr/local/bin/micro
        Function: file.managed
          Result: True
         Comment: File /usr/local/bin/micro updated
         Started: 21:54:58.073129
        Duration: 356.835 ms
         Changes:
                  ----------
                  diff:
                      New file
                  mode:
                      0755

    Summary for t001
    ------------
    Succeeded: 3 (changed=1)
    Failed:    0
    ------------
    Total states run:     3
    Total run time: 385.095 ms
    
    
Testit.

![image](https://user-images.githubusercontent.com/122887067/233976430-605ab89d-4a1e-4820-bf4d-41f8dca84c06.png)

![image](https://user-images.githubusercontent.com/122887067/233976394-854d0efa-99b7-4878-b322-3ee8d73d9012.png)

Nyt micro toimii myös orjilla.

## Lähteet 
https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/
