---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Entwerfen der logischen Struktur
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 09a26589d2301e4b4d2e206c4953e9ae7810f3cc
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069302"
---
# <a name="designing-the-logical-structure"></a>Entwerfen der logischen Struktur

>Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mit Active Directory Domain Services (AD DS) können Organisationen eine skalierbare, sichere und verwaltbare Infrastruktur für die Benutzer-und Ressourcenverwaltung erstellen. Sie ermöglicht außerdem die Unterstützung von Verzeichnis fähigen Anwendungen.

Eine gut entworfene Active Directory logische Struktur bietet die folgenden Vorteile:

-   Vereinfachte Verwaltung von Microsoft Windows-basierten Netzwerken, die eine große Anzahl von Objekten enthalten

-   Konsolidierte Domänen Struktur und geringere Verwaltungskosten

-   Die Möglichkeit, die administrative Kontrolle über Ressourcen nach Bedarf zu delegieren

-   Geringere Auswirkungen auf die Netzwerkbandbreite

-   Vereinfachte Ressourcen Freigabe

-   Optimale Suchleistung

-   Niedrige Gesamtkosten

Eine gut entworfene Active Directory logische Struktur vereinfacht die effiziente Integration solcher Funktionen wie Gruppenrichtlinie. Desktop Sperrung; Software Verteilung; und die Benutzer-, Gruppen-, Arbeitsstations-und Serververwaltung in Ihrem System. Außerdem ermöglicht eine sorgfältig entworfene logische Struktur die Integration von Microsoft-Anwendungen und-Diensten, die keine Microsoft-Anwendungen sind, wie z. b. Microsoft Exchange Server, Public Key-Infrastruktur (PKI) und ein domänenbasiertes verteiltes Dateisystem (DFS).

Wenn Sie eine Active Directory logische Struktur entwerfen, bevor Sie AD DS bereitstellen, können Sie den Bereitstellungs Prozess optimieren, um Active Directory Features optimal zu nutzen. Um die Active Directory logische Struktur zu entwerfen, identifiziert das Entwurfs Team zuerst die Anforderungen für Ihre Organisation und entscheidet basierend auf diesen Informationen, wo die Gesamtstruktur-und Domänen Grenzen zu platzieren sind. Anschließend entscheidet das Entwurfs Team, wie die Domain Name System (DNS)-Umgebung konfiguriert wird, um die Anforderungen der Gesamtstruktur zu erfüllen. Zum Schluss identifiziert das Entwurfs Team die Struktur der Organisationseinheit (OU), die zum Delegieren der Verwaltung von Ressourcen in Ihrer Organisation erforderlich ist.

## <a name="in-this-guide"></a>Inhalt dieser Anleitung

-   [Grundlegendes zum Active Directory logisches Modell](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)

-   [Bestimmen der Teilnehmer am Bereitstellungsprojekt](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)

-   [Erstellen eines Gesamtstrukturentwurfs](../../ad-ds/plan/Creating-a-Forest-Design.md)

-   [Erstellen eines Domänenentwurfs](../../ad-ds/plan/Creating-a-Domain-Design.md)

-   [Erstellen eines DNS-Infrastrukturentwurfs](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)

-   [Erstellen eines Organisationseinheitsentwurfs](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)

-   [Anhang A: DNS-Inventur](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)



