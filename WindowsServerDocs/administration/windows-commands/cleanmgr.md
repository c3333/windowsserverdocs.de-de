---
title: cleanmgr
description: Konfigurieren Sie das Tool für die Datenträger Bereinigung (Cleanmgr.exe), um bestimmte Dateien automatisch zu bereinigen.
ms.reviewer: cosmosdarwin
author: JasonGerend
ms.author: jgerend
manager: daveba
ms.date: 06/20/2019
ms.openlocfilehash: dd8a015ff27809d0ef960241ce9221b4215c20aa
ms.sourcegitcommit: 7499749ce7baaf58a523cae2dd46737d635475ce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043882"
---
# <a name="cleanmgr"></a>cleanmgr

> Gilt für: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (halbjährlicher Kanal)

Löscht unnötige Dateien von der Festplatte Ihres Computers. Sie können Befehlszeilenoptionen verwenden, um anzugeben, dass **cleanmgr** temporäre Dateien, Internet Dateien, heruntergeladene Dateien und Papierkorb Dateien bereinigt. Anschließend können Sie die Ausführung der Aufgabe zu einem bestimmten Zeitpunkt mithilfe des Tools " **geplante Aufgaben** " planen.

## <a name="syntax"></a>Syntax

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
| --------- | ----------- |
| /d `<driveletter>` | Gibt das Laufwerk an, das Sie bereinigen möchten.<p>**Hinweis:** Die **/d** -Option wird mit nicht verwendet `/sagerun:n` . |
| /sageset: n | Zeigt das Dialogfeld Einstellungen für die Datenträger **Bereinigung** an und erstellt außerdem einen Registrierungsschlüssel zum Speichern der von Ihnen ausgewählten Einstellungen. Der- `n` Wert, der in der Registrierung gespeichert ist, ermöglicht Ihnen das Angeben von Aufgaben für die Datenträger Bereinigung, die ausgeführt werden soll. Der `n` Wert kann ein beliebiger ganzzahliger Wert zwischen 0 und 9999 sein. |
| /sagerun: n | Führt die angegebenen Aufgaben aus, die dem n-Wert zugewiesen werden, wenn Sie die **/sageset** -Option verwenden. Alle Laufwerke auf dem Computer werden aufgezählt, und das ausgewählte Profil wird für jedes Laufwerk ausgeführt. |
| /TuneUp: n | Führen Sie **/sageset** und **/sagerun** für das gleiche aus `n` . |
| /lowdisk | Führen Sie mit den Standardeinstellungen aus. |
| /verylowdisk | Führen Sie mit den Standardeinstellungen aus, keine Eingabe Aufforderungen. |
| /? | Zeigt die Hilfe an der Eingabeaufforderung an. |

#### <a name="options"></a>Optionen

Zu den Optionen für die Dateien, die Sie für die Datenträger Bereinigung mithilfe von **/sageset** und **/sagerun** angeben können, gehören:

- **Temporäre Setup Dateien** : Hierbei handelt es sich um Dateien, die von einem Setup Programm erstellt wurden, das nicht mehr ausgeführt wird.

- **Heruntergeladene Programmdateien** : heruntergeladene Programmdateien sind ActiveX-Steuerelemente und Java-Programme, die automatisch aus dem Internet heruntergeladen werden, wenn Sie bestimmte Seiten anzeigen. Diese Dateien werden temporär im Ordner "heruntergeladene Programmdateien" auf der Festplatte gespeichert. Diese Option umfasst die Schaltfläche Dateien anzeigen, sodass Sie die Dateien sehen können, bevor Sie von der Datenträger Bereinigung entfernt werden. Mit der Schaltfläche wird der Ordner "c:\winnt\herunter geladene Programmdateien" geöffnet.

- **Temporäre Internetdateien** : der Ordner "temporäre Internetdateien" enthält Webseiten, die für die schnelle Anzeige auf der Festplatte gespeichert sind. Mit der Datenträger Bereinigung werden diese Seiten entfernt, die personalisierten Einstellungen für Webseiten bleiben jedoch erhalten. Diese Option enthält auch die Schaltfläche Dateien anzeigen, die den Ordner "c:\Dokumente und Einstellungen\Benutzername\Lokale Einstellungen\Temporäre Internet Dateien \ Content.IE5" öffnet.

- **Alte CHKDSK-Dateien** : Wenn Chkdsk einen Datenträger auf Fehler überprüft, speichert CHKDSK möglicherweise verlorene Dateifragmente als Dateien im Stamm Ordner auf dem Datenträger. Diese Dateien sind nicht erforderlich.

- **Papier** Korb: der Papierkorb enthält Dateien, die Sie vom Computer gelöscht haben. Diese Dateien werden erst dauerhaft entfernt, wenn Sie den Papierkorb leeren. Diese Option umfasst die Schaltfläche Dateien anzeigen, mit der der Papierkorb geöffnet wird.<p>**Hinweis:** Ein Papierkorb kann in mehr als einem Laufwerk angezeigt werden, beispielsweise nicht nur in% SystemRoot%.

- **Temporäre Dateien** : Programme speichern manchmal temporäre Informationen in einem temporären Ordner. Vor dem Ausführen eines Programms löscht das Programm diese Informationen in der Regel. Sie können temporäre Dateien, die nicht innerhalb der letzten Woche geändert wurden, sicher löschen.

- **Temporäre Offlinedateien** temporäre Offline Dateien sind lokale Kopien der zuletzt verwendeten Netzwerkdateien. Diese Dateien werden automatisch zwischengespeichert, sodass Sie Sie nach dem Trennen der Verbindung mit dem Netzwerk verwenden können. Die Schaltfläche **Dateien anzeigen** öffnet den Ordner Offlinedateien.

- **Offlinedateien** -Offline Dateien sind lokale Kopien von Netzwerkdateien, die Sie speziell offline zur Verfügung stellen möchten, sodass Sie Sie nach dem Trennen der Verbindung mit dem Netzwerk verwenden können. Die Schaltfläche **Dateien anzeigen** öffnet den Ordner Offlinedateien.

- **Alte Dateien komprimieren** : Windows kann Dateien komprimieren, die Sie in jüngster Zeit nicht verwendet haben. Durch das Komprimieren von Dateien wird Speicherplatz gespart, aber Sie können die Dateien weiterhin verwenden. Es werden keine Dateien gelöscht. Da Dateien mit unterschiedlichen Raten komprimiert werden, ist die angezeigte Menge an Speicherplatz ungefähr annähernd. Mit der Schaltfläche "Optionen" können Sie angeben, wie viele Tage gewartet werden soll, bevor eine Datenträger Bereinigung eine nicht verwendete Datei komprimiert.

- **Katalogdateien für den inhaltsindexer** : der Indizierungs Dienst beschleunigt und verbessert die Dateisuche, indem ein Index der Dateien auf dem Datenträger verwaltet wird. Diese Katalogdateien bleiben von einem vorherigen Indizierungs Vorgang und können problemlos gelöscht werden.<p>**Hinweis:** Die Katalog Datei wird möglicherweise in mehr als einem Laufwerk angezeigt, z. b. nicht nur in `%SystemRoot%` .

>[!NOTE]
> Wenn Sie die Bereinigung des Laufwerks angeben, das die Windows-Installation enthält, sind alle diese Optionen auf der Registerkarte "Datenträger **Bereinigung** " verfügbar. Wenn Sie ein anderes Laufwerk angeben, sind nur die Optionen Papierkorb und Katalogdateien für Inhalts Index auf der Registerkarte Datenträger **Bereinigung** verfügbar.

## <a name="examples"></a>Beispiele

Geben Sie Folgendes ein, um die Datenträger Bereinigungs-APP so auszuführen, dass Sie in Ihrem Dialogfeld Optionen für die spätere Verwendung angeben können, indem Sie die Einstellungen in Set **1** speichern:

```
cleanmgr /sageset:1
```

Zum Ausführen der Datenträger Bereinigung und einschließen der Optionen, die Sie mit dem Befehl cleanmgr/sageset: 1 angegeben haben, geben Sie Folgendes ein:

```
cleanmgr /sagerun:1
```

Geben Sie Folgendes ein `cleanmgr /sageset:1` , um und `cleanmgr /sagerun:1` zusammenzuführen:

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Weitere Verweise

- [Freigeben von Laufwerksspeicherplatz unter Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
