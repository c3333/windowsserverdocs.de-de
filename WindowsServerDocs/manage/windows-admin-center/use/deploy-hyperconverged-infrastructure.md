---
title: Bereitstellen hyperkonvergierter Infrastrukturen mit dem Windows Admin Center
description: Stellen Sie eine hyperkonvergierte Infrastruktur mit dem Windows Admin Center bereit.
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.date: 11/04/2019
ms.openlocfilehash: 1f8e8a5433b986333975f45009819a98d90b61fc
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517506"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>Bereitstellen hyperkonvergierter Infrastrukturen mit dem Windows Admin Center

> Gilt für: Windows Admin Center, Windows Admin Center (Vorschauversion)

Sie können Windows Admin Center, [Version 1910](../overview.md) oder höher, verwenden, um eine hyperkonvergierte Infrastruktur mithilfe von mindestens zwei passenden Windows-Servern bereitzustellen. Dieses neue Feature ist ein mehrstufiger Workflow, der Sie durch die Installation von Features, das Konfigurieren von Netzwerken, das Erstellen des Clusters und das Bereitstellen von direkte Speicherplätze und/oder Software-Defined Networking (SDN) (sofern ausgewählt) führt.

Ab Version 2007 des Windows Admin Centers unterstützt das Windows Admin Center die Azure Stack HCI-Betriebssystem. Weitere Informationen zum Bereitstellen [eines Clusters im Windows Admin Center finden Sie in der Azure Stack HCI](/azure-stack/hci/get-started)-Dokumentation. Obwohl sich diese Dokumentation auf Azure Stack HCI konzentriert, gelten die Anweisungen auch größtenteils für Windows Server-bereit Stellungen.

## <a name="undo-and-start-over"></a>Rückgängig machen und beginnen

Verwenden Sie diese Windows PowerShell-Cmdlets, um vom Workflow vorgenommene Änderungen rückgängig zu machen und zu beginnen.

### <a name="remove-virtual-machines-or-other-clustered-resources"></a>Entfernen von virtuellen Computern oder anderen geclusterten Ressourcen

Wenn Sie virtuelle Computer oder andere geclusterte Ressourcen erstellt haben, z. b. die Netzwerk Controller für Software-Defined Networking, entfernen Sie Sie zuerst.

Verwenden Sie z. b. Folgendes, um Ressourcen nach dem Namen zu entfernen:

```PowerShell
Get-ClusterResource -Name "<NAME>" | Remove-ClusterResource
```

### <a name="undo-the-storage-steps"></a>Rückgängig machen der Speicher Schritte

Wenn Sie direkte Speicherplätze aktiviert haben, deaktivieren Sie dieses Skript:

> [!Warning]
> Mit diesen Cmdlets werden alle Daten in direkte Speicherplätze Volumes dauerhaft gelöscht. Dies kann nicht rückgängig gemacht werden.

```PowerShell
Get-VirtualDisk | Remove-VirtualDisk
Get-StoragePool -IsPrimordial $False | Remove-StoragePool
Disable-ClusterS2D
```

### <a name="undo-the-clustering-steps"></a>Rückgängig machen der Clustering-Schritte

Wenn Sie einen Cluster erstellt haben, entfernen Sie ihn mit diesem Cmdlet:

```PowerShell
Remove-Cluster -CleanUpAD
```

Um auch Cluster Validierungs Berichte zu entfernen, führen Sie dieses Cmdlet auf jedem Server aus, der Teil des Clusters war:

```PowerShell
Get-ChildItem C:\Windows\cluster\Reports\ | Remove-Item
```

### <a name="undo-the-networking-steps"></a>Rückgängigmachen der Netzwerk Schritte

Führen Sie diese Cmdlets auf jedem Server aus, der Teil des Clusters war.

Wenn Sie einen virtuellen Hyper-V-Switch erstellt haben:

```PowerShell
Get-VMSwitch | Remove-VMSwitch
```

> [!Note]
> `Remove-VMSwitch`Mit dem-Cmdlet werden alle virtuellen Adapter automatisch entfernt, und der Switch-Embedded-Team Vorgang physischer Adapter wird rückgängig machen.

Wenn Sie die Eigenschaften des Netzwerkadapters geändert haben, z. b. Name, IPv4-Adresse und VLAN-ID:

> [!Warning]
> Mit diesen Cmdlets werden Netzwerkadapter Namen und IP-Adressen entfernt. Stellen Sie sicher, dass Sie über die erforderlichen Informationen verfügen, um eine Verbindung herzustellen, z. b. einen Adapter für die Verwaltung, der aus dem folgenden Skript ausgeschlossen wird. Stellen Sie außerdem sicher, dass Sie wissen, wie die Server in Bezug auf physische Eigenschaften wie Mac-Adresse, nicht nur den Namen des Adapters in Windows verbunden sind.

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

Sie sind jetzt bereit, um den Workflow zu starten.

## <a name="additional-references"></a>Zusätzliche Referenzen

- [Hallo, Windows Admin Center](../overview.md)
- [Bereitstellen von direkten Speicherplätzen](../../../storage/storage-spaces/deploy-storage-spaces-direct.md)
