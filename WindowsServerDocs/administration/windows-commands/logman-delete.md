---
title: logman delete
description: Referenz Artikel für den Befehl logman delete, mit dem ein vorhandener Datensammler gelöscht wird.
ms.topic: reference
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 57d909a23e65de3c74daff4fe82f42943cb3875d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634372"
---
# <a name="logman-delete"></a>logman delete

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Löscht einen vorhandenen Datensammler.

## <a name="syntax"></a>Syntax

```
logman delete <[-n] <name>> [options]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
| --------- | ----------- |
| -s `<computer name>` | Führt den Befehl auf dem angegebenen Remote Computer aus. |
| -config `<value>` | Gibt die Einstellungsdatei an, die Befehlsoptionen enthält. |
| [-n] `<name>` | Name des Zielobjekts |
| -ETS | Sendet Befehle direkt an Ereignis Ablauf Verfolgungs Sitzungen, ohne zu speichern oder zu planen. |
| -[-] u `<user [password]>` | Gibt den Benutzer an, der als ausgeführt werden soll. Wenn Sie einen als Kennwort eingeben, wird \* eine Eingabeaufforderung für das Kennwort ausgegeben. Das Kennwort wird nicht angezeigt, während Sie es an der Eingabeaufforderung eingeben. |
| /? | Zeigt die kontextbezogene Hilfe an. |

### <a name="examples"></a>Beispiele

Um den Datensammler *perf_log*zu löschen, geben Sie Folgendes ein:

```
logman delete perf_log
```

## <a name="additional-references"></a>Weitere Verweise

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)

- [logman-Befehl](logman.md)
