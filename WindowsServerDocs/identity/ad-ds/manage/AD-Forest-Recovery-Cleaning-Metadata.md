---
title: 'AD-Gesamtstruktur Wiederherstellung: Bereinigen von Metadaten entfernter DCS'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.openlocfilehash: 6ae65803abc9120c781ad7131d30072b9d70131b
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067942"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>AD-Gesamtstruktur Wiederherstellung: Bereinigen von Metadaten entfernter Beschreib barer Domänen Controller

>Gilt für: Windows Server 2016, Windows Server 2012 und 2012 R2, Windows Server 2008 und 2008 R2

Bei der Metadatenbereinigung werden Active Directory Daten entfernt, die einen Domänen Controller zum Replikationssystem identifizieren

Mithilfe des folgenden Verfahrens können Sie die DC-Objekte für DCS löschen, die Sie wieder dem Netzwerk hinzufügen möchten, indem Sie AD DS neu installieren.

Wenn Sie die-Version von Active Directory Benutzer und Computer oder Active Directory Websites und-Dienste verwenden, die Remoteserver-Verwaltungstools (RSAT) enthalten sind, wird die Metadatenbereinigung automatisch ausgeführt, wenn Sie ein DC-Objekt löschen.

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Löschen eines Domänen Controllers mit Active Directory Benutzern und Computern

Wenn Sie die-Version von Active Directory Benutzer und Computer oder Active Directory-Verwaltungscenter in Remoteserver-Verwaltungstools (RSAT) verwenden, wird die Metadatenbereinigung automatisch ausgeführt, wenn Sie das DC-Objekt löschen. Das Server Objekt und das Computer Objekt werden ebenfalls automatisch gelöscht.

Als Alternative können Sie auch Active Directory Websites und Dienste in RSAT verwenden, um ein DC-Objekt zu löschen. Wenn Sie Active Directory Websites und Dienste verwenden, müssen Sie das zugehörige Server Objekt und das NTDS-Einstellungs Objekt löschen, bevor Sie das DC-Objekt löschen können.

Weitere Informationen zum Installieren von RSAT finden Sie im Artikel [Remoteserver-Verwaltungstools](../../../remote/remote-server-administration-tools.md).

Das folgende Verfahren ist für DCS, die entweder Windows Server 2016, 2012, 2008 R2 oder 2008 ausführen, identisch. Der Ziel-DC des Metadatenbereinigungs Vorgangs kann eine beliebige Version von Windows Server ausführen.

### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>So löschen Sie ein Domänen Controller Objekt mit Active Directory Benutzern und Computern in RSAT

1. Klicken Sie auf **Start** , zeigen Sie auf **Verwaltung** , und klicken Sie dann auf **Active Directory-Benutzer und -Computer** .
2. Doppelklicken Sie in der Konsolen Struktur auf den Domänen Container, und doppelklicken Sie dann auf die Organisationseinheit der **Domänen Controller** .
3. Klicken Sie im Detailfenster mit der rechten Maustaste auf den Domänen Controller, den Sie löschen möchten, und klicken Sie dann auf **Löschen** .
   ![Löschen](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png)
4. Klicken Sie zum Bestätigen des Löschvorgangs auf **Ja** . Wählen Sie die Option **dieser Domänen Controller ist dauerhaft offline und kann nicht mehr mithilfe des Kontrollkästchens Assistent zum Installieren von Active Directory Domain Services (Dcpromo) herabgestuft werden** aus, und klicken Sie auf **Löschen** .
5. Wenn der Domänen Controller ein globaler Katalogserver war, klicken Sie auf **Ja** , um den Löschvorgang zu bestätigen.

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellung der AD-Gesamtstruktur: Leitfaden](AD-Forest-Recovery-Guide.md)
- [Wiederherstellung der AD-Gesamtstruktur: Verfahren](AD-Forest-Recovery-Procedures.md)
