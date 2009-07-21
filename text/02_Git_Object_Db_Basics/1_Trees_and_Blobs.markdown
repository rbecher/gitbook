### Blob-Objekt ###

Ein Blob speichert im Allgemeinen den Inhalt einer Datei.

[fig:object-blob]

Sie können linkgit:git-show[1] benutzen, um den Inhalt eines beliebigen
Blobs anzuzeigen.  Wenn Sie den SHA-Wert eines Blobs haben, können Sie
ihn sich so anzeigen lassen:

    $ git show 6ff87c4664

     Note that the only valid version of the GPL as far as this project
     is concerned is _this_ particular version of the license (ie v2, not
     v2.2 or v3.x or whatever), unless explicitly otherwise stated.
    ...

Ein "blob"-Objekt ist nichts Weiteres als ein Stück Binärdaten.  Es hat
keine Verweise auf andere Dinge und hat keinerlei Attribute, nicht
einmal einen Dateinamen.

Da der Blob einzig und allein durch seinen Inhalt definiert wird, teilen
sich zwei Dateien in einem Verzeichnisbaum (oder sogar in mehreren
verschiedenen Versionen einer Repository) dasselbe Blob-Objekt, wenn sie
denselben Inhalt haben.  Das Objekt ist vollkommon unabhängig von dem
Ort, an dem eine Datei in einem Verzeichnisbaum liegt, und das Objekt,
mit dem eine Datei verbunden ist, ändert sich nicht, wenn die Datei
umbenannt wird.

### Baum-Objekt ###

Ein Baum ist einfach ein Objekt mit einer Anzahl von Zeigern auf Blobs
und auf weitere Bäume - es stellt im Allgemeinen den Inhalt eines
Verzeichnisses oder Unterverzeichnisses dar.

[fig:object-tree]

Der vielseitige Befehl linkgit:git-show[1] kann auch benutzt werden, um
Baum-Objekte anzuzeigen, aber linkgit:git-ls-tree[1] gibt Ihnen mehr
Details.  Wenn wir den SHA-Wert für einen Baum haben, können wir ihn wie
folgt ansehen:

    $ git ls-tree fb3a8bdd0ce
    100644 blob 63c918c667fa005ff12ad89437f2fdc80926e21c    .gitignore
    100644 blob 5529b198e8d14decbe4ad99db3f7fb632de0439d    .mailmap
    100644 blob 6ff87c4664981e4397625791c8ea3bbb5f2279a3    COPYING
    040000 tree 2fb783e477100ce076f6bf57e4a6f026013dc745    Documentation
    100755 blob 3c0032cec592a765692234f1cba47dfdcc3a9200    GIT-VERSION-GEN
    100644 blob 289b046a443c0647624607d471289b2c7dcd470b    INSTALL
    100644 blob 4eb463797adc693dc168b926b6932ff53f17d0b1    Makefile
    100644 blob 548142c327a6790ff8821d67c2ee1eff7a656b52    README
    ...

Wie Sie sehen können, enthält ein Baum-Objekt eine Liste von Einträgen,
jeweils mit einem Modus, Objekttyp, SHA1-Namen und Namen, sortiert nach
dem Namen.  Es stellt den Inhalt eines einzelnen Verzeichnisbaumes dar.

Ein Objekt, das von einem Baum referenziert wird, kann ein Blob sein,
das den Inhalt einer Datei darstellt, oder ein weiterer Baum, der den
Inhalt eines Unterverzeichnisses darstellt.  Da Bäume und Blobs, wie
alle anderen Objekte, als Namen den SHA1-Hashwert ihres Inhaltes haben,
haben zwei Bäume genau dann denselben SHA1-Namen, wenn ihr Inhalt
(inklusive dem rekursiven Inhalt aller Unterverzeichnisse) identisch
ist.  Dies erlaubt es git, schnell den Unterschied zwischen zwei
verwandten Baum-Objekten zu ermitteln, da es Einträge mit identischen
Objektnamen ignorieren kann.

(Hinweis: wenn Submodule vorhanden sind, können Bäume ebenfalls Commits
als Einträge haben.  Sehen Sie hierfür den Abschnitt **Submodule**.)

Beachten Sie, dass die Dateien alle den Modus 644 oder 755 haben: git
kümmert sich nur um den Zustand des Ausführen-Bits (executable bit).

