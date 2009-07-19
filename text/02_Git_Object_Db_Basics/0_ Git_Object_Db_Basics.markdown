## Das Git-Objektmodell ##

### Die SHA ###

Sämtliche Information, die benötigt wird, um die Versionsgeschichte
eines Projekts darzustellen wird in Dateien gespeichert, auf die ein
40-stelliger "Objektname" verweist, der etwa folgendermaßen aussieht:
    
    6ff87c4664981e4397625791c8ea3bbb5f2279a3
    
Diese 40-stelligen Zeichenketten werden Sie beim Gebrauch von Git
überall sehen.  In jedem Fall wird der Name berechnet, indem der
SHA1-Hashwert des Inhaltes des Objektes.  Der SHA1-Hashwert ist eine
kryptografische Hashfunktion.  Was das für uns bedeutet ist, dass es
praktisch unmöglich ist, zwei verschiedene Objekte zu finden, die
denselben Namen haben.  Dies hat eine Reihe von Vorteilen, unter
anderem:

- Git kann schnell herausfinden, ob zwei Objekte identisch sind oder
  nicht, indem es nur den Namen vergleich.
- Da Objektnamen in jedem Repository auf dieselbe Weise berechnet
  werden, wird derselbe Inhalt in mehreren Repositories immer unter
  demselben Namen abgespeichert werden.
- Git kann Fehler bemerken, wenn es ein Objekt einliest, indem es
  überprüft, dass der Name des Objektes immer noch dem SHA1-Hashwert
  seines Inhalts entspricht.

### Die Objekte ###

Jedes Objekt besteht aus drei Teilen - einem **Typ**, einer **Größe**
und dem **Inhalt**.
Die _Größe_ ist einfach die Größe des Inhaltes, der Inhalt hängt von
dem Typ des Objektes ab und es gibt vier verschiedene Typen von
Objekten: "blob", "tree", "commit" und "tag".

- Ein **blob** speichert Dateidaten; es entspricht im Allgemeinen einer
    Datei.
- Ein **tree** (Baum) ist grundsätzlich wie ein Verzeichnis bzw. ein
    Ordner; er verweist auf eine Menge anderer Bäume und/oder Blobs
    (d.h.  Dateien und Unterverzeichniss)
- Ein **commit** verweist auf einen einzelnen Baum; er zeigt, wie das
    Projekt zu einem bestimmten Zeitpunkt ausgesehen hat.  Das Commit
    enthält Metainformationen über diesen Zeitpunkt wie z.B. einen
    Zeitstempel, den Autor der Änderungen seit dem letzten Commit, einen
    Zeiger auf den/die vorherigen Commit(s) usw.
- Ein **tag** ist eine Möglichkeit, einen bestimmten Commit als
    irgendwie besonders zu kennzeichnen. Er wird normalerweise
    verwendet, um bestimmte Commmits als benannte Releases zu
    kennzeichnen, oder etwas Ähnliches.

Fast der gesamte Umfang von Git ist darauf aufgebaut, diese einfache
Struktur von vier verschiedenen Objekttypen zu manipulieren.  Es ist
gewissermaßen sein eigenes kleines Dateisystem, das auf dem Dateisystem
Ihres Rechners draufsitzt.

### Unterschied von SVN ###

Es ist wichtig festzuhalten, dass diese Vorgehensweise sich grundlegend
von der anderer SCM-Systeme unterscheidet, mit denen Sie eventuell
vertraut sind.  Subversion, CVS, Perforce, Mercurial und so weiter
verwenden alle _Deltaspeicherung_ssysteme - sie speichern den
Unterschied zwischen einem Commit und dem nächsten.  Git tut dies nicht
- es speichert einen Schnappschuss davon, wie alle Dateien in Ihrem
Projekt zum Zeitpunkt eines jeden Commits aussehen.  Dies ist ein sehr
wichtiges Konzept, das Sie verstehen müssen, wenn Sie Git einsetzen.
