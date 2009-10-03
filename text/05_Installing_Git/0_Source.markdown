### Git vom Quellcode installieren ###

Kurz gesagt können Sie auf einem Unix-basierten System den Git-Quellcode
von der [Git-Downloadseite](http://git-scm.com/download) herunterladen
und dann etwa Folgendes ausführen:

    $ make prefix=/usr all ;# als ihr normaler Benutzer
    $ make prefix=/usr install ;# als root

Für das Übersetzen werden Sie die
[expat](http://expat.sourceforge.net/)-,
[curl](http://curl.linux-mirror.org)-, [zlib](http://www.zlib.net)- und
[openssl](http://www.openssl.org)-Bibliotheken bereits installieret
haben müssen - allerdings werden diese (mit der möglichen Ausnahme von
*expat*) in der Regel bereits vorhanden sein.
