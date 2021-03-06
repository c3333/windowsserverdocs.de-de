---
title: Konfigurieren virtueller Computer, auf denen Windows 7 ausgeführt wird, ohne mehr als vier virtuelle Prozessoren
description: Online Version des Texts für diese Best Practices Analyzer Regel.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
ms.date: 8/16/2016
ms.openlocfilehash: 09388f843e963252dfcaca1eb587778658b48039
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745765"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>Konfigurieren virtueller Computer, auf denen Windows 7 ausgeführt wird, ohne mehr als vier virtuelle Prozessoren

>Gilt für: Windows Server 2016

Weitere Informationen zu bewährten Methoden und Scans finden Sie unter [Ausführen von Best Practices Analyzer-Scans und Verwalten der Scanergebnisse](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Eigenschaft|Details|
|-|-|
|**Betriebssystem**|Windows Server 2016|
|**Produkt/Feature**|Hyper-V|
|**Severity**|Fehler|
|**Kategorie**|Konfiguration|

In den folgenden Abschnitten gibt kursiv formatics den UI-Text an, der im Best Practices Analyzer Tool für dieses Problem angezeigt wird.

## <a name="issue"></a>**Problem**
*Ein virtueller Computer, auf dem Windows 7 ausgeführt wird, ist mit mehr als vier virtuellen Prozessoren konfiguriert.*

## <a name="impact"></a>**Auswirkungen**
*Microsoft unterstützt nicht die Konfiguration der folgenden virtuellen Computer:*

\<list of virtual machines>

## <a name="resolution"></a>**Lösung**
*Fahren Sie den virtuellen Computer herunter, und entfernen Sie einen oder mehrere virtuelle Prozessoren.*

#### <a name="to-remove-virtual-processors"></a>So entfernen Sie virtuelle Prozessoren

1.  Öffnen Sie den Hyper-V-Manager. Klicken Sie auf **Start**, zeigen Sie auf **Verwaltung**, und klicken Sie dann auf **Hyper-V-Manager**.

2.  Wählen Sie im Ergebnisbereich unter **Virtual Machines**den virtuellen Computer aus, den Sie konfigurieren möchten. Der Status der virtuellen Maschine sollte als **Off**aufgeführt werden. Wenn dies nicht der Fall ist, klicken Sie mit der rechten Maustaste auf den virtuellen Computer und dann auf **herunter**fahren.

3.  Klicken Sie im Bereich **Aktion** unter dem Namen des virtuellen Computers auf **Einstellungen**.

4.  Klicken Sie im Navigationsbereich auf **Prozessor**.

5.  Legen Sie auf der Seite **Prozessor** die Anzahl der Prozessoren auf **drei** oder weniger Prozessoren fest, und klicken Sie dann auf **OK**.



