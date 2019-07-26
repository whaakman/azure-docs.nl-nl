---
title: Toegang tot apps beheren met Azure AD | Microsoft Docs
description: Hierin wordt beschreven hoe u met beAzure Active Directory organisaties de apps kunt opgeven waarvoor elke gebruiker toegang heeft.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99c7947b6469f64f2ea05b2290305710db1ee796
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477113"
---
# <a name="managing-access-to-apps"></a>Toegang tot apps beheren
Doorlopend toegangs beheer, evaluatie van het gebruik en rapportage blijven een uitdaging nadat een app is geïntegreerd in het identiteits systeem van uw organisatie. In veel gevallen moeten IT-beheerders of Help Desk een voortdurende actieve rol nemen bij het beheren van de toegang tot uw apps. Soms wordt de toewijzing uitgevoerd door een algemeen of divisie IT-team. De toewijzings beslissing is vaak bedoeld om te worden gedelegeerd aan de bedrijfs beslissings Maker, waardoor de goed keuring wordt vereist voordat de toewijzing wordt gemaakt.  Andere organisaties investeren in integratie met een bestaand geautomatiseerd systeem voor identiteits-en toegangs beheer, zoals op rollen gebaseerde Access Control (RBAC) of op kenmerken gebaseerde Access Control (ABAC). Zowel de integratie-als regel ontwikkeling is vaak gespecialiseerd en kostbaar. Bewaking of rapportage over beide beheer methoden is een eigen afzonderlijke, dure en complexe investering.

## <a name="how-does-azure-active-directory-help"></a>Hoe helpt Azure Active Directory?
 Azure AD biedt ondersteuning voor uitgebreid toegangs beheer voor geconfigureerde toepassingen, waardoor organisaties eenvoudig het juiste toegangs beleid kunnen bereiken, variërend van automatische, toewijzing op basis van kenmerken (ABAC-of RBAC-scenario's) via delegering en met inbegrip van beheer van Administrators. Met Azure AD kunt u eenvoudig complexe beleids regels bereiken, meerdere beheer modellen combi neren voor één toepassing en beheer regels zelfs opnieuw gebruiken voor verschillende toepassingen met dezelfde doel groepen.

* [Nieuwe of bestaande toepassingen toevoegen](add-gallery-app.md)

  De toepassings toewijzing van Azure AD richt zich op twee primaire toewijzings modi:

* **Afzonderlijke toewijzing** Een IT-beheerder met globale mappen beheerder machtigingen kan afzonderlijke gebruikers accounts selecteren en toegang verlenen tot de toepassing.
* **Toewijzing op basis van een groep (alleen betaalde Azure AD)** Een IT-beheerder met de machtigingen van de globale Directory beheerder kan een groep toewijzen aan de toepassing. De toegang van specifieke gebruikers wordt bepaald door het feit of ze lid zijn van de groep op het moment dat ze toegang proberen te krijgen tot de toepassing. Met andere woorden, een beheerder kan effectief een toewijzings regel maken met de tekst ' elk huidig lid van de toegewezen groep heeft toegang tot de toepassing '. Met deze toewijzings optie kunnen beheerders profiteren van Azure AD-opties voor groeps beheer, waaronder [dynamische groepen op basis van kenmerken](../fundamentals/active-directory-groups-create-azure-portal.md), externe systeem groepen (bijvoorbeeld on-premises Active Directory of workday), of Door de beheerder beheerde of selfservice beheerde groepen. Eén groep kan eenvoudig worden toegewezen aan meerdere apps, zodat toepassingen met toewijzings affiniteit toewijzings regels kunnen delen, waardoor de algehele beheer complexiteit wordt verminderd. Houd er rekening mee dat geneste groepslid maatschappen op dit moment niet worden ondersteund voor toewijzing op basis van een groep.

Met behulp van deze twee toewijzings modi kunnen beheerders een wenselijk toewijzings beheer methode bereiken.

Met Azure AD is het rapporteren van gebruik en toewijzing volledig geïntegreerd, waardoor beheerders eenvoudig kunnen rapporteren over de toewijzings status, toewijzings fouten en zelfs gebruik.

## <a name="complex-application-assignment-with-azure-ad"></a>Complexe toepassings toewijzing met Azure AD
Overweeg een toepassing zoals Sales Force. In veel organisaties wordt Sales Force voornamelijk gebruikt door de marketing-en verkoop teams. Vaak hebben leden van het marketing team zeer uitgebreide toegang tot Sales Force, terwijl leden van het verkoop team beperkte toegang hebben. In veel gevallen heeft een brede populatie aan informatie medewerkers de toegang tot de toepassing beperkt. Uitzonde ringen op deze regels zijn belang rijk voor het bemoeilijken. Het is vaak de prerogative van de marketing-of verkoop leiders om een gebruiker toegang te verlenen of hun rollen onafhankelijk van deze algemene regels te wijzigen.

Met Azure AD kunnen toepassingen zoals Sales Force vooraf worden geconfigureerd voor eenmalige aanmelding (SSO) en geautomatiseerde inrichting. Zodra de toepassing is geconfigureerd, kan een beheerder de eenmalige actie ondernemen om de juiste groepen te maken en toe te wijzen. In dit voor beeld kan een beheerder de volgende toewijzingen uitvoeren:

* [Dynamische groepen](../fundamentals/active-directory-groups-create-azure-portal.md) kunnen worden gedefinieerd om automatisch alle leden van de marketing-en verkoop teams weer te geven met behulp van kenmerken als afdeling of rol:
  
  * Alle leden van de marketing groepen worden toegewezen aan de rol ' Marketing ' in Sales Force
  * Alle leden van de verkoop team groepen worden toegewezen aan de rol ' Sales ' in Sales Force. Een verdere verfijning kan gebruikmaken van meerdere groepen die regionale verkoop teams vertegenwoordigen die aan verschillende Sales Force-rollen zijn toegewezen.
* Om het uitzonderings mechanisme in te scha kelen, kan voor elke rol een self-service groep worden gemaakt. Bijvoorbeeld, de groep ' Sales Force marketing Exception ' kan worden gemaakt als een self-service groep. De groep kan worden toegewezen aan de marketing functie van Sales Force en het marketing leiderschaps team kan eigenaar zijn. Leden van het marketing leiderschaps team kunnen gebruikers toevoegen of verwijderen, een lidmaatschaps beleid instellen of zelfs aanvragen voor individuele gebruikers goed keuren of weigeren om lid te worden. Dit mechanisme wordt ondersteund door een informatie medewerker die de juiste ervaring heeft die geen gespecialiseerde training vereist voor eigen aren of leden.

In dit geval worden alle toegewezen gebruikers automatisch ingericht voor Sales Force, omdat ze worden toegevoegd aan verschillende groepen hun roltoewijzing wordt bijgewerkt in Sales Force. Gebruikers zouden Sales Force kunnen ontdekken en openen via het toegangs venster van micro soft-toepassingen, Office-webclients of zelfs door te navigeren naar de aanmeldings pagina van de organisatie Sales Force. Beheerders kunnen de gebruiks-en toewijzings status eenvoudig weer geven met Azure AD-rapportage.

Beheerders kunnen [voorwaardelijke toegang tot Azure AD](../active-directory-conditional-access-azure-portal.md) gebruiken om toegangs beleid in te stellen voor specifieke rollen. Deze beleids regels kunnen omvatten of toegang is toegestaan buiten de bedrijfs omgeving en zelfs multi-factor Authentication of apparaat-vereisten om in verschillende gevallen toegang te verkrijgen.

## <a name="next-steps"></a>Volgende stappen
* [Apps beveiligen met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md)
* [Self-service voor groeps beheer/SSAA](../users-groups-roles/groups-self-service-management.md)
