Cyberbezpieczeństwo, I Stopień, I Semestr

## Podstawowe komendy cz. 2:

### Zadania:
##### 1. Zapoznaj się z poleceniami tr oraz cut. W jaki sposób mogą one zastąpić podstawowe funkcje dostarczane przez awk?

' tr ' - usuwa lub zamienia litery
' cut ' - wybiera wybrane części z lini


##### 2.Jak w systemie opisany został (zawartość piątej kolumny pliku /etc/passwd) użytkownik, którego numer identyfikacyjny (UID) wynosi 14?

```console
$ awk -F: '{ if ($3 == 14) { print $5 } }' /etc/passwd 
```
	nic nie wypisało, użytkownik o takim numerze nie istnieje


##### 3. Ilu użytkowników zdefiniowanych w systemie używa jako podstawowego interpretera poleceń interpretera Bash?

```console
$ awk -F: '{ if($7 == "/bin/bash") { print $7 } }' /etc/passwd | wc -l   # 1
```

##### 4. Ilu użytkowników w systemie posiada numer identyfikacyjny (UID) większy od 20?

```console
$ awk -F: '{ if($3 > 20) { print $3 } }' /etc/passwd | wc -l   # 22
```

##### 5. Jaki jest największy numer grupy lokalnej w systemie?

```console
$ awk -F: '{ print $4 }' /etc/passwd | sort -n | tail -n1   # 65534
```

##### 6. Ile jest w systemie grup, które nie są grupą podstawową dla ani jednego jednego użytkownika?

```console
$ expr `cat /etc/group | wc -l` - `awk -F: '{ print $4 }' /etc/passwd |sort |uniq |wc -l   # 29
```

##### 7. Ile grup w systemie nie jest grupami dodatkowymi dla żadnego użytkownika?

```console
$ awk -F: '{ if($4 == "") { print $4 } }' /etc/group |wc -l   # 70
```

##### 8. Ilu jest użytkowników w systemie, których grupa podstawowa jest Twoją grupą?

```console
$ cat /etc/passwd | cut -d: -f4 | grep ”$(id -g)” | wc -l   # 0
```

##### 9. Wypisz użytkowników, których nazwa składa się z dokładnie 8 znaków.

```console
$ cat /etc/passwd |cut -d: -f1 |grep -E ”^.{8}$” |wc -l 
```
	www-data
	redsocks
	postgres
	stunnel4


##### 10. W katalogu domowym utwórz plik zawierający strony manuala dla komendy ls. Jakie są różnice w działaniu operatorów przekierowania > oraz >>?

```console
$ man ls > ~/plik.txt
```
	> - nadpisuje plik
	>> - dopisuje do pliku


### Operacje na plikach:
##### 1. Zapoznaj się z poleceniami mkdir, rmdir oraz ls. Następnie, wykorzystując jedno wywołanie polecenia mkdir, utwórz w swoim katalogu osobistym strukturę katalogów tak, aby poprawne były następujące ścieżki:

```console
$ mkdir -p $HOME/c2/{text,bin}
```

##### 2. Sprawdź prawa dostępu katalogów. Co decyduje o ich postaci? Czy stojąc w katalogu $HOME jednym wywołaniem komendy ls można obejrzeć zawartość wszystkich katalogów do samego końca drzewa plików? Sprawdź to w manualu.

```console
$ ls -lR   # wypisze zawartość katalogów
```

##### 3. Sprawdź prawa dostępu utworzonego w poprzednim zadaniu pliku. Zapoznaj się z poleceniami chmod i chown. Zmień prawa dostępu pliku tak, aby tylko jego właściciel indywidualny mógł go odczytać i zapisać.

```console
$ ls -l 
``` 

	-rw-r--r-- 1 julia julia  8236 Jan  3 16:15 plik.txt

```console
$ chmod 600 ~/plik.txt 
```

	-rw------- 1 julia julia  8236 Jan  8 16:15 plik.txt


##### 4. Ustaw prawa dostępu pliku i katalogu, w którym plik się znajduje, tak, aby pliku nie można było usunąć komendą rm. Skopiuj plik na dowolny inny. Jakie są prawa dostępu nowego pliku? Zmień prawa dostępu nowego pliku tak, aby tylko właściciel indywidualny i inni członkowie jego grupy podstawowej mogli go odczytać.

```console
$ touch ~/c2/test.txt   # tworzę plik w folderze c2

$ chmod 555 ~/c2   # zmieniam jego prawa dostępu

$ cp test.txt ~   # kopiuję plik do katalogu $HOME, prawa dostępu są takie same

$ chmod 440 ~/test.txt   # zmieniam jego prawa dostępu
```

##### 5. Skopiuj z katalogu /bin do swojego katalogu ~/c2/bin plik ls, zmieniając jego nazwę na moj_ls. W jaki sposób można skopiować cały katalog /usr/include/scsi?

```console
$ cp /bin/ls c2/bin/moj_ls

$ cp -R /usr/include/scsi destination
```

### Zadania sprawdzające:
##### 1. Ile jest takich grup, które są grupami podstawowymi dla dwóch lub trzech użytkowników?

```console
$ cat /etc/passwd | cut -d: -f4 | sort -n | uniq -c | awk '$1 ~ /[23]/' | wc -l   # 0
```

##### 2. Podaj liczbę lokalnych użytkowników, którzy nie mogą się zalogować (/sbin/nologin jako shell)?

```console
$ cat /etc/passwd | cut -d: -f7 | grep /sbin/nologin | wc -l   # 45
```


##### 3. Ile jest osób w /etc/passwd o nazwisku Nowak?

```console
$ cat /etc/passwd | cut -d: -f5 | grep Nowak | wc -l   # 0
```

#### 4. Ilu użytkowników było jednorazowo zalogowanych co najmniej godzinę (wykorzystaj wyniki polecenia last)?

```console
$ last | awk '{print $NF}' | awk -F '[(:]' '$2 >=1 {print $2}' | wc -l   # 17
```

