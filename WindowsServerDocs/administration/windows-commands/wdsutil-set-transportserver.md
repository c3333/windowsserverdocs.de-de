---
title: WDSUTIL Set-TransportServer
description: Referenz Artikel für den Unterbefehl Set-TransportServer, mit dem Konfigurationseinstellungen für einen Transport Server festgelegt werden.
ms.topic: reference
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fd2c1a82dbb83dc45b08fca6259a4aa6b18a4f1b
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730517"
---
# <a name="wdsutil-set-transportserver"></a>WDSUTIL Set-TransportServer

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Legt Konfigurationseinstellungen für einen Transport Server fest.

## <a name="syntax"></a>Syntax
```
wdsutil [Options] /Set-TransportServer [/Server:<Server name>]
     [/ObtainIpv4From:{Dhcp | Range}]
        [/start:<starting IP address>]
        [/End:<Ending IP address>]
     [/ObtainIpv6From:Range]\n\
        [/start:<start IP address>]\n\
        [/End:<End IP address>]
     [/startPort:<starting port>
        [/EndPort:<starting port>
     [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
     [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]
```
### <a name="parameters"></a>Parameter
|Parameter|BESCHREIBUNG|
|-------|--------|
|[/Server:<Server name>]|Gibt den Namen des Transport Servers an. Dabei kann es sich um den NetBIOS-Namen oder den voll qualifizierten Domänen Namen (FQDN) handeln. Wenn kein Transport Servername angegeben ist, wird der lokale Server verwendet.|
|[/ObtainIpv4From: {DHCP-&#124; Bereich}]|Legt die Quelle der IPv4-Adressen wie folgt fest:<p>-[/Start: <IP address> ] legt den Anfang des IP-Adress Bereichs fest. Dies ist nur erforderlich und gültig, wenn diese Option auf **Bereich**festgelegt ist.<br />-[/End: <IP address> ] legt das Ende des IP-Adress Bereichs fest. Dies ist nur erforderlich und gültig, wenn diese Option auf **Bereich**festgelegt ist.<br />-[/startPort: <port> ] legt den Anfang des Port Bereichs fest.<br />-[/EndPort: <port> ] legt das Ende des Port Bereichs fest.|
|[/ObtainIpv6From: Bereich]|Gibt die Quelle von IPv6-Adressen an. Diese Option gilt nur für Windows Server 2008 R2, und der einzige unterstützte Wert ist **Range**.<p>-[/Start: <IP address> ] legt den Anfang des IP-Adress Bereichs fest. Dies ist nur erforderlich und gültig, wenn diese Option auf **Bereich**festgelegt ist.<br />-[/End: <IP address> ] legt das Ende des IP-Adress Bereichs fest. Dies ist nur erforderlich und gültig, wenn diese Option auf **Bereich**festgelegt ist.<br />-[/startPort: <port> ] legt den Anfang des Port Bereichs fest.<br />-[/EndPort: <port> ] legt das Ende des Port Bereichs fest.|
|[/Profile: {10Mbit/s &#124; 100 Mbit/s &#124; 1Gbit/s &#124; Custom}]|Gibt das zu verwendende Netzwerk Profil an. Diese Option ist nur für Server verfügbar, auf denen Windows Server 2008 oder Windows Server 2003 ausgeführt wird.|
|[/MulticastSessionPolicy]|Konfiguriert die Übertragungs Einstellungen für Multicast Übertragungen. Dieser Befehl ist nur für Windows Server 2008 R2 verfügbar.<p>-[/Policy: {None &#124; Autodisconnect &#124; Multistream}] bestimmt, wie langsame Clients behandelt werden. " **None** " bedeutet, dass alle Clients in einer Sitzung mit derselben Geschwindigkeit aufbewahrt werden. **Autodisconnect** bedeutet, dass alle Clients, die unter den angegebenen **/Threshold** ablegen, getrennt werden. **Multistream** bedeutet, dass Clients in mehrere Sitzungen unterteilt werden, wie von **/StreamCount**angegeben.<br />-[/Threshold: <Speed in KBps> ] legt die minimale Übertragungsrate in Kbit/s für **/Policy: Autodisconnect**fest. Clients, die diese Rate unterhalb dieses Satzes ablegen, werden von Multicast Übertragungen getrennt.<br />-[/StreamCount: {2 &#124; 3}] [/Fallback: {Yes &#124; No}] legt die Anzahl von Sitzungen für **/Policy: Multistream**fest. **2** bedeutet zwei Sitzungen (schnell und langsam) und **3** drei Sitzungen (langsam, Mittel, schnell).<br />-[/Fallback: {yes &#124; No}] bestimmt, ob Clients, die getrennt sind, die Übertragung mithilfe einer anderen Methode (sofern vom Client unterstützt) fortgesetzt wird. Wenn Sie den WDS-Client verwenden, wird für den Computer ein Fallback auf Unicasting durchlaufen. Wdsmcast.exe unterstützt keinen Fall Back Mechanismus. Diese Option gilt auch für Clients, die **Multistream**nicht unterstützen. In diesem Fall wird der Computer auf eine andere Methode zurückgreifen, anstatt zu einer langsameren Übertragungs Sitzung zu wechseln.|
## <a name="examples"></a>Beispiele
Geben Sie Folgendes ein, um den IPv4-Adressbereich für den Server festzulegen:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Geben Sie Folgendes ein, um den IPv4-Adressbereich, den Port Bereich und das Profil für den Server festzulegen:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
## <a name="additional-references"></a>Zusätzliche Referenzen
- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
- [Befehl "WDSUTIL deaktivieren-Transportserver"](wdsutil-disable-transportserver.md)
- [Befehl "WDSUTIL Enable-Transportserver"](wdsutil-enable-transportserver.md)
- [Befehl "WDSUTIL Get-TransportServer"](wdsutil-get-transportserver.md)
- [WDSUTIL Start-TransportServer-Befehl](wdsutil-start-transportserver.md)
- [WDSUTIL-Befehl zum Abbrechen von Transportserver](wdsutil-stop-transportserver.md)
