## Normaler Arbeitsablauf ##

Nachdem Sie einige Dateien geändert haben, fügen Sie den aktualisierten
Inhalt der Dateien zum Index hinzu:

Modify some files, then add their updated contents to the index:

    $ git add datei1 datei2 datei3

Jetzt können Sie committen.  Sie können sehen, was Sie demnächst
committen würden, indem Sie linkgit:git-diff[1] mit der Option --cached
aufrufen:

    $ git diff --cached

(Ohne --cached würde Ihnen linkgit:git-diff[1] Änderungen zeigen, die
Sie gemacht haben aber die Sie noch nicht dem Index hinzugefügt haben.)
Sie können auch eine kurze Zusammenfassung der Situation erhalten, indem
Sie linkgit:git-status[1] aufrufen:

    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	modified:   datei1
    #	modified:   datei2
    #	modified:   datei3
    #

Wenn Sie weitere Anpassungen vornehmen müssen, tun Sie dies jetzt, und
fügen sie den neuen Inhalt dem Index hinzu.  Schließlich können Sie Ihre
Änderungen mit folgendem Befehl committen:

    $ git commit

Dies wird sie um eine Zusammenfassung der Änderungen bitten und dann
eine neue Version des Projektes verzeichnen.

Alternativ können sie, statt vorher `git add` aufzurufen,

    $ git commit -a
    
benutzen; dies wird automatisch alle geänderten Dateien (aber keine neu
hinzugekommenen Dateien) erkennen, sie zum Index hinzufügen, und die
Änderungen committen, alles in einem Schritt.

Ein Hinweis zu Commitzusammenfassungen: obwohl es nicht erforderlich
ist, ist es dennoch eine gute Idee, die Zusammenfassung mit einer
einzigen kurzen (weniger als 50 Zeichen umfassenden) Zeile einzuleiten,
gefolgt von einer Leerzeile und dann einer umfassenderen Beschreibung.
Zum Beispiel verwenden Werkzeuge, die aus Commits E-Mail-Nachrichten
erstellen, die erste Zeile als Betreff der E-Mail und den Rest der
Commitzusammenfassung als Text der E-Mail.


#### Git verwaltet Inhalte, nicht Dateien ####

Viele Source-Code-Verwaltungssystem bieten einen "add"-Befehl an, der
dem System sagt, er möge ab jetzt Änderungen an einer neuen Datei
verfolgen.  Der "add"-Befehl bei Git tut etwas Einfacheres, jedoch Mächtigeres:
`git add` wird sowohl für neue als auch für geänderte Dateien verwendet,
und in beiden Fällen nimmt es einen Schnappschuss der betreffenden
Dateien und stellt diesen Inhalt im Index bereit, wo er dann im nächsten
Commit verwendet werden kann.

[gitcast:c2_normal_workflow]("GitCast #2: Normaler Arbeitsablauf")
