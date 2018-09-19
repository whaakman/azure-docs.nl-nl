---
title: Hybride identiteit benodigde poorten en protocollen - Azure | Microsoft Docs
description: Deze pagina wordt een pagina met technische naslaginformatie voor poorten die nodig zijn om te worden geopend voor Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 4ae221efe19d548c3b19cfcebc6728390cabf2b1
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312604"
---
# <a name="hybrid-identity-required-ports-and-protocols"></a>Voor hybride identiteit benodigde poorten en protocollen
Het volgende document is een technische naslaginformatie over de vereiste poorten en protocollen voor het implementeren van een oplossing voor hybride identiteit. Gebruik de volgende afbeelding en verwijzen naar de bijbehorende tabel.

![Wat is Azure AD Connect?](./media/reference-connect-ports/required3.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabel 1: Azure AD Connect en On-premises AD
Deze tabel beschrijft de poorten en protocollen die vereist voor communicatie tussen de Azure AD Connect-server zijn en on-premises AD.

| Protocol | Poorten | Beschrijving |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |DNS-zoekacties van het doelforest. |
| Kerberos |88 (TCP/UDP) |Kerberos-verificatie met de AD-forest. |
| MS-RPC |135 (TCP/UDP) |Gebruikt tijdens de eerste configuratie van de Azure AD Connect-wizard wanneer deze is gekoppeld aan de AD-forest, en ook tijdens Wachtwoordsynchronisatie. |
| LDAP |389 (TCP/UDP) |Gebruikt voor het importeren van gegevens uit Active Directory. Gegevens worden versleuteld met Kerberos ondertekenen & verzegelen. |
| RPC | 445 (TCP/UDP) |Door naadloze eenmalige aanmelding gebruikt voor het maken van een computeraccount in het AD-forest. |
| LDAP/SSL |636 (TCP/UDP) |Gebruikt voor het importeren van gegevens uit Active Directory. De gegevensoverdracht is ondertekend en versleuteld. Alleen als u SSL gebruikt. |
| RPC |49152 - 65535 (willekeurige hoge RPC Port)(TCP/UDP) |Tijdens de eerste configuratie van Azure AD Connect wanneer deze is gekoppeld aan de AD-forests, en tijdens synchronisatie van wachtwoord gebruikt. Zie [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017), en [KB224196](https://support.microsoft.com/kb/224196) voor meer informatie. |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tabel 2 - Azure AD Connect en Azure AD
Deze tabel worden beschreven de poorten en protocollen die vereist voor communicatie tussen de Azure AD Connect-server en Azure AD zijn.

| Protocol | Poorten | Beschrijving |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Voor het downloaden van CRL's (certificaatintrekkingslijsten) om te controleren of SSL-certificaten. |
| HTTPS |443(TCP/UDP) |Gebruikt om te synchroniseren met Azure AD. |

Voor een lijst van URL's en IP-adressen die u wilt openen in uw firewall, Zie [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>Tabel 3 - Azure AD Connect en AD FS-Federation-Servers/WAP
Deze tabel worden beschreven de poorten en protocollen die vereist voor communicatie tussen de AD FS-Federation/WAP-servers en Azure AD Connect-server zijn.  

| Protocol | Poorten | Beschrijving |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Voor het downloaden van CRL's (certificaatintrekkingslijsten) om te controleren of SSL-certificaten. |
| HTTPS |443(TCP/UDP) |Gebruikt om te synchroniseren met Azure AD. |
| WinRM |5985 |WinRM-Listener |

## <a name="table-4---wap-and-federation-servers"></a>Tabel 4 - WAP en Federation-Servers
Deze tabel worden de poorten en protocollen die vereist voor communicatie tussen de Federation-servers en WAP-servers zijn beschreven.

| Protocol | Poorten | Beschrijving |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Gebruikt voor verificatie. |

## <a name="table-5---wap-and-users"></a>Tabel 5 - WAP en gebruikers
Deze tabel worden de poorten en protocollen die vereist voor communicatie tussen gebruikers en de WAP-servers zijn beschreven.

| Protocol | Poorten | Beschrijving |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Gebruikt voor verificatie van apparaten. |
| TCP |49443 (TCP) |Gebruikt voor verificatie via certificaat. |

## <a name="table-6a--6b---pass-through-authentication-with-single-sign-on-sso-and-password-hash-sync-with-single-sign-on-sso"></a>Tabel 6a & 6ter - Pass through-verificatie met eenmalige aanmelding (SSO) en Wachtwoordhashsynchronisatie met eenmalige aanmelding (SSO)
De volgende tabel beschrijft de poorten en protocollen die vereist voor communicatie tussen de Azure AD Connect en Azure AD zijn.

### <a name="table-6a---pass-through-authentication-with-sso"></a>Tabel 6a - Pass through-verificatie met eenmalige aanmelding
|Protocol|Poortnummer|Beschrijving
| --- | --- | ---
|HTTP|80|Uitgaande HTTP-verkeer voor beveiligingsvalidatie zoals SSL inschakelen. Ook die nodig zijn voor de mogelijkheid voor het automatisch bijwerken van connector te laten functioneren.
|HTTPS|443| Uitgaande HTTPS-verkeer voor bewerkingen zoals het inschakelen en uitschakelen van de functie, connectors registreren, connector updates downloaden en verwerken van alle gebruiker aanmeldingsaanvragen inschakelen.

Daarnaast Azure AD Connect moet kunnen direct IP-verbindingen maken met de [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="table-6b---password-hash-sync-with-sso"></a>Tabel 6 ter - synchronisatie van Wachtwoordhashes met eenmalige aanmelding

|Protocol|Poortnummer|Beschrijving
| --- | --- | ---
|HTTPS|443| SSO-registratie (alleen vereist voor het registratieproces SSO) inschakelen.

Daarnaast Azure AD Connect moet kunnen direct IP-verbindingen maken met de [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Nogmaals, dit is alleen vereist voor het registratieproces voor eenmalige aanmelding.

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabel 7a & 7b - Azure AD Connect Health-agent voor (AD FS/Sync) en Azure AD
De volgende tabellen beschrijven de eindpunten, poorten en protocollen die vereist voor communicatie tussen Azure AD Connect Health-agents en Azure AD zijn

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabel 7 - poorten en protocollen voor Azure AD Connect Health-agent voor (AD FS/Sync) en Azure AD
Deze tabel beschrijft de volgende uitgaande poorten en protocollen die vereist voor communicatie tussen de Azure AD Connect Health-agents en de Azure AD zijn.  

| Protocol | Poorten | Beschrijving |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Uitgaand |
| Azure Service Bus |5671 (TCP/UDP) |Uitgaand |

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>7B - eindpunten voor Azure AD Connect Health-agent voor (AD FS/Sync) en Azure AD
Zie voor een lijst met eindpunten [de sectie vereisten voor de Azure AD Connect Health-agent](how-to-connect-health-agent-install.md#requirements).

