---
title: WDSUTIL-Add-Device
description: Referenz Artikel zu WDSUTIL Add-Device, der einen Computer in den Active Directory-Domänen Diensten vorab bereitstellt. Vorab bereitgestellte Computer werden auch als bekannte Computer bezeichnet.
ms.topic: reference
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7d87b34028f841340eeaf294a18784c757467b29
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730467"
---
# <a name="wdsutil-add-device"></a>WDSUTIL-Add-Device

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vorab Stufen eines Computers in den Active Directory-Domänen Diensten. Vorab bereitgestellte Computer werden auch als bekannte Computer bezeichnet. Dadurch können Sie Eigenschaften konfigurieren, um die Installation für den Client zu steuern. Beispielsweise können Sie das Netzwerkstart Programm und die Datei für die unbeaufsichtigte Installation konfigurieren, die vom Client empfangen werden soll, sowie den Server, von dem der Client das Netzwerkstart Programm herunterladen soll.

## <a name="syntax"></a>Syntax
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>]
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
### <a name="parameters"></a>Parameter
|Parameter|BESCHREIBUNG|
|-------|--------|
|Schutz<computer name>|Gibt den Namen des hinzu zufügenden Computers an.|
|/ID: <UUID &#124; Mac-Adresse>|Gibt entweder die GUID/UUID oder die Mac-Adresse des Computers an. Eine GUID/UUID muss in einem von zwei Formaten eine binäre Zeichenfolge oder eine GUID-Zeichenfolge sein. Beispiel:<p>Binäre Zeichenfolge: **/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6**<p>GUID-Zeichenfolge: **/ID: E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<p>Eine Mac-Adresse muss folgendes Format aufweisen: **00b056882(** keine Bindestriche) oder **00-B0-56-88-2F-DC** (mit Bindestrichen)|
|[/ReferralServer: <Server name> ]|Gibt den Namen des Servers an, für den eine Verbindung hergestellt werden soll, um das Netzwerkstart Programm und das Start Abbild mit Trivial File Transfer Protocol (TFTP) herunterzuladen.|
|[/BootProgram: <Relative path> ]|Gibt den relativen Pfad vom Ordner RemoteInstall zum Netzwerkstart Programm an, das von diesem Computer empfangen werden soll. Beispiel: boot\x86\pxeboot.com|
|[/WdsClientUnattend: <Relative path> ]|Gibt den relativen Pfad vom Ordner RemoteInstall zur unbeaufsichtigten Installationsdatei an, mit der die Installations Bildschirme des Windows-Bereitstellungsdiensteclient automatisiert werden.|
|[/User: <Domäne \ Benutzer &#124; User@Domain>]|Legt Berechtigungen für das Computer Konto Objekt fest, um dem angegebenen Benutzer die erforderlichen Rechte zum Hinzufügen des Computers zur Domäne zu übergeben.|
|[/JoinRights: {joinonly &#124; Full}]|Gibt den Typ der Rechte an, die dem Benutzer zugewiesen werden sollen.<p>-   Für **joinonly** muss der Administrator das Computer Konto zurücksetzen, bevor der Benutzer den Computer der Domäne hinzufügen kann.<br />-   **Full** bietet vollen Zugriff auf den Benutzer, der das Recht zum Hinzufügen des Computers zur Domäne umfasst.|
|[/JoinDomain: {yes &#124; No}]|Gibt an, ob der Computer während der Installation des Betriebssystems mit der Domäne verknüpft werden soll oder nicht. Der Standardwert ist **Ja**.|
|[/BootImagepath: <Relative path> ]|Gibt den relativen Pfad vom Ordner RemoteInstall zum Start Abbild an, das von diesem Computer verwendet werden soll.|
|[/Ou: <DN of OU> ]|Der Distinguished Name der Organisationseinheit, in der das Computer Konto Objekt erstellt werden soll. Beispiel: **OU = MyOU, CN = Test, DC = Domain, DC = com**. Der Standard Speicherort ist der Container des Standard Computers.|
|[/Domain: <Domain> ]|Die Domäne, in der das Computer Konto Objekt erstellt werden soll. Der Standard Speicherort ist die lokale Domäne.|
## <a name="examples"></a>Beispiele
Wenn Sie einen Computer mit einer Mac-Adresse hinzufügen möchten, geben Sie Folgendes ein:
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
Um einen Computer mithilfe einer GUID-Zeichenfolge hinzuzufügen, geben Sie Folgendes ein:
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```
## <a name="additional-references"></a>Zusätzliche Referenzen
- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
- [WDSUTIL-Befehl "Get-alldevices"](wdsutil-get-alldevices.md)
- [WDSUTIL-Befehl "Get-Device"](wdsutil-get-device.md)
- [WDSUTIL Set-Device-Befehl](wdsutil-set-device.md)
- [New-wdsclient](/previous-versions/windows/powershell-scripting/dn283430(v=wps.630))
