Cyberbezpieczeństwo, I Stopień, I Semestr

## Procesy

### Zadania:
##### 1.

```console
$ ps -ef
```

PID - numer procesu
PPID - numer procesu rodzica
STIME - czas startu
%MEM - zajęcie pamięci
TTY - terminal
COMMAND/CMD - komenda wywołująca proces

Opcje Unixowe są poprzedzane myślnikiem, a BSD nie


##### 2. Zaproponuj postać komendy pozwalającej zwrócić informacje o właścicielu procesu, numerze PID, wartościach priorytetu i parametru NICE oraz z jakiego terminala korzysta proces. W jaki sposób można specyfikować oczekiwane kolumny?

```console
$ ps -eo user,pid,pri,ni,tty
```

##### 3.
```console
$ mkdir c3 # tworzę katalog c3

$ nano ./prog.c # tworzę plik i zapisuję kod

$ ./prog & # uruchammiam program w tle

$ ps -o command,pid,pri,ni
```

	COMMAND                         PID PRI  NI
	/bin/bash                     24145  19   0
	./prog                        24188  19   0
	ps -o command,pid,pri,ni      26706  19   0

	$ renice 19 24188
	COMMAND                         PID PRI  NI
	/bin/bash                     24145  19   0
	./prog                        24188   0  19

Przy parametrze NICE 19 \%CPU wynosi 100, a przy -20 również 100

PRI = 19 - NICE

	$ jobs -> 
	[1]+  Running                 ./prog &

	$ kill 24188

Proces posiada NICE 0, \%CPU 99.7

NICE można zmieniać w zakresie od -20 do 19


##### 4. Uruchom program prog jeszcze raz, jako pierwszoplanowy. Zobacz, jak wygląda jego wpis w wynikach polecenia ps. Wyloguj się i zaloguj ponownie. Czy proces nadal działa?

```console
$ ./prog
```
Po wylogowaniu się, proces przestał działać
##### 5. Powtórz powyższą operację, ale uruchom proces w tle. Czy po wylogowaniu i zalogowaniu proces dalej istnieje?

Po wylogowaniu, proces przestał działać

##### 6. Uruchom program prog w tle tak, aby wykonywał się on mimo odłączenia się od systemu. Gdzie szukać tego, co program wypisuje na ekran? Usuń uruchomiony proces.

```console
$ nohup ./prog &

$ pkill prog
```

##### 7. Uruchom program prog w tle. Wyślij do niego sygnał SIGSTOP. Jaki jest stan uruchomionego procesu? Jak z czasem zmienia się stopień wykorzystania procesora przez ten proces? Do zatrzymanego procesu wyślij sygnał SIGCONT. Jaki jest teraz stan procesu? Co dzieje się z wartością w kolumnie opisującej stopień wykorzystania procesora? Usuń uruchomiony proces.

```console
$ kill -s SIGSTOP 24188 
```
Stan zmienił się na T, CPU się z czasem zmniejsza

```console
$ kill -s SIGCONT 24188 
```
Stan zmienił się na R, CPU zwiększa się z czasem

SIGQUIT domyślnie powoduje zakończenie procesu z zapisem obrazu pamięci.
SIGTSTP domyślnie powoduje zatrzymanie procesu.
SIGTERM domyślnie powoduje zakończenie procesu.
Jednak po wysłaniu tych sygnałów program nadal działał
Dopiero sygnał SIGKILL zabił proces ( $ pkill -9 signal )


##### 9. Podaj PID procesu zastopowanego, który zużywa najwięcej pamięci wirtualnej.

```console
$ ps -eo state,vsz,pid | awk '$1 == "T" {print $2, $3}' | sort -n | tail -n1 | awk '{print $2}' 
```
24188

##### 10. Podaj nazwę użytkownika, który ma najwięcej procesów z zerową wartością parametru NICE.

```console
$ ps -eo nice,user |sort -n | uniq -c | awk '$2 == 0 {print $1, $3}' | sort -n | tail -n1 
```
58 julia

##### 11. Podaj numery PID trzech procesów, które mają najwięcej pamięci wirtualnej.

```console
$ ps -eo vsz,pid | sort -n | tail -n3 | awk '{print $2}' 
``` 
583
24142
936

### Zadania sprawdzające:
##### 1. Jak wypisać wszystkie dostępne nazwy kolumn dla komendy ps?

```console
$ ps L
```
##### 2. Jak wyświetlić procesy użytkownika root posortowane wg rozmiaru zużytej pamięci (np. size), używając tylko ps?

```console
$ ps -o user,size,command --user 0 --sort size 
```
	
	USER      SIZE COMMAND
	root         0 [kthreadd]
	root         0 [rcu_gp]
	root         0 [rcu_par_gp]
	root         0 [kworker/0:0H-events_highpri]
	root         0 [mm_percpu_wq]
	root         0 [rcu_tasks_rude_]
	root         0 [rcu_tasks_trace]
	root         0 [ksoftirqd/0]
	...

##### 3. Ile jest procesów, których ojcem jest proces o PID równym 1?

```console
$ ps -eo ppid | sort | uniq -c | awk '$2 == 1 {print $1}'
```
25

##### 4.Znajdź użytkownika w systemie, który ma uruchomioną największą liczbę procesów.

```console
$ ps -eo user,state | awk '$2 == "S" {print $1, $2}' | sort | uniq -c | sort -n | tail -n1 | awk '{print $2}' 
```
root

##### 5. Znajdź proces, który zużył najwięcej czasu procesora.

```console
$ ps -eo time,pid,command --sort time | tail -n1 -
```
	00:10:50   24188 ./prog

##### 6. Ile procesów potomnych ma bieżący terminal? Nie uwzględniaj komendy, która dokonuje obliczeń.

```console
$ ps -eF | awk '{print $1, $3}' | grep $$ | wc -l 
```
5
