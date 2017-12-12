---
title: 'Azure Active Directory Domain Services: Synchronisatie in beheerde domeinen | Microsoft Docs'
description: Inzicht in synchronisatie in een beheerd domein van Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 5c324ea5e268d97134202eff6e96764bedc6ca75
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Een beheerd domein van Azure AD Domain Services-synchronisatie
Het volgende diagram illustreert hoe synchronisatie werkt in Azure AD Domain Services beheerde domeinen.

![Synchronisatie-topologie in Azure AD Domain Services](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronisatie van uw on-premises directory naar uw Azure AD-tenant
Azure AD Connect-synchronisatie wordt gebruikt om te synchroniseren van gebruikersaccounts, groepslidmaatschappen en referentie-hashes naar uw Azure AD-tenant. Kenmerken van een gebruiker zoals de UPN-accounts en on-premises SID (security identifier) worden gesynchroniseerd. Als u Azure AD Domain Services gebruikt, worden ook verouderde referentie-hashes voor NTLM en Kerberos-verificatie gesynchroniseerd met uw Azure AD-tenant.

Als u terugschrijven configureert, worden de veranderingen in uw Azure AD-directory gesynchroniseerd terug naar uw lokale Active Directory. Bijvoorbeeld, als u uw wachtwoord met Azure AD selfservice voor wachtwoordherstel wijziging functies wijzigen, het gewijzigde wachtwoord is bijgewerkt in uw on-premises AD-domein.

> [!NOTE]
> Gebruik altijd de meest recente versie van Azure AD Connect om te controleren of er oplossingen voor alle bekende fouten.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronisatie van uw Azure AD-tenant naar uw beheerde domein
Accounts van gebruikers, groepslidmaatschappen en referentie-hashes worden gesynchroniseerd vanuit uw Azure AD-tenant aan uw Azure AD Domain Services beheerd domein. Dit synchronisatieproces is automatisch. U hoeft niet te configureren, bewaken of beheren van dit synchronisatieproces. Nadat de eenmalige initiële synchronisatie van uw directory voltooid is, doorgaans duurt het ongeveer 20 minuten om de wijzigingen in Azure AD worden weergegeven in uw beheerde domein. Deze synchronisatie-interval is van toepassing op wachtwoord te wijzigen of wijzigingen in kenmerken die zijn aangebracht in Azure AD.

Het synchronisatieproces is ook een-way/Unidirectioneel aard. Uw beheerde domein is grotendeels alleen-lezen, met uitzondering van eventuele aangepaste OE's die u maakt. Daarom kan niet u wijzigingen aanbrengt in gebruikerskenmerken, wachtwoorden of groepslidmaatschappen binnen het beheerde domein. Als gevolg hiervan is er geen omgekeerde synchronisatie van wijzigingen in uw beheerde domein terug naar uw Azure AD-tenant.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronisatie van een forest met meerdere on-premises-omgeving
Veel organisaties hebben een relatief complex on-premises identity-infrastructuur die bestaan uit meerdere accountforests. Azure AD Connect ondersteunt het synchroniseren van gebruikers, groepen en referentie-hashes van omgevingen met meerdere forests met uw Azure AD-tenant.

Uw Azure AD-tenant is daarentegen een veel eenvoudiger en platte naamruimte. Om gebruikers veilig toegang krijgen tot toepassingen die zijn beveiligd door Azure AD, UPN conflicten tussen gebruikersaccounts in verschillende forests worden opgelost. Uw Azure AD Domain Services beheerd domein bears sluit waardoor de gelijkenis met uw Azure AD-tenant. Daarom ziet u een platte OE-structuur in uw beheerde domein. Alle gebruikers en groepen worden opgeslagen in de container 'AADDC gebruikers', ongeacht het lokale domein of forest waarin ze zijn gesynchroniseerd in. U hebt een hiërarchische OU geconfigureerd lokale structuur. Uw beheerde domein heeft echter nog steeds een eenvoudige platte OE-structuur.

## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Uitsluitingen - wat niet is gesynchroniseerd met uw beheerde domein
De volgende kenmerken of objecten zijn niet gesynchroniseerd met uw Azure AD-tenant of aan het beheerde domein:

* **Kenmerken uitgesloten:** als u wilt uitsluiten van bepaalde kenmerken worden gesynchroniseerd met uw Azure AD-tenant van uw lokale domein via Azure AD Connect. Deze uitgesloten kenmerken zijn niet beschikbaar in uw beheerde domein.
* **Groepsbeleid:** Groepsbeleid zijn geconfigureerd in uw lokale domein zijn niet gesynchroniseerd naar uw beheerde domein.
* **SYSVOL-share:** op dezelfde manier de inhoud van de SYSVOL-share op uw lokale domein niet worden gesynchroniseerd naar uw beheerde domein.
* **Computerobjecten:** computerobjecten voor computers die zijn gekoppeld aan het lokale domein niet zijn gesynchroniseerd met uw beheerde domein. Deze computers niet op een vertrouwensrelatie heeft met uw beheerde domein en deel uitmaken van uw lokaal domein. Zoek in uw beheerde domein computerobjecten alleen voor computers die u hebt expliciet domein aan het beheerde domein.
* **SID-geschiedenis kenmerken voor gebruikers en groepen:** de primaire gebruiker en de primaire groeps-SID's van uw lokale domein worden gesynchroniseerd naar uw beheerde domein. Bestaande SidHistory kenmerken voor gebruikers en groepen worden echter niet gesynchroniseerd vanuit uw lokale domein naar uw beheerde domein.
* **Organisatie-eenheden (OE) structuren:** organisatie-eenheden die zijn gedefinieerd in uw lokale domein niet synchroniseren naar uw beheerde domein. Er zijn twee ingebouwde OE's in uw beheerde domein. Uw beheerde domein heeft standaard een platte OE-structuur. Als u echter wilt [een aangepaste organisatie-eenheid maken in uw beheerde domein](active-directory-ds-admin-guide-create-ou.md).

## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Hoe specifieke kenmerken worden gesynchroniseerd naar uw beheerde domein
De volgende tabel bevat enkele algemene kenmerken en beschrijft hoe ze worden gesynchroniseerd naar uw beheerde domein.

| Kenmerk in uw beheerde domein | Bron | Opmerkingen |
|:--- |:--- |:--- |
| UPN |De UPN-kenmerk van de gebruiker in uw Azure AD-tenant |Het UPN-kenmerk van uw Azure AD-tenant is gesynchroniseerd omdat uw beheerde domein. Daarom is de meest betrouwbare manier aan te melden bij uw beheerde domein met behulp van de UPN. |
| SAMAccountName |Van de gebruiker mailNickname kenmerk in uw Azure AD-tenant of automatisch wordt gegenereerd |Het kenmerk SAMAccountName afkomstig is uit het kenmerk mailNickname in uw Azure AD-tenant. Als meerdere gebruikersaccounts hetzelfde kenmerk voor mailNickname hebben, is de SAMAccountName automatisch wordt gegenereerd. Als de mailNickname of de UPN-voorvoegsel van de gebruiker meer dan 20 tekens, is de SAMAccountName automatisch gegenereerde om te voldoen aan de maximaal 20 tekens op SAMAccountName kenmerken. |
| Wachtwoorden |Het wachtwoord van uw Azure AD-tenant van gebruiker |Referentie-hashes voor NTLM of Kerberos-verificatie (ook wel aanvullende referenties) worden gesynchroniseerd vanuit uw Azure AD-tenant. Als uw Azure AD-tenant een gesynchroniseerde tenant is, worden deze referenties zijn afkomstig van het lokale domein. |
| Primaire gebruiker of groep SID |Automatisch gegenereerde |De primaire SID voor gebruiker of groep accounts is automatisch gegenereerd in uw beheerde domein. Dit kenmerk komt niet overeen met de primaire gebruiker of groep-SID van het object in uw on-premises AD-domein. Dit verschil is dat het beheerde domein een andere SID-naamruimte dan uw lokale domein heeft. |
| SID-geschiedenis voor gebruikers en groepen |On-premises primaire gebruiker en groep-SID |Het kenmerk SidHistory voor gebruikers en groepen in uw beheerde domein is ingesteld op overeenkomen met de bijbehorende primaire gebruiker of groep-SID in uw lokale domein. Deze functie kunt eenvoudiger lift-en-verschuiving van een on-premises toepassingen aan het beheerde domein, omdat u niet re ACL-resources wilt. |

> [!NOTE]
> **Meld u bij het beheerde domein met de UPN-indeling:** de SAMAccountName kenmerk mogelijk automatisch wordt gegenereerd voor sommige gebruikersaccounts in uw beheerde domein. Als meerdere gebruikers het kenmerk met dezelfde mailNickname hebben of gebruikers overmatig lange UPN-voorvoegsels hebben, moet u de SAMAccountName voor deze gebruikers mogelijk automatisch wordt gegenereerd. De SAMAccountName-indeling (bijvoorbeeld ' CONTOSO100\joeuser') is daarom niet altijd een betrouwbare manier aan te melden bij het domein. Gebruikers automatisch gegenereerde SAMAccountName kan afwijken van de UPN-voorvoegsel. Gebruik de UPN-indeling (bijvoorbeeld 'joeuser@contoso100.com') aan te melden bij het beheerde domein betrouwbaar.
>
>

### <a name="attribute-mapping-for-user-accounts"></a>Toewijzing van de kenmerken voor gebruikersaccounts
De volgende tabel ziet u hoe specifieke kenmerken voor gebruiker objecten in uw Azure AD-tenant worden gesynchroniseerd met de bijbehorende kenmerken in uw beheerde domein.

| Gebruikerskenmerk in uw Azure AD-tenant | Gebruikerskenmerk in uw beheerde domein |
|:--- |:--- |
| accountEnabled |userAccountControl (ingesteld of wist u de ACCOUNT_DISABLED bit) |
| city |L |
| Land |CO |
| Afdeling |Afdeling |
| Weergavenaam |Weergavenaam |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| Voornaam |Voornaam |
| Functie |titel |
| E-mail |E-mail |
| mailNickname |msDS-AzureADMailNickname |
| mailNickname |SAMAccountName (soms mogelijk automatisch gegenereerd) |
| mobiele |mobiele |
| object-id |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID-geschiedenis |
| passwordPolicies |userAccountControl (ingesteld of wist u de DONT_EXPIRE_PASSWORD bit) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| Postcode |Postcode |
| preferredLanguage |preferredLanguage |
| state |St |
| StreetAddress |StreetAddress |
| Achternaam |SN |
| telephoneNumber |telephoneNumber |
| userPrincipalName |userPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Toewijzing van de kenmerken voor groepen
De volgende tabel ziet u hoe specifieke kenmerken voor een groep objecten in uw Azure AD-tenant worden gesynchroniseerd met de bijbehorende kenmerken in uw beheerde domein.

| Kenmerk van de groep in uw Azure AD-tenant | Het kenmerk Group in uw beheerde domein |
|:--- |:--- |
| Weergavenaam |Weergavenaam |
| Weergavenaam |SAMAccountName (soms mogelijk automatisch gegenereerd) |
| E-mail |E-mail |
| mailNickname |msDS-AzureADMailNickname |
| object-id |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID-geschiedenis |
| securityenabled moet |groupType |

## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objecten die niet zijn gesynchroniseerd met uw Azure AD-tenant van uw beheerde domein
Als in een voorgaande sectie van dit artikel wordt beschreven, vindt er geen synchronisatie van uw beheerde domein terug naar uw Azure AD-tenant. U kunt [maken van een aangepaste organisatie-eenheid (OE)](active-directory-ds-admin-guide-create-ou.md) in uw beheerde domein. Bovendien kunt u andere organisatie-eenheden, gebruikers, groepen of serviceaccounts binnen deze aangepaste organisatie-eenheden. Geen van de objecten die zijn gemaakt in aangepaste OE's gesynchroniseerd terug naar uw Azure AD-tenant. Deze objecten zijn beschikbaar voor gebruik binnen uw beheerde domein. Deze objecten zijn daarom niet zichtbaar met behulp van Azure AD PowerShell-cmdlets, Azure AD Graph API of via de gebruikersinterface voor het beheer van de Azure AD.

## <a name="related-content"></a>Gerelateerde inhoud
* [Onderdelen - Azure AD Domain Services](active-directory-ds-features.md)
* [Implementatiescenario - Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Overwegingen voor Azure AD Domain Services netwerken](active-directory-ds-networking.md)
* [Aan de slag met Azure AD Domain Services](active-directory-ds-getting-started.md)
