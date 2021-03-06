---
title: Einführung in MultiPoint Services
description: Bietet eine Übersicht über Multipoint Services, eine Möglichkeit, um mehreren Benutzern die Freigabe eines Systems zu ermöglichen.
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 663b3d4afade9b4fb459f34120d1e6b18984285a
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997429"
---
# <a name="introducing-multipoint-services"></a>Einführung in MultiPoint Services
Die Multipoint Services-Rolle in Windows Server 2016 ermöglicht es mehreren Benutzern, die jeweils über eine eigene unabhängige und vertraute Windows-Benutzeroberflächen verfügen, gleichzeitig einen Computer gemeinsam zu nutzen. Es gibt mehrere Möglichkeiten, wie Benutzer auf Ihre Sitzungen zugreifen können. Eine Möglichkeit besteht darin, mithilfe der [Remote Desktop-Apps](../remote-desktop-services/clients/remote-desktop-clients.md) mit beliebigen Geräten einen Remoting in den Server zu verwenden. Eine andere Möglichkeit ist die Verwendung von physischen Stationen, die Stationen an den Multipoint-Server angefügt sind:

-   Direkt an videports auf dem Computer

-   Über spezialisierte USB Zero-Clients (auch als Multifunktions-USB-Hubs bezeichnet) sowie über ähnliche USB-over-Ethernet-Geräte.

-   Über das LAN (Local Area Network)

Diese Methoden werden weiter unten in diesem Dokument in [Multipoint Services-Stationen](MultiPoint-services-Stations.md) ausführlicher beschrieben.

In diesem Dokument werden die folgenden Faktoren behandelt, die bei der Planung der Bereitstellung von Multipoint Services zu berücksichtigen sind:

-   Welche Art von Desktops für Ihr Multipoint Services-System verwendet werden soll: benötigen Sie Sitzungen, virtuelle Computer oder Windows-PCs?

-   [Auswählen von Hardware für Ihr Multipoint Services-System](./select-hardware-mps.md): welche Hardware Entscheidungen sollten Sie treffen?

-   [Hardwareanforderungen und Empfehlungen zur Leistung](./hardware-and-performance-recommendations.md): welche Hardware ist für Multipoint Services erforderlich?

-   [Planung der Multipoint Services-Site](MultiPoint-services-Site-Planning.md): wo werden die Computer, auf denen Multipoint Services und deren Stationen ausgeführt werden, und wie werden Sie konfiguriert?

-   [Netzwerküberlegungen und Benutzerkonten](Network-Considerations-and-User-Accounts.md): die Netzwerkumgebung, in der das Multipoint Services-System bereitgestellt wird, kann sich darauf auswirken, wie Benutzerkonten verwaltet werden. Was ist Ihre Netzwerkumgebung? Wie werden Benutzerkonten verwaltet?

-   [Speichern von Dateien mit Multipoint Services](Storing-Files-with-MultiPoint-services.md): wo werden Benutzer Dateien gespeichert, und wie wird darauf zugegriffen?

-   [Prüfliste vor der Bereitstellung](Predeployment-Checklist.md)