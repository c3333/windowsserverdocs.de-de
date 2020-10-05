---
title: tftp
description: Referenz Artikel für den TFTP-Befehl, der Dateien an einen und von einem Remote Computer überträgt.
ms.topic: reference
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 89a4edb69753ae34814d13f90f21332c6f748244
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717907"
---
# <a name="tftp"></a>tftp

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Überträgt Dateien an und von einem Remote Computer, in der Regel auf einem Computer mit UNIX, auf dem der Trivial File Transfer Protocol-Dienst oder-Daemon ausgeführt wird. TFTP wird in der Regel von eingebetteten Geräten oder Systemen verwendet, die während des Startvorgangs von einem TFTP-Server aus Firmware, Konfigurationsinformationen oder ein System Abbild abrufen.

> Wichtig Das TFTP-Protokoll unterstützt keine Authentifizierungs-oder Verschlüsselungsmechanismen und kann daher ein Sicherheitsrisiko darstellen, wenn es vorhanden ist. Die Installation des TFTP-Clients wird für Systeme, die mit dem Internet verbunden sind, nicht empfohlen. Ein TFTP-Server Dienst wird von Microsoft aus Sicherheitsgründen nicht mehr bereitgestellt.

## <a name="syntax"></a>Syntax

```
tftp [-i] [<host>] [{get | put}] <source> [<destination>]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
|--|--|
| -i | Gibt den binären Bild Übertragungsmodus (auch als Octett-Modus bezeichnet) an. Im binären Bild Modus wird die Datei in 1-Byte-Einheiten übertragen. Verwenden Sie diesen Modus beim Übertragen von Binärdateien. Wenn Sie die Option **-i** nicht verwenden, wird die Datei im ASCII-Modus übertragen. Dies ist der Standard Übertragungsmodus. In diesem Modus werden die Zeilenende (EOL)-Zeichen in ein entsprechendes Format für den angegebenen Computer konvertiert. Verwenden Sie diesen Modus beim Übertragen von Textdateien. Wenn eine Dateiübertragung erfolgreich ist, wird die Datenübertragungsrate angezeigt. |
| `<host>` | Gibt den lokalen oder Remote Computer an. |
| get | Überträgt das *Dateiziel* auf dem Remote Computer an die Datei *Quelle* auf dem lokalen Computer. |
| put | Überträgt die Datei *Quelle* auf dem lokalen Computer an das *Dateiziel* auf dem Remote Computer. Da das TFTP-Protokoll die Benutzerauthentifizierung nicht unterstützt, muss der Benutzer auf dem Remote Computer angemeldet sein, und die Dateien müssen auf dem Remote Computer beschreibbar sein. |
| `<source>` | Gibt die zu übertragenden Datei an. |
| `<destination>` | Gibt an, wohin die Datei übertragen werden soll. |

## <a name="examples"></a>Beispiele

Geben Sie Folgendes ein, um die Datei *Boot. img* vom Remote Computer *host1*zu kopieren:

```
tftp  -i Host1 get boot.img
```

## <a name="additional-references"></a>Zusätzliche Referenzen

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
