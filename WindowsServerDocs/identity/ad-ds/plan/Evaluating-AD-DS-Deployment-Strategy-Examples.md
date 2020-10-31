---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: 'Bewerten der AD DS-Bereitstellungsstrategie: Beispiele'
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 58236397485e0998e26e9ca67908c7937d32fbbb
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068631"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Bewerten der AD DS-Bereitstellungsstrategie: Beispiele

>Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sehen Sie sich das folgende Beispiel für ein fiktives Unternehmen an, das in seiner Umgebung Active Directory Domain Services (AD DS) bereitstellt. Die Umgebung von "Configuration Manager" besteht aus vier Domänen. Die Gesamtstruktur Funktionsebene ist Windows Server 2003. In der folgenden Abbildung wird die aktuelle Domänen Struktur der Organisation "Configuration Manager" veranschaulicht.

![AD DS Bereitstellungs Strategie](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)

Nach der Überprüfung der vorhandenen Umgebung und der Identifizierung der Bereitstellungs Ziele hat die folgende AD DS Bereitstellungs Strategie von "Configuration Manager" festgelegt:

-   Aktualisieren Sie Windows Server 2003-Domänen auf Windows Server 2008-Domänen.

-   Aktivieren Sie Erweiterte AD DS Features, indem Sie die Domänen-und Gesamtstruktur Funktionsebenen auf Windows Server 2008 erhöhen.

-   Strukturieren Sie die Africa.concorp.contoso.com-Domäne innerhalb der Gesamtstruktur so um, dass diese Domäne mit der EMEA. KONP. Configuration. con-Domäne konsolidiert wird.

Wenn Sie die Gesamtstruktur Funktionsebene auf Windows Server 2008 erhöhen, wird die Nutzung der neuen AD DS Features durch "Microsoft Configuration Manager" ermöglicht. Durch die Umstrukturierung der Domänen in der Gesamtstruktur, wie in der folgenden Abbildung dargestellt, wird die für die Verwaltung der Domänen erforderliche Verwaltungs Menge reduziert.

![AD DS Bereitstellungs Strategie](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)



