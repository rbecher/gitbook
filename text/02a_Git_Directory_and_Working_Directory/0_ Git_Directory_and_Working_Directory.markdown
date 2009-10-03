## Git-Vereichnis und Arbeitsverzeichnis ##

### Das Git-Verzeichnis ###

Das 'Git-Verzeichnis' ist das Verzeichnis, in dem Git die gesamte
Historie und alle Metainformationen für Ihr Projekt speichert --
inklusiver aller Objekte (Commits, Bäume, Blogs, Tags), alle Zeiger auf
die verschiedenen Zweige und viele mehr.

Es gibt pro Projekt nur ein Git-Verzeichnis (im Gegensatz zu einem
pro Unterverzeichnis wie bei SVN oder CVS), und dieses Verzeichnis heißt
(per Default, aber nicht notwendigerweise) '.git' im Wurzelverzeichnis
Ihres Projektes.  Wenn Sie sich den Inhalt dieses Verzeichnisses
ansehen, sehen Sie alle Ihre wichtigen Dateien:

    $>tree -L 1
    .
    |-- HEAD         # Zeiger auf Ihren aktuellen Zweig
    |-- config       # Ihre Konfigurations-Vorlieben
    |-- description  # Beschreibung Ihres Projektes
    |-- hooks/       # "Haken", die vor oder nach bestimmten Aktionen ausgeführt werden
    |-- index        # Indexdatei (siehe nächsten Abschnitt)
    |-- logs/        # eine Historie dessen, wo Ihre Zweige gewesen sind
    |-- objects/     # Ihre Objekte (Commits, Bäume, Blobs, Tags)
    `-- refs/        # Zeiger auf Ihre Zweige

(dort können auch andere Dateien/Verzeichnisse enthalten sein, aber
diese sind im Moment nicht wichtig)

### Das Arbeitsverzeichnis ###

Das Git-Arbeitsverzeichnis ist das Verzeichnis, das den aktuelle
ausgecheckten Stand der Dateien enthält, die Sie im Moment bearbeiten.
Dateien in diesem Verzeichnis werden oft von Git gelöscht oder ersetzt,
wenn Sie zwischen Zweigen hin- und herwechseln - das ist normal.  All
Ihre Historie wird im Git-Verzeichnis gespeichert; das
Arbeitsverzeichnis ist einfach ein temporärer Checkout-Ort, wo Sie die
Dateien bis zum Ihrem nächsten Commit bearbeiten können.
