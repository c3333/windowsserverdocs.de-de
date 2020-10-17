---
title: Ausgehende Messung im virtuellen Netzwerk
description: 'Ein grundlegender Aspekt der cloudantennetzwerkmonetarisierung ist die ausgehende Netzwerkbandbreite. Beispiel: ausgehende Datenübertragungen in Microsoft Azure Geschäftsmodell. Ausgehende Daten werden basierend auf der Gesamtmenge der Daten berechnet, die aus den Azure-Rechenzentren über das Internet in einem bestimmten Abrechnungszeitraum verschoben werden.'
manager: grcusanz
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: a9e939b4a810848e91b5d2cb8e4b878bbcf56e84
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156468"
---
# <a name="egress-metering-in-a-virtual-network"></a>Messen des ausgehenden Datenverkehrs in einem virtuellen Netzwerk

>Gilt für: Windows Server 2019


Ein grundlegender Aspekt der cloudantennetzwerkmonetarisierung ist die Abrechnung nach der Auslastung der Netzwerkbandbreite. Ausgehende Daten werden basierend auf der Gesamtmenge der Daten berechnet, die über das Internet in einem bestimmten Abrechnungszeitraum aus dem Rechenzentrum verschoben werden.

Die ausgehende Messung des SDN-Netzwerk Datenverkehrs in Windows Server 2019 ermöglicht das anbieten von Nutzungs Zählern für ausgehende Datenübertragungen. Netzwerk Datenverkehr, der die einzelnen virtuellen Netzwerke verlässt, aber innerhalb des Rechenzentrums verbleibt, kann einzeln nachverfolgt werden, sodass Sie von Abrechnungs Berechnungen ausgeschlossen werden können. An Ziel-IP-Adressen gebundene Pakete, die nicht in einem der nicht berechneten Adressbereiche enthalten sind, werden als in Rechnung gestellte ausgehende Datenübertragungen nachverfolgt.

## <a name="virtual-network-unbilled-address-ranges-allowlist-of-ip-ranges"></a>Nicht berechnete Adressbereiche des virtuellen Netzwerks (AllowList von IP-Bereichen)

Sie finden nicht berechnete Adressbereiche unter der **unbilledaddressranges** -Eigenschaft eines vorhandenen virtuellen Netzwerks. Standardmäßig werden keine Adressbereiche hinzugefügt.

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

Ihre Ausgabe sieht in etwa wie folgt aus:
   ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  :
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Beispiel: Verwalten der nicht berechneten Adressbereiche eines virtuellen Netzwerks

Sie können den Satz von IP-Subnetzpräfixen, die von der Abrechnungs Messung in Rechnung ausgeschlossen werden sollen, durch Festlegen der **unbilledaddressrange** -Eigenschaft eines virtuellen Netzwerks verwalten.  Sämtlicher Datenverkehr, der von Netzwerkschnittstellen im virtuellen Netzwerk mit einer Ziel-IP-Adresse gesendet wird, die mit einem der Präfixe übereinstimmt, ist nicht in der billedegressbytes-Eigenschaft enthalten.

1.  Aktualisieren Sie die **unbilledaddressranges** -Eigenschaft so, dass Sie die Subnetze enthält, für die kein Zugriff in Rechnung gestellt wird.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Wenn Sie mehrere IP-Subnetze hinzufügen, verwenden Sie ein Komma zwischen jedem der IP-Subnetze.  Fügen Sie keine Leerzeichen vor oder nach dem Komma ein.

2.  Aktualisieren Sie die Virtual Network Ressource mit der geänderten **unbilledaddressranges** -Eigenschaft.

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    Ihre Ausgabe sieht in etwa wie folgt aus:
      ```
         Confirm
         Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
         'Microsoft.Windows.NetworkController.VirtualNetwork' via
         'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
         [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


         Tags             :
         ResourceRef      : /virtualNetworks/VNet1
         InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
         Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
         ResourceMetadata :
         ResourceId       : VNet1
         Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
      ```


3. Überprüfen Sie die Virtual Network, um die konfigurierten **unbilledaddressranges**anzuzeigen.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   Ihre Ausgabe sieht nun in etwa wie folgt aus:
   ```
   AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
   DhcpOptions            :
   UnbilledAddressRanges  : 10.10.2.0/24,192.168.2.0/24
   ConfigurationState     :
   ProvisioningState      : Succeeded
   Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                        29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
   VirtualNetworkPeerings :
   EncryptionCredential   :
   LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Überprüfen der kostenpflichtigen ausgehenden Nutzung eines virtuellen Netzwerks

Nachdem Sie die **unbilledaddressranges** -Eigenschaft konfiguriert haben, können Sie die in Rechnung gestellte und nicht in Rechnung gestellte ausgehende Nutzung der einzelnen Subnetze innerhalb eines virtuellen Netzwerks überprüfen. Der ausgehende Datenverkehr wird alle vier Minuten durch die Gesamtanzahl der Bytes der in Rechnung gestellten und nicht berechneten Bereiche aktualisiert.

Die folgenden Eigenschaften sind für jedes virtuelle Subnetz verfügbar:

-   **Unbilledegressbytes** zeigt die Anzahl der nicht in Rechnung gestellten Bytes an, die von Netzwerkschnittstellen gesendet werden, die mit diesem virtuellen Subnetz verbunden sind. Nicht berechnete Bytes sind bytes, die an Adressbereiche gesendet werden, die Teil der **unbilledaddressranges** -Eigenschaft des übergeordneten virtuellen Netzwerks sind.

-   **Billedegressbytes** zeigt die Anzahl der in Rechnung gestellten Bytes an, die von den mit diesem virtuellen Subnetz verbundenen Netzwerkschnittstellen gesendet werden. In Rechnung gestellte Bytes werden an Adressbereiche gesendet, die nicht Teil der **unbilledaddressranges** -Eigenschaft des übergeordneten virtuellen Netzwerks sind.

Verwenden Sie das folgende Beispiel, um die ausgehende Verwendung abzufragen:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

Ihre Ausgabe sieht in etwa wie folgt aus:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---
