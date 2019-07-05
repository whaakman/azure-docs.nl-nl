---
title: Gebruik van verificatie-methoden en inzichten reporting (preview) - Azure Active Directory
description: Rapportage over Azure AD Self-service voor wachtwoord opnieuw instellen en het gebruik van multi-factor Authentication-verificatie methode
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0f6a74308f1bc4a7b77576fb9f39f965de0a4f8
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561027"
---
# <a name="authentication-methods-usage--insights-preview"></a>Gebruik van verificatie-methoden en inzichten (preview)

Gebruik en inzichten krijgt u inzicht in hoe uw organisatie verificatiemethoden voor functies, zoals Azure multi-factor Authentication en selfservice voor wachtwoord opnieuw instellen werkt. Door deze rapportagemogelijkheid biedt uw organisatie de mogelijkheid om te begrijpen welke methoden worden wordt geregistreerd en hoe deze worden gebruikt.

## <a name="permissions-and-licenses"></a>Machtigingen en licenties

De volgende rollen hebben toegang tot gebruik en informatie:

- Globale beheerder
- Beveiligingslezer
- Beveiligingsbeheerder
- Rapportenreader

Er is geen aanvullende licenties is vereist voor het gebruik van de toegang en informatie. Azure multi-factor Authentication en licentiegegevens voor self-service voor wachtwoord opnieuw instellen (SSPR) kunnen u vinden op de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="how-it-works"></a>Hoe werkt het?

Voor toegang tot verificatie methode gebruik en informatie:

1. Blader naar de [Azure-portal](https://portal.azure.com).
1. Blader naar **Azure Active Directory** > **wachtwoordherstel** > **gebruik & insights**.
1. Uit de **registratie** of **gebruik** overzichten, kunt u de vooraf gefilterde open rapporten te filteren op basis van uw behoeften.

![Overzicht van gebruik en inzichten](./media/howto-authentication-methods-usage-insights/usage-insights-overview.png)

Als u wilt rechtstreeks toegang hebben tot gebruik en inzichten, gaat u naar [ https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade). Deze koppeling gaat u naar het Registratieoverzicht van de.

De gebruikers zijn geregistreerd, wordt gebruikers ingeschakeld en gebruikers kunnen tegels weergeven in de volgende registratiegegevens voor uw gebruikers:

- Geregistreerd: Een gebruiker wordt beschouwd als geregistreerd als ze (of een beheerder) voldoende verificatiemethoden om te voldoen aan de self-service voor Wachtwoordherstel of multi-Factor Authentication beleid van uw organisatie hebt geregistreerd.
- Ingeschakeld: Een gebruiker wordt beschouwd als ingeschakeld als ze zich binnen het bereik van de SSPR-beleid. Self-service voor Wachtwoordherstel is ingeschakeld voor een groep, klikt u vervolgens de gebruiker worden beschouwd als ingeschakeld als ze zich in die groep. Als de self-service voor Wachtwoordherstel is ingeschakeld voor alle gebruikers, en vervolgens alle gebruikers in de tenant (met uitzondering van gasten) worden beschouwd als ingeschakeld.
- Kan: Een gebruiker wordt beschouwd als geschikt als ze zijn zowel geregistreerd en ingeschakeld. Deze status betekent dat ze SSPR op elk gewenst moment uitvoeren kunnen indien nodig.

Te klikken op een van deze tegels of de inzichten die erin worden weergegeven, gaat u naar een vooraf gefilterde lijst met Registratiedetails van de.

De **registraties** grafiek op het **registratie** tabblad toont het aantal geslaagde en mislukte verificatie methode registraties door de methode voor verificatie. De **stelt** grafiek op het **gebruik** tabblad toont het aantal geslaagde en mislukte verificaties tijdens het wachtwoord opnieuw instellen van stroom door een methode voor verificatie.

Te klikken op een van de grafieken zorgen voor een vooraf gefilterde lijst van de inschrijving of opnieuw instellen van gebeurtenissen.

Met behulp van het besturingselement in de bovenste, rechter hoek, kunt u het datumbereik voor de audit-gegevens wordt weergegeven in de registratie en opnieuw instellen van wachtwoorden en grafieken tot 24 uur, 7 dagen of 30 dagen wijzigen.

Registratiegegevens van de 

### <a name="registration-details"></a>Details van inschrijving

Te klikken op de **gebruikers geregistreerd**, **gebruikers ingeschakeld**, of **kunnen gebruikers** tegels of inzichten gaat u naar de registratiedetails van de.

De registratie-rapport details bevat de volgende informatie voor elke gebruiker:

- Name
- Gebruikersnaam
- Registratiestatus (alle, geregistreerd, niet is geregistreerd)
- De status ingeschakeld (alle, ingeschakeld, is niet ingeschakeld)
- Kan de status (All, mogelijk, niet geschikt)
- Methoden (App-melding, App-code, telefonische oproepen, SMS, E-mail, vragen over de beveiliging)

Met behulp van de besturingselementen aan de bovenkant van de lijst, kunt u een gebruiker zoeken en filteren van de lijst met gebruikers op basis van de kolommen die worden weergegeven.

### <a name="reset-details"></a>Details van het opnieuw instellen

Op de grafieken registraties of opnieuw instellen van wachtwoorden te klikken, gaat u naar de details van het opnieuw instellen.

Het rapport opnieuw instellen van details ziet u gebeurtenissen voor registratie en opnieuw instellen van de afgelopen 30 dagen, met inbegrip van:

- Name
- Gebruikersnaam
- Functie (alle, registratie, opnieuw ingesteld)
- Methode voor netwerkverificatie (App-melding, App-code, telefonische oproep, Office-oproep, SMS, E-mail, vragen over de beveiliging)
- Status (alle, geslaagd, mislukt)

Met behulp van de besturingselementen aan de bovenkant van de lijst, kunt u een gebruiker zoeken en filteren van de lijst met gebruikers op basis van de kolommen die worden weergegeven.

## <a name="limitations"></a>Beperkingen

De gegevens op deze rapporten worden vertraagd door tot 60 minuten. Er bestaat een veld 'Laatst bijgewerkt' in de Azure portal om te bepalen hoe recent is van uw gegevens.

Gebruiks-en inzichten is geen vervanging voor de Azure multi-factor Authentication-activiteitenrapporten of de informatie in het rapport van Azure AD-aanmeldingen.

## <a name="next-steps"></a>Volgende stappen

- [Werken met het gebruiksrapport van de verificatie-methoden API](https://docs.microsoft.com/graph/api/resources/authenticationmethods-usage-insights-overview?view=graph-rest-beta)
- [Verificatiemethoden voor uw organisatie te kiezen](concept-authentication-methods.md)
- [Registratie-ervaring gecombineerd](concept-registration-mfa-sspr-combined.md)
