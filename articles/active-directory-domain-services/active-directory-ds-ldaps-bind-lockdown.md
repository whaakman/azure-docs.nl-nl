---
title: Binden met behulp van Secure LDAP (LDAPS) aan een Azure AD Domain Services beheerd domein | Microsoft Docs
description: Een binding met een door Azure AD Domain Services beheerd domein maken met behulp van secure LDAP (LDAPS)
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6871374a-0300-4275-9a45-a39a52c65ae4
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: cbc5bee0f4cc59f59af6e3f57219279cd8fcb030
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988582"
---
# <a name="bind-to-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Een binding met een door Azure AD Domain Services beheerd domein maken met behulp van secure LDAP (LDAPS)

## <a name="before-you-begin"></a>Voordat u begint
[Taak 4 volt ooien: CONFIGUREER DNS voor toegang tot het beheerde domein via internet](active-directory-ds-ldaps-configure-dns.md).


## <a name="task-5-bind-to-the-managed-domain-over-ldap-using-ldpexe"></a>Taak 5: Verbinding maken met het beheerde domein via LDAP met behulp van LDP. exe
U kunt het hulp programma LDP. exe gebruiken, dat is opgenomen in het pakket Remote Server Administration Tools voor het binden en doorzoeken van LDAP.

Open eerst LDP en maak verbinding met het beheerde domein. Klik op **verbinding** en klik op **verbinden...** in het menu. Geef de DNS-domein naam van het beheerde domein op. Geef de poort op die moet worden gebruikt voor verbindingen. Gebruik voor LDAP-verbindingen poort 389. Gebruik poort 636 voor LDAPS-verbindingen. Klik op de knop **OK** om verbinding te maken met het beheerde domein.

Maak vervolgens een binding met het beheerde domein. Klik op **verbinding** en klik in het menu op **binden...** . Geef de referenties op van een gebruikers account dat deel uitmaakt van de groep AAD DC Administrators.

> [!IMPORTANT]
> Gebruikers (en service accounts) kunnen geen LDAP-eenvoudige bindingen uitvoeren als NTLM-wachtwoord synchronisatie is uitgeschakeld op uw Azure AD Domain Services-exemplaar.  Lees [Beveilig uw door Azure AD Domain Services beheerde domein](secure-your-domain.md)voor meer informatie over het uitschakelen van NTLM-wachtwoord synchronisatie.
>
>

Selecteer **weer gave**en selecteer vervolgens **structuur** in het menu. Laat het veld basis-DN leeg en klik op OK. Ga naar de container waarin u wilt zoeken, klik met de rechter muisknop op de container en selecteer zoeken.

> [!TIP]
> - Gebruikers en groepen die zijn gesynchroniseerd vanuit Azure AD worden opgeslagen in de organisatie-eenheid **AADDC-gebruikers** . Het zoekpad voor deze organisatie-eenheid ziet er als ```OU=AADDC Users,DC=CONTOSO100,DC=COM```volgt uit.
> - Computer accounts voor computers die zijn gekoppeld aan het beheerde domein, worden opgeslagen in de organisatie-eenheid **AADDC computers** . Het zoekpad voor deze organisatie-eenheid ziet er als ```OU=AADDC Computers,DC=CONTOSO100,DC=COM```volgt uit.
>
>

Meer informatie- [basis beginselen van LDAP-query's](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)


## <a name="task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>Taak 6: Veilige LDAP-toegang tot uw beheerde domein via internet vergren delen
> [!NOTE]
> Als u LDAPS-toegang tot het beheerde domein niet via internet hebt ingeschakeld, kunt u deze configuratie taak overs Laan.
>
>

Voordat u met deze taak begint, voert u de stappen uit die worden beschreven in [taak 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md).

Wanneer u LDAPS-toegang via internet naar uw beheerde domein inschakelt, wordt er een beveiligings risico gegenereerd. Het beheerde domein is bereikbaar vanaf internet op de poort die wordt gebruikt voor beveiligde LDAP (poort 636). U kunt ervoor kiezen om de toegang tot het beheerde domein te beperken tot specifieke IP-adressen. Maak een netwerk beveiligings groep (NSG) en koppel deze aan het subnet waar u Azure AD Domain Services hebt ingeschakeld.

De voor beeld-NSG in de volgende tabel vergrendelt beveiligde LDAP-toegang via internet. De regels in de NSG geven binnenkomende beveiligde LDAP-toegang via TCP-poort 636 alleen uit een opgegeven reeks IP-adressen. De standaard regel DenyAll is van toepassing op alle andere inkomende verkeer van het internet. De NSG-regel voor het toestaan van LDAPS-toegang via internet vanaf opgegeven IP-adressen heeft een hogere prioriteit dan de DenyAll NSG-regel.

![Voor beeld van NSG voor het beveiligen van LDAPS-toegang via Internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Meer informatie** - [netwerk beveiligings groepen](../virtual-network/security-overview.md).


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services aan de slag-hand leiding](create-instance.md)
* [Een Azure AD Domain Services domein beheren](manage-domain.md)
* [Basis principes LDAP-query's](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)
* [groepsbeleid voor Azure AD Domain Services beheren](manage-group-policy.md)
* [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md)
* [Een netwerk beveiligings groep maken](../virtual-network/tutorial-filter-network-traffic.md)


## <a name="next-step"></a>Volgende stap
[Problemen met het beveiligen van LDAP op een beheerd domein oplossen](tshoot-ldaps.md)
