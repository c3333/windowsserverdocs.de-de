---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Delegieren der Verwaltung von Standardcontainern und Organisationseinheiten
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 8f52ca20cce86c869f66ea9d0e2e795956a0c75f
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069412"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delegieren der Verwaltung von Standardcontainern und Organisationseinheiten

>Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Jede Active Directory Domäne enthält einen Standardsatz von Containern und Organisationseinheiten (OUs), die während der Installation von Active Directory Domain Services (AD DS) erstellt werden. Hierzu gehört Folgendes:

-   Domänen Container, der als Stamm Container für die Hierarchie fungiert.

-   Integrierter Container, der die standardmäßigen Dienst Administrator Konten enthält

-   Benutzer Container. Dies ist der Standard Speicherort für neue Benutzerkonten und Gruppen, die in der Domäne erstellt wurden.

-   Computer Container. Dies ist der Standard Speicherort für neue Computer Konten, die in der Domäne erstellt werden.

-   Domänen Controller-Organisationseinheit, die der Standard Speicherort für die Computer Konten für Domänen Controller-Computer Konten ist

Der Gesamtstruktur Besitzer steuert diese Standardcontainer und Organisationseinheiten.

## <a name="domain-container"></a>Domänen Container
Der Domänen Container ist der Stamm Container der Hierarchie einer Domäne. Änderungen an den Richtlinien oder der Zugriffs Steuerungs Liste (Access Control List, ACL) für diesen Container können sich potenziell auf die Domänen weite auswirken. Die Steuerung dieses Containers nicht delegieren. Er muss von den Dienst Administratoren gesteuert werden.

## <a name="users-and-computers-containers"></a>Benutzer und Computer Container
Wenn Sie ein direktes Domänen Upgrade von Windows Server 2003 auf Windows Server 2008 durchführen, werden vorhandene Benutzer und Computer automatisch in die Benutzer und die Computer Container eingefügt. Wenn Sie eine neue Active Directory Domäne erstellen, sind die Container "Benutzer" und "Computer" die Standard Speicherorte für alle neuen Benutzerkonten und Computer Konten ohne Domänen Controller in der Domäne.

> [!IMPORTANT]
> Wenn Sie die Kontrolle über Benutzer oder Computer delegieren müssen, ändern Sie die Standardeinstellungen für die Container "Benutzer" und "Computer" nicht. Erstellen Sie stattdessen neue Organisationseinheiten (bei Bedarf), und verschieben Sie die Benutzer-und Computer Objekte aus ihren Standardcontainern und in die neuen Organisationseinheiten. Delegieren Sie die Steuerung bei Bedarf über die neuen Organisationseinheiten. Es wird empfohlen, dass Sie die Steuerelemente der Standardcontainer nicht ändern.

Außerdem können Sie Gruppenrichtlinie Einstellungen nicht auf die Standardcontainer für Benutzer und Computer anwenden. Wenn Sie Gruppenrichtlinie auf Benutzer und Computer anwenden möchten, erstellen Sie neue Organisationseinheiten, und verschieben Sie die Benutzer-und Computer Objekte in diese Organisationseinheiten. Wenden Sie die Gruppenrichtlinie Einstellungen auf die neuen Organisationseinheiten an.

Optional können Sie die Erstellung von Objekten, die in den Standardcontainern abgelegt werden, so umleiten, dass Sie in Container Ihrer Wahl eingefügt werden.

## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Bekannte Benutzer und Gruppen und integrierte Konten
Standardmäßig werden mehrere bekannte Benutzer und Gruppen und integrierte Konten in einer neuen Domäne erstellt. Es wird empfohlen, dass die Verwaltung dieser Konten von den Dienst Administratoren gesteuert wird. Delegieren Sie die Verwaltung dieser Konten nicht an eine Person, die kein Dienst Administrator ist. In der folgenden Tabelle werden die bekannten Benutzer und Gruppen sowie integrierte Konten aufgelistet, die unter der Kontrolle der Dienst Administratoren bleiben müssen.

|Bekannte Benutzer und Gruppen|Integrierte Konten|
|--------------------------------|----------------------|
|Zertifikatherausgeber<p>Domänencontroller<p>Gruppenrichtlinienersteller-Besitzer<p>KRBTGT<p>Domänen-Gäste<p>Administrator<p>Domänenadministratoren<p>Schema-Admins (nur Gesamtstruktur-Stamm Domäne)<p>Organisations-Admins (nur Gesamtstruktur-Stamm Domäne)<p>Domänenbenutzer|Administrator<p>Gast<p>Gäste<p>Konten-Operatoren<p>Administratoren<p>Sicherungsoperatoren<p>Eingehende Gesamtstruktur-Vertrauensstellung<p>Druck-Operatoren<p>Prä-Windows 2000 kompatibler Zugriff<p>Server-Operatoren<p>Benutzer|

## <a name="domain-controller-ou"></a>Domänen Controller-OE
Wenn Domänen Controller der Domäne hinzugefügt werden, werden Ihre Computer Objekte der Organisationseinheit des Domänen Controllers automatisch hinzugefügt. Auf diese Organisationseinheit wird ein Standardsatz an Richtlinien angewendet. Um sicherzustellen, dass diese Richtlinien einheitlich auf alle Domänen Controller angewendet werden, wird empfohlen, dass Sie die Computer Objekte der Domänen Controller nicht aus dieser Organisationseinheit verschieben. Fehler beim Anwenden der Standardrichtlinien können dazu führen, dass ein Domänen Controller nicht ordnungsgemäß funktioniert.

Standardmäßig steuern diese Organisationseinheit die Dienst Administratoren. Delegieren Sie die Steuerung dieser Organisationseinheit nicht an andere Personen als die Dienst Administratoren.



