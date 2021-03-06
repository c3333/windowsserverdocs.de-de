---
title: Konfigurieren der Reihenfolge von Netzwerkschnittstellen
description: Dieses Thema ist Teil des Handbuch zur Leistungsoptimierung des Netzwerk Subsystems für Windows Server 2016.
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: 8e2188fd0a1cd1d07212c4eed216570eb8023a72
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993696"
---
# <a name="configure-the-order-of-network-interfaces"></a>Konfigurieren der Reihenfolge von Netzwerkschnittstellen

>Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2016

In Windows Server 2016 und Windows 10 können Sie die Schnittstellen Metrik verwenden, um die Reihenfolge der Netzwerkschnittstellen zu konfigurieren.

Dies unterscheidet sich von früheren Versionen von Windows und Windows Server, mit denen Sie die Bindungs Reihenfolge von Netzwerkadaptern konfigurieren konnten, indem Sie entweder die Benutzeroberfläche oder die Befehle **inetcfgcomponentbinding:: muvebefore** und **inetcfgcomponentbinding:: muveafter**verwenden. Diese beiden Methoden zum Anordnen von Netzwerkschnittstellen sind in Windows Server 2016 und Windows 10 nicht verfügbar.

Stattdessen können Sie die neue Methode zum Festlegen der aufgezählten Reihenfolge von Netzwerkadaptern verwenden, indem Sie die Schnittstellen Metrik der einzelnen Adapter konfigurieren. Sie können die Schnittstellenmetrik mit dem Windows PowerShell [-Befehl Set-nettipinterface](/powershell/module/nettcpip/set-netipinterface) konfigurieren.

Wenn Netzwerkverkehrs Routen ausgewählt werden und Sie den **InterfaceMetric** -Parameter des **Set-nettipinterface** -Befehls konfiguriert haben, ist die allgemeine Metrik, die zur Bestimmung der Schnittstellen Einstellung verwendet wird, die Summe der Routen Metrik und der Schnittstellen Metrik. In der Regel gibt die Schnittstellen Metrik eine bestimmte Schnittstelle an, z. b. die Verwendung von verdrahteten, wenn Kabel-und Drahtlos Verbindungen verfügbar sind.

Das folgende Windows PowerShell-Befehlsbeispiel zeigt die Verwendung dieses Parameters.

```powershell
Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15
```

Die Reihenfolge, in der Adapter in einer Liste angezeigt werden, wird durch die IPv4-oder IPv6-Schnittstellen Metrik bestimmt.  Weitere Informationen finden Sie unter [getadaptersadressen-Funktion](/windows/win32/api/iphlpapi/nf-iphlpapi-getadaptersaddresses?f=255&MSPPError=-2147217396).

Links zu allen Themen in diesem Handbuch finden Sie unter [Network Subsystem Performance Tuning](net-sub-performance-top.md).