
### Tag-Objekt ###

[fig:object-tag]

Ein Tag-Objekt besteht aus einem Objektnamen (das einfach 'object'
genannt wird), einem Objekttyp, einem Tagnamen, dem Namen der Person,
die den Tag erstellt hat ("Tagger"), und einer Nachricht, die eine
Signatur enthalten kann.  Dies kann man zum Beispiel mit
linkgit:git-cat-file[1] sehen:

    $ git cat-file tag v1.5.0
    object 437b1b20df4b356c9342dac8d38849f24ef44f27
    type commit
    tag v1.5.0
    tagger Junio C Hamano <junkio@cox.net> 1171411200 +0000

    GIT 1.5.0
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v1.4.6 (GNU/Linux)

    iD8DBQBF0lGqwMbZpPMRm5oRAuRiAJ9ohBLd7s2kqjkKlq1qqC57SbnmzQCdG4ui
    nLE/L9aUXdWeTFPron96DLA=
    =2E+0
    -----END PGP SIGNATURE-----

Sehen Sie beim Befehl linkgit:git-tag[1] nach, wie Sie Tag-Objekte
erzeugen und verifizieren können.  (Bitte beachten Sie, dass
linkgit:git-tag[1] ebenfalls dazu benutzt werden kann, "leichtgewichtige
Tags" (lightweight tags) zu erzeugen, die allerdings gar keine
Tag-Objekte sind, sondern einfache Referenzen, deren Namen mit
"refs/tags/" anfangen.)
