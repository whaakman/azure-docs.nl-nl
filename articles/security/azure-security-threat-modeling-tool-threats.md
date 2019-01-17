---
title: Bedreigingen - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: Threat categoriepagina voor de Microsoft Threat Modeling Tool met categorieën voor alle blootgesteld bedreigingen gegenereerd.
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: fd7c5fd929163dc7fcd22fbb045dee0fe3070394
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359031"
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Microsoft Threat Modeling Tool bedreigingen

De Threat Modeling Tool is een core-element van de Microsoft Security Development Lifecycle (SDL). Hiermee kunt de software-architecten kunt herkennen en mogelijke beveiligingsproblemen vroeg, verminderen wanneer ze zijn relatief eenvoudig en kosteneffectief worden opgelost. Als gevolg hiervan vermindert aanzienlijk de totale kosten voor ontwikkeling. We ook ontworpen het hulpprogramma met experts van de niet-beveiligde Denk eraan dat u gemakkelijker risicomodel voor alle ontwikkelaars door het bieden van duidelijke richtlijnen over het maken en analyseren van bedreigingsmodellen.

> Ga naar de **[Threat Modeling Tool](./azure-security-threat-modeling-tool.md)** om vandaag nog aan de slag!

De Threat Modeling Tool kunt u bepaalde beantwoorden, zoals de volgende oplossingen:

* Hoe kan een aanvaller de gegevens van de verificatie wijzigen?
* Wat zijn de gevolgen als een aanvaller de gebruikersprofielgegevens kunt lezen?
* Wat gebeurt er als de toegang is geweigerd tot de database met gebruikersprofielen?

## <a name="stride-model"></a>STRIDE model

Betere Help u dit soort verwezen vragen formuleren, Microsoft maakt gebruik van de STRIDE-model, die verschillende soorten bedreigingen ingedeeld en vereenvoudigt de algehele beveiliging-conversaties.

| Categorie | Description |
| -------- | ----------- |
| **Adresvervalsing (spoofing)** | Omvat het illegale wijze toegang tot en met behulp van de verificatiegegevens van een andere gebruiker, zoals gebruikersnaam en wachtwoord |
| **Tampering** | Omvat de schadelijke wijzigen van gegevens. Voorbeelden zijn onder meer niet-geautoriseerde wijzigingen aangebracht in uw permanente gegevens, zoals die in een database en de wijziging van de gegevens bewaard als deze tussen de twee computers via een open-netwerk, zoals het Internet stromen |
| **Repudiation** | Dat is gekoppeld aan gebruikers die een actie wordt uitgevoerd zonder andere partijen die een manier om te bewijzen dat anders weigeren, bijvoorbeeld een gebruiker een ongeldige bewerking uitvoert in een systeem dat u beschikt niet over de mogelijkheid om de niet-toegestane bewerkingen traceren. Niet-afwijzing verwijst naar de mogelijkheid van een systeem op teller ontkenning bedreigingen. Bijvoorbeeld, een gebruiker die een item aankopen mogelijk aan te melden voor het item bij ontvangst. De leverancier kan vervolgens de ondertekende ontvangst worden gebruikt als bewijs dat de gebruiker het pakket is ontvangen |
| **Openbaarmaking van informatie** | De blootstelling van personen die niet moeten toegang hebben tot deze informatie omvat — bijvoorbeeld, de mogelijkheid van gebruikers lezen van een bestand dat ze geen toegang tot zijn verleend, of de mogelijkheid van een indringer gebruik om te lezen van gegevens in transit tussen twee computers |
| **Denial of Service** | Denial of service (DoS) aanval service tot geldige gebruikers weigeren, bijvoorbeeld door een webserver tijdelijk niet beschikbaar of onbruikbaar. U moet zich beschermen tegen bepaalde typen DoS-dreigingen gewoon voor betere beschikbaarheid van het systeem en betrouwbaarheid |
| **Misbruik van bevoegdheden** | Een niet-gemachtigde gebruiker bevoegde toegang te krijgen en daardoor heeft voldoende toegangsrechten voor beschadigen of vernietigen van het hele systeem. Misbruik van bevoegdheden bedreigingen situaties waarbij een aanvaller effectief alle systeem-beveiliging heeft gepenetreerd opnemen en deel uitmaken van het vertrouwde systeem zelf, inderdaad een gevaarlijke situaties |

## <a name="next-steps"></a>Volgende stappen

Gaat u verder met **[Threat Modeling Tool oplossingen](./azure-security-threat-modeling-tool-mitigations.md)** voor meer informatie over de verschillende manieren waarop u deze bedreigingen met Azure kunt beperken.