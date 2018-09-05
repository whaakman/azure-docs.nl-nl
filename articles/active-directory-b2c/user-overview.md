---
title: Overzicht van de gebruiker-accounts in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over gebruikersaccounts in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2354bf02b47c9fbc74dbc0dab07e30ca321279cf
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669927"
---
# <a name="overview-of-user-accounts-in-azure-active-directory-b2c"></a>Overzicht van gebruikersaccounts in Azure Active Directory B2C

In Azure Active Directory (Azure AD) B2C, kunt u verschillende soorten accounts. Azure Active Directory, Azure Active Directory B2B en Azure Active Directory B2C delen in de typen gebruikersaccounts die kunnen worden gebruikt.

De volgende typen accounts zijn beschikbaar:

- **Werkaccount** : een work-account toegang heeft tot resources in een tenant, en met een administrator-rol, tenants kunt beheren.
- **Gast-account** -een Gast-account kan alleen bestaan uit een Microsoft-account of een Azure Active Directory-gebruiker die kan worden gebruikt voor toegang tot toepassingen of tenants beheren. 
- **Klantaccount** -een klantaccount is gemaakt door zich via een registratiebeleid in een Azure AD B2C-toepassing of met behulp van Azure AD Graph API, en wordt gebruikt door gebruikers van de toepassingen die zijn geregistreerd bij Azure AD B2C. 

## <a name="work-account"></a>Werkaccount

Een werkaccount, wordt dezelfde manier gemaakt voor alle tenants op basis van Azure AD. Voor het maken van een werkaccount, kunt u de informatie in [Quick Start: nieuwe gebruikers toevoegen aan Azure Active Directory](../active-directory/fundamentals/add-users-azure-active-directory.md). Een werkaccount wordt gemaakt met behulp van de **nieuwe gebruiker** keuze in Azure portal.

Wanneer u een nieuwe werkaccount toevoegt, moet u rekening houden met de volgende configuratie-instellingen:

- **Naam** en **gebruikersnaam** : de **naam** eigenschap bevat de gegeven en achternaam van de gebruiker. De **gebruikersnaam** is de id die de gebruiker moet invoeren om aan te melden. Naam van de gebruiker bevat de volledige domein. Het domeingedeelte van de gebruikersnaam moet lid zijn van de aanvankelijke standaarddomeinnaam *uwdomein.onmicrosoft.com*, of een geverifieerde, niet-gefedereerde [aangepast domein](../active-directory/fundamentals/add-custom-domain.md) naam zoals  *Contoso.com*.
- **Profiel** -het account is ingesteld met een profiel van gebruikersgegevens. U hebt de mogelijkheid om in te voeren een voornaam, achternaam, functie en de naam van afdeling. Nadat het account is gemaakt, kunt u het profiel bewerken.
- **Groepen** -een groep gebruiken voor het uitvoeren van beheertaken zoals het toewijzen van licenties of machtigingen voor een aantal gebruikers of apparaten in één keer. U kunt het nieuwe account in een bestaande plaatsen [groep](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) in uw tenant. 
- **Maprol** -moet u het niveau van toegang dat het gebruikersaccount tot resources in uw tenant heeft opgeven. De volgende machtigingsniveaus zijn beschikbaar:

    - **Gebruiker** -gebruikers kunnen toegang tot toegewezen resources, maar de meeste tenantbronnen niet beheren.
    - **Globale beheerder** -globale beheerders hebben volledige controle over alle tenantresources.
    - **Beperkte beheerder** -selecteert u de administratieve rollen of functies voor de gebruiker. Zie voor meer informatie over de functies die kunnen worden geselecteerd, [beheerdersrollen toewijzen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md). 

### <a name="create-a-work-account"></a>Een werkaccount maken

Gebruik de volgende informatie kunt u een nieuw werkaccount maken:

- [Azure Portal](../active-directory/fundamentals/add-users-azure-active-directory.md)
- [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_post_users)

### <a name="update-a-user-profile"></a>Een gebruikersprofiel bijwerken

Gebruik de volgende informatie kunt u het profiel van een gebruiker bijwerken:

- [Azure Portal](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
- [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_update)

### <a name="reset-a-password-for-a-user"></a>Een wachtwoord voor een gebruiker opnieuw instellen

Gebruik de volgende informatie kunt u het wachtwoord van een gebruiker opnieuw instellen: 

- [Azure Portal](../active-directory/fundamentals/active-directory-users-reset-password-azure-portal.md)
- [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_update)

## <a name="guest-user"></a>Gastgebruiker

Als een gastgebruiker, kunt u externe gebruikers uitnodigen voor uw tenant. Een typisch scenario voor het uitnodigen van een gastgebruiker aan uw Azure AD B2C-tenant is voor het delen van de verantwoordelijkheden van beheer. Zie voor een voorbeeld van het gebruik van een gastaccount [eigenschappen van de gebruiker van een Azure Active Directory B2B-samenwerking](../active-directory/b2b/user-properties.md).

Wanneer u een gastgebruiker voor uw tenant uitnodigen, kunt u het e-mailadres van de ontvanger en een bericht met een beschrijving van de uitnodiging opgeven. De voor de uitnodigingskoppeling brengt de gebruiker naar de pagina toestemming waar de **aan de slag** knop wordt geselecteerd en de controle van machtigingen is geaccepteerd. Als een postvak in is niet gekoppeld aan het e-mailadres, betekent dit dat de gebruiker zich door te gaan naar een Microsoft-pagina met behulp van de uitgenodigde referenties kunt navigeren naar de pagina toestemming. De gebruiker wordt vervolgens gedwongen om in te wisselen de uitnodiging dezelfde manier als op de koppeling in het e-mailbericht te klikken. Bijvoorbeeld: `https://myapps.microsoft.com/B2CTENANTNAME`.

U kunt ook de [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/api/invitation_post) uitnodigen een gastgebruiker.

## <a name="consumer-user"></a>Consumenten-gebruiker

De gebruiker consumenten kan zich aanmelden bij toepassingen die zijn beveiligd door Azure AD B2C, maar geen toegang tot Azure-resources, zoals de Azure-portal.  De consument-gebruiker kunt gebruiken voor een lokaal account of federatieve accounts, zoals Facebook of Twitter. Een consumentenaccount is gemaakt met behulp van een [beleid voor registreren of aanmelden](../active-directory-b2c/active-directory-b2c-reference-policies.md).

U kunt de gegevens die worden verzameld als een consument-gebruikersaccount is gemaakt met behulp van aangepaste kenmerken opgeven. Zie voor meer informatie, [aangepaste kenmerken definiëren in Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-reference-custom-attr.md).

Kunt u de informatie in de **maken van gebruikersaccounts van consumenten** sectie van [gebruiken de Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) om een Azure AD B2C-consument-account te maken. U kunt ook de informatie in de **bijwerken van gebruikersaccounts van consumenten** sectie in hetzelfde artikel voor het beheren van de eigenschappen van het account.

### <a name="migrate-consumer-user-accounts"></a>Migreren van gebruikersaccounts van consumenten

Mogelijk hebt u behoefte aan het migreren van bestaande gebruikersaccounts van consumenten van een id-provider naar Azure AD B2C. Zie voor meer informatie, [gebruikersmigratie](active-directory-b2c-user-migration.md) of [gebruikers met sociale identiteiten migreren](active-directory-b2c-social-migration.md).