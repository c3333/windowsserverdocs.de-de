---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Computerbenennung
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1be5d49b90f9e0573054f8d541a1118516b27f01
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068922"
---
# <a name="computer-naming"></a>Computerbenennung

> Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Wenn ein Computer, auf dem das Betriebssystem Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 oder Windows Vista ausgeführt wird, einer Domäne Beitritt, weist der Computer standardmäßig selbst einen Namen zu. Der Name, den er selbst zuweist, umfasst den Hostnamen des Computers (d. h. den Computernamen in den Systemeigenschaften) und den Domain Name System (DNS)-Namen der Active Directory Domäne, der der Computer beigetreten ist (d. h. das primäre DNS-Suffix in den Systemeigenschaften). Die Verkettung des Host Namens und des DNS-Namens der Domäne wird als voll qualifizierter Domänen Name (Fully Qualified Domain Name, FQDN) bezeichnet. Wenn z. b. ein Computer mit dem Hostnamen server1 der Domäne corp.contoso.com Beitritt, lautet der voll qualifizierte Domänen Name des Computers Server1.Corp.contoso.com.

Wenn ein Computer bereits über einen anderen DNS-Domänen Namen verfügt, der statisch in eine DNS-Zone eingegeben oder durch einen integrierten DNS/Dynamic Host Configuration-Protokolldienst (DHCP) registriert wurde, unterscheidet sich der voll qualifizierte Domänen Name des Computers von dem zuvor registrierten Namen. Auf den Computer kann mit einem der Namen verwiesen werden.

Weitere Informationen zu Benennungs Konventionen in Active Directory Domain Services (AD DS) finden Sie unter [Benennungs Konventionen in Active Directory für Computer, Domänen, Standorte und](https://support.microsoft.com/help/909264/)Organisationseinheiten.
