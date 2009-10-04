## Grundlegendes zu Branches und Merges ##
                                          
Jedes Git-Repository kann verschiedene "Branches" verwalten.
Wie es das Wort schon andeutet, können Sie sich Branches vorstellen als verschiedene Varianten ihres Programms. Um einen neuen Branch namens "experimentell" anzulegen, tippen Sie

    $ git branch experimentell

Sobald Sie jetzt 

    $ git branch

eingeben, bekommen Sie eine Liste aller existenten Branches:

      experimentell
    * master

Der "experimentell"-Branch wurde gerade von Ihnen angelegt, und der "master"-Branch ist per Konvention der Hauptzweig, der auch automatisch angelegt wird. Das Sternchen weist darauf hin, dass dies der aktuell verwendete Zweig ist.

    $ git checkout experimentell

lässt Sie in den neuen Branch wechseln. Editieren Sie nun eine Datei, fügen Sie sie dem System mit "commit" hinzu und wechseln Sie zurück in den Hauptzweig:

    (editiere datei)
    $ git commit -a -m "nur zum test"
    $ git checkout master
                                    
Bemerken Sie, dass Ihre Änderungen nicht mehr sichtbar sind, da Sie ja im Nebenzweig getätigt wurden und Sie sich jetzt nicht mehr dort aufhalten?
                                                                        
Ändern Sie etwas Anderes im Hauptzweig:

    (edit file)
    $ git commit -a -m "weiterer test"
                                     
Jetzt sind die beiden Zweige auseinandergelaufen, da es unterschiedliche Änderungen in beiden gibt.
Wollen Sie die Änderungen zusammenführen, geben Sie ein

    $ git merge experimentell
                                                                 
Wenn es keine Konflikte gegeben hat, sind Sie fertig. Sollte es Konflikte gegeben haben, werden sprechende Markierungen in den problematischen Dateien erzeugt.

    $ git diff
        
zeigt diese Änderungen an. Sobald Sie die Konflikte gelöst haben, können Sie mit

    $ git commit -a

das Ergebnis committen und damit das Zusammenführen beenden.
Ein "Merge" (englisch für "zusammenfassen") führt also zwei Zweige zusammen.

    $ gitk
                                                                            
zeigt Ihnen eine graphische Repräsentation über den Verlauf des Projektes.
                                                                          
Sind wir hier angekommen, so können Sie ihren experimentellen Branch auch wieder löschen. Das passiert mit

    $ git branch -d experimentell
                                 
Die Option "-d" löscht den angegebenen Branch nur, wenn er bereits wieder im aktuellen Zweig aufgegangen ist.
                                
Sollten Sie etwas Ungewöhnliches ausprobiert haben wollen, so können Sie einen solchen Branch (nennen wir ihn "komisch") mit der Option "-D" auch löschen, ohne dass seine Änderungen wieder zurückgeführt wurden.

    $ git branch -D crazy-idea
                                                       
Branches sind in Git "billig" und der Umgang sehr einfach, deshalb sind sie eine gute Möglichkeit, um Dinge auszuprobieren.
Manche Projekte haben auch sehr umfangreiche Test-Suites, deshalb hat es sich bei manchen Entwicklern eingebürgert, ihre Tests im "test"-Zweig laufen zu lassen und diesen dann rückstandsfrei wieder zu löschen.

### Die hohe Kunst des Zusammenführens ###
                                                                 
Zwei divergierende Zweige können mit linkgit:git-merge[1] wieder zusammengeführt werden:

    $ git merge branchname
                      
führt die Änderungen in "branchname" in den aktuellen Branch ein.
Gibt es Konflikte --zum Beispiel, wenn dieselbe Datei an derselben Stelle mit verschiedenen Inhalten geändert wurde--, werden Sie gewarnt. 
Eine solche Ausgabe sieht wie folgt aus:

    $ git merge next
     100% (4/4) done
    Auto-merged file.txt
    CONFLICT (content): Merge conflict in file.txt
    Automatic merge failed; fix conflicts and then commit the result.
                
In der entsprechenden Datei wurden Markierungen hinterlassungen; Wenn Sie die Probleme aufgelöst haben, können Sie den Index updaten und mit "git commit" normal weiter arbeiten.
                       
In gitk können Sie beobachten, dass Ihr resultierender Commit jetzt zwei Elternzweige hat: Einmal ihren vorher aktuellen Branch, und dann den Zweig, dessen Änderungen Sie mit "git merge" eingepflegt haben.

### Einen Merge auflösen ###                     
                                                 
Sollte ein Merge nicht automatisch aufgelöst worden sein, hinterlässt Git Ihnen die Dateien in einem Sonderzustand, und gibt Ihnen alle Informationen, die Sie zum Auflösen benötigen.
                               
Dateien mit Konflikten werden speziell im Index markiert, so dass linkgit:git-commit[1] fehlschlägt, solange Sie die Probleme nicht ausgeräumt haben:

    $ git commit
    file.txt: needs merge
     
Auch linkgit:git-status[1] zeigt Ihnen diese Dateien als "unmerged" an und die Dateien selbst haben Markierungen im Text bekommen, die so oder so ähnlich aussehen:

    <<<<<<< HEAD:file.txt
    Hello world
    =======
    Goodbye
    >>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt
        
Um diesen Konflikt aufzulösen, entscheiden Sie sich für eine Version (oder bei komplexeren Änderungen für eine Mischlösung) und tippen Sie dann

    $ git add file.txt
    $ git commit
                                                                
Die Commit-Nachricht ist nun schon vorgefertigt mit Informationen über den Merge. 
Sie können diese verwenden und nach Belieben weitere Informationen hinzufügen.

The above is all you need to know to resolve a simple merge.  But git
also provides more information to help resolve conflicts:

### Undoing a merge ###

If you get stuck and decide to just give up and throw the whole mess
away, you can always return to the pre-merge state with

    $ git reset --hard HEAD

Or, if you've already committed the merge that you want to throw away,

    $ git reset --hard ORIG_HEAD

However, this last command can be dangerous in some cases--never throw away a
commit if that commit may itself have been merged into another branch, as
doing so may confuse further merges.

### Fast-forward merges ###

There is one special case not mentioned above, which is treated differently.
Normally, a merge results in a merge commit with two parents, one for each of
the two lines of development that were merged.

However, if the current branch has not diverged from the other--so every
commit present in the current branch is already contained in the other--then
git just performs a "fast forward"; the head of the current branch is moved
forward to point at the head of the merged-in branch, without any new commits
being created.

[gitcast:c6-branch-merge]("GitCast #6: Branching and Merging")
