Hier ist die Übersetzung des Textes ins Deutsche:

---

**BUILDING WINGS 3D AUF WINDOWS**
============================

Diese Anleitung beschreibt, wie Sie Wings 3D aus den Quellen auf einem Windows-System bauen können.

Erforderliche Software
=================

Wings kann unter Windows nur mit freier/open-source Software gebaut werden.

Folgende Software wird benötigt:

- Die Wings-Quellen unter http://www.wings3d.com oder git

- WSL für make und bash-Skripte

- Erlang/Otp 26.0 oder neuer. http://www.erlang.org
  Fügen Sie das mit Erlang gelieferte vcredist-Paket Ihrer Installation hinzu.
  (Es ist am einfachsten, die vorgefertigten Binärdateien für Windows herunterzuladen.)

Optionale Software
=================

- NSIS 2.02 oder höher, ein Installations-/Deinstallations-Ersteller.
  http://nsis.sf.net.

- git (zum Abrufen von Quellen von GitHub)

- cl-1.2.3 oder neuer. http://github.com/tonyrog/cl

- rebar3-Skript siehe https://github.com/erlang/rebar3
  Um das OpenCL-Paket oben zu bauen

Installation der Software
=======================

Im Allgemeinen sollten Sie den Anweisungen für jedes Paket folgen.

Einrichten der Umgebung
==========================

Einige Umgebungsvariablen müssen gesetzt werden. Sie können global für Windows von "Mein Computer" aus gesetzt werden.

Ändern Sie die PATH-Umgebungsvariable so, dass die folgenden Programme
aus einer (bash) Shell ausgeführt werden können.

  erl.exe escript.exe erlc.exe     (Erlang/OTP)

MSYS:
  gcc  (MinGW)
  export GCC=x86_64-w64-mingw32-gcc (für 64-Bit-Build)

WSL: (Linux in Windows 10)
  Erfordert derzeit installierten Microsoft Compiler,
  Richten Sie die MSVC-Umgebung ein mit:
  eval `cmd.exe /C "C:/Pfad/zu/Wings/win32/SetupWSLcross.bat" x64`

  Außerdem benötigen Sie für den Bau von OpenCL und den Aufbau des Installationsprogramms ein
  Linux-Erlang in WSL installiert, damit 'rebar3' und 'escript' funktionieren.

Zum Erstellen einer Distribution benötigen Sie:
  WINGS_VCREDIST muss so gesetzt sein, dass es auf vcredist.exe in Ihrer Erlang
  Installation zeigt. (c:\erl5.8.1.1\vcredist_x86.exe)

  makensis.exe (NSIS)

Eine einfache Möglichkeit zu überprüfen, ob die Programme ausführbar sind, ist die Verwendung des
"which"-Befehls in einer Shell wie dieser:

$ which make
/usr/bin/make
$

Abrufen/Entpacken des Wings-Quellcodes
===============================

$ git clone https://github.com/dgud/wings.git
$ cd wings

Oder entpacken Sie die Quelle:
$ tar jxf wings-1.3.1.tar.bz2
$ cd wings-1.3.1
Die folgenden Build-Schritte setzen voraus, dass Sie sich im Wings-Quellverzeichnis befinden.

Grundlegender Build
===========

Um ein minimales Wings zu bauen, das für Entwicklungszwecke verwendet werden kann,
müssen Sie nur make im Verzeichnis ausführen, in dem die Quellen entpackt wurden.

Beispiel:

$ pwd
c:/wings-1.3.1
$ make
.

<Es folgt eine Menge Ausgabe>
.
$

OpenCL wird automatisch abgerufen (über git) und gebaut, falls es nicht in
$ERL_LIBS verfügbar ist (Beispiel export ERL_LIBS=/Users/bjorng/src)

Ein gebauter CL ist nicht erforderlich, um Wings zu verwenden, aber empfohlen,
einige Funktionen sind ohne CL nicht verfügbar,
aber der Build wird nicht fehlschlagen, wenn der CL-Build fehlschlägt.

Lesen Sie cl/README für Informationen zu den Anforderungen, hauptsächlich
OpenCL-Header am richtigen Ort und rebar3 im PATH

Wings ausführen
============

Um das soeben gebaute Wings auszuführen, müssen Sie eine Befehlszeile
ähnlich wie diese schreiben:

    werl.exe -pa ../wings/ebin -run wings_start start_halt

wobei Sie <MY_APP_DATA_DIRECTORY> durch den Pfad zum
lokalen Einstellungsordner ersetzen sollten.

Oder fügen Sie das Quellverzeichnis der ERL_LIBS-Umgebungsvariable hinzu.

Beispiel:

$ erl.exe -pa /Users/username/src/wings/ebin -run wings_start start_halt
$

Eine Erlang-Konsole sollte erscheinen, gefolgt vom Wings-Fenster.

Anstatt jedes Mal die Befehlszeile zu schreiben, wenn Sie Wings starten möchten,
können Sie es in ein Skript wie dieses verpacken:

#!/bin/bash
exec werl.exe -pa /Users/username/src/wings/ebin -smp -run wings_start start_halt ${1+"$@"}

Hinweise:

[1] "exec" beendet den Shell-Prozess, der das Skript ausführt, und spart dadurch
    eine geringe Menge an Systemspeicher.

[2] Das "${1+"$@"}" übermittelt alle Argumente (oder keine) an Wings,
    damit Wings eine wings-Datei öffnen kann, wenn es gestartet wird.

Sie könnten die Befehlszeile auch in eine Standard-Windows-Verknüpfung verpacken.

Alles bauen
============

Stellen Sie sicher, dass Ihr aktuelles Verzeichnis das ist, in dem die
Quellen entpackt wurden.

Beispiel:

$ pwd
c:/wings-1.3.1
$

Um alles zu bauen (einschließlich des Installationsprogramms), führen Sie den folgenden Befehl aus:
dies erfordert ein funktionierendes OpenCL-Setup

$ make win32
.
<Es folgt eine Menge Ausgabe>
.
$

Wenn alles erledigt ist, sollte sich eine Datei namens wie

 wings-1.3.1.exe

im aktuellen Verzeichnis befinden.

--- 

Das sollte den Text umfassend ins Deutsche übersetzen.
