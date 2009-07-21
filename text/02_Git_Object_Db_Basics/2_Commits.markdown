### Commit-Objekt ###

Das "commit"-Objekt verbindet den physikalischen Zustand eines Baumes
mit einer Beschreibung dessen, wie und warum wir zu ihm gelangt sind.

[fig:object-commit]

Sie können die Option --pretty-raw von linkgit:git-show[1] oder
linkgit:git-log[1] benutzen, um Ihren Lieblings-Commit anzusehen:

    $ git show -s --pretty=raw 2be7fcb476
    commit 2be7fcb4764f2dbcee52635b91fedb1b3dcf7ab4
    tree fb3a8bdd0ceddd019615af4d57a53f43d8cee2bf
    parent 257a84d9d02e90447b149af58b271c19405edb6a
    author Dave Watson <dwatson@mimvista.com> 1187576872 -0400
    committer Junio C Hamano <gitster@pobox.com> 1187591163 -0700

        Fix misspelling of 'suppress' in docs

        Signed-off-by: Junio C Hamano <gitster@pobox.com>

Wie Sie sehen können, wird ein Commit definiert durch:

- einen **Baum** (tree): Der SHA1-Name eines Baum-Objektes (wie weiter unten
  erklärt); dies stellt den Inhalt eines Verzeichnisses zu einem
  bestimmten Zeitpunkt dar.
- **Vorgänger** (ein oder mehrere "parent"-Einträge): Die SHA1-Namen von
  einem oder mehreren Commits, welche den/die unmittelbar vorangehenden
  Schritt(e) in der Geschichte des Projektes bezeichnen.  Das Beispiel
  oben hat einen Vorgänger; Merge-Commits können mehr als einen haben.
  Einen Commit ohne Vorgänger nennen wir ein "Root-Commit"
  (Wurzel-Commit); so ein Commit stellt den Anfangszustand eines
  Projektes dar.  Jedes Projekt muss mindestens eine Wurzel haben.  Ein
  Projekt kann auch mehrere Wurzeln haben, aber das ist nicht sehr
  häufig (und auch nicht unbedingt eine gute Idee).
- einen **Autor** (author): Der Name der Person, die für diese Änderung
  verantwortlich war, sowie das Datum der Änderung.
- einen **Committer**: Der Name der Person, die diesen Commit erzeugt
  hat, zusammen mit dem Datum, wann dies geschehen ist.  Dies kann eine
  andere Person sein als der Autor, zum Beispiel, wenn der Autor einen
  Patch geschrieben hat und diesen einer anderen Person per E-Mail
  geschickt hat, die dann diesen Patch benutzt hat, um ein Commit zu
  erzeugen.
- einen **Kommentar** (comment), der diesen Commit beschreibt.

Beachten Sie, dass ein Commit selber keine Informationen darüber hat,
was sich genau geändert hat; alle Änderungen werden dadurch berechnet,
indem die Inhalte des Baumes, auf das dieser Commit verweist, mit den
Bäumen verglichen werden, auf den die Vorgänger verweisen.  Insbesondere
versucht git nicht explizit, Dateiumbenennungen zu verzeichnen, obgleich
es Fälle identifizieren kann, wo die Existenz derselben Dateidaten in
unterschiedlichen Pfaden eine Umbenennung nahelegt.  (Sehen Sie hierzu
zum Beispiel die Option -M von linkgit:git-diff[1].)

Ein Commit wird im Allgemeinen von linkgit:git-commit[1] erzeugt; dieser
Befehl erzeugt einen Commit, dessen Vorgänger normalerweise der aktuelle
HEAD ist und dessen Baum aus den Inhalten besteht, die gerade im Index
gespeichert sind..

### Das Objekt-Modell ###

So, nun, da wir die drei wesentlichen Objekttypen angesehen haben (Blob,
Baum und Commit), lassen Sie uns kurz ansehen, wie sie zusammenpassen.

Wenn wir ein einfaches Projekt mit folgender Verzeichnisstruktur haben:

    $>tree
    .
    |-- README
    `-- lib
        |-- inc
        |   `-- tricks.rb
        `-- mylib.rb

    2 Verzeichnisse, 3 Dateien

und dieses Projekt in ein Git-Repository committen, würde es wie folgt
dargestellt werden:

[fig:objects-example]

Sie sehen, dass wir ein **tree**-Objekt für jedes Verzeichnis (inklusive
des Wurzelverzeichnisses) sowie ein **blob**-Objekt für jede Datei
erstellt haben.  Schließlich haben wir noch ein **commit**-Objekt, das
auf die Wurzel zeigt, sodass wir hinterher nachvollziehen können, wie
unser Projekt aussah, als es committet wurde.
