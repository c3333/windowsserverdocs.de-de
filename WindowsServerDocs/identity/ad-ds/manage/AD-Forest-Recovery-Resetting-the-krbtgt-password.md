---
title: 'AD-Gesamtstruktur Wiederherstellung: Zurücksetzen des krbtgt-Kennworts'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.openlocfilehash: 62416758d373423246ccb030ea6baf325515fb49
ms.sourcegitcommit: b39ea3b83280f00e5bb100df0dc8beaf1fb55be2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2020
ms.locfileid: "94520433"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>AD-Gesamtstruktur Wiederherstellung: Zurücksetzen des krbtgt-Kennworts

>Gilt für: Windows Server 2016, Windows Server 2012 und 2012 R2, Windows Server 2008 und 2008 R2

Verwenden Sie das folgende Verfahren, um das krbtgt-Kennwort für die Domäne zurückzusetzen. Im folgenden Verfahren werden beschreibbare DCS, aber keine schreibgeschützten Domänen Controller (Read-Only Domain Controllers, RODCs) angewendet.

> [!IMPORTANT]
> Wenn Sie RODCs während der Wiederherstellung der Gesamtstruktur Online wiederherstellen möchten, löschen Sie die krbtgt-Konten für die RODCs nicht. Das krbtgt-Konto für einen RODC ist im Format krbtgt_ *Nummer* aufgeführt.
>
> Wenn Sie einen angepassten Kenn Wortfilter (z. b. passfilt.dll) auf einem Domänen Controller verwenden, erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie versuchen, das krbtgt-Kennwort zurückzusetzen. Weitere Informationen, einschließlich einer Problem Umgehung, finden Sie im Microsoft Knowledge Base- [Artikel 2549833](https://support.microsoft.com/kb/2549833) ( https://support.microsoft.com/kb/2549833) .

## <a name="to-reset-the-krbtgt-password"></a>So setzen Sie das krbtgt-Kennwort zurück

1. Klicken Sie auf **Start** , zeigen Sie auf **Systemsteuerung** , zeigen Sie auf **Verwaltung** , und klicken Sie dann auf **Active Directory Benutzer und Computer**.
2. Klicken Sie auf **Ansicht** und dann auf **Erweiterte Funktionen**.
3. Doppelklicken Sie in der Konsolen Struktur auf den Domänen Container, und klicken Sie dann auf **Benutzer**.
4. Klicken Sie im Detailfenster mit der rechten Maustaste auf das Benutzerkonto **krbtgt** , und klicken Sie dann auf **Kennwort zurücksetzen**.
   ![Kennwort zurücksetzen](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. Geben Sie unter **Neues Kennwort** ein neues Kennwort ein, geben Sie das Kennwort unter **Kennwort bestätigen** erneut ein, und klicken Sie auf **OK**. Das angegebene Kennwort ist nicht signifikant, weil das System automatisch unabhängig vom angegebenen Kennwort ein sicheres Kennwort generiert.

> [!NOTE]
> Dieser Vorgang sollte zweimal durchgeführt werden. Der Kenn Wort Verlauf des krbtgt-Kontos lautet "2". das bedeutet, dass es die beiden neuesten Kenn Wörter enthält. Wenn Sie das Kennwort zweimal zurücksetzen, löschen Sie die alten Kenn Wörter aus dem Verlauf, sodass es keine Möglichkeit gibt, dass ein anderer Domänen Controller mit diesem Domänen Controller mit einem alten Kennwort repliziert wird.

> [!NOTE]
> Wenn Sie das Kennwort für das Schlüsselverteilungscenter-Dienst Konto zweimal zurücksetzen, ist zwischen den zurück Stellungen eine Wartezeit von 10 Stunden erforderlich.

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellung der AD-Gesamtstruktur: Leitfaden](AD-Forest-Recovery-Guide.md)
- [Wiederherstellung der AD-Gesamtstruktur: Verfahren](AD-Forest-Recovery-Procedures.md)
