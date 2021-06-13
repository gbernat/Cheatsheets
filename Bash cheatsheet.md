# Bash cheatsheet


- whois
```
Giskard:~ guido$ whois anses.gob.ar
% IANA WHOIS server
% for more information on IANA, visit http://www.iana.org
% This query returned 1 object

refer:        whois.nic.ar

domain:       AR

organisation: Presidencia de la Nación – Secretaría Legal y Técnica
address:      Balcarce N°50 – Planta Baja
address:      Buenos Aires C1064AAB
address:      Argentina

contact:      administrative
name:         Mg. Marcelo Patricio Funes
organisation: Presidencia de la Nación – Secretaría Legal y Técnica - Dirección Nacional del Registro de Dominios de Internet
address:      San Martín 536 – Piso 1
address:      Buenos Aires C1004AAL
address:      Argentina
phone:        +54 11 5235 1160
fax-no:       +54 11 5235 1160
e-mail:       funesmp@nic.gob.ar

contact:      technical
name:         Sr. Juan Olmos
organisation: Presidencia de la Nación – Secretaría Legal y Técnica - Dirección General de Sistemas Informaticos
address:      Balcarce N°50 - Planta Baja
address:      Buenos Aires C1064AAB
address:      Argentina
phone:        +54 11 5235 1160
fax-no:       +54 11 5235 1160
e-mail:       jolmos@nic.gob.ar

nserver:      A.DNS.AR 200.108.145.50 2801:140:0:0:0:0:0:10
nserver:      AR.CCTLD.AUTHDNS.RIPE.NET 193.0.9.59 2001:67c:e0:0:0:0:0:59
nserver:      B.DNS.AR 200.108.147.50 2801:140:11:0:0:0:0:50
nserver:      C.DNS.AR 200.108.148.50 2801:140:10:0:0:0:0:10
nserver:      D.DNS.AR 192.140.126.50 2801:140:dddd:0:0:0:0:50
nserver:      E.DNS.AR 170.238.66.50 2801:140:eeee:0:0:0:0:50
nserver:      F.DNS.AR 130.59.31.20 2001:620:0:ff:0:0:0:20
ds-rdata:     19606 8 2 4415CF1A2CF10DE94B92BC020F21D1BF4163B2E90F2A6F6A5D2A1740339D566C

whois:        whois.nic.ar

status:       ACTIVE
remarks:      Registration information: https://nic.ar

created:      1987-09-23
changed:      2020-04-28
source:       IANA

# whois.nic.ar

% La información a la que estás accediendo se provee exclusivamente para
% fines relacionados con operaciones sobre nombres de dominios y DNS,
% quedando absolutamente prohibido su uso para otros fines.
%
% La DIRECCIÓN NACIONAL DEL REGISTRO DE DOMINIOS DE INTERNET es depositaria
% de la información que los usuarios declaran con la sola finalidad de
% registrar nombres de dominio en ‘.ar’, para ser publicada en el sitio web
% de NIC Argentina.
%
% La información personal que consta en la base de datos generada a partir
% del sistema de registro de nombres de dominios se encuentra amparada por
% la Ley N° 25326 “Protección de Datos Personales” y el Decreto
% Reglamentario 1558/01.

domain:		anses.gob.ar
registrant:	33637617449
registrar:	nicar
registered:	1997-04-16 00:00:00
changed:	2021-04-26 16:58:16.730984
expire:		2022-05-16 00:00:00

contact:	33637617449
name:		ADMINISTRACION NACIONAL DE LA SEGURIDAD SOCIAL ANSES
registrar:	nicar
created:	2014-04-08 00:00:00
changed:	2021-04-26 16:58:15.404333

nserver:	ns02.anses.gov.ar (200.10.199.115/32)
nserver:	ns01.anses.gov.ar (200.10.199.106/32)
registrar:	nicar
created:	2016-06-30 22:16:04.268417
```

- dig
```
Giskard:~ guido$ dig anses.gob.ar

; <<>> DiG 9.10.6 <<>> anses.gob.ar
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12057
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;anses.gob.ar.			IN	A

;; ANSWER SECTION:
anses.gob.ar.		3411	IN	A	45.60.37.128
anses.gob.ar.		3411	IN	A	45.60.31.128

;; Query time: 45 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Wed Apr 28 18:52:30 -03 2021
;; MSG SIZE  rcvd: 73
```


- Previous command
```
esc . o !$ => repeat the last argument of the previous command
!^ => the first no-command argument of the previous command
!* => all non-command arguments of the previous command
!!         => repeat the last command
Ctrl + a => Return to the start of the command you’re typing
Ctrl + e => Go to the end of the command you’re typing
Ctrl + w => Delete the word / argument left of the cursor
^string^string2 => takes the last command, replaces string with string2
```

- Mandar proceso a bg:
```
# Ctrl+z (suspende proc)
# bg
Luego, para volver a fg:
# fg
Para ver procesos:
# jobs
Para pasar a fg/bg un proceso en particular:
# <fg|bg> %n
```

- tcpdump  
tcpdump -i ens160 -vvv -nn "src host 10.6.9.66 and dst port 20514"

- Matar session ssh:
```
# pkill -9 -u username
Para una tty especifica:
# w
Identifico que tty quiero
# ps -ft pts/n
Mato al PID
# kill -9 PID
```

- Directory moves
```
# cd ~      - moves to the user's home
# cd -      - moves to the last directory
```

- Expansions  
This runs the command with the same arguments, only with the parts inside the brace changed—the first part corresponding to the first argument, the second part corresponding to the second argument.
```
# mv /path/to/file.{txt,xml}
# sudo cp /etc/rc.conf{,-old}
```

- Shortcuts  
```
Alt + F  : Go right (forward) one word.
Ctrl + F : Go right (forward) one character.
Ctrl + K : Delete all characters after the cursor on the current line.
Ctrl + U : Delete all characters before the cursor on the current line.
```


Ver:
tee
netstat 
nc
tcpdump
lsof
iptables
ifconfig
dig
wall
screen
nslookup
xargs
rsync
find

archivos importantes (por ej /proc/xxx)

- SCP file from remote to local, using a key  
```
$ scp -i /tmp/EC2tutorial.pem ec2-user@52.67.66.155:/home/ec2-user/paramiko.zip /tmp/paramiko.zip
```

- zip recursivelly a fila and a directory altogheter
```
$ zip -r result.zip /tmp/file.py /tmp/directory
```

## Systemd / Systemctl
- Creating Linux Service with systemd/systemctl
Create a file called **/etc/systemd/system/rot13.service**:
```
[Unit]
Description=ROT13 demo service
After=network.target
StartLimitIntervalSec=0  
  
[Service]
Type=simple
Restart=always
RestartSec=1
User=centos
ExecStart=/usr/bin/env php /path/to/server.php  
  
[Install]
WantedBy=multi-user.target
```

You’ll need to:  
set your actual username after User=  
set the proper path to your script in ExecStart=  
set the services that need to be running before in After=  

If necessary, reload the service files to include the new service:  
$ sudo systemctl daemon-reload

Enable service:  
$ systemctl enable rot13  

Use service:  
$ systemctl start rot13

**OBS**: Para projects Python en env, se puede agregar en [Service]:
WorkingDirectory=<path to your project directory>
Environment="PATH=<path to virtual environment>/bin"
ExecStart=<path to python script>

- Comandos utiles systemctl
List all system units of systemd: ```$ systemctl list-units --all```  
List service type units: ```$ systemctl list-units --type=service```  
Displaying Dependencies: ```$ systemctl list-dependencies sshd.service```  




