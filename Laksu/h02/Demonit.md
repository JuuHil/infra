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
    vagrant@tmaster:/srv/salt$ sudo micro sshd.sls
    
    openssh-server:
    pkg.installed
    /etc/ssh/sshd_config:
    file.managed:
     - source: salt://sshd_config
    sshd:
     service.running:
     - watch:
         - file: /etc/ssh/sshd_config
         
Kopioin masterin sshd tiedoston samaan kansioon 
   
    vagrant@tmaster:/srv/salt$ sudo cp /etc/ssh/sshd_config /srv/salt/
 
Testasin 
    
![image](https://user-images.githubusercontent.com/122887067/230783563-85b42231-4a4c-4c54-8328-92f5c38ce398.png)




Ongelmia  $ sudo salt '*' state.apply sshd. Palaan tehtävään maanantaina

Maanantai 21:30-23:15

Palasin tehtävään ja väänsin noin 80min tiedostoja ympäriämpäri kunnes sain toimimaan. Tälläisillä asetuksilla.

    vagrant@tmaster:/srv/salt/h2$ ls
    sshd.sls  sshd_config
    vagrant@tmaster:/srv/salt/h2$ cat sshd.sls
    openssh-server:
      pkg.installed
    /etc/ssh/sshd_config:
      file.managed:
        - source: salt://h2/sshd_config
    sshd:
      service.running:
        - watch:
         - file: /etc/ssh/sshd_config
       
    vagrant@tmaster:/srv/salt/h2$ cat sshd_config
    # DON'T EDIT - managed file, changes will be overwritten
    Port 22
    Port 123
    Port 8888
    Protocol 2
    HostKey /etc/ssh/ssh_host_rsa_key
    HostKey /etc/ssh/ssh_host_dsa_key
    HostKey /etc/ssh/ssh_host_ecdsa_key
    HostKey /etc/ssh/ssh_host_ed25519_key
    UsePrivilegeSeparation yes
    KeyRegenerationInterval 3600
    ServerKeyBits 1024
    SyslogFacility AUTH
    LogLevel INFO
    LoginGraceTime 120
    PermitRootLogin prohibit-password
    StrictModes yes
    RSAAuthentication yes
    PubkeyAuthentication yes
    IgnoreRhosts yes
    RhostsRSAAuthentication no
    HostbasedAuthentication no
    PermitEmptyPasswords no
    ChallengeResponseAuthentication no
    X11Forwarding yes
    X11DisplayOffset 10
    PrintMotd no
    PrintLastLog yes
    TCPKeepAlive yes
    AcceptEnv LANG LC_*
    Subsystem sftp /usr/lib/openssh/sftp-server
    UsePAM yes

Testi. 

    vagrant@tmaster:/srv/salt$ sudo salt '*' state.apply h2/sshd

Toimii!!

![image](https://user-images.githubusercontent.com/122887067/230987215-ef657060-471c-4001-9027-840c369d8836.png)

    sudo systemctl restart ssh

Portit toimivat, mutta en saanut publickeytä toimimaan.

![image](https://user-images.githubusercontent.com/122887067/230989197-106362a1-b16f-478f-97ba-94b6e3146153.png)


## c) Tee jokin muu asetus äsken tekemääsi SSH-palveluun. Osoita testein, että Salt käynnistää demonin uudelleen, kun asetustiedosto on muuttunut (jolloin uudet asetukset tulevat voimaan). Osoita, että Saltin ajaminen ei käynnistä demonia uudelleen, jos asetukset eivät ole muuttuneet. (Helpoin asetus on lisätä kolmas portti mukaan, haastavampia löytyy esim 'man sshd_config').

Lisäsin edellisen tehtävän vaiheissa jo kolmannen portin mukaan.

![image](https://user-images.githubusercontent.com/122887067/230989298-f7214db8-f073-447f-9854-a71f61d5cff3.png)


## Lähteet

https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh

https://terokarvinen.com/2023/salt-vagrant/

https://myllys.wordpress.com/palvelinten-hallinta-harjoitus-2-spring-2021/

## Erityiskiitokset

![image](https://user-images.githubusercontent.com/122887067/230989583-a02a25c9-d939-487a-9b12-eb8eb3f5658a.png)

