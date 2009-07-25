## Der Git-Index ##

Der Git-Index ist eine Art Sammelstelle für Änderungen zwischen Ihrem
Arbeitsverzeichnis und Ihrem Repository.  Sie können den Index benutzen,
um eine Reihe von Änderungen zusammenzustellen, die sie auf einmal
committen möchten.  Wenn Sie ein Commit erzeugen, ist der Inhalt des
Commits der Inhalt des Index, nicht der Inhalt Ihres
Arbeitsverzeichnisses.

### Den Index ansehen ###

Der einfachste Weg zu sehen, was in dem Index ist, ist der Befehl
linkgit:git-status[1]. Wenn Sie git status ausführen, können Sie sehen,
welche Dateien bereitgestellt wurden (sich aktuell im Index befinden),
welche verändert aber noch nicht bereitgestellt wurden sowie welche
Datien überhaupt nicht von Git verwaltet werden.

    $>git status
    # On branch master
    # Your branch is behind 'origin/master' by 11 commits, and can be fast-forwarded.
    #
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	modified:   daemon.c
    #
    # Changed but not updated:
    #   (use "git add <file>..." to update what will be committed)
    #
    #	modified:   grep.c
    #	modified:   grep.h
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	blametree
    #	blametree-init
    #	git-gui/git-citool

Wenn Sie den Index komplett löschen, haben Sie in der Regel keine
Informationen verloren, solange Sie den Namen des Baums haben, den der
Index beschrieben hatte.

Hiermit haben Sie schon ein recht gutes Verständnis der Grundlagen
dessen, was Git hinter den Kulissen tut und warum es ein bisschen anders
ist als die meisten anderen SCM-Systeme.  Keine Sorge, wenn Sie nicht
sofort alles verstanden haben; wir werden in den nächsten Abschnitten
auf all diese Themen zurückkommen.  Jetzt können wir uns daran machen,
Git zu installieren, zu konfigurieren und zu benutzen.
