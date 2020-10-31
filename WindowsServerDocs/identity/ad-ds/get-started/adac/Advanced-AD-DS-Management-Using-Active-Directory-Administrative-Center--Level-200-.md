---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Advanced AD DS Management Using Active Directory Administrative Center (Level 200)
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 255cb3da2056ed69d13cfd5814bb038420d9adc5
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070702"
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Advanced AD DS Management Using Active Directory Administrative Center (Level 200)

>Gilt für: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dieser Artikel behandelt das aktualisierte Active Directory-Verwaltungscenter mit dem neuen Active Directory-Papierkorb, differenzierten Kennwortrichtlinien und der Windows PowerShell-Verlaufsanzeige im Detail, inklusive Architektur, Beispielen für gängige Aufgaben und Informationen zur Problembehandlung. Eine Einführung finden Sie unter [Einführung in Active Directory-Verwaltungscenter Erweiterungen &#40;Ebene 100&#41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).

- [Active Directory-Verwaltungscenter: Architektur](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)
- [Aktivieren und Verwalten des Active Directory-Papierkorbs im Active Directory-Verwaltungscenter](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)
- [Konfigurieren und Verwalten der differenzierten Kennwortrichtlinien mit dem Active Directory-Verwaltungscenter](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)
- [Verwenden der PowerShell-Verlaufsanzeige im Active Directory-Verwaltungscenter](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)
- [Problembehandlung bei der AD DS-Verwaltung](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)

## <a name="active-directory-administrative-center-architecture"></a><a name="BKMK_Arch"></a>Active Directory-Verwaltungscenter: Architektur

### <a name="active-directory-administrative-center-executables-dlls"></a>Ausführbare Active Directory-Verwaltungscenter ausführbare Dateien, DLLs

Modul und zugrunde liegende Architektur des Active Directory-Verwaltungscenters haben sich mit dem neuen Papierkorb, FGPP und der Verlaufsanzeige nicht geändert.

- Microsoft.ActiveDirectory.Management.UI.dll
- Microsoft.ActiveDirectory.Management.UI.resources.dll
- Microsoft.ActiveDirectory.Management.dll
- Microsoft.ActiveDirectory.Management.resources.dll
- ActiveDirectoryPowerShellResources.dll

Die zugrunde liegende Windows PowerShell und Operationsebene für die neuen Papierkorb-Funktionen sind wie folgt:

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)

## <a name="enabling-and-managing-the-active-directory-recycle-bin-using-active-directory-administrative-center"></a><a name="BKMK_EnableRecycleBin"></a>Aktivieren und Verwalten des Active Directory-Papierkorbs im Active Directory-Verwaltungscenter

### <a name="capabilities"></a>Funktionen

- Mit dem Active Directory-Verwaltungscenter Windows Server 2012 oder höher können Sie den Active Directory Papierkorb für beliebige Domänen Partitionen in einer Gesamtstruktur konfigurieren und verwalten. Windows PowerShell oder Ldp.exe werden zum Aktivieren des Active Directory-Papierkorbs oder zum Wiederherstellen von Objekten in Domänenpartitionen nicht mehr benötigt.
- Das Active Directory-Verwaltungscenter bietet erweiterte Filterkriterien und erleichtert die gezielte Wiederherstellung in großen Umgebungen mit vielen absichtlich gelöschten Objekten.

### <a name="limitations"></a>Einschränkungen

- Da das Active Directory-Verwaltungscenter nur Domänenpartitionen verwaltet, können keine gelöschten Objekte aus Konfiguration, Domänen-DNS oder Gesamtstruktur-DNS-Partitionen wiederhergestellt werden (Objekte aus der Schemapartition können nicht gelöscht werden). Verwenden Sie [Restore-ADObject](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617262(v=technet.10))zum Wiederherstellen von Objekten aus Nicht-Domänenpartitionen.

- Das Active Directory-Verwaltungscenter kann keine Unterstrukturen von Objekten in einer einzigen Aktion wiederherstellen. Wenn Sie z. B. eine OU mit verschachtelten OUs, Benutzern, Gruppen und Computern löschen, werden die untergeordneten Objekte beim Wiederherstellen der Stamm-OU nicht wiederhergestellt.

    > [!NOTE]
    > Der Active Directory-Verwaltungscenter Batch Wiederherstellungs Vorgang führt eine "bestmögliche" Sortierung der gelöschten Objekte *innerhalb der Auswahl* durch, sodass übergeordnete Elemente vor den untergeordneten Elementen für die Wiederherstellungs Liste sortiert werden. In einfachen Testfällen kann es sein, dass Unterstrukturen von Objekten in einer einzigen Aktion wiederhergestellt werden. In eckfällen (z. b. einer Auswahl, die Teilstruktur Strukturen enthält) mit einigen gelöschten übergeordneten Knoten fehlen oder Fehlerfälle, wie z. b. das Überspringen der untergeordneten Objekte, wenn die übergeordnete Wiederherstellung fehlschlägt, funktioniert möglicherweise nicht wie erwartet. Aus diesem Grund sollten Sie Unterstrukturen von Objekten immer als separate Aktion wiederherstellen, nachdem Sie die übergeordneten Objekte wiederhergestellt haben.

Der Active Directory Papierkorb erfordert eine Windows Server 2008 R2-Gesamtstruktur Funktionsebene, und Sie müssen Mitglied der Gruppe "Organisations-Admins" sein. Nach der Aktivierung können Sie den Active Directory-Papierkorb nicht mehr deaktivieren. Mit dem Active Directory-Papierkorb wachsen die Active Directory-Datenbanken (NTDS-DIT) auf jedem Domänencontroller in der Gesamtstruktur. Der vom Papierkorb verwendete Speicherplatz wächst mit der Zeit, da dort Objekte mit all ihren Attributdaten aufbewahrt werden.

### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Aktivieren des Active Directory-Papierkorbs im Active Directory-Verwaltungscenter

Um den Active Directory-Papierkorb zu aktivieren, öffnen Sie das **Active Directory-Verwaltungscenter** und klicken Sie im Navigationsbereich auf den Namen Ihrer Gesamtstruktur. Klicken Sie im Bereich **Aufgaben** auf **Papierkorb aktivieren** .

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)

Das Active Directory-Verwaltungscenter zeigt das Dialogfeld **Papierkorbaktivierung bestätigen** an. Dieses Dialogfeld weist Sie darauf hin, dass die Aktivierung nicht rückgängig gemacht werden kann. Klicken Sie auf **OK** , um den Active Directory-Papierkorb zu aktivieren. Das Active Directory-Verwaltungscenter zeigt ein weiteres Dialogfeld an und weist Sie darauf hin, dass der Active Directory-Papierkorb erst dann voll funktionsfähig ist, wenn die Konfigurationsänderung auf alle Domänencontroller repliziert wurde.

> [!IMPORTANT]
> Der Active Directory-Papierkorb kann nicht aktiviert werden, wenn:
>
> - Die Funktionsebene der Gesamtstruktur nicht mindestens Windows Server 2008 R2 entspricht
> - Der Papierkorb bereits aktiviert ist

Das entsprechende Active Directory Windows PowerShell-Cmdlet ist:

```powershell
Enable-ADOptionalFeature
```

Weitere Informationen zum Aktivieren des Active Directory-Papierkorbs mithilfe von Windows PowerShell finden Sie unter [Schrittweise Anleitung zum Active Directory-Papierkorb](./introduction-to-active-directory-administrative-center-enhancements--level-100-.md#active-directory-recycle-bin-step-by-step).

### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Verwalten des Active Directory-Papierkorbs im Active Directory-Verwaltungscenter

Dieser Abschnitt verwendet eine existierende Beispieldomäne mit dem Namen **corp.contoso.com** . In dieser Domäne sind die Benutzer in eine übergeordnete OU mit dem Namen **UserAccounts** geordnet. Die OU **UserAccounts** enthält drei untergeordnete OUs mit den Abteilungsnamen, die wiederum weitere OUs, Benutzer und Gruppen enthalten.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)

#### <a name="storage-and-filtering"></a>Speicherung und Filterung

Der Active Directory-Papierkorb bewahrt alle in der Gesamtstruktur gelöschten Objekte auf. Die Objekte werden gemäß des **msDS-deletedObjectLifetime** -Attributs gespeichert, dessen Wert standardmäßig dem **tombstoneLifetime** -Attribut der Gesamtstruktur entspricht. Der Wert für **tombstoneLifetime** ist in allen unter Windows Server 2003 SP1 oder später erstellten Gesamtstrukturen ist standardmäßig auf 180 Tage gesetzt. In allen von Windows 2000 aktualisierten oder mit Windows Server 2003 (ohne Service Pack) erstellten Gesamtstrukturen ist das tombstoneLifetime-Standardattribut NICHT gesetzt, und Windows verwendet daher den internen Standardwert von 60 Tagen. All diese Werte sind konfigurierbar. Sie können das Active Directory-Verwaltungscenter verwenden, um beliebige aus den Domänenpartitionen der Gesamtstruktur gelöschte Objekte wiederherzustellen. Sie müssen weiterhin das Cmdlet **Restore-ADObject** verwenden, um gelöschte Objekte aus anderen Partitionen wiederherzustellen, wie z. B. der Konfiguration. Durch die Aktivierung des Active Directory-Papierkorbs wird der **Deleted Objects** -Container für die einzelnen Partitionen im Active Directory-Verwaltungscenter sichtbar.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)

Der **Deleted Objects** -Container enthält alle wiederherstellbaren Objekte der jeweiligen Domänenpartition. Gelöschte Objekte, die älter als **msDS-deletedObjectLifetime** sind, werden auch als wiederverwendete Objekte bezeichnet. Wiederverwendete Objekte werden im Active Directory-Verwaltungscenter nicht angezeigt und können von dort nicht wiederhergestellt werden.

Eine detailliertere Beschreibung von Architektur und Verarbeitungsregeln des Papierkorbs finden Sie unter [The AD Recycle Bin: Understanding, Implementing, Best Practices, and Troubleshooting](/archive/blogs/askds/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting).

Das Active Directory-Verwaltungscenter beschränkt die Standardanzahl der pro Container zurückgegebenen Objekte auf 20.000. Sie können dieses Limit auf maximal 100.000 anheben, indem Sie im Menü **Verwalten** auf **Verwaltungslistenoptionen** klicken.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)

#### <a name="restoration"></a>Wiederherstellung

##### <a name="filtering"></a>Filterung

Das Active Directory-Verwaltungscenter bietet umfassende Kriterien und Filteroptionen, mit denen Sie sich vertraut machen sollten, bevor Sie diese bei einer tatsächlichen Wiederherstellung einsetzen. Zahlreiche Domänenobjekte werden mit der Zeit absichtlich gelöscht. Für eine erwartete Lebensdauer von 180 Tagen für gelöschte Objekte können Sie im Problemfall nicht einfach alle Objekte wiederherstellen.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)

Anstatt komplexe LDAP-Filter zu schreiben und UTC-Werte in Datums- und Uhrzeitobjekte zu konvertieren, können Sie das erweiterte **Filter** -Menü verwenden, um nur relevante Objekte aufzulisten. Wenn Sie den Tag der Löschung, die Namen von Objekten oder andere Schlüsseldaten kennen, können Sie dies beim Filtern zu Ihrem Vorteil nutzen. Klicken Sie auf die Chevron-Schaltfläche neben dem Suchfeld, um die erweiterten Filteroptionen zu aktivieren/deaktivieren.

Die Wiederherstellung unterstützt alle Standardoptionen für Filterkriterien, wie auch jede andere Suche. Wichtige integrierte Filter für die Wiederherstellung von Objekten:

- *ANR (Auflösung mehrdeutiger Namen-nicht im Menü aufgelistet, aber was wird verwendet, wenn Sie in das Feld * * * * Filter * * * * eingeben)*
- Zuletzt zwischen den angegebenen Datumswerten geändert
- Objekt ist user/inetorgperson/computer/group/organization unit
- Name
- Löschdatum
- Letztes bekanntes übergeordnetes Element
- type
- Description
- City
- Land/Region
- Department
- Mitarbeiter-ID
- Vorname
- Berufsbezeichnung
- Nachname
- SAMAccountName
- Bundesland/Kanton
- Telefonnummer
- UPN
- Postleitzahl

Sie können mehrere Kriterien verwenden. Beispielsweise können Sie alle Benutzer Objekte finden, die am 24. September 2012 aus Chicago, Illinois, mit der Auftrags Bezeichnung Manager gelöscht wurden.

Außerdem können Sie Spaltenüberschriften hinzufügen, ändern oder neu sortieren, um zusätzliche Details für die wiederherzustellenden Objekte zu erhalten.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)

Weitere Informationen zur Auflösung mehrdeutiger Namen finden Sie unter [ANR-Attribute](/windows/win32/adschema/attributes-anr).

##### <a name="single-object"></a>Einzelne Objekte

Die Wiederherstellung gelöschter Objekte war schon immer eine einzelne Operation.  Das Active Directory-Verwaltungscenter vereinfacht diese Operation. So stellen Sie ein gelöschtes Objekt wie z. B. einen einzelnen Benutzer wieder her:

1. Klicken Sie im Navigationsbereich des Active Directory-Verwaltungscenters auf den Domänennamen.
2. Doppelklicken Sie in der Verwaltungsliste auf **Gelöschte Objekte** .
3. Klicken Sie mit der rechten Maustaste auf **Wiederherstellen** , oder klicken Sie auf **Wiederherstellen** im Bereich **Aufgaben** .

Das Objekt wird an seinem ursprünglichen Ort wiederhergestellt.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)

Klicken Sie auf **Wiederherstellen, um** den Wiederherstellungs Speicherort zu ändern. Dies ist hilfreich, wenn der übergeordnete Container des gelöschten Objekts ebenfalls gelöscht wurde, Sie jedoch das übergeordnete Element nicht wiederherstellen möchten.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)

##### <a name="multiple-peer-objects"></a>Mehrere Objekte auf derselben Ebene

Sie können mehrere Objekte auf derselben Ebene wiederherstellen, z. B. alle Benutzer in einer OU. Halten Sie die STRG-Taste gedrückt und klicken Sie auf alle gelöschten Objekte, die Sie wiederherstellen möchten. Klicken Sie im Bereich Aufgaben auf **Wiederherstellen** . Sie können alle angezeigten Objekte markieren, indem Sie STRG und die A-Taste gleichzeitig drücken, oder einen Bereich von Objekten per Umschalttaste und Klick.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)

##### <a name="multiple-parent-and-child-objects"></a>Mehrere über- und untergeordnete Objekte

Sie müssen den Wiederherstellungsprozess unbedingt verstehen, um mehrere über- und untergeordnete Objekte wiederherzustellen, da das Active Directory-Verwaltungscenter verschachtelte Strukturen gelöschter Objekte nicht in einer einzelnen Aktion wiederherstellen kann.

1. Stellen Sie das oberste Objekt in der Struktur wieder her.
2. Stellen Sie die direkt untergeordneten Objekte dieses übergeordneten Objekts wieder her.
3. Stellen Sie die direkt untergeordneten Objekte dieser übergeordneten Objekte wieder her.
4. Wiederholen Sie den Vorgang so lange, bis Sie alle Objekte wiederhergestellt haben.

Sie können untergeordnete Objekte nur wiederherstellen, wenn Sie deren übergeordnetes Objekt wiederhergestellt haben. Andernfalls wird der folgende Fehler ausgegeben:

**Der Vorgang konnte nicht ausgeführt werden, da das übergeordnete Objekt instanziiert oder gelöscht wurde.**

Das Attribut **Letztes bekanntes übergeordnetes Element** enthält die hierarchische Beziehung des jeweiligen Objekts. Das Attribut **Letztes bekanntes übergeordnetes Element** wird vom Löschort zum Wiederherstellungsort geändert, wenn Sie das Active Directory-Verwaltungscenter nach der Wiederherstellung eines übergeordneten Elements aktualisieren. Daher können Sie das untergeordnete Objekt wiederherstellen, wenn der Speicherort eines übergeordneten Objekts nicht mehr den Distinguished Name des gelöschten Objekte Containers anzeigt.

Nehmen wir ein Szenario an, in dem ein Administrator versehentlich die Sales-OU löscht, die alle untergeordneten OUs und Benutzer enthält.

Beachten Sie zunächst den Wert des **letzten bekannten übergeordneten** Attributs für alle gelöschten Benutzer und die Art der Lesevorgänge * *OU = sales\0adel:* <GUID und gelöschte Objekte Container Distinguished Name> * * *:

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)

Filtern Sie nach dem mehrdeutigen Namen "Sales", um die gelöschte OU anzuzeigen und wiederherzustellen:

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)

Aktualisieren Sie die Active Directory-Verwaltungscenter, um die Änderung des letzten bekannten übergeordneten Attributs des Benutzer Objekts in den Namen der wiederhergestellten Organisationseinheit "Sales ou" anzuzeigen:

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)

Filtern Sie nach allen "Sales"-Benutzern. Drücken Sie STRG + A, um alle gelöschten "Sales"-Benutzer auszuwählen. Klicken Sie auf **Wiederherstellen** , um die Objekte aus dem Container **Gelöschte Objekte** in die OU "Sales" zu verschieben. Gruppenmitgliedschaften und Attribute der Objekte bleiben dabei erhalten.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)

Wenn die OU **Sales** eigene untergeordnete OUs enthält, müssten Sie diese untergeordneten OUs wiederherstellen, bevor Sie deren untergeordnete Elemente wiederherstellen, und so weiter.

Informationen zum Wiederherstellen aller verschachtelten Objekte durch Angabe eines gelöschten übergeordneten Containers finden Sie im [Anhang B: Wiederherstellen mehrerer gelöschter Active Directory-Objekte (Beispielskript)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379504(v=ws.10)).

Das Active Directory Windows PowerShell-Cmdlet zum Wiederherstellen gelöschter Objekte ist:

```powershell
Restore-adobject
```

Die Funktionen des **Restore-ADObject** -Cmdlets haben sich von Windows Server 2008 R2 zu Windows Server 2012 nicht geändert.

##### <a name="server-side-filtering"></a>Serverseitiges Filtern

Der Container "Gelöschte Objekte" kann in mittelgroßen und großen Unternehmen mit der Zeit über 20.000 (oder sogar 100.000) Objekte überschreiten und die Anzeige aller Objekte erschweren. Da die Filtermechanismen im Active Directory-Verwaltungscenter nur clientseitig filtern, können diese zusätzlichen Objekte nicht angezeigt werden. Mit den folgenden Schritten können Sie eine serverseitige Suche durchführen und diese Einschränkung umgehen:

1. Klicken Sie mit der rechten Maustaste auf den Container **Gelöschte Objekte** und klicken Sie auf **Unter diesem Knoten suchen** .
2. Klicken Sie auf die Chevron-Schaltfläche, um das Menü **+Kriterien hinzufügen** anzuzeigen, wählen Sie **Zuletzt zwischen den angegebenen Datumswerten geändert** aus und fügen Sie diesen Filter hinzu. Der Zeitpunkt der letzten Änderung (im Attribut **Änderungszeitpunkt** ) ist eine Annäherung des Löschzeitpunkts. In den meisten Umgebungen sind die beiden Zeitpunkte identisch. Diese Abfrage führt eine serverseitige Suche durch.
3. Suchen Sie die wiederherzustellenden gelöschten Objekte mithilfe von Filterung, Sortierung usw. in der Ergebnisliste und stellen Sie die Objekte wieder her.

## <a name="configuring-and-managing-fine-grained-password-policies-using-active-directory-administrative-center"></a><a name="BKMK_FGPP"></a>Konfigurieren und Verwalten der differenzierten Kennwortrichtlinien mit dem Active Directory-Verwaltungscenter

### <a name="configuring-fine-grained-password-policies"></a>Konfigurieren fein abgestimmter Kennwortrichtlinien

Im Active Directory-Verwaltungscenter können Sie Objekte für fein abgestimmte Kennwortrichtlinien (Fine-Grained Password Policy FGPP) erstellen und verwalten. Das FGPP-Feature wurde mit Windows Server 2008 eingeführt, und Windows Server 2012 enthält die erste grafische Verwaltungsoberfläche für dieses Feature. Fein abgestimmte Kennwortrichtlinien werden auf Domänenebene angewendet und überschreiben das von Windows Server 2003 benötigte einzelne Domänenkennwort. Sie können FGPP mit unterschiedlichen Einstellungen erstellen, um Kennwortrichtlinien für einzelne Benutzer oder Gruppen in einer Domäne zu implementieren.

Weitere Informationen zu fein abgestimmten Kennwortrichtlinien finden Sie unter [Schrittweise Anleitung für die Konfiguration abgestimmter Kennwort- und Kontosperrungsrichtlinien für AD DS (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770842(v=ws.10)).

Klicken Sie im Navigationsbereich auf die Strukturansicht, dann auf Ihre Domäne und auf **System** . Klicken Sie auf **Kennworteinstellungscontainer** und anschließend im Taskbereich auf **Neu** und auf **Kennworteinstellungen** .

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)

### <a name="managing-fine-grained-password-policies"></a>Verwalten fein abgestimmter Kennwortrichtlinien

Beim Erstellen oder Bearbeiten von FGPPs wird der Editor für **Kennworteinstellungen** geöffnet. Dort können Sie alle gewünschten Kennwortrichtlinien konfigurieren, so wie Sie dies in Windows Server 2008 oder Windows Server 2008 R2 getan hätten, allerdings mit einem speziell dafür entwickelten Editor.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)

Füllen Sie alle Pflichtfelder (rotes Sternchen) und optionalen Felder aus und klicken Sie auf **Hinzufügen** , um die Gruppen und Benutzer zu konfigurieren, für die diese Richtlinie gelten soll. FGPP überschreibt die Standardrichtlinien der Domäne für diese angegebenen Sicherheitsprinzipale. In der obigen Abbildung wird eine extrem restriktive Richtlinie nur für das integrierte Administratorkonto angewendet, um dessen Sicherheit zu verbessern. Die Richtlinie ist viel zu komplex für Standardbenutzer, eignet sich jedoch perfekt für ein Konto mit hohem Risiko, das nur von IT-Fachkräften verwendet wird.

Außerdem können Sie Rangfolgen konfigurieren und angeben, für welche Benutzer und Gruppen innerhalb der angegebenen Domäne eine Richtlinie gelten soll.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)

Die Active Directory Windows PowerShell-Cmdlets für fein abgestimmte Kennwortrichtlinien sind:

```powershell
Add-ADFineGrainedPasswordPolicySubject
Get-ADFineGrainedPasswordPolicy
Get-ADFineGrainedPasswordPolicySubject
New-ADFineGrainedPasswordPolicy
Remove-ADFineGrainedPasswordPolicy
Remove-ADFineGrainedPasswordPolicySubject
Set-ADFineGrainedPasswordPolicy
```

Die Funktionen der Cmdlets für fein abgestimmte Kennwortrichtlinien haben sich von Windows Server 2008 R2 zu Windows Server 2012 nicht geändert. Das folgende Diagramm zeigt die jeweiligen Argumente für die einzelnen Cmdlets:

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)

Im Active Directory-Verwaltungscenter können Sie außerdem die resultierenden FGPP-Sets für einen bestimmten Benutzer abrufen. Klicken Sie mit der rechten Maustaste auf den Benutzer, und klicken Sie auf **resultierende Kenn Wort Einstellungen anzeigen...** , um die Seite mit den Kenn *Wort Einstellungen* zu öffnen

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)

In den **Eigenschaften** von Benutzern und Gruppen werden die **Direkt zugeordneten Kennworteinstellungen** angezeigt (explizit zugewiesene FGPPs):

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)

Die implizite Zuweisung von "f" wird hier nicht angezeigt. Hierzu müssen Sie die Option resultierende Kenn **Wort Einstellungen anzeigen...** verwenden.

## <a name="using-the-active-directory-administrative-center-windows-powershell-history-viewer"></a><a name="BKMK_HistoryViewer"></a>Verwenden der PowerShell-Verlaufsanzeige im Active Directory-Verwaltungscenter

Windows PowerShell ist die Zukunft der Windows-Verwaltung. Die Verwaltung komplexer verteilter Systeme wird durch grafische Tools, die auf Task-Automatisierungs-Frameworks aufbauen, einheitlicher und effizienter gestaltet. Sie müssen Sich mit der Funktionsweise von Windows PowerShell vertraut machen, um deren volles Potenzial auszuschöpfen und maximalen Nutzen aus Ihren IT-Investitionen zu ziehen.

Das Active Directory-Verwaltungscenter bietet nun eine komplette Verlaufsanzeige für alle ausgeführten Windows PowerShell-Cmdlets inklusive Argumente und Werte. Sie können den Cmdlet-Verlauf an einen anderen Ort kopieren, um ihn dort zu untersuchen, zu verändern und wiederzuverwenden. Sie können Notizen zu Tasks erstellen, um herauszufinden, welche Windows PowerShell-Befehle sich aus Ihren Eingaben im Active Directory-Verwaltungscenter ergeben haben. Außerdem können Sie den Verlauf filtern, um interessante Punkte herauszustellen.

Die PowerShell-Verlaufsanzeige im Active Directory-Verwaltungscenter hilft Ihnen dabei, aus praktischen Erfahrungen zu lernen.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)

Klicken Sie auf die Chevron-Schaltfläche (Pfeil), um die PowerShell-Verlaufsanzeige zu öffnen.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)

Erstellen Sie anschließend einen Benutzer oder ändern Sie die Mitgliedschaft einer Gruppe. Die Verlaufsanzeige wird fortlaufend mit einer reduzierten Ansicht der einzelnen Cmdlets und deren Argumente aktualisiert, die vom Active Directory-Verwaltungscenter ausgeführt wurden.

Erweitern Sie einzelne Positionen, um alle Argumentwerte der Cmdlets anzuzeigen:

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)

Klicken Sie auf das Menü **Aufgabe starten** , um eine Notation manuell einzugeben, bevor Sie ein Objekt im Active Directory-Verwaltungscenter erstellen, ändern oder löschen. Geben Sie eine Beschreibung dessen ein, was Sie tun.  Führen Sie Ihre Änderungen aus und klicken Sie auf **Aufgabe beenden** . Die Task-Notiz gruppiert alle ausgeführten Aktionen in einer reduzierbaren Notiz, die dem besseren Verständnis dient.

Beispiel: Anzeigen der Windows PowerShell-Befehle, mit denen das Kennwort eines Benutzers geändert und der Benutzer aus einer Gruppe entfernt wird:

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)

Wenn Sie das Kontrollkästchen "Alle anzeigen" auswählen, wird außerdem das Get-*-Verb für Windows PowerShell-Cmdlets angezeigt, das zum Abrufen von Daten dient.

![Erweiterte AD DS Verwaltung](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)

Die Verlaufsanzeige enthält die exakten vom Active Directory-Verwaltungscenter ausgeführten Befehle. Es kann den Anschein erwecken, dass manche Cmdlets grundlos ausgeführt werden. Sie können einen Benutzer z. B. mit dem folgenden Cmdlet erstellen:

```powershell
new-aduser
```

und brauchen das folgende Cmdlet dafür nicht:

```powershell
set-adaccountpassword
enable-adaccount
set-aduser
```

Beim Design des Active Directory-Verwaltungscenters wurde die Code-Nutzung minimiert und auf Modularität geachtet. Anstatt also eine Reihe von Funktionen zum Erstellen neuer Benutzer und eine weitere Reihe zum Ändern von Benutzern zu verwenden, werden die einzelnen Funktionen minimal ausgeführt und anschließend über Cmdlets verkettet. Beachten Sie dies, wenn Sie sich mit Active Directory Windows PowerShell auseinander setzen. Dies dient auch als Lerntechnik und zeigt Ihnen, wie einfach Sie einzelne Aufgaben mit Windows PowerShell ausführen können.

## <a name="troubleshooting-ad-ds-management"></a><a name="BKMK_Tshoot"></a>Problembehandlung bei der AD DS-Verwaltung

### <a name="introduction-to-troubleshooting"></a>Einführung in die Problembehandlung

Aufgrund des geringen Alters und des Mangels an existierenden Kundenumgebungen sind die Optionen zur Problembehandlung im Active Directory-Verwaltungscenter eingeschränkt.

### <a name="troubleshooting-options"></a>Optionen zur Problembehebung

#### <a name="logging-options"></a>Protokollierungsoptionen

Die Active Directory-Verwaltungscenter enthält jetzt eine integrierte Protokollierung als Teil einer Ablaufverfolgungs-Konfigurationsdatei. Erstellen/ändern Sie die folgende Datei im gleichen Ordner, der auch dsac.exe enthält:

**dsac.exe.config**

Erstellen Sie den folgenden Inhalt:

```xml
<appSettings>
  <add key="DsacLogLevel" value="Verbose" />
</appSettings>
<system.diagnostics>
 <trace autoflush="false" indentsize="4">
  <listeners>
   <add name="myListener"
    type="System.Diagnostics.TextWriterTraceListener"
    initializeData="dsac.trace.log" />
   <remove name="Default" />
  </listeners>
 </trace>
</system.diagnostics>
```

Die Protokollebenen für **DsacLogLevel** sind **None** , **Error** , **Warning** , **Info** und **Verbose** . Der Name der Ausgabedatei ist konfigurierbar, und die Datei wird im gleichen Ordner geschrieben, der auch dsac.exe enthält. Diese Ausgabe enthält weitere Informationen über den Betrieb von ADAC, die kontaktierten Domänencontroller, ausgeführte Windows PowerShell-Befehle, deren Antworten und sonstige Details.

Beispiel für die Stufe INFO, bei der alle Ergebnisse mit Ausnahme von Ablaufverfolgungsnachrichten angezeigt werden:

- DSAC.exe startet
- Protokollierung startet
- Domänencontroller fragt Ausgangs-Domäneninformationen ab

   ```
   [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)
   [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null
   ```

- Domänencontroller DC1 aus Domäne Corp zurückgegeben
- Virtuelles Laufwerk PS AD geladen

   ```
   [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.
   [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   ```

- DSE-Stamminformationen für die Domäne abrufen

   ```
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   ```

- AD-Papierkorbinformationen für die Domäne abrufen

   ```
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50
   ```

- AD-Gesamtstruktur abrufen

   ```
   [12:42:50][TID 3][Info] Get-ADForest -Identity:"corp.contoso.com" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50
   ```

- Schemainformationen für unterstützte Verschlüsselungstypen, FGPP und bestimmte Benutzer abrufen

   ```
   [12:42:50][TID 3][Info] Get-ADObject
   -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"
   -Properties:lDAPDisplayName
   -ResultPageSize:"100"
   -ResultSetSize:$null
   -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"
   -SearchScope:"OneLevel"
   -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7
   [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50
   ```

- Alle Informationen über das Domänenobjekt abrufen, um dem Administrator anzuzeigen, wer auf den Domänenkopf geklickt hat.

   ```
   [12:42:50][TID 3][Info] Get-ADObject
   -IncludeDeletedObjects:$false
   -LDAPFilter:"(objectClass=*)"
   -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence
   -ResultPageSize:"100"
   -ResultSetSize:"20201"
   -SearchBase:"DC=corp,DC=contoso,DC=com"
   -SearchScope:"Base"
   -Server:"dc1.corp.contoso.com"
   ```

Mit der Stufe "Verbose" werden außerdem die .NET-Stacks für die einzelnen Funktionen angezeigt. Diese enthalten jedoch zu wenig Daten, um besonders nützlich zu sein, insbesondere bei der Problembehandlung von Zugriffsverletzungen oder Abstürzen von Dsac.exe. Die zwei Hauptgründe für dieses Problem sind:

- Der ADWS-Dienst läuft auf keinem der erreichbaren Domänencontroller.
- Die Netzwerkkommunikation wird vom Computer, auf dem die Active Directory-Verwaltungscenter ausgeführt wird, für den ADWS-Dienst blockiert.

> [!IMPORTANT]
> Außerdem existiert eine nicht im Lieferumfang enthaltene Version des Diensts mit dem Namen [Active Directory-Verwaltungsgateway](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), die unter Windows Server 2008 SP2 und Windows Server 2003 SP2 läuft.
>

Wenn keine Active Directory Webdienste-Instanzen installiert sind, werden die folgenden Fehler angezeigt:

|Fehler|Vorgang|
| --- | --- |
|"Es kann keine Verbindung zu irgendeiner Domäne hergestellt werden. Aktualisieren Sie, oder wiederholen Sie den Vorgang, wenn eine Verbindung verfügbar ist"|Angezeigt beim Start der Active Directory-Verwaltungscenter-Anwendung|
|"In der *<NetBIOS domain name>* Domäne, auf der der Active Directory-Webdienst (ADWS) ausgeführt wird, wurde kein verfügbarer Server gefunden"|Angezeigt beim Versuch, einen Domänenknoten in der Active Directory-Verwaltungscenter-Anwendung auszuwählen|

Führen Sie zur Problembehandlung die folgenden Schritte aus:

1. Prüfen Sie, ob der Active Directory Webdienste-Dienst auf mindestens einem Domänencontroller in der Domäne (und bevorzugt auf allen Domänencontrollern in der Gesamtstruktur) gestartet ist. Stellen Sie sicher, dass der Dienst auch auf allen Domänencontrollern automatisch gestartet wird.
2. Prüfen Sie auf dem Computer, auf dem das Active Directory-Verwaltungscenter ausgeführt wird, ob Sie einen Server mit ADWS erreichen können. Verwenden Sie dazu die folgenden NLTest.exe-Befehle:

   ```
   nltest /dsgetdc:<domain NetBIOS name> /ws /force
   nltest /dsgetdc:<domain fully qualified DNS name> /ws /force
   ```

   Wenn diese Tests fehlschlagen, obwohl der ADWS-Dienst läuft, liegt das Problem an der Namensauflösung oder an LDAP und nicht bei ADWS oder beim Active Directory-Verwaltungscenter. Dieser Test schlägt jedoch mit "1355 0x54B ERROR_NO_SUCH_DOMAIN" fehl, wenn ADWS auf keinem Domänencontroller läuft. Prüfen Sie dies also, bevor Sie Schlussfolgerungen ziehen.

3. Rufen Sie eine Liste geöffneter Ports auf den von NLTest zurückgegebenen Domänencontrollern mit dem folgenden Befehl ab:

   ```
   Netstat -anob > ports.txt
   ```

   Untersuchen Sie die Datei ports.txt und vergewissern Sie sich, dass der ADWS-Dienst den Port 9389 geöffnet hat. Beispiel:

   ```
   TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828
   [Microsoft.ActiveDirectory.WebServices.exe]

   TCP    [::]:9389       [::]:0       LISTENING    1828
   [Microsoft.ActiveDirectory.WebServices.exe]
   ```

   Prüfen Sie für den Listening-Modus die Windows-Firewall-Regeln und stellen Sie sicher, dass der TCP-Port 9389 für eingehende Verbindungen erlaubt ist. Standardmäßig aktivieren Domänencontroller die Firewallregel "Active Directory Webdienste (TCP-eingehend)". Falls Sie nicht im Listening-Modus arbeiten, prüfen Sie erneut, ob der Dienst auf diesem Server läuft und starten Sie den Dienst neu. Vergewissern Sie sich, dass kein anderer Prozess den Port 9389 geöffnet hat.

4. Installieren Sie NetMon oder ein anderes Netzwerkerfassungs-Tool auf dem Computer, auf dem das Active Directory-Verwaltungscenter ausgeführt wird und auf dem von NLTEST zurückgegebenen Domänencontroller. Erstellen Sie parallele Netzwerkerfassungen auf beiden Computern während Sie das Active Directory-Verwaltungscenter starten und der Fehler angezeigt wird, bevor Sie die Erfassungen anhalten. Vergewissern Sie sich, dass der Client vom und zum Domänencontroller auf dem TCP-Port 9389 senden bzw. empfangen kann. Falls Pakete gesendet werden und nicht ankommen, oder ankommen und die Antwort des Domänencontrollers den Client nicht erreicht, liegt vermutlich eine Firewall zwischen den Computern im Netzwerk, die die Pakete auf diesem Port abfängt. Dies kann eine Software- oder Hardware-Firewall sein, die möglicherweise zu einem Endpunktschutz eines Drittanbieters (Virenschutz) gehört.

## <a name="see-also"></a>Weitere Informationen

[AD-Papierkorb, differenzierte Kennwortrichtlinie und PowerShell-Verlauf](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)
