---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 'Prüfliste: Konfigurieren von Legacy-AD FS für die Nutzung von Ansprüchen von AD FS 1. x'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 0d27f72aee5289b0ce8dc63aa31c2490d0bf2a62
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969777"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Prüfliste: Konfigurieren von AD FS für die Nutzung von Ansprüchen von AD FS 1. x


## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Prüfliste: Konfigurieren von AD FS für die Nutzung von Ansprüchen von AD FS 1. x
Diese Prüfliste enthält die Aufgaben, die zum Konfigurieren Ihrer Active Directory-Verbunddienste (AD FS) \( AD FS \) Verbunddienst in Windows Server 2012 erforderlich sind, um Ansprüche zu nutzen, die von einem AD FS 1 gesendet werden.* x* -Verbunddienst.

> [!NOTE]
> Führen Sie die Aufgaben in dieser Prüfliste in der angegebenen Reihenfolge aus. Wenn Sie über einen Referenzlink zu einer Prozedur gelangen, kehren Sie zu diesem Thema zurück, nachdem Sie die Schritte in der betreffenden Prozedur ausgeführt haben, sodass Sie die Ausführung der verbleibenden Aufgaben in dieser Prüfliste fortsetzen können.

![Verwenden von Ansprüchen aus AD FS Prüfliste](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: Konfigurieren von AD FS für die Nutzung von Ansprüchen von AD FS 1. x**

|Aufgabe|Verweis|
|--------|-------------|
|Planen Sie die Interoperabilität zwischen AD FS in Windows Server 2012 und früheren Versionen von AD FS, und erfahren Sie mehr über den Anspruchstyp "Name ID".|![beanspruchen von Ansprüchen aus AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Planen der Interoperabilität mit AD FS 1. x](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
| Bevor Sie mit einer früheren Version von AD FS zusammenarbeiten können, müssen Sie zunächst eine Anspruchs Anbieter-Vertrauensstellung in der AD FS Verbunddienst erstellen. **Hinweis:** Es ist nicht möglich, eine Vertrauensstellung mit einem AD FS 1 zu erstellen. *x* Verbunddienst mithilfe von Verbund Metadaten.<p>Wenn Sie die Vertrauensstellung mithilfe des Verfahrens im Link auf der rechten Seite einrichten, müssen Sie im Assistenten zum Hinzufügen von Anspruchs Anbieter-Vertrauens Stellungen die folgenden Schritte ausführen, um diese Vertrauensstellung für die Zusammenarbeit mit einem AD FS 1 einzurichten. *x* -Verbunddienst:<p>1. Wählen Sie auf der Seite **Datenquelle auswählen** die Option **Daten über die Vertrauensstellung der vertrauenden Seite manuell eingeben**aus.<br />2. Wählen Sie auf der Seite **Profil auswählen** die Option **AD FS 1,0-und 1,1-Profil**aus.<br />3. Geben Sie auf der Seite **URL konfigurieren** unter URL des ** \- passiven WS**-Verbunds die **Verbunddienst Endpunkt-URL** ein, wie in der AD FS 1 definiert.* x* Verbunddienst des Partners.<br />4. Geben Sie auf der Seite Bezeichner **Konfigurieren** unter **Anspruchs Anbieter-Vertrauensstellungs**-ID den **Verbunddienst URI** gemäß der Definition in AD FS 1 ein. *x* Verbunddienst des Partners.|![Verwenden von Ansprüchen aus AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Manuelles Erstellen einer Anspruchs Anbieter-Vertrauens](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md) Stellung|
| Auf der Anspruchs Anbieter-Vertrauensstellung, die Sie zuvor erstellt haben, müssen Sie eine Anspruchs Regel erstellen, die Ansprüche verwendet, die vom AD FS 1. x-Verbunddienst eingehenden werden, und Sie in einen namens-ID-Anspruchstyp weiterleiten, Filtern oder transformieren.<p>Wenn der Name-ID-Anspruchstyp durchlaufen, gefiltert oder transformiert wurde, kann er als Eingabe für eine andere Regel oder Regel verwendet werden, damit er von der AD FS Verbunddienst in Windows Server 2012 verstanden und genutzt werden kann.|![Verwenden von Ansprüchen aus AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Erstellen einer Regel zum Senden eines AD FS 1. x-kompatiblen Anspruchs](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|
| Wenden Sie sich an den Administrator des AD FS 1. *x* Verbunddienst und haben den Administrator des AD FS 1. *x* Verbunddienst Einrichten einer neuen Ressourcen Partner Vertrauensstellung. Stellen Sie diesem Administrator außerdem den Verbunddienst-URI \( in den Verbunddienst Eigenschaften \) , die Verbunddienst Endpunkt-URL und eine exportierte \- Tokensignaturzertifikats-Datei \( mit dem öffentlichen Schlüssel bereit \) . Der Administrator benötigt diese Elemente, um die Vertrauensstellung einzurichten.|N\/V|
