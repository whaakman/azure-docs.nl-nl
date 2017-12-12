---
title: Eigenschappen van een gebruiker met Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking gebruikerseigenschappen kunnen worden geconfigureerd
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: f7cf0451147adf539fd11753c41b1f08c442e527
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Eigenschappen van een gebruiker met Azure Active Directory B2B-samenwerking

Een gebruiker met samenwerking business-to-business (B2B) voor Azure Active Directory (Azure AD) is een gebruiker met UserType = Gast. Deze gastgebruiker meestal afkomstig is van een partnerorganisatie en beperkte rechten in de map uitnodigen en standaard.

Afhankelijk van de organisatie uitnodigen behoeften, kan een gebruiker met Azure AD B2B-samenwerking zijn in een van de volgende statussen voor account:

- Status 1: Homed in een extern exemplaar van Azure AD en weergegeven als gastgebruiker in de organisatie uitnodigen. In dit geval zich de B2B-gebruiker aanmeldt met behulp van een Azure AD-account dat is gekoppeld aan de uitgenodigde tenant. Als de andere organisatie geen Azure AD gebruikt, wordt de gastgebruiker in Azure AD nog steeds gemaakt. De vereisten zijn dat ze gebruikmaken van hun uitnodiging en Azure AD het e-mailadres verifieert. Deze benadering wordt ook wel een just-in-time-tenancymodus (Just in time) of 'een'-tenancymodus.

- Status van 2: Homed in een Microsoft-account en weergegeven als een gastgebruiker in de organisatie van de host. In dit geval de gastgebruiker meldt zich aan met een Microsoft-account. Sociale identiteit van de uitgenodigde gebruiker (google.com of vergelijkbaar), die niet is een Microsoft-account is gemaakt als een Microsoft-account tijdens inwisseling aanbieding.

- Status van 3: Homed in van de organisatie van de host op lokale Active Directory en gesynchroniseerd met de host-organisatie Azure AD. Bij deze release moet u PowerShell gebruiken het UserType van deze gebruikers in de cloud handmatig wijzigen.

- Status 4: Homed in van de organisatie van de host Azure AD met UserType = Gast en de referenties die de organisatie van de host beheert.

  ![Initialen van de afzender van de uitnodiging weergeven](media/active-directory-b2b-user-properties/redemption-diagram.png)


Nu gaan we kijken hoe een gebruiker met de Azure AD B2B-samenwerking in status 1 eruit in Azure AD.

### <a name="before-invitation-redemption"></a>Voordat u een uitnodiging voor inwisseling

![Voordat de aanbieding inwisseling](media/active-directory-b2b-user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Na een uitnodiging voor inwisseling

![Na de aanbieding inwisseling](media/active-directory-b2b-user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>De sleuteleigenschappen van de gebruiker Azure AD B2B-samenwerking
### <a name="usertype"></a>UserType
Deze eigenschap geeft de relatie van de gebruiker aan de host-tenancymodus. Deze eigenschap kan twee waarden hebben:
- Lid: Deze waarde geeft een medewerker van de organisatie van de host en een gebruiker in de organisatie salarissen. Bijvoorbeeld, wordt deze gebruiker toegang hebben tot interne sites verwacht. Deze gebruiker kan niet worden gezien als een externe medewerker.

- Gast: Deze waarde geeft aan dat een gebruiker die wordt niet beschouwd als intern gebruik binnen het bedrijf, zoals een externe medewerker, partner, klant of dezelfde gebruiker. Een gebruiker wouldn't ontvangen van een Directeur interne factuur of ontvangen van de voordelen van het bedrijf, bijvoorbeeld worden verwacht.

  > [!NOTE]
  > Het UserType heeft geen relatie op hoe de gebruiker zich aanmeldt, de functie van de directory van de gebruiker, enzovoort. Deze eigenschap geeft de relatie van de gebruiker naar de organisatie host gewoon en kan de organisatie beleid afdwingen dat afhankelijk van deze eigenschap zijn.

### <a name="source"></a>Bron
Deze eigenschap geeft aan hoe de gebruiker zich aanmeldt.

- Gebruiker uitgenodigd: Deze gebruiker heeft uitgenodigd maar een uitnodiging nog niet is gebruikt.

- Externe Active Directory: Deze gebruiker in een externe organisatie is toegewezen en verifieert met behulp van een Azure AD-account bij de andere organisatie hoort. Status 1 overeenkomt met dit type aanmelden.

- Microsoft-account: deze gebruiker in een Microsoft-account is homed en verifieert met behulp van een Microsoft-account. Dit type aanmelden overeenkomt met status 2.

- Windows Server Active Directory: Deze gebruiker is aangemeld vanaf de lokale Active Directory, die deel uitmaakt van deze organisatie. Dit type aanmelden overeenkomt met status 3.

- Azure Active Directory: Deze gebruiker wordt geverifieerd met behulp van een Azure AD-account die deel uitmaakt van deze organisatie. Dit type aanmelden overeenkomt met status 4.
  > [!NOTE]
  > Bron- en UserType zijn onafhankelijk eigenschappen. Een waarde van de bron betekent niet dat voor een bepaalde waarde voor UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Kunnen Azure AD B2B-gebruikers worden toegevoegd als leden in plaats van gasten?
Een Azure AD B2B-gebruiker en de gastgebruiker zijn meestal synoniem. Daarom wordt de gebruiker van een Azure AD B2B-samenwerking toegevoegd als een gebruiker met UserType Gast = standaard. In sommige gevallen is de partnerorganisatie echter lid is van een grotere organisatie waarvan de organisatie van de host ook deel uitmaakt. Zo ja, de host organisatie mogelijk wil gebruikers in de partnerorganisatie als leden in plaats van gasten worden behandeld. Gebruik de Azure AD B2B-uitnodiging Manager API's toevoegen of een gebruiker van de partnerorganisatie naar de organisatie van de host als een lid uit te nodigen.

## <a name="filter-for-guest-users-in-the-directory"></a>Filter voor gastgebruikers in de map

![Gastgebruikers filteren](media/active-directory-b2b-user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>UserType converteren
Op dit moment is het mogelijk gebruikers UserType van lid converteren naar gast en vice versa met behulp van PowerShell. De eigenschap UserType moet echter relatie van de gebruiker voor de organisatie te vertegenwoordigen. Daarom moet de waarde van deze eigenschap alleen wijzigen als de relatie van de gebruiker naar de organisatie wordt gewijzigd. Als de relatie van de gebruiker wordt gewijzigd, moeten problemen, bijvoorbeeld of de UPN (user Principal name) wijzigen moet, worden opgelost? Moet de gebruiker toegang hebben tot dezelfde bronnen doorgaan? Een postvak toegewezen? Daarom niet aangeraden de UserType wijzigen met behulp van PowerShell als een atomic-activiteit. Bovendien, als deze eigenschap niet-wijzigbaar wordt met behulp van PowerShell, raadzaam niet duurt een afhankelijkheid op deze waarde.

## <a name="remove-guest-user-limitations"></a>Beperkingen van Gast-gebruiker verwijderen
Mogelijk zijn er gevallen waarbij u wilt geven uw gastgebruikers hogere rechten. U kunt een gastgebruiker toevoegen aan een rol en zelfs de standaardbeperkingen van de Gast-gebruiker in de map zodat een gebruiker dezelfde bevoegdheden als leden te verwijderen.

Het is mogelijk de standaardbeperkingen van de Gast-gebruiker uitschakelen zodat de gastgebruiker in de directory van het bedrijf krijgt dezelfde machtigingen als een gebruiker lid is.

![Beperkingen van Gast-gebruiker verwijderen](media/active-directory-b2b-user-properties/remove-guest-limitations.png)

## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
* [B2B-samenwerking uitnodigingen delegeren](active-directory-b2b-delegate-invitations.md)
* [B2B-samenwerking gebruiker controle en rapportage](active-directory-b2b-auditing-and-reporting.md)
* [Dynamische groepen en B2B-samenwerking](active-directory-b2b-dynamic-groups.md)
* [B2B-samenwerking code en PowerShell-voorbeelden](active-directory-b2b-code-samples.md)
* [SaaS-apps voor B2B-samenwerking configureren](active-directory-b2b-configure-saas-apps.md)
* [B2B-samenwerking gebruikerstokens](active-directory-b2b-user-token.md)
* [Gebruikersclaims voor B2B-samenwerking toewijzing](active-directory-b2b-claims-mapping.md)
* [Office 365 extern delen](active-directory-b2b-o365-external-user.md)
* [Huidige beperkingen voor B2B-samenwerking](active-directory-b2b-current-limitations.md)
