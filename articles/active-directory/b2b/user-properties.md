---
title: Eigenschappen van de gebruiker van een Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking gebruikerseigenschappen kunnen worden geconfigureerd
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5f999a17cd375a3338aa936e2f405c36f6021ebc
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984806"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Eigenschappen van de gebruiker van een Azure Active Directory B2B-samenwerking

Een Azure Active Directory (Azure AD) business-to-business (B2B) samenwerking is een gebruiker met UserType = Gast. Deze gastgebruiker meestal afkomstig is van een partnerorganisatie en beperkte bevoegdheden in de map van de uitnodigende, heeft standaard.

Afhankelijk van de behoeften van de uitnodigende organisatie, kan de gebruiker van een Azure AD B2B-samenwerking in een van de volgende account statussen zijn:

- Status 1: Homed in een extern exemplaar van Azure AD en weergegeven als een gastgebruiker in de organisatie. In dit geval zich de B2B-gebruiker aanmeldt met een Azure AD-account die deel uitmaakt van de uitgenodigde tenant. Als de andere organisatie geen Azure AD gebruikt, wordt de gastgebruiker in Azure AD nog steeds gemaakt. De vereisten zijn dat ze hun uitnodiging inwisselen en Azure AD verifieert u het e-mailadres. Deze benadering wordt ook een just-in-time (JIT) tenants of een 'viraal' tenants genoemd.

- Status van 2: Homed in een Microsoft-account en wordt weergegeven als een gastgebruiker in de organisatie van de host. In dit geval de gastgebruiker zich aanmeldt met een Microsoft-account. Sociale identiteit van de uitgenodigde gebruiker (google.com of vergelijkbaar), die niet een Microsoft-account is gemaakt als een Microsoft-account tijdens de inschrijving van de aanbieding.

- Status van 3: Homed in van de organisatie van de host on-premises Active Directory en gesynchroniseerd met de host-organisatie in Azure AD. Tijdens deze release, moet u PowerShell gebruiken voor het handmatig wijzigen het UserType van deze gebruikers in de cloud.

- Status van 4: Homed in host organisatie in Azure AD met UserType = Gast en referenties die de organisatie van de host wordt beheerd.

  ![Initialen van de afzender van de uitnodiging weergeven](media/user-properties/redemption-diagram.png)


Nu gaan we kijken hoe een gebruiker Azure AD B2B-samenwerking in staat 1 eruit in Azure AD.

### <a name="before-invitation-redemption"></a>Voordat u verzilvering van uitnodiging

![Voordat u inschrijving van de aanbieding](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Na verzilvering van uitnodiging

![Na inwisseling van de aanbieding](media/user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Eigenschappen van de sleutel van de gebruiker van Azure AD B2B-samenwerking
### <a name="usertype"></a>UserType
Deze eigenschap geeft aan dat de relatie van de gebruiker op de host-tenants. Deze eigenschap kan twee waarden hebben:
- Lid: Deze waarde geeft aan dat een werknemer van de organisatie van de host en een gebruiker in de organisatie salarisadministratie. Deze gebruiker wordt bijvoorbeeld verwacht kunnen toegang hebben tot interne websites. Deze gebruiker kan niet worden gezien als een externe samenwerker.

- Gast: Deze waarde geeft aan dat een gebruiker die wordt niet beschouwd als intern gebruik binnen het bedrijf, zoals een externe samenwerker, partner, klant of dezelfde gebruiker. Een gebruiker wouldn't ontvangen van een CEO interne memo of ontvangen van de voordelen van het bedrijf, bijvoorbeeld worden verwacht.

  > [!NOTE]
  > Het UserType is niet gekoppeld aan hoe de gebruiker zich aanmeldt, de directory-rol van de gebruiker, enzovoort. Deze eigenschap geeft aan dat de relatie van de gebruiker voor de organisatie van de host gewoon en kan de organisatie om af te dwingen van beleidsregels die afhankelijk van deze eigenschap zijn.

### <a name="source"></a>Bron
Deze eigenschap geeft aan hoe de gebruiker zich aanmeldt.

- Uitgenodigd gebruiker: Deze gebruiker is uitgenodigd maar een uitnodiging nog niet is ingewisseld.

- Externe Active Directory: Deze gebruiker is toegewezen in een externe organisatie en verifieert met behulp van een Azure AD-account die deel uitmaakt van de andere organisatie. Dit type aanmelding komt overeen met de status van 1.

- Microsoft-account: deze gebruiker is toegewezen in een Microsoft-account en verifieert met behulp van een Microsoft-account. Dit type aanmelding komt overeen met de status 2.

- Windows Server Active Directory: Deze gebruiker is aangemeld vanuit on-premises Active Directory die deel uitmaakt van deze organisatie. Dit type aanmelding komt overeen met de status 3.

- Azure Active Directory: Deze gebruiker wordt geverifieerd met behulp van een Azure AD-account die deel uitmaakt van deze organisatie. Dit type aanmelding komt overeen met status 4.
  > [!NOTE]
  > Bron- en UserType zijn onafhankelijke eigenschappen. Een waarde van de bron betekent niet dat voor een bepaalde waarde voor UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Kunnen Azure AD B2B-gebruikers worden toegevoegd als leden in plaats van gasten?
Een Azure AD B2B-gebruiker en de gastgebruiker zijn meestal gelijk. Daarom wordt de gebruiker van een Azure AD B2B-samenwerking toegevoegd als een gebruiker met UserType = Gast standaard. In sommige gevallen is de partnerorganisatie echter lid is van een grotere organisatie waarvan de organisatie van de host ook deel uitmaakt. Als dit het geval is, kan de organisatie van de host wilt voor het behandelen van gebruikers in de partnerorganisatie als leden in plaats van gasten. Gebruik de Azure AD B2B uitnodiging Manager API's toe te voegen of een gebruiker van de partnerorganisatie aan de organisatie van de host als een lid uitnodigen.

## <a name="filter-for-guest-users-in-the-directory"></a>Filter voor gastgebruikers ook kunnen in de map

![Filteren van gastgebruikers](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>UserType converteren
Op dit moment is het mogelijk dat gebruikers converteren naar UserType lid Gast en vice versa met behulp van PowerShell. De eigenschap UserType is echter mag om weer te geven van de gebruiker-relatie voor de organisatie. De waarde van deze eigenschap moet daarom wijzigen alleen als de relatie van de gebruiker voor de organisatie wordt gewijzigd. Als de relatie van de gebruiker wordt gewijzigd, moeten problemen, zoals of de UPN (User Principal Name) wijzigen wilt, worden opgelost? Moet de gebruiker nog steeds toegang tot dezelfde resources? Een postvak worden toegewezen? Daarom niet aangeraden de UserType wijzigen met behulp van PowerShell als een atomische activiteit. Bovendien, als deze eigenschap onveranderbare wordt met behulp van PowerShell, raden we niet maken van een afhankelijkheid op deze waarde.

## <a name="remove-guest-user-limitations"></a>Beperkingen van Gast-gebruiker verwijderen
Mogelijk zijn er gevallen waar u wilt uw gastgebruikers hogere rechten te verlenen. U kunt een gastgebruiker toevoegen aan een rol en zelfs de standaardbeperkingen voor de Gast-gebruiker verwijderen in de map waarin een gebruiker dezelfde bevoegdheden als leden.

Het is mogelijk de standaardbeperkingen voor de gebruiker van Gast uitschakelen zodat een gastgebruiker in de directory van het bedrijf dezelfde machtigingen als een gebruiker lid is opgegeven.

![Beperkingen van Gast-gebruiker verwijderen](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Kan ik gastgebruikers zichtbaar maken in de algemene Exchange-adreslijst?
Ja. Standaard Gast objecten zijn niet zichtbaar in de globale adreslijst van uw organisatie, maar u kunt Azure Active Directory PowerShell gebruiken om ze zichtbaar. Zie voor meer informatie, **kan ik objecten Gast zichtbaar maken in de globale adreslijst?** in [toegang voor gasten in Office 365-groepen](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ). 

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [B2B-samenwerking gebruikerstokens](user-token.md)
* [B2B-samenwerking-gebruikersclaims toewijzen](claims-mapping.md)
