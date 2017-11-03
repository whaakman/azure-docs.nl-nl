---
title: Azure - hulpmiddel voor het modelleren van dreiging van Microsoft - oplossingen | Microsoft Docs
description: De pagina oplossingen voor het Microsoft Threat Modeling hulpprogramma mogelijke oplossingen voor de meest blootgestelde markeren gegenereerd bedreigingen.
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 07ef1fd3d81d795c9164741d22b5a689f86bd720
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Microsoft Threat Modeling Tool oplossingen

Het hulpprogramma voor het modelleren van dreiging is een hoofdelement van de Microsoft Security Development Lifecycle (SDL). Kunt u softwarearchitecten te identificeren en beperken van mogelijke beveiligingsproblemen vroeg, wanneer ze zijn relatief eenvoudige en voordelige om op te lossen. Als gevolg hiervan vermindert aanzienlijk de totale kosten van ontwikkeling. Ook wordt het hulpprogramma ontworpen met experts van de niet voor beveiliging in acht nemen, waardoor risicomodel gemakkelijker voor alle ontwikkelaars doordat duidelijke richtlijn voor het maken en analyseren van threat modellen.

Ga naar de  **[Threat Modeling hulpprogramma](./azure-security-threat-modeling-tool.md)**  vandaag aan de slag!

## <a name="mitigation-categories"></a>Risicobeperking categorieën

De oplossingen Threat Modeling hulpprogramma zijn ingedeeld volgens de Web Application Security Frame uit het volgende bestaat:

| Category | Beschrijving |
| -------- | ----------- |
| **[Controle en logboekregistratie](./azure-security-threat-modeling-tool-auditing-and-logging.md)** | Wie en wanneer is? Controle en logboekregistratie verwijzen naar hoe uw toepassing beveiligingsgebeurtenissen registreert |
| **[Verificatie](./azure-security-threat-modeling-tool-authentication.md)** | Wie ben jij? Verificatie is het proces waarbij een entiteit de identiteit van een andere entiteit, doorgaans via referenties, zoals een gebruikersnaam en wachtwoord bewijst |
| **[Autorisatie](./azure-security-threat-modeling-tool-authorization.md)** | Wat kunt u doen? Autorisatie is hoe uw toepassing zorgt voor toegangsbeheer voor bronnen en bewerkingen |
| **[Beveiligde communicatie](./azure-security-threat-modeling-tool-communication-security.md)** | Wie zijn u praten met Beveiligde communicatie zorgt ervoor dat alle communicatie gedaan zo veilig mogelijk is |
| **[Configuratiebeheer](./azure-security-threat-modeling-tool-configuration-management.md)** | Wie uw toepassing uitvoeren als? Welke databases maakt deze verbinding? Hoe wordt uw toepassing beheerd? Hoe worden deze instellingen beveiligd? Configuratiebeheer verwijst naar hoe deze operationele problemen in uw toepassing worden verwerkt |
| **[Cryptografie](./azure-security-threat-modeling-tool-cryptography.md)** | Hoe houdt u geheimen (vertrouwelijkheid)? Hoe weet u draagbare taalprogramma-bibliotheken (integriteit) of uw gegevens? Hoe biedt u zaden voor willekeurige waarden dat cryptografisch sterke zijn moeten? Cryptografie verwijst naar hoe uw toepassing worden afgedwongen vertrouwelijkheid en integriteit. |
| **[Uitzonderingsbeheer](./azure-security-threat-modeling-tool-exception-management.md)** | Wanneer een methodeaanroep in uw toepassing mislukt, wat is uw toepassing? Hoeveel onthullen u? Kan u beschrijvende informatie voor eindgebruikers retourneren? U waardevolle uitzonderingsgegevens doorgeven aan de aanroepfunctie Uw toepassing probleemloos mislukt? |
| **[Validatie voor invoer](./azure-security-threat-modeling-tool-input-validation.md)** | Hoe weet u dat de invoer die uw toepassing krijgt geldig en veilige is? Validatie voor invoer verwijst naar hoe uw toepassing filtert struikvegetaties of invoer voorafgaand aan de extra verwerking wordt geweigerd. Rekening houden met beperkingen invoer via toegangspunten en uitvoer via punten afsluiten codering. Vertrouwt u gegevens uit andere bronnen zoals databases en bestandsshares? |
| **[Gevoelige gegevens](./azure-security-threat-modeling-tool-sensitive-data.md)** | Hoe wordt gevoelige gegevens in uw toepassing verwerkt? Gevoelige gegevens verwijst naar hoe uw toepassing alle gegevens die moet worden beveiligd in het geheugen, via het netwerk of in permanente winkels verwerkt |
| **[Sessiebeheer](./azure-security-threat-modeling-tool-session-management.md)** | Hoe uw toepassing verwerken en gebruikerssessies beveiligen? Een sessie verwijst naar een reeks van verwante interacties tussen een gebruiker en uw webtoepassing |

Dit helpt u identificeren:

* Waar zijn de meest voorkomende fouten beschreven gemaakt
* Waar zijn de meest bruikbare verbeteringen

Als gevolg hiervan gebruiken deze categorieën kunnen richten en prioriteiten beveiliging, zodat als u weet dat de meest gangbare beveiligingsproblemen optreden in de validatie, verificatie en autorisatie-categorieën, u er kunt starten. Voor meer informatie gaat u naar  **[deze patent koppeling](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Volgende stappen

Ga naar  **[Threat Modeling hulpprogramma bedreigingen](./azure-security-threat-modeling-tool-threats.md)**  voor meer informatie over de bedreiging categorieën maakt gebruik van het hulpprogramma voor het genereren van ontwerp mogelijke bedreigingen.