---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Rolle „Standorttopologiebesitzer“
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 162256df7b35b1d0d7033036bbfc097187670762
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070132"
---
# <a name="site-topology-owner-role"></a>Rolle „Standorttopologiebesitzer“

>Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Der Administrator, der die Standort Topologie verwaltet, wird als Besitzer der Standort Topologie bezeichnet. Der Besitzer der Standort Topologie versteht die Bedingungen des Netzwerks Zwischenstand Orten und verfügt über die Berechtigung, die Einstellungen in Active Directory Domain Services (AD DS) zu ändern, um Änderungen an der Standort Topologie zu implementieren. Änderungen an der Standort Topologie wirken sich auf Änderungen in der Replikations Topologie aus. Zu den Zuständigkeiten der Standort Topologie gehören:

-   Steuern der Änderungen an der Standort Topologie, wenn sich die Netzwerk Konnektivität ändert.

-   Abrufen und Verwalten von Informationen über Netzwerkverbindungen und Router aus der Netzwerkgruppe. Der Standort topologiebesitzer muss eine Liste von Subnetzadressen, Subnetzmasken und dem Speicherort, zu dem die einzelnen gehören, verwalten. Außerdem muss der Besitzer der Standort Topologie alle Probleme bezüglich der Netzwerkgeschwindigkeit und-Kapazität verstehen, die die Standort Topologie beeinflussen, um die Kosten für Standort Verknüpfungen effektiv festzulegen.

-   Verschieben von Active Directory Server-Objekten, die Domänen Controller Zwischenstand Orten darstellen, wenn sich die IP-Adresse eines Domänen Controllers in einem anderen Subnetz an einem anderen Standort ändert, oder wenn das Subnetz selbst einem anderen Standort zugewiesen ist. In beiden Fällen muss der Besitzer der Standort Topologie das Active Directory Server-Objekt des Domänen Controllers manuell auf den neuen Standort verschieben.



