# Laborator 07


```sh
student@host:~# update_lab --force
student@host:~# start_lab lab-iptables
```


## Task 01 | Conectare SSH folosind cheie publica


> Tot ce a trebuit sa fac a fost sa rulez comenzile (copy paste direct).

```sh
student@red:~$ ssh student@host
student@red:~$ ssh -l student host   # La fel ca mai sus
student@red:~$ cat ~/.ssh/id_rsa.pub
student@red:~$ ssh -l student host "cat ~/.ssh/authorized_keys"
```


## Task 02 | Generare cheie publica si autentificare


```sh
corina@blue:~$ # Va genera automat, fara sa mai puna intrebari (in stdin)
corina@blue:~$ ssh-keygen -t rsa -f ~/.ssh/id_rsa -N ""
```
```
Generating public/private rsa key pair.
Created directory '/home/corina/.ssh'.
Your identification has been saved in /home/corina/.ssh/id_rsa
Your public key has been saved in /home/corina/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:Q5NgDifhHuRowGD9+Lgm8l/ZqeLfne3BLB+5BeZd84A corina@blue
The key's randomart image is:
+---[RSA 3072]----+
|=.. =.+          |
|.o * * . .       |
|  o * . +        |
| . o o . .   .   |
|    +   S   E ...|
|   . . o o = + oo|
|    . o o . B o .|
|.. o.. o . = =   |
|..+oooo . o.=    |
+----[SHA256]-----+
```
```sh
corina@blue:~$ cat ~/.ssh/id_rsa.pub   # O copiez in student@host:~/.ssh/authorized_keys
```
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/WCRqCRamkGgX+cYds/LtxkapEFHGUYKYiafed9C6wMoAIeIvACwD9+JEkPaU96inoVxWPLAgkUjtXG6QHQ8KafXCp/YnvedqBDAwZd1jfCuU4fIEOA7NXB2+xL2x2YAyA1JBm25ELEOrs+cra40R2JgSwd6/xOOLdbpIV5CsRdVJEIvum3H9FbzdXoX6oeza1GbCUxemRtvmEDTHCovfNZAuhQiff+lGaYlveMPrWvIy9qZexjsee/lIYbDKsCA9ZfgE6RInOVuOJu0w/F+TMAivzmUOyTc+2ACBtaHU2LgducM6AB+kUHbSerzf5Oz0yGSWMgfVSiJmZT0q9n/jGS9w0IvVUcgWM5oxwusmYkCY6t1HYHU0zDM/ZHTsD1fAWvnI6wkbcQVpWtGBhQTEyQ80hJedW66gJN1uPjx9b5whOjorTrXQZZqCGXPRh3sMCabQy8AA2HGCxDxfLxPVeku5HX+/YQoTRT16vPYYRJtmhVfRZIydrHrDk/Cxyp0= corina@blue
```


```sh
student@host:~$ nano -l ~/.ssh/authorized_keys
student@host:~$ grep --color=ALWAYS 'corina@blue' ~/.ssh/authorized_keys 
```
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/WCRqCRamkGgX+cYds/LtxkapEFHGUYKYiafed9C6wMoAIeIvACwD9+JEkPaU96inoVxWPLAgkUjtXG6QHQ8KafXCp/YnvedqBDAwZd1jfCuU4fIEOA7NXB2+xL2x2YAyA1JBm25ELEOrs+cra40R2JgSwd6/xOOLdbpIV5CsRdVJEIvum3H9FbzdXoX6oeza1GbCUxemRtvmEDTHCovfNZAuhQiff+lGaYlveMPrWvIy9qZexjsee/lIYbDKsCA9ZfgE6RInOVuOJu0w/F+TMAivzmUOyTc+2ACBtaHU2LgducM6AB+kUHbSerzf5Oz0yGSWMgfVSiJmZT0q9n/jGS9w0IvVUcgWM5oxwusmYkCY6t1HYHU0zDM/ZHTsD1fAWvnI6wkbcQVpWtGBhQTEyQ80hJedW66gJN1uPjx9b5whOjorTrXQZZqCGXPRh3sMCabQy8AA2HGCxDxfLxPVeku5HX+/YQoTRT16vPYYRJtmhVfRZIydrHrDk/Cxyp0= corina@blue
```


Verificare
```sh
corina@blue:~$ ssh student@host
```


> La prima autentificare cu `ssh`,
> ma va intreba "Are you sure you want to continue connecting".
> Raspund cu "yes", iar altadata nu va mai intreba nimic (se va conecta automat).


## Task 03 | Download si upload de director folosind `scp`


### Task 03 | Download prin `scp` (from remote to local)

**Descarc** directorul `assignment/` din directorul `home` al utilizatorului `student` de pe statia `host`.

```sh
corina@blue:~$ scp -r student@host:~/assignment student-assignment
```

```sh
corina@blue:~$ scp -r student@host:~/assignment student-assignment
linear.txt                                                             100%   10    15.7KB/s   00:00    
cubic.txt                                                              100%   24    50.2KB/s   00:00    
quadratic.txt                                                          100%   17    39.9KB/s   00:00    
corina@blue:~$ ls
blue-file-10M.dat  solution  student-ass*ignment
corina@blue:~$ cd student-assignment/
corina@blue:~/student-assignment$ ls
cubic.txt  linear.txt  quadratic.txt
corina@blue:~/student-assignment$ cat cubic.txt 
x^3 - 6x^2 + 11x -6 = 0
corina@blue:~/student-assignment$ cat linear.txt 
x - 1 = 0
corina@blue:~/student-assignment$ cat quadratic.txt 
x^2 - 3x + 2 = 0
```



### Task 03 | Upload prin `scp` (from local to remote)


**Uploadez** direcorul `solution/` in directorul `home` al utilizatorului `student` de pe `host`.


```sh
corina@blue:~$ scp -r student@host:~/assignment student-assignment
```

```sh
corina@blue:~$ scp -r student@host:~/assignment student-assignment
linear.txt                                                             100%   10    15.7KB/s   00:00    
cubic.txt                                                              100%   24    50.2KB/s   00:00    
quadratic.txt                                                          100%   17    39.9KB/s   00:00  

student@host:~$ ls
assignment  host-file-10M.dat  pwndbg  solution
student@host:~$ cd solution/
student@host:~/solution$ ls
cubic.txt  linear.txt  quadratic.txt
student@host:~/solution$ cat cubic.txt 
x1 = 1, x2 = 2, x3 = 3
student@host:~/solution$ cat linear.txt 
x = 1
student@host:~/solution$ cat quadratic.txt 
x1 = 1, x2 = 2
```



## Task 04 | Copiere fisiere cu diverse protocoale: durata si consum de resurse


### Task 04 | Transfer prin `netcat` (a.k.a `nc`)

```sh
# Terminal 1
student@host:~$ nc -l 12345 > file-100M-nc.dat
```

```sh
# Terminal 2
student@green:~$ time cat file-100M.dat | nc -q0 host 12345

real	0m0.225s
user	0m0.011s
sys	0m0.081s
```




Verific **hash**-urile:

```sh
# Terminal 1
student@host:~$ sha512sum file-100M-nc.dat 
0fd2d103367c010b4f21ab2c6d1be7adf888e186f0e19363d8e19dbbfef1b491540566894a82f2d701f108a6aff589e70286537da9641f076639626058c38614  file-100M-nc.dat
```


```sh
# Terminal 2
student@green:~$ sha512sum file-100M.dat 
0fd2d103367c010b4f21ab2c6d1be7adf888e186f0e19363d8e19dbbfef1b491540566894a82f2d701f108a6aff589e70286537da9641f076639626058c38614  file-100M.dat
```



### Task 04 | Transfer prin FTP



```sh
student@green:~$ time curl -T file-100M.dat -u student:student ftp://host/file-100M-ftp.dat

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  100M    0     0  100  100M      0   278M --:--:-- --:--:-- --:--:--  279M

real	0m0.505s
user	0m0.004s
sys	0m0.048s
```


### Task 04 | Transfer prin SSH


```sh
student@green:~$ time scp file-100M.dat student@host:file-100M-scp.dat

file-100M.dat                                                          100%  100MB 181.5MB/s   00:00    

real	0m0.714s
user	0m0.164s
sys	0m0.083s
```


## Task 05 | Trafic criptat si necriptat


TODO: continua
