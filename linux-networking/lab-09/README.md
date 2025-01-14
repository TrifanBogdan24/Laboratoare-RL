# Laborator 09

*Cuprins*:
- [Laborator 09](#laborator-09)
  - [Sheet](#sheet)
  - [Task 1 | Observare porturi deschise pe o statie](#task-1--observare-porturi-deschise-pe-o-statie)
  - [Task 2 | Port forwarding pentru accesare serviciu web](#task-2--port-forwarding-pentru-accesare-serviciu-web)
  - [Task 3 | Accesare serviciu web folosind `wget`](#task-3--accesare-serviciu-web-folosind-wget)
  - [Task 4 | Descarcare recursiva de resurse web (`wget -r`)](#task-4--descarcare-recursiva-de-resurse-web-wget--r)
  - [Task 5 | Accesare UR: cptomamd caractere speciale (**escapare**)](#task-5--accesare-ur-cptomamd-caractere-speciale-escapare)
  - [Task 6 | Accesare de pagini web folosind `curl`](#task-6--accesare-de-pagini-web-folosind-curl)
  - [Task 7 | Download FTP folosind `curl` (fara autentificare)](#task-7--download-ftp-folosind-curl-fara-autentificare)
  - [Task 8 | Trimitere FTP cu `curl` (cu autentificare)](#task-8--trimitere-ftp-cu-curl-cu-autentificare)
  - [Task 9 | Trimitere **e-mail** din linia de comanda (`mail`)](#task-9--trimitere-e-mail-din-linia-de-comanda-mail)
  - [Task 10 | Trimitere **e-mail** cu adresa personalizata din linia de comanda (`mail`)](#task-10--trimitere-e-mail-cu-adresa-personalizata-din-linia-de-comanda-mail)
  - [Task 11 (Bonus) | Upload prin FTP](#task-11-bonus--upload-prin-ftp)
    - [Task 11 | Uploadare de fisier](#task-11--uploadare-de-fisier)
    - [Task 11 | Descarcare fisier uploadat](#task-11--descarcare-fisier-uploadat)
  - [Task 12 (Bonus) | Trimitere **e-mail** cu atasament din linia de comanda](#task-12-bonus--trimitere-e-mail-cu-atasament-din-linia-de-comanda)


## Sheet


```sh
netcat   # creare server/client de TCP/UDP
netstat  # vedem pe ce porturi asculta (TCP/UDP)

curl     # afiseaza pagina web/resursa
wget     # downloadeaza pagina web/resursa

# FTP -> merge cu autentificare 
```

> Pentru a descarca un fisir/director de pe `GitHub` folosind `curl`/`wget`,
> trebuie folosit **URL**-ul pentru fisierul `raw`.



```sh
$ wget -O LICENSE.md https://raw.githubusercontent.com/TrifanBogdan24/workshop-soft-eng/refs/heads/main/LICENSE

$  wget -O angoster-terminal-prompt-theme.json https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/agnoster.minimal.omp.json

$ curl -o avit.omp.json https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/avit.omp.json
```



## Task 1 | Observare porturi deschise pe o statie

In terminalul 1 (serverul):
```sh
root@host:~# netcat -l -u 2024
```


In terminalul 2 (clientul):
```sh
root@host:~$ netstat -tlnp
root@host:~$ netstat -anu
root@host:~$ echo "Un mesaj" | nc -u localhost 2024
```


## Task 2 | Port forwarding pentru accesare serviciu web




```sh
# Afiseaza regulile de IPTABLEs
root@host:~# iptables-save 

root@host:~# iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.2:80

root@host:~# iptables-save 
```

Apoi


```sh
moodle-username@fep8.grid.pub.ro$ curl http://10.9.3.64:8080
<h1>Laborator 10 - pe red</h1>


moodle-username@fep8.grid.pub.ro$ wget http://10.9.3.64:8080
--2024-12-04 23:50:38--  http://10.9.3.64:8080/
Connecting to 10.9.3.64:8080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31 [text/html]
Saving to: 'index.html'

index.html                   100%[============================================>]      31  --.-KB/s    in 0s      

2024-12-04 23:50:38 (2.13 MB/s) - 'index.html' saved [31/31]

moodle-username@fep8.grid.pub.ro$ cat index.html 
<h1>Laborator 10 - pe red</h1>
```


## Task 3 | Accesare serviciu web folosind `wget`


```sh
moodle-username@fep8.grid.pub.ro$ curl http://10.9.3.64:8080
<h1>Laborator 10 - pe red</h1>


moodle-username@fep8.grid.pub.ro$ wget http://10.9.3.64:8080
--2024-12-04 23:50:38--  http://10.9.3.64:8080/
Connecting to 10.9.3.64:8080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31 [text/html]
Saving to: 'index.html'

index.html                   100%[============================================>]      31  --.-KB/s    in 0s      

2024-12-04 23:50:38 (2.13 MB/s) - 'index.html' saved [31/31]

moodle-username@fep8.grid.pub.ro$ cat index.html 
<h1>Laborator 10 - pe red</h1>
```


```sh
moodle-username@fep8.grid.pub.ro$ wget -q http://10.9.3.64:8080
```



```sh
root@host:~# wget https://red/file.dat
root@host:~# wget --no-check-certificate https://red/file.dat
root@host:~# ls -alh file.dat
root@host:~# cat file.dat
```


## Task 4 | Descarcare recursiva de resurse web (`wget -r`)


```sh
$ wget -h | grep 'recursive'
```

```sh
root@host:~# apt-get install tree
```

```sh
roo@host:~# which tree
roo@host:~# clear
roo@host:~# wget -r http://red/folder
roo@host:~# ls -a
roo@host:~# ls
roo@host:~# tree red
```



## Task 5 | Accesare UR: cptomamd caractere speciale (**escapare**)



```sh
root@host:~# wget -O ciudat.txt http://localhost/login.php?name=Lab10&email=rl@upb.ro
root@host:~# cat ciudat.txt 
<html>
<body>
Welcome Lab10<br>
Your email address is: </body>
</html>
```


Rezolvare (cu escapare):

```sh
root@host:~# wget -O raspuns.txt "http://localhost/login.php?name=Lab10\&email=rl@upb.ro"

root@host:~# cat raspuns.txt 
<html>
<body>
Welcome Lab10\<br>
Your email address is: rl@upb.ro</body>
</html>
```


## Task 6 | Accesare de pagini web folosind `curl`


```sh
root@host:~# curl http://red/index.html
<h1>Laborator 10 - pe red</h1>
root@host:~# curl icanhazip.com
141.85.150.39
root@host:~# 
```



## Task 7 | Download FTP folosind `curl` (fara autentificare)



> NU rula `$ cat file-2-of-10M`! (Mult text, not human-readable)

```sh
root@host:~# curl ftp://red/download/file-10M.dat -o file-10M.dat
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 10.0M  100 10.0M    0     0   498M      0 --:--:-- --:--:-- --:--:--  526M
root@host:~# ls -alh file-10M.dat 
-rw-r--r-- 1 root root 10M Dec  5 00:16 file-10M.dat
root@host:~# # NU RULA: cat file-10M.dat
root@host:~#  wget -O file-2-of-10M ftp://red/download/file-10M.dat
--2024-12-05 00:17:49--  ftp://red/download/file-10M.dat
           => ‘file-2-of-10M’
Resolving red (red)... 192.168.1.2
Connecting to red (red)|192.168.1.2|:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD (1) /download ... done.
==> SIZE file-10M.dat ... 10485760
==> PASV ... done.    ==> RETR file-10M.dat ... done.
Length: 10485760 (10M) (unauthoritative)

file-10M.dat                 100%[============================================>]  10.00M  --.-KB/s    in 0.02s   

2024-12-05 00:17:49 (577 MB/s) - ‘file-2-of-10M’ saved [10485760]

root@host:~# ls -alh file-2-of-10M 
-rw-r--r-- 1 root root 10M Dec  5 00:17 file-2-of-10M
root@host:~# diff file-2-of-10M file-10M.dat 
```




## Task 8 | Trimitere FTP cu `curl` (cu autentificare)



> NU rula `$ ana-ftp-file-5M.dat`! (Mult text, not human-readable)



```sh
root@host:~# curl -o ana-ftp-file-5M.dat ftp://red/ana-ftp-file-5M.dat --user ana:student 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 5120k  100 5120k    0     0  96.8M      0 --:--:-- --:--:-- --:--:-- 98.0M
root@host:~# ls -alh ana-ftp-file-5M.dat 
-rw-r--r-- 1 root root 5.0M Dec  5 00:21 ana-ftp-file-5M.dat
root@host:~# # NU RULA: cat ana-ftp-file-5M.dat 
```


## Task 9 | Trimitere **e-mail** din linia de comanda (`mail`)


> Utilitarul `mail` foloseste **?** drept prompt.
> Asteapta interctiune la **stdin**.

```sh
root@host:~# su bogdan
bogdan@host:/root$ cd ~

# Tre' sa introduci date la stdin
bogdan@host:~$  mail -s "Invitatie la film" corina


root@host:~# su corina
corina@host:/root$ cd ~

# Afiseaza mail-ul, tot la fel, interactiune la stdin
corina@host:~$ mail
corina@host:~$ ls /home/corina/mbox
corina@host:~$ ls -alh /home/corina/mbox
corina@host:~$ cat /home/corina/mbox
```



```sh
bogdan@host:/root$ cd ~
bogdan@host:~$ echo 'Salut' | mail -s ", merge?" corina
bogdan@host:~$ su corina
corina@host:/home/bogdan$ cd ~
corina@host:~$ mail
```


```sh
# Trimit mail pe localhost (sie insasi/insusi)
corina@host:~$ echo "<Mesaj>" | mail -s "<Titlu>" corina
corina@host:~$ mail
```


```sh
bogdan@host:~$ echo "<Mesaj>" | mail -s "<Titlu>" corina
bogdan@host:~$ su corina
corina@host:/home/bogdan$ mail
```




## Task 10 | Trimitere **e-mail** cu adresa personalizata din linia de comanda (`mail`)


> Adresa de mail `nipasip578@luxyss.com` este o adresa temporara de mail
> (oferita de `https://temp-mail.org/en/`)


```sh
bogdan@host:~$ whoami
bogdan
bogdan@host:~$ echo 'Ba Bogdane, tocmai ai trimis cv din terminal\nIa vezi\n:))' | mail -s 'Use Of Linux' $GMAIL

bogdan@host:~$ mail
```


> Nu am putut sa trimit cu adresa mea de gmail;
> a blocat gmail-ul trimiterea.



Dar, am reusit dupa ce am obtinut o adresa de pe <https://temp-mail.org/en>.

```sh
bogdan@host:~$ echo 'Ba Bogdane, tocmai ai trimis cv din terminal\nIa vezi\n:))' | mail -s 'Use Of Linux' nipasip578@luxyss.com
bogdan@host:~$ echo 'MESAJ' | mail -s 'TITLU' nipasip578@luxyss.combogdan@host:~$ mail
No mail for bogdan
```

```sh
bogdan@host:~$ which s-nail
/usr/bin/s-nail

bogdan@host:~$ echo -e "MESAJ" | s-nail -r "bogdan.georgescu@rl.cs.pub.ro" -s "TITLU" nipasip578@luxyss.com

bogdan@host:~$ echo -e "Luni in intervalul 18:00-20:00 are loc un minunat workshop de Rust (da da da). E de Rust. E rapid (blazzingly fast). Stim cu toti cine preda" | s-nail -r "bogdan.georgescu@rl.cs.pub.ro" -s "Workshop" nipasip578@luxyss.com
bogdan@host:~$ 
```


## Task 11 (Bonus) | Upload prin FTP


### Task 11 | Uploadare de fisier

```sh
student@host:~$ su bogdan

bogdan@host:/home/student$ cd ~

bogdan@host:~$ ls
bogdan-ftp-data-3M.dat

bogdan@host:~$ whoami
bogdan

bogdan@host:~$ curl -T ~/bogdan-ftp-data-3M.dat ftp://red/ --user ana:student
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3072k    0     0  100 3072k      0  66.0M --:--:-- --:--:-- --:--:-- 66.6M
```


Verificare:

```sh
root@host:/home/student# go red

student@red:~$ su ana

ana@red:/home/student$ cd ~

ana@red:~$ ls -1
ana-ftp-file-5M.dat
bogdan-ftp-data-3M.dat
```


### Task 11 | Descarcare fisier uploadat

```sh
student@host:~$ su corina
corina@host:/home/student$ cd ~

corina@host:~$ curl -o ./bogdan-ftp-data-3M.dat ftp://red/bogdan-ftp-data-3M.dat --user ana:student
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3072k  100 3072k    0     0  65.6M      0 --:--:-- --:--:-- --:--:-- 66.6M

corina@host:~$ ls -1

bogdan-ftp-data-3M.dat

corina@host:~$ rm bogdan-ftp-data-3M.dat

corina@host:~$ curl -o ./bogdan-ftp-data-3M.dat ftp://red//home/ana/bogdan-ftp-data-3M.dat --user ana:student
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3072k  100 3072k    0     0  65.9M      0 --:--:-- --:--:-- --:--:-- 66.6M
```


Verificare:


```sh
root@host:/home/student# go red

student@red:~$ su ana

ana@red:/home/student$ cd ~

ana@red:~$ whoami
ana

ana@red:~$ la -1
ana-ftp-file-5M.dat
.bash_history
.bash_logout
.bashrc
bogdan-ftp-data-3M.dat
.profile

ana@red:~$ ls /home/ana/bogdan-ftp-data-3M.dat 
/home/ana/bogdan-ftp-data-3M.dat

ana@red:~$ ls -1
ana-ftp-file-5M.dat
bogdan-ftp-data-3M.dat
```



## Task 12 (Bonus) | Trimitere **e-mail** cu atasament din linia de comanda


```sh
root@host:~# apt-get install sharutils

root@host:~# uuencode store.zip store.zip > arhiva_store.uue

root@host:~# echo  "Citeste cu atentie arhiva\nArhiva nu stiu ce contine\nChiar nu\n" | mail -s "Assignment Archive" nipasip578@luxyss.com < arhiva_store.uue
```



```sh
$ uuencode file file > file.uue

$ echo <MESAJ> | mail -s <TITLU> <mail-address> < file.uue
```