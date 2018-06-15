---
title: Wat is Azure Active Directory B2B-samenwerking? | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te krijgen tot uw zakelijke toepassingen.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 1eff04cf43260a12a50a08b9145e1975b51af0cc
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33928477"
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Wat is Azure AD B2B-samenwerking? (Engelstalig artikel)

Azure Active Directory (Azure AD) business-to-business (B2B)-samenwerkingsmogelijkheden inschakelen voor elke organisatie gebruikmaken van Azure AD veilig werken met gebruikers vanaf elke organisatie klein of groot. Deze organisaties kunnen worden met of zonder Azure AD en zelfs geen nodig om een IT-afdeling.

Organisaties die gebruikmaken van Azure AD kunnen toegang bieden tot documenten, bronnen en toepassingen naar hun partners terwijl volledige controle over hun eigen zakelijke gegevens behouden. Ontwikkelaars kunnen gebruikmaken van de Azure AD business-to-business API's voor het schrijven van toepassingen die twee organisaties elkaar in meer veilig brengt. Daarnaast is het eenvoudig voor eindgebruikers te navigeren.

De volgende video biedt een handig overzicht.
>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="key-benefits-of-azure-ad-b2b-collaboration"></a>Belangrijkste voordelen van Azure AD B2B-samenwerking

### <a name="work-with-any-user-from-any-partner"></a>Werken met alle gebruikers van een partner

- Partners hun eigen referenties gebruiken
- Er is geen vereiste voor het gebruik van Azure AD-partners
- Er zijn geen externe mappen of ingewikkelde installatie vereist

### <a name="simple-and-secure-collaboration"></a>Eenvoudig en veilig samenwerking

- Toegang tot alle zakelijke Apps en gegevens, bieden tijdens het toepassen van geavanceerde, Azure AD-aangedreven autorisatiebeleid
- Gemakkelijker voor gebruikers
- Beveiliging van bedrijfsniveau voor apps en gegevens

### <a name="no-management-overhead"></a>Er is geen beheeroverhead

- Er zijn geen externe account of wachtwoord management
- Er is geen synchronisatie of handmatig levenscyclusbeheer van account
- Er zijn geen externe beheertaken

## <a name="easily-add-b2b-collaboration-users"></a>B2B-samenwerking gebruikers eenvoudig toevoegen

Als beheerder, kunt u eenvoudig B2B-samenwerking (Gast) gebruikers toevoegen aan uw organisatie in de [Azure-portal](https://portal.azure.com).

![gastgebruikers toevoegen](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Uw deelnemers om hun eigen identiteit inschakelen

B2B-deelnemers kunnen zich aanmelden met een identiteit van hun keuze. Als de gebruiker heeft geen Microsoft-account of een Azure AD-account – een is gemaakt voor deze naadloos op het moment van aanbieding inwisseling.

### <a name="delegate-to-application-and-group-owners"></a>Delegeren aan de toepassing en eigenaars Groepsbeleid

Als een toepassing of de eigenaar van de groep, kunt u B2B gebruikers rechtstreeks aan een toepassing die u belangrijk vindt, toevoegen, of u nu een Microsoft-toepassing of niet. Beheerders kunnen machtiging B2B-gebruikers toevoegen aan niet-beheerders kunnen delegeren. Niet-beheerders kunnen gebruiken de [Azure AD-Toegangsvenster voor toepassing](https://myapps.microsoft.com) B2B-samenwerking gebruikers toevoegen aan de toepassingen of -groepen.

![toegangspaneel](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![lid toevoegen](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Autorisatiebeleid beveiligen uw zakelijke inhoud

Beleid voor voorwaardelijke toegang, zoals meervoudige verificatie kunnen worden afgedwongen:
- Op het niveau van de tenant
- Op het toepassingsniveau van de
- Voor specifieke gebruikers zakelijke apps en gegevens te beschermen

### <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>API's gebruiken en voorbeeldcode eenvoudig om toepassingen te bouwen om vrij te geven

Breng uw externe partners boord op manieren aangepast aan de behoeften van uw organisatie.

Gebruik de [B2B-samenwerking uitnodiging API's](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) voor het aanpassen van uw voorbereiding optreedt, zoals het maken van registratie selfserviceportals. We bieden voorbeeldcode voor een self-service portal [op Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![registratie-portal](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

U kunt de kracht van Azure AD voor het beveiligen van uw partnerrelaties op een manier die eindgebruikers kunnen gemakkelijk en intuïtief zoeken ophalen met de Azure AD B2B-samenwerking.

## <a name="next-steps"></a>Volgende stappen

- [Hoe voeg beheerders van Azure Active Directory B2B-samenwerking gebruikers?](active-directory-b2b-admin-add-users.md)
- [Hoe kunnen IT-medewerkers B2B-samenwerking gebruikers toevoegen](active-directory-b2b-iw-add-users.md)
- [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md)
- [Azure AD B2B-samenwerking licentieverlening](active-directory-b2b-licensing.md)
- En als altijd verbinding maken met het productteam feedback, discussies en suggesties via onze [Microsoft Tech-Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).