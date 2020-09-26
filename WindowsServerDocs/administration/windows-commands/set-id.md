---
title: set id
description: Referenz Artikel für den DiskPart Set ID-Befehl, der das Feld Partitionstyp für die Partition mit dem Fokus ändert.
ms.topic: reference
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f25498a89cdb7347a40825af70621d5cd09440a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389084"
---
# <a name="set-id-diskpart"></a>ID festlegen (DiskPart)

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ändert das Feld Partitionstyp für die Partition mit dem Fokus. Dieser Befehl funktioniert nicht auf dynamischen Datenträgern oder auf von Microsoft reservierten Partitionen.

> [!IMPORTANT]
> Dieser Befehl ist nur für die Verwendung durch Originalgerätehersteller (OEMs) vorgesehen. Das Ändern von Partitionstyp Feldern mit diesem Parameter kann dazu führen, dass der Computer ausfällt oder nicht gestartet werden kann. Wenn Sie nicht OEM sind oder mit GPT-Datenträgern vertraut sind, sollten Sie die Partitionstyp Felder auf GPT-Datenträgern nicht mithilfe dieses Parameters ändern. Verwenden Sie stattdessen immer den Befehl [create partition efi](create-partition-efi.md) zum Erstellen von EFI-Systempartitionen, den [create partition msr](create-partition-msr.md) -Befehl zum Erstellen von reservierten Microsoft-Partitionen und den [create partition primary](create-partition-primary.md) -Befehl ohne den ID-Parameter, um primäre Partitionen auf GPT-Datenträgern zu erstellen.

## <a name="syntax"></a>Syntax

```
set id={ <byte> | <GUID> } [override] [noerr]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
|--|--|
| `<byte>` | Gibt für Master Boot Record-Datenträger (MBR) den neuen Wert für das typanfeld (in Hexadezimal Form) für die Partition an. Alle Partitionstypen **Bytes** können mit diesem Parameter angegeben werden, mit Ausnahme des Typs 0x42, der eine LDM-Partition angibt. Beachten Sie, dass das führende 0x beim Angeben des hexadezimalen Partitions Typs ausgelassen wird. |
| `<GUID>` | Gibt für GPT-Datenträger (GUID-Partitionstabelle) den neuen GUID-Wert für das typanfeld für die Partition an. Zu den erkannten GUIDs zählen:<ul><li>**EFI-Systempartition:** c12a7328-f81f-11d2-ba4b-00a0c93ec93b</li><li>**Grundlegende Daten Partition:** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li></ul>Jeder GUID des Partitions Typs kann mit diesem Parameter angegeben werden, mit Ausnahme der folgenden:<ul><li>**Reservierte Microsoft-Partition:** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**LDM-Metadatenpartition auf einem dynamischen** Datenträger: 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**LDM-Daten Partition auf einem dynamischen Datenträger:** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>**Clustermetadatenpartition:** db97dba9-0840-4bae-97f0-ffb9a327c7e1</li></ul> |
| override | erzwingt das Aufheben der Einbindung des Dateisystems auf dem Volume, bevor der Partitionstyp geändert wird. Wenn Sie den Befehl **Set ID** ausführen, versucht Diskpart, das Dateisystem auf dem Volume zu sperren und zu entfernen. Wenn **override** nicht angegeben wird und der aufzurufende Befehl zum Sperren des Dateisystems fehlschlägt (z. b. weil ein geöffnetes Handle vorhanden ist), schlägt der Vorgang fehl. Wenn **override** angegeben ist, erzwingt DiskPart die Aufhebung der Einbindung, auch wenn der aufzurufende Befehl zum Sperren des Dateisystems fehlschlägt und alle geöffneten Handles für das Volume nicht mehr gültig sind. |
| Noerr | Wird nur für die Skripterstellung verwendet. Wenn ein Fehler auftritt, verarbeitet DiskPart weiterhin Befehle so, als ob der Fehler nicht aufgetreten ist. Ohne diesen Parameter bewirkt ein Fehler, dass DiskPart mit einem Fehlercode beendet wird. |

## <a name="remarks"></a>Hinweise

- Abgesehen von den zuvor erwähnten Einschränkungen überprüft DiskPart nicht die Gültigkeit des Werts, den Sie angeben (außer um sicherzustellen, dass es sich um ein Byte in Hexadezimal Form oder eine GUID handelt).

## <a name="examples"></a>Beispiele

Geben Sie Folgendes ein, um das typanfeld auf *0x07* festzulegen und das Aufheben der Einbindung des Dateisystems zu erzwingen:

```
set id=0x07 override
```

Geben Sie Folgendes ein, um das typanfeld als einfache Daten Partition festzulegen:

```
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7
```

## <a name="additional-references"></a>Zusätzliche Referenzen

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
