---
title: Hardwareanforderungen für direkte Speicherplätze
description: Mindesthardwareanforderungen zum Testen von „Direkte Speicherplätze“.
ms.author: eldenc
manager: eldenc
ms.topic: article
author: eldenchristensen
ms.date: 09/24/2020
ms.localizationpriority: medium
ms.openlocfilehash: fcbd7a955efda11543010d3e4705bf52b7d9bdea
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517496"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Hardwareanforderungen für „Direkte Speicherplätze“

> Gilt für: Windows Server 2019, Windows Server 2016

In diesem Thema werden die Mindesthardwareanforderungen für direkte Speicherplätze unter Windows Server beschrieben. Informationen zu den Hardwareanforderungen Azure Stack HCI, dem Betriebssystem, das für hyperkonvergierte bereit Stellungen mit einer Verbindung mit der Cloud entwickelt wurde, finden Sie unter vor der Bereitstellung von [Azure Stack HCI: Ermitteln der Hardwareanforderungen](/azure-stack/hci/deploy/before-you-start#determine-hardware-requirements).

In der Produktionsumgebung empfiehlt Microsoft, eine überprüfte Hardware-/Softwarelösung unserer Partner zu erwerben, die Bereitstellungs Tools und-Verfahren umfasst. Diese Lösungen wurden anhand unserer Referenzarchitektur entworfen, zusammengestellt und validiert und garantieren Kompatibilität und Zuverlässigkeit, sodass Sie sofort loslegen können. Informationen zu Windows Server 2019-Lösungen finden Sie auf der [Azure Stack HCI Solutions-Website](https://azure.microsoft.com/overview/azure-stack/hci). Informationen zu Windows Server 2016-Lösungen finden Sie unter [Windows Server-Software definiert](https://microsoft.com/wssd).

   > [!TIP]
   > Möchten Sie direkte Speicherplätze auswerten, aber keine Hardware? Verwenden Sie Hyper-V oder Azure Virtual Machines, wie in [Verwenden von direkte Speicherplätze in virtuellen Gastcomputer Clustern](storage-spaces-direct-in-vm.md)beschrieben.

## <a name="base-requirements"></a>Basisanforderungen

Systeme, Komponenten, Geräte und Treiber müssen für das Betriebssystem zertifiziert sein, das Sie im [Windows Server-Katalog](https://www.windowsservercatalog.com)verwenden. Außerdem wird empfohlen, dass Server, Laufwerke, Hostbus Adapter und Netzwerkadapter über das **Software-Defined Data Center (SDDC) Standard** -und/oder **Software-Defined Data Center (SDDC) Premium** -Standard Qualifizierungen (AQS) verfügen, wie unten dargestellt. Es sind mehr als 1.000 Komponenten mit dem SDDC AQS vorhanden.

![Screenshot des Windows Server-Katalogs mit einem System, das die Software-Defined Data Center (SDDC) Premium-Zertifizierung enthält](media/hardware-requirements/sddc-aqs.png)

Der vollständig konfigurierte Cluster (Server, Netzwerk und Speicher) muss alle [Cluster Validierungstests](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) gemäß dem Assistenten in Failovercluster-Manager oder mit dem `Test-Cluster` [Cmdlet](/powershell/module/failoverclusters/test-cluster) in PowerShell bestehen.

Außerdem gelten die folgenden Anforderungen:

## <a name="servers"></a>Server

- Mindestens zwei, maximal 16 Server
- Es wird empfohlen, dass alle Server den gleichen Hersteller und das gleiche Modell aufweisen

## <a name="cpu"></a>CPU

- Intel Nehalem oder höher kompatibler Prozessor; noch
- Mit AMD epyc oder höher kompatibler Prozessor

## <a name="memory"></a>Arbeitsspeicher

- Arbeitsspeicher für Windows Server, VMS und andere apps oder Arbeits Auslastungen; ZZ
- 4 GB RAM pro Terabyte (TB) der Cache Laufwerks Kapazität auf jedem Server, für direkte Speicherplätze-Metadaten

## <a name="boot"></a>Start

- Alle Start Geräte, die von Windows Server unterstützt werden und [nun SATADOM enthalten](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1-Spiegel ist **nicht** erforderlich, wird jedoch für den Start unterstützt.
- Empfohlen: Mindestgröße von 200 GB

## <a name="networking"></a>Netzwerk

Direkte Speicherplätze erfordert eine zuverlässige Netzwerkverbindung mit hoher Bandbreite und geringer Latenz zwischen den einzelnen Knoten.

Minimale Verbindungs Verbindung für kleinen 2-3-Knoten
- Netzwerkschnittstellenkarte (NIC) mit 10 Gbit/s oder schneller
- Zwei oder mehr Netzwerkverbindungen von jedem Knoten, der für Redundanz und Leistung empfohlen wird

Empfohlene Interverbindung für hohe Leistung, Skalierbarkeit oder bereit Stellungen von 4 und höher
- NICs, die RDMA (Remote Direct Memory Access), IWarp (empfohlen) oder ROCE sind
- Zwei oder mehr Netzwerkverbindungen von jedem Knoten, der für Redundanz und Leistung empfohlen wird
- Netzwerkkarte mit 25 Gbit/s oder schneller

Umgeschaltete oder switchlose Knoten Verbindungen
- Umgestellt: Netzwerk Switches müssen ordnungsgemäß für die Handhabung der Bandbreite und des Netzwerk Typs konfiguriert werden.  Wenn Sie RDMA verwenden, das das ROCE-Protokoll implementiert, ist die Netzwerkgeräte-und Switchkonfiguration noch wichtiger.
- Switchless: Knoten können mithilfe direkter Verbindungen miteinander verbunden werden, sodass die Verwendung eines Schalters vermieden wird.  Es ist erforderlich, dass jeder Knoten über eine direkte Verbindung mit allen anderen Knoten des Clusters verfügt.


## <a name="drives"></a>Laufwerke

Direkte Speicherplätze funktioniert mit direkt angeschlossenen SATA-, SAS-, nvme-oder persistenten Speicher Laufwerken (pMem), die physisch an nur einen Server angeschlossen sind. Weitere Informationen zum Auswählen von Laufwerken finden Sie in den Themen [Auswählen von Laufwerken](choosing-drives.md) und verstehen und Bereitstellen von [persistenten Speicher](deploy-pmem.md)

- SATA-, SAS-, persistente Speicher-und nvme-Laufwerke (M. 2, U. 2 und Add-in-Card) werden unterstützt.
- 512 n, 512 e und 4K Native Laufwerke werden unterstützt.
- Solid-State-Laufwerke müssen den [Schutz von Energieverlusten](https://techcommunity.microsoft.com/t5/storage-at-microsoft/don-t-do-it-consumer-grade-solid-state-drives-ssd-in-storage/ba-p/425914) bereitstellen.
- Gleiche Anzahl und Arten von Laufwerken auf jedem Server – siehe [Überlegungen zur Laufwerk Symmetrie](drive-symmetry-considerations.md)
- Cache Geräte müssen 32 GB oder größer sein.
- Persistente Speichergeräte werden im Blockspeicher Modus verwendet.
- Wenn Sie persistente Speichergeräte als Cache Geräte verwenden, müssen Sie nvme-oder SSD-Kapazitäts Geräte verwenden (HDDs können nicht verwendet werden).
- Der nvme-Treiber ist der von Microsoft bereitgestellte, der in Windows enthalten ist (stornvme.sys).
- Empfohlen: die Anzahl der Kapazitäts Laufwerke ist ein gesamtes Vielfaches der Anzahl von Cache Laufwerken.
- Empfohlen: Cache Laufwerke sollten hohe Schreib Ausdauer aufweisen: mindestens 3 Laufwerke pro Tag (dwpd) oder mindestens 4 Terabyte (TBW) pro Tag – Siehe Grundlegendes [zu Laufwerks Schreibvorgängen pro Tag (dwpd), Terabyte (TBW) und die empfohlene Mindestanzahl für direkte Speicherplätze](https://techcommunity.microsoft.com/t5/storage-at-microsoft/understanding-ssd-endurance-drive-writes-per-day-dwpd-terabytes/ba-p/426024)

So können Laufwerke für direkte Speicherplätze verbunden werden:

- Direkt angeschlossene SATA-Laufwerke
- Direkt angeschlossene nvme-Laufwerke
- SAS-Hostbus Adapter (HBA) mit SAS-Laufwerken
- SAS-Hostbus Adapter (HBA) mit SATA-Laufwerken
- **nicht unterstützt:** RAID-Controller-Karten oder SAN-Speicher (Fibre Channel, iSCSI, FCoE). Hostbusadapterkarten müssen einen einfachen Pass-Through-Modus implementieren.

![Diagramm mit unterstützten Laufwerk-Verbindungen, bei denen keine RAID-Karten unterstützt werden](media/hardware-requirements/drive-interconnect-support-1.png)

Laufwerke können intern im Server installiert sein oder sich in einem externen Gehäuse befinden, das nur mit einem Server verbunden ist. SES (SCSI Enclosure Services) ist für die Zuordnung und Identifikation von Slots erforderlich. Jedes externe Gehäuse muss einen eindeutigen Bezeichner (eindeutige ID) besitzen.

- Interne Laufwerke für den Server
- Laufwerke in einem externen Gehäuse ("JBOD"), die mit einem Server verbunden sind
- **nicht unterstützt:** Freigegebene SAS-Gehäuse, die mit mehreren Servern oder einer beliebigen Form von Multipfad-e/a (MPIO) verbunden sind

![Diagramm, das zeigt, wie interne und externe Laufwerke, die direkt mit einem Server verbunden sind, unterstützt werden, aber freigegebene SAS nicht](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Mindestanzahl von Laufwerken (durch das Start Laufwerk ausgeschlossen)

- Wenn Laufwerke als Cache verwendet werden, sind mindestens zwei pro Server erforderlich.
- Es müssen mindestens vier Kapazitäts Laufwerke (ohne Cache) pro Server vorhanden sein.

| Vorhandene Laufwerktypen   | Erforderliche Mindestanzahl |
|-----------------------|-------------------------|
| Alle persistenten Speicher (Gleiches Modell) | 4 persistenter Speicher |
| Alle NVMe (gleiches Modell) | 4x NVMe                  |
| Alle SSD (gleiches Modell)  | 4x SSD                   |
| Persistenter Arbeitsspeicher + nvme oder SSD | 2 persistenter Arbeitsspeicher + 4 nvme oder SSD |
| NVMe und SSD            | 2x NVMe und 4x SSD          |
| NVMe und HDD            | 2x NVMe und 4x HDD          |
| SSD und HDD             | 2x SSD und 4x HDD           |
| NVMe und SSD und HDD      | 2x NVMe und vier andere       |

   >[!NOTE]
   > Diese Tabelle enthält die Mindestanforderungen für Hardware Bereitstellungen. Wenn Sie mit virtuellen Computern und virtualisiertem Speicher wie z. b. in Microsoft Azure bereitstellen, finden Sie weitere Informationen unter [Verwenden von direkte Speicherplätze in Clustern virtueller Gastcomputer](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Maximale Kapazität

| Höchstwerte                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| Rohkapazität pro Server | 400 TB               | 100 TB               |
| Pool Kapazität           | 4 PB (4.000 TB)      | 1 PB                 |
