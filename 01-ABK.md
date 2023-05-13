Cyberbezpieczeństwo, I Stopień, I Semestr

## Podstawowe komendy cz. 1:

### Zadania:

##### 1. Przy pomocy komendy cat obejrzyj zawartość pliku /etc/passwd. Wykonaj komendy cat /etc/passwd i cat /etc/passwd | more.Porównaj ich działanie.

```console
$ cat /etc/passwd  # wyświetli nam zawartość pliku /etc/passwd
```
```console
$ cat /etc/passwd | more  # umożliwi nam przewijanie zawartości plik
```

##### 2. Do czego służy komenda less? Zastosuj ją do pliku /etc/group. Sprawdź działanie komendy less dla pliku /etc/passwd dla wyszukania wszystkich napisów 12.

```console
$ cat /etc/group | less  # wyświeli nam plik, nie wpisując go do terminala i pozwoli przewijać go spacją
```

```console
$ cat /etc/group | less -p12  # wyróżni nam wszystkie '12' zawarte w pliku
```

##### 3. Korzystając z komend head oraz tail, wypisz 17 pierwszych oraz21 ostatnich linii pliku /etc/passwd. Jak wypisać 27. linię pliku /etc/passwd?

```console
$ cat /etc/passwd | tail -n21  # wypisze 21 ostatnich lini
```

```console
$ cat /etc/passwd | tail -n21  # wypisze 21 ostatnich lini
```

```console
$ cat /etc/passwd | head -n27 | tail -n1  # wypisze 27 linię
```

##### 4. Sprawdź w manualu składnię oraz przeznaczenie komendy wc.

```console
$ man wc  # liczy ilość znaków, wyrazów, lini
```

##### 5.Do czego służy komenda uniq?

```console
$ man uniq  # informuje lub pomija powtórzone linie
```

##### 6. Do czego służy komenda cal? Jakie są jej opcje i argumenty? Sprawdź, w jaki dzień tygodnia wypada Boże Narodzenie w tym roku. Ile dni miał miesiąc wrzesień w 1752 roku?

```console
$ man cal
```

```console
$ cal 12 2021  # wypisze kalendarz na grudzień 2021
```

```console
$ cal 9 1752   # wypisze kalendarz na wrzesień 1752. Wrzesień wtedy miał 19 dni
```

##### 7. Jaką komendą uruchamia się kalkulator (w konsoli)?

```console
$ apropos calculator  
```
	mate-calc (1)        - (mate-calculator) - The MATE Desktop Environment Calculator
	mate-calc-cmd (1)    - A console calculator for the MATE Desktop Environment.
	mate-calculator (1)  - (mate-calculator) - The MATE Desktop Environment Calculator
	xcalc (1)            - scientific calculator for X


##### 8. Do czego służy komenda pwd (sprawdź w manualu). Zobacz, w którym miejscu drzewa katalogów aktualnie się znajdujesz.

```console
$ man pwd  # wypisuje w którym miejscu drzewa katalogów aktualnie znajduje się użytkownik
```

```console
$ pwd  # /home/julia
```

##### 9. Do czego służy komenda cd? Przejdź do katalogu głównego (katalog root, /), do katalogu /tmp, /usr.  Przejdź do katalogu osobistego, używając minimalnej liczby znaków w linii komend.

```console
$ man cd  # przenosi użytkownika do danego katalogu

$ cd /  # przeniesie do katalogu głównego

$ cd /tmp  # przeniesie do katalogu /tmp

$ cd /usr  # przeniesie do katalogu /usr

$ cd ~  # przeniesie do katalogu osobistego
```

##### 10. Do czego służy komenda ls? Sprawdź parametry jej wywołania. Sprawdź zawartość katalogu /tmp, /usr/include, katalogu osobistego. Jakie znaczenie mają opcje –l, -d, -i, -t, -R?

```console
$ man ls  # komenda 'ls' listuje zawartość katalogu

$ ls /tmp  # wylistuje zawartość folderu /tmp

$ ls /usr/include  # wylistuje zawartość katalogu /usr/include
```
     -l - listuje szczegóły zawartości np. uprawnienia
     -d - zapobiega listowaniu folder
     -i - pokazuje numer każdego pliku
     -t - listuje foldery sortując według czasu modyfikacji
     -R - wyświetlanie podkatalogów rekursywnie

##### 11. Do czego służy komenda echo? Sprawdź wartość zmiennych środowiskowych HOME, PATH oraz PS1, korzystając z komendy echo.

```console
$ echo $HOME 
```
	/home/julia

```console
$ echo $PATH 
```
	/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games

```console
$ echo $PS1
``` 
	\[\e]0;\u@\h: \w\a\]\[\033[;32m\]┌──${debian_chroot:+($debian_chroot)──}${VIRTUAL_ENV:+(\
	[\033[0;1m\]$(basename $VIRTUAL_ENV)\[\033[;32m\])}(\[\033[1;34m\]\u${prompt_symbol}\h\[\
	033[;32m\])-[\[\033[0;1m\]\w\[\033[;32m\]]\n\[\033[;32m\]└─\[\033[1;34m\]\$\[\033[0m\]

##### 12. Gdzie w systemie zapisywana jest informacja o wszystkich (udanych i nieudanych) podłączeniach do systemu. Zapoznaj się z komendą last.

```console
$ last  # wypisze podłączenia do systemu
```

##### 13. Do czego służy komenda who? Jakie są opcje i argumenty jej wywołania? Jakich informacji dostarcza komenda w?

```console
$ who  # pokazuje kto aktualnie jest zalogowany
```
```console
$ w  # dodatkowo pokazuje statystyki użycia procesora i czas zalogowania
```

##### 14. Sprawdź jakich informacji dostarcza komenda id. Jaki jest twój numer identyfikacyjny w systemie (UID), jaki grupy podstawowej (GUID), do jakich grup należysz?

```console
$ id  # wyświetla id wskazanego użytkownika/grupy. Gdy żaden nie jest wskazany, wyświetla dla obecnego użytkownika
```

	uid=1001(julia) gid=1001(julia) groups=1001(julia),27(sudo)

### Postacie lini komend:
##### 1. Ilu użytkowników lokalnych jest zdefiniowanych w systemie?

```console
$ cat /etc/passwd | wc -l   # 55
```

##### 2. Jak w systemie opisany został (zawartość 5-tej kolumny pliku /etc/passwd) użytkownik, którego numer identyfikacyjny wynosi 14?

```console
$ awk -F: '{ if ($3 == 14) { print $5 } }' /etc/passwd  # nic nie wypisało, użytkownik o takim numerze nie istnieje
```


##### 3.Ilu jest użytkowników w systemie, których grupa podstawowa jest Twoją grupą?

```console
$ cat /etc/passwd | cut -d: -f4 | grep ”$(id -g)” |wc -l  # 0
```


##### 4. Ilu użytkowników zdefiniowanych w systemie używa jako podstawowego interpretera poleceń interpretera Bash?

```console
$ awk -F: '{ if($7 == "/bin/bash") { print $7 } }' /etc/passwd | wc -l   # 1
```


##### 5. Ilu użytkowników w systemie posiada numer identyfikacyjny UID większy od 12000?

```console
$ awk -F: '{ if($3 > 12000) {print} }' /etc/passwd | wc -l   # 1 
```                            


##### 6.Jak wylistować zawartość kilku katalogów na raz (w jednej komendzie)?

```console
$ ls -R 
```


##### 7. Jak wypisać za pomocą jednej komendy echo cyfry od 0 do 5, każdą w osobnej linii?

```console
$ echo -e "0\n1\n2\n3\n4\n5" 
```
	0
	1
	2
	3
	4
	5


### Zadania sprawdzające

##### 1. Ile plików i katalogów znajduje się w katalogu głównym (/)?

```console
$ ls / | wc -w   # 26
```


##### 2. Ilu użytkowników jest aktualnie zalogowanych?

```console
$ who | wc -l   # 1
```

##### 3. Ile słów liczy plik /usr/include/utime.h?

```console
$ cat /usr/include/utime.h | wc -w   # 224
```



