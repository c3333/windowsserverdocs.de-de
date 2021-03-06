---
title: nslookup set
description: Referenz Artikel für den Befehl "nslookup Set", mit dem die Konfigurationseinstellungen geändert werden, die sich auf das Verhalten von Such Vorgängen
ms.topic: reference
ms.assetid: 1fe5b36d-e93e-468b-abca-43b0204b32d1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 70850cd40f7eb54212b112a6e1410a25628a3d7f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636478"
---
# <a name="nslookup-set"></a>nslookup set

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ändert die Konfigurationseinstellungen, die die Funktionsweise von suchen beeinflussen.

## <a name="syntax"></a>Syntax

```
set all [class | d2 | debug | domain | port | querytype | recurse | retry | root | search | srchlist | timeout | type | vc] [options]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
| --------- | ----------- |
| [nslookup set all](nslookup-set-all.md) | Listet alle aktuellen Einstellungen auf. |
| [nslookup set class](nslookup-set-class.md) | Ändert die Query-Klasse, die die Protokoll Gruppe der Informationen angibt. |
| [nslookup set d2](nslookup-set-d2.md) | Schaltet den ausführlichen Debugmodus ein oder aus. |
| [nslookup set debug](nslookup-set-debug.md) | Deaktiviert den Debugmodus vollständig. |
| [nslookup set domain](nslookup-set-domain.md) | Ändert den Standard-DNS-Domänen Namen (Domain Name System) in den angegebenen Namen. |
| [nslookup set port](nslookup-set-port.md) | Ändert den Standard-DNS-Namen Server Port (TCP/UDP Domain Name System) in den angegebenen Wert.
| [nslookup set querytype](nslookup-set-querytype.md) | Ändert den Ressourcen Daten Satz für die Abfrage. |
| [nslookup set recurse](nslookup-set-recurse.md) | Weist den Domain Name System (DNS)-Namen Server an, andere Server abzufragen, wenn keine Informationen gefunden werden. |
| [nslookup set retry](nslookup-set-retry.md) | Legt die Anzahl der Wiederholungen fest. |
| [nslookup set root](nslookup-set-root.md) | Ändert den Namen des Stamm Servers, der für Abfragen verwendet wird. |
| [nslookup set search](nslookup-set-search.md) | Fügt Domain Name System die DNS-Domänen Namen (DNS) in der DNS-Domänen Suchliste an die Anforderung an, bis eine Antwort empfangen wird. |
| [nslookup set srchlist](nslookup-set-srchlist.md) | Ändert den Domain Name System Standard-DNS-Domänen Namen und die Suchliste. |
| [nslookup set timeout](nslookup-set-timeout.md) | Ändert die anfängliche Anzahl von Sekunden, die auf eine Antwort auf eine Such Anforderung gewartet werden soll. |
| [nslookup set type](nslookup-set-type.md) | Ändert den Ressourcen Daten Satz für die Abfrage. |
| [nslookup set vc](nslookup-set-vc.md) | Gibt an, ob beim Senden von Anforderungen an den Server eine virtuelle Verbindung verwendet werden soll. |

## <a name="additional-references"></a>Weitere Verweise

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
