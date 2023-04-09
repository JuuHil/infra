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

- Saltin avulla voi ohjata isoja määriä demoneja
- Näytetään kuinka voi muuttaa sshd configuraatioita master:illa
- Ja siirtää config. orja koneille.

## a) Asenna OpenSSH-palvelin käsin. Laita se kuuntelemaan oletusportin lisäksi jotain toista porttia. Testaa lopputulos. 17:00-17:25

Käynnistin vagrantin ja menin t001 koneelle

    PS C:\Users\xhild\vagrant\debian> vagrant up
    PS C:\Users\xhild\vagrant\debian> vagrant ssh t001

Testasin toimiiko ssh localhost.

    vagrant@t001:~$ ssh localhost
    vagrant@localhost: Permission denied (publickey).
    
Ei toiminut, joten tein seuraavat (kopioin id_rsa.pub authroized_keys kansioon.)
       
       ssh-keygen #pari kertaa enteriä
       vagrant@t001:~/.ssh$ cat authorized_keys | nl
        1  ssh-rsa      AAAAB3NzaC1yc2EAAAADAQABAAABAQC6APKrf96smtfWNHpu55x8og5G8puY0hwZgu/YSbK3QwH7HfQbRfhlnDUBX5HDlJJlQON3OAKMOWgqFotZtEiVhQvNQk4yWur9rKuy1VV9nl2nYLJpJR0vopj8tOAJFqvRwWen8Hx/K7bPJvhEoIqOQmq1pyMlE2s9gHpfbnxaQka77oPe3NHZ3jqmhGCuIHhDl4EBd3gW/CvbpxIq5CDirgy0/oXVXIVVt2KD0LdOfc14LdX0tTMTLNVM850UT3Xm47QcVPy2N9068j3lmfP1CO5BaIXvVlMVervQSV7hm6B13aLJCaL+uuaK8dPrMl6uCuXAXilbrA8BWm6E/lcp vagrant
        vagrant@t001:~/.ssh$ cat id_rsa.pub >>authorized_keys
        vagrant@t001:~/.ssh$ cat authorized_keys | nl
     1  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6APKrf96smtfWNHpu55x8og5G8puY0hwZgu/YSbK3QwH7HfQbRfhlnDUBX5HDlJJlQON3OAKMOWgqFotZtEiVhQvNQk4yWur9rKuy1VV9nl2nYLJpJR0vopj8tOAJFqvRwWen8Hx/K7bPJvhEoIqOQmq1pyMlE2s9gHpfbnxaQka77oPe3NHZ3jqmhGCuIHhDl4EBd3gW/CvbpxIq5CDirgy0/oXVXIVVt2KD0LdOfc14LdX0tTMTLNVM850UT3Xm47QcVPy2N9068j3lmfP1CO5BaIXvVlMVervQSV7hm6B13aLJCaL+uuaK8dPrMl6uCuXAXilbrA8BWm6E/lcp vagrant
     2  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDLyemxu2uQiU1PHXXR/v87+9Br347yOUvvsLaD13yQKqKIr0xB36Mcbf+YpaS8D/vFDYUG9mkYvZ3ymoju+8GhPgL1SSRXo0xpanyu7H5fbV8DYPtKnVL4NVGQjHySocdvSoX8LzWwz1hzDJk38pvTYK3w9A4l83QjxSfbgW0sbbTGSuqilEA7quMg4epx5XpnEl2I8PmpoRRIxcZtO9sEDkmAvfikmGtL0j5tNEuYvbiydeNLo1THyZpW/40Xm6Sh7a+rsaTUWkVygW3uA0U7bcciyDEK8n8c2JPbsAvpbIRtio6keUHLNK3AJ8v0rHcwoL0A9WrHnPumM8kYB5w/ksnSZXUMNTRlesfvWDIYS8zHmbzxFzkjPpa2QqAzSKPzSpbSjGsCvl3BdDKxSlL1lYDbVGqXlmNBmWqxyNGDL+ZhCyk9va/lAB4nMhuN7VQGn4m9AEmiShHBwT1wjLOI/dmXrzLA0/M9iZjNkIJLIYRFUGnaOu2zCNpzg3EcwIk= vagrant@t001
    
    
Uudelleen testi

    vagrant@t001:~/.ssh$ ssh localhost
    Linux t001 5.10.0-20-amd64 #1 SMP Debian 5.10.158-2 (2022-12-13) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Mon Apr  3 02:31:24 2023 from 10.0.2.2
    vagrant@t001:~$ exit
    logout
    Connection to localhost closed.
    
Ja toimii.

Seuraavaksi menin.

    cd /etc/ssh
    sudoedit sshd_config
    
Muutin portin asetuksia.

![image](https://user-images.githubusercontent.com/122887067/230777750-2fb02daa-bc1a-4216-82d9-b7ef8918f944.png)

Käynnistin demonin uudelle, jotta uudet asetukset tulevat käyttöön
    
    sudo systemctl restart sshd
    
Lopullinen testi tulos

    vagrant@t001:~/.ssh$ ssh -p 22 localhost
    Linux t001 5.10.0-20-amd64 #1 SMP Debian 5.10.158-2 (2022-12-13) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Mon Apr  3 02:49:20 2023 from ::1
    vagrant@t001:~$ exit
    logout
    Connection to localhost closed.
    vagrant@t001:~/.ssh$ ssh -p 123 localhost
    Linux t001 5.10.0-20-amd64 #1 SMP Debian 5.10.158-2 (2022-12-13) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Mon Apr  3 02:51:07 2023 from ::1
   
Toimii!

## b) Automatisoi äsken tekemäsi SSH-konfiguraatio Saltilla. 17:30-18:30
Tulin exitillä ulos t001 koneelta ja menin masterin puolelle

    vagrant ssh tmaster

Tein uuden kansion ja menin sinne 

    vagrant@tmaster:~$ cd /srv/salt/
    vagrant@tmaster:/srv/salt$ sudo mkdir ssh
    vagrant@tmaster:/srv/salt$ cd ssh/

Loin init.sls tiedoston

    vagrant@tmaster:/srv/salt/ssh$ EDITOR=micro sudoedit init.sls

![image](https://user-images.githubusercontent.com/122887067/230780374-68bb7291-60e5-46e3-8575-668e7cdd6b89.png)

Ongelmia  $ sudo salt '*' state.apply sshd. Palaan tehtävään maanantaina


## c) Tee jokin muu asetus äsken tekemääsi SSH-palveluun. Osoita testein, että Salt käynnistää demonin uudelleen, kun asetustiedosto on muuttunut (jolloin uudet asetukset tulevat voimaan). Osoita, että Saltin ajaminen ei käynnistä demonia uudelleen, jos asetukset eivät ole muuttuneet. (Helpoin asetus on lisätä kolmas portti mukaan, haastavampia löytyy esim 'man sshd_config').


## Lähteet
https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh

![image](https://user-images.githubusercontent.com/122887067/230779197-3390847b-c5f9-460f-92bb-4823500f5c94.png)
