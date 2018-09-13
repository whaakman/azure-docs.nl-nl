---
title: Wat is Azure Active Directory B2B-samenwerking? | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te krijgen tot uw bedrijfstoepassingen.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 51a969ae215583a0be8d75ff1de11173e0696a22
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644388"
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Wat is Azure AD B2B-samenwerking? (Engelstalig artikel)

Azure Active Directory (Azure AD) business-to-business (B2B) functionaliteit voor samenwerking in inschakelen elke organisatie met behulp van Azure AD veilig werken met gebruikers van een andere organisatie, kleine of grote. Organisaties kunnen worden met of zonder Azure AD en hoeft niet eens te hebben van een IT-afdeling.

Organisaties met behulp van Azure AD krijgt u toegang tot documenten, bronnen en toepassingen naar hun partners, behoud van volledige controle over hun eigen zakelijke gegevens. Ontwikkelaars kunnen gebruikmaken van de Azure AD business-to-business API's voor het schrijven van toepassingen die samenbrengen die twee organisaties in meer veilig. Bovendien is het eenvoudig voor eindgebruikers om te navigeren.

De volgende video biedt een handig overzicht.
>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="key-benefits-of-azure-ad-b2b-collaboration"></a>Belangrijkste voordelen van Azure AD B2B-samenwerking

### <a name="work-with-any-user-from-any-partner"></a>Werken met alle gebruikers van een partner

- Partners gebruiken hun eigen referenties
- Er is geen vereiste voor partners Azure AD gebruiken
- Er zijn geen externe mappen of ingewikkelde installatie vereist

### <a name="simple-and-secure-collaboration"></a>Eenvoudige en veilige samenwerking

- Toegang bieden tot alle zakelijke Apps en gegevens, terwijl geavanceerde, Azure AD aangestuurde autorisatiebeleid toepassen
- Gemakkelijk voor gebruikers
- Geavanceerde beveiliging van apps en gegevens

### <a name="no-management-overhead"></a>Er is geen beheeroverhead

- Er zijn geen externe beheer van account of wachtwoord
- Er is geen synchronisatie of handmatige levenscyclusbeheer van account
- Er zijn geen externe administratieve overhead

## <a name="easily-add-b2b-collaboration-users"></a>Eenvoudig toevoegen van gebruikers van B2B-samenwerking

Als beheerder, kunt u eenvoudig gebruikers van B2B-samenwerking (Gast) toevoegen aan uw organisatie in de [Azure-portal](https://portal.azure.com).

![gastgebruikers toevoegen](media/what-is-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Uw medewerkers om de eigen identiteit inschakelen

B2B-deelnemers kunnen zich aanmelden met een identiteit van hun keuze. Als de gebruiker beschikt niet over een Microsoft-account of een Azure AD-account: een voor hem naadloos op het moment van inwisseling van de aanbieding gemaakt.

### <a name="delegate-to-application-and-group-owners"></a>Delegeren aan de toepassing en eigenaren van groepen

Als een toepassing of de eigenaar van de groep, kunt u B2B-gebruikers rechtstreeks aan een toepassing die u interessante toevoegen of het is een Microsoft-toepassing of niet. Beheerders kunnen delegeren machtiging B2B-gebruikers toevoegen aan niet-beheerders. Niet-beheerders kunnen gebruiken de [deelvenster van de Azure AD-toepassing](https://myapps.microsoft.com) gebruikers van B2B-samenwerking toevoegen aan toepassingen of -groepen.

![Toegangsvenster](media/what-is-b2b/access-panel.png)

![lid toevoegen](media/what-is-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Autorisatiebeleid beveiligen van uw zakelijke inhoud

Beleid voor voorwaardelijke toegang zoals multi-factor authentication kunnen worden afgedwongen:
- Op het tenantniveau van de
- Op het toepassingsniveau van de
- Voor specifieke gebruikers om zakelijke apps en gegevens te beveiligen

### <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Gebruik API's en voorbeeldcode eenvoudig toepassingen voor onboarding

Breng uw externe partners aan boord op manieren die zijn aangepast aan de behoeften van uw organisatie.

Gebruik de [B2B-samenwerking uitnodiging API's](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) voor het aanpassen van uw onboarding-ervaringen, met inbegrip van het maken van aanmelding selfserviceportals. We bieden voorbeeldcode voor een selfservice portal [op Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![Meld u aan portal](media/what-is-b2b/sign-up-portal.png)

Met Azure AD B2B-samenwerking krijgt u de volledige kracht van Azure AD om te beveiligen van uw partnerrelaties op een manier die eindgebruikers makkelijk en intuïtief vinden.

## <a name="next-steps"></a>Volgende stappen

- [Hoe voeg beheerders van Azure Active Directory gebruikers van B2B-samenwerking toe?](add-users-administrator.md)
- [Hoe voeg informatiemedewerkers gebruikers van B2B-samenwerking toe?](add-users-information-worker.md)
- [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md)
- [Licentieverlening voor Azure AD B2B collaboration](licensing-guidance.md)
- En zoals altijd verbinding maken met het productteam feedback, discussies en suggesties via onze [Microsoft Tech-Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).