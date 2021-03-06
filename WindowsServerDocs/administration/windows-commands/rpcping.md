---
title: rpcping
description: Referenz Artikel für den Befehl "Rpcping", der die RPC-Konnektivität zwischen dem Computer, auf dem Microsoft Exchange Server ausgeführt wird, und allen unterstützten Microsoft Exchange-Client Arbeitsstationen im Netzwerk bestätigt.
ms.topic: reference
ms.assetid: 7382aa0d-90fc-47c0-84b3-15f52dd656d0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7351195d8206cb13b334ca3e06ffb794e4a13461
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641173"
---
# <a name="rpcping"></a>rpcping

> Gilt für: Windows Server (halbjährlicher Kanal), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Bestätigt die RPC-Konnektivität zwischen dem Computer, auf dem Microsoft Exchange Server ausgeführt wird, und den unterstützten Microsoft Exchange-Client Arbeitsstationen im Netzwerk. Dieses Hilfsprogramm kann verwendet werden, um zu überprüfen, ob die Microsoft Exchange Server-Dienste über das Netzwerk auf RPC-Anforderungen von den Client Arbeitsstationen Antworten.

## <a name="syntax"></a>Syntax

```
rpcping [/t <protseq>] [/s <server_addr>] [/e <endpoint>
        |/f <interface UUID>[,majorver]] [/O <interface object UUID]
        [/i <#_iterations>] [/u <security_package_id>] [/a <authn_level>]
        [/N <server_princ_name>] [/I <auth_identity>] [/C <capabilities>]
        [/T <identity_tracking>] [/M <impersonation_type>]
        [/S <server_sid>] [/P <proxy_auth_identity>] [/F <RPCHTTP_flags>]
        [/H <RPC/HTTP_authn_schemes>] [/o <binding_options>]
        [/B <server_certificate_subject>] [/b] [/E] [/q] [/c]
        [/A <http_proxy_auth_identity>] [/U <HTTP_proxy_authn_schemes>]
        [/r <report_results_interval>] [/v <verbose_level>] [/d]
```

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG |
|--|--|
| /t `<protseq>` | Gibt die zu verwendende Protokoll Sequenz an. Kann eine der standardmäßigen RPC-Protokoll Sequenzen sein: ncacn_ip_tcp, ncacn_np oder ncacn_http.<p>Wenn nicht angegeben, wird der Standardwert ncacn_ip_tcp. |
| /s `<server_addr>` | Gibt die Server Adresse an. Wenn nicht angegeben, wird der lokale Computer gepingt. |
| /e `<endpoint>` | Gibt den Endpunkt für den Ping an. Wenn keine Angabe erfolgt, wird die Endpunkt Zuordnung auf dem Zielcomputer gepingt.<p>Diese Option schließt sich mit der Option Interface (**/f**) gegenseitig aus. |
| /o `<binding_options>` | Gibt die Bindungs Optionen für den RPC-Ping an. |
| /f `<interface UUID>[,Majorver]` | Gibt die Schnittstelle für Ping an. Diese Option schließt sich mit der Option Endpunkt gegenseitig aus. Die Schnittstelle wird als UUID angegeben.<p>Wenn die *majorver* nicht angegeben ist, wird die Version 1 der Schnittstelle gesucht.<p>Wenn Interface angegeben wird, fragt **Rpcping** die Endpunkt Zuordnung auf dem Zielcomputer ab, um den Endpunkt für die angegebene Schnittstelle abzurufen. Der Endpunkt Mapper wird mithilfe der in der Befehlszeile angegebenen Optionen abgefragt. |
| /O `<object UUID>` | Gibt die Objekt-UUID an, wenn die Schnittstelle eines registriert hat. |
| /i `<#_iterations>` | Gibt die Anzahl von Aufrufen an. Der Standardwert ist 1. Diese Option ist nützlich zum Messen der Verbindungs Wartezeit, wenn mehrere Iterationen angegeben werden. |
| /u `<security_package_id>` | Gibt das Sicherheitspaket (Sicherheitsanbieter) an, das von RPC zum Ausführen des Aufrufs verwendet wird. Das Sicherheitspaket wird als eine Zahl oder ein Name identifiziert. Wenn eine Zahl verwendet wird, entspricht dies der Nummer der rpcbindingsetauthinfoex-API. Wenn Sie diese Option angeben, müssen Sie eine andere Authentifizierungs Ebene als *keine*angeben. Es gibt keinen Standardwert für diese Option. Wenn Sie nicht angegeben ist, verwendet RPC keine Sicherheit für den Ping. Die nachstehende Liste zeigt die Namen und Zahlen. Bei Namen wird die Groß-/Kleinschreibung beachtet<ul><li>Aushandeln/9 oder eine von "NGO", "snego" oder "aushandeln"</li><li>NTLM/10 oder NTLM</li><li>SChannel/14 oder SChannel</li><li>Kerberos/16 oder Kerberos</li><li>Kernel/20 oder Kernel</li></ul> |
| /a `<authn_level>` | Gibt die zu verwendende Authentifizierungs Ebene an. Wenn diese Option angegeben ist, muss auch die Sicherheitspaket-ID (**/u**) angegeben werden. Wenn diese Option nicht angegeben wird, verwendet RPC keine Sicherheit für den Ping. Es gibt keinen Standardwert für diese Option. Mögliche Werte:<ul><li>Verbinden</li><li>Aufruf</li><li>Pkt</li><li>Integrität</li><li>privacy</li></ul> |
| /N `<server_princ_name>` | Gibt einen Server Prinzipal Namen an.<p>Dieses Feld kann nur verwendet werden, wenn die Authentifizierungs Ebene und das Sicherheitspaket ausgewählt sind. |
| /I `<auth_identity>` | Ermöglicht das Angeben einer alternativen Identität, um eine Verbindung mit dem Server herzustellen. Die Identität hat die Form Benutzer, Domäne und Kennwort. Wenn der Benutzername, die Domäne oder das Kennwort Sonderzeichen enthält, die von der Shell interpretiert werden können, müssen Sie die Identität in doppelte Anführungszeichen einschließen. `\*`Anstatt das Kennwort anzugeben, werden Sie von RPC aufgefordert, das Kennwort einzugeben, ohne es auf dem Bildschirm wiederzugeben. Wenn dieses Feld nicht angegeben wird, wird die Identität des angemeldeten Benutzers verwendet.<p>Dieses Feld kann nur verwendet werden, wenn die Authentifizierungs Ebene und das Sicherheitspaket ausgewählt sind. |
| /C `<capabilities>` | Gibt eine hexadezimale Bitmaske von Flags an. Dieses Feld kann nur verwendet werden, wenn die Authentifizierungs Ebene und das Sicherheitspaket ausgewählt sind. |
| /T `<identity_tracking>` | Gibt Static oder Dynamic an. Wenn nicht angegeben, ist Dynamic der Standard.<p>Dieses Feld kann nur verwendet werden, wenn die Authentifizierungs Ebene und das Sicherheitspaket ausgewählt sind. |
| /M `<impersonation_type>` | Gibt Anonymous, identifizieren, annehmen oder delegieren an. Der Standardwert ist der Identitätswechsel.<p>Dieses Feld kann nur verwendet werden, wenn die Authentifizierungs Ebene und das Sicherheitspaket ausgewählt sind. |
| /S `<server_sid>` | Gibt die erwartete SID des Servers an.<p>Dieses Feld kann nur verwendet werden, wenn die Authentifizierungs Ebene und das Sicherheitspaket ausgewählt sind. |
| /P `<proxy_auth_identity>` | Gibt die Identität an, die für den RPC-/HTTP-Proxy authentifiziert werden soll. Hat das gleiche Format wie bei der Option **/I** . Um diese Option verwenden zu können, müssen Sie das Sicherheitspaket (**/u**), die Authentifizierungs Ebene (**/a**) und die Authentifizierungs Schemas (**/H**) angeben. |
| /F `<RPCHTTP_flags>` | Gibt die zu über gebenden Flags für die RPC/HTTP-Front-End-Authentifizierung an. Die Flags können als Zahlen oder Namen angegeben werden, die derzeit erkannten Flags sind:<ul><li>SSL/1 oder SSL oder use_ssl verwenden</li><li>Verwenden des ersten Authentifizierungs Schemas/2 oder des ersten oder use_first</li></ul>Sie müssen das Sicherheitspaket (**/u**) und die Authentifizierungs Ebene (**/a**) angeben, um diese Option zu verwenden. |
| /H `<RPC/HTTP_authn_schemes>` | Gibt die Authentifizierungs Schemas für die RPC/HTTP-Front-End-Authentifizierung an. Bei dieser Option handelt es sich um eine durch Kommas getrennte Liste numerischer Werte oder Namen. Beispiel: Basic, NTLM. Erkannte Werte sind (bei Namen wird die Groß-/Kleinschreibung beachtet):<ul><li>Basic/1 oder Basic</li><li>NTLM/2 oder NTLM</li><li>Zertifikat/65536 oder Zertifikat</li></ul><p>Sie müssen das Sicherheitspaket (**/u**) und die Authentifizierungs Ebene (**/a**) angeben, um diese Option verwenden zu können. |
| /B `<server_certificate_subject>` | Gibt den Betreff des Serverzertifikats an. Sie müssen SSL verwenden, damit diese Option funktioniert.<p>Sie müssen das Sicherheitspaket (**/u**) und die Authentifizierungs Ebene (**/a**) angeben, um diese Option verwenden zu können. |
| /b | Ruft den Serverzertifikats Betreff aus dem vom Server gesendeten Zertifikat ab und druckt ihn auf einem Bildschirm oder in einer Protokolldatei. Nur gültig, wenn die Option Proxy Echo only (/E) und die Option SSL verwenden angegeben sind.<p>Sie müssen das Sicherheitspaket (**/u**) und die Authentifizierungs Ebene (**/a**) angeben, um diese Option verwenden zu können. |
| /R | Gibt den HTTP-Proxy an. Wenn *keine*, wird der RPC-Proxy verwendet. Der Wert *default* bedeutet, dass die IE-Einstellungen auf dem Client Computer verwendet werden. Alle anderen Werte werden als expliziter http-Proxy behandelt. Wenn Sie dieses Flag nicht angeben, wird der Standardwert angenommen, d. h. die IE-Einstellungen werden geprüft. Dieses Flag ist nur gültig, wenn das Flag **/E** (nur Echo) aktiviert ist. |
| /E | Schränkt das Ping nur auf den RPC/HTTP-Proxy ein. Der Ping erreicht den Server nicht. Nützlich, wenn Sie feststellen möchten, ob der RPC/HTTP-Proxy erreichbar ist. Um einen HTTP-Proxy anzugeben, verwenden Sie das/R-Flag. Wenn ein HTTP-Proxy im/o-Flag angegeben ist, wird diese Option ignoriert.<p>Sie müssen das Sicherheitspaket (**/u**) und die Authentifizierungs Ebene (**/a**) angeben, um diese Option verwenden zu können. |
| /q | Gibt den stillen Modus an. Gibt keine Aufforderungen mit Ausnahme von Kenn Wörtern aus. Geht von der *Y* -Antwort auf alle Abfragen aus. Verwenden Sie diese Option mit Sorgfalt. |
| /C | Smartcardzertifikat verwenden. Rpcping fordert den Benutzer auf, Smartcard auszuwählen. |
| /A | Gibt die Identität an, mit der der HTTP-Proxy authentifiziert werden soll. Hat das gleiche Format wie bei der Option/I.<p>Um diese Option verwenden zu können, müssen Sie die Authentifizierungs Schemas (/U), das Sicherheitspaket (**/U**) und die Authentifizierungs Ebene (**/a**) angeben. |
| /U | Gibt die für die HTTP-Proxy Authentifizierung zu verwendenden Authentifizierungs Schemas an. Bei dieser Option handelt es sich um eine durch Kommas getrennte Liste numerischer Werte oder Namen. Beispiel: Basic, NTLM. Erkannte Werte sind (bei Namen wird die Groß-/Kleinschreibung beachtet):<ul><li>Basic/1 oder Basic</li><li>NTLM/2 oder NTLM</li></ul>Sie müssen das Sicherheitspaket (**/u**) und die Authentifizierungs Ebene (**/a**) angeben, um diese Option verwenden zu können. |
| /r | Wenn mehrere Iterationen angegeben werden, wird die aktuelle Ausführungs Statistik von **Rpcping** in regelmäßigen Abständen nach dem letzten-Aufrufen angezeigt. Das Berichts Intervall wird in Sekunden angegeben. Der Standardwert ist 15. |
| /v | Weist **Rpcping** an, wie ausführlich die Ausgabe ausgegeben werden soll. Der Standardwert ist 1. 2 und 3 bieten weitere Ausgaben von **Rpcping**. |
| /d | Starten der RPC-Netzwerk Diagnose-Benutzeroberfläche |
| /p | Gibt an, dass bei Authentifizierungs Fehlern Anmelde Informationen angefordert werden sollen. |
| /? | Zeigt die Hilfe an der Eingabeaufforderung an. |

## <a name="examples"></a>Beispiele

Geben Sie Folgendes ein, um herauszufinden, ob der Exchange-Server, auf dem Sie über RPC/HTTP eine Verbindung herstellen,

```
rpcping /t ncacn_http /s exchange_server /o RpcProxy=front_end_proxy /P username,domain,* /H Basic /u NTLM /a connect /F 3
```

## <a name="additional-references"></a>Weitere Verweise

- [Erläuterung zur Befehlszeilensyntax](command-line-syntax-key.md)
