---
slug: 2024-04-03-newsletter
title: KCL Newsletter (2024.03.20 - 2024.04.03)
authors:
  name: KCL Team
  title: KCL Team
tags: [KCL, Newsletter]
image: /img/biweekly-newsletter.png
---

![](/img/biweekly-newsletter.png)

[KCL](https://github.com/kcl-lang) ist eine auf Einschränkungen basierende rekursive und funktionale Sprache, die von der Cloud Native Computing Foundation (CNCF) gehostet wird und die Erstellung komplexer Konfigurationen und Richtlinien, einschließlich solcher für Cloud-native Szenarien, erleichtert. Mit ihrer fortschrittlichen Programmiersprachentechnologie und -praxis ist KCL darauf ausgerichtet, eine bessere Modularität, Skalierbarkeit und Stabilität für Konfigurationen zu fördern. Sie ermöglicht einfacheres Schreiben von Logik und bietet eine einfache Automatisierung von APIs sowie Integration mit hausinternen Systemen.

Dieser Abschnitt wird die neuesten Nachrichten der KCL language community's aktualisieren, einschließlich Funktionen, Website Updates und den neuesten community nachrichten, um allen ein besseres Verständnis der KCL Community zu ermöglichen!

**_KCL Website: [https://kcl-lang.io](https://kcl-lang.io)_**

## Übersicht

Vielen Dank an alle Mitwirkenden für ihre herausragende Arbeit in den letzten zwanzig Tagen (06.03.2024 - 20.03.2024).

Hier ist ein Überblick über die wichtigsten Inhalte:

**🏄 Language Updates**
**Veröffentlichung der Versionen 0.8.3 und 0.8.4 für KCL**

Die Hauptaktualisierungen:

- Hinzufügen von built-in methods für das Dateisystem: `file.abs` zur Berechnung des absoluten Dateipfads und `file.exists` zur Überprüfung, ob eine Datei existiert.

**🔧 Toolchain Updates**

- KCL-Paketmanager: Unterstützung für OCI-URLs in der `kcl.mod` hinzugefügt.
- KCL-Paketmanager: Entfernen von Updates für indirekte dependencies in `kcl.mod`.
- KCL-Paketmanager: Entfernen der checksum für lokale dependencies.
- KCL-Paketmanager: Korrektur eines Problems, bei dem Versionsnummern lokaler dependencies fehlten.
- KCL-Paketmanager: Behebung des Problems fehlender lokaler dependencies.
- KCL-Paketmanager: Behebung eines internen Fehlers, der das Erstellen von symbolischen Verknüpfungen scheitern ließ.

**🔥 SDK Updates**

- Veröffentlichung der Version 0.8.3 für das KCL Go SDK.
- KCL Go SDK hat ein Problem behoben, bei dem während des ParseFile Prozesses ein Panic issue auftrat.
- KCL Go SDK unterstützt jetzt das automatische Downloads des KCL Compilers über environment.

**💻 IDE Update**

- Fixing Formatierungs function der IDE für ungespeicherten Code.

## Special Thanks

Wir möchten uns bei allen Teilnehmern der Community der letzten zwei Wochen bedanken.

Die folgenden sind in keiner bestimmten Reihenfolge aufgeführt:

- Vielen Dank an @bozaro für seine Beiträge zum KCL Go SDK 🙌
- Vielen Dank an @reckless-huang für seine Beiträge zum KCL Go SDK 🙌
- Vielen Dank an @vemoo für seine Beiträge zur KCL IDE 🙌
- Vielen Dank an @wilsonwang371 für seine Beiträge zum KCL Docker-Image und zur KCL-Website 🙌
- Vielen Dank an @d4v1d03 für seine Beiträge zur KCL-Website 🙌
- Vielen Dank an @liangyuanpeng für seine Beiträge zur KCL GitHub-Action 🙌
- Vielen Dank an @octonawish-akcodes für seine Beiträge zur KCL IDE 🙌
- Vielen Dank an @AkashKumar7902 für seine Beiträge zum KCL-Paketverwaltungstool 🙌
- Vielen Dank an @empath-nirvana für seine Beiträge zur Crossplane-Funktion-KCL 🙌
- Vielen Dank an @reckless-huang, @steeling, @vfarcic, @wilsonwang371 und andere für ihre wertvollen Vorschläge und Feedback während der jüngsten Nutzung von KCL 🙌

## Resources

❤️ Siehe dir [here](https://github.com/kcl-lang/community) an um uns zu Folgen!

Für weitere Ressourcen, bitte siehe dir folgendes an:

- [KCL Website](https://kcl-lang.io/)
- [KusionStack Website](https://kusionstack.io/)
- [KCL v0.9.0 Milestone](https://github.com/kcl-lang/kcl/milestone/9)
