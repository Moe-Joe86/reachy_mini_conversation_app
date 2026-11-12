# Änderungen gegenüber Pollen

**Basis:** `pollen-robotics/reachy_mini_conversation_app`, Commit `2e43e80` (Branch `momo`). `main` dieses Forks bleibt
unverändert.

**Regel (Patrick, 2. November 2026):** Die Anwendung bleibt unverändert, außer einzeln begründeten Änderungen. Jede
Änderung steht als eigene Zeile in dieser Datei; sonst entstehen keine neuen Dateien, keine Doku und kein README-Umbau.

Begründungen und Prüfungen stehen im Projekt MoMo (`Moe-Joe86/LoLa`), vor allem in `docs/KOERPER-KLAERUNG.md` und
`docs/ENTSCHEIDUNGEN.md` (2. November 2026).

| Nr | Datum | Datei und Stelle | Was | Warum (Verweis LoLa) | Wie prüfen |
| --- | --- | --- | --- | --- | --- |
| 1 | 2026-11-02 | `src/reachy_mini_conversation_app/moves.py`: `_momo_koerper_ziel` (neu), `MovementManager.__init__`, `_issue_control_command`, `_sende_an_koerper` (neu), `working_loop` Schritt 3 (alle mit „MoMo, AENDERUNGEN Nr. 1“ markiert, 34 Zeilen) | Ist `MOMO_KOERPER_ADRESSE` (host:port) gesetzt, schickt die Schleife je Takt ein UDP-Paket (Kopfpose 4×4, Antennen, Rohantennen vor dem Einfrieren, Körper, Art der Bewegung, „hört zu“) an den Compositor statt `set_target`. Ohne die Variable unverändert wie Pollen | Nur MoMos Compositor soll `set_target` rufen (E1) und alles wissen, was er für Atmen und Antennen braucht (E2). LoLa: `docs/KOERPER-KLAERUNG.md` (Frage 2), `docs/FAHRPLAN.md` L1, ENTSCHEIDUNGEN 2. November 2026 | Pollens `tests/test_moves.py` grün; ohne Variable sendet die App wie vorher. LoLa-Simulation: dieselben Bewegungen über den Compositor weichen weniger ab als zwei direkte Läufe untereinander; die App ruft dabei 0-mal `set_target` |
| 2 | 2026-11-12 | `src/reachy_mini_conversation_app/main.py`: `import os` und der Start nach `movement_manager.start()` (beide mit „MoMo, AENDERUNGEN Nr. 2“ markiert, 9 Zeilen) | Ist `MOMO_KOPFWACKELN=aus` gesetzt, ruft die App beim Start `robot.disable_wobbling()` statt `robot.enable_wobbling()` (Daemon- und SDK-Seite); sie loggt einmal „Kopfwackeln aus“ oder „Kopfwackeln an“. Ohne die Variable schaltet sie das Wackeln ein wie Pollen | Vergleich in G2: wirkt MoMo ohne Kopfwackeln beim Sprechen besser, Gefühl nur über den Ausdruck? Nur die App weiß, wann sie startet; ein Abschalten von außen träfe auch andere Apps. LoLa: `docs/FAHRPLAN.md` 7a-4, ENTSCHEIDUNGEN 12. November 2026 | Pollens `tests/test_main.py` grün (ganze Suite 512 grün; 3 rote in `test_personality_avatars.py` sind ohne die Änderung genauso rot); Logzeile „Kopfwackeln …“ im `app.log` von LoLa; in G2 Kopf beim Sprechen ruhig |
