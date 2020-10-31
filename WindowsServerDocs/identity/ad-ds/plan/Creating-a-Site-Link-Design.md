---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Erstellen eines Entwurfs für Standortverknüpfungen
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 390cc0d69fa4d43a957500c0078d53dcdc69c10c
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068772"
---
# <a name="creating-a-site-link-design"></a>Erstellen eines Entwurfs für Standortverknüpfungen

> Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Erstellen Sie einen Standort Verknüpfungs Entwurf, um Ihre Standorte mit Standort Verknüpfungen zu verbinden. Standort Verknüpfungen reflektieren die standortübergreifende Konnektivität und Methode zum Übertragen von Replikations Datenverkehr. Sie müssen Standorte mit Standort Verknüpfungen verbinden, damit die Domänen Controller an jedem Standort Active Directory Änderungen replizieren können.

## <a name="connecting-sites-with-site-links"></a>Verbinden von Standorten mit Standortverknüpfungen

Um Standorte mit Standort Verknüpfungen zu verbinden, geben Sie die Mitgliedsstandorte an, die Sie mit der Standort Verknüpfung verbinden möchten, erstellen Sie ein Standort Verknüpfungs Objekt im jeweiligen Container Inter-Site Transports, und benennen Sie die Standort Verknüpfung. Nachdem Sie die Standort Verknüpfung erstellt haben, können Sie mit dem Festlegen der Standort Verknüpfungs Eigenschaften fortfahren.

Wenn Sie Standort Verknüpfungen erstellen, stellen Sie sicher, dass jeder Standort in einer Standort Verknüpfung enthalten ist. Stellen Sie darüber hinaus sicher, dass alle Standorte über andere Standort Verknüpfungen miteinander verbunden sind, damit die Änderungen von Domänen Controllern an allen Standorten repliziert werden können. Wenn Sie dies nicht tun, wird im Verzeichnisdienst Ereignisanzeige Protokoll eine Fehlermeldung mit dem Hinweis generiert, dass die Standort Topologie nicht verbunden ist.

Wenn Sie Websites zu einer neu erstellten Standort Verknüpfung hinzufügen, stellen Sie fest, ob der hinzugefügte Standort Mitglied anderer Standort Verknüpfungen ist, und ändern Sie bei Bedarf die Standort Verknüpfungs Mitgliedschaft des Standorts. Wenn Sie z. b. einen Standort als Mitglied der Standard-First-Site-Verknüpfung festlegen, wenn Sie den Standort erstmalig erstellen, achten Sie darauf, dass Sie den Standort aus dem Standard-First-Site-Link entfernen, nachdem Sie den Standort einer neuen Standort Verknüpfung hinzugefügt haben. Wenn Sie den Standort nicht aus dem Standard-First-Site-Link entfernen, trifft die Konsistenzprüfung (KCC) Routing Entscheidungen auf Grundlage der Mitgliedschaft beider Standort Verknüpfungen, was zu einem falschen Routing führen kann.

Verwenden Sie die Liste der Standorte und verknüpften Speicherorte, die Sie im Arbeitsblatt "geografische Standorte und Kommunikations Links" (DSSTOPO_1.doc) notiert haben, um die Mitgliedsstandorte zu identifizieren, die Sie mit einer Standort Verknüpfung verbinden möchten. Wenn mehrere Standorte die gleiche Konnektivität und Verfügbarkeit aufweisen, können Sie Sie mit derselben Standort Verknüpfung verbinden.

Der Container für Inter-Site Transporte bietet die Möglichkeit zum Mapping von Standort Verknüpfungen zu dem Transport, den der Link verwendet. Wenn Sie ein Standort Verknüpfungs Objekt erstellen, erstellen Sie es entweder in dem IP-Container, der die Standort Verknüpfung mit dem Remote Prozedur Aufruf (RPC) über den IP-Transport verknüpft, oder den Simple Mail Transfer Protocol (SMTP)-Container, der die Standort Verknüpfung dem SMTP-Transport zuordnet.

> [!NOTE]
> Die SMTP-Replikation wird in zukünftigen Versionen von Active Directory Domain Services (AD DS) nicht unterstützt. Daher wird das Erstellen von Standort Verknüpfungs Objekten im SMTP-Container nicht empfohlen.

Wenn Sie ein Standort Verknüpfungs Objekt in der jeweiligen Inter-Site Transports-Container erstellen, verwendet AD DS RPC-over-IP, um die standortübergreifende und Standort interne Replikation zwischen Domänen Controllern zu übertragen. Um Daten während der Übertragung sicher zu halten, verwendet die RPC-over-IP-Replikation sowohl das Kerberos-Authentifizierungsprotokoll als auch die Datenverschlüsselung

Wenn keine direkte IP-Verbindung verfügbar ist, können Sie die Replikation Zwischenstand Orten für die Verwendung von SMTP konfigurieren. Die SMTP-Replikations Funktionalität ist jedoch begrenzt und erfordert eine Unternehmens Zertifizierungsstelle (Certification Authority, ca). SMTP kann nur die Konfigurations-, Schema-und Anwendungsverzeichnis Partitionen replizieren. die Replikation von Domänen Verzeichnis Partitionen wird nicht unterstützt.

Verwenden Sie zum Benennen von Standort Verknüpfungen ein konsistentes Benennungs Schema, z. b. name_of_site1-name_of_site2. Notieren Sie die Liste der Standorte, verknüpften Sites und die Namen der Standort Verknüpfungen, die diese Standorte in einem Arbeitsblatt verbinden. Ein Arbeitsblatt, das Sie beim Aufzeichnen von Standortnamen und verknüpften Standort Verknüpfungs Namen unterstützt, finden Sie unter [Auftrags Hilfen für das Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608), herunterladen Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip und Öffnen von "Websites und zugeordnete Standort Verknüpfungen" (DSSTOPO_5.doc).

## <a name="in-this-guide"></a>Inhalt dieser Anleitung

[Festlegen von Standortverknüpfungseigenschaften](Setting-Site-Link-Properties.md)
