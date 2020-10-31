---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Windows PowerShell-Cmdlets zum Sichern und Wiederherstellen von Zertifizierungsstellen
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1a36e72858e5ea2ae95cc0f1730e5906b3c85866
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070732"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets zum Sichern und Wiederherstellen von Zertifizierungsstellen

> Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
> **Autor** : Justin Turner, Senior Support Eskalations Techniker mit der Windows-Gruppe
>
> [!NOTE]
> Dieser Inhalt wurde von einem Mitarbeiter des Microsoft-Kundendiensts geschrieben und richtet sich an erfahrene Administratoren und Systemarchitekten, die einen tieferen technischen Einblick in die Funktionen und Lösungen von Windows Server 2012 R2 suchen, als Ihnen die Themen im TechNet bieten können. Allerdings wurde er nicht mit der gleichen linguistischen Sorgfalt überprüft wie für die Artikel des TechNet üblich, so dass die Sprache gelegentlich holprig klingen mag.

## <a name="overview"></a>Übersicht
Das Windows PowerShell-Modul adcsadministration wurde in Windows Server 2012 eingeführt.  Diesem Modul in Windows Server 2012 R2 wurden zwei neue Cmdlets hinzugefügt, um die Sicherung und Wiederherstellung einer Zertifizierungsstelle zu unterstützen.

-   Backup-CARoleService

-   Restore-CARoleService

## <a name="backup-caroleservice"></a>Backup-CARoleService
**Tabelle "Tabelle", \\ \* Arabisch 17: Sichern und Wiederherstellen von Windows PowerShell-Cmdlets**

**Adcsadministration-Cmdlet: Backup-caroleservice**

|Argumente: **Fett** formatierte Argumente sind erforderlich.|Beschreibung|
|------------------------------------------------|---------------|
|**-Path**|-Zeichenfolge-Speicherort zum Speichern der Sicherung<br />: Dies ist der einzige unbenannte Parameter.<br />-Positions Parameter<p>**Beispiel:**<p>Backup-caroleservice.-Path c:\adcsbackup1<p>Backup-CARoleService c:\adcsbackup2|
|-Keyonly|-Sichern des Zertifizierungsstellen Zertifikats ohne die Datenbank<p>**Beispiel:**<p>Backup-CARoleService "c:\adcsbackup3-keyonly"|
|-Password|: Hiermit wird das Kennwort zum Schützen von Zertifizierungsstellen Zertifikaten und privaten Schlüsseln angegeben<br />-Muss eine sichere Zeichenfolge sein.<br />-Nicht gültig mit dem Parameter "-DatabaseOnly"<p>Beispiel:<p>Backup-CARoleService c:\adcsbackup4-Password (Read-Host-prompt "Password:"-assecurestring)<p>Backup-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -Asplaintext-Force)|
|-DatabaseOnly|-Sichern der Datenbank ohne das Zertifizierungsstellen Zertifikat<p>Backup-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|1. ermöglicht das Überschreiben der Sicherung, die an dem im Parameter "-Path" angegebenen Speicherort bereits vorhanden ist.<p>Backup-CARoleService c:\adcsbackup1-Force|
|Inkrementell|-Ausführen einer inkrementellen Sicherung<p>Backup-CARoleService c:\adcsbackup7-inkrementell|
|-Keeplog|1. weist den Befehl an, Protokolldateien beizubehalten. Wenn der Schalter nicht angegeben ist, werden Protokolldateien mit Ausnahme des inkrementellen Szenarios standardmäßig abgeschnitten.<p>Backup-CARoleService c:\adcsbackup7-keeplog|

### <a name="-password-secure-string"></a>-Kennwort <Secure String>
Wenn der-password-Parameter verwendet wird, muss das angegebene Kennwort eine sichere Zeichenfolge sein.  Verwenden Sie das Cmdlet " **Read-Host** ", um eine interaktive Eingabeaufforderung für einen sicheren Kenn Wort Eintrag zu starten, oder verwenden Sie das Cmdlet **ConvertTo-SecureString** , um das Kennwort Inline anzugeben.

Überprüfen Sie die folgenden Beispiele.

**Angeben einer sicheren Zeichenfolge für den password-Parameter mithilfe von "Read-Host"**

```powershell
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)
```

**Angeben einer sicheren Zeichenfolge für den password-Parameter mithilfe von ConvertTo-SecureString**

```powershell
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)
```

## <a name="restore-caroleservice"></a>Restore-CARoleService
**Adcsadministration-Cmdlet: Restore-caroleservice**

|Argumente: **Fett** formatierte Argumente sind erforderlich.|Beschreibung|
|------------------------------------------------|---------------|
|**-Path**|-String-Speicherort für die Wiederherstellung der Sicherung<br />: Dies ist der einzige unbenannte Parameter.<br />-Positions Parameter<p>**Beispiel:**<p>Restore-caroleservice.-Path c:\adcsbackup1-Force<p>Restore-CARoleService c:\adcsbackup2-Force|
|-Keyonly|-Das Zertifizierungsstellen Zertifikat ohne die Datenbank wiederherstellen<br />-Muss angegeben werden, wenn die Sicherung mit der Option-keyonly erstellt wurde.<p>**Beispiel:**<p>Restore-CARoleService c:\adcsbackup3-keyonly-Force|
|-Password|: Hiermit wird das Kennwort der Zertifizierungsstellen Zertifikate und privaten Schlüssel angegeben.<br />-Muss eine sichere Zeichenfolge sein.<p>**Beispiel:**<p>Restore-CARoleService c:\adcsbackup4-Password (Read-Host-prompt "Password:"-assecurestring)-Force<p>Restore-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -Asplaintext-Force)-Force|
|-DatabaseOnly|-Wiederherstellen der Datenbank ohne das Zertifizierungsstellen Zertifikat<p>Restore-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|: Hiermit können Sie die bereits vorhandenen Schlüssel überschreiben.<br />-Ist ein optionaler Parameter, aber wenn er direkt wieder hergestellt wird, ist es wahrscheinlich erforderlich<p>Restore-CARoleService c:\adcsbackup1-Force|

### <a name="issues"></a>Probleme
Eine nicht Kenn Wort geschützte Sicherung wird durchgeführt, wenn die ConvertTo-SecureString Funktion bei Verwendung des-Backup-CARoleService mit dem-password-Parameter fehlschlägt.

![Sichern und Wiederherstellen von Zertifizierungsstellen](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)

**Table-Tabelle, \\ \* Arabisch, 18: häufige Fehler**

|Aktion|Fehler|Kommentar|
|----------|---------|-----------|
|**Restore-caroleservice c:\adcsbackup**|Restore-CARoleService: der Prozess kann nicht auf die Datei zugreifen, da Sie von einem anderen Prozess verwendet wird. (Ausnahme von HRESULT:<p>0x80070020|Vor dem Ausführen des Restore-CARoleService-Cmdlets den Active Directory Certificate Services-Dienst abbrechen|
|**Restore-caroleservice c:\adcsbackup**|Restore-CARoleService: das Verzeichnis ist nicht leer. (Ausnahme von HRESULT: 0x80070091)|Verwenden Sie den Parameter-Force, um bereits vorhandene Schlüssel zu überschreiben.|
|**Backup-caroleservice c:\adcsbackup-Password (Read-Host-prompt "Password:"-assecurestring)-DatabaseOnly**|Backup-CARoleService: der Parameter Satz kann mit den angegebenen benannten Parametern nicht aufgelöst werden.|Der Parameter "-Password" wird nur zum Kenn Wort Schutz für private Schlüssel verwendet und ist daher ungültig, wenn Sie Sie nicht sichern.|
|**Restore-caroleservice c:\adcsback15-Password (Read-Host-prompt "Password:"-assecurestring)-DatabaseOnly**|Restore-CARoleService: der Parameter Satz kann mit den angegebenen benannten Parametern nicht aufgelöst werden.|Der Parameter "-Password" wird nur zum Kenn Wort Schutz für private Schlüssel verwendet und ist daher ungültig, wenn Sie nicht wieder hergestellt werden.|
|**Restore-caroleservice c:\adcsback14-Password (Read-Host-prompt "Password:"-assecurestring)**|Restore-CARoleService: das System kann die angegebene Datei nicht finden. (Ausnahme von HRESULT: 0x80070002)|Der angegebene Pfad enthält keine gültige Datenbanksicherung.  Der Pfad ist möglicherweise ungültig, oder die Sicherung wurde mit der Option "-keysonly" erstellt?|

## <a name="additional-resources"></a>Weitere Ressourcen
[Active Directory Certificate Services Migration Guide](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10)) (Migrationshandbuch für die Active Directory-Zertifikatdienste)

[Sichern der Datenbank der Zertifizierungsstelle und des privaten Schlüssels](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_BackUpDB)

[Wiederherstellen der Datenbank und Konfiguration der Zertifizierungsstelle auf dem Zielserver](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_RestoreCA)

## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Versuchen Sie Folgendes: Sichern Sie die Zertifizierungsstelle in Ihrem Lab mithilfe von Windows PowerShell.

1.  Verwenden Sie die Befehle in dieser Lektion, um die Datenbank und den privaten Schlüssel, die mit einem Kennwort geschützt sind, zu sichern.

2.  Halten Sie die Wiederherstellung der Zertifizierungsstelle zu diesem Zeitpunkt an.

