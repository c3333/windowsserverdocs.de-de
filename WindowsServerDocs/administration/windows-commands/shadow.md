---
title: shadow
description: Referenz Artikel zum Schatten Befehl, mit dem Sie eine aktive Sitzung eines anderen Benutzers auf einem Remotedesktop-Sitzungshost Server remote steuern können.
ms.topic: reference
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4689806b810fa46ccef3cd73b94c4d729c47f641
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718357"
---
# <a name="shadow"></a>shadow

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ermöglicht die Remote Steuerung einer aktiven Sitzung eines anderen Benutzers auf einem Remotedesktop-Sitzungshost Server.

## <a name="syntax"></a>Syntax

```
shadow {<sessionname> | <sessionID>} [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
|--|--|
| `<sessionname>` | Gibt den Namen der Sitzung an, die Sie remote steuern möchten. |
| `<sessionID>` | Gibt die ID der Sitzung an, die Sie remote steuern möchten. Verwenden Sie den **Abfrage Benutzer** , um die Liste der Sitzungen und ihre Sitzungs-IDs anzuzeigen. |
| /server:`<servername>` | Gibt den Remotedesktop-Sitzungshost Server mit der Sitzung an, die Sie remote steuern möchten. Standardmäßig wird der aktuelle Remotedesktop Sitzung Host4 Server verwendet. |
| /v | Zeigt Informationen zu den Aktionen an, die ausgeführt werden. |
| /? | Zeigt die Hilfe an der Eingabeaufforderung an. |

#### <a name="remarks"></a>Bemerkungen

- Sie können die Sitzung entweder anzeigen oder aktiv steuern. Wenn Sie die Sitzung eines Benutzers aktiv steuern möchten, können Sie Tastatur-und Mausaktionen für die Sitzung eingeben.

- Sie können Ihre eigenen Sitzungen (außer der aktuellen Sitzung) jederzeit Remote steuern. Sie müssen jedoch über die Berechtigung "Vollzugriff" oder "Remote Steuerung" verfügen, um eine andere Sitzung Remote zu steuern.

- Sie können die Remote Steuerung auch mithilfe von Remotedesktopdienste-Manager initiieren.

- Vor Beginn der Überwachung warnt der Server den Benutzer, dass die Sitzung remote gesteuert wird, es sei denn, diese Warnung ist deaktiviert. Die Sitzung scheint einige Sekunden lang eingefroren zu sein, während Sie auf eine Antwort des Benutzers wartet. Verwenden Sie zum Konfigurieren der Remote Steuerung für Benutzer und Sitzungen das Remotedesktopdienste-Konfigurationstool oder die Remotedesktopdienste Erweiterungen für lokale Benutzer und Gruppen sowie für Active Directory-Benutzer und-Computer.

- Ihre Sitzung muss in der Lage sein, die Videoauflösung zu unterstützen, die in der Sitzung verwendet wird, für die Sie eine Remote Steuerung durchführt.

- Die Konsolen Sitzung kann keine Remote Steuerung einer anderen Sitzung durchführt, und Sie kann nicht von einer anderen Sitzung remote gesteuert werden.

- Wenn Sie die Remote Steuerung beenden möchten (shadowingover), drücken Sie STRG + `*` ( `*` nur mit der numerischen Tastatur).

## <a name="examples"></a>Beispiele

Zum Schatten der *Sitzung 93*geben Sie Folgendes ein:

```
shadow 93
```

Geben Sie Folgendes ein, um die Sitzungs *ACCTG01*zu schattieren

```
shadow ACCTG01
```

## <a name="additional-references"></a>Zusätzliche Referenzen

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)

- [Remotedesktopdienste (Terminaldienste): Befehlsreferenz](remote-desktop-services-terminal-services-command-reference.md)
