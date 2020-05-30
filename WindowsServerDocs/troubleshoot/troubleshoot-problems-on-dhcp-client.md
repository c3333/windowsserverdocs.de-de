---
title: Beheben von Problemen auf dem DHCP-Client
description: Diese Artilce bietet eine Einführung in die Behandlung von Problemen auf dem DHCP-Client und das Sammeln von Daten.
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: a6064b9e497fcd54671292ade77a08c06ba42920
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150298"
---
# <a name="troubleshoot-problems-on-the-dhcp-client"></a>Beheben von Problemen auf dem DHCP-Client

In diesem Artikel wird erläutert, wie Sie Probleme beheben, die auf DHCP-Clients auftreten.

## <a name="troubleshooting-checklist"></a>Checkliste zur Problembehandlung

Überprüfen Sie die folgenden Geräte und Einstellungen:

  - Kabel sind verbunden und funktionieren.

  - Die MAC-Filterung ist auf den Switches aktiviert, mit denen der Client verbunden ist.

  - Der Netzwerkadapter ist aktiviert.

  - Der richtige Netzwerkadapter Treiber ist installiert und wird aktualisiert.

  - Der DHCP-Client Dienst wird gestartet und ausgeführt. Um dies zu überprüfen, führen Sie den Befehl **net Start** aus, und suchen Sie nach **DHCP-Client**.

  - Es sind keine firewallsperr Ports 67 und 68 UDP auf dem Client Computer vorhanden.

## <a name="event-logs"></a>Ereignisprotokolle

Überprüfen Sie die Ereignisprotokolle "Microsoft-Windows-DHCP-Client Ereignisse/betriebsbereit" und "Microsoft-Windows-DHCP-Client Ereignisse/admin". Alle Ereignisse, die sich auf den DHCP-Client Dienst beziehen, werden an diese Ereignisprotokolle gesendet.  
Die Microsoft-Windows-DHCP-Client Ereignisse befinden sich im Ereignisanzeige unter **Anwendungs-und Dienst Protokolle**.

Der PowerShell-Befehl "Get-netadapter-includehidden" enthält die erforderlichen Informationen, um die in den Protokollen aufgelisteten Ereignisse zu interpretieren. Beispielsweise Schnittstellen-ID, Mac-Adresse usw.

## <a name="data-collection"></a>Datensammlung

Es wird empfohlen, dass Sie Daten gleichzeitig sowohl auf dem DHCP-Client als auch auf der Serverseite sammeln, wenn das Problem auftritt. Je nach tatsächlichem Problem können Sie die Untersuchung jedoch auch mit einem einzelnen Dataset auf dem DHCP-Client oder DHCP-Server starten.

Verwenden Sie [wireshark](https://www.wireshark.org/download.html)zum Sammeln von Daten vom Server und dem betroffenen Client. Starten Sie die Erfassung gleichzeitig auf dem DHCP-Client und den DHCP-Server Computern.

Führen Sie auf dem Client, auf dem das Problem auftritt, die folgenden Befehle aus:

```console
ipconfig /release  
ipconfig /renew
```

Dann können Sie Wireshark auf dem Client und dem Server abbrechen. Überprüfen Sie die generierten Ablauf Verfolgungen. Diese sollten zumindest angeben, in welcher Phase die Kommunikation beendet wird.