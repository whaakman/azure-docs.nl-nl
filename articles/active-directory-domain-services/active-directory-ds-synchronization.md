---
title: 'Azure Active Directory Domain Services: Synchronisatie in beheerde domeinen | Microsoft Docs'
description: Inzicht in synchronisatie in een Azure Active Directory Domain Services beheerde domein
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: ergreenl
ms.openlocfilehash: b5544be37015368173f3f8eb39288b7a97ae11c6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58084994"
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronisatie in een Azure AD Domain Services beheerde domein
Het volgende diagram illustreert hoe synchronisatie werkt in Azure AD Domain Services beheerde domeinen.

![Synchronisatie in Azure AD Domain Services](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronisatie tussen uw on-premises directory en uw Azure AD-tenant
Azure AD Connect-synchronisatie wordt gebruikt voor het synchroniseren van gebruikersaccounts, groepslidmaatschappen en referentie-hashes naar uw Azure AD-tenant. Kenmerken van de gebruiker, zoals de UPN-accounts en on-premises SID (security identifier) worden gesynchroniseerd. Als u Azure AD Domain Services gebruikt, worden ook verouderde referentie-hashes voor NTLM en Kerberos-verificatie met uw Azure AD-tenant gesynchroniseerd.

Als u terugschrijven configureert, worden wijzigingen moet worden uitgevoerd in uw Azure AD-directory gesynchroniseerd naar uw on-premises Active Directory. Bijvoorbeeld, als u uw wachtwoord met behulp van Azure AD selfservice wachtwoordbeheer wijzigt, het gewijzigde wachtwoord is bijgewerkt in uw on-premises AD-domein.

> [!NOTE]
> Gebruik altijd de meest recente versie van Azure AD Connect om ervoor te zorgen dat u oplossingen voor alle bekende fouten optreden.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronisatie van uw Azure AD-tenant met uw beheerde domein
Accounts voor gebruikers, groepslidmaatschappen en referentie-hashes worden gesynchroniseerd vanuit uw Azure AD-tenant aan uw Azure AD Domain Services beheerde domein. Dit synchronisatieproces wordt automatisch. U hoeft niet te configureren, controleren of dit synchronisatieproces beheren. Initiële synchronisatie kan duren vanaf een paar uur een aantal dagen, afhankelijk van het aantal objecten in uw Azure AD-directory. Nadat de initiële synchronisatie is voltooid, duurt het ongeveer 20-30 minuten voor wijzigingen die zijn aangebracht in Azure AD in uw beheerde domein worden bijgewerkt. Deze synchronisatie-interval is van toepassing op wachtwoord te wijzigen of wijzigingen in kenmerken die u in Azure AD.

Het proces van synchronisatie is ook een-way/Unidirectioneel van aard. Uw beheerde domein is grotendeels alleen-lezen, met uitzondering van eventuele aangepaste OE's die u maakt. Daarom kan geen u wijzigingen aanbrengen op gebruikerskenmerken, wachtwoorden van gebruikers of lidmaatschap van groepen binnen het beheerde domein. Als gevolg hiervan is er geen omgekeerde synchronisatie van wijzigingen in uw beheerde domein terug naar uw Azure AD-tenant.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronisatie van een forest met meerdere on-premises-omgeving
Veel organisaties hebben een relatief complex on-premises infrastructuur die bestaan uit meerdere accountforests. Azure AD Connect ondersteunt het synchroniseren van gebruikers, groepen en referentie-hashes van omgevingen met meerdere forests met uw Azure AD-tenant.

Uw Azure AD-tenant is daarentegen een veel eenvoudiger en platte naamruimte. Als u wilt dat gebruikers kunnen op betrouwbare wijze toegang tot toepassingen die zijn beveiligd door Azure AD, UPN conflicten worden opgelost voor gebruikersaccounts in verschillende forests bevinden. Uw Azure AD Domain Services beheerde domein bears sluit gelijkenis met uw Azure AD-tenant. U ziet een platte OE-structuur in uw beheerde domein. Alle gebruikersaccounts en groepen worden opgeslagen in de container 'AADDC gebruikers', ondanks dat wordt gesynchroniseerd vanaf verschillende on-premises domeinen of forests. U hebt een hiërarchische organisatie-eenheid geconfigureerd structuur, on-premises. Uw beheerde domein heeft nog steeds een eenvoudige, platte OE-structuur.

## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Uitsluitingen - wat is niet gesynchroniseerd met uw beheerde domein
De volgende kenmerken of objecten zijn niet gesynchroniseerd naar uw Azure AD-tenant of naar uw beheerde domein:

* **Uitgesloten kenmerken:** U wilt uitsluiten van bepaalde kenmerken gesynchroniseerd met uw Azure AD-tenant van uw on-premises domein met Azure AD Connect. Deze uitgesloten kenmerken zijn niet beschikbaar in uw beheerde domein.
* **Groepsbeleid:** Groepsbeleid is geconfigureerd in uw on-premises domein zijn niet gesynchroniseerd met uw beheerde domein.
* **Sysvol-share:** De inhoud van de Sysvol-share op uw on-premises domein zijn op deze manier niet gesynchroniseerd met uw beheerde domein.
* **Computerobjecten:** Computerobjecten voor computers die zijn gekoppeld aan uw on-premises domein zijn niet gesynchroniseerd met uw beheerde domein. Deze computers niet hebben een vertrouwensrelatie met uw beheerde domein en deel uitmaken van uw on-premises domein. Zoek in uw beheerde domein computerobjecten alleen voor computers die u hebt expliciet domein aan het beheerde domein.
* **De kenmerken van de SID-geschiedenis voor gebruikers en groepen:** De primaire gebruiker en de primaire groeps-SID's van uw on-premises domein worden gesynchroniseerd met uw beheerde domein. Bestaande SID-geschiedenis kenmerken voor gebruikers en groepen worden echter niet gesynchroniseerd vanuit uw on-premises domein aan uw beheerde domein.
* **Structuur van organisatie-eenheden (OE):** Organisatie-eenheden die zijn gedefinieerd in uw on-premises domein synchroniseren niet met uw beheerde domein. Er zijn twee ingebouwde OE's in uw beheerde domein. Uw beheerde domein heeft standaard een platte OE-structuur. U kunt er echter voor kiezen om [een aangepaste organisatie-eenheid maken in uw beheerde domein](active-directory-ds-admin-guide-create-ou.md).

## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Hoe specifieke kenmerken worden gesynchroniseerd met uw beheerde domein
De volgende tabel bevat enkele algemene kenmerken en wordt beschreven hoe ze zijn gesynchroniseerd met uw beheerde domein.

| Kenmerk in uw beheerde domein | Bron | Opmerkingen |
|:--- |:--- |:--- |
| UPN |UPN-kenmerk van de gebruiker in uw Azure AD-tenant |Het UPN-kenmerk uit uw Azure AD-tenant is gesynchroniseerd met uw beheerde domein is. Daarom is de meest betrouwbare manier om te melden bij uw beheerde domein met behulp van de UPN. |
| SAMAccountName |MailNickname van de gebruiker in uw Azure AD-tenant van het kenmerk of automatisch gegenereerde |Het kenmerk SAMAccountName afkomstig is uit het kenmerk mailNickname in uw Azure AD-tenant. Als meerdere gebruikersaccounts het kenmerk mailNickname van dezelfde hebben, wordt de SAMAccountName automatisch gegenereerd. Als de mailNickname of de UPN-voorvoegsel van de gebruiker meer dan 20 tekens is, is de SAMAccountName automatisch gegenereerde om te voldoen aan de limiet van 20 tekens op SAMAccountName kenmerken. |
| Wachtwoorden |Het wachtwoord van gebruiker in uw Azure AD-tenant |Referentie-hashes voor NTLM of Kerberos-verificatie (ook wel aanvullende referenties) worden gesynchroniseerd vanuit uw Azure AD-tenant. Als uw Azure AD-tenant een gesynchroniseerde tenant is, worden deze referenties zijn afkomstig van uw on-premises domein. |
| Primaire gebruiker of groep SID |Automatisch gegenereerde |De primaire SID voor gebruiker/groep accounts wordt automatisch gegenereerd in uw beheerde domein. Dit kenmerk komt niet overeen met de primaire gebruiker/groep SID van het object in uw on-premises AD-domein. Dit verschil is dat het beheerde domein een andere SID-naamruimte dan uw on-premises domein heeft. |
| SID-geschiedenis voor gebruikers en groepen |On-premises primaire gebruiker en groep-SID |Het kenmerk SidHistory voor gebruikers en groepen in uw beheerde domein is ingesteld zodat deze overeenkomen met de bijbehorende primaire gebruiker of groep SID in uw on-premises domein. Deze functie helpt eenvoudiger lift-and-shift van on-premises toepassingen bij het beheerde domein, omdat u niet re-ACL-resources wilt. |

> [!NOTE]
> **Meld u aan het beheerde domein met behulp van de UPN-indeling:** Het kenmerk SAMAccountName mogelijk automatisch wordt gegenereerd voor sommige gebruikersaccounts in uw beheerde domein. Als meerdere gebruikers het kenmerk mailNickname van dezelfde hebben of gebruikers overmatig lange UPN-voorvoegsels hebben, kan de SAMAccountName voor deze gebruikers worden automatisch gegenereerde. Daarom is de SAMAccountName-indeling (bijvoorbeeld ' CONTOSO100\joeuser') niet altijd een betrouwbare manier om aan te melden bij het domein. Gebruikers automatisch gegenereerde SAMAccountName kan afwijken van de UPN-voorvoegsel. Gebruik de UPN-indeling (bijvoorbeeld 'joeuser@contoso100.com') op een betrouwbare manier aanmelden bij het beheerde domein.

### <a name="attribute-mapping-for-user-accounts"></a>Kenmerktoewijzing voor gebruikersaccounts
De volgende tabel ziet u hoe specifieke kenmerken voor objecten in uw Azure AD-tenant worden gesynchroniseerd met de bijbehorende kenmerken in uw beheerde domein gebruiker.

| Gebruikerskenmerk in uw Azure AD-tenant | Gebruikerskenmerk in uw beheerde domein |
|:--- |:--- |
| accountEnabled |userAccountControl (wordt ingesteld of wordt gewist van het ACCOUNT_DISABLED-bits) |
| city |l |
| Land/regio |CO |
| department |department |
| displayName |displayName |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| givenName |givenName |
| Functie |titel |
| mail |mail |
| mailNickname |msDS-AzureADMailNickname |
| mailNickname |SAMAccountName (soms is automatisch gegenereerd) |
| mobiele |mobiele |
| object-id |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID-geschiedenis |
| passwordPolicies |userAccountControl (wordt ingesteld of wordt gewist van het DONT_EXPIRE_PASSWORD-bits) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| Postcode |Postcode |
| preferredLanguage |preferredLanguage |
| state |St |
| streetAddress |streetAddress |
| surname |SN |
| telephoneNumber |telephoneNumber |
| userPrincipalName |userPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Kenmerktoewijzing voor groepen
De volgende tabel ziet u hoe specifieke kenmerken voor groep objecten in uw Azure AD-tenant worden gesynchroniseerd met de bijbehorende kenmerken in uw beheerde domein.

| Kenmerk van de groep in uw Azure AD-tenant | Kenmerk van de groep in uw beheerde domein |
|:--- |:--- |
| displayName |displayName |
| displayName |SAMAccountName (soms is automatisch gegenereerd) |
| mail |mail |
| mailNickname |msDS-AzureADMailNickname |
| object-id |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID-geschiedenis |
| securityEnabled |groupType |

## <a name="password-hash-synchronization-and-security-considerations"></a>Wachtwoord-hash-synchronisatie en de beveiligingsoverwegingen
Wanneer u Azure AD Domain Services inschakelt, wordt uw Azure AD-directory genereert en slaat wachtwoord-hashes in NTLM en Kerberos-compatibel indelingen. 

Worden voor bestaande cloud-gebruikersaccounts, omdat Azure AD nooit hun wachtwoorden niet-versleutelde tekst, slaat deze hashes kunnen niet automatisch gegenereerd. Daarom moet [cloud-gebruikers hun wachtwoord opnieuw instellen/wijzigen](active-directory-ds-getting-started-password-sync.md) in volgorde van hun wachtwoord-hashes worden gegenereerd en opgeslagen in Azure AD. Voor elk cloud-gebruikersaccount hebt gemaakt in Azure AD nadat Azure AD Domain Services is ingeschakeld, worden de wachtwoord-hashes gegenereerd en opgeslagen in de indelingen van NTLM en Kerberos-compatibel. 

Voor gebruikersaccounts die zijn gesynchroniseerd vanuit on-premises AD met behulp van Azure AD Connect Sync, moet u [configureren van Azure AD Connect voor het synchroniseren van wachtwoord-hashes in de indelingen van NTLM en Kerberos-compatibele](active-directory-ds-getting-started-password-sync-synced-tenant.md).

De compatibel wachtwoord-hashes van NTLM en Kerberos worden altijd versleuteld opgeslagen in Azure AD. Deze hashes worden versleuteld zodat alleen Azure AD Domain Services heeft toegang tot de ontsleutelingssleutels. Er zijn geen andere services of onderdelen in Azure AD heeft toegang tot de ontsleutelingssleutels. De versleutelingssleutels zijn unieke per Azure AD-tenant. Azure AD Domain Services synchroniseert de wachtwoord-hashes naar de domeincontrollers voor uw beheerde domein. Deze wachtwoord-hashes worden opgeslagen en beveiligd op deze domeincontrollers die vergelijkbaar is met de manier waarop wachtwoorden worden opgeslagen en beveiligd op Windows Server AD-domeincontrollers. De schijven voor deze beheerde domeincontrollers worden in rust versleuteld.

## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objecten die niet zijn gesynchroniseerd met uw Azure AD-tenant van uw beheerde domein
Als in een vorige sectie van dit artikel wordt beschreven, is er geen synchronisatie uit uw beheerde domein terug naar uw Azure AD-tenant. U kunt kiezen om [maken van een aangepaste organisatie-eenheid (OE)](active-directory-ds-admin-guide-create-ou.md) in uw beheerde domein. U kunt bovendien andere organisatie-eenheden, gebruikers, groepen of service-accounts binnen deze aangepaste OE's maken. Geen van de objecten die zijn gemaakt in aangepaste OE's worden gesynchroniseerd naar uw Azure AD-tenant. Deze objecten zijn beschikbaar voor gebruik binnen uw beheerde domein. Deze objecten zijn daarom niet zichtbaar met behulp van Azure AD PowerShell-cmdlets, Azure AD Graph API of de gebruikersinterface voor het beheer van de Azure AD.

## <a name="related-content"></a>Gerelateerde inhoud
* [Functies - Azure AD Domain Services](active-directory-ds-features.md)
* [Implementatiescenario's - Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Aandachtspunten voor netwerken voor Azure AD Domain Services](active-directory-ds-networking.md)
* [Aan de slag met Azure AD Domain Services](active-directory-ds-getting-started.md)
