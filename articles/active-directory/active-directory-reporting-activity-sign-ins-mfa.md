---
title: Naslaginformatie voor Multi-Factor Authentication-rapportage in Azure Portal | Microsoft Docs
description: Naslaginformatie voor Multi-Factor Authentication-rapportage in Azure Portal
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: cb4ef0e361cb3495a03e5a7ed49d9a84a54dbdc3
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="reference-for-multi-factor-authentication-reporting-in-the-azure-portal"></a>Naslaginformatie voor Multi-Factor Authentication-rapportage in Azure Portal

Met [Azure AD-rapporten (Azure Active Directory)](active-directory-reporting-azure-portal.md) in [Azure Portal](https://portal.azure.com) ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

De [rapporten voor aanmeldingsactiviteiten](active-directory-reporting-activity-sign-ins.md) bieden u informatie over het gebruik van beheerde toepassingen en aanmeldingsactiviteiten van gebruikers, inclusief informatie over MFS-gebruik. 

De MFA-gegevens geven u inzicht in hoe MFA werkt in uw organisatie. Hiermee kunt u vragen beantwoorden zoals: 

- Is de aanmelding gelukt met MFA? 

- Hoe heeft de gebruiker MFA voltooid? 

- Waarom kan de gebruiker MFA niet voltooien?  

Door de aanmeldingsgegevens samen te voegen, kunt u de MFA-ervaring in uw organisatie beter begrijpen. De gegevens helpen u vragen te beantwoorden zoals: 

- Hoeveel gebruikers gebruiken MFA?  

- Hoeveel gebruikers kunnen MFA niet voltooien? 

- Wat zijn de meest voorkomende MFA-problemen waarmee eindgebruikers te maken hebben? 


Deze gegevens zijn beschikbaar via Azure Portal en de [rapportage-API](active-directory-reporting-api-getting-started-azure-portal.md). 


## <a name="data-structure"></a>Gegevensstructuur


De rapporten voor aanmeldingsactiviteiten voor MFA bieden u toegang tot de volgende informatie:

**MFA is vereist:** of MFA is vereist voor aanmelding of niet. MFA kan worden vereist vanwege MFA per gebruiker, voorwaardelijke toegang of om andere redenen. Mogelijke waarden zijn `Yes` en `No`.

**MFA-verificatiemethode:** de verificatiemethode die de gebruiker heeft gebruikt om MFA te voltooien. Mogelijke waarden zijn: 

- Sms-bericht 

- Meldingen via mobiele app 

- Telefonische oproep (verificatie via telefoonnummer) 

- Verificatiecode via mobiele app 

- Telefonische oproep (telefoon op het werk) 

- Telefonische oproep (verificatie via telefoonnummer) 

**Detail van MFA-verificatie:** verwijderde versie van het telefoonnummer, bijvoorbeeld: + X XXXXXXXX64. 

**MFA-resultaat:** meer informatie over of MFA is voldaan of geweigerd:

- Als MFA is voldaan, bevat deze kolom meer informatie over hoe dit is gebeurd. 

- Als MFA is geweigerd, bevat deze kolom de reden van weigering. Mogelijke waarden zijn `Satisfied` en `Denied`. 

In de volgende sectie worden mogelijke tekenreekswaarden vermeld voor het MFA-resultatenveld.

## <a name="status-strings"></a>Statustekenreeksen

In deze sectie worden de mogelijke waarden vermeld voor de resultaten voor de MFA-statustekenreeks.

### <a name="satisfied-status-strings"></a>Voldane statustekenreeksen


- Azure Multi-Factor Authentication

    - voltooid in de cloud 

    - verlopen als gevolg van beleid dat is geconfigureerd in de tenant 

    - registratie gevraagd 

    - voldaan door de claim in het token 

    - voldaan door de claim in het token 

    - voldaan door de claim in het token 

    - voldaan door de claim in het token 

    - voldaan door de claim geleverd door de externe provider 

    - voldaan door geavanceerde verificatie 

    - overgeslagen omdat de uitgevoerde stroom een Windows broker-aanmeldingsstroom is 

    - overgeslagen omdat de uitgevoerde stroom een Windows broker-aanmeldingsstroom is 

    - overgeslagen vanwege een app-wachtwoord 

    - overgeslagen vanwege de locatie 

    - overgeslagen vanwege een geregistreerd apparaat 
    
    - overgeslagen vanwege een geregistreerd apparaat 

    - voltooid 

- Omgeleid naar externe provider voor Multi-Factor Authentication 

 
### <a name="denied-status-strings"></a>Geweigerde statustekenreeksen

Azure Multi-Factor Authentication is geweigerd 

- verificatie wordt uitgevoerd 

- dubbele verificatiepoging 

- onjuiste code te vaak ingevoerd 

- ongeldige verificatie 

- ongeldig verificatiecode via mobiele app 

- onjuiste configuratie 

- telefonische oproep verzonden naar voicemail 

- telefoonnummer heeft een ongeldige indeling 

- servicefout 

- kan telefoonnummer van gebruiker niet bereiken 

- kan de mobiele-app-melding niet verzenden naar het apparaat 

- kan de mobiele-app-melding niet verzenden 

- gebruiker heeft verificatie geweigerd 

- gebruiker heeft niet gereageerd op mobiele-app-melding 

- gebruiker heeft geen verificatiemethoden geregistreerd 

- gebruiker heeft onjuiste code ingevoerd 

- gebruiker heeft onjuiste pincode ingevoerd 

- gebruiker heeft opgehangen zonder te verifiëren 

- gebruiker is geblokkeerd 

- gebruiker heeft de verificatiecode niet ingevoerd 

- gebruiker is niet gevonden 
 
- verificatiecode is al gebruikt 



## <a name="next-steps"></a>Volgende stappen

Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor meer informatie.




























