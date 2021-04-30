# Mit Merge Requests Arbeiten

[Vorheriges Kapitel](/git-workshop/2-basics/) | [Zurück zur Übersicht](/git-workshop/) | [Nächstes Kapitel](/git-workshop/4-advanced/)

**Wenn wir im Team gemeinsam an Code arbeiten, ist das arbeiten mit Branches
und Merge Requests unverzichtbar. Wir zeigen dir, wie sie funktionieren.**

## Branches im Team nutzen

Im letzten Modul haben wir Branches kennen gelernt, und wie Commits diese
Branches formen, indem sie aufeinander verweisen. Für ein großes Projekt, an dem
viele Personen zusammen arbeiten, kann das dann zum Beispiel so aussehen:

![Extrembeispiel für Git-Branches](https://i.stack.imgur.com/aPOr8.png)

[[Quelle]](https://softwarerecs.stackexchange.com/questions/34660/)

Jetzt nicht erschrecken. Das ist ein Extrembeispiel und soll nur zeigen, wie
mächtig das Branching-Feature Git macht. Es gibt Arbeitsstile für
Entwicklungs-Teams, die sich aus diesem Modell herausgebildet haben. Eines der
häufigsten, verständlichsten und am leichtesten zu erlernenden Modelle heißt
**GitHub Flow**. Wir empfehlen es dir für Projekte im Studium. Es läuft wie
folgt:

1. Du hast in deinem Projekt einen Haupt-Branch, `master` oder `main`.
2. Für jedes neue Feature, also für jeden Code-Beitrag zum Projekt, erstellst du
   einen neuen Branch. Am besten mit einem aussagekräftigen Namen, wie z. B.
   `feature/add-pdf-support`.
3. Auf diesem Branch machst du Commits mit deinem Arbeitsfortschritt, bis du
   damit zufrieden bist.
4. Du stellst einen **Merge Request**. Was das ist, klären wir gleich.
5. In diesem Request können deine Teamkolleginnen ihre Meinung abgeben, und,
   falls es Änderungswünsche gibt, kannst du weitere Commits hinzufügen
6. Wenn alle zufrieden sind, wandern deine Entwicklungen in den Haupt-Branch und
   sind ab dann fester Bestandteil des Projekts!

[Visualisierung und Quelle](https://guides.github.com/introduction/flow/)

## Mergen - Aber Richtig

Im letzten Modul haben wir schon darüber gesprochen, dass Branches auch wieder
zusammengeführt werden können, und dass dieser Vorgang **Mergen** heißt.

Ganz kurz wollen wir dir zeigen, wie du lokale Branches mergen kannst. Wir sind,
wenn du seit dem letzten Modul nichts verändert hast, noch auf dem
`master`-Branch. Wir hatten ja aber noch unseren zweiten Tagebuch-Eintrag auf
dem anderen Branch `my-branch`. Diesen Beitrag (unser neues Feature quasi)
wollen wir jetzt auf `master` bringen:

```bash
git merge my-branch
```

Git meldet zurück:

> `Updating 16cf2d4..4f7ea9f`
>
> `Fast-forward`
>
> `entry_2.txt | 1 +`
>
> `1 file changed, 1 insertion(+)`
>
> `create mode 100644 entry_2.txt`

Die IDs sind bei dir nicht dieselben, aber du kannst sehen, dass der
`master`-Branch "vorgespult" (fast-forward) wurde. Der neue Commit vom Branch
`my-branch` und damit di Datei `entry_2.txt` wurde hinzugefügt.
`git status` sagt dir nun, dass das lokale und das remote Repository nicht
übereinstimmen. Das beheben wir mit einem simplen

```bash
git push
```
Dieser Vorgang, mit dem lokalen mergen auf der Kommandozeile, wird aber selten
gebraucht. Alleine arbeitet man oft eh nur an einer Sache und Branching kann da
nicht wirklich seine Vorteile ausspielen. Viel häufiger als Merges sind die
bereits angesprochenen Merge Requests.

### Einen Merge Request erstellen

Wir erstellen uns einen neuen Branch, einen neuen Tagebucheintrag und committen
das Ganze. Außerdem pushen wir gleich. Klingt schon wahnsinnig professionell,
oder?

```bash
git checkout -b awesome-entry
echo "Das ist mein dritter Tagebucheintrag." > entry_3.txt
git add entry_3.txt
git commit -m "Add third entry"
git push
```

Wenn dir das zu viele Befehle hintereinander ohne Erklärung sind, lies nochmal
Modul 2 in Ruhe durch. Wir haben hier nur die ganzen `git status` zwischendrin
weggelassen.

Wechsel nun zu GitHub. Vermutlich wird es dir auf deiner Repository-Seite schon
einen Hinweis anzeigen, dass es einen Branch gibt, für den ein Merge Request
angelegt werden kann. Wir gehen den "offiziellen" Weg. Du hast oben in der
Menü-Leiste deines Repositories die Punkte "Code", "Issues", "Pull Requests",
"Actions" usw. Wir klicken auf "Pull Requests" (Pull Requests und Merge Request
sind dasselbe). Klicke auf "New Pull Request" rechts. Auf der sich öffnenden
Seite kannst du nun auswählen welchen Branch du mit welchem mergen möchtest. Wir
wählen `master` als Base und `awesome-entry` als Compare. GitHub zeigt dir nun
schon, wo di Unterschiede zwischen den beiden Branches liegen. Klicke auf
"Create Pull Request". GitHub zeigt dir ein Textfeld, in dem du genaueres zu
deinen Änderungen festhalten kannst. Bei einem "echten" Merge Request solltest
du das auch immer tun! Hier sparen wir uns das und bestätigen mit einem zweiten
Klick auf "Create Pull Request".

Herzlichen Glückwunsch, du hast einen ersten Merge Request erstellt!

### Merge Requests in der freien Wildbahn

Wie schon beschrieben kommen die Vorteile von Branching erst dann zum Tragen,
wenn mehrere Leute zusammen an einem Projekt arbeiten. Eure Fachschaft IWI
verwaltet zum Beispiel die Website auch über ein Git-Repository bei GitHub.
Schauen wir uns mal einen kürzlich erstellten Pull Request an:

**[Update pruefungen.md, ein Beispiel-Merge-Request](https://github.com/fsi-hska/iwi-website/pull/57)**

Der Nutzer Wekra hat Änderungen an einer Datei vorgenommen und ein Review von
anderen Mitglieder:innen des Repositories angefordert. Der Nutzer jwestenhoff
hat gleich noch ein paar Verbesserungsvorschläge geliefert, andere haben die
Änderungen einfach nur "approved", also bestätigt. Ich (SimonKienzler) habe dann
den Merge vorgenommen. So haben wir als Team zusammen neue Inhalte auf die
Website gebracht. (Die betroffende Seite findest du
[hier](https://iwi-hka.de/faq/pruefungen/).)

GitHub bietet mehrere Ansichten eines Pull Requests, wir haben uns gerade die
"Conversation" angeschaut. Ebenfalls interessant ist die Ansicht "Files
changed", die in einem Vorher-Nachher-Split gegenüberstellt, welche Änderungen
an welchen Dateien durch diesen Pull Request vorgenommen werden.

### Mere Requests abschließen

Du kannst jetzt auch deinen ersten Merge Request beenden, indem du auf dessen
"Conversation"-Übersicht unten auf "Merge Pull Request" klickst. Wenn du einen
Hauch Realismus dabei haben möchtest, lade einen Freund oder eine Freundin,
dessen GitHub-Account-Namen du kennst, als Reviewer ein. (Achtung, das geht
natürlich nur, wenn dein Repository public ist oder du besagten Freund als
Bearbeiter für dein privates Repository freischaltest.) Sobald diese Person ihr
OK gegeben hat, kannst du auf den Button klicken und dann mit "Confirm Merge"
bestätigen. GitHub merged dann automatisch deinen Pull Request in den
`master`-Branch.

Dieser Vorgang findet ja im Remote Repository statt, also musst du nun noch
dafür sorgen, dass er auch im lokalen Repo ankommt. Wie geht das noch mal?

Genau.

```bash
git pull
```

Wenn du nun noch ein `git log` ausführst, siehst du, dass GitHub in deinem Namen
einen sogenannten "Merge Commit" angelegt hat, in dem die Änderungen aus deinem
Feature-Branch in den Haupt-Branch gelangt sind. Damit hast du den GitHub
Workflow einmal durchgespielt.

## Kontrollfragen

// TODO

## Zusammenfassung

* Wir haben uns lokales Mergen angesehen.
* Wir haben uns angeschaut, wie man mit GitHub Feature-Branches über Pull
  Requests in den Haupt-Branch überführt.
* Damit haben wir den GitHub Workflow durchgespielt.
* Wir haben uns angeschaut wie Merge Conflicts zustande kommen, und dass sie
  höchstens lästig, aber nicht schlimm sind.
* Wir haben gelernt, dass man Repositories forken kann, um dort etwas
  beizutragen, auch wenn man nicht Mitglied des Teams ist.

Damit hast du alles wichtige zusammen, was die Grundlagen von Git und die Arbeit
mit GitHub angeht. Im nächsten Modul zeigen wir dir noch einige Tricks und
Kniffe für angehende Git-Profis, bevor dann die große Abschlussaufgabe ansteht.

[Vorheriges Kapitel](/git-workshop/2-basics/) | [Zurück zur Übersicht](/git-workshop/) | [Nächstes Kapitel](/git-workshop/4-advanced/)