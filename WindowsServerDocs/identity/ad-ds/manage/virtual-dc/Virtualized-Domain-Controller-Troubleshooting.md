---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: Problembehandlung bei virtualisierten Domänencontrollern
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 28e1f82322389e7b46b7d597b6657f9512ab011f
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071262"
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Problembehandlung bei virtualisierten Domänencontrollern

>Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dieses Thema enthält eine ausführliche Methode für die Problembehandlung virtualisierter Domänencontroller.

- [Problembehandlung beim Klonen von virtualisierten Domänencontrollern](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)

- [Problembehandlung bei bei der sicheren Wiederherstellung des virtualisierten Domänencontrollers](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)

## <a name="introduction"></a><a name="BKMK_Intro"></a>Einführung
Die wichtigste Methode zur Verbesserung Ihrer Fehlerbehandlungsfertigkeiten ist der Aufbau eines Testlabors und die gründliche Untersuchung von normalen, funktionierenden Szenarien. Wenn Sie auf Fehler treffen, sind diese offensichtlicher und einfacher zu verstehen, da Sie dann ein solides Grundlagenwissen zur Funktionsweise des Heraufstufens von Domänencontrollern erlangt haben. Auf diese Weise können Sie auch Ihre Analyse- und Netzwerkanalysefertigkeiten weiterentwickeln. Dies gilt für alle Technologien für verteilte Systeme, nicht nur für die Bereitstellung virtualisierter Domänencontroller.

Die wichtigen Elemente für eine erweiterte Fehlerbehandlung bei der Domänencontrollerkonfiguration sind die folgenden:

1. Lineare Analyse in Kombination mit einer Fokussierung und Aufmerksamkeit für Details

2. Verstehen der Netzwerkerfassungsanalyse

3. Verstehen der integrierten Protokolle

Der erste und zweite Punkt gehen über den Umfang dieses Themas hinaus, aber der dritte Punkt kann ausführlich erläutert werden. Für die Fehlerbehandlung bei virtualisierte Domänencontroller ist eine logische und lineare Methode erforderlich. Wichtig ist, das Problem mit den bereitgestellten Daten anzugehen und nur auf komplexe Tools und Analysen zurückzugreifen, wenn Sie die bereitgestellte Ausgabe und Protokollierung erschöpft haben.

## <a name="troubleshooting-virtualized-domain-controller-cloning"></a><a name="BKMK_TshootVDCCloning"></a>Problembehandlung beim Klonen von virtualisierten Domänencontrollern
In diesen Abschnitten werden folgende Themen behandelt:

- [Tools für die Problembehandlung](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)

- [Protokollierungs Optionen](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)

- [Allgemeine Methode für die Problembehandlung beim Klonen von Domänencontrollern](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)

- [Server Core und das Ereignisprotokoll](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)

- [Beheben von bestimmten Problemen](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)

Die Problembehandlungsstrategie für das Klonen virtueller Domänencontroller basiert auf dem folgenden allgemeinen Format:

![Problembehandlung bei Virtual DC](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)

### <a name="tools-for-troubleshooting"></a><a name="BKMK_Tools"></a>Tools für die Problembehandlung

#### <a name="logging-options"></a><a name="BKMK_LoggingOptions"></a>Protokollierungs Optionen

Die integrierten Protokolle sind das wichtigste Tool für die Problembehandlung beim Klonen von Domänencontrollern. Alle Protokolle sind standardmäßig für maximale Ausführlichkeit aktiviert und konfiguriert.

| **Vorgang** | **Log** |
|--|--|
| **Klonen?** | -Ereignisviewer\windows-Protokolle\System<br />-Ereignisviewer\anwendungs-und dienstprotokolle\verzeichnisdienst<br />-%systemroot%\debug\dcpromo.log |
| **Promotion** | -%systemroot%\debug\dcpromo.log<br />-Ereignisviewer\anwendungs-und dienstprotokolle\verzeichnisdienst<br />-Ereignisviewer\windows-Protokolle\System<br />-Ereignisviewer\anwendungs-und dienstprotokolle\datei Replikations Dienst<br />-Ereignisviewer\anwendungs-und dienstprotokolle\dfs-Replikation |

#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Tools und Befehle für die Problembehandlung bei der Konfiguration von Domänencontrollern
Verwenden Sie zum Beheben von Problemen, die nicht von den Protokollen erklärt werden, die folgenden Tools als Ausgangspunkt:

- Dcdiag.exe

- Repadmin.exe

- Network Monitor 3.4

### <a name="general-methodology-for-troubleshooting-domain-controller-cloning"></a><a name="BKMK_GeneralMethodology"></a>Allgemeine Methode für die Problembehandlung beim Klonen von Domänencontrollern

1. Wird der virtuelle Computer im Verzeichnisdienst-Wiederherstellungsmodus (DS Repair Mode, DSRM) gestartet? Das ist ein Hinweis, dass eine Problembehandlung erforderlich ist. Zum Anmelden beim DSRM verwenden Sie das Konto **.\Administrator** , und geben Sie das DSRM-Kennwort an.

    1. Untersuchen Sie die Datei Dcpromo.log.

        1. Waren die ersten Schritte beim Klonen erfolgreich, aber ist das Heraufstufen des Domänencontrollers fehlgeschlagen?

        2. Weisen Fehler auf Probleme mit dem lokalen Domänencontroller oder mit der AD DS-Umgebung hin, beispielsweise durch vom PDC-Emulator zurückgegebene Fehler?

    2. Untersuchen Sie die System- und Verzeichnisdienst-Ereignisprotokolle sowie die Dateien dccloneconfig.xml und CustomDCCloneAllowList.xml.

        1. Muss sich eine inkompatible Anwendung in der CustomDCCloneAllowList.xml-Liste „Zulassen“ befinden?

        2. Ist die IP-Adresse oder der Computername in der Datei dccloneconfig.xml entweder dupliziert oder ungültig?

        3. Ist der Active Directory-Standort in der Datei dccloneconfig.xml ungültig?

        4. Ist die IP-Adresse in der Datei dccloningconfig.xml nicht festgelegt und deshalb kein DHCP-Server verfügbar?

        5. Ist der PDC-Emulator online und über das RPC-Protokoll verfügbar?

        6. Ist der Domänencontroller Mitglied der Gruppe „Klonbare Domänencontroller“? Ist die Berechtigung **Domänencontroller die Erstellung eines Klons von sich selbst erlauben** am Domänenstamm für diese Gruppe festgelegt?

        7. Enthält die Datei Dccloneconfig.xml Syntaxfehler, die eine korrekte Analyse verhindern?

        8. Wird der Hypervisor unterstützt?

        9. Ist das Heraufstufen des Domänencontrollers fehlgeschlagen, nachdem das Klonen erfolgreich begonnen hat?

        10. Wurde die maximale Anzahl von automatisch generierten Domänencontrollernamen (9999) überschritten?

        11. Ist die MAC-Adresse dupliziert?

2. Ist der Hostname des Klons derselbe wie der des Quelldomänencontrollers?

    1. Ist eine Dccloneconfig.xml-Datei an einem der zulässigen Speicherorte vorhanden?

3. Wird der virtuelle Computer im normalen Modus gestartet und ist das Klonen abgeschlossen, aber der Domänencontroller funktioniert nicht ordnungsgemäß?

    1. Überprüfen Sie zunächst, ob der Hostname auf dem Klon geändert wurde. Wenn sich der Hostname unterscheidet, wurde das Klonen zumindest teilweise abgeschlossen.

    2. Hat der Domänencontroller eine duplizierte IP-Adresse des Quelldomänencontrollers aus der Datei dccloneconfig.xml, aber war der Quelldomänencontroller während des Klonens offline?

    3. Wenn sich der Domänencontroller ankündigt, behandeln Sie das Problem als ein normales Problem nach dem Heraufstufen, das Sie auch ohne das Klonen hätten.

    4. Wenn sich der Domänencontroller nicht ankündigt, untersuchen Sie die Verzeichnisdienst-, System-, Anwendungs-, Dateireplikations- und DFS-Replikationsereignisprotokolle auf Fehler nach dem Heraufstufen.

#### <a name="disabling-dsrm-boot"></a>Deaktivieren des DSRM-Starts
Nach dem Starten im DSRM aufgrund eines Fehlers, diagnostizieren Sie die Fehlerursache. Wenn die dcpromo.log-Datei nicht darauf hinweist, dass das Klonen nicht erneut versucht werden, beheben Sie die Fehlerursache, und setzen Sie das DSRM-Kennzeichen zurück. Ein fehlerhafter Klon kehrt nicht von allein beim nächsten Neustart in seinen normalen Modus zurück, sondern Sie müssen das DSRM-Startkennzeichen löschen, um den Klonvorgang erneut auszuführen. Für all diese Schritte ist eine Ausführung als erweiterter Administrator erforderlich.

##### <a name="removing-dsrm-with-msconfigexe"></a>Entfernen von DSRM mit Msconfig.exe
Verwenden Sie das Systemkonfigurationstool, um den DSRM-Start über eine GUI zu deaktivieren:

1. Führen Sie msconfig.exe aus.

2. Deaktivieren Sie auf der Registerkarte **Start** unter **Startoptionen** die Option **Abgesicherter Start** (diese Option ist bereits mit der **Active Directory-Reparatur** aktiviert).

3. Klicken Sie auf OK, und starten Sie nach Aufforderung neu.

##### <a name="removing-dsrm-with-bcdeditexe"></a>Entfernen von DSRM mit Bcdedit.exe
Verwenden Sie den Editor für den Startkonfigurationsdaten-Speicher, um den DSRM-Start über die Befehlszeile zu deaktivieren:

1. Öffnen Sie eine CMD-Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

    ```
    Bcdedit.exe /deletevalue safeboot
    ```

2. Starten Sie den Computer mit dem folgenden Befehl neu:

    ```
    Shutdown.exe /t /0 /r
    ```

> [!NOTE]
> Bcdedit.exe funktioniert auch in einer Windows PowerShell-Konsole. Dabei werden die folgenden Befehle verwendet:
>
> Bcdedit.exe /deletevalue safeboot
>
> Restart-computer

### <a name="server-core-and-the-event-log"></a><a name="BKMK_ServerCoreEvents"></a>Server Core und das Ereignisprotokoll
Die Ereignisprotokolle enthalten einen Großteil der nützlichen Informationen zu den Vorgängen beim Klonen des virtuellen Domänencontrollers. Standardmäßig ist eine Windows Server 2012-Computerinstallation eine Serverkerninstallation, was bedeutet, dass es keine grafische Benutzeroberfläche und damit keine Möglichkeit gibt, das lokale Snap-In „Ereignisanzeige“ auszuführen.

So überprüfen Sie die Ereignisprotokolle auf einem Server mit einer Serverkerninstallation:

- Führen Sie das Tool Wevtutil.exe lokal aus.

- Führen Sie das PowerShell-Cmdlet Get-WinEvent lokal aus.

- Wenn Sie die erweiterten Windows-Firewallregeln für die Gruppen "Remote-Ereignisprotokoll Verwaltung" (oder die entsprechenden Ports) aktiviert haben, um eingehende Kommunikation zuzulassen, können Sie das Ereignisprotokoll mithilfe von "Eventvwr.exe", "wevtutil.exe" oder "Get-WinEvent" Remote verwalten. Die kann auf Serverkerninstallationen mithilfe von NETSH.exe, der Gruppenrichtlinie oder dem neuen Cmdlet Set-NetFirewallRule in Windows PowerShell 3.0 durchgeführt werden.

> [!WARNING]
> Versuchen Sie nicht, die grafische Shell wieder zum Computer hinzuzufügen, während dieser sich im DSRM befindet. Der Windows-Bereitstellungsstapel (CBS) kann im abgesicherten Modus oder DSRM nicht ordnungsgemäß betrieben werden. Versuche, Features oder Rollen im DSRM hinzuzufügen, werden nicht abgeschlossen und lassen den Computer in einem instabilen Zustand zurück, bis er normal gestartet wird. Da ein virtualisierter Domänencontrollerklon im DSRM in den meisten Fällen nicht normal gestartet werden kann, ist es unmöglich, die grafische Shell auf sichere Weise hinzuzufügen. Eine solche Vorgehensweise wird nicht unterstützt und führt möglicherweise dazu, dass Ihr Server unbrauchbar ist.

### <a name="troubleshooting-specific-problems"></a><a name="BKMK_SpecificProblems"></a>Beheben von bestimmten Problemen

#### <a name="events"></a>Ereignisse
Alle Ereignisse beim Klonen eines virtualisierten Domänencontroller werden in das Verzeichnisdienst-Ereignisprotokoll des geklonten virtuellen Domänencontrollercomputers geschrieben. Das Anwendungs-, Dateireplikationsdienst- und DFS-Replikationsereignisprotokoll enthält möglicherweise auch nützliche Problembehandlungsinformationen für fehlgeschlagene Klonvorgänge. Fehler bei RPC-Aufrufen an den PDC-Emulator sind möglicherweise im Ereignisprotokoll auf dem PDC-Emulator verfügbar.

Im Folgenden sind die klonspezifischen Windows Server 2012-Ereignisse im Verzeichnisdienst-Ereignisprotokoll mit Notizen und vorgeschlagenen Lösungen für Fehler aufgeführt.

##### <a name="directory-services-event-log"></a>Verzeichnisdienst-Ereignisprotokoll

| Events | BESCHREIBUNG |
| -- |--|
| **Ereignis-ID** | **2160** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung**
| Die lokale *<COMPUTERNAME>* Datei hat eine Klon Konfigurationsdatei für virtuelle Domänen Controller gefunden.<p>Fundstelle der Klonkonfigurationsdatei: %1<p>Das Vorliegen einer Klonkonfigurationsdatei lässt darauf schließen, dass der lokale virtuelle Domänencontroller ein Klon eines anderen virtuellen Domänencontrollers ist. *<COMPUTERNAME>* Startet den Klon Vorgang. |
 **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. Überprüfen Sie das DSA-Arbeitsverzeichnis, %systemroot%\ntds, und den Stamm aller lokalen oder Wechseldatenträger auf die Datei dcclconeconfig.xml. |

| Events | BESCHREIBUNG |
| -- |--|
| **Ereignis-ID** | **2161** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Vom lokalen *<COMPUTERNAME>* wurde die Klon Konfigurationsdatei für den virtuellen Domänen Controller nicht gefunden. Folglich ist der lokale Computer kein geklonter Domänencontroller.| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. Überprüfen Sie das DSA-Arbeitsverzeichnis, %systemroot%\ntds, und den Stamm aller lokalen oder Wechseldatenträger auf die Datei dcclconeconfig.xml. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2162** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | Fehler beim Klonen des virtuellen Domänencontrollers.<p>Überprüfen Sie die in den Systemereignisprotokollen und in %systemroot%\debug\dcpromo.log protokollierten Ereignisse, um weitere Informationen zum Versuch des Klonens eines virtuellen Domänencontrollers zu erhalten.<p>Fehlercode: %1 |
| **Hinweise und Lösung** | Befolgen Sie die Anweisungen in der Meldung, dieser Fehler ist ein „catchall“. |

| Events | BESCHREIBUNG |
| -- |--|
|**Ereignis-ID**|**2163**|
|**Quelle**|Microsoft-Windows-ActiveDirectory_DomainService|
|**Severity**|Informational|
|**Meldung**|Der DsRoleSvc-Dienst zum Klonen des lokalen virtuellen Domänencontrollers wurde gestartet.|
|**Hinweise und Lösung**|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. Überprüfen Sie das DSA-Arbeitsverzeichnis, %systemroot%\ntds, und den Stamm aller lokalen oder Wechseldatenträger auf die Datei dcclconeconfig.xml.|

| Events | BESCHREIBUNG |
| -- |--|
| **Ereignis-ID** | **2164** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Starten des dsrolesvc-Dienes zum Klonen des lokalen virtuellen Domänen Controllers. |
| **Hinweise und Lösung** | Untersuchen Sie die Diensteinstellungen für den DS-Rollenserverdienst (DsRoleSvc), und stellen Sie sicher, dass der Starttyp auf normal festgelegt ist. Vergewissern Sie sich, dass kein Drittanbieterprogramm das Starten dieses Diensts verhindert. |

| Events | BESCHREIBUNG |
| -- |--|
| **Ereignis-ID** | **2165** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Starten eines Threads beim Klonen des lokalen virtuellen Domänen Controllers.<p>Fehlercode:%1<p>Fehlermeldung:%2<p>Name des Threads:%3 |
| **Hinweise und Lösung** | Wenden Sie sich an den Microsoft-Produktsupport. |

| Events | BESCHREIBUNG |
| -- |--|
| **Ereignis-ID** | **2166** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* erfordert den RPCSS-Dienst, um einen Neustart in DSRM zu initiieren. Fehler beim Warten auf das Initialisieren des RPCSS-Diensts bis zum Erreichen eines Ausführungsstatus.<p>Fehlercode:%1 |
| **Hinweise und Lösung** | Untersuchen Sie die Systemereignisprotokolle und die Diensteinstellungen für den RPC-Serverdienst (Rpcss) |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2168** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Microsoft-Windows-ActiveDirectory_DomainService<p>Der Domänencontroller wird auf einem unterstützten Hypervisor ausgeführt. Eine VM-Generations-ID wurde erkannt.<p>Aktueller Wert der VM-Generations-ID: %1 |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2169** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Es wurde keine VM-Generations-ID erkannt. Der Host des Domänencontrollers ist ein physischer Computer, eine ältere Version von Hyper-V oder ein Hypervisor, der VM-Generations-IDs nicht unterstützt.<p>Zusätzliche Daten<p>Fehlercode beim Überprüfen der VM-Generations-ID:%1 |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis, wenn kein Klonen beabsichtigt ist. Andernfalls untersuchen Sie das Systemereignisprotokoll, und lesen Sie die Produktsupportdokumentation zum Hypervisor noch einmal durch. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2170** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Warnung |
| **Meldung** | Es wurde keine Änderung der Generations-ID erkannt.<p>Zwischengespeicherte Generations-ID (DS, alter Wert):%1<p>Aktuelle Generations-ID des virtuellen Computers (neuer Wert):%2<p>Die Änderung der Generations-ID erfolgt nach der Anwendung einer Momentaufnahme des virtuellen Computers, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. *<COMPUTERNAME>* erstellt eine neue Aufruf-ID, um den Domänen Controller wiederherzustellen. Virtualisierte Domänencontroller dürfen nicht mit einer Momentaufnahme des virtuellen Computers wiederhergestellt werden. Das unterstützte Verfahren zum Wiederherstellen oder Zurücksetzen des Inhalts einer Datenbank der Active Directory-Domänendienste ist das Wiederherstellen einer Systemstatussicherung, die mit einer für die Active Directory-Domänendienste geeigneten Sicherungsanwendung erstellt wurde. |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis, wenn ein Klonen beabsichtigt ist. Andernfalls untersuchen Sie das Systemereignisprotokoll. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2171** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Es wurde keine Änderung der Generations-ID erkannt.<p>Zwischengespeicherte Generations-ID (DS, alter Wert):%1<p>Aktuelle Generations-ID des virtuellen Computers (neuer Wert):%2 |
| **Hinweise und Lösung** | Dies ist eine Erfolgsmeldung, wenn kein Klonen beabsichtigt ist, die bei jedem Neustart eines virtualisierten Domänencontrollers angezeigt werden sollte. Andernfalls untersuchen Sie das Systemereignisprotokoll. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2172** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Das msDS-GenerationId-Attribut des Domänencontrollers wurde gelesen.<p>Wert des msDS-GenerationId-Attributs:%1 |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis, wenn ein Klonen beabsichtigt ist. Andernfalls untersuchen Sie das Systemereignisprotokoll. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2173** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Fehler beim Lesen des msDS-GenerationId-Attributs des Computerobjekts des Domänencontrollers. Mögliche Ursachen sind, dass eine Datenbanktransaktion nicht korrekt ausgeführt wurde oder dass die Generations-ID in der lokalen Datenbank nicht vorhanden ist. Zu bedenken ist auch, dass beim ersten Neustart nach einem DCPromo-Vorgang noch kein msDS-GenerationId-Attribut vorhanden ist.<p>Zusätzliche Daten<p>Fehlercode:%1 |
| **Hinweise und Lösung** | Dies ist eine Erfolgsnachricht, wenn ein Klonen beabsichtigt ist und es sich um den ersten Neustart des virtuellen Computers nach dem Klonen handelt. Auf nicht virtuellen Domänencontrollern kann sie ignoriert werden. Andernfalls untersuchen Sie das Systemereignisprotokoll. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2174** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Der Domänencontroller ist weder ein Klon noch eine wiederhergestellte Momentaufnahme eines virtuellen Domänencontrollers. |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis, wenn kein Klonen beabsichtigt ist. Andernfalls untersuchen Sie das Systemereignisprotokoll. |

| Events | BESCHREIBUNG |
| -- |--|
|**Ereignis-ID**|**2175**|
|**Quelle**|Microsoft-Windows-ActiveDirectory_DomainService|
|**Severity**|Fehler|
|**Meldung**|Die Konfigurationsdatei zum Klonen des virtuellen Domänencontrollers befindet sich auf einer nicht unterstützten Plattform.|
|**Hinweise und Lösung**|Zu dieser Meldung kommt es, wenn eine dccloneconfig.xml-Datei, aber keine VM-Generations-ID gefunden wird, beispielsweise wenn eine dccloneconfig.xml-Datei auf einem physischen Computer oder einem Hypervisor gefunden wird, der die VM-Generations-ID nicht unterstützt.|

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2176** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Die Klonkonfigurationsdatei für virtuelle Domänencontroller wurde umbenannt.<p>Zusätzliche Daten<p>Bisheriger Dateiname:%1<p>Neuer Dateiname:%2 |
| **Hinweise und Lösung** | Die Umbenennung ist erwartet, wenn eine Sicherung eines virtuellen Quellcomputers gestartet wird, da sich die VM-Generations-ID nicht geändert hat. Dies verhindert, dass der Quelldomänencontroller einen Klonvorgang versucht. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2177** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | Fehler beim Umbenennen der Konfigurationsdatei zum Klonen des virtuellen Domänencontrollers.<p>Zusätzliche Daten<p>Dateiname:%1<p>Fehlercode:%2 %3 |
| **Hinweise und Lösung** | Der Umbenennungsversuch ist erwartet, wenn eine Sicherung eines virtuellen Quellcomputers gestartet wird, da sich die VM-Generations-ID nicht geändert hat. Dies verhindert, dass der Quelldomänencontroller einen Klonvorgang versucht. Nennen Sie die Datei manuell um, und untersuchen Sie installierte Drittanbieterprodukte, die möglicherweise die Umbenennung der Datei verhindern. |

| Events | BESCHREIBUNG |
| -- |--|
|**Ereignis-ID**|**2178**|
|**Quelle**|Microsoft-Windows-ActiveDirectory_DomainService|
|**Severity**|Informational|
|**Meldung**|Eine Klonkonfigurationsdatei für virtuelle Domänencontroller wurde erkannt, doch die VM-Generations-ID wurde nicht geändert. Der lokale Domänencontroller ist der Quelldomänencontroller für den Klonvorgang. Benennen Sie die Klonkonfigurationsdatei um.|
|**Hinweise und Lösung**|Erwartet, wenn eine Sicherung eines virtuellen Quellcomputers gestartet wird, da sich die VM-Generations-ID nicht geändert hat. Dies verhindert, dass der Quelldomänencontroller einen Klonvorgang versucht.|

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2179** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Für das msDS-GenerationId-Attribut des Computerobjekts des Domänencontrollers wurde der folgende Parameter festgelegt:<p>Generations-ID-Attribut:%1 |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2180** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Warnung |
| **Meldung** | Fehler beim Festlegen des Attributs "msDS-GenerationId" für das Computerobjekt des Domänencontrollers.<p>Zusätzliche Daten<p>Fehlercode:%1 |
| **Hinweise und Lösung** | Untersuchen Sie das Systemereignisprotokoll und die Datei Dcpromo.log. Suchen Sie unter MS TechNet, in der MS Knowledge Base und in MS-Blogs nach dem bestimmten Fehler, um seine gewöhnliche Bedeutung zu bestimmen und ihn dann auf der Basis dieser Ergebnisse zu behandeln. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2182** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Internes Ereignis: der Verzeichnisdienst wurde zum Klonen eines remotedsa aufgefordert: |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2183** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Internes Ereignis: *<COMPUTERNAME>* die Anforderung zum Klonen des Remote Verzeichnis System-Agents wurde abgeschlossen.<p>Name des ursprünglichen DC:%3<p>Name des angeforderten Klon-DC:%4<p>Standort des angeforderten Klon-DC:%5<p>Zusätzliche Daten<p>Fehlerwert:%1 %2 |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2184** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Erstellen eines Domänen Controller Kontos für den geklonten Domänen Controller.<p>Name des ursprünglichen DC: %1<p>Zulässige Anzahl geklonter DCs:%2<p>Der Grenzwert für die Anzahl der Domänen Controller Konten, die durch Klonen generiert werden können, <em> <COMPUTERNAME> </em> wurde überschritten. |
| **Hinweise und Lösung** | Der Name eines einzigen Quelldomänencontrollers kann basieren auf der Benennungskonvention nur 9.999-mal automatisch generiert werden, wenn die Domänencontroller nicht herabgestuft werden. Verwenden Sie das Elemente <computername> in der XML, um einen neuen eindeutigen Namen zu generieren oder von einem Domänencontroller mit einem anderen Namen zu klonen. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2191** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* Legen Sie den folgenden Registrierungs Wert fest, um DNS-Updates zu deaktivieren.<p>Registrierungswert:%1<p>Registrierungswert: %2<p>Registrierungswertdaten: %3<p>Während des Klonens sind die Computernamen des lokalen Computers und des Quellcomputers möglicherweise kurzzeitig identisch. Für diese Zeitspanne wird die Registrierung von DNS A- und AAAA-Datensätzen deaktiviert, sodass Clients keine Anforderungen an den lokalen Computer senden können. Nach dem Klonen werden DNS-Aktualisierungen vom Klonprozess wieder aktiviert. |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2192** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* der folgende Registrierungs Wert zum Deaktivieren von DNS-Aktualisierungen konnte nicht festgelegt werden.<p>Registrierungswert:%1<p>Registrierungswert: %2<p>Registrierungswertdaten: %3<p>Fehlercode: %4<p>Fehlermeldungen: %5<p>Während des Klonens sind die Computernamen des lokalen Computers und des Quellcomputers möglicherweise kurzzeitig identisch. Für diese Zeitspanne wird die Registrierung von DNS A- und AAAA-Datensätzen deaktiviert, sodass Clients keine Anforderungen an den lokalen Computer senden können. |
| **Hinweise und Lösung** | Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise Registrierungsaktualisierungen blockieren. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2193** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* Legen Sie den folgenden Registrierungs Wert fest, um DNS-Updates zu aktivieren.<p>Registrierungswert:%1<p>Registrierungswert: %2<p>Registrierungswertdaten: %3<p>Während des Klonens sind die Computernamen des lokalen Computers und des Quellcomputers möglicherweise kurzzeitig identisch. Für diese Zeitspanne wird die Registrierung von DNS A- und AAAA-Datensätzen deaktiviert, sodass Clients keine Anforderungen an den lokalen Computer senden können. |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2194** |
|--|--|
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* der folgende Registrierungs Wert zum Aktivieren von DNS-Updates konnte nicht festgelegt werden.<p>Registrierungswert:%1<p>Registrierungswert: %2<p>Registrierungswertdaten: %3<p>Fehlercode: %4<p>Fehlermeldungen: %5<p>Während des Klonens sind die Computernamen des lokalen Computers und des Quellcomputers möglicherweise kurzzeitig identisch. Für diese Zeitspanne wird die Registrierung von DNS A- und AAAA-Datensätzen deaktiviert, sodass Clients keine Anforderungen an den lokalen Computer senden können. |
| **Hinweise und Lösung** | Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise Registrierungsaktualisierungen blockieren. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2195** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | Fehler beim Festlegen des DSRM-Starts.<p>Fehlercode:%1<p>Fehlermeldung:%2<p>Wenn beim Klonen eines virtuellen Domänencontrollers Fehler auftreten oder wenn der Hypervisor mit der Klonkonfigurationsdatei nicht unterstützt wird, wird der lokale Computer zur Problembehandlung im Verzeichnisdienst-Wiederherstellungsmodus (DSRM) neu gestartet. Der DSRM-Start konnte jedoch nicht festgelegt werden. |
| **Hinweise und Lösung** | Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise Registrierungsaktualisierungen blockieren. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2196** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | Fehler beim Aktivieren des Rechts zum Herunterfahren.<p>Fehlercode:%1<p>Fehlermeldung:%2<p>Wenn beim Klonen eines virtuellen Domänencontrollers Fehler auftreten oder wenn der Hypervisor mit der Klonkonfigurationsdatei nicht unterstützt wird, wird der lokale Computer zur Problembehandlung im Verzeichnisdienst-Wiederherstellungsmodus (DSRM) neu gestartet. Das Recht zum Herunterfahren konnte jedoch nicht aktiviert werden. |
| **Hinweise und Lösung** | Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise die Verwendung von Berechtigungen blockieren. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2197** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | Fehler beim Initiieren des Herunterfahrens.<p>Fehlercode:%1<p>Fehlermeldung:%2<p>Wenn beim Klonen eines virtuellen Domänencontrollers Fehler auftreten oder wenn der Hypervisor mit der Klonkonfigurationsdatei nicht unterstützt wird, wird der lokale Computer zur Problembehandlung im Verzeichnisdienst-Wiederherstellungsmodus (DSRM) neu gestartet. Das Herunterfahren des Systems konnte jedoch nicht initiiert werden. |
| **Hinweise und Lösung** | Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise die Verwendung von Berechtigungen blockieren. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2198** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Erstellen oder Ändern des folgenden geklonten DC-Objekts.<p>Zusätzliche Daten:<p>Objekt:<p>%1<p>Fehlerwert: %2<p>%3 |
| **Hinweise und Lösung** | Suchen Sie unter MS TechNet, in der MS Knowledge Base und in MS-Blogs nach dem bestimmten Fehler, um seine gewöhnliche Bedeutung zu bestimmen und ihn dann auf der Basis dieser Ergebnisse zu behandeln. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2199** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Erstellen des folgenden geklonten DC-Objekts, da das Objekt bereits vorhanden ist.<p>Zusätzliche Daten:<p>Quelldomänencontroller:<p>%1<p>Objekt:<p>%2 |
| **Hinweise und Lösung** | Vergewissern Sie sich, dass in der Datei dccloneconfig.xml kein vorhandener Domänencontroller angegeben ist und dass keine Kopien der Datei dccloneconfig.xml auf mehreren Klonen verwendet wurden, ohne den Namen zu bearbeiten. Wenn die Kollision immer noch unerwartet ist, legen Sie fest, welcher Administrator sie heraufgestuft hat. Wenden Sie sich an den Administrator, um zu besprechen, ob der vorhandene Domänencontroller herabgestuft, die Metadaten des vorhandenen Domänencontrollers bereinigt oder der Klon einen anderen Namen verwenden sollte. |

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2203** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | Fehler beim Klonen des letzten virtuellen Domänencontrollers. Dies ist der erste Neustart nach dem Fehler, daher sollte der Klonvorgang nun wiederholt werden. Es ist jedoch weder eine Konfigurationsdatei für das Klonen von virtuellen Domänencontrollern noch eine Änderung der VM-Generations-ID vorhanden. Starten Sie im DSRM.<p>Fehler beim Klonen des letzten virtuellen Domänencontrollers:%1<p>Die Konfigurationsdatei zum Klonen des virtuellen Domänencontrollers ist vorhanden:%2<p>Eine Änderung der VM-Generations-ID wird erkannt:%3 |
| **Hinweise und Lösung** | Erwartet, wenn das Klonen zuvor aufgrund einer fehlenden oder ungültigen dccloneconfig.xml-Datei fehlgeschlagen ist. |

| Events | BESCHREIBUNG |
|--|--|
| Ereignis-ID | 2210 |
| `Source` | Microsoft-Windows-ActiveDirectory_DomainService |
| severity | Fehler |
| `Message` | <COMPUTERNAME> konnte keine Objekte für den geklonten Domänencontroller erstellen.<p>Zusätzliche Daten:<p>Klon-ID: %6<p>Name des geklonten Domänencontrollers: %1<p>Wiederholungsschleife: %2<p>Ausnahmewert: %3<p>Fehlerwert: %4<p>DSID: %5 |
| Hinweise und Lösung | Überprüfen Sie die System- und Verzeichnisdienstprotokolle sowie die Datei dcpromo.log auf weitere Details für das Fehlschlagen des Klonens. |

| Events | BESCHREIBUNG |
|--|--|
| Ereignis-ID | 2211 |
| `Source` | Microsoft-Windows-ActiveDirectory_DomainService |
| severity | Informational |
| `Message` | <COMPUTERNAME> hat Objekte für den geklonten Domänencontroller erstellt.<p>Zusätzliche Daten:<p>Klon-ID: %3<p>Name des geklonten Domänencontrollers: %1<p>Wiederholungsschleife: %2 |
| Hinweise und Lösung | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| Ereignis-ID | 2212 |
| `Source` | Microsoft-Windows-ActiveDirectory_DomainService |
| severity | Informational |
| `Message` | <COMPUTERNAME> hat mit der Erstellung von Objekten für den geklonten Domänencontroller begonnen.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Klonname: %2<p>Klonsite: %3<p>Geklonter RODC: %4 |
| Hinweise und Lösung | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
|--|--|
| Ereignis-ID | 2213 |
| `Source` | Microsoft-Windows-ActiveDirectory_DomainService |
| severity | Informational |
| `Message` | <COMPUTERNAME> hat ein neues KrbTgt-Objekt für das Klonen schreibgeschützter Domänencontroller erstellt.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>GUID des neuen KrbTgt-Objekts: %2 |
| Hinweise und Lösung | Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist. |

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2214|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> erstellt ein Computerobjekt für den geklonten Domänencontroller.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Ursprünglicher Domänencontroller: %2<p>Geklonter Domänencontroller: %3|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2215|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> fügen den geklonten Domänencontroller in der folgenden Site hinzu.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Website: %2|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2216|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> erstellt einen Servercontainer für den geklonten Domänencontroller.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Servercontainer: %2|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2217|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> erstellt ein Serverobjekt für den geklonten Domänencontroller.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Serverobjekt: %2|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2218|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> erstellt ein Objekt mit NTDS-Einstellungen für den geklonten Domänencontroller.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Objekt: %2|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2219|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> erstellt Verbindungsobjekte für den geklonten schreibgeschützten Domänencontroller.<p>Zusätzliche Daten:<p>Klon-ID: %1|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2220|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> erstellt SYSVOL-Objekte für den geklonten schreibgeschützten Domänencontroller.<p>Zusätzliche Daten:<p>Klon-ID: %1|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2221|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Fehler|
|`Message`|<COMPUTERNAME> konnte kein zufälliges Kennwort für den geklonten Domänencontroller erstellen.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Name des geklonten Domänencontrollers: %2<p>Fehler: %3 %4|
|Hinweise und Lösung|Untersuchen Sie das Systemereignisprotokoll auf weitere Details für die Gründe, aus denen das Computerkontokennwort nicht erstellt werden konnte.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2222|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Fehler|
|`Message`|<COMPUTERNAME> konnte kein Kennwort für den geklonten Domänencontroller festlegen.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Name des geklonten Domänencontrollers: %2<p>Fehler: %3 %4|
|Hinweise und Lösung|Untersuchen Sie das Systemereignisprotokoll auf weitere Details für die Gründe, aus denen das Computerkontokennwort nicht festgelegt werden konnte.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2223|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|<COMPUTERNAME> hat das Computerkontokennwort für den geklonten Domänencontroller festgelegt.<p>Zusätzliche Daten:<p>Klon-ID: %1<p>Name des geklonten Domänencontrollers: %2<p>Gesamtanzahl der Wiederholungen: %3|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2224|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Fehler|
|`Message`|Fehler beim Klonen des virtuellen Domänencontrollers. Die folgenden %1 verwalteten Dienstkonten sind auf dem geklonten Computer vorhanden:<p>%2<p>Damit der Klonvorgang erfolgreich ist, müssen alle verwalteten Dienstkonten entfernt werden. Dazu können Sie das PowerShell-Cmdlet "Remove-ADComputerServiceAccount" verwenden.|
|Hinweise und Lösung|Erwartet bei der Verwendung von eigenständigen MSAs (nicht bei Gruppen-MSA). Befolgen Sie *nicht* den Ereignisratschlag, das Konto zu entfernen – dieser ist falsch. Verwenden Sie Uninstall-AdServiceAccount- [https://technet.microsoft.com/library/hh852310](/previous-versions/windows/it-pro/windows-powershell-1.0/ee176927(v=technet.10)) .<p>Eigenständige MSAs, die zuerst in Windows Server 2008 R2 eingeführt wurden, wurden in Windows Server 2012 durch Gruppen-MSAs (gMSA) ersetzt. GMSAs bieten Unterstützung für das Klonen.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2225|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Informational|
|`Message`|Die zwischengespeicherten geheimen Schlüssel des folgenden Sicherheitsprinzipals wurden erfolgreich vom lokalen Domänencontroller entfernt:<p>%1<p>Nach dem Klonen eines schreibgeschützten Domänencontrollers werden geheime Schlüssel, die zuvor auf dem als Klonquelle verwendeten schreibgeschützten Domänencontroller zwischengespeichert wurden, vom geklonten Domänencontroller entfernt.|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2226|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Fehler|
|`Message`|Fehler beim Entfernen von zwischengespeicherten geheimen Schlüsseln des folgenden Sicherheitsprinzipals vom lokalen Domänencontroller:<p>%1<p>Fehler: %2 (%3)<p>Nach dem Klonen eines schreibgeschützten Domänencontrollers müssen geheime Schlüssel, die zuvor auf dem als Klonquelle verwendeten schreibgeschützten Domänencontroller zwischengespeichert wurden, aus dem Klon entfernt werden. Geschieht dies nicht, wird das Risiko eines Angriffs zum Abrufen dieser Anmeldeinformationen von einem gestohlenen oder kompromittierten Klon erhöht. Falls der Sicherheitsprinzipal ein Konto mit umfangreichen Rechten ist und dagegen geschützt werden sollte, verwenden Sie den rootDSE-Vorgang "rODCPurgeAccount", um die geheimen Schlüssel manuell auf dem lokalen Domänencontroller zu löschen.|
|Hinweise und Lösung|Untersuchen Sie die System- und Verzeichnisdienst-Ereignisprotokolle auf weitere Informationen.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2227|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Fehler|
|`Message`|Bei dem Versuch, zwischengespeicherte geheime Schlüssel vom lokalen Domänencontroller zu entfernen, wurde eine Ausnahme ausgegeben.<p>Zusätzliche Daten:<p>Ausnahmewert: %1<p>Fehlerwert: %2<p>DSID: %3<p>Nach dem Klonen eines schreibgeschützten Domänencontrollers müssen geheime Schlüssel, die zuvor auf dem als Klonquelle verwendeten schreibgeschützten Domänencontroller zwischengespeichert wurden, aus dem Klon entfernt werden. Geschieht dies nicht, wird das Risiko eines Angriffs zum Abrufen dieser Anmeldeinformationen von einem gestohlenen oder kompromittierten Klon erhöht. Falls einer dieser Sicherheitsprinzipale ein Konto mit umfangreichen Rechten ist und dagegen geschützt werden sollte, verwenden Sie den rootDSE-Vorgang "rODCPurgeAccount", um die geheimen Schlüssel manuell auf dem lokalen Domänencontroller zu löschen.|
|Hinweise und Lösung|Untersuchen Sie die System- und Verzeichnisdienst-Ereignisprotokolle auf weitere Informationen.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|2228|
|`Source`|Microsoft-Windows-ActiveDirectory_DomainService|
|severity|Fehler|
|`Message`|Die Generierungs-ID des virtuellen Computers in der Active Directory-Datenbank dieses Domänencontrollers unterscheidet sich vom aktuellen Wert dieses virtuellen Computers. Eine Konfigurationsdatei zum Klonen des virtuellen Domänencontrollers (DCCloneConfig.xml) konnte jedoch nicht gefunden werden, sodass der Domänencontroller nicht geklont wurde. Falls ein Klonvorgang für den Domänencontroller ausgeführt werden sollte, stellen Sie sicher, dass eine DCCloneConfig.xml-Datei in einem der unterstützten Speicherorte bereitgestellt ist. Darüber hinaus steht die IP-Adresse dieses Domänencontrollers in Konflikt mit der IP-Adresse eines anderen Domänencontrollers. Um sicherzustellen, dass der Betrieb nicht unterbrochen wird, wurde der Domänencontroller so konfiguriert, dass er im DSRM gestartet wird.<p>Zusätzliche Daten:<p>Doppelte IP-Adresse: %1|
|Hinweise und Lösung|Dieser Schutzmechanismus beendet doppelte Domänencontroller, wenn möglich (bei der Verwendung von DHCP beispielsweise nicht). Fügen Sie eine gültige DcCloneConfig.xml-Datei hinzu, entfernen Sie das DSRM-Kennzeichen, und führen Sie das Klonen dann erneut durch.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29218|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Fehler beim Klonen des virtuellen Domänencontrollers. Der Klonvorgang konnte nicht abgeschlossen werden, und der geklonte Domänencontroller wurde im DSRM (Directory Services Restore Mode) neu gestartet.<p>Weitere Informationen zu Fehlern, die mit dem Klonversuch für den virtuellen Domänencontroller im Zusammenhang stehen, finden Sie in den zuvor protokollierten Ereignissen und in "%systemroot%\debug\dcpromo.log". Hier finden Sie auch Angaben dazu, ob dieses Klonimage wiederverwendet werden kann.<p>Falls mindestens ein Protokolleintrag darauf hinweist, dass der Klonvorgang nicht wiederholt werden kann, muss das Image auf sichere Art zerstört werden. Andernfalls können Sie die Fehler beheben, das DSRM-Startkennzeichen löschen und normal neu starten. Beim Neustart wird der Klonvorgang wiederholt.|
|Hinweise und Lösung|Überprüfen Sie die System- und Verzeichnisdienstprotokolle sowie die Datei dcpromo.log auf weitere Details für das Fehlschlagen des Klonens.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29219|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Informational|
|`Message`|Der virtuelle Domänencontroller wurde erfolgreich geklont.|
|Hinweise und Lösung|Dies ist ein Erfolgsereignis und nur ein Problem, wenn es unerwartet ist.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29248|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Für das Klonen des Domänencontrollers konnte die Winlogon-Benachrichtigung nicht abgerufen werden. Folgender Fehlercode wurde zurückgegeben: %1 (%2).<p>Weitere Informationen zu diesem Fehler finden Sie in "%systemroot%\debug\dcpromo.log". Suchen Sie dort nach Fehlern, die mit dem Klonversuch für den virtuellen Domänencontroller zu tun haben.|
|Hinweise und Lösung|Wenden Sie sich an den Microsoft-Produktsupport.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29249|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Fehler bei dem Versuch, beim Klonen des virtuellen Domänencontrollers die Konfigurationsdatei für den virtuellen Domänencontroller zu analysieren.<p>Folgender HRESULT-Code wurde zurückgegeben: %1.<p>Die Konfigurationsdatei ist:%2<p>Beheben Sie die Fehler in der Konfigurationsdatei, und wiederholen Sie den Klonvorgang.<p>Weitere Informationen zu diesem Fehler finden Sie in "%systemroot%\debug\dcpromo.log".|
|Hinweise und Lösung|Untersuchen Sie die Datei dclconeconfig.xml mithilfe eines XML-Editors auf Syntaxfehler und die DCCloneConfigSchema.xsd-Schemadatei.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29250|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Fehler beim Klonen des virtuellen Domänencontrollers. Auf dem geklonten virtuellen Domänencontroller sind momentan Anwendungen oder Dienste aktiviert, die in der für das Klonen von virtuellen Domänencontrollern verwendeten Liste zulässiger Anwendungen nicht aufgeführt sind.<p>Die folgenden Einträge fehlen:<p>%2<p>%1 (falls vorhanden) wurde als definierte Aufnahmeliste verwendet.<p>Der Klonvorgang kann nicht abgeschlossen werden, wenn nicht klonbare Anwendungen installiert sind.<p>Führen Sie das Active Directory-Powershell-Cmdlet "Get-ADDCCloningExcludedApplicationList" aus, um zu prüfen, welche Anwendungen auf dem geklonten Computer installiert, aber nicht in der Liste zugelassener Anwendungen enthalten sind. Fügen Sie diese Anwendungen der Liste zulässiger Anwendungen hinzu, wenn sie mit dem Klonen eines virtuellen Domänencontrollers kompatibel sind. Deinstallieren Sie alle Anwendungen, die nicht mit dem Klonen des virtuellen Domänencontrollers kompatibel sind, und wiederholen Sie dann den Klonvorgang.<p>Der Klonvorgang für virtuelle Domänencontroller sucht nach der Datei mit der Liste zugelassener Anwendungen, "CustomDCCloneAllowList.xml", basierend auf der folgenden Reihenfolge: Die zuerst gefundene Datei wird verwendet, und alle anderen werden ignoriert:<p>1. Name des Registrierungs Werts: HKEY_LOCAL_MACHINE \system\currentcontrolset\services\ntds\parameters\allowlistfolder<p>2. das Verzeichnis, in dem sich der DSA-Arbeitsverzeichnis Ordner befindet<p>3. %windir%\NTDS<p>4. Wechselmedien mit Lese-/Schreibzugriff in der Reihenfolge der Laufwerk Buchstaben im Stammverzeichnis des Laufwerks|
|Hinweise und Lösung|Befolgen Sie die Anweisungen in der Meldung.|

| Events | BESCHREIBUNG |
|--|--|
| Ereignis-ID | 29251 |
| `Source` | Microsoft-Windows-DirectoryServices-DSROLE-Server |
| severity | Fehler |
| `Message` | Die IP-Adresse des geklonten Rechners konnte nicht zurückgesetzt werden.<p>Folgender Fehlercode wurde zurückgegeben: %1 (%2).<p>Ursache dieses Fehlers könnte eine fehlerhafte Konfiguration in den Netzwerkkonfigurationsabschnitten in der Konfigurationsdatei des virtuellen Domänencontrollers sein.<p>Weitere Informationen zu Fehlern, die beim Klonen eines virtuellen Domänencontrollers hinsichtlich des Zurücksetzens von IP-Adressen auftreten können, finden Sie in "%systemroot%\debug\dcpromo.log".<p>Details zum Zurücksetzen von Computer-IP-Adressen auf dem geklonten Computer finden Sie unter https://go.microsoft.com/fwlink/?LinkId=208030 |
| Hinweise und Lösung | Vergewissern Sie sich, dass die in der dccloneconfig.xml- Datei festgelegten IP-Informationen gültig sind und den ursprünglichen Quellcomputer nicht duplizieren. |

| Events | BESCHREIBUNG |
|--|--|
| Ereignis-ID | 29253 |
| `Source` | Microsoft-Windows-DirectoryServices-DSROLE-Server |
| severity | Fehler |
| `Message` | Fehler beim Klonen des virtuellen Domänencontrollers. Der Klondomänencontroller konnte den als primären Domänencontroller (PDC) fungierenden Betriebsmaster nicht in der Ursprungsdomäne des geklonten Computers finden.<p>Folgender Fehlercode wurde zurückgegeben: %1 (%2).<p>Vergewissern Sie sich, dass der primäre Domänencontroller in der Ursprungsdomäne des geklonten Computers einem aktiven Domänencontroller zugeordnet, online und betriebsbereit ist. Vergewissern Sie sich, dass der geklonte Computer über die erforderlichen Ports und Protokolle eine LDAP/RPC-Verbindung mit dem primären Domänencontroller hat. |
| Hinweise und Lösung | Vergewissern Sie sich, dass die IP-Adresse des geklonten Domänencontrollers und die DNS-Informationen festgelegt sind. Verwenden Sie Dcdiag.exe/Test: loercheck, um zu überprüfen, ob der PDCE Online ist. verwenden Sie Nltest.exe/Server: *<PDCE>* /DCLIST: *<domain>* zum gültigen RPC, rufen Sie eine Netzwerk Erfassung vom PDCE ab, während das Klonen fehlschlägt, und analysieren Sie den Datenverkehr. |

| Events | BESCHREIBUNG |
|--|--|
| Ereignis-ID | 29254 |
| `Source` | Microsoft-Windows-DirectoryServices-DSROLE-Server |
| severity | Fehler |
| `Message` | Es konnte keine Bindung mit dem primären Domänencontroller %1 hergestellt werden.<p>Folgender Fehlercode wurde zurückgegeben: %2 (%3).<p>Vergewissern Sie sich, dass der primäre Domänencontroller %1 online und betriebsbereit ist. Vergewissern Sie sich, dass der geklonte Computer über die erforderlichen Ports und Protokolle eine LDAP/RPC-Verbindung mit dem primären Domänencontroller hat. |
| Hinweise und Lösung | Vergewissern Sie sich, dass die IP-Adresse des geklonten Domänencontrollers und die DNS-Informationen festgelegt sind. Verwenden Sie Dcdiag.exe/Test: loercheck, um zu überprüfen, ob der PDCE Online ist. verwenden Sie Nltest.exe/Server: *<PDCE>* /DCLIST: *<domain>* zum gültigen RPC, rufen Sie eine Netzwerk Erfassung vom PDCE ab, während das Klonen fehlschlägt, und analysieren Sie den Datenverkehr. |

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29255|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Fehler beim Klonen des virtuellen Domänencontrollers.<p>Bei dem Versuch, Objekte auf dem primären Domänencontroller %1 zu erstellen, die für das zu klonende Image erforderlich sind, wurde der Fehler %2 (%3) zurückgegeben.<p>Überprüfen Sie, ob der geklonte Domänencontroller über die Berechtigung verfügt, sich selbst zu klonen. Suchen Sie auf dem primären Domänencontroller %1 im Ereignisprotokoll für den Verzeichnisdienst nach zugehörigen Ereignissen.|
|Hinweise und Lösung|Suchen Sie unter MS TechNet, in der MS Knowledge Base und in MS-Blogs nach dem bestimmten Fehler, um seine typische Bedeutung zu bestimmen und ihn dann auf der Basis dieser Ergebnisse zu behandeln.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29256|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Fehler beim Festlegen des Kennzeichens für Starten im Verzeichnisdienste-Wiederherstellungsmodus. Fehlercode: %1.<p>Weitere Informationen zu Fehlern finden Sie in der Datei %systemroot%\debug\dcpromo.log.|
|Hinweise und Lösung|Untersuchen Sie das Verzeichnisdienstprotokoll und dcpromo.log auf Details. Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise die Verwendung von Berechtigungen blockieren.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29257|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Der virtuelle Domänencontroller wurde geklont. Der Computer konnte wegen eines Fehlers nicht neu gestartet werden: Fehlercode: %1.<p>Starten Sie den Computer neu, damit der Klonvorgang abgeschlossen wird.|
|Hinweise und Lösung|Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise die Verwendung von Berechtigungen blockieren.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29264|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Fehler beim Löschen des Kennzeichens für Starten im Verzeichnisdienste-Wiederherstellungsmodus. Fehlercode: %1.<p>Weitere Informationen zu Fehlern finden Sie in der Datei %systemroot%\debug\dcpromo.log.|
|Hinweise und Lösung|Untersuchen Sie das Verzeichnisdienstprotokoll und dcpromo.log auf Details. Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise die Verwendung von Berechtigungen blockieren.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29265|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Informational|
|`Message`|Der virtuelle Domänencontroller wurde erfolgreich geklont. Die für das Klonen des virtuellen Domänencontrollers verwendete Konfigurationsdatei "%1" wurde in "%2" umbenannt.|
|Hinweise und Lösung|Nicht zutreffend, die ist ein Erfolgsereignis.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29266|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Der virtuelle Domänencontroller wurde erfolgreich geklont. Die für das Klonen des virtuellen Domänencontrollers verwendete Konfigurationsdatei "%1" konnte wegen eines Fehlers aber nicht umbenannt werden. Fehlercode: %2 (%3).|
|Hinweise und Lösung|Benennen Sie die Datei dccloneconfig.xml manuell um.|

| Events | BESCHREIBUNG |
| -- |--|
|Ereignis-ID|29267|
|`Source`|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|severity|Fehler|
|`Message`|Beim Klonen des virtuellen Domänencontrollers wurde die Liste der für das Klonen virtueller Domänencontroller zugelassenen Anwendungen nicht überprüft.<p>Folgender Fehlercode wurde zurückgegeben: %1 (%2).<p>Dieser Fehler wird möglicherweise durch einen Syntaxfehler in der Datei mit der Klonzulassungsliste verursacht. (Zurzeit wird diese Datei überprüft: %3) Weitere Informationen zu diesem Fehler finden Sie in "%systemroot%\debug\dcpromo.log".|
|Hinweise und Lösung|Befolgen Sie die Anweisungen im Ereignis.|

##### <a name="error-messages"></a>Fehlermeldungen
Es gibt keine direkten interaktiven Fehler für ein fehlgeschlagenes Klonen von virtuellen Domänencontrollern. Alle Kloninformationen werden in den System- und Verzeichnisdienstprotokollen sowie den Protokollen zum Heraufstufen von Domänencontrollern in dcpromo.log protokolliert. Wenn der Server jedoch im DS-Wiederherstellungsmodus gestartet wird, sollten Sie sofort untersuchen, wenn das Heraufstufen oder Klonen fehlgeschlagen ist.

Die Datei dcpromo.log ist der erste Ort, den Sie bei Klonfehlern überprüfen sollten. Je nach aufgelistetem Fehler ist es möglicherweise erforderlich, für eine weitere Diagnose die Verzeichnisdienst- und Systemprotokolle zu überprüfen.

#### <a name="known-issues-and-support-scenarios"></a>Bekannte Probleme und Supportszenarien
Im Folgenden sind bekannte Probleme aufgeführt, die während des Windows Server 2012-Bereitstellungsprozesses auftreten können. All diese Probleme sind entwurfsbedingt und verfügen entweder über eine gültige Problemumgehung oder eine geeignetere Technik, um sie von vornherein zu vermeiden. Einige werden möglicherweise in späteren Versionen von Windows Server 2012 behoben.

|**Problem**|**Fehler beim Klonen, DSRM**|
| -- |--|
|**Symptome**|Klonen wird im Verzeichnisdienste-Wiederherstellungsmodus gestartet|
|**Lösung und Hinweise**|Überprüfen Sie alle Schritte, die Sie aus den Abschnitten „Bereitstellen eines virtualisierten Domänencontrollers“ und [Allgemeine Methode für die Problembehandlung beim Klonen von Domänencontrollern](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology) befolgt haben.<p>Beschrieben in KB 2742844.|

|**Problem**|**Zusätzliche IP-Leases bei der Verwendung von DHCP zum Klonen**|
| -- |--|
|**Symptome**|Nach dem erfolgreichen Klonen eines Domänencontrollers und der Verwendung von DHCP übernimmt der erste Start des Klons eine DHCP-Lease. Wenn der Server dann umbenannt und als Domänencontroller neu gestartet ist, übernimmt er eine zweite DHCP-Lease. Die erste IP-Adresse ist nicht freigegeben, und Sie enden mit einer „Phantom“-Lease.|
|**Lösung und Hinweise**|Löschen Sie die ungenutzte Adresslease in DHCP, oder lassen Sie sie normal ablaufen. Beschrieben in KB 2742836.|

|**Problem**|**Klonen schlägt nach sehr langer Verzögerung im DSRM fehl**|
| -- |--|
|**Symptome**|Das Klonen scheint bei „Das Domänencontrollerklonen ist zu X% abgeschlossen“ für 8 bis 15 Minuten zu stoppen. Danach schlägt das Klonen fehl und wird im DSRM gestartet.|
|**Lösung und Hinweise**|Der geklonte Computer kann keine dynamische IP-Adresse von DHCP oder SLAAC abrufen, verwendet eine doppelte IP-Adresse oder kann den PDC nicht finden. Mehrere vom Klonvorgang durchgeführte erneute Versuche führen zu der Verzögerung. Beheben Sie das Netzwerkproblem, um das Klonen zuzulassen.<p>Beschrieben in KB 2742844.|

|**Problem**|**Beim Klonen werden nicht alle Dienstprinzipalnamen erneut erstellt.**|
| -- |--|
|**Symptome**|Wenn ein Satz von *dreiteiligen* Dienstprinzipalnamen sowohl einen NetBIOS-Namen mit einem Port als auch einen ansonsten identischen NetBIOS-Namen ohne einen Port enthalten, wird der Eintrag ohne Port nicht mit dem neuen Computernamen erneut erstellt. Beispiel:<p>customspn/DC1:200/app1 INVALID USE OF SYMBOLS *wird mit dem neuen Computernamen neu erstellt*<p>customspn/DC1/app1 INVALID USE OF SYMBOLS *wird nicht mit dem neuen Computernahmen neu erstellt*<p>Vollqualifizierte Namen und SPNs ohne drei Teile werden neu erstellt, unabhängig von Ports. Die folgenden werden beispielsweise erfolgreich auf dem Klon neu erstellt:<p>customspn/DC1:202 INVALID USE OF SYMBOLS *wird neu erstellt*<p>customspn/DC1 INVALID USE OF SYMBOLS *wird neu erstellt*<p>customspn/DC1.corp.contoso.com:202 INVALID USE OF SYMBOLS *wird neu erstellt*<p>customspn/DC1.corp.contoso.com INVALID USE OF SYMBOLS *wird neu erstellt*|
|**Lösung und Hinweise**|Dies ist eine Einschränkung des Benennungsvorgangs für Domänencontroller in Windows, nicht nur beim Klonen. Dreiteilige SPNS werden in keinem Szenario von der Benennungslogik verarbeitet. Die meisten in Windows enthaltenen Dienste sind davon nicht betroffen, da sie alle fehlenden SPNs nach Bedarf neu erstellen. Für andere Anwendungen ist möglicherweise eine manuelle Eingabe des SPN erforderlich, um das Problem zu beheben.<p>Beschrieben in KB 2742874.|

|**Problem**|**Fehler beim Klonen, Start im DSRM, allgemeine Netzwerkfehler**|
| -- |--|
|**Symptome**|Das Klonen wird im Verzeichnisdienst-Wiederherstellungsmodus gestartet Es sind allgemeine Netzwerkfehler vorhanden.|
|Lösung und Hinweise|Stellen Stellen Sie sicher, dass dem neuen Klon keine doppelte statische MAC-Adresse vom Quelldomänencontroller zugewiesen wurde. Sie können sehen, ob ein virtueller Computer statische MAC-Adressen verwendet, indem Sie den folgenden Befehl auf dem Hypervisorhost für den Quell- und den geklonten virtuellen Computer ausführen:<p>Get-VM-VMName *Test-VM* &#124; Get-VMNetworkAdapter &#124; FL *<p>Ändern Sie die MAC-Adresse in eine eindeutige statische Adresse, oder stellen Sie auf die Verwendung von dynamischen MAC-Adressen um.<p>Beschrieben in KB 2742844.|

| **Problem** | **Fehler beim Klonen, Start im DSRM als Duplikat des Quelldomänencontrollers** |
| -- |--|
| **Symptome** | Ein neuer Klon wird ohne Klonen gestartet. Die Datei dccloneconfig.xml wird nicht umbenannt, und der Server startet im Domänendienst-Wiederstellungsmodus. Das Verzeichnisdienst-Ereignisprotokoll zeigt Fehler 2164 an.<p>*<COMPUTERNAME>* Fehler beim Starten des dsrolesvc-Dienes zum Klonen des lokalen virtuellen Domänen Controllers. |
| **Lösung und Hinweise** | Untersuchen Sie die Diensteinstellungen für den DS-Rollenserverdienst (DsRoleSvc), und stellen Sie sicher, dass der Starttyp auf Manuell festgelegt ist. Vergewissern Sie sich, dass kein Drittanbieterprogramm das Starten dieses Diensts verhindert.<p>Weitere Informationen dazu, wie Sie diesen sekundären Domänencontroller freigeben und gleichzeitig sicherstellen, dass Updates ausgehend repliziert werden, finden Sie im Microsoft KB-Artikel 2742970. |

|**Problem**|**Fehler beim Klonen, Start im DSRM, Fehler 8610**|
| -- |--|
|**Symptome**|Der Klon wird im Verzeichnisdienst-Wiederherstellungsmodus gestartet. In der Datei Dcpromo .log wird Fehler 8610 angezeigt (das heißt ERROR_DS_ROLE_NOT_VERIFIED 8610 oder 0x21A2)|
|**Lösung und Hinweise**|Dies passiert, wenn der PDC sichtbar sein kann, aber keine ausreichende Replikation durchgeführt hat, um die Übernahme der Rolle selbst zuzulassen. Beispielsweise, wenn das Klonen gestartet wird und ein anderer Administrator die Rolle PDCE FSMO zu einem neuen Domänencontroller verschiebt.<p>Beschrieben in KB 2742916.|

|**Problem**|**Fehler beim Klonen, Start im DSRM, allgemeine Netzwerkfehler**|
| -- |--|
|**Symptome**|Der Klon wird im Verzeichnisdienst-Wiederherstellungsmodus gestartet. Es sind allgemeine Netzwerkfehler vorhanden.|
|**Lösung und Hinweise**|Stellen Stellen Sie sicher, dass dem neuen Klon keine doppelte statische MAC-Adresse vom Quelldomänencontroller zugewiesen wurde. Sie können sehen, ob ein virtueller Computer statische MAC-Adressen verwendet, indem Sie den folgenden Befehl auf dem Hyper-V-Host für den Quell- und den geklonten virtuellen Computer ausführen:<p>Get-VM-VMName *Test-VM* &#124; Get-VMNetworkAdapter &#124; FL *<p>Ändern Sie die MAC-Adresse in eine eindeutige statische Adresse, oder stellen Sie auf die Verwendung von dynamischen MAC-Adressen um.<p>Beschrieben in KB 2742844.|

|**Problem**|**Fehler beim Klonen, Start im DSRM**|
| -- |--|
|**Symptome**|Der Klon wird im Verzeichnisdienst-Wiederherstellungsmodus gestartet.|
|**Lösung und Hinweise**|Stellen Sie sicher, dass die Datei dccloneconfig.xml die Schemadefinition enthält (siehe sampledccloneconfig.xml, Zeile 2):<p>**<d3c: dccloneconfig xmlns:d3c = "URI:Microsoft. com: Schemas: dccloneconfig" >**<p>Beschrieben in KB 2742844.|

|**Problem**|**Keine Anmeldeserver verfügbar, Fehlerprotokollierung im DSRM**|
| -- |--|
|**Symptome**|Das Klonen wird im Verzeichnisdienst-Wiederherstellungsmodus gestartet Sie versuchen sich anzumelden und erhalten den Fehler:<p>**Derzeit sind keine Anmeldeserver vorhanden, um die Anmeldeanforderung zu bearbeiten.**|
|**Lösung und Hinweise**|Stellen Stellen Sie sicher, dass Sie sich mit dem DSRM-Administratorkonto und nicht dem Domänenkonto anmelden. Verwenden Sie den linken Pfeil, und geben den folgenden Benutzer ein:<p>**.\administrator**<p>Beschrieben in KB 2742908.|

|**Problem**|**Klonquelle schlägt fehl und wird im DSRM gestartet, Fehler**|
|--|--|
|**Symptome**|Während des Klonens wird Fehler 8437 „Fehler beim Erstellen von Domänencontrollerobjekten auf PDC (0x20f5)“ angezeigt|
|**Lösung und Hinweise**|Ein doppelter Computername wurde in DCCloneConfig.xml als Quelldomänencontroller oder ein vorhandener Domänencontroller festgelegt. Der Computername muss außerdem im NetBIOS-Computernamenformat sein (höchstens 15 Zeichen, kein FQDN).<p>Korrigieren Sie die Datei dccloneconfig.xml, indem Sie einen eindeutigen, gültigen Namen festlegen.<p>Beschrieben in KB 2742959.|

|**Problem**|**New-addccloneconfigfile-Fehler „Index lag außerhalb des zulässigen Bereichs“**|
|--|--|
|**Symptome**|Bei der Ausführung des Cmdlets new-addccloneconfigfile erhalten Sie die Fehlermeldung:<p>Index lag außerhalb des zulässigen Bereichs. Er darf nicht negativ und kleiner als die Sammlung sein.|
|**Lösung und Hinweise**|Sie müssen das Cmdlet in einer Windows PowerShell-Konsole mit Administratorrechten ausführen. Der Fehler wird durch eine fehlende Mitgliedschaft in der Gruppe der lokalen Administratoren auf dem Computer verursacht.<p>Beschrieben in KB 2742927.|

|**Problem**|**Fehler beim Klonen, doppelter Domänencontroller**|
|--|--|
|**Symptome**|Klon wird ohne Klonen gestartet und dupliziert vorhandenen Domänencontroller|
|**Lösung und Hinweise**|Der Computer wurde kopiert und gestartet, enthält aber keine DcCloneConfig.xml-Datei an einem der unterstützten Speicherorte und hat keine doppelte IP-Adresse mit dem Quelldomänencontroller. Der Domänencontroller muss ordnungsgemäß entfernt werden, um einen Datenverlust zu vermeiden.<p>Beschrieben in KB 2742970.|

|**Problem**|**New-ADDCCloneConfigFile schlägt mit dem Fehler „Der Server ist nicht funktionstüchtig“ fehl, wenn geprüft wird, ob der Quelldomänencontroller ein Mitglied der Gruppe „Klonbare Domänencontroller“ ist, wenn kein globaler Katalogserver verfügbar ist.**|
|--|--|
|**Symptome**|Beim Ausführen von New-ADDCCloneConfigFile zum Erstellen einer dccloneconfig.xml-Datei erhalten Sie die folgende Fehlermeldung:<p>Code: der Server ist nicht betriebsbereit.|
|**Lösung und Hinweise**|Überprüfen Sie die Konnektivität mit einem globalen Katalogserver von dem Server, auf dem Sie New-ADDCCloneConfigFile ausführen, und vergewissern Sie sich, dass die Mitgliedschaft des Quelldomänencontrollers in der Gruppe „Klonbare Domänencontroller“ an diesen globalen Katalogserver repliziert wurde.<p>Führen Sie den folgenden Befehl als Mittel aus, um den Locatorcache des Domänencontrollers in Fällen zu leeren, in denen ein globaler Katalogserver oder ein Domänencontroller kürzlich offline geschaltet wurde:<p>Code-nltest/dsgetdc:/GC/Force|

### <a name="advanced-troubleshooting"></a>Erweiterte Fehlerbehebung
In diesem Modul erfahren Sie mehr über die erweiterte Fehlerbehandlung, indem *funktionierende* Protokolle als Beispiele verwendet und Erläuterungen zu den Ereignissen bereitgestellt werden. Wenn Sie verstehen, wie ein erfolgreicher Vorgang auf einem virtualisierten Quelldomänencontroller aussieht, werden Fehler in Ihrer Umgebung offensichtlich. Diese Protokolle werden nach ihrer Quelle dargestellt, aufsteigend in der Reihenfolge *erwarteter* Ereignisse (auch wenn es sich um Warnungen und Fehler handelt) in jedem Protokoll, die mit einem geklonten Domänencontroller zusammenhängen.

#### <a name="cloning-a-domain-controller"></a>Klonen eines Domänencontrollers
In diesem Beispiel verwendet der geklonte Domänencontroller DHCP, um eine IP-Adresse abzurufen, repliziert SYSVOL über FRS oder DFSR (sehen Sie sich bei Bedarf das entsprechende Protokoll an), ist ein globaler Katalogserver und verwendet eine leere dccloneconfig.xml-Datei.

##### <a name="directory-services-event-log"></a>Verzeichnisdienst-Ereignisprotokoll
Das Verzeichnisdienst-Ereignisprotokoll enthält den größten Teil der ereignisbasierten Betriebsinformationen beim Klonen. Der Hypervisor ändert die VM-Generations-ID. Der NTDS-Dienst bemerkt dies, macht den RID-Pool ungültig und ändert die Aufrufkennung. Die neue VM-Generations-ID wird festgelegt, und der Server repliziert Active Directory-Daten eingehend. Der DFSR-Dienst wird angehalten, und seine Datenbank, die SYSVOL hostet, wird gelöscht, wodurch eine nicht autoritative eingehende Synchronisierung erzwungen wird. Der hohe USN-Grenzwert wird angepasst.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **2160** | ActiveDirectory_DomainService | Von der lokalen Instanz der Active Directory-Domänendienste wurde eine Klonkonfigurationsdatei für virtuelle Domänencontroller gefunden.<p>Fundstelle der Klonkonfigurationsdatei:<p>*<path>* \DCCloneConfig.xml<p>Das Vorliegen einer Klonkonfigurationsdatei lässt darauf schließen, dass der lokale virtuelle Domänencontroller ein Klon eines anderen virtuellen Domänencontrollers ist. Es wird begonnen, die Active Directory -Domänendienste zu klonen. |
| **2191** | ActiveDirectory_DomainService | Fehler beim Festlegen des unten stehenden Registrierungswerts zum Deaktivieren von DNS-Aktualisierungen durch die Active Directory-Domänendienste.<p>Registrierungsschlüssel:<p>SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<p>Registrierungswert:<p>UseDynamicDns<p>Registrierungswertdaten:<p>0<p>Während des Klonens sind die Computernamen des lokalen Computers und des Quellcomputers möglicherweise kurzzeitig identisch. Für diese Zeitspanne wird die Registrierung von DNS A- und AAAA-Datensätzen deaktiviert, sodass Clients keine Anforderungen an den lokalen Computer senden können. Nach dem Klonen werden DNS-Aktualisierungen vom Klonprozess wieder aktiviert. |
| **2191** | ActiveDirectory_DomainService | Fehler beim Festlegen des unten stehenden Registrierungswerts zum Deaktivieren von DNS-Aktualisierungen durch die Active Directory-Domänendienste.<p>Registrierungsschlüssel:<p>SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<p>Registrierungswert:<p>RegistrationEnabled<p>Registrierungswertdaten:<p>0<p>Während des Klonens sind die Computernamen des lokalen Computers und des Quellcomputers möglicherweise kurzzeitig identisch. Für diese Zeitspanne wird die Registrierung von DNS A- und AAAA-Datensätzen deaktiviert, sodass Clients keine Anforderungen an den lokalen Computer senden können. Nach dem Klonen werden DNS-Aktualisierungen vom Klonprozess wieder aktiviert.<p>"Information 2/7/2012 3:12:49 pm Microsoft-Windows-ActiveDirectory_DomainService 2191 Internal Configuration" Active Directory Domain Services legen Sie den folgenden Registrierungs Wert fest, um DNS-Updates zu deaktivieren.<p>Registrierungsschlüssel:<p>SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<p>Registrierungswert:<p>DisableDynamicUpdate<p>Registrierungswertdaten:<p>1<p>Während des Klonens sind die Computernamen des lokalen Computers und des Quellcomputers möglicherweise kurzzeitig identisch. Für diese Zeitspanne wird die Registrierung von DNS A- und AAAA-Datensätzen deaktiviert, sodass Clients keine Anforderungen an den lokalen Computer senden können. Nach dem Klonen werden DNS-Aktualisierungen vom Klonprozess wieder aktiviert. |
| **2172** | ActiveDirectory_DomainService | Das msDS-GenerationId-Attribut des Domänencontrollers wurde gelesen.<p>Wert des msDS-GenerationId-Attributs:<p>*<Number>* |
| **2170** | ActiveDirectory_DomainService | Es wurde keine Änderung der Generations-ID erkannt.<p>Zwischengespeicherte Generations-ID in DS (alter Wert):<p>*<Number>*<p>Aktuelle Generations-ID des virtuellen Computers (neuer Wert):<p>*<Number>*<p>Die Änderung der Generations-ID erfolgt nach der Anwendung einer Momentaufnahme des virtuellen Computers, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. Von den Active Directory-Domänendiensten wird eine neue Aufruf-ID zur Wiederherstellung des Domänencontrollers erstellt. Virtualisierte Domänencontroller dürfen nicht mit einer Momentaufnahme des virtuellen Computers wiederhergestellt werden. Das unterstützte Verfahren zum Wiederherstellen oder Zurücksetzen des Inhalts einer Datenbank der Active Directory-Domänendienste ist das Wiederherstellen einer Systemstatussicherung, die mit einer für die Active Directory-Domänendienste geeigneten Sicherungsanwendung erstellt wurde. |
| **1109** | ActiveDirectory_DomainService | Das invocationID-Attribut für diesen Domänencontroller wurde geändert. Die höchste Aktualisierungssequenznummer zum Zeitpunkt der Erstellung der Sicherung lautet wie folgt:<p>InvocationID-Attribut (alter Wert):<p>*<GUID>*<p>InvocationID-Attribut (neuer Wert):<p>*<GUID>*<p>Aktualisierungssequenznummer:<p>*<Number>*<p>Die Aufruf-ID (invocationID-Attribut) wird in den folgenden Situationen geändert: nach Wiederherstellung des Verzeichnisservers von einem Sicherungsmedium, nach Konfiguration des Verzeichnisservers als Host einer beschreibbaren Anwendungsverzeichnispartition, bei fortgesetzter Ausführung des Verzeichnisservers, nachdem eine Momentaufnahme des virtuellen Computers angewendet wurde, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. Virtualisierte Domänencontroller dürfen nicht mit einer Momentaufnahme des virtuellen Computers wiederhergestellt werden. Das unterstützte Verfahren zum Wiederherstellen oder Zurücksetzen des Inhalts einer Datenbank der Active Directory-Domänendienste ist das Wiederherstellen einer Systemstatussicherung, die mit einer für die Active Directory-Domänendienste geeigneten Sicherungsanwendung erstellt wurde. |
| **1000** | ActiveDirectory_DomainService | Das Starten der Microsoft-Active Directory-Domänendienste wurde abgeschlossen. |
| **1394** | ActiveDirectory_DomainService | Alle Probleme, die Aktualisierungen der Active Directory-Domänendienste-Datenbank verhinderten, wurden behoben. Neue Aktualisierungen der Active Directory-Domänendienste-Datenbank sind erfolgreich. Der Netzwerkanmeldedienst wird neu gestartet. |
| **2163** | ActiveDirectory_DomainService | Der DsRoleSvc-Dienst zum Klonen des lokalen virtuellen Domänencontrollers wurde gestartet. |
| **326** | NTDS ISAM | NTDS (536) NTDSA: das Datenbankmodul hat eine Datenbank angefügt (1, c:\windows\ntds\ntds.dit). (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<p>Gespeicherter Cache: 1 |
| **103** | NTDS ISAM | NTDS (536) NTDSA: die Datenbank-Engine hat die Instanz (0) beendet.<p>Dirty Shutdown: 0<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.032, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000. |
| **102** | NTDS ISAM | NTDS (536) NTDSA: die Datenbank-Engine (6.02.8225.0000) startet eine neue Instanz (0). |
| **105** | NTDS ISAM | NTDS (536) NTDSA: das Datenbankmodul hat eine neue Instanz (0) gestartet. (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.016, [2] 0.000, [3] 0.015, [4] 0.078, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.046, [10] 0.000, [11] 0.000. |
| **1004** | ActiveDirectory_DomainService | Die Active Directory-Domänendienste wurden heruntergefahren. |
| **102** | NTDS ISAM | NTDS (536) NTDSA: die Datenbank-Engine (6.02.8225.0000) startet eine neue Instanz (0). |
| **326** | NTDS ISAM | NTDS (536) NTDSA: das Datenbankmodul hat eine Datenbank angefügt (1, c:\windows\ntds\ntds.dit). (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.015, [3] 0.016, [4] 0.000, [5] 0.031, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<p>Gespeicherter Cache: 1 |
| **105** | NTDS ISAM | NTDS (536) NTDSA: das Datenbankmodul hat eine neue Instanz (0) gestartet. (Zeit=1 Sekunde)<p>Interne Zeitsteuerungsabfolge: [1] 0.031, [2] 0.000, [3] 0.000, [4] 0.391, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000. |
| **1109** | ActiveDirectory_DomainService | Das invocationID-Attribut für diesen Domänencontroller wurde geändert. Die höchste Aktualisierungssequenznummer zum Zeitpunkt der Erstellung der Sicherung lautet wie folgt:<p>InvocationID-Attribut (alter Wert):<p>*<GUID>*<p>InvocationID-Attribut (neuer Wert):<p>*<GUID>*<p>Aktualisierungssequenznummer:<p>*<Number>*<p>Die Aufruf-ID (invocationID-Attribut) wird in den folgenden Situationen geändert: nach Wiederherstellung des Verzeichnisservers von einem Sicherungsmedium, nach Konfiguration des Verzeichnisservers als Host einer beschreibbaren Anwendungsverzeichnispartition, bei fortgesetzter Ausführung des Verzeichnisservers, nachdem eine Momentaufnahme des virtuellen Computers angewendet wurde, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. Virtualisierte Domänencontroller dürfen nicht mit einer Momentaufnahme des virtuellen Computers wiederhergestellt werden. Das unterstützte Verfahren zum Wiederherstellen oder Zurücksetzen des Inhalts einer Datenbank der Active Directory-Domänendienste ist das Wiederherstellen einer Systemstatussicherung, die mit einer für die Active Directory-Domänendienste geeigneten Sicherungsanwendung erstellt wurde. |
| **1168** | ActiveDirectory_DomainService | Interner Fehler: ein Active Directory Domain Services Fehler ist aufgetreten.<p>Zusätzliche Daten<p>Fehlerwert (dezimal):<p>2<p>Fehlerwert (hexadezimal):<p>2<p>Interne ID:<p>7011658 |
| **1110** | ActiveDirectory_DomainService | Das Heraufstufen dieses Servers zum globalen Katalog wird um das folgende Intervall verzögert.<p>Intervall (Minuten):<p>5<p>Diese Verzögerung ist notwendig, damit die erforderlichen Partitionen vorbereitet werden können, bevor der globale Katalog angekündigt wird. Sie können in der Registrierung die Anzahl an Sekunden angeben, die der Verzeichnissystem-Agent vor dem Heraufstufen des Domänencontrollers zum globalen Katalog warten soll, Weitere Informationen zum Registrierungswert Global Catalog Delay Advertisement finden Sie im Resource Kit Distributed Systems Guide. |
| **103** | NTDS ISAM | NTDS (536) NTDSA: die Datenbank-Engine hat die Instanz (0) beendet.<p>Dirty Shutdown: 0<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.047, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.016, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000. |
| **1004** | ActiveDirectory_DomainService | Die Active Directory-Domänendienste wurden heruntergefahren. |
| **1539** | ActiveDirectory_DomainService | Die Active Directory-Domänendienste konnten den softwarebasierten Datenträgerschreibungscache auf der folgenden Festplatte nicht deaktivieren.<p>Festplatte:<p>c:<p>Möglicherweise gehen Daten bei einem Systemfehler verloren. |
| **2179** | ActiveDirectory_DomainService | Für das msDS-GenerationId-Attribut des Computerobjekts des Domänencontrollers wurde der folgende Parameter festgelegt:<p>Generations-ID-Attribut:<p>*<Number>* |
| **2173** | ActiveDirectory_DomainService | Fehler beim Lesen des msDS-GenerationId-Attributs des Computerobjekts des Domänencontrollers. Mögliche Ursachen sind, dass eine Datenbanktransaktion nicht korrekt ausgeführt wurde oder dass die Generations-ID in der lokalen Datenbank nicht vorhanden ist. Zu bedenken ist auch, dass beim ersten Neustart nach einem DCPromo-Vorgang noch kein msDS-GenerationId-Attribut vorhanden ist.<p>Zusätzliche Daten<p>Fehlercode:<p>6 |
| **1000** | ActiveDirectory_DomainService | Das Starten der Microsoft-Active Directory-Domänendienste wurde abgeschlossen. Version 6.2.8225.0 |
| **1394** | ActiveDirectory_DomainService | Alle Probleme, die Aktualisierungen der Active Directory-Domänendienste-Datenbank verhinderten, wurden behoben. Neue Aktualisierungen der Active Directory-Domänendienste-Datenbank sind erfolgreich. Der Netzwerkanmeldedienst wird neu gestartet. |
| **1128** | ActiveDirectory_DomainService | 1128 Konsistenzprüfung "Eine Replikationsverbindung wurde vom folgenden Quellverzeichnisdienst zum lokalen Verzeichnisdienst erstellt.<p>Quellverzeichnisdienst:<p>CN = NTDS-Einstellungen, *<Domain Controller DN>*<p>Lokaler Verzeichnisdienst:<p>CN = NTDS-Einstellungen, *<Domain Controller DN>*<p>Zusätzliche Daten<p>Begründungscode:<p>0x2<p>Interne Kennung für Erstellungspunkt:<p>f0a025d |
| **1999** | ActiveDirectory_DomainService | Der Quellverzeichnisdienst hat die Aktualisierungssequenznummer optimiert, die vom Zielverzeichnisdienst dargestellt wird. Der Quell- und der Zielverzeichnisdienst haben einen gemeinsamen Replikationspartner. Der Zielverzeichnisdienst ist auf dem gleichen Stand wie der gemeinsame Replikationspartner, und der Quellverzeichnisdienst wurde mit Hilfe einer Sicherung dieses Partners installiert.<p>Kennung des Zielverzeichnisdiensts:<p>*<GUID> (<FQDN>)*<p>Kennung des gemeinsamen Verzeichnisdiensts:<p>*<GUID>*<p>Gemeinsame Eigenschafts-Aktualisierungssequenznummer:<p>*<Number>*<p>Als Folge dessen wurde der Aktualitätsvektor des Zielverzeichnisdienstes mit den folgenden Einstellungen konfiguriert.<p>Vorherige Objekt-Aktualisierungssequenznummer:<p>0<p>Vorherige Eigenschafts-Aktualisierungssequenznummer:<p>0<p>Datenbank-GUID:<p>*<GUID>*<p>Objekt-Aktualisierungssequenznummer:<p>*<Number>*<p>Eigenschafts-Aktualisierungssequenznummer:<p>*<Number>* |

##### <a name="system-event-log"></a>Systemereignisprotokoll
Die nächsten Hinweise zu Klonvorgängen befinden sich im Systemereignisprotokoll. Wenn der Hypervisor dem Gastcomputer mitteilt, dass er geklont oder aus einer Momentaufnahme wiederhergestellt wurde, macht der Domänencontroller sofort seinen RID-Pool ungültig, um ein späteres Duplizieren von Sicherheitsprinzipalen zu vermeiden. Wenn das Klonen fortgesetzt wird, kommt es zu verschiedenen erwarteten Vorgängen und Meldungen, hauptsächlich rund um das Starten und Anhalten von Diensten und die dadurch verursachten Fehler. Nach Abschluss des Klonvorgangs notiert das Systemereignisprotokoll einen Erfolg des gesamten Klonvorgangs.

|**Ereignis-ID**|**Quelle**|**Meldung**|
|--|--|--|
|**16654**|Verzeichnisdienst-SAM|Ein Pool aus Kontobezeichnern (RIDs) wurde ungültig gemacht. Dies kann in folgenden erwarteten Fällen vorkommen:<p>1. ein Domänen Controller wird aus der Sicherung wieder hergestellt.<p>2. ein Domänen Controller, der auf einer virtuellen Maschine ausgeführt wird, wird aus einer Momentaufnahme wieder hergestellt<p>3. ein Administrator hat den Pool manuell ungültig gemacht.|
|**7036**|Dienststeuerungs-Manager|Der Dienst der Active Directory-Verzeichnisdienste wird jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Der Dienst für das Kerberos-Schlüsselverteilungscenter wird jetzt ausgeführt.|
|**3096**|Netlogon|Der primäre Domänencontroller für diese Domäne konnte nicht gefunden werden.|
|**7036**|Dienststeuerungs-Manager|Der Dienst für den Sicherheitskonten-Manager wird jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Der Serverdienst wird jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Der Netlogon-Dienst wird jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Die Active Directory-Webdienste werden jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Der DFS-Replikationsdienst wird jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Der Dateireplikationsdienst wird jetzt ausgeführt.|
|**14533**|Microsoft-Windows-DfsSvc|DFS hat alle Namespaces erstellt.|
|**14531**|Microsoft-Windows-DfsSvc|DFS-Server wurde vollständig initialisiert.|
|**7036**|Dienststeuerungs-Manager|Der DFS-Namespacedienst wird jetzt ausgeführt.|
|**7023**|Dienststeuerungs-Manager|Der standortübergreifende Messagingdienst wurde mit folgendem Fehler beendet:<p>Der angegebene Server kann den angeforderten Vorgang nicht ausführen.|
|**7036**|Dienststeuerungs-Manager|Der standortübergreifende Messagingdienst wurde beendet.|
|**5806**|Netlogon|Dynamische Aktualisierungen wurden auf diesem Domänencontroller manuell deaktiviert.<p>BENUTZERAKTION<p>Konfigurieren Sie diesen Domänencontroller zur Verwendung dynamischer Aktualisierungen neu, oder fügen Sie die DNS-DNS-Datensätze aus der Datei '%SystemRoot%\System32\Config\Netlogon.dns' manuell zur DNS-Datenbank hinzu.|
|**16651**|Verzeichnisdienst-SAM|Die Anforderung eines neuen Kontenidentifizierungspool ist fehlgeschlagen. Der Vorgang wird solange wiederholt, bis er erfolgreich ausgeführt wird. Der Fehler ist:<p>Der angeforderte FSMO-Vorgang konnte nicht ausgeführt werden. Der aktuelle FSMO-Inhaber war nicht erreichbar.|
|**7036**|Dienststeuerungs-Manager|Der DNS-Serverdienst wird jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Der DS-Rollenserverdienst wird jetzt ausgeführt.|
|**7036**|Dienststeuerungs-Manager|Der Netlogon-Dienst wurde beendet.|
|**7036**|Dienststeuerungs-Manager|Der Dateireplikationsdienst wurde beendet.|
|**7036**|Dienststeuerungs-Manager|Der Dienst für das Kerberos-Schlüsselverteilungscenter wurde beendet.|
|**7036**|Dienststeuerungs-Manager|Der DNS-Serverdienst wurde beendet.|
|**7036**|Dienststeuerungs-Manager|Der Dienst der Active Directory-Verzeichnisdienste wurde beendet.|
|**7036**|Dienststeuerungs-Manager|Der Netlogon-Dienst wird jetzt ausgeführt.|
|**7.040**|Dienststeuerungs-Manager|Der Starttyp der Active Directory-Domänendienste wurde von automatischem Start zu deaktiviert geändert.|
|**7036**|Dienststeuerungs-Manager|Der Netlogon-Dienst wurde beendet.|
|**7036**|Dienststeuerungs-Manager|Der Dateireplikationsdienst wird jetzt ausgeführt.|
|**29219**|DirectoryServices-DSROLE-Server|Der virtuelle Domänencontroller wurde erfolgreich geklont.|
|**29223**|DirectoryServices-DSROLE-Server|Dieser Server ist jetzt ein Domänencontroller.|
|**29265**|DirectoryServices-DSROLE-Server|Der virtuelle Domänencontroller wurde erfolgreich geklont. Die für das Klonen des virtuellen Domänencontrollers verwendete Konfigurationsdatei C:\Windows\NTDS\DCCloneConfig.xml wurde in C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml umbenannt.|
|**1074**|User32|Der Prozess C:\Windows\system32\lsass.exe (DC2) hat den Neustart von Computer DC2 im Auftrag des Benutzers NT-Autorität \ System aus folgendem Grund initiiert: Betriebs System: Neukonfiguration (geplant)<p>Ursachen Code: 0x80020004<p>Herunterfahrtyp: Neustart<p>Kommentar: „|

##### <a name="dcpromolog"></a>DCPROMO.LOG
Dcpromo.log enthält den tatsächlichen Heraufstufungsteil des Klonvorgangs, der im Verzeichnisdienst-Ereignisprotokoll nicht beschrieben ist. Das das Protokoll keine Erläuterung zur Bedeutung der Ereignisprotokolleinträge bietet, enthält dieser Abschnitt des Moduls zusätzliche Anmerkungen.

Der Heraufstufungsprozess bedeutet, dass der Klonvorgang beginnt, der Domänencontroller von seiner aktuellen Konfiguration bereinigt und über die vorhandene AD-Datenbank erneut heraufgestuft wird (ähnlich einer IFM-Heraufstufung). Dann repliziert der Domänencontroller eingehende Änderungsdeltas von AD und SYSVOL, und das Klonen ist abgeschlossen.

> [!NOTE]
> Das Protokoll wurde in diesem Modul aus Gründen der besseren Lesbarkeit geändert, indem die Datumsspalte entfernt wurde.
>
> Ein weitere Erläuterung zu dcpromo.log finden Sie unter „Grundlegendes über und Problembehandlung für die vereinfachte AD DS-Verwaltung in Windows Server 2012“.
>
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)

- Starten der klonbasierten Heraufstufung

- Setzen Sie das Kennzeichen für den Verzeichnisdienst-Wiederherstellungsmodus, damit der Server nicht wieder normal neu gestartet wird wie der ursprüngliche Klon und Benennungs- oder Verzeichnisdienstkollisionen verursacht.

- Aktualisieren Sie das Verzeichnisdienst-Ereignisprotokoll.

```
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.
```

- Beenden Sie den NetLogon-Dienst, damit der Domänencontroller nicht ankündigt.

```
15:14:01 [INFO] Stopping service NETLOGON
15:14:01 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)
15:14:01 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states
15:14:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3
15:14:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1
15:14:02 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state
15:14:02 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0
15:14:02 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)
15:14:02 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state
15:14:02 [INFO] StopService on NETLOGON returned 0
15:14:02 [INFO] Configuring service NETLOGON to 1 returned 0
15:14:02 [INFO] Updating service status to 4
15:14:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- Untersuchen Sie die Datei dccloneconfig.xml auf administratorspezifische Anpassungen.

- In diesem Beispielfall ist die Datei leer, sodass alle Einstellungen automatisch generiert werden und eine automatische IP-Adressierung vom Netzwerk erforderlich ist.

```
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0
```

- Vergewissern Sie sich, dass keine Dienste oder Programme installiert sind, die nicht Teil von DefaultDCCloneAllowList.xml oder CustomDCCloneAllowList.xml sind.

```
15:14:02 [INFO] vDC Cloning: Checking allowed list:
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- Aktivieren Sie DHCP auf den Netzwerkadaptern, da die IP-Informationen nicht vom Administrator angegeben wurden.

```
15:14:03 [INFO] vDC Cloning: Enable DHCP:
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12
15:14:03 [INFO] Method: EnableDHCP
15:14:03 [INFO] HRESULT code: 0x0 (0)
15:14:03 [INFO] Return Value: 0x0 (0)
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- Suchen Sie nach dem PDC-Emulator.

- Legen Sie den Standort des Klons fest (wird in diesem Fall automatisch generiert).

- Legen Sie den Namen des Klons fest (wird in diesem Fall automatisch generiert).

```
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name
```

- Erstellen Sie das neue Kloncomputerobjekt.

- Benennen Sie den Klon gemäß dem neuen Namen um.

```
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)
```

- Stellen Sie die Einstellungen für die Heraufstufung bereit, basierend auf der früheren dccloneconfig.xml oder automatischen Generierungsregeln.

```
15:14:05 [INFO] vDC Cloning: Promotion parameters setting:
15:14:05 [INFO] DNS Domain Name: root.fabrikam.com
15:14:05 [INFO] Replica Partner: \\DC1.root.fabrikam.com
15:14:05 [INFO] Site Name: Default-First-Site-Name
15:14:05 [INFO] DS Database Path: C:\Windows\NTDS
15:14:05 [INFO] DS Log Path: C:\Windows\NTDS
15:14:05 [INFO] SysVol Root Path: C:\Windows\SYSVOL
15:14:05 [INFO] Account: root.fabrikam.com\DC2-CL0001$
15:14:05 [INFO] Options: DSROLE_DC_CLONING (0x800400)
```

- Starten Sie die Heraufstufung.

```
15:14:05 [INFO] Promote DC as a clone
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #3: Domain Controller cloning is at 15% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #4: Domain Controller cloning is at 16% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] Validate supplied paths
15:14:05 [INFO] Validating path C:\Windows\NTDS.
15:14:05 [INFO] Path is a directory
15:14:05 [INFO] Path is on a fixed disk drive.
15:14:05 [INFO] Validating path C:\Windows\NTDS.
15:14:05 [INFO] Path is a directory
15:14:05 [INFO] Path is on a fixed disk drive.
15:14:05 [INFO] Validating path C:\Windows\SYSVOL.
15:14:05 [INFO] Path is on a fixed disk drive.
15:14:05 [INFO] Path is on an NTFS volume
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #5: Domain Controller cloning is at 17% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] Start the worker task
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #6: Domain Controller cloning is at 20% completion...
15:14:05 [INFO] Request for promotion returning 0
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #7: Domain Controller cloning is at 21% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- Stoppen Sie, und konfigurieren Sie alle AD DS-relevanten Dienste (NTDS, NTFRS/DFSR, KDC, DNS).

> [!NOTE]
> Dass der DNS-Dienst zum Herunterfahren lange braucht, ist in diesem Szenario erwartet, da er AD-integrierte Zonen verwendet, die auch vor dem Stoppen des NTDS-Diensts nicht mehr verfügbar waren. Weitere Informationen dazu finden Sie in den später in diesem Abschnitt des Moduls beschriebenen DNS-Ereignissen.

```
15:14:15 [INFO] Stopping service NTDS
15:14:15 [INFO] Stopping service NtFrs
15:14:15 [INFO] ControlService(STOP) on NtFrs returned 1(gle=0)
15:14:15 [INFO] DsRolepWaitForService: waiting for NtFrs to enter one of 7 states
15:14:15 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=3
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=1
15:14:16 [INFO] DsRolepWaitForService: exiting because NtFrs entered STOPPED state
15:14:16 [INFO] DsRolepWaitForService(for any end state) on NtFrs service returned 0
15:14:16 [INFO] ControlService(STOP) on NtFrs returned 0(gle=1062)
15:14:16 [INFO] Exiting service-stop loop after service NtFrs entered STOPPED state
15:14:16 [INFO] StopService on NtFrs returned 0
15:14:16 [INFO] Configuring service NtFrs to 1 returned 0
15:14:16 [INFO] Stopping service Kdc
15:14:16 [INFO] ControlService(STOP) on Kdc returned 1(gle=0)
15:14:16 [INFO] DsRolepWaitForService: waiting for Kdc to enter one of 7 states
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=3
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=1
15:14:17 [INFO] DsRolepWaitForService: exiting because Kdc entered STOPPED state
15:14:17 [INFO] DsRolepWaitForService(for any end state) on Kdc service returned 0
15:14:17 [INFO] ControlService(STOP) on Kdc returned 0(gle=1062)
15:14:17 [INFO] Exiting service-stop loop after service Kdc entered STOPPED state
15:14:17 [INFO] StopService on Kdc returned 0
15:14:17 [INFO] Configuring service Kdc to 1 returned 0
15:14:17 [INFO] Stopping service DNS
15:14:17 [INFO] ControlService(STOP) on DNS returned 1(gle=0)
15:14:17 [INFO] DsRolepWaitForService: waiting for DNS to enter one of 7 states
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:18 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:19 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:20 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:21 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:22 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:23 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:24 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:25 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:26 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:27 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:28 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:29 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:30 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:31 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:32 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:33 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:34 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:35 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:36 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:37 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:38 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:39 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:40 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:41 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:42 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:43 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:44 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:45 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:46 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:47 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:48 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:49 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:50 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:51 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:52 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:53 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:54 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:55 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:56 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:57 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:58 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:59 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=1
15:15:00 [INFO] DsRolepWaitForService: exiting because DNS entered STOPPED state
15:15:00 [INFO] DsRolepWaitForService(for any end state) on DNS service returned 0
15:15:00 [INFO] ControlService(STOP) on DNS returned 0(gle=1062)
15:15:00 [INFO] Exiting service-stop loop after service DNS entered STOPPED state
15:15:00 [INFO] StopService on DNS returned 0
15:15:00 [INFO] Configuring service DNS to 1 returned 0
15:15:00 [INFO] ControlService(STOP) on NTDS returned 1(gle=1062)
15:15:00 [INFO] DsRolepWaitForService: waiting for NTDS to enter one of 7 states
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=3
15:15:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=1
15:15:01 [INFO] DsRolepWaitForService: exiting because NTDS entered STOPPED state
15:15:01 [INFO] DsRolepWaitForService(for any end state) on NTDS service returned 0
15:15:01 [INFO] ControlService(STOP) on NTDS returned 0(gle=1062)
15:15:01 [INFO] Exiting service-stop loop after service NTDS entered STOPPED state
15:15:01 [INFO] StopService on NTDS returned 0
15:15:01 [INFO] Configuring service NTDS to 1 returned 0
15:15:01 [INFO] Configuring service NTDS
15:15:01 [INFO] Configuring service NTDS to 64 returned 0
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #8: Domain Controller cloning is at 22% completion...
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #9: Domain Controller cloning is at 25% completion...
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- Erzwingen Sie die NT5DS (NTP)-Zeitsynchronisierung mit einem anderen Domänencontroller (normalerweise dem PDCE).

```
15:15:02 [INFO] Forcing time sync
```

- Kontaktieren Sie einen Domänencontroller, auf dem das Konto des Quelldomänencontrollers des Klons vorhanden ist.

- Löschen Sie alle vorhandenen Kerberos-Tickets.

```
15:15:02 [INFO] Searching for a domain controller for the domain root.fabrikam.com that contains the account DC2$
15:15:02 [INFO] Located domain controller DC1.root.fabrikam.com for domain root.fabrikam.com
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #10: Domain Controller cloning is at 26% completion...
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:02 [INFO] Directing kerberos authentication to DC1.root.fabrikam.com returns 0
15:15:02 [INFO] DsRolepFlushKerberosTicketCache() successfully flushed the Kerberos ticket cache
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #11: Domain Controller cloning is at 27% completion...
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:02 [INFO] Using site Default-First-Site-Name for server \\DC1.root.fabrikam.com
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- Stoppen Sie den NetLogon-Dienst, und legen Sie seinen Starttyp fest.

```
15:15:02 [INFO] Stopping service NETLOGON
15:15:02 [INFO] Stopping service NETLOGON
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #12: Domain Controller cloning is at 29% completion...
15:15:02 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)
15:15:02 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states
15:15:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3
15:15:03 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1
15:15:03 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state
15:15:03 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0
15:15:03 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)
15:15:03 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state
15:15:03 [INFO] StopService on NETLOGON returned 0
15:15:03 [INFO] Configuring service NETLOGON to 1 returned 0
15:15:03 [INFO] Stopped NETLOGON
15:15:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:03 [INFO] vDC Cloning: Winlogon UI Notification #13: Domain Controller cloning is at 30% completion...
```

- Konfigurieren Sie die automatische Ausführung für die DFSR/NTFRS-Dienste.

- Löschen Sie ihre vorhandenen Datenbankdateien, um eine nicht autoritative Synchronisierung von SYSVOL beim nächsten Start des Dienstes zu erzwingen.

```
15:15:03 [INFO] Configuring service DFSR
15:15:03 [INFO] Configuring service DFSR to 256 returned 0
15:15:03 [INFO] Configuring service NTFRS
15:15:03 [INFO] Configuring service NTFRS to 256 returned 0
15:15:03 [INFO] Removing DFSR Database files for SysVol
15:15:03 [INFO] Removing FRS Database files in C:\Windows\ntfrs\jet
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edb.log
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00001.jrs
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00002.jrs
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbtmp.log
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\ntfrs.jdb
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\sys\edb.chk
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\temp\tmp.edb
15:15:04 [INFO] Created system volume path
15:15:04 [INFO] Configuring service DFSR
15:15:04 [INFO] Configuring service DFSR to 128 returned 0
15:15:04 [INFO] Configuring service NTFRS
15:15:04 [INFO] Configuring service NTFRS to 128 returned 0
15:15:04 [INFO] vDC Cloning: Winlogon UI Notification #14: Domain Controller cloning is at 40% completion...
15:15:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- Starten Sie den Heraufstufungsprozess mit der vorhandenen NTDS-Datenbankdatei.

- Kontaktieren Sie den RID-Master.

> [!NOTE]
> Der AD DS-Dienst ist hier nicht tatsächlich installiert, dies ist eine Vorversionsinstrumentation im Protokoll.

```
15:15:04 [INFO] Installing the Directory Service
15:15:04 [INFO] Calling NtdsInstall for root.fabrikam.com
15:15:04 [INFO] Starting Active Directory Domain Services installation
15:15:04 [INFO] Validating user supplied options
15:15:04 [INFO] Determining a site in which to install
15:15:04 [INFO] Examining an existing forest...
15:15:04 [INFO] Starting a replication cycle between DC1.root.fabrikam.com and the RID operations master (2008r2-01.root.fabrikam.com), so that the new replica will be able to create users, groups, and computer objects...
15:15:04 [INFO] Configuring the local computer to host Active Directory Domain Services
15:15:04 [INFO] EVENTLOG (Warning): NTDS General / Service Control : 1539
Active Directory Domain Services could not disable the software-based disk write cache on the following hard disk.
Hard disk:
c:
Data might be lost during system failures.
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Processing : 2041
Duplicate event log entries were suppressed.
See the previous event log entry for details. An entry is considered a duplicate if
the event code and all of its insertion parameters are identical. The time period for
this run of duplicates is from the time of the previous event to the time of this event.
Event Code:
80000603
Number of duplicate entries:
2
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Configuration : 2121
This Active Directory Domain Services server is disabling the Recycle Bin. Deleted objects may not be undeleted at this time.
```

- Ändern Sie die vorhandene Aufruf-ID, die in der Datenbank des Quellcomputers vorhanden war.

- Erstellen Sie ein neues NTDS-Einstellungsobjekt für diesen Klon.

- Replizieren Sie das AD-Objektdelta vom Partnerdomänencontroller.

> [!NOTE]
> Auch wenn alle Objekte als repliziert aufgelistet sind, handelt es sich nur um Metadaten, die für die Zusammenfassung der Aktualisierungen erforderlich sind. Alle unveränderten Objekte in der geklonten NTDS-Datenbank sind bereits vorhanden und müssen nicht erneut repliziert werden, wie bei der Verwendung der IFM-basierten Heraufstufung.

```
15:15:10 [INFO] EVENTLOG (Informational): NTDS Replication / Replication : 1109
The invocationID attribute for this directory server has been changed. The highest update sequence number at the time the backup was created is as follows:
InvocationID attribute (old value):
24e7b22f-4706-402d-9b4f-f2690f730b40
InvocationID attribute (new value):
f74cefb2-89c2-442c-b1ba-3234b0ed62f8
Update sequence number:
20520
The invocationID is changed when a directory server is restored from backup media, is configured to host a writeable application directory partition, has been resumed after a virtual machine snapshot has been applied, after a virtual machine import operation, or after a live migration operation. Virtualized domain controllers should not be restored using virtual machine snapshots. The supported method to restore or rollback the content of an Active Directory Domain Services database is to restore a system state backup made with an Active Directory Domain Services-aware backup application.
15:15:10 [INFO] EVENTLOG (Error): NTDS General / Internal Processing : 1168
Internal error: An Active Directory Domain Services error has occurred.
Additional Data
Error value (decimal):
2
Error value (hexadecimal):
2
Internal ID:
7011658
15:15:11 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC1.root.fabrikam.com...
15:15:11 [INFO] Replicating the schema directory partition
15:15:11 [INFO] Replicated the schema container.
15:15:12 [INFO] Active Directory Domain Services updated the schema cache.
15:15:12 [INFO] Replicating the configuration directory partition
15:15:12 [INFO] Replicating data CN=Configuration,DC=root,DC=fabrikam,DC=com: Received 2612 out of approximately 2612 objects and 94 out of approximately 94 distinguished name (DN) values...
15:15:12 [INFO] Replicated the configuration container.
15:15:13 [INFO] Replicating critical domain information...
15:15:13 [INFO] Replicating data DC=root,DC=fabrikam,DC=com: Received 109 out of approximately 109 objects and 35 out of approximately 35 distinguished name (DN) values...
15:15:13 [INFO] Replicated the critical objects in the domain container.
```

- Füllen Sie die Partitionen des globalen Katalogservers nach Bedarf mit fehlenden Aktualisierungen.

- Schließen Sie den kritischen AD DS-Teil der Heraufstufung ab.

```
15:15:13 [INFO] EVENTLOG (Informational): NTDS General / Global Catalog : 1110
Promotion of this domain controller to a global catalog will be delayed for the following interval.
Interval (minutes):
5
This delay is necessary so that the required directory partitions can be prepared before the global catalog is advertised. In the registry, you can specify the number of seconds that the directory system agent will wait before promoting the local domain controller to a global catalog. For more information about the Global Catalog Delay Advertisement registry value, see the Resource Kit Distributed Systems Guide.
15:15:14 [INFO] EVENTLOG (Informational): NTDS General / Service Control : 1000
Microsoft Active Directory Domain Services startup complete, version 6.2.8225.0
15:15:15 [INFO] Creating new domain users, groups, and computer objects
15:15:16 [INFO] Completing Active Directory Domain Services installation
15:15:16 [INFO] NtdsInstall for root.fabrikam.com returned 0
15:15:16 [INFO] DsRolepInstallDs returned 0
15:15:16 [INFO] Installed Directory Service
```

- Schließen Sie die eingehende Replikation von SYSVOL ab.

```
15:15:16 [INFO] vDC Cloning: Winlogon UI Notification #15: Domain Controller cloning is at 60% completion...
15:15:16 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] Completed system volume replication
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #16: Domain Controller cloning is at 70% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] SetProductType to 2 [LanmanNT] returned 0
15:15:18 [INFO] Set the product type
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #17: Domain Controller cloning is at 71% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #18: Domain Controller cloning is at 72% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] Set the system volume path for NETLOGON
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #19: Domain Controller cloning is at 73% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] Replicating non critical information
15:15:18 [INFO] User specified to not replicate non-critical data
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #20: Domain Controller cloning is at 80% completion...
15:15:18 [INFO] Stopped the DS
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #21: Domain Controller cloning is at 90% completion...
15:15:18 [INFO] Configuring service NTDS
15:15:18 [INFO] Configuring service NTDS to 16 returned 0
```

- Aktivieren Sie die Client-DNS-Registrierung.

```
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.
```

- Führen Sie die SYSPREP-Module aus, die vom DefaultDCCloneAllowList.xml <SysprepInformation>-Element angegeben werden.

```
15:15:18 [INFO] vDC Cloning: Running sysprep providers.
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.
```

- Die Heraufstufung des Klonvorgangs ist abgeschlossen.

- Entfernen Sie das DSRM-Startkennzeichen, damit der Server beim nächsten Mal normal gestartet wird.

- Benennen Sie die Datei dccloneconfig.xml um, damit Sie nicht beim nächsten Start erneut gelesen wird.

- Starten Sie den Computer neu.

```
15:15:32 [INFO] The attempted domain controller operation has completed
15:15:32 [INFO] Updating service status to 4
15:15:32 [INFO] DsRolepSetOperationDone returned 0
15:15:32 [INFO] vDC Cloning: Set vDCCloningComplete event.
15:15:32 [INFO] vDC Cloneing: Clearing Boot into DSRM flag succeeded.
15:15:32 [INFO] vDC Cloning: Winlogon UI Notification #22: Cloning Domain Controller succeeded. Now rebooting...
15:15:33 [INFO] vDC Cloning: Renamed vDC clone configuration file.
15:15:33 [INFO] vDC Cloning: The old name is: C:\Windows\NTDS\DCCloneConfig.xml
15:15:33 [INFO] vDC Cloning: The new name is: C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml
15:15:34 [INFO] vDC Cloning: Release Ipv4 on interface 'Wired Ethernet Connection 2', result=0.
15:15:34 [INFO] vDC Cloning: Release Ipv6 on interface 'Wired Ethernet Connection 2', result=0.
15:15:34 [INFO] Rebooting machine
```

##### <a name="active-directory-web-services-event-log"></a>Ereignisprotokoll der Active Directory-Webdienste
Während des Klonens ist die NTDS.DIT-Datenbank oft für längere Zeit offline geschaltet. Der ADWS-Dienst protokolliert dafür mindestens ein Ereignis. Wenn das Klonen abgeschlossen ist, wird der ADWS-Dienst neu gestartet und bemerkt, dass noch kein gültiges Computerzertifikat vorhanden ist (je nachdem, ob In Ihrer Umgebung eine Microsoft PKI mit automatischer Registrierung vorhanden ist, kann dies sein oder nicht), und startet dann die Instanz für den neuen Domänencontroller.

|**Ereignis-ID**|**Quelle**|**Meldung**|
|--|--|--|
|**1202**|Ereignisse der ADWS-Instanz|Auf diesem Computer wird nun die angegebene Verzeichnisinstanz gehostet, doch konnte diese von Active Directory-Webdiensten nicht bedient werden. Von Active Directory-Webdiensten wird in regelmäßigen Abständen erneut versucht, den Vorgang auszuführen.<p>Verzeichnis Instanz: NTDS<p>LDAP-Port der Verzeichnis Instanz: 389<p>SSL-Port der Verzeichnis Instanz: 636|
|**1000**|Ereignisse der ADWS-Instanz|Die Active Directory-Webdienste werden gestartet.|
|**1008**|Ereignisse der ADWS-Instanz|Die Sicherheitsberechtigungen von Active Directory-Webdiensten wurden erfolgreich reduziert.|
|**1100**|Ereignisse der ADWS-Instanz|Die Werte im Abschnitt <appsettings> der Konfigurationsdatei der Active Directory-Webdienste wurden fehlerfrei geladen.|
|**1400**|Ereignisse der ADWS-Instanz|ADWS-Zertifikatereignisse"Von den Active Directory-Webdiensten konnte kein Serverzertifikat mit dem angegebenen Zertifikatnamen gefunden werden. Für Zertifikate ist die Verwendung von SSL/TLS-Verbindungen erforderlich. Wenn Sie SSL/TLS-Verbindungen verwenden möchten, stellen Sie sicher, dass auf dem Computer ein gültiges Serverauthentifizierungszertifikat von einer vertrauenswürdigen Zertifizierungsstelle installiert ist.<p>Zertifikats Name: *<Server FQDN>*|
|**1100**|Ereignisse der ADWS-Instanz|Die Werte im Abschnitt <appsettings> der Konfigurationsdatei der Active Directory-Webdienste wurden fehlerfrei geladen.|
|**1200**|Ereignisse der ADWS-Instanz|Die angegebene Verzeichnisinstanz wird nun von Active Directory-Webdiensten bedient.<p>Verzeichnis Instanz: NTDS<p>LDAP-Port der Verzeichnis Instanz: 389<p>SSL-Port der Verzeichnis Instanz: 636|

##### <a name="dns-server-event-log"></a>Ereignisprotokoll des DNS-Servers
Beim DNS-Dienst treten während des Klonvorgangs kurze erwartete Ausfälle auf, da der DNS-Dienst weiter ausgeführt wird, während die AD DS-Datenbank offline geschaltet ist. Dazu kommt es, wenn Sie den in Active Directory integrierten DNS verwenden, nicht, wenn Sie den standardmäßigen primären oder sekundären DNS verwenden. Diese Fehler werden mehrmals protokolliert. Nach Abschluss des Klonvorgangs ist DNS wieder ganz normal online.

|**Ereignis-ID**|**Quelle**|**Meldung**|
|--|--|--|
|**4013**|DNS-Server-Service|Der DNS-Server wartet darauf, dass von den Active Directory-Domänendiensten angezeigt wird, dass die Erstsynchronisierung des Verzeichnisses durchgeführt wurde. Der DNS-Serverdienst kann erst nach der Erstsynchronisierung gestartet werden, da wichtige DNS-Daten möglicherweise noch nicht auf diesen Domänencontroller repliziert wurden. Sofern die im Ereignisprotokoll der Active Directory-Domänendienste protokollierten Ereignisse deutlich machen, dass Probleme bei der DNS-Namensauflösung vorliegen, sollte ggf. die IP-Adresse eines weiteren DNS-Servers für diese Domäne der DNS-Serverliste in den Internetprotokolleigenschaften dieses Computers hinzugefügt werden. Dieses Ereignis wird alle zwei Minuten protokolliert, bis von den Active Directory-Domänendiensten angezeigt wird, dass die Erstsynchronisierung durchgeführt wurde.|
|**4015**|DNS-Server-Service|Der DNS-Server hat einen kritischen Active Directory-Fehler ermittelt. Stellen Sie sicher, dass Active Directory ordnungsgemäß funktioniert. Die erweiterten Fehlerdebuginformationen (die eventuell leer sind) lauten """. Die Ereignisdaten enthalten den Fehlercode.|
|**4000**|DNS-Server-Service|Der DNS-Server konnte Active Directory nicht öffnen. Dieser DNS-Server ist für das Abrufen und Verwenden von Informationen aus dem Verzeichnis für diese Zone konfiguriert und kann die Zone ohne die Informationen nicht laden. Stellen Sie sicher, dass Active Directory ordnungsgemäß funktioniert, und laden Sie die Zone neu. Die Ereignisdaten enthalten den Fehlercode.|
|**4013**|DNS-Server-Service|Der DNS-Server wartet darauf, dass von den Active Directory-Domänendiensten angezeigt wird, dass die Erstsynchronisierung des Verzeichnisses durchgeführt wurde. Der DNS-Serverdienst kann erst nach der Erstsynchronisierung gestartet werden, da wichtige DNS-Daten möglicherweise noch nicht auf diesen Domänencontroller repliziert wurden. Sofern die im Ereignisprotokoll der Active Directory-Domänendienste protokollierten Ereignisse deutlich machen, dass Probleme bei der DNS-Namensauflösung vorliegen, sollte ggf. die IP-Adresse eines weiteren DNS-Servers für diese Domäne der DNS-Serverliste in den Internetprotokolleigenschaften dieses Computers hinzugefügt werden. Dieses Ereignis wird alle zwei Minuten protokolliert, bis von den Active Directory-Domänendiensten angezeigt wird, dass die Erstsynchronisierung durchgeführt wurde.|
|**2**|DNS-Server-Service|Der DNS-Server wurde gestartet.|
|**4**|DNS-Server-Service|Der DNS-Server hat das Laden von Zonen im Hintergrund abgeschlossen. Alle Zonen sind nun für DNS-Aktualisierungen und Zonenübertragungen verfügbar, sofern ihre jeweilige Konfiguration dies zulässt.|

##### <a name="file-replication-service-event-log"></a>Ereignisprotokoll des Dateireplikationsdiensts
Der Dateireplikationsdienst synchronisiert während des Klonvorgangs nicht autoritativ von einem Partner. Der Klonvorgang erreicht dies durch Löschen der NTFRS-Datenbankdateien und unveränderten Inhalten von SYSVOL zur Verwendung als zuvor per Seeding hinzugefügte Daten. Die zwei Synchronisierungsversuche sind erwartet.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **13562** | NtFrs | Es folgt eine Zusammenfassung aller Warnungen und Fehler, die beim Abfragen des Domänencontrollers DC2.root.fabrikam.com der Konfigurationsinformationen des FRS-Replikatsatzes vom Dateireplikationsdienst ermittelt wurden.<p>Es konnte keine Bindung mit dem Domänencontroller hergestellt werden. Der Vorgang wird beim nächsten Abrufzyklus wiederholt. |
| **13502** | NtFrs | Der Dateireplikationsdienst wird beendet. |
| **13565** | NtFrs | Der Dateireplikationsdienst initialisiert den Systemdatenträger mit Daten eines anderen Domänencontrollers. Computer DC2 kann nicht zum Domänencontroller benannt werden, bis dieser Prozess beendet ist. Das Systemvolumen wird dann als SYSVOL geteilt.<p>Um die SYSVOL-Freigabe zu überprüfen, geben Sie an der Eingabeaufforderung Folgendes ein:<p>Netzwerkfreigabe<p>Wenn der Dateireplikationsdienst den Initialisierungsprozess beendet, wird die SYSVOL-Freigabe angezeigt.<p>Die Initialisierung des Systemdatenträgers kann einige Zeit in Anspruch nehmen. Der Zeitaufwand ist von der Datenmenge im Systemdatenträger, der Verfügbarkeit anderer Domänencontroller, und dem Replikationsintervall zwischen anderen Domänencontrollern abhängig. |
| **13501** | NtFrs | Der Dateireplikationsdienst wird gestartet. |
| **13502** | NtFrs | Der Dateireplikationsdienst wird beendet. |
| **13503** | NtFrs | Der Dateireplikationsdienst wurde beendet. |
| **13565** | NtFrs | Der Dateireplikationsdienst initialisiert den Systemdatenträger mit Daten eines anderen Domänencontrollers. Computer DC2 kann nicht zum Domänencontroller benannt werden, bis dieser Prozess beendet ist. Das Systemvolumen wird dann als SYSVOL geteilt.<p>Um die SYSVOL-Freigabe zu überprüfen, geben Sie an der Eingabeaufforderung Folgendes ein:<p>Netzwerkfreigabe<p>Wenn der Dateireplikationsdienst den Initialisierungsprozess beendet, wird die SYSVOL-Freigabe angezeigt.<p>Die Initialisierung des Systemdatenträgers kann einige Zeit in Anspruch nehmen. Der Zeitaufwand ist von der Datenmenge im Systemdatenträger, der Verfügbarkeit anderer Domänencontroller, und dem Replikationsintervall zwischen anderen Domänencontrollern abhängig. |
| **13501** | NtFrs | Der Dateireplikationsdienst wird gestartet. |
| **13553** | NtFrs | Der Dateireplikationsdienst hat diesen Computer dem folgenden Replikatsatz hinzugefügt:<p>"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<p>Informationen zu diesem Ereignis sind unten angezeigt:<p>DNS-Name des Computers ist  *<Domain Controller FQDN>*<p>Mitgliedsname des Replikat Satzes ist *<Domain Controller>*<p>Stammpfad der Replikat Gruppe ist *<path>*<p>Verzeichnispfad für Replikat Staging *<path>*<p>Pfad des Replikat Arbeitsverzeichnisses ist *<path>* |
| **13520** | NtFrs | Der Datei Replikations Dienst hat die bereits vorhandenen Dateien in in <path> *<path>* \ NtFrs_PreExisting___See_EventLog verschoben.<p>Der Datei Replikations Dienst kann die Dateien in *<path>* \ NtFrs_PreExisting___See_EventLog jederzeit löschen. Dateien können vor dem Löschen gespeichert werden, indem Sie aus *<path>* \ NtFrs_PreExisting___See_EventLog kopiert werden. Das Kopieren der Dateien nach c:\windows\sysvol\domain kann zu Namenskonflikten führen, wenn die Dateien auf einem anderen replizierenden Computer bereits vorhanden sind.<p>In einigen Fällen kopiert der Datei Replikations Dienst eine Datei von *<path>* \ NtFrs_PreExisting___See_EventLog in, *<path>* anstatt die Datei von einem anderen replizierenden Partner zu replizieren.<p>Der Speicherplatz kann jederzeit wieder hergestellt werden, indem die Dateien in *<path>* \ NtFrs_PreExisting___See_EventLog gelöscht werden. " |
| **13508** | NtFrs | der Datei Replikations Dienst hat Probleme beim Aktivieren der Replikation von *\\\\<Domain Controller FQDN>* zu *<Domain Controller>* für *<path>* die Verwendung des<p>DNS-Name *\\\\<Domain Controller FQDN>* . Es wird ein neuer Versuch gestartet.<p>Mögliche Ursachen für diese Warnung sind:<p>[1] der DNS-Name kann von diesem Computer nicht ordnungsgemäß aufgelöst werden *\\\\<Domain Controller FQDN>* .<p>[2] der FRS wird auf nicht ausgeführt *\\\\<Domain Controller FQDN>* .<p>[3] Die Topologieinformationen im Active Directory dieses Replikats wurden noch nicht auf allen Domänencontrollern repliziert.<p>Diese Ereignisprotokollmeldung wird einmal pro Verbindung angezeigt. Nachdem der Fehler behoben wurde, wird eine andere Ereignisprotokollmeldung angezeigt, die bestätigt, dass die Verbindung hergestellt wurde. |
| **13509** | NtFrs | Der Datei Replikations Dienst hat die Replikation von nach *\\\\<Domain Controller FQDN>* *<Domain Controller>* *<Path>* wiederholten Wiederholungen von auf aktiviert. |
| **13516** | NtFrs | Der Datei Replikations Dienst verhindert nicht mehr, dass der Computer *<Domain Controller>* zu einem Domänen Controller wird. Der Systemdatenträger wurde erfolgreich initialisiert. Der Netzwerkanmeldedienst wurde benachrichtigt, dass der Systemdatenträger jetzt als SYSVOL freigegeben werden kann.<p>Geben Sie "net share" ein, um die SYSVOL-Freigabe zu überprüfen." |

##### <a name="dfs-replication-event-log"></a>Ereignisprotokoll der DFS-Replikation
Die DFSR-Dienste synchronisieren während des Klonvorgangs nicht autoritativ von einem Partner. Der Klonvorgang erreicht dies durch Löschen der DFSR-Datenbankdateien und unveränderten Inhalten von SYSVOL zur Verwendung als zuvor per Seeding hinzugefügte Daten. Die zwei Synchronisierungsversuche sind erwartet.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **1004** | DFSR | Der DFS-Replikationsdienst wurde gestartet. |
| **1314** | DFSR | Der DFS-Replikationsdienst hat die Debugprotokolldateien erfolgreich konfiguriert.<p>Weitere Informationen:<p>Pfad der Debugprotokoll Datei: c:\windows\debug |
| **6102** | DFSR | Der DFS-Replikationsdienst hat den WMI-Anbieter erfolgreich registriert. |
| **1206** | DFSR | Der DFS-Replikationsdienst hat die Verbindung zum Domänencontroller DC2.corp.contoso.com zum Zugriff auf Konfigurationsinformationen erfolgreich hergestellt. |
| **1210** | DFSR | Der DFS-Replikationsdienst hat erfolgreich einen RPC-Listener für eingehende Replikationsanforderungen eingerichtet.<p>Weitere Informationen:<p>Port: 0 " |
| **4614** | DFSR | Der DFS-Replikationsdienst hat SYSVOL im lokalen Pfad C:\Windows\SYSVOL\domain initialisiert und wartet darauf, die erste Replikation auszuführen. Der replizierte Ordner bleibt im ersten Synchronisierungsstatus, bis er mit seinem Partner repliziert wurde. Wenn der Server zu einem Domänencontroller heraufgestuft wurde, führt der Domänencontroller keine Ankündigung durch und dient als Domänencontroller, bis dieses Problem behoben wurde. Dies kann darauf zurückzuführen sein, dass der angegebene Partner sich selbst in einem ersten Synchronisierungsstatus befindet. Eine weitere mögliche Ursache ist, dass auf diesem Server oder beim Synchronisierungspartner Zugriffsverletzungen aufgetreten sind. Wenn dieses Ereignis bei der Migration von SYSVOL aus dem Dateireplikationsdienst (FRS) zur DFS-Replikation aufgetreten ist, werden die Änderungen nicht nach außen repliziert, bis dieses Problem behoben wurde. Dies kann dazu führen, dass der SYSVOL-Ordner auf diesem Server nicht mehr mit anderen Domänencontrollern synchron ist.<p>Weitere Informationen:<p>Name des replizierten Ordners: SYSVOL-Freigabe<p>ID des replizierten Ordners: *<GUID>*<p>Replikations Gruppen Name: Domänen System Volume<p>Replikations Gruppen-ID: *<GUID>*<p>Mitglieds-ID: *<GUID>*<p>Schreibgeschützt: 0 |
| **4604** | DFSR | Der DFS-Replikationsdienst hat den SYSVOL-replizierten Ordner im lokalen PfadC:\Windows\SYSVOL\domain erfolgreich initialisiert. Dieses Mitglied hat die erste Synchronisierung von SYSVOL mit Partner  dc1.corp.contoso.com abgeschlossen. Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie dann "net share" ein, um das Vorhandensein der SYSVOL-Freigabe zu prüfen.<p>Weitere Informationen:<p>Name des replizierten Ordners: SYSVOL-Freigabe<p>ID des replizierten Ordners: *<GUID>*<p>Replikations Gruppen Name: Domänen System Volume<p>Replikations Gruppen-ID: *<GUID>*<p>Mitglieds-ID: *<GUID>*<p>Synchronisierungs Partner: *<domain controller FQDN>* |

## <a name="troubleshooting-virtualized-domain-controller-safe-restore"></a><a name="BKMK_TshootVDCSafeRestore"></a>Problembehandlung bei bei der sicheren Wiederherstellung des virtualisierten Domänencontrollers

### <a name="tools-for-troubleshooting"></a>Tools zur Problembehandlung

#### <a name="logging-options"></a>Protokollierungsoptionen
Die integrierten Protokolle sind das wichtigste Tool für die Problembehandlung bei der sicheren Momentaufnahmenwiederherstellung von Domänencontrollern. Alle Protokolle sind standardmäßig für maximale Ausführlichkeit aktiviert und konfiguriert.

|**Vorgang**|**Log**|
|--|--|
|**Erstellung der Momentaufnahme**|-Ereignisviewer\anwendungs-und dienstprotokolle\microsoft\windows\hyper-v-Worker|
|**Momentaufnahmenwiederherstellung**|-Ereignisviewer\anwendungs-und dienstprotokolle\verzeichnisdienst<br />-Ereignisviewer\windows-Protokolle\System<br />-Ereignisviewer\windows-Protokolle\Anwendung<br />-Ereignisviewer\anwendungs-und dienstprotokolle\datei Replikations Dienst<br />-Ereignisviewer\anwendungs-und dienstprotokolle\dfs-Replikation<br />-Ereignisviewer\anwendungs-und dienstprotokolle\dns<br />-Ereignisviewer\anwendungs-und dienstprotokolle\microsoft\windows\hyper-v-Worker|

#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Tools und Befehle für die Problembehandlung bei der Konfiguration von Domänencontrollern
Verwenden Sie zum Beheben von Problemen, die nicht von den Protokollen erklärt werden, die folgenden Tools als Ausgangspunkt:

- Dcdiag.exe

- Repadmin.exe

- Network Monitor 3.4

#### <a name="general-methodology-for-troubleshooting-domain-controller-safe-restore"></a><a name="BKMK_TshhotSafeRestore"></a>Allgemeine Methode für die Problembehandlung bei der sicheren Wiederherstellung von Domänencontrollern

1. Ist die sichere Momentaufnahmenwiederherstellung erwartet, hat aber Probleme?

    1. Untersuchen Sie das Verzeichnisdienst-Ereignisprotokoll.

        1. Treten bei der Momentaufnahmenwiederherstellung Fehler auf?

        2. Treten bei der AD-Replikation Fehler auf?

    2. Untersuchen Sie das Systemereignisprotokoll.

        1. Gibt es Kommunikationsfehler?

        2. Gibt es AD-Fehler?

2. Ist die sichere Momentaufnahmenwiederherstellung unerwartet?

    1. Untersuchen Sie Überwachungsprotokolle des Hypervisors, um herauszufinden, von wem und wodurch ein Rollback verursacht wurde.

    2. Kontaktieren Sie alle Administratoren des Hypervisors, und befragen Sie sie, wer ein Rollback des virtuellen Computers ohne Benachrichtigung durchgeführt hat.

3. Implementiert der Server einen USN-Rollbackschutz und wird nicht sicher wiederhergestellt?

    1. Untersuchen Sie die Verzeichnisdienst-Ereignisprotokolle auf einen nicht unterstützten Hypervisor oder nicht unterstützte Integrationsdienste.

    2. Untersuchen Sie das Betriebssystem und überprüfen Sie die Ausführung von Windows Server 2012.

### <a name="troubleshooting-specific-problems"></a><a name="BKMK_TshootSpecificSafeRestore"></a>Beheben von bestimmten Problemen

#### <a name="events"></a>Ereignisse
Alle Ereignisse bei der sicheren Momentaufnahmenwiederherstellung eines virtualisierten Domänencontroller werden in das Verzeichnisdienst-Ereignisprotokoll des wiederhergestellten virtuellen Domänencontrollercomputers geschrieben. Das Anwendungs-, Dateireplikationsdienst- und DFS-Replikationsereignisprotokoll enthält möglicherweise auch nützliche Problembehandlungsinformationen für fehlgeschlagene Wiederherstellungsvorgänge.

Im Folgenden sind die für die sichere Wiederherstellung in Windows Server 2012 spezifischen Ereignisse im Verzeichnisdienst-Ereignisprotokoll aufgeführt.

| Events | BESCHREIBUNG |
|--|--|
| **Ereignis-ID** | **2170** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Warnung |
| **Meldung** | Es wurde keine Änderung der Generations-ID erkannt.<p>Zwischengespeicherte Generations-ID (DS, alter Wert):%1<p>Aktuelle Generations-ID des virtuellen Computers (neuer Wert):%2<p>Die Änderung der Generations-ID erfolgt nach der Anwendung einer Momentaufnahme des virtuellen Computers, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. *<COMPUTERNAME>* erstellt eine neue Aufruf-ID, um den Domänen Controller wiederherzustellen. Virtualisierte Domänencontroller dürfen nicht mit einer Momentaufnahme des virtuellen Computers wiederhergestellt werden. Das unterstützte Verfahren zum Wiederherstellen oder Zurücksetzen des Inhalts einer Datenbank der Active Directory-Domänendienste ist das Wiederherstellen einer Systemstatussicherung, die mit einer für die Active Directory-Domänendienste geeigneten Sicherungsanwendung erstellt wurde. |
| **Hinweise und Lösung** | Dies ist ein Erfolgsereignis, wenn die Momentaufnahme erwartet war. Falls nicht, untersuchen Sie das Hyper-V-Worker-Ereignisprotokoll, oder wenden Sie sich an den Hypervisoradministrator. |

| Events | BESCHREIBUNG |
| -- |--|
|**Ereignis-ID**|**2174**|
|**Quelle**|Microsoft-Windows-ActiveDirectory_DomainService|
|**Severity**|Informational|
|**Meldung**|Der Domänencontroller ist weder ein Klon noch eine wiederhergestellte Momentaufnahme eines virtuellen Domänencontrollers.|
|**Hinweise und Lösung**|Erwartetes Ereignis beim Starten von physischen Domänencontrollern oder virtualisierten Domänencontrollern, die nicht von einer Momentaufnahme wiederhergestellt wurden|

| Events | BESCHREIBUNG |
| -- |--|
|**Ereignis-ID**|**2181**|
|**Quelle**|Microsoft-Windows-ActiveDirectory_DomainService|
|**Severity**|Informational|
|**Meldung**|Die Transaktion wurde aufgrund der Wiederherstellung des virtuellen Computers in einen früheren Zustand abgebrochen. Dies erfolgt nach der Anwendung einer Momentaufnahme des virtuellen Computers, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration.|
|**Hinweise und Lösung**|Beim Wiederherstellen einer Momentaufnahme erwartet. Transaktionen verfolgen die Änderung der VM-Generations-ID.|

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2185** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* der FRS-oder DFSR-Dienst zum Replizieren des Ordners "SYSVOL" wurde beendet.<p>Dienstname:%1<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Es muss eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Zu diesem Zweck wird der FRS oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" beendet und mit den geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. Beim Neustart des Diensts wird das Ereignis 2187 protokolliert. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Alle SYSVOL-Daten auf diesem Domänencontroller werden mit der Kopie eines Partnerdomänencontrollers ersetzt. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | 2186 |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* der FRS-oder DFSR-Dienst zum Replizieren des Ordners "SYSVOL" konnte nicht beendet werden.<p>Dienstname:%1<p>Fehlercode: %2<p>Fehlermeldung: %3<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Es muss eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Zu diesem Zweck wird der FRS oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" beendet und dann mit den geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. *<COMPUTERNAME>* der aktuell ausgelaufende Dienst konnte nicht beendet werden, und die nicht autoritative Wiederherstellung kann nicht durchgeführt werden. Führen Sie manuell eine nicht autoritative Wiederherstellung aus. |
| **Hinweise und Lösung** | Untersuchen Sie die System-, FRS- und DFSR-Ereignisprotokolle auf weitere Informationen. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2187** |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* der FRS-oder DFSR-Dienst zum Replizieren des Ordners "SYSVOL" wurde gestartet.<p>Dienstname:%1<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* erforderlich, um eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat zu initialisieren. Zu diesem Zweck wurde der FRS oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" beendet und mit den geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Alle SYSVOL-Daten auf diesem Domänencontroller werden mit der Kopie eines Partnerdomänencontrollers ersetzt. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2188** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* der FRS-oder DFSR-Dienst zum Replizieren des Ordners "SYSVOL" konnte nicht gestartet werden.<p>Dienstname:%1<p>Fehlercode: %2<p>Fehlermeldung: %3<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Es muss eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Zu diesem Zweck wird der FRS oder der DFS-Replikationsdienst zum Replizieren von SYSVOL beendet und mit geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. *<COMPUTERNAME>* der FRS-oder DFSR-Dienst zum Replizieren des SYSVOL-Ordners konnte nicht gestartet werden. die nicht autoritative Wiederherstellung kann nicht durchgeführt werden. Führen Sie manuell eine nicht autoritative Wiederherstellung aus, und starten Sie den Dienst neu. |
| **Hinweise und Lösung** | Untersuchen Sie die System-, FRS- und DFSR-Ereignisprotokolle auf weitere Informationen. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2189** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* Legen Sie die folgenden Registrierungs Werte zum Initialisieren des SYSVOL-Replikats während einer nicht autorisierenden Wiederherstellung fest:<p>Registrierungswert:%1<p>Registrierungswert: %2<p>Registrierungswertdaten: %3<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Es muss eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Zu diesem Zweck wird der FRS oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" beendet und mit den geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Alle SYSVOL-Daten auf diesem Domänencontroller werden mit der Kopie eines Partnerdomänencontrollers ersetzt. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2190** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Festlegen der folgenden Registrierungs Werte zum Initialisieren des SYSVOL-Replikats während einer nicht autorisierenden Wiederherstellung:<p>Registrierungswert:%1<p>Registrierungswert: %2<p>Registrierungswertdaten: %3<p>Fehlercode: %4<p>Fehlermeldungen: %5<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der die Domänencontrollerrolle hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Es muss eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Zu diesem Zweck wird der FRS oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" beendet und mit den geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. *<COMPUTERNAME>* die oben aufgeführten Registrierungs Werte konnten nicht festgelegt werden, und die nicht autoritative Wiederherstellung kann nicht ausgeführt werden. Führen Sie manuell eine nicht autoritative Wiederherstellung aus. |
| **Hinweise und Lösung** | Untersuchen Sie die Anwendungs- und Systemereignisprotokolle. Überprüfen Sie Drittanbieteranwendungen, die möglicherweise Registrierungsaktualisierungen blockieren. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2200** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Initialisiert die Replikation, um den Domänen Controller aktuell zu machen. Nach Abschluss der Replikation wird das Ereignis 2201 protokolliert. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Kennzeichnet den Beginn einer eingehenden Replikation. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2201** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* hat die Replikation beendet, um den Domänen Controller aktuell zu machen. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Kennzeichnet das Ende einer eingehenden Replikation. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2202** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Fehler bei der Replikation, um den Domänen Controller auf den neuesten Stand zu bringen. Er wird nach der nächsten periodischen Replikation aktualisiert. |
| **Hinweise und Lösung** | Untersuchen Sie das Verzeichnisdienst- und das Systemereignisprotokoll. Verwenden Sie repadmin.exe, um eine Replikation zu erzwingen, und notieren Sie alle Fehler. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2204** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* hat eine Änderung der Generations-ID der virtuellen Maschine erkannt. Die Änderung bedeutet, dass der virtuelle Domänencontroller in einem früheren Zustand wiederhergestellt wurde. *<COMPUTERNAME>* führt die folgenden Vorgänge aus, um den wiederhergestellten Domänen Controller vor möglichen Daten Abweichungen zu schützen und die Erstellung von Sicherheits Prinzipalen mit doppelten SIDs zu schützen:<p>Es wird eine neue Aufruf-ID erstellt.<p>Der aktuelle RID-Pool wird ungültig gemacht.<p>Bei der nächsten eingehenden Replikation erfolgt eine Überprüfung hinsichtlich des Besitzers der FSMO-Rollen. Wenn der Domänencontroller Besitzer einer FSMO-Rolle ist, ist diese Rolle bis zum Abschluss der Replikation nicht verfügbar.<p>Der SYSVOL-Replikationsdienst wird wiederhergestellt.<p>Die Replikation wird gestartet, um den wiederhergestellten Domänencontroller auf den aktuellen Stand zu bringen.<p>Es wird ein neuer RID-Pool angefordert. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Das erklärt die verschiedenen Vorgänge zum Zurücksetzen, die als Teil des sicheren Wiederherstellungsvorgangs vorkommen. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2205** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* der aktuelle RID-Pool wurde ungültig, nachdem der virtuelle Domänen Controller in den vorherigen Zustand zurückversetzt wurde. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Der lokale RID-Pool muss zerstört werden, da der Domänencontroller eine Zeitreise gemacht hat und der Pool möglicherweise bereits ausgestellt wurde. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2206** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | ERROR |
| **Meldung** | *<COMPUTERNAME>* der aktuelle RID-Pool konnte nicht ungültig gemacht werden, nachdem der virtuelle Domänen Controller in den vorherigen Zustand zurückversetzt wurde.<p>Zusätzliche Daten:<p>Fehlercode: %1<p>Fehlerwert: %2 |
| **Hinweise und Lösung** | Untersuchen Sie das Verzeichnisdienst- und das Systemereignisprotokoll. Vergewissern Sie sich über Dcdiag.exe /test:ridmanager, dass der RID-Master online ist und von diesem Server erreicht werden kann. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2207** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | ERROR |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Wiederherstellen, nachdem der virtuelle Domänen Controller in den vorherigen Zustand zurückversetzt wurde. Es war ein Neustart im Verzeichnisdienst-Wiederherstellungsmodus (DSRM) erforderlich. Weitere Informationen finden Sie in den vorhergehenden Ereignissen. |
| **Hinweise und Lösung** | Untersuchen Sie das Verzeichnisdienst- und das Systemereignisprotokoll. |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2208** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Informational |
| **Meldung** | *<COMPUTERNAME>* DFSR-Datenbanken zum Initialisieren des SYSVOL-Replikats während einer nicht autorisierenden Wiederherstellung gelöscht. |
| **Hinweise und Lösung** | Beim Wiederherstellen einer Momentaufnahme erwartet. Damit wird garantiert, dass DFSR SYSVOL nicht autoritativ von einem Partnerdomänencontroller synchronisiert. Beachten Sie, dass alle anderen von DFSR replizierten Ordner auf demselben Volume wie SYSVOL ebenfalls nicht autoritativ synchronisiert werden (es wird nicht empfohlen, dass Domänencontroller benutzerdefinierte DFSR-Sätze auf demselben Volume wie SYSVOL hosten). |

| Ereignis | Beschreibung |
|--|--|
| **Ereignis-ID** | **2209** |
| **Quelle** | Microsoft-Windows-ActiveDirectory_DomainService |
| **Severity** | Fehler |
| **Meldung** | *<COMPUTERNAME>* Fehler beim Löschen von DFSR-Datenbanken.<p>Zusätzliche Daten:<p>Fehlercode: %1<p>Fehlerwert: %2<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. *<COMPUTERNAME>* Es muss eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Für DFSR wird dies ausgeführt, indem der DFSR-Dienst beendet wird, die DFSR-Datenbanken gelöscht werden und der Dienst neu gestartet wird. Beim Neustart wird von DSFR so vorgegangen, dass die Datenbanken wiederhergestellt werden und die Erstsynchronisierung gestartet wird. |
| **Hinweise und Lösung** | Untersuchen Sie das DFSR-Ereignisprotokoll. |

#### <a name="error-messages"></a>Fehlermeldungen
Es gibt keine direkten interaktiven Fehler für eine fehlgeschlagene sichere Momentaufnahmenwiederherstellung eines virtualisierten Domänencontrollers. Alle Kloninformationen werden im den Verzeichnisdienst-Ereignisprotokollen protokolliert. Natürlich manifestieren sich alle kritischen Replikations- oder Serverankündigungsfehler als Symptome an anderer Stelle.

#### <a name="known-issues-and-support-scenarios"></a>Bekannte Probleme und Supportszenarien
Die [allgemeine Methode für die Problembehandlung der sicheren Wiederherstellung von Domänen Controllern](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) ist in der Regel für die Behandlung der meisten

|**Problem**|**Die Erstellung neuer Sicherheitsprinzipale auf einem kürzlich sicher wiederhergestellten Domänencontroller ist nicht möglich.**|
| -- |--|
|**Symptome**|Nach der Wiederherstellung einer Momentaufnahme schlagen Versuche, einen neuen Sicherheitsprinzipal (Benutzer, Computer, Gruppe) auf diesem Computer zu erstellen, mit dem folgenden Fehler fehl:<p>Fehler 0x2010<p>Der Verzeichnisdienst kann keinen relativen Bezeichner zuweisen.|
|**Lösung und Hinweise**|Dieses Problem wird durch die veraltete Kenntnis der FSMO-Rolle des RID-Masters auf dem wiederhergestellten Computer verursacht. Wenn die Rolle nach der Erstellung und späteren Wiederherstellung einer Momentaufnahme an diesen oder einen anderen Domänencontroller verschoben wurde, hat der wiederhergestellte Domänencontroller keine Kenntnis des RID-Masters, bis die Erstreplikation abgeschlossen ist.<p>Lassen Sie zum Beheben dieses Problems zu, dass die AD-Replikation an den wiederhergestellten Domänencontroller eingehend abgeschlossen wird. Falls das Problem bestehen bleibt, vergewissern Sie sich, dass alle Domänencontroller die korrekte Kenntnis darüber haben, auf dem Domänencontroller der RID-Master gehostet ist.|

|**Problem**|**Auf wiederhergestellten Domänencontrollern ist SYSVOL nicht freigegeben, und sie kündigen nicht an.**|
| -- |--|
|**Symptome**|Nach der Wiederherstellung einer Momentaufnahme kündigt mindestens ein Domänencontroller nicht an, SYSVOL ist nicht freigegeben, und er verfügt nicht über aktuelle SYSVOL-Inhalte.|
|**Lösung und Hinweise**|Auf den Upstreampartnern des Domänencontrollers ist kein funktionierendes SYSVOL-Replikat vorhanden, das ordnungsgemäß mit DFSR oder FRS repliziert. Dieses Problem steht nicht im Zusammenhang mit der sicheren Wiederherstellung, manifestiert sich aber wahrscheinlich als ein solches, da dem Kunden nicht bewusst war, dass das andere Replikationsproblem nicht wiederhergestellte Domänencontroller betrifft.|

### <a name="advanced-troubleshooting"></a>Erweiterte Fehlerbehebung
In diesem Modul erfahren Sie mehr über die erweiterte Fehlerbehandlung, indem *funktionierende* Protokolle als Beispiele verwendet und Erläuterungen zu den Ereignissen bereitgestellt werden. Wenn Sie verstehen, wie ein erfolgreicher Vorgang auf einem virtualisierten Quelldomänencontroller aussieht, werden Fehler in Ihrer Umgebung offensichtlich. Diese Protokolle werden nach ihrer Quelle dargestellt, aufsteigend in der Reihenfolge *erwarteter* Ereignisse in jedem Protokoll, die mit einem geklonten Domänencontroller zusammenhängen.

#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Wiederherstellen eines Domänencontrollers, der SYSVOL über DFSR repliziert

##### <a name="directory-services-event-log"></a>Verzeichnisdienst-Ereignisprotokoll
Das Verzeichnisdienst-Ereignisprotokoll enthält den größten Teil der Betriebsinformationen für die sichere Wiederherstellung. Der Hypervisor ändert die VM-Generations-ID. Der NTDS-Dienst bemerkt dies, macht den RID-Pool ungültig und ändert die Aufrufkennung. Die neue VM-Generations-ID wird festgelegt, und der Server repliziert AD-Daten eingehend. Der DFSR-Dienst wird angehalten, und seine Datenbank, die SYSVOL hostet, wird gelöscht, wodurch eine nicht autoritative eingehende Synchronisierung erzwungen wird. Der hohe USN-Grenzwert wird angepasst.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **2170** | ActiveDirectory_DomainService | Es wurde keine Änderung der Generations-ID erkannt.<p>Zwischengespeicherte Generations-ID in DS (alter Wert):<p>*<number>*<p>Aktuelle Generations-ID des virtuellen Computers (neuer Wert):<p>*<number>*<p>Die Änderung der Generations-ID erfolgt nach der Anwendung einer Momentaufnahme des virtuellen Computers, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. Von den Active Directory-Domänendiensten wird eine neue Aufruf-ID zur Wiederherstellung des Domänencontrollers erstellt. Virtualisierte Domänencontroller dürfen nicht mit einer Momentaufnahme des virtuellen Computers wiederhergestellt werden. Das unterstützte Verfahren zum Wiederherstellen oder Zurücksetzen des Inhalts einer Datenbank der Active Directory-Domänendienste ist das Wiederherstellen einer Systemstatussicherung, die mit einer für die Active Directory-Domänendienste geeigneten Sicherungsanwendung erstellt wurde. |
| **2181** | ActiveDirectory_DomainService | Die Transaktion wurde aufgrund der Wiederherstellung des virtuellen Computers in einen früheren Zustand abgebrochen. Dies erfolgt nach der Anwendung einer Momentaufnahme des virtuellen Computers, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. |
| **2204** | ActiveDirectory_DomainService | Von den Active Directory-Domänendiensten wurde eine Änderung der Generations-ID der virtuellen Maschine erkannt. Die Änderung bedeutet, dass der virtuelle Domänencontroller in einem früheren Zustand wiederhergestellt wurde. Folgende Vorgänge werden von den Active Directory-Domänendiensten ausgeführt, um den wiederhergestellten Domänencontroller vor möglichen Datenabweichungen zu schützen sowie zu verhindern, dass Sicherheitsprinzipale mit doppelten SIDs erstellt werden:<p>Es wird eine neue Aufruf-ID erstellt.<p>Der aktuelle RID-Pool wird ungültig gemacht.<p>Bei der nächsten eingehenden Replikation erfolgt eine Überprüfung hinsichtlich des Besitzers der FSMO-Rollen. Wenn der Domänencontroller Besitzer einer FSMO-Rolle ist, ist diese Rolle bis zum Abschluss der Replikation nicht verfügbar.<p>Der SYSVOL-Replikationsdienst wird wiederhergestellt.<p>Die Replikation wird gestartet, um den wiederhergestellten Domänencontroller auf den aktuellen Stand zu bringen.<p>Es wird ein neuer RID-Pool angefordert." |
| **2181** | ActiveDirectory_DomainService | Die Transaktion wurde aufgrund der Wiederherstellung des virtuellen Computers in einen früheren Zustand abgebrochen. Dies erfolgt nach der Anwendung einer Momentaufnahme des virtuellen Computers, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. |
| **1109** | ActiveDirectory_DomainService | Das invocationID-Attribut für diesen Domänencontroller wurde geändert. Die höchste Aktualisierungssequenznummer zum Zeitpunkt der Erstellung der Sicherung lautet wie folgt:<p>InvocationID-Attribut (alter Wert):<p>*<GUID>*<p>InvocationID-Attribut (neuer Wert):<p>*<GUID>*<p>Aktualisierungssequenznummer:<p>*<number>*<p>Die Aufruf-ID (invocationID-Attribut) wird in den folgenden Situationen geändert: nach Wiederherstellung des Verzeichnisservers von einem Sicherungsmedium, nach Konfiguration des Verzeichnisservers als Host einer beschreibbaren Anwendungsverzeichnispartition, bei fortgesetzter Ausführung des Verzeichnisservers, nachdem eine Momentaufnahme des virtuellen Computers angewendet wurde, nach einem Importvorgang für den virtuellen Computer oder nach einer Livemigration. Virtualisierte Domänencontroller dürfen nicht mit einer Momentaufnahme des virtuellen Computers wiederhergestellt werden. Das unterstützte Verfahren zum Wiederherstellen oder Zurücksetzen des Inhalts einer Datenbank der Active Directory-Domänendienste ist das Wiederherstellen einer Systemstatussicherung, die mit einer für die Active Directory-Domänendienste geeigneten Sicherungsanwendung erstellt wurde. |
| **2179** | ActiveDirectory_DomainService | Für das msDS-GenerationId-Attribut des Computerobjekts des Domänencontrollers wurde der folgende Parameter festgelegt:<p>Generations-ID-Attribut:<p>*<number>* |
| **2200** | ActiveDirectory_DomainService | Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. Von den Active Directory-Domänendiensten wird die Replikation initialisiert, um den Domänencontroller auf den neuesten Stand zu bringen. Nach Abschluss der Replikation wird das Ereignis 2201 protokolliert. |
| **2201** | ActiveDirectory_DomainService | Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. Die Replikation, mit der der Domänencontroller auf den neuesten Stand gebracht wurde, wurde abgeschlossen. |
| **2185** | ActiveDirectory_DomainService | Der Dateireplikationsdienst (FRS) oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" wurde beendet.<p>Dienstname:<p>DFSR<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. Daher muss von den Active Directory-Domänendiensten eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Zu diesem Zweck wird der FRS oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" beendet und mit den geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. Beim Neustart des Diensts wird das Ereignis 2187 protokolliert." |
| **2208** | ActiveDirectory_DomainService | Die DFSR-Datenbanken wurden von den Active Directory-Domänendiensten gelöscht, um im Verlauf einer nicht autoritativen Wiederherstellung das SYSVOL-Replikat zu initialisieren.<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. Daher muss von den Active Directory-Domänendiensten eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Für DFSR wird dies ausgeführt, indem der DFSR-Dienst beendet wird, die DFSR-Datenbanken gelöscht werden und der Dienst neu gestartet wird. Beim Neustarten von DFSR werden die Datenbanken neu erstellt und die erste Synchronisierung gestartet. " |
| **2187** | ActiveDirectory_DomainService | Der Dateireplikationsdienst (FRS) oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" wurde gestartet.<p>Dienstname:<p>DFSR<p>Von Active Directory wurde erkannt, dass der virtuelle Computer, der den Domänencontroller hostet, in einen früheren Zustand zurückversetzt wurde. Daher muss von den Active Directory-Domänendiensten eine nicht autoritative Wiederherstellung für das lokale SYSVOL-Replikat initialisiert werden. Zu diesem Zweck wurde der FRS oder der DFS-Replikationsdienst zum Replizieren des Ordners "SYSVOL" beendet und mit den geeigneten Registrierungsschlüsseln und -werten zum Auslösen der Wiederherstellung neu gestartet. " |
| **1587** | ActiveDirectory_DomainService | Dieser Verzeichnisdienst wurde wiederhergestellt, oder er wurde als Host für eine Anwendungsverzeichnispartition konfiguriert. Darum hat sich seine Replikationsidentität geändert. Ein Partner hat Replikationsänderungen angefordert und dabei die alte Identität verwendet. Die Anfangssequenznummer wurde angepasst.<p>Der Zielverzeichnisdienst, der der folgenden Objekt-GUID entspricht, hat Änderungen angefordert, die bei einer Aktualisierungssequenznummer beginnen sollen, die vor der Aktualisierungssequenznummer liegen, bei der der lokale Verzeichnisdienst vom Sicherungsmedium wiederhergestellt wurde.<p>Objekt-GUID:<p>*<GUID> (<FQDN of partner domain controller>)*<p>Aktualisierungssequenznummer zur Zeit der Wiederherstellung:<p>*<number>*<p>Als Folge dessen wurde der Aktualitätsvektor des Zielverzeichnisdienstes mit den folgenden Einstellungen konfiguriert.<p>Vorherige Datenbank-GUID:<p>*<GUID>*<p>Vorherige Objekt-Aktualisierungssequenznummer:<p>*<number>*<p>Vorherige Eigenschafts-Aktualisierungssequenznummer:<p>*<number>*<p>Neue Datenbank-GUID:<p>*<GUID>*<p>Neue Objekt-Aktualisierungssequenznummer:<p>*<number>*<p>Neue Eigenschafts-Aktualisierungssequenznummer:<p>*<number>* |

##### <a name="system-event-log"></a>Systemereignisprotokoll
Im Systemereignisprotokoll ist die Computerzeit vermerkt, zu der ein offline geschalteter virtueller Computer wieder online geschaltet und mit der Hostzeit synchronisiert wird. Der RID-Pool wird ungültig gemacht, und der DFSR- oder FRS-Dienst wird neu gestartet.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **1** | Kernel-General | Hat sich die Systemzeit in *geändert <now> ?* aus *<Momentaufnahme Zeit/Datum>* .<p>Änderungs Grund: eine Anwendung oder Systemkomponente hat die Zeit geändert. |
| **16654** | Verzeichnisdienst-SAM | Ein Pool aus Kontobezeichnern (RIDs) wurde ungültig gemacht. Dies kann in folgenden erwarteten Fällen vorkommen:<p>1. ein Domänen Controller wird aus der Sicherung wieder hergestellt.<p>2. ein Domänen Controller, der auf einer virtuellen Maschine ausgeführt wird, wird aus einer Momentaufnahme wieder hergestellt<p>3. ein Administrator hat den Pool manuell ungültig gemacht.<p>Weitere Informationen finden Sie unter <https://go.microsoft.com/fwlink/?LinkId=226247>. |
| **7036** | Dienststeuerungs-Manager | Der DFS-Replikationsdienst wurde beendet. |
| **7036** | Dienststeuerungs-Manager | Der DFS-Replikationsdienst wird jetzt ausgeführt. |

##### <a name="application-event-log"></a>Anwendungsereignisprotokoll
Im Anwendungsereignisprotokoll wird das Starten und Beenden der DFSR-Datenbank notiert.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **103** | ESENT | Dfsrs (1360) \\ \\ .\c: \System Volume information\dfsr\database <em> _ <GUID> </em> \dfsr.DB: das Datenbankmodul hat die Instanz (0) beendet.<p>Dirty Shutdown: 0<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.141, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000. |
| **102** | ESENT | Dfsrs (532) \\ \\ .\c: \System Volume information\dfsr\database <em> _ <GUID> </em> \dfsr.DB: die Datenbank-Engine (6.02.8189.0000) startet eine neue Instanz (0). |
| **105** | ESENT | Dfsrs (532) \\ \\ .\c: \System Volume information\dfsr\database <em> _ <GUID> </em> \dfsr.DB: das Datenbankmodul hat eine neue Instanz (0) gestartet. (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000. |
|  |  | Dfsrs (532) \\ \\ .\c: \System Volume information\dfsr\database <em> _<GUID> </em> \dfsr.DB: das Datenbankmodul hat eine neue Datenbank erstellt (1, \\ \\ .\c: \System Volume information\dfsr\database <em>_ <GUID> </em> \dfsr.DB). (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.062, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.015, [10] 0.000, [11] 0.000. |

##### <a name="dfs-replication-event-log"></a>Ereignisprotokoll der DFS-Replikation
Der DFSR-Dienst wird beendet, und die Datenbank, die SYSVOL enthält, wird gelöscht, wodurch eine eingehende nicht autoritative Synchronisierung erzwungen wird.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **1006** | DFSR | Der DFS-Replikationsdienst wird beendet. |
| **1008** | DFSR | Der DFS-Replikationsdienst wurde beendet. |
| **1002** | DFSR | Der DFS-Replikationsdienst wird gestartet. |
| **1004** | DFSR | Der DFS-Replikationsdienst wurde gestartet. |
| **1314** | DFSR | Der DFS-Replikationsdienst hat die Debugprotokolldateien erfolgreich konfiguriert.<p>Weitere Informationen:<p>Pfad der Debugprotokoll Datei: c:\windows\debug |
| **6102** | DFSR | Der DFS-Replikationsdienst hat den WMI-Anbieter erfolgreich registriert. |
| **1206** | DFSR | Der DFS-Replikation Dienst hat eine Verbindung mit dem Domänen Controller hergestellt *<domain controller FQDN>* , um auf Konfigurationsinformationen zuzugreifen |
| **1210** | DFSR | Der DFS-Replikationsdienst hat erfolgreich einen RPC-Listener für eingehende Replikationsanforderungen eingerichtet.<p>Weitere Informationen:<p>Port: 0 |
| **4614** | DFSR | Der DFS-Replikationsdienst hat SYSVOL im lokalen Pfad C:\Windows\SYSVOL\domain initialisiert und wartet darauf, die erste Replikation auszuführen. Der replizierte Ordner bleibt im ersten Synchronisierungsstatus, bis er mit seinem Partner repliziert wurde. Wenn der Server zu einem Domänencontroller heraufgestuft wurde, führt der Domänencontroller keine Ankündigung durch und dient als Domänencontroller, bis dieses Problem behoben wurde. Dies kann darauf zurückzuführen sein, dass der angegebene Partner sich selbst in einem ersten Synchronisierungsstatus befindet. Eine weitere mögliche Ursache ist, dass auf diesem Server oder beim Synchronisierungspartner Zugriffsverletzungen aufgetreten sind. Wenn dieses Ereignis bei der Migration von SYSVOL aus dem Dateireplikationsdienst (FRS) zur DFS-Replikation aufgetreten ist, werden die Änderungen nicht nach außen repliziert, bis dieses Problem behoben wurde. Dies kann dazu führen, dass der SYSVOL-Ordner auf diesem Server nicht mehr mit anderen Domänencontrollern synchron ist.<p>Weitere Informationen:<p>Name des replizierten Ordners: SYSVOL-Freigabe<p>ID des replizierten Ordners: *<GUID>*<p>Replikations Gruppen Name: Domänen System Volume<p>Replikations Gruppen-ID: *<GUID>*<p>Mitglieds-ID: *<GUID>*<p>Schreibgeschützt: 0 |
| **4604** | DFSR | Der DFS-Replikationsdienst hat den SYSVOL-replizierten Ordner im lokalen PfadC:\Windows\SYSVOL\domain erfolgreich initialisiert. Dieses Mitglied hat die erste Synchronisierung von SYSVOL mit Partner  dc1.corp.contoso.com abgeschlossen. Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie dann "net share" ein, um das Vorhandensein der SYSVOL-Freigabe zu prüfen.<p>Weitere Informationen:<p>Name des replizierten Ordners: SYSVOL-Freigabe<p>ID des replizierten Ordners: *<GUID>*<p>Replikations Gruppen Name: Domänen System Volume<p>Replikations Gruppen-ID: *<GUID>*<p>Mitglieds-ID: *<GUID>*<p>Synchronisierungs Partner: *<partner domain controller FQDN>* |

#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Wiederherstellen eines Domänencontrollers, der SYSVOL über FRS repliziert
In diesem Fall wird das Dateireplikations-Ereignisprotokoll anstelle des DFSR-Ereignisprotokolls verwendet. In das Anwendungsereignisprotokoll werden auch verschiedene FRS-relevante Ereignisse geschrieben. Davon abgesehen sind die Meldungen des Verzeichnisdienst- und Systemereignisprotokolls im Allgemeinen dieselben und werden in derselben zuvor beschriebenen Reihenfolge ausgegeben.

##### <a name="file-replication-service-event-log"></a>Ereignisprotokoll des Dateireplikationsdiensts
Der FRS-Dienst wird mit einem D2 BURFLAGS-Wert beendet und neu gestartet, um SYSVOL nicht autoritativ zu synchronisieren.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **13502** | NTFRS | Der Dateireplikationsdienst wird beendet. |
| **13503** | NTFRS | Der Dateireplikationsdienst wurde beendet. |
| **13501** | NTFRS | Der Dateireplikationsdienst wird gestartet. |
| **13512** | NTFRS | Der Dateireplikationsdienst hat einen aktivierten Datenträgerschreibungs-Cache in dem Laufwerk mit dem Verzeichnis c:\windows\ntfrs\jet auf dem Computer DC4 ermittelt. Der Dateireplikationsdienst kann eventuell nicht wiederhergestellt werden, wenn die Stromzufuhr des Laufwerks unterbrochen wird und wichtige Updates verloren gehen. |
| **13565** | NTFRS | Der Dateireplikationsdienst initialisiert den Systemdatenträger mit Daten eines anderen Domänencontrollers. Computer DC4 kann nicht zum Domänencontroller benannt werden, bis dieser Prozess beendet ist. Das Systemvolumen wird dann als SYSVOL geteilt.<p>Um die SYSVOL-Freigabe zu überprüfen, geben Sie an der Eingabeaufforderung Folgendes ein:<p>Netzwerkfreigabe<p>Wenn der Dateireplikationsdienst den Initialisierungsprozess beendet, wird die SYSVOL-Freigabe angezeigt.<p>Die Initialisierung des Systemdatenträgers kann einige Zeit in Anspruch nehmen. Der Zeitaufwand ist von der Datenmenge im Systemdatenträger, der Verfügbarkeit anderer Domänencontroller, und dem Replikationsintervall zwischen anderen Domänencontrollern abhängig." |
| **13520** | NTFRS | Der Datei Replikations Dienst hat die bereits vorhandenen Dateien in in *<path>* *<path>* \ NtFrs_PreExisting___See_EventLog verschoben.<p>Der Datei Replikations Dienst kann die Dateien in *<path>* \ NtFrs_PreExisting___See_EventLog jederzeit löschen. Dateien können vor dem Löschen gespeichert werden, indem Sie aus *<path>* \ NtFrs_PreExisting___See_EventLog kopiert werden. Das Kopieren der Dateien in *<path>* kann zu Namenskonflikten führen, wenn die Dateien auf einem anderen replizierenden Partner bereits vorhanden sind.<p>In einigen Fällen kopiert der Datei Replikations Dienst eine Datei von *<path>* \ NtFrs_PreExisting___See_EventLog in, *<path>* anstatt die Datei von einem anderen replizierenden Partner zu replizieren.<p>Der Speicherplatz kann jederzeit wieder hergestellt werden, indem die Dateien in *<path>* \ NtFrs_PreExisting___See_EventLog gelöscht werden. |
| **13553** | NTFRS | Der Dateireplikationsdienst hat diesen Computer dem folgenden Replikatsatz hinzugefügt:<p>"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<p>Informationen zu diesem Ereignis sind unten angezeigt:<p>Der DNS-Name des Computers ist " *<domain controller FQDN>* ".<p>Mitgliedsname des Replikat Satzes ist " *<domain controller name>* "<p>Stammpfad des Replikat Satzes ist " *<path>* "<p>Pfad des Replikat Stagingverzeichnisses ist " *<path>* "<p>Der Pfad des Replikat Arbeitsverzeichnisses ist " *<path>* ". |
| **13554** | NTFRS | Der Dateireplikationsdienst hat dem Replikatsatz die folgenden Verbindungen hinzugefügt:<p>"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<p>Eingehender von " *<partner domain controller FQDN>* "<p>Ausgehend zu " *<partner domain controller FQDN>* "<p>Weitere Informationen werden in den folgenden Ereignisprotokollmeldungen angezeigt. |
| **13516** | NTFRS | Der Dateireplikationsdienst verhindert nicht mehr die Heraufstufung des Computers DC4 zum Domänencontroller. Der Systemdatenträger wurde erfolgreich initialisiert. Der Netzwerkanmeldedienst wurde benachrichtigt, dass der Systemdatenträger jetzt als SYSVOL freigegeben werden kann.<p>Geben Sie "net share" ein, um die SYSVOL-Freigabe zu überprüfen. |

##### <a name="application-event-log"></a>Anwendungsereignisprotokoll
Die FRS-Datenbank wird beendet und startet und wird aufgrund des D2 BURFLAGS-Vorgangs analysiert.

| **Ereignis-ID** | **Quelle** | **Meldung** |
|--|--|--|
| **327** | ESENT | ntfrs (1424) Die Datenbank-Engine hat eine Datenbank getrennt (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.516, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.063, [12] 0.000.<p>Cache wiederbelebt: 0 |
| **103** | ESENT | ntfrs (1424) Die Datenbank-Engine hat die Instanz (0) beendet.<p>Dirty Shutdown: 0<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.047, [15] 0.000. |
| **102** | ESENT | ntfrs (3000) Die Datenbank-Engine (6.02.8189.0000) startet eine neue Instanz (0). |
| **105** | ESENT | ntfrs (3000) Die Datenbank-Engine hat eine neue Instanz (0) gestartet. (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.062, [10] 0.000, [11] 0.141. |
| **103** | ESENT | ntfrs (3000) Die Datenbank-Engine hat die Instanz (0) beendet.<p>Dirty Shutdown: 0<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.015, [14] 0.000, [15] 0.000. |
| **102** | ESENT | ntfrs (3000) Die Datenbank-Engine (6.02.8189.0000) startet eine neue Instanz (0). |
| **105** | ESENT | ntfrs (3000) Die Datenbank-Engine hat eine neue Instanz (0) gestartet. (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.000, [11] 0.109. |
| **325** | ESENT | ntfrs (3000) Die Datenbank-Engine hat eine neue Datenbank erstellt (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.016, [5] 0.000, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.016, [11] 0.000. |
| **103** | ESENT | ntfrs (3000) Die Datenbank-Engine hat die Instanz (0) beendet.<p>Dirty Shutdown: 0<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.078, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.125, [10] 0.016, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000. |
| **102** | ESENT | ntfrs (3000) Die Datenbank-Engine (6.02.8189.0000) startet eine neue Instanz (0). |
| **105** | ESENT | ntfrs (3000) Die Datenbank-Engine hat eine neue Instanz (0) gestartet. (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.016, [2] 0.000, [3] 0.000, [4] 0.094, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.032, [10] 0.000, [11] 0.000. |
| **326** | ESENT | ntfrs (3000) Die Datenbank-Engine hat eine Datenbank angehängt (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Zeit=0 Sekunden)<p>Interne Zeitsteuerungsabfolge: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.016, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<p>Gespeicherter Cache: 1 |
