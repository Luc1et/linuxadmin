# Úkoly – Správa uživatelů, Sudo

## 🧠 Teoretická část

### A. Práce se soubory a výstupem

1. Co udělá příkaz `cat` a kdy je vhodné ho použít?
2. Jaký je rozdíl mezi `less` a `cat`?
3. K čemu slouží příkaz `head` a jak určíš, kolik řádků zobrazí?
4. Jak bys zjistil, kolik má soubor řádků nebo slov?
5. Jaký příkaz bys použil, pokud chceš zobrazit pouze posledních 10 řádků souboru?

### B. Uživatelé, skupiny a práva

1. Jaký je rozdíl mezi uživatelem a skupinou v Linuxu?
2. Co znamenají jednotlivé části výpisu oprávnění u souboru (např. `-rw-r--r--`)?
3. Jaký je rozdíl mezi vlastníkem souboru a skupinou souboru?
4. Co obsahují soubory `/etc/passwd` a `/etc/shadow`?
5. K čemu slouží příkaz `chmod` a jak se používá?
6. K čemu slouží příkazy `useradd`, `groupadd`, `usermod`, `passwd`, `userdel` a `groupdel`?


## 💻 Praktická část

### Úkol 1: Vytvoření skupin a uživatelů

1. Vytvoř skupiny:
   - `studenti`
   - `ucitele`
2. Vytvoř uživatele:
   - `eva` (člen skupiny `studenti`)
   - `adam` (člen skupiny `studenti`)
   - `petr` (člen skupiny `ucitele`)
3. Nastav každému uživateli heslo (např. `test123`).
4. Ověř, do jakých skupin uživatelé patří.
5. Přidej uživatele `eva` také do skupiny `ucitele`.
6. Smaž uživatele `adam` a skupinu `ucitele`.

### Úkol 2: Práva a přístup k souborům

1. Přepni se na uživatele `petr`.
2. V jeho domovské složce vytvoř soubor `poznamka.txt`.
3. Zobraz výpis oprávnění.
4. Nastav oprávnění tak, aby:
   - vlastník mohl číst i zapisovat,
   - skupina mohla pouze číst,
   - ostatní neměli žádný přístup.
5. Ověř, že změna proběhla správně.
6. Změň vlastníka souboru na uživatele `eva` a skupinu na `studenti`.
7. Ověř, že soubor má nového vlastníka i skupinu.

### Úkol 3: Sdílená složka pro skupinu

1. Jako `root` vytvoř složku `/home/spolecne`.
2. Nastav vlastníka `root` a skupinu `studenti`.
3. Nastav práva tak, aby:
   - vlastník měl práva `rw`,
   - skupina měla práva `rw`,
   - ostatní neměli žádná.
4. Nastav **setgid bit**, aby nově vytvořené soubory ve složce zdědily skupinu `studenti`:
   ```bash
   chmod g+s /home/spolecne
5. Ve složce `/home/spolecne` vytvoř soubor `zkouska.txt` jako uživatel `eva`.
6. Ověř, že soubor má skupinu `studenti`.
7. Přepni se na uživatele `petr` a zkontroluj, že může tento soubor číst i upravovat.

### Úkol 4: Struktura „Školní tým“

1. Znovu vytvoř skupinu `ucitele`.

2. Vytvoř uživatele:
   - `reditel`
   - `ucitelka`
   - `student1`
   - `student2`

3. Přiřaď uživatele do skupin:
   - `reditel`, `ucitelka` → `ucitele`
   - `student1`, `student2` → `studenti`

4. Vytvoř strukturu složek:
    ```console
    /opt/skola/
      ├── ucitele/
      └── studenti/
    ```
5. Nastav:
   - `/opt/skola/ucitele` → vlastník `root`, skupina `ucitele`, práva `770`
   - `/opt/skola/studenti` → vlastník `root`, skupina `studenti`, práva `770`
6. Do obou složek přidej soubor `info.txt` s krátkým textem *(např. „Vítejte ve složce učitelů“)*.

7. Ověř, že:
   - Učitelé mají přístup pouze do složky `ucitele`.
   - Studenti pouze do `studenti`.
   - Ostatní uživatelé nemají přístup do žádné z nich.


## Bonus: Sudo zapomíná

*(`sudo` ti dává oprávnění, ale i zodpovědnost. Nauč se před každým použitím `sudo` přečíst spouštěný příkaz a zamyslet se, jestli ho opravdu chceš spustit. Ale opět, neboj se experimentovat – na virtuálním počítači není co zkazit.)*

Co by měl dělat tento příkaz?

```console
ll /var/db/sudo/lectured/
```

Příkaz ale pod běžným uživatelským účtem nefunguje; potřebuješ `sudo ll /var/db/sudo/lectured/`. To ale taky nefunguje. Proč? Jak to spravit?

* Jaké soubory jsou v `/var/db/sudo/lectured/`? Komu patří? Jak jsou velké?
* Jaké soubory jsou v `/var/run/sudo/ts`? Komu patří? Jak jsou velké? Kdo k nim má jaká práva?
* Jak by vypadal příkaz, který smaže „tvůj“ soubor v `/var/run/sudo/ts`?

Když spustíš příkaz `sudo`, zeptá se tě na heslo. Když ho spustíš podruhé (ve stejném terminálu), už se neptá – pamatuje si, že jsi heslo před chvilkou zadala.

Vyzkoušej si ale, že tohle nefunguje s příkazem pro smazání „tvého“ souboru v `/var/run/sudo/ts`. Když ho pustíš několikrát za sebou, `sudo` se vždycky znovu zeptá na heslo.

* Proč?
* Zadej tyhle příkazy:
  * Smaž „svůj“ soubor z `/var/db/sudo/lectured/`.
  * Smaž „svůj“ soubor z `/var/run/sudo/ts`.
  * Spusť `sudo echo`.
 
  A odpověz:
  * Co je u `sudo echo` jinak?
  * K čemu slouží soubory ve `/var/db/sudo/lectured/`?
