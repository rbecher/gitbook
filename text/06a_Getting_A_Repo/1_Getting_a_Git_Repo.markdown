## Zu einem Git-Repository kommen ##

So - nun, da wir alles eingerichtet haben, brauchen wir ein
Git-Repository.  Dies können wir auf eine von zwei Arten erreichen - wir
können ein Repository, das bereits existiert, *clonen* oder wir können
ein Repository *initialisieren*, entweder aus existierenden Dateien, die
noch nicht unter Source-Code-Kontrolle stehen oder aus einem leeren
Verzeichnis.

### Ein Repository clonen ###

Um eine Kopie eines Projektes anzulegen, müssen Sie die Git-URL des
Projektes kennen - den Standort des Repositorys.  Git kann über viele
verschiedene Protokolle arbeiten, daher kann die URL mit ssh://,
http(s)://, git:// oder einfach mit einem Benutzernamen anfangen (im
letztgenannten Fall wird Git ssh als Transportprotokoll annehmen).
Einige Repositorys können auch über mehr als ein Protokoll angesprochen
werden.  Zum Beispiel kann der Quellcode für Git selber entweder über
das git://-Protokoll geclont werden:

    git clone git://git.kernel.org/pub/scm/git/git.git

oder über http:

    git clone http://www.kernel.org/pub/scm/git/git.git

Das git://-Protokoll ist schneller und effizienter, aber manchmal ist es
notwendig, HTTP zu benutzen, zum Beispiel wenn Sie hinter einer
Firmen-Firewall oder so stecken.  In jedem Fall sollten Sie hinterher
ein neues Verzeichnis namens 'git' haben, in dem der gesamte
Git-Quellcode inklusive seiner Historie gespeichert ist - im Grunde
genommen eine vollständige Kopie dessen, was auf dem Server ist.

Defaultmäßig wird Git das neue Verzeichnis, in das es den geclonten Code
auscheckt, nach dem benennen, was unmittelbar vor dem '.git' im Pfad des
geclonten Projektes kommt.  (Als Beispiel wird der Befehl *git clone
http://git.kernel.org/linux/kernel/git/torvalds/linux-2.6.git* zu einem
neuen Verzeichnis namens 'linux-2.6' führen.)

### Ein neues Repository initialisieren ###

Nehmen wir an, Sie haben eine Tar-Datei namens projekt.tar.gz, die Ihre
Arbeit enthält.  Sie können diese Dateien folgendermaßen unter
Git-Source-Code-Kontrolle stellen:

    $ tar xzf projekt.tar.gz
    $ cd projekt
    $ git init

Git wird antworten

    Initialized empty Git repository in .git/

Jetzt haben Sie das Arbeitsverzeichnis initialisiert - sie werden
vielleicht bemerken, dass ein neues Verzeichnis namens ".git" angelegt
worden ist.

[gitcast:c1_init](GitCast #1 - Konfiguration, Initialisieren und Clonen)
