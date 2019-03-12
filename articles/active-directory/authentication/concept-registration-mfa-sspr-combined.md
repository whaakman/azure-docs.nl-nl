---
title: Gecombineerde registratie voor Azure AD SSPR en MFA (preview)
description: Azure AD multi-factor Authentication en Self-service voor wachtwoord opnieuw instellen van inschrijving (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa9872ef76264956430bb69856a197042c196dfd
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57550187"
---
# <a name="combined-security-information-registration-preview"></a>Gecombineerde security informatie registratie (preview)

Vóór de gecombineerde inschrijving gebruikers verificatiemethoden geregistreerd voor Azure multi-factor Authentication (MFA) en selfservice voor wachtwoordherstel (SSPR) via twee verschillende ervaringen. Personen zijn verward dat vergelijkbare methoden zijn gebruikt voor Azure MFA en SSPR, maar ze afzonderlijk registreren voor elke functie heeft. Nu, met de registratie van de gecombineerde, gebruikers kunnen één keer registreren en profiteer van de voordelen van Azure MFA en de self-service voor Wachtwoordherstel.

![Informatie over beveiliging - Mijn profiel geregistreerde beveiligingsgegevens voor een gebruiker, met inbegrip van Microsoft Authenticator en het telefoonnummer voor een voorbeeldgebruiker in de map met gecombineerd.](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Bekijk voordat u de nieuwe ervaring is ingeschakeld, deze beheerder gerichte documentatie en de gebruiker gerichte documentatie om te controleren of u informatie over de functionaliteit en de gevolgen van deze functie. Baseer uw training over de documentatie voor de gebruiker voor het voorbereiden van uw gebruikers voor de nieuwe ervaring en ervoor zorgen dat een geslaagde implementatie.

|     |
| --- |
| Registratie van de gecombineerde security informatie voor Azure multi-factor Authentication en Azure AD Self-service voor wachtwoord opnieuw instellen is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

> [!IMPORTANT]
> Als een gebruiker is ingeschakeld voor zowel de oorspronkelijke Preview-versie en de uitgebreide gecombineerde registratie-ervaring, zien ze de nieuwe ervaring. Gebruikers die zijn ingeschakeld voor beide ervaringen zien alleen de nieuwe ervaring voor mijn profiel. De nieuwe Mijn profiel uitgelijnd met het uiterlijk van de gecombineerde inschrijving en biedt een naadloze ervaring voor gebruikers. Gebruikers kunnen mijn profiel bekijken door te gaan naar [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com).

De pagina Mijn profiel worden gelokaliseerd op basis van de huidige taalinstellingen op de computer die toegang tot de pagina. De meest recente taal gebruikt in de browser-cache, zodat de daaropvolgende pogingen worden gedaan om toegang te blijven moeten worden weergegeven in de laatste taal die wordt gebruikt door Microsoft wordt opgeslagen. De cache wissen zorgt ervoor dat de pagina's opnieuw te genereren. Als u wilt afdwingen dat een specifieke taal toe te voegen een `?lng=de-DE` aan het einde van de URL waar `de-DE` is ingesteld op de juiste taal code de pagina's om weer te geven in die taal wordt afgedwongen.

![Mijn profiel-interface van beveiligingsgegevens en de mogelijkheid voor gebruikers voor het instellen van SSPR of andere verificatiemethoden voor extra beveiliging.](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-converged-registration"></a>Methoden die beschikbaar zijn in geconvergeerde registratie

Op dit moment ondersteunt gecombineerde registratie van de volgende methoden en acties voor deze methoden:

|   | Registreren | Wijzigen | Verwijderen |
| --- | --- | --- | --- |
| Microsoft Authenticator | Ja (maximaal 5) | Nee | Ja |
| Andere verificator-app | Ja (maximaal 5) | Nee | Ja |
| Hardwaretoken | Nee | Nee | Ja |
| Telefoon | Ja | Ja | Ja |
| Alternatief telefoonnummer | Ja | Ja | Ja |
| Telefoon (werk) | Nee | Nee | Nee |
| Email | Ja | Ja | Ja |
| Beveiligingsvragen | Ja | Nee | Ja |
| App-wachtwoorden | Ja | Nee | Ja |

> [!NOTE]
> App-wachtwoorden zijn alleen beschikbaar voor gebruikers die zijn ingesteld voor MFA. App-wachtwoorden zijn niet beschikbaar voor gebruikers die zijn ingeschakeld voor MFA via beleid voor voorwaardelijke toegang.

Gebruikers kunnen de volgende opties instellen als de standaardmethode voor MFA:

- Microsoft Authenticator-melding
- Authenticator-app of hardware-token-code
- Telefoonoproep
- Sms-bericht

Naarmate er steeds meer verificatiemethoden dergelijke toevoegen aan Azure AD, is deze methoden zijn beschikbaar in de registratie van de gecombineerde.

## <a name="combined-registration-modes"></a>Gecombineerde registratie modi

Er zijn twee "modi' van de gecombineerde inschrijving: onderbreken en te beheren. 

Interrupt-modus, wordt een wizard-achtige ervaring, die wordt weergegeven aan een gebruiker wanneer ze registreren of hun beveiligingsgegevens bij het aanmelden vernieuwen. 

Beheren van modus is onderdeel van het gebruikersprofiel en kunnen ze hun beveiligingsgegevens beheren. 

Voor beide modi wordt als een gebruiker heeft eerder een methode die kan worden gebruikt voor MFA, geregistreerd moet ze uit te voeren MFA voordat ze krijgen hun beveiligingsgegevens tot toegang.

### <a name="interrupt-mode"></a>Interrupt-modus

De registratie van de gecombineerde respecteert zowel MFA en SSPR-beleid als beide zijn ingeschakeld voor uw tenant. Deze beleidsregels bepalen, of een gebruiker om u te registreren tijdens de aanmelding wordt onderbroken en welke methoden zijn beschikbaar om u te registreren.

De volgende lijst diverse scenario's waarbij een gebruiker mogelijk gevraagd om te registreren of vernieuwen hun beveiligingsgegevens:

* MFA-registratie afgedwongen via Identity Protection: Gebruikers wordt gevraagd om u te registreren tijdens het aanmelden. Ze zich registreren MFA-methoden en de SSPR-methoden (als de gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel).
* MFA-inschrijving via MFA per gebruiker afgedwongen: Gebruikers wordt gevraagd om u te registreren tijdens het aanmelden. Ze zich registreren MFA-methoden en de SSPR-methoden (als de gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel).
* MFA-registratie via voorwaardelijke toegang of een ander beleid afgedwongen: Gebruikers wordt gevraagd om u te registreren bij het openen van een resource die MFA is vereist. Gebruikers registreren MFA-methoden en de SSPR-methoden (als de gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel).
* SSPR-registratie in afgedwongen: Gebruikers wordt gevraagd om u te registreren tijdens het aanmelden. Ze SSPR-methoden voor het alleen registreren
* SSPR vernieuwen afgedwongen: Gebruikers moeten hun beveiligingsgegevens te controleren op basis van een interval dat is ingesteld door de beheerder. Gebruikers hun gegevens worden weergegeven en kies 'Ziet er goed' of wijzigingen aanbrengen, indien nodig.

Wanneer een registratie van kracht is, worden gebruikers het minimum aantal methoden die nodig zijn om te voldoen aan zowel MFA en SSPR-beleid van de meeste weergegeven aan de minst veilige optie.

Voorbeeld:

* Een gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel. De SSPR-beleid vereist twee methoden voor het opnieuw instellen en mobiele app-code, e-mailadres en telefoonnummer is ingeschakeld.
   * Deze gebruiker is vereist voor de registratie van twee methoden.
      * Standaard worden deze bent authenticator-app en het telefoonnummer weergegeven.
      * De gebruiker kan kiezen voor het registreren van e-mailadres in plaats van de authenticator-app of telefoon.

Het volgende stroomdiagram wordt beschreven welke methoden worden weergegeven aan een gebruiker wanneer onderbroken registreren tijdens het aanmelden:

![Gecombineerd security info-stroomdiagram uitleg over het aantal methoden dat is vereist wanneer u meer informatie is vereist bij het aanmelden. Dit kan invloed hebben als er slechts MFA of alleen SSPR vereist is](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Als u MFA en SSPR ingeschakeld hebt, raden wij registratie bij MFA af te dwingen.

Als de SSPR-beleid vereist dat gebruikers hun beveiligingsgegevens te controleren met een regelmatig interval, worden gebruikers onderbroken tijdens het aanmelden en hun geregistreerde methoden die worden weergegeven. Ze kunnen 'Ziet er goed' kiezen als de gegevens bijgewerkt is of ze kiezen kunnen 'Info bewerken' te wijzigen.

### <a name="manage-mode"></a>Modus beheren

Gebruikers hebben toegang tot modus beheren door te gaan naar [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo) of door te kiezen 'Beveiligingsgegevens' in Mijn profiel. Van daaruit kunnen gebruikers methoden toevoegen, verwijderen of wijzigen van bestaande methoden, wijzigen de standaardmethode en meer.

## <a name="key-usage-scenarios"></a>Belangrijkste gebruiksscenario 's

### <a name="set-up-security-info-during-sign-in"></a>Instellen van beveiligingsgegevens tijdens het aanmelden

Een beheerder heeft registratie afgedwongen.

Een gebruiker heeft niet alle vereiste beveiligingsgegevens ingesteld en navigeert u naar de Azure-portal. Na het invoeren van hun gebruikersnaam en wachtwoord, wordt de gebruiker gevraagd voor het instellen van beveiligingsgegevens. De gebruiker vervolgens volgende de stappen wordt weergegeven in de wizard voor het instellen van de vereiste beveiligingsgegevens. De gebruiker kan kiezen voor het instellen van andere methoden dan wat standaard wordt weergegeven als in uw instellingen. Aan het einde van de wizard controleert de gebruiker de methoden die ze hebben ingesteld en de standaardmethode voor MFA. Als u wilt het installatieproces hebt voltooid, wordt de gebruiker wordt bevestigd dat de gegevens en blijft de Azure-portal.

### <a name="set-up-security-info-from-my-profile"></a>Instellen van beveiligingsgegevens van mijn profiel

Een beheerder heeft de registratie niet afgedwongen.

Een gebruiker die nog niet is ingesteld om alle vereiste beveiligingsgegevens navigeert naar [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com). De gebruiker kiest **beveiligingsgegevens** in de navigatie aan de linkerkant. Van daaruit de gebruiker kiest voor het toevoegen van een methode, selecteert een van de methoden die voor hen beschikbaar, en volgt de stappen voor het instellen van deze methode. Wanneer u klaar bent, ziet de gebruiker de methode die ze zojuist hebt ingesteld op de pagina beveiligingsgegevens.

### <a name="delete-security-info-from-my-profile"></a>Beveiligingsgegevens verwijderen uit Mijn profiel

Een gebruiker die ten minste één methode eerder heeft ingesteld navigeert naar [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). De gebruiker ervoor kiest om een van de eerder geregistreerde methoden te verwijderen. Wanneer u klaar bent, ziet de gebruiker die methode niet meer op de pagina beveiligingsgegevens.

### <a name="change-default-method-from-my-profile"></a>Standaardmethode van mijn profiel wijzigen

Een gebruiker die ten minste één methode die kan worden gebruikt voor MFA eerder heeft ingesteld navigeert naar [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). De gebruiker wijzigt de standaardmethode voor hun huidige op een andere standaard-methode. Wanneer u klaar bent, ziet de gebruiker de standaardmethode voor nieuwe op de pagina beveiligingsgegevens.

## <a name="next-steps"></a>Volgende stappen

[De gecombineerde registratie in uw tenant inschakelen](howto-registration-mfa-sspr-combined.md)

[Beschikbare methoden voor MFA en Self-service voor Wachtwoordherstel](concept-authentication-methods.md)

[Self-service voor wachtwoord opnieuw instellen configureren](howto-sspr-deployment.md)

[Azure multi-factor Authentication configureren](howto-mfa-getstarted.md)
