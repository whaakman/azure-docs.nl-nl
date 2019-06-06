---
title: Toepassingen beheren met Azure Active Directory | Microsoft Docs
description: Dit artikel beschrijft de voordelen van Azure Active Directory integreren met uw on-premises, cloud en SaaS-toepassingen.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 06/05/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 80680de6602b26488a8bade8a69fdd85b7f83ed1
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729951"
---
# <a name="application-management-with-azure-active-directory"></a>Toepassingsbeheer met Azure Active Directory

Azure Active Directory (Azure AD) vereenvoudigt de manier waarop u uw toepassingen beheren met het bieden van een systeem met één identiteit voor uw cloud en on-premises apps. U kunt uw software toevoegen als een service (SaaS)-toepassingen, on-premises toepassingen en line-of-business (LOB)-apps naar Azure AD. Vervolgens zich gebruikers één keer veilig en naadloos toegang krijgen tot deze toepassingen, samen met Office 365 en andere zakelijke toepassingen van Microsoft. U kunt de administratieve kosten verlagen door het automatiseren van gebruikersinrichting. U kunt ook multi-factor authentication en beleid voor voorwaardelijke toegang gebruiken voor toegang tot beveiligde toepassingen.

![Apps die via Azure AD zijn gefedereerd](media/what-is-application-management/app-management-overview.png)

## <a name="why-manage-applications-with-a-cloud-solution"></a>Waarom toepassingen beheren met een cloud-oplossing?

Organisaties hebben vaak honderden toepassingen waar gebruikers afhankelijk van zijn voor hun werk. Gebruikers hebben toegang tot deze toepassingen van groot aantal apparaten en -locaties. Elke dag worden nieuwe toepassingen toegevoegd, ontwikkeld en buiten gebruik gesteld. Met vele toepassingen en -toegangspunten is het belangrijker dan ooit aan een cloud-gebaseerde oplossing gebruiken voor het beheren van toegang tot alle toepassingen.

## <a name="what-types-of-applications-can-i-integrate-with-azure-ad"></a>Welke typen toepassingen kan ik integreren met Azure AD?
Er zijn vier soorten toepassingen die u kunt toevoegen aan uw **bedrijfstoepassingen** en beheren met Azure AD:

-   **Azure AD-galerietoepassingen** – Azure AD heeft een galerie met duizenden toepassingen die vooraf geïntegreerde voor eenmalige aanmelding met Azure AD zijn. Sommige toepassingen die worden gebruikt in uw organisatie, bevinden zich waarschijnlijk in de galerie. [Meer informatie over het plannen van uw app-integratie](plan-an-application-integration.md), of op te halen van van gedetailleerde integratiestappen voor afzonderlijke apps in de [zelfstudies voor SaaS-toepassingen](https://docs.microsoft.com/azure/active-directory/saas-apps/). 

-   **On-premises toepassingen met Application Proxy** – met Azure AD-toepassingsproxy, kunt u uw on-premises web-apps integreren met Azure AD voor de ondersteuning van eenmalige aanmelding. Vervolgens eindgebruikers hebben toegang tot uw on-premises web-apps op dezelfde manier als die ze toegang krijgen Office 365 en andere SaaS-apps tot. [Meer informatie over waarom Application Proxy gebruiken en hoe het werkt](what-is-application-proxy.md).

-   **Aangepaste toepassingen** : bij het bouwen van uw eigen line-of-business-toepassingen, kunt u deze integreren met Azure AD voor de ondersteuning van eenmalige aanmelding. Door het registreren van uw toepassing met Azure AD, hebt u controle over het verificatiebeleid voor de toepassing. Zie voor meer informatie, [hulp voor ontwikkelaars](developer-guidance-for-integrating-applications.md).

-   **Niet-galerietoepassingen** : Breng uw eigen toepassingen. Ondersteuning voor eenmalige aanmelding voor andere apps door ze toe te voegen aan Azure AD. U kunt een webkoppeling die u wilt, of een toepassing die een gebruikersnaam en wachtwoord veld wordt weergegeven, biedt ondersteuning voor SAML- of OpenID Connect-protocol of SCIM ondersteunt integreren. Zie voor meer informatie, [configureren van eenmalige aanmelding voor apps van buiten de galerie](configure-single-sign-on-non-gallery-applications.md).

## <a name="manage-risk-with-conditional-access-policies"></a>Beheer de risico’s met een voorwaardelijk toegangsbeleid
Azure AD eenmalige aanmelding (SSO) met koppeling [voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) biedt een hoge mate van beveiliging voor toegang tot toepassingen. Mogelijkheden voor beveiliging zijn schaalbare cloud-identiteitsbeveiliging, risicogebaseerde meervoudige verificatie en beleid voor voorwaardelijke toegang. Dankzij deze mogelijkheden kunt een nauwkeurig beheer creëren op basis van toepassingen of van groepen die een hoger niveau van beveiliging nodig hebben.

## <a name="improve-productivity-with-single-sign-on"></a>Verhoog de productiviteit met eenmalige aanmelding
Eenmalige aanmelding (SSO) voor toepassingen en Office 365 biedt een superieure aanmeldingservaring voor bestaande gebruikers door verschillende aanmeldingen te elimineren. Er wordt een samenhangende gebruikersomgeving gecreëerd zonder afleiding door meerdere aanmeldschermen of de noodzaak om meerdere wachtwoorden te beheren. De bedrijfsgroep kan toegang beheren en goedkeuren via selfservice en dynamisch lidmaatschap. De zorg dat alleen de juiste personen in het bedrijf toegang hebben tot een toepassing verbetert de beveiliging van het identiteitssysteem.

SSO verbetert de beveiliging. *Zonder eenmalige aanmelding* moeten beheerders gebruikersaccounts maken en bijwerken voor elke afzonderlijke toepassing. Dat kost waardevolle tijd. Gebruikers moeten meerdere referenties bijhouden voor toegang tot hun toepassingen. Als gevolg hiervan schrijven gebruikers vaak hun wachtwoorden op of gebruiken andere oplossingen voor wachtwoordbeheer waarmee het beveiligingsrisico stijgt. [Meer informatie over eenmalige aanmelding](what-is-single-sign-on.md).

## <a name="address-governance-and-compliance"></a>Voeg governance en naleving toe
Met Azure AD, kunt u de toepassing-aanmeldingen bewaken met behulp van rapporten die gebruikmaken van hulpprogramma's voor beveiligingsincidenten en gebeurtenisbewaking (SIEM). U kunt de rapporten openen vanuit de portal of via API's. Programmatisch controleren wie toegang heeft tot uw toepassingen en toegang verwijderen van niet-actieve gebruikers via toegangsbeoordelingen.

## <a name="manage-costs"></a>Kosten beheren
Door te migreren naar Azure AD, kunt u kosten besparen en wordt het beheer van on-premises infrastructuur makkelijk. Azure AD biedt ook self-service tot toepassingen, en bespaart daarmee tijd voor beheerders en gebruikers. Eenmalige aanmelding elimineert de noodzaak van toepassingsspecifieke wachtwoorden. De mogelijkheid u slechts eenmaal te hoeven aanmelden, bespaart op kosten in verband met wachtwoordherstel voor toepassingen en wordt productieverlies voorkomen bij het ophalen van wachtwoorden.

## <a name="next-steps"></a>Volgende stappen

- [Wat is Application Proxy?](what-is-application-proxy.md)
- [Snelstart: Een toepassing in de galerie aan uw Azure AD-tenant toevoegen](add-application-portal.md)
