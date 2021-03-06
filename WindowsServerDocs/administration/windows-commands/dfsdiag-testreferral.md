---
title: dfsdiag testreferral
description: Referenz Artikel für den Dfsdiag testreferral-Befehl, der verteiltes Dateisystem (DFS)-Verweise überprüft.
ms.topic: reference
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e1fb5526f49994ddbfb35c0c64f53933ab67674f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634135"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag testreferral

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prüft verteiltes Dateisystem (DFS)-Verweise durch Ausführen der folgenden Tests:

- Wenn Sie den **dfspath***-Parameter ohne Argumente verwenden, überprüft der Befehl, ob die Verweis Liste alle vertrauenswürdigen Domänen enthält.

- Wenn Sie eine Domäne angeben, führt der Befehl eine Integritäts Überprüfung von Domänen Controllern ( `dfsdiag /testdcs` ) durch und testet die Standort Zuordnungen und den Domänen Cache des lokalen Hosts.

- Wenn Sie eine Domäne und \Sysvol oder \netlogon angeben, führt der Befehl die gleichen Domänen Controller-Integritätsprüfungen durch und überprüft, ob die Gültigkeitsdauer **(Time to Live, TTL)** der SYSVOL-oder Netlogon-Verweise mit dem Standardwert von 900 Sekunden übereinstimmt.

- Wenn Sie einen Namespace Stamm angeben, führt der Befehl die gleichen Domänen Controller-Integritätsprüfungen zusammen mit einer DFS-Konfigurations Überprüfung ( `dfsdiag /testdfsconfig` ) und einer Namespace-Integritäts Überprüfung ( `dfsdiag /testdfsintegrity` ) durch.

- Wenn Sie einen DFS-Ordner (Link) angeben, führt der Befehl die gleichen Namespace-Stamm Integritätsprüfungen aus, zusammen mit der Überprüfung der Standort Konfiguration für Ordner Ziele (Dfsdiag/Testsites) und der Überprüfung der Standort Zuordnung des lokalen Hosts.

## <a name="syntax"></a>Syntax

```
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
| --------- | ----------- |
| /DFSpath:`<path to get referrals>` | Dabei kann es sich um eine der folgenden Methoden handeln:<ul><li>**Leer:** Testet nur vertrauenswürdige Domänen.</li><li>`\\Domain:` Testet nur die Verweise von Domänen Controllern.</li><li>`\\Domain\SYSvol:` Testet nur SYSVOL-Verweise.</li><li>`\\Domain\NETLOGON:` Testet nur Netlogon-Verweise.</li><li>`\\<domain or server>\<namespace root>:` Testet nur Namespace-Stamm Verweise.</li><li>`\\<domain or server>\<namespace root>\<DFS folder>:` Testet nur die Verweise auf den DFS-Ordner (Link).</li></ul> |
| /full | Gilt nur für Domänen-und Stamm Verweise. Überprüft die Konsistenz der Standort Zuordnungs Informationen zwischen der Registrierung und den Active Directory-Domänen Diensten (AD DS). |

## <a name="examples"></a>Beispiele

Geben Sie Folgendes ein, um die verteiltes Dateisystem (DFS)-Verweise in der Datei "%% amp; quot;" *.*

```
dfsdiag /testreferral /DFSpath:\\contoso.com\MyNamespace
```

Um die verteiltes Dateisystem (DFS)-Verweise in allen vertrauenswürdigen Domänen zu überprüfen, geben Sie Folgendes ein:

```
dfsdiag /testreferral /DFSpath:
```

## <a name="additional-references"></a>Weitere Verweise

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)

- [Dfsdiag-Befehl](dfsdiag.md)
