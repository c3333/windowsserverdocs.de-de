---
title: ksetup changepassword
description: Referenz Artikel für den Befehl "Ksetup ChangePassword", bei dem das Kennwort für den Schlüsselverteilungscenter (KDC)-Kennwort (kpasswd) verwendet wird, um das Kennwort des angemeldeten Benutzers zu ändern.
ms.topic: reference
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ff88136c51b9790867c76d54467996e9305cbc93
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639717"
---
# <a name="ksetup-changepassword"></a>ksetup changepassword

Verwendet das Kennwort für den Schlüsselverteilungscenter (KDC)-Kennwort (kpasswd), um das Kennwort des angemeldeten Benutzers zu ändern. Die Ausgabe des Befehls informiert Sie über den Status Erfolg oder Fehler.

Sie können überprüfen, ob **kpasswd** festgelegt ist, indem Sie den `ksetup /dumpstate` Befehl ausführen und die Ausgabe anzeigen.


## <a name="syntax"></a>Syntax

```
ksetup /changepassword <oldpassword> <newpassword>
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
| --------- | ----------- |
| `<oldpassword>` | Gibt das vorhandene Kennwort des angemeldeten Benutzers an. |
| `<newpassword>` | Gibt das neue Kennwort des angemeldeten Benutzers an. Dieses Kennwort muss alle Kenn Wort Anforderungen erfüllen, die auf diesem Computer festgelegt sind. |

#### <a name="remarks"></a>Hinweise

- Wenn das Benutzerkonto in der aktuellen Domäne nicht gefunden wird, werden Sie vom System aufgefordert, den Domänen Namen anzugeben, in dem sich das Benutzerkonto befindet.

- Wenn Sie bei der nächsten Anmeldung eine Kenn Wort Änderung erzwingen möchten, kann mit diesem Befehl das Sternchen (*) verwendet werden, damit der Benutzer zur Eingabe eines neuen Kennworts aufgefordert wird.

-

### <a name="examples"></a>Beispiele

Geben Sie Folgendes ein, um das Kennwort eines Benutzers zu ändern, der derzeit in dieser Domäne an diesem Computer angemeldet ist:

```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```

Geben Sie Folgendes ein, um das Kennwort eines Benutzers zu ändern, der zurzeit in der Domäne "Domäne" angemeldet ist:

```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```

Geben Sie Folgendes ein, um zu erzwingen, dass der aktuell angemeldete Benutzer das Kennwort bei der nächsten Anmeldung ändert:

```
ksetup /changepassword Pas$w0rd *
```

## <a name="additional-references"></a>Weitere Verweise

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)

- [Ksetup-Befehl](ksetup.md)

- [Ksetup (dumpstate-Befehl)](ksetup-dumpstate.md)

- [Ksetup-Befehl "addkpasswd"](ksetup-addkpasswd.md)

- [Ksetup-Befehl "Delta passwd"](ksetup-delkpasswd.md)

- [Ksetup (dumpstate-Befehl)](ksetup-dumpstate.md)
