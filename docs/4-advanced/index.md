# Die Untiefen von Git

[Vorheriges Kapitel](/git-workshop/3-merge-requests/) | [Zurück zur Übersicht](/git-workshop/) | [Nächstes Kapitel](/git-workshop/5-exercise/)

**Du kannst nach dem Bearbeiten der ersten vier Module schon alles Wichtige.
Wenn es dich interessiert, zeigen wir dir hier, was du tun kannst, wenn etwas
mal nicht funktioniert. Außerdem schauen wir uns an, wie man GitHub möglichst
effektiv benutzt.**

Konkret schauen wir uns die folgenden Themen an:

* Merge Conflicts - wenn Mergen Probleme macht (was aber überhaupt nicht schlimm
  ist)
* Repositories Forken - weil man nicht immer direkten Zugriff auf Projekte hat
* Die `.gitignore`-Datei - nicht alles in einem Verzeichnis gehört auch in die
  Versionskontrolle

## Merge Conflicts

Bei Merges, wie wir sie im letzten Kapitel betrachtet haben, kann auch mal etwas
schief gehen. Wir versuchen ja, zwei Branches zusammenzubringen. Bisher war das
immer relativ einfach, da sich unser Ziel-Branch (meistens `master`) nicht
verändert hatte. In Team-Situationen kommt es natürlich durchaus vor, dass
andere ihre Änderungen in den Master mergen und so eine Menge Aktivität zusammen
kommt. Wenn dabei alle unterschiedliche Dateien bearbeiten, ist das kein
Problem. Diese Merges bringt git ohne Hilfe zusammen. Selbst wenn zwei Personen
die gleiche Datei an unterschiedlichen Stellen bearbeiten, kann git das
verkraften. Problematisch wird es lediglich dann, wenn in zwei Branches, die
gemerged werden sollen, in der gleichen Zeile einer Datei Änderungen enthalten.
Dann weiß git nicht, welche davon behalten werden sollen.

> Merge Conflicts sind nicht schlimm. Viele haben Angst davor, irgendwann mal
> einen solchen Konflikt lösen zu müssen, aber das ist halb so wild.

Um dir zu zeigen, wie ein Merge Conflict verursacht wird und wie man ihn behebt,
erzeugen wir uns einfach schnell einen. Wir gehen dabei von einem sauberen
Repository aus (keine nicht committeten Änderungen, nichts in der Staging-Area,
synchronisiert mit remote). Wir befinden uns auf dem `master`-Branch. Von
unseren vorherigen Modulen haben wir dort ja noch die Dateien `entry_1.txt`,
`entry_2.txt` und `entry_3.txt`. Wir nehmen jetzt Änderungen an `entry_1.txt`
vor.

```bash
git checkout -b conflicts-are-okay
echo "Ich aktualisiere mal meinen ersten Eintrag." > entry_1.txt
git add entry_1.txt
git commit -m "Replace contents in Entry 1"
```

Wir haben auf einem Extra-Branch Änderungen am ersten Eintrag vorgenommen.
Simulieren wir nun, dass jemand gleichzeitig auch im `master` an dieser Datei
gearbeitet hat.

```bash
git checkout master
echo "Ich ergänze meinen ersten Eintrag" >> entry_1.txt
git add entry_1.txt
git commit -m "Extend content of entry 1"
```

Beachte, dass wir statt `>`, was die Datei überschreibt, `>>`, verwenden. Dieser
Operator hängt die Inhalte hinten an die Datei an. Jetzt sind wir soweit. Wir
haben zwei verschiedene Versionen der Datei `entry_1.txt` auf zwei verschiedenen
Branches. Jetzt nehmen wir den Merge vor, ausnahmweise über die Kommandozeile
und nicht als Merge Request.

```bash
git merge conflicts-are-okay
```

Die Ausgabe berichtet uns von einem Vorfall:

> `CONFLICT (add/add): Merge conflict in entry_1.txt`
> 
> `Auto-merging entry_1.txt`
>
> `Automatic merge failed; fix conflicts and then commit the result.`

`git status` hilft wie immer weiter und beschreibt, was wir tun können.
Entweder:

* Wir brechen den Merge ab `git merge --abort`

Oder:

* Wir bearbeiten die Datei und
* Benutzen `git add entry_1.txt`, um den Merge abzuschließen.

Wenn du dir die Datei `entry_1.txt` anschaust, wirst du viele `========`,
`>>>>>` und `<<<<<<<` finden. Das ist die Art von git, dir zu zeigen, wo welche
Änderungen aus welchem Branch stammen. In einem beliebigen Editor kannst du als
Löser des Konflikts nun machen, was du möchtest. Die Änderungen eines Branches
übernehmen, beide Änderungen zusammen führen oder etwas gänzlich anderes. Du
musst danach nur git mitteilen, dass der Konflikt "resolved" ist. Das machst du,
indem du die Änderungen einfach in die Staging-Area bringst und dann committest:

```bash
git add entry_1.txt
git commit
```

Alternatvi kannst du die Kurzform `git commit -a` benutzen. git meldet den
Erfolg des Merges. Alles erledigt! Du kannst jetzt noch pushen, um das Remote
Repository up-to-date zu halten.

## Repositories Forken

GitHub erlaubt es dir, öffentliche Repositories zu forken, "gabeln". Damit wird
dir ermöglicht, an einem Projekt mitzuarbeiten, ohne dass du zum Team dieses
Projekts gehörst. Das kannst du zum Beispiel nutzen, um Änderungen an der
[offiziellen Fachschaftswebsite](https://github.com/fsi-hska/iwi-website)
vorzuschlagen. Du kannst das Repository, das in unserer offiziellen GitHub-Orga
zuhause ist, mit einem Klick auf den Fork-Button oben rechts nämlich einfach zu
dir holen, es wird dann in deinem persönlichen Account liegen. Dort hast du
sämtliche Berechtigungen und kannst Branches erstellen und Commits hinzufügen.

Der Kniff: Einen Merge Request kannst du auch von einem geforkten Repository
zurück ins Original-Repository eröffnen. So kommen Änderungen von dir dann in
"offizielle" Repositories. Das ist das Standard-Vorgehen, wenn du bei
Open-Source-Projekten mitwirken willst.

## Die `.gitignore`-Datei

Vielleicht hast du dir schon überlegt, dass es manchmal gar nicht so schlau ist,
gewisse Dateien in einer Versionsverwaltungs-Software wie git zu halten. Wenn du
in Java programmierst, sind das zum Beispiel die `class`-Dateien, die du nach
der Ausführung deines Programms neben deinen `.java`-Dateien liegen hast. Sie
sind einfach nur das Ergebnis eines Compile-Schritts und direkt abhängig von
deinem Java-Quellcode. Sie gehören also nicht in git, sondern nur die "Quelle
der Wahrheit" - also dein Java-Source-Code.

Das Gleiche gilt für persönliche Zugangsdaten, die du für deine Programme als
Datei bereitstellen, aber nicht mit deinen Teamkolleg:innen teilen möchtest. Das
ist z. B. der Fall, wenn ihr mit einer API arbeitet, für die man sich
authentifzieren muss.

Noch ein Beispiel: Du nutzt einen statischen Website-Generator, um aus einfachen
Text-Dateien Websites zu generieren. Die fertigen HTML-Dateien sind dann
ebenfalls abhängig von deinen Quell-Files und gehören nicht in git.

Für all diese Szenarien gibt es eine einfache Lösung. Die Datei `.gitignore`.
Sie kann in jedem Repository angelegt werden. Meistens liegt sie im
Wurzelverzeichnis des Repos, aber auch Unterverzeichnisse können eine
`.gitignore`-Datei enthalten. Probieren wir mal aus, wie sie funktioniert. Dafür
nehmen wir an, dass wir nun einen Tagebucheintrag verfassen, der geheim ist.

> Wir ziehen dafür ein bisschen was an den Haaren herbei. In der Regel sind
> Tagebucheinträge ja immer geheim. Die Einträge 1 bis 3 aus den bisherigen
> Modulen sind wohl so semi-geheim.

Wir wollen Top-Secret-Einträge dadurch kennzeichnen, dass sie die Datei-Endung
`.secret.txt` haben. Legen wir also einen geheimen Eintrag an:

```bash
echo "Ich weiß überhaupt nicht, wie man Wäsche richtig wäscht." > entry_4.secret.txt
```


Diese peinliche Offenbarung wollen wir vor git geheimhalten. Wenn du allerdings
`git status` ausführst, siehst du, dass git schon von der Datei Wind bekommen
hat. Wir müssen nun also dafür sorgen, dass git diese Datei nicht mehr sieht.
Dafür legen wir uns eine weitere Datei mit dem Namen `.gitignore` an und öffnen
diese mit einem Editor.

> Unter Windows kann es mit dem führenden Punkt Probleme geben. Legt die Datei
> wirklich über das Terminal an und nicht im Explorer. Es handelt sich dabei um
> eine versteckte Datei, d. h. auch unter Linux/Mac seht ihr sie nur mit 
> `ls -a`, nicht mit `ls`.

Dort tragen wir folgendes ein:

```java
*.secret.txt
```

Wenn du nun ein `git status` ausprobierst, siehst du:

* Wie zu erwarten eine Datei `.gitignore`, die du stagen kannst
* Keine Datei `entry_4.secret.txt`!

**Wie geht das?**

git checkt automatisch, ob eine `.gitignore`-Datei vorhanden ist und wendet die
darin enthaltenen Regeln auf das Repository an. Jede Zeile der Datei entspricht
dabei einer Regel. Für unser Tagebuch-Repo gibt es aktuell also genau eine
Regel:

```java
*.secret.txt
```

Jede Zeile beschreibt, wie eine Datei auszusehen hat, die git ignorieren soll.
Dabei kann man sich Platzhalter zunutze machen, und das haben wir hier gemacht.
Der `*` am Anfang der Zeile steht für "beliebige Zeichenkette". D. h., jede
Datei, die mit `.secret.txt` endet, wird durch diese Regel von git
ausgeschlossen.

Jetzt wollen wir einen ganzen Unterordner vor git verstecken.

```bash
mkdir secret
cd secret
cat "Passwort für den Banktresor: 1234" > tresorzugang.txt
cd ..
```

Der Unterordner `secret` soll ab sofort geheime Informationen beinhalten.
Deshalb muss auch er auf die Liste, die wir wie folgt erweitern:

```java
*.secret.txt
secret
```

Die zweite Zeile schließt das gesamte Unterverzeichnis mit dem Namen `secret`
von der Versionsverwaltung aus. Du kannst nun die Datei `.gitignore` committen,
um in deinem Repo wieder einen cleanen Stand herzustellen.

> **Pro-Tipp**: Für die meisten Programmiersprachen oder Frameworks gibt es
> vorgefertigte .gitignore-Files, die du dir ergooglen kannst. Damit bist du für
> neue Projekte gewappnet und kannst direkt loslegen, ohne Gefahr zu laufen,
> eine nicht für die Versionsverwaltung gedachte Datei einzuchecken.

## Kontrollfragen

// TODO

## Zusammenfassung

[Vorheriges Kapitel](/git-workshop/3-merge-requests/) | [Zurück zur Übersicht](/git-workshop/) | [Nächstes Kapitel](/git-workshop/5-exercise/)