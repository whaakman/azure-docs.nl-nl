---
title: Feedback geven over risicogebeurtenissen in Azure AD Identity Protection - Azure Active Directory
description: Hoe en waarom moet u feedback geven over risicogebeurtenissen Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66d53590e89afb1a903b22ff60e32871a1502ada
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827903"
---
# <a name="how-to-give-risk-feedback-in-azure-ad-identity-protection"></a>Procedure: Feedback geven risico's in Azure AD Identity Protection

Azure AD Identity Protection kunt u feedback geven over de risico-evaluatie. Het volgende document geeft een lijst van de scenario's waar u graag feedback geven over de risico-evaluatie van Azure AD Identity Protection en hoe we opnemen.

## <a name="what-is-a-detection"></a>Wat is een detectie?

De detectie van een Identity Protection is een indicator van verdachte activiteiten van een identiteit risico perspectief. Deze verdachte activiteiten worden risicogebeurtenissen genoemd. Deze detecties op basis van identiteit kunnen worden gebaseerd op methodiek, machine learning of kunnen afkomstig zijn van de producten van partners. Deze detecties worden gebruikt om te bepalen aanmeldingsrisico en gebruikersrisico,

* Gebruikersrisico vertegenwoordigt de kans dat een identiteit is geknoeid.
* Aanmeldingsrisico vertegenwoordigt de kans dat een aanmelding is aangetast (bijvoorbeeld: de aanmelding is niet gemachtigd door de eigenaar van de identiteit).

## <a name="why-should-i-give-risk-feedback-to-azure-ads-risk-assessments"></a>Waarom moet ik risico feedback geven aan Azure AD-risicobeoordelingen? 

Er zijn diverse redenen waarom u Azure AD risico feedback te geven:

1. **U Azure AD-gebruiker of aanmelden risicoanalyse onjuist gevonden**. Bijvoorbeeld, een aanmelding die wordt weergegeven in het rapport 'Riskante aanmeldingen' is goedaardige en alle detecties voor die aanmelding zijn fout-positieven.
1. **U gevalideerd die Azure AD-gebruiker of aanmelden risico-evaluatie is juist**. Bijvoorbeeld, een aanmelding die wordt weergegeven in het rapport 'Riskante aanmeldingen' inderdaad schadelijk is en u wilt dat Azure AD om te weten dat alle detecties voor die aanmelding echt positieven zijn.
1. **Hersteld van de risico's op die gebruiker buiten Azure AD Identity Protection** en u wilt dat het risiconiveau van de gebruiker worden bijgewerkt.

## <a name="how-does-azure-ad-use-my-risk-feedback"></a>Hoe wordt mijn feedback risico's in Azure AD gebruikt?

Azure AD maakt gebruik van uw feedback om bij te werken het risico van de onderliggende gebruiker en/of aanmelden. Deze feedback helpt beveiligen van de eindgebruiker. Zodra u hebt geverifieerd dat een aanmelding is geknoeid, Azure AD onmiddellijk wordt verhoogd risico's van de gebruiker en de cumulatieve aanmeldingsrisico (niet realtime risico's) op hoog is. Als deze gebruiker is opgenomen in uw beleid voor gebruikersrisico's om af te dwingen een hoog risico gebruikers veilig hun wachtwoord opnieuw instellen, worden de gebruiker automatisch hersteld zelf de volgende keer dat ze zich aanmelden bij.

## <a name="how-should-i-give-risk-feedback-and-what-happens-under-the-hood"></a>Hoe geef ik risico feedback en wat er gebeurt achter de schermen?

Hier worden de scenario's en mechanismen voor risico's om feedback te geven aan Azure AD.

| Scenario | Hoe kan ik feedback geven? | Wat gebeurt er achter de schermen? | Opmerkingen |
| --- | --- | --- | --- |
| **Aanmelding niet worden gecompromitteerd (fout-positief)** <br> 'Riskante aanmeldingen' rapport bevat een hoog risico aanmelding [status risico lopen =], maar dat aanmelding niet is aangetast. | Selecteer de aanmelding en klik op 'Bevestigen aanmelden veilige'. | Azure AD gaat van de aanmelding cumulatieve risico op none [risico status = bevestigd veilige; Risiconiveau (totaal) =-] en de invloed ervan op de gebruikersrisico wordt omkeren. | De optie 'Bevestigen aanmelden veilig' is momenteel alleen beschikbaar in 'Riskante aanmeldingen' rapport. |
| **Aanmelden aangetast (terecht positief)** <br> 'Riskante aanmeldingen' rapport bevat een hoog risico aanmelding [status risico lopen =] met laag risico [risiconiveau (totaal) = laag] en dat aanmelding inderdaad is aangetast. | Selecteer de aanmelding en klik op 'Bevestigen aanmelden aangetast'. | Azure AD wordt verplaatst van de aanmelding cumulatieve risico's en het gebruikersrisico op hoog [risico status = bevestigd aangetast; Risiconiveau = hoog]. | De optie 'Bevestigen aanmelden aangetast' is momenteel alleen beschikbaar in 'Riskante aanmeldingen' rapport. |
| **Gebruiker aangetast (terecht positief)** <br> 'Riskante gebruikers' rapport bevat een hoog risico gebruiker [staat het risico lopen =] met laag risico [risiconiveau = laag] en dat de gebruiker inderdaad is aangetast. | Selecteer de gebruiker en klik op 'Bevestigen user aangetast'. | Azure AD gaat het gebruikersrisico op hoog [risico status = bevestigd aangetast; Risiconiveau = hoog] en een nieuwe detectie wordt toegevoegd 'Admin bevestigd gebruiker aangetast'. | De optie 'Gebruiker bevestigen aangetast' is momenteel alleen beschikbaar in 'Riskante gebruikers' rapport. <br> De detectie 'Admin bevestigd gebruiker aangetast' wordt weergegeven op het tabblad 'Risicogebeurtenissen niet is gekoppeld aan een aanmelding' in het rapport 'Riskante gebruikers'. |
| **Gebruiker hersteld buiten Azure AD Identity Protection (terecht positief + Remediated)** <br> 'Riskante gebruikers' rapport toont een hoog risico en ik vervolgens de gebruiker buiten Azure AD Identity Protection hebt hersteld. | 1. Selecteer de gebruiker en klik op 'Bevestigen user aangetast'. (Dit proces wordt bevestigd dat naar Azure AD dat de gebruiker inderdaad is aangetast.) <br> 2. Wachten op 'risiconiveau' van de gebruiker om te gaan op hoog. (Dit moment biedt Azure AD de benodigde tijd voor het nemen van de bovenstaande feedback aan de risico-engine.) <br> 3. Selecteer de gebruiker en klik op 'Gebruikersrisico negeren'. (Dit proces wordt bevestigd dat naar Azure AD dat de gebruiker niet meer is geknoeid.) |  Azure AD wordt verplaatst van het gebruikersrisico geen [risico status = genegeerd; Risiconiveau =-] en sluit u het risico op alle bestaande aanmeldingen met actieve risico's. | Alle risico's op de gebruiker en het verleden aanmeldingen wordt nu afgesloten te klikken op 'Gebruikersrisico negeren'. Deze actie kan niet ongedaan worden gemaakt. |
| **Gebruiker niet worden gecompromitteerd (fout-positief)** <br> 'Riskante gebruikers' rapport staan op welke gebruiker, maar de gebruiker niet is geknoeid. | Selecteer de gebruiker en klik op 'Gebruikersrisico negeren'. (Dit proces wordt bevestigd dat naar Azure AD dat de gebruiker niet worden gecompromitteerd.) | Azure AD wordt verplaatst van het gebruikersrisico geen [risico status = genegeerd; Risiconiveau =-]. | Alle risico's op de gebruiker en het verleden aanmeldingen wordt nu afgesloten te klikken op 'Gebruikersrisico negeren'. Deze actie kan niet ongedaan worden gemaakt. |
| Ik wil de gebruikersrisico sluiten, maar ik weet niet zeker of de gebruiker waarmee is geknoeid / veilig is. | Selecteer de gebruiker en klik op 'Gebruikersrisico negeren'. (Dit proces wordt bevestigd dat naar Azure AD dat de gebruiker niet meer is geknoeid.) | Azure AD wordt verplaatst van het gebruikersrisico geen [risico status = genegeerd; Risiconiveau =-]. | Alle risico's op de gebruiker en het verleden aanmeldingen wordt nu afgesloten te klikken op 'Gebruikersrisico negeren'. Deze actie kan niet ongedaan worden gemaakt. U wordt aangeraden u herstellen van de gebruiker door te klikken op 'Wachtwoord opnieuw instellen' of vraag de gebruiker veilig opnieuw instellen/wijzigen hun referenties. |

Feedback op gebruiker risicogebeurtenissen in Identity Protection offline wordt verwerkt en duurt enige tijd om bij te werken. Het verwerken van de kolom Status risico biedt de huidige status van de verwerking van de feedback.

![Het risico van de verwerkingsstatus voor rapport riskante gebruikers](./media/howto-provide-risk-event-feedback/risky-users-provide-feedback.png)

## <a name="next-steps"></a>Volgende stappen

[Azure Active Directory Identity Protection risicogebeurtenissen verwijzen naar](risk-events-reference.md)