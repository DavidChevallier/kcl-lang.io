---
sidebar_position: 1
---

# Wie trägt man zur Dokumentation bei?

Dieses Dokument umfasst hauptsächlich partielle Änderungen an vorhandenen Dokumenten. Wenn du Blogbeiträge einreichst, neue Dokumente hinzufügst oder die Verzeichnisstruktur des Dokuments anpasst, kontaktiere bitte zuerst Teammitglieder.

Die KCL-Dokumente sind in Benutzerhandbücher, Entwicklungsunterlagen, interne Dokumente, Referenzhandbücher und Blogartikel unterteilt. Ihre Unterschiede sind wie folgt:

- Benutzerhandbuch: Das entsprechende Benutzerdokument ermöglicht es Benutzern, das KCL-Tool schnell zu verwenden, um ihre Arbeit mit minimalen Kosten zu erledigen, ohne zu viele interne Prinzipien und Implementierungen einzubeziehen.
- Referenz: KCL-Sprache, Tools, IDE und andere Dokumente mit allen Funktionen, die den umfangreichsten, aber trivialen Inhalt abdecken.
- Blog: Hier gibt es keine speziellen Einschränkungen. Sie können für spezifische Szenarien, technische Punkte oder allgemeine Entwicklungsansichten geteilt werden.

Beim Beitrag unterschiedlicher Dokumenttypen ist es am besten, die oben genannte Positionierung zu kombinieren, um eine angemessene Anpassung für unterschiedliche Inhalte vorzunehmen und den Lesern das bestmögliche Erlebnis zu bieten.

## 1. Grundlegende Spezifikationen

- Neben dem Titel sollten die internen Untertitel nach Möglichkeit nummeriert werden, um das Lesen zu erleichtern.
- Das automatisch vom Tool ausgegebene Dokument benötigt einen Link zum Quellcode, und der Untertitel kann ohne Nummer sein.
- Versuche, keine großen Codeabschnitte einzufügen (innerhalb von 30 Zeilen). Es ist besser, Texterklärungen und entsprechende Referenzlinks für den Code bereitzustellen.
- Es gibt Diagramme und Abbildungen, aber übermäßig komplexe Architekturdiagramme werden nicht empfohlen.
- Interne Verknüpfung: in Form eines absoluten Pfades [`/docs/user_docs/getting-started/intro`](/docs/user_docs/getting-started/intro).

**Zeichensetzung und Leerzeichen**

- In chinesischen Dokumenten wird chinesische Zeichensetzung bevorzugt.
- Ein Leerzeichen ist zwischen Chinesisch und Englisch erforderlich.
- Ein Leerzeichen muss zwischen Chinesisch und Zahlen hinzugefügt werden.
- Chinesisch verwendet Vollbreitenzeichen ohne Leerzeichen vor und nach der Zeichensetzung.
- Englischer Inhalt verwendet Halbbreitenzeichen mit einem Leerzeichen nach der Zeichensetzung.
- Du musst einen Abstand vor und nach dem Link lassen, aber du musst keinen Abstand in der Nähe des Absatzanfangs und der chinesischen Vollbreitenzeichen hinzufügen.

**Bild- und Ressourcendateinamen**

- The file name und directory name can only use numbers, English letters und underscores`_` And minus sign '-'
- Pictures of the current document are placed in the images directory of the current directory
- Vector pictures can be viewed through [drawio offline version](https://github.com/jgraph/drawio-desktop/releases) (und submit source files at the same time), und export png format pictures at 200% resolution

- Der Dateiname und Verzeichnisname dürfen nur Zahlen, englische Buchstaben und Unterstriche `_` und Minuszeichen '-' verwenden.
- Bilder des aktuellen Dokuments werden im Bilder Verzeichnis des aktuellen Verzeichnisses platziert.
- Vektorgrafiken können über [drawio offline version](https://github.com/jgraph/drawio-desktop/releases) angesehen werden (und gleichzeitig die source eingereicht werden) und als PNG Format bilder mit 200% Auflösung exportiert werden.

## 2. Grundlegender Modus der Verwendung von Dokumentinhalten

Jedes Benutzerhandbuch kann als relativ vollständiger Beitrag oder Blogbeitrag angesehen werden (das reference manual ist nicht mehr so). 
Die Organisation von Inhalten in den reference manuals folgt dem folgenden Muster:

1. Übersicht: Welche Probleme möchtest du in diesem Artikel lösen und welche Effekte möchtest du erzielen? Du kannst zuerst einen Screenshot des endgültigen Effekts einfügen.
2. Abhängige Umgebung: Welche Tools müssen installiert sein, und bereitstelle relevante Links.
3. Einführung: In diesem Artikel wird ein Beziehungsdiagramm oder Architekturdiagramm der Ressourcen erstellt.
4. Gib die Testmethode an. Versuche, gängige Methoden der Community (wie kube, curl command oder Browser) zum Testen zu verwenden.
5. Zusammenfassung und Outlook. Überprüfe kurz den aktuellen process und einige Stellen, die erweitert werden können (du kannst einige Links angeben werden).

## 3. Testing und Einreichen von PRs

Clone zuerst das Dokumenten Repository und teste dann die lokale Instanz mit den Befehlen 'npm run start' und 'npm run build', um sicherzustellen, dass du normal navigieren kannst, und reiche dann den PR ein.
