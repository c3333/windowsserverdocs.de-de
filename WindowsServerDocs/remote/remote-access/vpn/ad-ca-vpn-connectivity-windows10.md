---
title: Bedingter Zugriff für VPN-Konnektivität mit Azure AD
description: In diesem optionalen Schritt können Sie optimieren, wie autorisierte VPN-Benutzer mithilfe des bedingten Zugriffs von Azure Active Directory (Azure AD) auf Ihre Ressourcen zugreifen.
ms.topic: article
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: 9c57d120106041692b920891b7d0c3341daec314
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958228"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Schritt 7. Optionale Bedingter Zugriff für VPN-Konnektivität mithilfe von Azure AD

- [**Vorheriges:** Schritt 6: Konfigurieren von Windows 10-Client Always on-VPN-Verbindungen](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**Weiter:** Schritt 7,1. Konfigurieren von EAP-TLS zum Ignorieren der CRL-Überprüfung (Zertifikat Sperr Liste)](vpn-config-eap-tls-to-ignore-crl-checking.md)

In diesem optionalen Schritt können Sie optimieren, wie VPN-Benutzer mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](/azure/active-directory/active-directory-conditional-access-azure-portal)auf Ihre Ressourcen zugreifen. Mit Azure AD bedingten Zugriff für VPN-Konnektivität (virtuelles privates Netzwerk) können Sie die VPN-Verbindungen schützen. Beim bedingten Zugriff handelt es sich um ein richtlinienbasiertes Auswertungsmodul, mit dem Sie Zugriffsregeln für alle mit Azure Active Directory (Azure AD) verknüpften Anwendungen erstellen können.

## <a name="prerequisites"></a>Voraussetzungen

Sie sind mit den folgenden Themen vertraut:

- [Bedingter Zugriff in Azure Active Directory](/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN und bedingter Zugriff](/windows/access-protection/vpn/vpn-conditional-access)

Zum Konfigurieren Azure Active Directory bedingten Zugriffs für VPN-Konnektivität muss Folgendes konfiguriert sein:

- [Server Infrastruktur](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [RAS-Server für Always on-VPN](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Netzwerkrichtlinienserver](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS-und Firewalleinstellungen](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Windows 10-Client Always on-VPN-Verbindungen](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>[Schritt 7.1: Konfigurieren von EAP-TLS für das Ignorieren der Zertifikatssperrlisten-Überprüfung](vpn-config-eap-tls-to-ignore-crl-checking.md)

In diesem Schritt können Sie **IgnoreNoRevocationCheck** hinzufügen und festlegen, dass die Authentifizierung von Clients zulässig ist, wenn das Zertifikat keine CRL-Verteilungs Punkte enthält. Standardmäßig ist IgnoreNoRevocationCheck auf 0 (deaktiviert) festgelegt.

Ein EAP-TLS-Client kann keine Verbindung herstellen, es sei denn, der NPS-Server schließt eine Sperr Überprüfung der Zertifikat Kette (einschließlich des Stamm Zertifikats) ab. Cloud-Zertifikate, die von Azure AD an den Benutzer ausgegeben werden, verfügen nicht über eine Zertifikat Sperr Liste, da es sich um kurzlebige Zertifikate mit einer Lebensdauer von einer Stunde handelt. EAP auf NPS muss so konfiguriert werden, dass das Fehlen einer CRL ignoriert wird. Da die Authentifizierungsmethode EAP-TLS ist, wird dieser Registrierungs Wert nur unter **eap\13**benötigt. Wenn andere EAP-Authentifizierungsmethoden verwendet werden, sollte der Registrierungs Wert ebenfalls hinzugefügt werden.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-ad"></a>[Schritt 7.2: Erstellen von Stammzertifikaten für die VPN-Authentifizierung mit Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

In diesem Schritt konfigurieren Sie Stamm Zertifikate für die VPN-Authentifizierung mit Azure AD, wodurch automatisch eine VPN-Server-Cloud-App im Mandanten erstellt wird.

Erforderliche Schritte zum Konfigurieren des bedingten Zugriffs für VPN-Verbindungen:

1. Erstellen eines VPN-Zertifikats über das Azure-Portal
2. Herunterladen des VPN-Zertifikats
3. Bereitstellen des Zertifikats auf Ihrem VPN-Server

> [!IMPORTANT]
> Sobald ein VPN-Zertifikat in der Azure-Portal erstellt wurde, wird es von Azure AD sofort verwendet, um dem VPN-Client kurzlebige Zertifikate auszustellen. Es ist wichtig, dass das VPN-Zertifikat sofort auf dem VPN-Server bereitgestellt wird, um Probleme mit der Überprüfung der Anmelde Informationen des VPN-Clients zu vermeiden.

## <a name="step-73-configure-the-conditional-access-policy"></a>[Schritt 7.3: Konfigurieren der Richtlinie für bedingten Zugriff](vpn-config-conditional-access-policy.md)

In diesem Schritt konfigurieren Sie die Richtlinie für bedingten Zugriff für VPN-Konnektivität.

Zum Konfigurieren der Richtlinie für bedingten Zugriff müssen Sie folgende Schritte ausführen:

1. Erstellen Sie eine Richtlinie für bedingten Zugriff, die VPN-Benutzern zugewiesen wird.
2. Legen Sie die Cloud-App auf **VPN-Server**fest.
3. Legen Sie für Grant (Zugriffs Steuerung) die **Multi-Factor Authentication**fest.  Sie können bei Bedarf andere Steuerelemente verwenden.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>[Schritt 7,4. Bereitstellen von Stamm Zertifikaten für den bedingten Zugriff im lokalen AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

In diesem Schritt stellen Sie ein vertrauenswürdiges Stamm Zertifikat für die VPN-Authentifizierung für das lokale Ad bereit.

Zum Bereitstellen des vertrauenswürdigen Stamm Zertifikats müssen Sie folgende Schritte ausführen:

1. Fügen Sie das heruntergeladene Zertifikat als *Vertrauenswürdige*Stamm Zertifizierungsstelle für die VPN-Authentifizierung hinzu.
2. Importieren Sie das Stamm Zertifikat auf den VPN-Server und den VPN-Client.
3. Vergewissern Sie sich, dass die Zertifikate vorhanden sind und als vertrauenswürdig angezeigt werden.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>[Schritt 7.5: Erstellen von OMA-DM-basierten VPNv2-Profilen für Windows 10-Geräte](vpn-create-oma-dm-based-vpnv2-profiles.md)

In diesem Schritt können Sie OMA-DM-basierte VPNv2-Profile mithilfe von InTune erstellen, um eine VPN-Geräte Konfigurationsrichtlinie bereitzustellen. Wenn Sie zum Erstellen von VPNv2-Profilen Configuration Manager oder PowerShell-Skript verwenden möchten, finden Sie weitere Informationen unter [VPNv2 CSP Settings](/windows/client-management/mdm/vpnv2-csp) .

## <a name="next-steps"></a>Nächste Schritte

[Schritt 7,1. Konfigurieren von EAP-TLS zum Ignorieren der CRL-Überprüfung (Zertifikat Sperr Liste)](vpn-config-eap-tls-to-ignore-crl-checking.md): in diesem Schritt müssen Sie **IgnoreNoRevocationCheck** hinzufügen und festlegen, dass die Authentifizierung von Clients zugelassen wird, wenn das Zertifikat keine CRL-Verteilungs Punkte enthält. Standardmäßig ist IgnoreNoRevocationCheck auf 0 (deaktiviert) festgelegt.

## <a name="related-topics"></a>Verwandte Themen

- [Konfigurieren von VPNv2-Profilen](/windows/access-protection/vpn/vpn-conditional-access): der VPN-Client ist nun in der Lage, die cloudbasierte Plattform für den bedingten Zugriff zu integrieren, um eine Geräte Kompatibilitäts Option für Remote Clients bereitzustellen. In diesem Schritt konfigurieren Sie die VPNv2-Profile mit " ** \<DeviceCompliance> \<Enabled> true \</Enabled> **".

- [Verbessern des Remote Zugriffs in Windows 10 mit einem automatischen VPN-Profil](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): erfahren Sie, wie Microsoft den bedingten Zugriff für VPN-Konnektivität implementiert. VPN-Profile enthalten alle Informationen, die ein Gerät für die Verbindung mit dem Unternehmensnetzwerk benötigt, einschließlich der unterstützten Authentifizierungsmethoden und des VPN-Servers, mit dem das Gerät eine Verbindung herstellen soll. Änderungen in Windows 10 Anniversary Update, einschließlich bedingtem Zugriff und Single Sign-on, ermöglichten es uns, unser Always-on-VPN-Verbindungsprofil zu erstellen.

- [Bedingter Zugriff in Azure Active Directory](/azure/active-directory/active-directory-conditional-access-azure-portal): Sicherheit ist bei Organisationen, die die Cloud nutzen, ein Hauptanliegen. Wichtige Aspekte der Cloudsicherheit, wenn es um die Verwaltung von Cloudressourcen geht, sind Identität und Zugriff. In einer von Mobilgeräten und der Cloud geprägten Welt können Benutzer mithilfe einer Vielzahl von Geräten und Apps von überall aus auf Ressourcen in Ihrer Organisation zugreifen. Daher reicht eine Konzentration darauf, wer auf Ihre Ressourcen Zugriff hat, nicht mehr aus. Um das Gleichgewicht zwischen Sicherheit und Produktivität zu bewahren, müssen IT-Experten bei der Wahl der Zugriffssteuerung auch berücksichtigen, auf welche Weise auf eine Ressource zugegriffen wird.

- [VPN und bedingter Zugriff](/windows/access-protection/vpn/vpn-conditional-access): der VPN-Client ist jetzt in der Lage, die cloudbasierte Plattform für den bedingten Zugriff zu integrieren, um eine Geräte Kompatibilitäts Option für Remote Clients bereitzustellen. Beim bedingten Zugriff handelt es sich um ein richtlinienbasiertes Auswertungsmodul, mit dem Sie Zugriffsregeln für alle mit Azure Active Directory (Azure AD) verknüpften Anwendungen erstellen können.
