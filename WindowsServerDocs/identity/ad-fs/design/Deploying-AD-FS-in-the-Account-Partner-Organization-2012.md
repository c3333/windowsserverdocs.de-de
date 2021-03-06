---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: Bereitstellen von AD FS in der Kontopartnerorganisation
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: db72dd69a44d90a89a255b7d974f0cf1816bd722
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954326"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Bereitstellen von AD FS in der Kontopartnerorganisation

Ein Konto Partner in Active Directory-Verbunddienste (AD FS) \( AD FS \) stellt die Organisation in der Verbund Vertrauensstellung dar, die Benutzerkonten physisch in einem unterstützten Attribut Speicher speichert. Weitere Informationen zu den unterstützten Attribut speichern finden Sie unter [der Rolle von Attribut speichern](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).

Der Verbund Server in der Konto Partnerorganisation authentifiziert lokale Benutzer und erstellt Sicherheits Token, die vom Ressourcen Partner verwendet werden, um Autorisierungs Entscheidungen zu treffen. Vertrauende Seiten wie Websites und Webdienste können sich dann problemlos beim Verbund Server registrieren und ausgestellte Token für die Authentifizierung und Zugriffs Steuerung nutzen.

In Szenarien, in denen Sie Ihren Benutzern Zugriff auf mehrere Verbund Anwendungen oder-Dienste bereitstellen müssen – wenn jede Anwendung oder jeder Dienst von einer anderen Organisation gehostet wird – können Sie den Konto Partner Verbund Server so konfigurieren, dass Sie mehrere vertrauende Seiten bereitstellen können.

Weitere Informationen zum Einrichten und Konfigurieren von Kontopartnerorganisationen finden Sie unter [Checklist: Configuring the Account Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).

## <a name="in-this-section"></a>In diesem Abschnitt

-   [Überprüfen der Rolle des Verbundservers beim Kontopartner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)

-   [Überprüfen der Rolle des Verbundserverproxys beim Kontopartner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)

-   [Vorbereiten von Client Computern im Konto Partner](Prepare-Client-Computers-in-the-Account-Partner.md)

## <a name="see-also"></a>Weitere Informationen
[AD FS-Entwurfshandbuch in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
