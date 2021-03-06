---
title: bitsadmin gettype
description: Referenz Artikel für den bizadmin GetType-Befehl, der den Auftragstyp des angegebenen Auftrags abruft.
ms.topic: reference
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ccb938e1fe67164567bbe8d6c87d87423026db96
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631562"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

Ruft den Auftragstyp des angegebenen Auftrags ab.

## <a name="syntax"></a>Syntax

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
| -------------- | -------------- |
| Auftrag | Der Anzeige Name oder GUID des Auftrags. |

#### <a name="output"></a>Output

Die zurückgegebenen Ausgabewerte können wie folgt lauten:

| type | BESCHREIBUNG |
| --------------- | ----------- |
| Download | Der Auftrag ist ein Download. |
| Upload | Der Auftrag ist ein Upload. |
| Upload-Antwort | Der Auftrag ist ein Upload-Antwort-Vorgang. |
| Unbekannt | Der Auftrag weist einen unbekannten Typ auf. |

## <a name="examples"></a>Beispiele

So rufen Sie den Auftragstyp für den Auftrag mit dem Namen *mydownloadjob*ab

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Weitere Verweise

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)

- [Bider admin-Befehl](bitsadmin.md)
