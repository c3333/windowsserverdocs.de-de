---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: Unterstützung für Hyper-V-Replikate als virtualisierte Domänencontroller
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0a8d59da05f7dbf675114c96ceac5e755b06a66a
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071052"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Unterstützung für Hyper-V-Replikate als virtualisierte Domänencontroller

> Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In diesem Thema wird die Unterstützung für die Verwendung eines Hyper-V-Replikats für die Replikation eines virtuellen Computers (VM) dargestellt, auf dem ein Domänencontroller ausgeführt wird. Das Hyper-V-Replikat ist eine neue Funktion von Hyper-V, die mit Windows Server 2012 eingeführt wurde und einen integrierten Replikationsmechanismus auf VM-Ebene bereitstellt.

Das Hyper-V Replikat repliziert ausgewählte VMs asynchron von einem primären Hyper-V-Host zu einem Replikat-Hyper-V-Host über LAN- oder WAN-Verbindungen. Wenn die erste Replikation abgeschlossen ist, werden nachfolgende Änderungen in einem vom Administrator angegebenen Intervall repliziert.

Das Failover kann geplant oder ungeplant erfolgen. Ein geplantes Failover wird von einem Administrator auf dem primären virtuellen Computer initiiert, und alle nicht replizierten Änderungen werden an den virtuellen Replikatcomputer kopiert, um Datenverluste zu verhindern. Ein ungeplantes Failover wird auf dem virtuellen Replikatcomputer in Reaktion auf einen unerwarteten Ausfall des primären virtuellen Computerts initiiert. Datenverluste sind möglich, da es keine Gelegenheit gibt, Änderungen auf dem primären virtuellen Computer zu übertragen, die möglicherweise noch nicht repliziert wurden.

Weitere Informationen zum Hyper-v-Replikat finden Sie unter [Übersicht über Hyper-v-Replikate](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134172(v=ws.11)) und Bereitstellen von [Hyper](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134207(v=ws.11))-v

> [!NOTE]
> Das Hyper-V-Replikat kann nur unter Windows Server Hyper-V ausgeführt werden, nicht unter der Version, die auf Windows 8 ausgeführt wird.

## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>Windows Server 2012 oder neuere Domänen Controller erforderlich

Mit Windows Server 2012 Hyper-V wurde VM-GenerationID (vmgenid) eingeführt. VMGenID sorgt dafür, dass der Hypervisor mit dem Gastbetriebssysteme kommunizieren kann, wenn bedeutende Änderungen aufgetreten sind. Der Hypervisor kann beispielsweise einem virtualisierten Domänencontroller mitteilen, dass eine Wiederherstellung von einer Momentaufnahme durchgeführt wurde (Hyper-V-Technologie zur Momentaufnahmenwiederherstellung, keine Sicherungswiederherstellung). AD DS in Windows Server 2012 und höher ist von der vmgenid-VM-Technologie abhängig und verwendet diese, um zu erkennen, wann Hypervisor-Vorgänge durchgeführt werden, wie z. b. die Momentaufnahme Wiederherstellung, sodass Sie sich besser schützen kann.

> [!NOTE]
> Nur AD DS auf Windows Server 2012 DCS oder neueren stellen diese Sicherheitsmaßnahmen bereit, die sich aus vmgenid; ergeben. DCS, die alle früheren Versionen von Windows Server ausführen, unterliegen Problemen, wie z. b. dem Rollback von Windows Server, die auftreten können, wenn ein virtualisierter Domänen Controller mit einem nicht unterstützten Mechanismus wieder hergestellt wird, z. b. Weitere Informationen zu diesen Sicherheitsvorkehrungen und deren Auslösung finden Sie unter [Architektur virtualisierter Domänen Controller](./virtualized-domain-controller-architecture.md).

Wenn ein Hyper-V-Replikat Failover (geplant oder ungeplant) erfolgt, erkennt der virtualisierte Domänen Controller eine vmgenid-zurück setzung, wodurch die oben genannten Sicherheitsfeatures ausgelöst werden. Active Directory-Vorgänge werden dann normal fortgesetzt. Der virtuelle Replikatcomputer wird anstelle des primären virtuellen Computers ausgeführt.

> [!NOTE]
> Aufgrund der Tatsache, dass jetzt zwei Instanzen derselben Domänencontrolleridentität vorhanden sind, können potenziell sowohl die primäre als auch die replizierte Instanz ausgeführt werden. Zwar bietet das Hyper-V-Replikat Kontrollmechanismen, um sicherzustellen, dass der primäre virtuelle Computer und der virtuelle Replikatcomputer nicht gleichzeitig ausgeführt werden, eine gleichzeitige Ausführung ist aber möglich, wenn die Verknüpfung zwischen ihnen nach der Replikation des virtuellen Computers fehlschlägt. Für diese sehr unwahrscheinliche Situation verfügen virtualisierte Domänencontroller, auf denen Windows Server 2012 ausgeführt wird, im Gegensatz zu virtualisierten Domänencontrollern mit früheren Versionen von Windows Server über Sicherheitsmaßnahmen für den Schutz von AD DS.

Wenn Sie das Hyper-v-Replikat verwenden, müssen Sie die bewährten Methoden für die [Ausführung virtueller Domänen Controller unter Hyper-v](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))befolgen. In diesem Thema werden z. B. Empfehlungen für das Speichern von Active Directory-Dateien auf virtuellen SCSI-Datenträgern erläutert, die eine höhere Garantie für die Beständigkeit der Daten bieten.

## <a name="supported-and-unsupported-scenarios"></a>Unterstützte und nicht unterstützte Szenarien

Nur virtuelle Computer, auf denen Windows Server 2012 oder höher ausgeführt wird, werden für ungeplante Failover und Test Failover unterstützt. Auch für das geplante Failover wird Windows Server 2012 oder höher für den virtualisierten Domänen Controller empfohlen, um Risiken zu minimieren, wenn ein Administrator versehentlich sowohl den primären als auch den replizierten virtuellen Computer gleichzeitig startet.

Virtuelle Computer mit früheren Versionen von Windows Server werden für geplante Failover unterstützt, für ungeplante Failover aufgrund eines potenziellen USN-Rollback dagegen nicht. Weitere Informationen über das Rollback für die Wiederverwendung finden Sie unter " [US-v" und "Rollback](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))".

> [!NOTE]
> Für die Domäne oder Gesamtstruktur gibt es keine Funktionsebenenanforderungen, sondern nur Betriebssystemanforderungen für die Domänencontroller, die als virtuelle Computer ausgeführt und mithilfe des Hyper-V-Replikats repliziert werden. Die virtuellen Computer können in einer Gesamtstruktur bereitgestellt werden, die andere physische oder virtuelle Domänencontroller enthält, auf denen frühere Versionen von Windows Server ausgeführt und die möglicherweise ebenfalls mithilfe des Hyper-V-Replikats repliziert werden.

Diese Aussage zur Unterstützung basiert auf Tests, die in einer Gesamtstruktur mit einer einzelnen Domäne durchgeführt wurden, obwohl auch Gesamtstrukturkonfigurationen mit mehreren Domänen unterstützt werden. Für diese Tests sind die virtualisierten Domänencontroller DC1 und DC2 Active Directory-Replikationspartner am selben Standort, die auf einem Server mit Hyper-V unter Windows Server 2012 gehostet werden. Auf dem VM-Gast, auf dem DC2 ausgeführt wird, wurde das Hyper-V-Replikat aktiviert. Der Replikatserver wird in einem anderen, geografisch entfernten Rechenzentrum gehostet. Für eine einfachere Erklärung der unten aufgeführten Testfallprozesse wird der virtuelle Computer, der auf dem Replikatserver ausgeführt wird, als DC2-Rec bezeichnet (obwohl er in der Praxis denselben Namen beibehält wie der ursprüngliche virtuelle Computer).

### <a name="windows-server-2012"></a>Windows Server 2012

In der folgenden Tabelle werden die Unterstützung für virtualisierte Domänencontroller mit Windows Server 2012 und Testfälle dargestellt.

| Geplantes Failover | Ungeplantes Failover |
|--|--|
| Unterstützt | Unterstützt |
| Testfall:<p>-DC1 und DC2 führen Windows Server 2012 aus.<p>-DC2 wird heruntergefahren, und auf DC2-REC wird ein Failover durchgeführt. Das Failover kann entweder geplant oder ungeplant sein.<p>-Nachdem DC2-Rec gestartet wurde, wird überprüft, ob der Wert von vmgenid in der Datenbank mit dem Wert des vom Hyper-V-Replikat Server gespeicherten virtuellen Computer Treibers übereinstimmt.<p>-Infolgedessen löst DC2-Rec virtualisierungssicherheitsmaßnahmen aus. Das heißt, dass die invocationID zurückgesetzt, der RID-Pool verworfen und eine anfängliche Synchronisierungs Anforderung festgelegt wird, bevor eine Betriebs Master Rolle angenommen wird. Weitere Information zur Anforderung für die anfängliche Synchronisierung finden Sie unter .<p>-DC2-Rec speichert dann den neuen Wert von vmgenid in der Datenbank und führt einen Commit für alle nachfolgenden Aktualisierungen im Kontext der neuen invocationID aus.<p>-Aufgrund der zurück Setzung invocationID wird DC1 bei allen AD-Änderungen, die durch DC2-Rec eingeführt wurden, auch dann, wenn ein Rollback erfolgt ist, zusammengeführt. Dies bedeutet, dass alle AD-Aktualisierungen, die auf DC2-Rec nach dem Failover durchgeführt wurden, sicher aufeinander | Für ein geplantes Failover ist der Testfall gleich, mit den folgenden Ausnahmen:<p>-Alle AD-Updates, die auf DC2 empfangen, aber noch nicht von AD an einen Replikations Partner repliziert wurden, bevor das failoverereignis verloren geht.<p>-AD-Updates, die nach dem Zeitpunkt des von AD an DC1 replizierten Wiederherstellungs Punkts auf DC2 empfangen werden, werden von DC1 zurück auf DC2-REC repliziert. |

### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 und frühere Versionen

In der folgenden Tabelle werden die Unterstützung für virtualisierte Domänencontroller mit Windows Server 2008 R2 und früheren Versionen dargestellt.

| Geplantes Failover | Ungeplantes Failover |
|--|--|
| Unterstützt, aber nicht empfohlen, da Domänencontroller mit diesen Versionen von Windows Server keine Unterstützung für VMGenID bieten oder damit verbundene Virtualisierungssicherheitsmaßnahmen verwenden. Damit entsteht das Risiko von einem USN-Rollback. Weitere Informationen finden Sie unter " [US-v" und "Wiederverwendungs-Rollback](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))". | Nicht unterstützt<p>**Hinweis:** Ein ungeplantes Failover wird unterstützt, wenn ein Rollback für die Wiederverwendung kein Risiko ist, z. b. ein einzelner Domänen Controller in der Gesamtstruktur (eine Konfiguration, die nicht empfohlen wird). |
| Testfall:<p>-DC1 und DC2 führen Windows Server 2008 R2 aus.<p>-DC2 wird heruntergefahren, und auf DC2-REC wird ein Geplantes Failover ausgeführt. Alle Daten auf DC2 werden auf DC2-Rec repliziert, bevor das Herunterfahren beendet ist.<p>-Nachdem DC2-Rec gestartet wurde, wird die Replikation mit DC1 unter Verwendung derselben invocationID wie DC2 fortgesetzt. | Nicht zutreffend |
