### Git-Konfiguration ###

Das erste, was Sie einrichten wollen werden ist, Ihren Namen und Ihre
E-Mail-Adresse zu hinterlegen, die Git benutzen wird, um Ihre Commits zu
unterschreiben.

    $ git config --global user.name "Scott Chacon"
    $ git config --global user.email "schacon@gmail.com"

Dies wird eine Datei in Ihrem Benutzerverzeichnis anlegen, die von allen
Ihren Projekten genutzt werden kann.  Defaultmäßig heißt diese Datei
*~/.gitconfig* und der Inhalt wird wie folgt aussehen:

    [user]
            name = Scott Chacon
            email = schacon@gmail.com

Wenn Sie diese Werte für ein bestimmtes Projekt übersteuern möchten (um
zum Beispiel eine Arbeits-E-Mail-Adresse zu verwenden), können Sie,
während Sie sich im Arbeitsverzeichnis des Projektes befinden, den
*git config*-Befehl ohne die *--global*-Option verwenden. Dies wird der
Datei *.git/config* im Wurzelverzeichnis Ihres Projektes einen Abschnitt
[user], ähnlich wie der oben gezeigte, hinzufügen.
