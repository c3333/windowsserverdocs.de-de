---
title: Leistungstools für Netzwerkauslastungen
description: Dieses Thema ist Teil des Handbuch zur Leistungsoptimierung des Netzwerk Subsystems für Windows Server 2016.
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 07/16/2018
ms.openlocfilehash: 4344357db92a65725a7bcdc749966d3889d20695
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995092"
---
# <a name="performance-tools-for-network-workloads"></a>Leistungstools für Netzwerkauslastungen

>Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2016

In diesem Thema finden Sie Informationen zu Leistungs Tools.

Dieses Thema enthält Abschnitte zum Tool "Client-zu-Server-Datenverkehr", "TCP/IP-Fenstergröße" und "Microsoft Server Performance Advisor".

##  <a name="client-to-server-traffic-tool"></a><a name="bkmk_tuning"></a>Client-zu-Server-Traffic Tool

Das Tool Client to Server Traffic \( ctstraffic \) bietet Ihnen die Möglichkeit, Netzwerk Datenverkehr zu erstellen und zu überprüfen.

Weitere Informationen und zum Herunterladen des Tools finden Sie unter [ctstraffic (Client-zu-Server-Datenverkehr)](https://github.com/Microsoft/ctsTraffic).

##  <a name="tcpip-window-size"></a><a name="bkmk_size"></a>Größe des TCP/IP-Fensters

Bei 1-GB-Adaptern sollten die in der vorherigen Tabelle gezeigten Einstellungen einen guten Durchsatz bereitstellen, da ntttcp die standardmäßige TCP-Fenstergröße durch eine bestimmte logische Prozessor Option \( SO_RCVBUF für die Verbindung auf 64 K festlegt \) . Dies ermöglicht eine gute Leistung in einem Netzwerk mit geringer Latenz.

Im Gegensatz dazu ergibt bei Netzwerken mit hoher Latenz oder bei 10-GB-Adaptern der Standardwert für die TCP-Fenstergröße für ntttcp eine geringere Leistung als die optimale Leistung. In beiden Fällen müssen Sie die TCP-Fenstergröße anpassen, um das größere Bandbreiten Verzögerungs Produkt zuzulassen.

Sie können die TCP-Fenstergröße mit der Option **-RB** statisch auf einen großen Wert festlegen. Mit dieser Option wird die automatische Optimierung von TCP-Fenstern deaktiviert, und es wird empfohlen, Sie nur zu verwenden, wenn der Benutzer die resultierende Änderung im TCP/IP-Verhalten vollständig versteht. Standardmäßig ist die TCP-Fenstergröße auf einen ausreichenden Wert festgelegt und passt sich nur bei hoher Last oder über Verbindungen mit hoher Latenz an.

##  <a name="microsoft-server-performance-advisor"></a><a name="bkmk_advisor"></a>Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \( Spa \) unterstützt IT-Administratoren beim Sammeln von Metriken, um potenzielle Leistungsprobleme in einer Windows Server 2016-, Windows Server 2012 R2-, Windows Server 2012-, Windows Server 2008 R2-oder Windows Server 2008-Bereitstellung zu identifizieren, zu vergleichen und zu diagnostizieren.

Spa generiert umfassende Diagnose Berichte und Diagramme und bietet Empfehlungen, mit denen Sie Probleme schnell analysieren und Korrekturmaßnahmen entwickeln können.

 Weitere Informationen und zum Herunterladen des Ratgebers finden Sie unter [Microsoft Server Performance Advisor](/previous-versions/dn481522(v=vs.85)) im Windows Hardware dev Center.

Links zu allen Themen in diesem Handbuch finden Sie unter [Network Subsystem Performance Tuning](net-sub-performance-top.md).