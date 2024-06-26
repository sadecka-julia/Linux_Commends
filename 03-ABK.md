Cyberbezpieczeństwo, I Stopień, I Semestr

## Operacje w systemie plików

### Zadania:
##### 1. Czy znajdując się w katalogu $HOME, jednym wywołaniem komendy ls można obejrzeć zawartość wszystkich katalogów do samego końca drzewa plików (czyli w sposób rekursywny)? Sprawdź to w manualu.

```console
$ ls -R
```

##### 2. Ustal, w jaki sposób oznaczane są ustawione uprawnienia dodatkowe (SUID, SGID i SVTX).


SUID - dla właściciela w miejscu x (S)
SGID - dla grupy w miejscu x (S)
SVTX (sticky bit) - dla pozostałych użytkowników w miejscu x (S)


##### 3. W katalogu $HOME/c2/text utwórz plik zawierający strony podręcznika dla komendy ls. Sprawdź prawa dostępu utworzonego pliku. W jaki sposób i przez kogo mogą zostać one zmienione?

```console
$ man ls > $HOME/c2/text/ls.txt

$ ls -l
```
	-rw-r--r-- 1 julia julia 8082 Jan  6 16:55 ls.txt
Plik może zostać zmieniony i odczytany przez właściciela i grupę, a przez resztę może być tylko odczytany. Prawa mogą zostać zmienione przez właściciela pliku i grupę


##### 4.Zmień prawa dostępu do utworzonego pliku tak, aby tylko jego właściciel indywidualny mógł go odczytać i zapisać. Ustaw prawa dostępu pliku i katalogu, w którym plik się znajduje tak, aby pliku niemożna było usunąć komendą rm.

```console
$ chmod 600 $HOME/c2/text/ls.txt 
```
zmienimy prawa dostępu, aby tylko jego właściciel indywidualny mógł go odczytać i zapisać

```console
$ chmod -p 500 $HOME/c2/text 
```

##### 5. Skopiuj plik na dowolny inny. Jakie są prawa dostępu nowego pliku? Zmień prawa dostępu nowego pliku tak, aby tylko właściciel indywidualny i inni członkowie jego grupy podstawowej mogli go odczytać. Prawa są takie same jak kopiowanego pliku

```console
$ cp ls.txt ls2.txt
```
Kopiujemy plik ls.txt na plik ls2.txt

```console
$ chmod 440 $HOME/c2/text/ls2.txt
```
Zmieniamy prawa dostępu


##### 6. W jaki sposób ustawiane są domyślne prawa do tworzonych przez użytkownika plików? Jak to zmienić?

Możemy zmienić za pomocą polecenia umask, gdy zmienimy zmienne maski,
wtedy ustawienie praw domyślnych będzie inne. Domyślna maska to 022.


##### 7. Podaj nazwę pliku w katalogu domowym (rekursywnie) o największym rozmiarze. Jaki jest największy plik tylko w katalogu domowym i jego bezpośrednich podkatalogach?

```console
$ find ~ -type f -exec du -b {} + | sort -n | tail -1
```
```console
	/home/julia/.mozilla/firefox/wir19nnc.default-esr/storage
	/permanent/chrome/idb/3870112724rsegmnoittet-es.sqlite
```

##### 8. Podaj nazwę pliku w katalogu domowym (rekursywnie) o największej ilości bloków.

```console
$ find ~ -type f -exec du {} + | sort -n | tail -1
```

	13540   /home/julia/.mozilla/firefox/wir19nnc.default-esr
	/storage/permanent/chrome/idb/3870112724rsegmnoittet-es.sqlite


##### 9. Korzystając z komendy find, wyszukaj i wylistuj w postaci długiej wszystkie pliki regularne, które znajdują się w katalogu /usr/include i jego podkatalogach, a ich nazwa zaczyna się na literę c.

```console
$ find /usr/include -type f -iname "c*" -exec ls -l {} +
```
	-rw-r--r-- 1 root root   5149 Aug 25 07:28 /usr/include/aircrack-ng/adt/circular_buffer.h
	-rw-r--r-- 1 root root   5479 Aug 25 07:28 /usr/include/aircrack-ng/adt/circular_queue.h
	-rw-r--r-- 1 root root   8520 Aug 25 07:28 /usr/include/aircrack-ng/ce-wpa/crypto_engine.h
	-rw-r--r-- 1 root root   2147 Aug 25 07:28 /usr/include/aircrack-ng/compat.h
	-rw-r--r-- 1 root root   2319 Aug 25 07:28 /usr/include/aircrack-ng/cowpatty/cowpatty.h
	-rw-r--r-- 1 root root   2381 Aug 25 07:28 /usr/include/aircrack-ng/cpu/cpuset.h
	-rw-r--r-- 1 root root  10559 Aug 25 07:28 /usr/include/aircrack-ng/crypto/crctable.h
	-rw-r--r-- 1 root root   7068 Aug 25 07:28 /usr/include/aircrack-ng/crypto/crypto.h
	-rw-r--r-- 1 root root   1029 Aug 25 07:28 /usr/include/aircrack-ng/osdep/channel.h
	-rw-r--r-- 1 root root   2392 Aug 25 07:28 /usr/include/aircrack-ng/osdep/common.h
	-rw-r--r-- 1 root root  10316 Aug 25 07:28 /usr/include/aircrack-ng/support/common.h
	-rw-r--r-- 1 root root   8854 Aug 25 07:28 /usr/include/aircrack-ng/support/communications.h
	...


##### 10. Czy w Twoim katalogu domowym (z podkatalogami) są pliki modyfikowane wcześniej niż 2 dni temu? Jak to sprawdzić? Wylistuj je w postaci długiej. Zrób to raz z uwzględnieniem podkatalogów, a raz – bez.

```console
$ find ~ -type f -mtime +2 -exec ls -l {} +

$ find ~ -maxdeph 1 -type f -mtime +2 -exec ls -l {} +
```

	-rw-r--r-- 1 julia julia     220 Oct 14 17:23  /home/julia/.bash_logout
	-rw------- 1 julia julia   10571 Nov 15 16:38  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/001BAA9A757219DA028FCC0D5EF3717CB1462765
	-rw------- 1 julia julia   11800 Nov 15 16:22  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/001F048248CA332263CFFC08FC20F7FF1B020E79
	-rw------- 1 julia julia   14563 Nov 12 05:36  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/003F1E373DD7278CFE131A728839707536E52A8F
	-rw------- 1 julia julia   11079 Nov 12 17:05  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/00F4F9E30A2798CC9CCB265C1B1D3E6585843E19
	-rw------- 1 julia julia   11144 Nov 15 16:28  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/0108C21A2099A86DA163FF344FA8810D033BFAE6
	-rw------- 1 julia julia   11040 Nov  8 11:16  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/013B0DB7750286A737CFC5CA9BFA8A1438080961
	-rw------- 1 julia julia   10874 Nov 15 16:49  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/017BE3C98BFDA6DF51F0991F9D11ADAA2672ADEF
	-rw------- 1 julia julia   11790 Nov 15 17:25  /home/julia/.cache/mozilla/firefox/wir19nnc.default-esr/cache2/entries/018BE5BB5BA4AD3787BB302900A108602A6C90B1
	...


##### 11. Znajdź liczbę plików regularnych w /usr/include (bez podkatalogów) które zaczynają się na m i kończą na .h oraz których rozmiar nieprzekracza 12KB.

```console
$ find /usr/include -maxdepth 1 -type f -size -12k -iname ”m*.h” | wc -l 
```
7

##### 12. Przy pomocy komendy tar stwórz archiwum katalogu c2, a następnie dokonaj jego kompresji (np. przy pomocy programu gzip). Powtórz tę operację w jednym kroku z użyciem dwóch komend, a następnie jednej. Następnie usuń katalog c2, a następnie odtwórz go ze zbioru *.tar.gz (lub *.tgz).

```console
$ tar -cf archiwum.tar c2 && gzip archiwum.tar

$ tar -czf archiwum.tar.gz c2 # stworzy archiwum i jednocześnie dokona kompresji (-z)

$ rm -r c2 #usunie katalog c2

$ tar -xvf archiwum.tar.gz 
```
	c2/
	c2/text/ls.txt
	2/text/ls1.txt
	c2/text/ls2.txt
	c2/plik.txt
	c2/bin/


##### 13. Sprawdź, ile miejsca na dysku zajmuje twój katalog osobisty i jakie ograniczenia zostały nałożone na jego rozmiar (komenda quota).

```console
$ quota -u `id -un`

$ du -sh ~ #94M     /home/julia
```

##### 14. Zapoznaj się ze stronami manuala dla komend df i free. Określ rozmiary systemów plików dostępnych w systemie oraz stopień ich zajętości w odniesieniu do liczby bloków danych i liczby struktur i-node.

```console
$ df -i
```

	Filesystem      Inodes  IUsed   IFree IUse% Mounted on
	udev            629412    370  629042    1% /dev
	tmpfs           638456    576  637880    1% /run
	/dev/sda1      5185536 307264 4878272    6% /
	tmpfs           638456      1  638455    1% /dev/shm
	tmpfs           638456      3  638453    1% /run/lock
	tmpfs           127691     74  127617    1% /run/user/1001


##### 15. Zidentyfikuj punkty montowania oraz typy używanych systemów plików za pomocą komend mount i lsblk. Jak uzyskać numery seryjne fizycznych dysków?

```console
$ mount -l # pierwsza kolumna - nazwa, druga - mountpoint, trzecia - typ systemu plików

$ lsblk -o NAME,SERIAL 
```

	NAME   SERIAL
	sda    VB8d0ba88e-69d909eb
	├─sda1 
	├─sda2 
	└─sda5 
	sr0    VB2-01700376


##### 16. Korzystając z komend du oraz sort, zaproponuj takie ich powiązanie, które pozwoli wylistować w kolejności malejącej listę katalogów z katalogu /usr uporządkowaną według wykorzystania przestrzeni dyskowej przez te katalogi i ich podkatalogi.

```console
$ find /usr -maxdepth 1 -type d | grep -Ev ”^/usr$” | xargs -n1 du -s | sort -nr
```

##### 17.

```console
$ ln -s c2/bin/moj_ls c2/text/symbol 
$ ln c2/text/man_ls.txt c2/bin/twardy 
```

i-node dowiązania twardego jest takie samo jak oryginalnego pliku
i-node dowiązania symbolicznego jest większe niż oryginalnego pliku


### Zadania sprawdzające:
##### 1. Znajdź plik w swoim katalogu domowym, który był najdawniej modyfikowany.

```console
$ find ~ -type f -exec ls -lt {} + | tail -1 
```
	-rw-r--r-- 1 julia julia     345 Nov 19  2010 /home/julia/c3/signal.c


##### 2. Ile katalogów znajduje się w /usr/include?

```console
$ find /usr/include -maxdepth 1 -type d | wc -l 
```
47


##### 3. Ile jest plików w /usr/include (bez podkatalogów), których nazwa składa się z 6 liter, zaczyna się na m i kończy na .h?

```console
$ find /usr/include/ -maxdepth 1 -type f -iname "m*.h" | awk -F "/" 'length($4) == 6 {print $4}' | grep ^m | wc -l
```
6


##### 4. Ile jest plików regularnych w katalogu i podkatalogach /usr/include, które mają rozmiar większy niż 12000B.

```console
$ find /usr/include -type f -size +12000c | wc -l 
```
1050


##### 5. Ile jest plików w /usr/bin o wielkości do 1MB?

```console
$ find /usr/bin -type f -size -1024k | wc -l 
```
1824



