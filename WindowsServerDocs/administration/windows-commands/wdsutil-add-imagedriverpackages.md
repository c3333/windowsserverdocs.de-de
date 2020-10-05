---
title: WDSUTIL Add-imagedriverpackages
description: Referenz Artikel zu WDSUTIL Add-imagedriverpackages, mit dem Treiber Pakete aus dem Treiber Speicher zu einem Start Abbild hinzugefügt werden.
ms.topic: reference
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2b8abcf119d43bd0f524f92c29219ad7e6027941
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730440"
---
# <a name="wdsutil-add-imagedriverpackages"></a>WDSUTIL Add-imagedriverpackages

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Hinzufügen von Treiber Paketen aus dem Treiber Speicher zu einem Start Abbild. Die Image Version muss Windows 7 oder Windows Server 2008 R2 oder höher sein.

## <a name="syntax"></a>Syntax
```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64}
[/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
### <a name="parameters"></a>Parameter

|                                         Parameter                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                   Servers<Server name>                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Gibt den Namen des Servers an. Dabei kann es sich um den NetBIOS-Namen oder den voll qualifizierten Namen handeln. Wenn kein Servername angegeben ist, wird der lokale Server verwendet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                     Medien<Image name>                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Gibt den Namen des Bilds an, dem der Treiber hinzugefügt werden soll.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                                       MediaType: Boot                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  Gibt den Typ des Bilds an, dem der Treiber hinzugefügt wird. Treiber Pakete können nur zu Start Abbildern hinzugefügt werden.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                         /Architecture: {x86 &#124; ia64 &#124; x64}                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Gibt die Architektur des Start Abbilds an. Da es möglich ist, den gleichen Image Namen für Start Images in verschiedenen Architekturen zu verwenden, sollten Sie die Architektur angeben, um sicherzustellen, dass das richtige Image verwendet wird.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                                   Einfügen<File name>                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Gibt den Dateinamen an. Wenn das Bild nicht anhand des Namens eindeutig identifiziert werden kann, muss der Dateiname angegeben werden.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                 Filter Type<Filter type>                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     Gibt das Attribut des Treiber Pakets an, nach dem gesucht werden soll. Sie können mehrere Attribute in einem einzelnen Befehl angeben. Sie müssen auch **/Operator** und **/value** mit dieser Option angeben.<p><Filter type> kann eines der folgenden sein:<p>**PackageId**<p>**PackageName**<p>**Packageaktivierte**<p>**Packagedateadded**<p>**Packageingeffilename**<p>**Packageclass**<p>**PackageProvider**<p>**PackageArchitecture**<p>**Packagelocale**<p>**Packagesigned**<p>**Packagedateveröffentlicht**<p>**Packageversion**<p>**Driverdescription**<p>**Driverhersteller**<p>**Driverhardwareid**<p>**Drivercompatibleid**<p>**Driverexcludeid**<p>**Drivergroupid**<p>**Drivergroupname**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| /Operator: {Equal &#124; NotEqual &#124; greaterOrEqual &#124; lessOrEqual &#124; enthält} |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Die Beziehung zwischen dem Attribut und den Werten. Sie können nur " **enthält** "-Attribute mit Zeichen folgen Attributen angeben. Sie können nur " **greaterOrEqual** " und " **lessOrEqual** " mit Datums-und Versions Attributen angeben.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|                                       Wert<Value>                                       | Gibt den Wert an, der relativ zum angegebenen gesucht werden soll <attribute> . Sie können mehrere Werte für ein einzelnes **/FilterType**angeben. Die nachstehende Liste enthält die Attribute, die Sie für die einzelnen Filter angeben können. Weitere Informationen zu diesen Attributen finden Sie unter [Treiber-und Paket Attribute](https://go.microsoft.com/fwlink/?LinkId=166895) ( <https://go.microsoft.com/fwlink/?LinkId=166895> ).<p>-PackageID-geben Sie eine gültige GUID an. Beispiel: {4D36E972-E325-11CE-BFC1-08002BE10318}.<br />-PackageName geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Packageaktivi-geben Sie **Ja** oder **Nein**an.<br />-Packagedateadded-geben Sie das Datum im folgenden Format an: yyyy/mm/dd<br />-Packageinffilename geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Packageclass: Geben Sie einen gültigen Klassennamen oder eine Klassen-GUID an. Beispiel: **diskdrive**, **net**oder {4D36E972-E325-11CE-BFC1-08002BE10318}.<br />-Packageprovider geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Packagearchitecture: Geben Sie **x86**,  **x64**oder **ia64**an. <b /> -Packagelocale-geben Sie einen gültigen sprach Bezeichner an. Beispiel: **en-US** oder **es-es**.<br />-Packagesigned-geben Sie **Ja** oder **Nein**an.<br />-Packagedatepublished-geben Sie das Datum im folgenden Format an: yyyy/mm/dd<br />-Packageversion: Geben Sie die Version im folgenden Format an: a.b.x.y. Beispiel: 6.1.0.0<br />-Driverdescription geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Drivermanufacturer geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Driverhardwareid-geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Drivercompatibleid: Geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Driverexcludeid-geben Sie einen beliebigen Zeichen folgen Wert an.<br />-Drivergroupid: Geben Sie eine gültige GUID an. Beispiel: {4D36E972-E325-11CE-BFC1-08002BE10318}.<br />-Drivergroupname geben Sie einen beliebigen Zeichen folgen Wert an. |

## <a name="examples"></a>Beispiele
Zum Hinzufügen von Treiber Paketen zu einem Start Abbild geben Sie eine der folgenden Informationen ein:
```
wdsutil /add-ImageDriverPackagemedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```
```
wdsutil /verbose /add-ImageDriverPackagemedia: WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
```
wdsutil /verbose /add-ImageDriverPackagemedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```
## <a name="additional-references"></a>Zusätzliche Referenzen
- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
- [WDSUTIL-Befehl "Add-imagedriverpackage"](wdsutil-add-imagedriverpackage.md)
- [WDSUTIL-Befehl "Add-AllDriverPackages"](wdsutil-add-alldriverpackages.md)