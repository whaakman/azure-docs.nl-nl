---
title: Registratie gecombineerd voor Azure AD SSPR en multi-factor Authentication (preview) - Azure Active Directory
description: Azure AD multi-factor Authentication en Self-service voor wachtwoord opnieuw instellen van inschrijving (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f3eec1f846f1b74ab3e19bca022d4e009540d1a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280006"
---
# <a name="combined-security-information-registration-preview"></a>Gecombineerde security informatie registratie (preview)

Vóór de gecombineerde inschrijving gebruikers verificatiemethoden voor meervoudige verificatie en selfservice voor wachtwoordherstel (SSPR) afzonderlijk geregistreerd. Personen zijn verward dat vergelijkbare methoden zijn gebruikt voor multi-factor Authentication en SSPR, maar ze waren om te registreren voor beide functies. Nu, met de registratie van de gecombineerde, gebruikers kunnen één keer registreren en profiteer van de voordelen van multi-factor Authentication en de self-service voor Wachtwoordherstel.

![Mijn profiel weergeven geregistreerd beveiligingsgegevens voor een gebruiker](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Bekijk voordat u de nieuwe ervaring is ingeschakeld, deze beheerder gerichte documentatie en de gebruiker gerichte documentatie om te controleren of u informatie over de functionaliteit en het effect van deze functie. Baseer uw training over de documentatie voor de gebruiker voor het voorbereiden van uw gebruikers voor de nieuwe ervaring en ervoor zorgen dat een geslaagde implementatie.

|     |
| --- |
| Gecombineerde security informatie registratie voor meervoudige verificatie en selfservice voor wachtwoord opnieuw instellen van Azure Active Directory (Azure AD) is een openbare preview-functie van Azure AD. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.|
|     |

> [!IMPORTANT]
> Gebruikers die zijn ingeschakeld voor zowel de oorspronkelijke Preview-versie en de uitgebreide gecombineerde registratie-ervaring wordt het nieuwe gedrag zien. Gebruikers die zijn ingeschakeld voor beide ervaringen ziet alleen de nieuwe Mijn profiel-ervaring. De nieuwe Mijn profiel uitgelijnd met het uiterlijk van de gecombineerde inschrijving en biedt een naadloze ervaring voor gebruikers. Gebruikers kunnen mijn profiel bekijken door te gaan naar [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com).

Mijn profiel pagina's worden gelokaliseerd op basis van de taalinstellingen van de computer die toegang tot de pagina. De meest recente taal gebruikt in de cache van de browser, zodat de daaropvolgende pogingen worden gedaan voor toegang tot de pagina's worden weergegeven in de laatste taal die wordt gebruikt door Microsoft wordt opgeslagen. Als u de cache uitschakelt, wordt de pagina opnieuw te genereren. Als u afdwingen dat een specifieke taal wilt, kunt u toevoegen `?lng=<language>` aan het einde van de URL waar `<language>` is de code van de taal die u wilt weergeven.

![Self-service voor Wachtwoordherstel of andere verificatiemethoden beveiliging instellen](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-combined-registration"></a>Methoden die beschikbaar zijn in de gecombineerde-registratie

Registratie ondersteunt de volgende verificatiemethoden en de acties gecombineerd:

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
> App-wachtwoorden zijn alleen beschikbaar voor gebruikers die zijn ingesteld voor meervoudige verificatie. App-wachtwoorden zijn niet beschikbaar voor gebruikers die zijn ingeschakeld voor multi-factor Authentication via beleid voor voorwaardelijke toegang.

Gebruikers kunnen een van de volgende opties als de standaardmethode voor multi-factor Authentication instellen:

- Microsoft Authenticator-melding.
- Authenticator-app of hardware-token-code.
- Telefonische oproep.
- SMS-bericht.

Naarmate er steeds meer verificatiemethoden toevoegen aan Azure AD, is deze methoden zijn beschikbaar in de registratie van de gecombineerde.

## <a name="combined-registration-modes"></a>Gecombineerde registratie-modi

Er zijn twee modi van de gecombineerde inschrijving: onderbreken en te beheren.

- **Modus interrupt** is een wizard-achtige ervaring, gepresenteerd aan gebruikers wanneer ze registreren of hun beveiligingsgegevens bij het aanmelden vernieuwen.

- **Modus beheren** maakt deel uit van het gebruikersprofiel en Hiermee kunnen gebruikers hun beveiligingsgegevens beheren.

Voor beide modi wordt moet gebruikers die een methode die kan worden gebruikt voor meervoudige verificatie, al hebben geregistreerd multi-factor Authentication uitvoeren voordat ze krijgen hun beveiligingsgegevens tot toegang.

### <a name="interrupt-mode"></a>Interrupt-modus

De registratie van de gecombineerde respecteert zowel de multi-factor Authentication en de SSPR-beleid als beide zijn ingeschakeld voor uw tenant. Dit beleid bepaalt of een gebruiker voor de registratie wordt onderbroken tijdens het aanmelden en welke methoden zijn beschikbaar voor registratie.

Hier volgen enkele scenario's waarin gebruikers mogelijk gevraagd om u te registreren of vernieuwen hun beveiligingsgegevens:

* Multi-factor Authentication-registratie is afgedwongen via Identity Protection: Gebruikers wordt gevraagd om u te registreren tijdens het aanmelden. Deze registreren verificatiemethoden op basis van meerdere factoren en SSPR methoden (als de gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel).
* Multi-factor Authentication inschrijving afgedwongen met behulp van de multi-factor Authentication per gebruiker: Gebruikers wordt gevraagd om u te registreren tijdens het aanmelden. Deze registreren verificatiemethoden op basis van meerdere factoren en SSPR methoden (als de gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel).
* Registratie van multi-factor Authentication-verificatie via voorwaardelijke toegang of andere beleidsregels afgedwongen: Gebruikers wordt gevraagd om u te registreren wanneer ze een bron waarvoor multi-factor Authentication gebruikt. Deze registreren verificatiemethoden op basis van meerdere factoren en SSPR methoden (als de gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel).
* SSPR-registratie in afgedwongen: Gebruikers wordt gevraagd om u te registreren tijdens het aanmelden. Ze registreren alleen methoden van self-service voor Wachtwoordherstel.
* SSPR vernieuwen afgedwongen: Gebruikers moeten hun beveiligingsgegevens te controleren op basis van een interval dat is ingesteld door de beheerder. Gebruikers kunnen hun gegevens worden weergegeven en bevestigen van de gegevens van de huidige of wijzigingen aanbrengen, indien nodig.

Wanneer een registratie van kracht is, worden gebruikers het minimum aantal methoden die nodig zijn om te voldoen aan zowel de multi-factor Authentication en de SSPR-beleid, van meest naar minst veilige optie weergegeven.

Bijvoorbeeld:

* Een gebruiker is ingeschakeld voor self-service voor Wachtwoordherstel. De SSPR-beleid vereist twee methoden voor het opnieuw instellen en mobiele app-code, e-mailadres en telefoonnummer is ingeschakeld.
   * Deze gebruiker is vereist voor de registratie van twee methoden.
      * De gebruiker wordt standaard authenticator-app en het telefoonnummer weergegeven.
      * De gebruiker kan kiezen voor het registreren van e-mailadres in plaats van de authenticator-app of telefoon.

Deze stroomdiagram wordt beschreven welke methoden worden weergegeven aan een gebruiker wanneer onderbroken registreren tijdens het aanmelden:

![Gecombineerde security info stroomdiagram](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Als u multi-factor Authentication en SSPR ingeschakeld hebt, wordt u aangeraden de registratie van de multi-factor Authentication af te dwingen.

Als de SSPR-beleid vereist dat gebruikers hun beveiligingsgegevens te controleren met regelmatige intervallen, worden gebruikers onderbroken tijdens het aanmelden en hun geregistreerde methoden die worden weergegeven. Als deze bijgewerkt is, of ze wijzigingen aanbrengen kunnen als ze moeten, kunnen ze de gegevens van de huidige bevestigen.

### <a name="manage-mode"></a>Modus beheren

Gebruikers hebben toegang tot modus beheren door te gaan naar [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo) of door te selecteren **beveiligingsgegevens** uit Mijn profiel. Van daaruit kunnen gebruikers methoden toevoegen, verwijderen of wijzigen van bestaande methoden, wijzigen de standaardmethode en meer.

## <a name="key-usage-scenarios"></a>Belangrijkste gebruiksscenario 's

### <a name="set-up-security-info-during-sign-in"></a>Instellen van beveiligingsgegevens tijdens het aanmelden

Een beheerder heeft registratie afgedwongen.

Een gebruiker heeft niet alle vereiste beveiligingsgegevens ingesteld en gaat u naar de Azure-portal. Na het invoeren van de gebruikersnaam en wachtwoord, wordt de gebruiker gevraagd voor het instellen van beveiligingsgegevens. De gebruiker vervolgens volgende de stappen wordt weergegeven in de wizard voor het instellen van de vereiste beveiligingsgegevens. Als uw instellingen toelaat, moet de gebruiker kan kiezen voor het instellen van andere methoden dan de standaard weergegeven. Nadat de wizard is voltooid, Controleer gebruikers de methoden die ze hebben ingesteld en de standaardmethode voor meervoudige verificatie. Als u wilt het installatieproces hebt voltooid, wordt de gebruiker wordt bevestigd dat de gegevens en blijft de Azure-portal.

### <a name="set-up-security-info-from-my-profile"></a>Instellen van beveiligingsgegevens van mijn profiel

Een beheerder heeft de registratie niet afgedwongen.

Een gebruiker die alle vereiste beveiligingsgegevens nog niet is ingesteld, gaat naar [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com). De gebruiker selecteert **beveiligingsgegevens** in het linkerdeelvenster. Van daaruit de gebruiker kiest voor het toevoegen van een methode kunt inschakelen, selecteert een van de beschikbare methoden en in de volgende stappen voor het instellen van deze methode. Wanneer u klaar bent, ziet de gebruiker de methode die zojuist is ingesteld op de pagina beveiligingsgegevens.

### <a name="delete-security-info-from-my-profile"></a>Beveiligingsgegevens verwijderen uit Mijn profiel

Een gebruiker die ten minste één methode eerder heeft ingesteld navigeert naar [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). De gebruiker ervoor kiest om een van de eerder geregistreerde methoden te verwijderen. Wanneer u klaar bent, ziet de gebruiker die methode niet meer op de pagina beveiligingsgegevens.

### <a name="change-the-default-method-from-my-profile"></a>De standaardmethode voor wijzigen van mijn profiel

Een gebruiker die ten minste één methode die kan worden gebruikt voor multi-factor Authentication eerder heeft ingesteld navigeert naar [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). De gebruiker wijzigt de huidige standaardmethode op een andere standaard-methode. Wanneer u klaar bent, ziet de gebruiker de standaardmethode voor nieuwe op de pagina beveiligingsgegevens.

## <a name="next-steps"></a>Volgende stappen

[De gecombineerde registratie in uw tenant inschakelen](howto-registration-mfa-sspr-combined.md)

[Beschikbare methoden voor meervoudige verificatie en Self-service voor Wachtwoordherstel](concept-authentication-methods.md)

[Self-service voor wachtwoord opnieuw instellen configureren](howto-sspr-deployment.md)

[Azure Multi-Factor Authentication configureren](howto-mfa-getstarted.md)
