---
title: manage-bde-Sperre
description: Referenz Artikel für den Befehl manage-bde Lock, mit dem ein durch BitLocker geschütztes Laufwerk gesperrt wird, um den Zugriff darauf zu verhindern, sofern der entsperrungs Schlüssel nicht verfügbar ist.
ms.topic: reference
ms.assetid: b8858e61-3a7e-4d03-8c98-5c09853f35e8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e9b8dfdac7bc8b833a89c9ba447b99984a923f86
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633748"
---
# <a name="manage-bde-lock"></a>manage-bde-Sperre

Sperrt ein durch BitLocker geschütztes Laufwerk, um den Zugriff darauf zu verhindern, es sei denn, der Entsperrschlüssel wird bereitgestellt.

## <a name="syntax"></a>Syntax

```
manage-bde -lock [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
| --------- | ----------- |
| `<drive>` | Stellt einen von einem Doppelpunkt gefolgten Laufwerkbuchstaben dar. |
| -Computername | Gibt an, dass manage-bde.exe zum Ändern des BitLocker-Schutzes auf einem anderen Computer verwendet wird. Sie können auch **-CN** als abgekürzte Version dieses Befehls verwenden. |
| `<name>` | Stellt den Namen des Computers dar, auf dem der BitLocker-Schutz geändert werden soll. Akzeptierte Werte sind der NetBIOS-Name des Computers und die IP-Adresse des Computers. |
| -? oder /? | Zeigt eine kurze Hilfe an der Eingabeaufforderung an. |
| -Help oder-h | Zeigt die gesamte Hilfe an der Eingabeaufforderung an. |

### <a name="examples"></a>Beispiele

Zum Sperren von Daten Laufwerk D geben Sie Folgendes ein:

```
manage-bde –lock D:
```

## <a name="additional-references"></a>Weitere Verweise

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)

- [Befehl "Manage-BDE"](manage-bde.md)
