---
title: Neues in Sdn für Windows Server
description: Dieses Thema enthält Informationen zu neuen Software-Defined Networking-Features für Windows Server 1709.
manager: grcusanz
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: 49f6573d0fc46fe9558b6c3af49c20ada5216cce
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156452"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Neues in SDN für Windows Server 2019

>Gilt für: Windows Server (Halbjährlicher Kanal)


|                         **Feature**                          |                                                                                                                                                                                         **Beschreibung**                                                                                                                                                                                         | **Neu/Aktualisiert** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Verschlüsselte Netzwerke](vnet-encryption/sdn-vnet-encryption.md) | Die Verschlüsselung virtueller Netzwerke ermöglicht die Verschlüsselung des Datenverkehrs von virtuellen Netzwerken zwischen virtuellen Computern, die in Subnetzen, die als "Verschlüsselung aktiviert" gekennzeichnet sind, miteinander kommunizieren. Sie nutzt auch Datagram Transport Layer Security (DTLS) im virtuellen Subnetz, um Pakete zu verschlüsseln. DTLS schützt vor Abhörversuchen, Manipulation und Fälschung durch jeden, der Zugriff auf das physische Netzwerk hat. |       Neu       |
|    [Firewallüberwachung](security/sdn-firewall-auditing.md)    |                                                                                            Die firewallüberwachung ist eine neue Funktion für die Sdn-Firewall in Windows Server 2019. Wenn Sie die Sdn-Firewall aktivieren, werden alle Flows aufgezeichnet, die von Sdn-Firewallregeln (ACLs) mit aktivierter Protokollierung verarbeitet werden.                                                                                            |       Neu       |
| [Peering von virtuellen Netzwerken](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      Das Peering virtueller Netzwerke ermöglicht das nahtlose Verbinden von zwei virtuellen Netzwerken. Nach dem Peer werden die virtuellen Netzwerke zu konnektivitätszwecken als eins angezeigt.                                                                                                                      |       Neu       |
|           [Ausgangs Messung](manage/sdn-egress.md)            |                  Dieses neue Feature in Windows Server 2019 ermöglicht Sdn das anbieten von Nutzungs Zählern für ausgehende Datenübertragungen. Wenn dieses Feature hinzugefügt wurde, speichert der Netzwerk Controller eine Zulassungs pro Virtual Network aller IP-Adressbereiche, die innerhalb des SDN verwendet werden, und berücksichtigt alle Pakete, die für ein Ziel, das nicht in einem dieser Bereiche enthalten ist, für ausgehende Datenübertragungen in Rechnung gestellt werden.                   |       Neu       |

---



