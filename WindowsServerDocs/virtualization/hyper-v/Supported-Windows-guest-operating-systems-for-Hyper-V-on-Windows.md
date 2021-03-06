---
title: Unterstützte Windows-Gast Betriebssysteme für Hyper-V unter Windows Server
description: Listet die Windows-Betriebssysteme auf, die für die Verwendung als Gast in einem virtuellen Computer unterstützt werden. Enthält auch Links zu ähnlichen Artikeln für frühere Versionen von Hyper-V.
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/08/2019
ms.openlocfilehash: 2e5cf6c94d0127a283c640a48aaa2fced5472e9d
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746725"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Unterstützte Windows-Gast Betriebssysteme für Hyper-V unter Windows Server

>Gilt für: Windows Server 2016, Windows Server 2019

Hyper-V unterstützt mehrere Versionen von Windows Server-, Windows-und Linux-Distributionen, die auf virtuellen Computern ausgeführt werden, als Gast Betriebssysteme. In diesem Artikel werden die unterstützten Windows Server-und Windows-Gast Betriebssysteme behandelt. Informationen zu Linux-und FreeBSD-Distributionen finden Sie [unter Unterstützte virtuelle Linux-und FreeBSD-Computer für Hyper-V unter Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).

Für einige Betriebssysteme ist Integration Services integriert. Andere erfordern, dass Sie Integrationsdienste in einem separaten Schritt installieren oder aktualisieren, nachdem Sie das Betriebssystem auf dem virtuellen Computer eingerichtet haben. Weitere Informationen finden Sie in den Abschnitten unten und  [Integration Services](/virtualization/hyper-v-on-windows/reference/integration-services).

## <a name="supported-windows-server-guest-operating-systems"></a>Unterstützte Gastbetriebssysteme für Windows Server

Im folgenden finden Sie die Versionen von Windows Server, die als Gast Betriebssysteme für Hyper-V in Windows Server 2016 und Windows Server 2019 unterstützt werden.

|Gastbetriebssystem (Server)|Maximale Anzahl virtueller Prozessoren|Integration Services|Hinweise|
|-------------------------------------|----------------------------------------|------------------------|---------|
|Windows Server, Version 1909 |240 für Generation 2;<br>64 für Generation 1|Integriert|Für die Unterstützung von mehr als 240 virtuellen Prozessoren sind Windows Server-, Version 1903-und spätere Gast Betriebssysteme erforderlich.|
|Windows Server, Version 1903 |240 für Generation 2;<br>64 für Generation 1|Integriert||
|Windows Server, Version 1809 |240 für Generation 2;<br>64 für Generation 1|Integriert||
|Windows Server 2019 |240 für Generation 2;<br>64 für Generation 1|Integriert||
|Windows Server Version 1803 |240 für Generation 2;<br>64 für Generation 1|Integriert||
|Windows Server 2016 |240 für Generation 2;<br>64 für Generation 1|Integriert||
|Windows Server 2012 R2 |64|Integriert||
|Windows Server 2012 |64|Integriert||
|Windows Server 2008 R2 mit Service Pack 1 (SP 1)|64|Installieren Sie alle wichtigen Windows-Updates nach dem Einrichten des Gast Betriebssystems.|Datacenter, Enterprise, Standard und Web Edition.|
|Windows Server 2008 mit Service Pack 2 (SP2)|8|Installieren Sie alle wichtigen Windows-Updates nach dem Einrichten des Gast Betriebssystems.|Datacenter, Enterprise, Standard und Web Edition (32-Bit und 64-Bit).|

## <a name="supported-windows-client-guest-operating-systems"></a>Unterstützte Windows Client-Gast Betriebssysteme

Im folgenden finden Sie die Versionen des Windows-Clients, die als Gast Betriebssysteme für Hyper-V in Windows Server 2016 und Windows Server 2019 unterstützt werden.

|Gastbetriebssystem (Client)|Maximale Anzahl virtueller Prozessoren|Integration Services|Hinweise|
|-------------------------------------|----------------------------------------|------------------------|---------|
|Windows 10|32|Integriert||
|Windows 8.1|32|Integriert||
|Windows 7 mit Service Pack 1 (SP 1)|4|Aktualisieren Sie die Integrationsdienste, nachdem Sie das Gast Betriebssystem eingerichtet haben.|Ultimate, Enterprise und Professional Edition (32-Bit und 64-Bit).|

## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>Unterstützung von Gastbetriebssystemen in anderen Versionen von Windows

Die folgende Tabelle enthält Links zu Informationen zu Gastbetriebssystemen, die für Hyper-V unter anderen Windows-Versionen unterstützt werden.

|Hostbetriebssystem|Thema|
|-------------------------|---------|
|Windows 10|[Unterstützte Gast Betriebssysteme für Hyper-V-Client in Windows 10](/virtualization/hyper-v-on-windows/about/supported-guest-os)|
|Windows Server 2012 R2 und Windows 8.1|-   [Unterstützte Windows-Gast Betriebssysteme für Hyper-V in Windows Server 2012 R2 und Windows 8.1](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Linux-und FreeBSD-Virtual Machines unter Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|
|Windows Server 2012 und Windows 8|[Unterstützte Windows-Gastbetriebssysteme für Hyper-V in Windows Server 2012 und Windows 8](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|
|Windows Server 2008 und Windows Server 2008 R2|[Grundlegendes zu virtuellen Computern und Gastbetriebssystemen](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|

## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>So bietet Microsoft Unterstützung für Gast Betriebssysteme

Microsoft bietet auf folgende Weise Unterstützung für Gastbetriebssysteme:

-   Der Microsoft-Support hilft beim Lösen von Problemen in Microsoft-Betriebssystemen und -Integrationsdiensten.

-   Für Probleme in anderen Betriebssystemen, die vom Anbieter des Betriebssystems für die Ausführung unter Hyper-V zertifiziert wurden, wird der Support vom Anbieter geleistet.

-   Andere in den Betriebssystemen ermittelte Probleme werden von Microsoft an die Supportcommunity mehrerer Anbieter weitergeleitet, [TSANet](https://www.tsanet.org/).

## <a name="additional-references"></a>Weitere Verweise

-   [Virtuelle Linux- und FreeBSD Computer unter Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

-   [Unterstützte Gast Betriebssysteme für Hyper-V-Client in Windows 10](/virtualization/hyper-v-on-windows/about/supported-guest-os)