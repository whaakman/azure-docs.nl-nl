---
title: -Microsoft Threat Modeling hulpprogramma - Azure bedreigingen | Microsoft Docs
description: "Threat categoriepagina voor het hulpprogramma Microsoft Threat Modeling met categorieën voor alle blootgesteld bedreigingen gegenereerd."
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
ms.openlocfilehash: 704f9995828866d4d2e4969e3aa922ed1e23c4ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Microsoft Threat Modeling Tool bedreigingen

Het hulpprogramma voor het modelleren van dreiging is een hoofdelement van de Microsoft Security Development Lifecycle (SDL). Kunt u softwarearchitecten te identificeren en beperken van mogelijke beveiligingsproblemen vroeg, wanneer ze zijn relatief eenvoudige en voordelige om op te lossen. Als gevolg hiervan vermindert aanzienlijk de totale kosten van ontwikkeling. Ook wordt het hulpprogramma ontworpen met experts van de niet voor beveiliging in acht nemen, waardoor risicomodel gemakkelijker voor alle ontwikkelaars doordat duidelijke richtlijn voor het maken en analyseren van threat modellen.

> Ga naar de  **[Threat Modeling hulpprogramma](./azure-security-threat-modeling-tool.md)**  vandaag aan de slag!

Het hulpprogramma Threat Modeling helpt u bepaalde zoals de onderstaande vragen beantwoorden:

* Hoe kan een aanvaller de verificatiegegevens wijzigen?
* Wat zijn de gevolgen als een aanvaller de gebruikersprofielgegevens kunt lezen?
* Wat gebeurt er als de toegang is geweigerd tot de database met gebruikersprofielen?

## <a name="stride-model"></a>STRIDE model

Betere Help u dit soort verwijzen vragen formuleren, Microsoft gebruikt de STRIDE-model, dat verschillende soorten bedreigingen ingedeeld en de algehele beveiliging conversaties vereenvoudigt.

| Category | Beschrijving |
| -------- | ----------- |
| **Adresvervalsing (spoofing)** | Omvat illegale wijze toegang tot en met behulp van de verificatiegegevens van een andere gebruiker, zoals gebruikersnaam en wachtwoord |
| **Manipulatie** | Omvat de schadelijke wijzigingen aan gegevens. Voorbeelden zijn onder meer niet-geautoriseerde wijzigingen aangebracht in de permanente gegevens, zoals in een database en de afwijking van gegevens, ondergebracht zoals wordt gebruikt tussen twee computers via een open netwerk, zoals het Internet |
| **Repudiation** | Die zijn gekoppeld aan gebruikers die een actie uitvoert zonder andere partijen die van een manier om te bewijzen anders weigeren — bijvoorbeeld een gebruiker een ongeldige bewerking in een systeem waarmee u beschikt niet over de mogelijkheid om te traceren, de niet-toegestane bewerkingen uitvoert. Niet-afwijzing verwijst naar de mogelijkheid van een systeem op teller repudiation bedreigingen. Bijvoorbeeld, wellicht een gebruiker die een artikel koopt aan te melden voor het item bij ontvangst. De leverancier kan vervolgens de ondertekende ontvangst worden gebruikt als bewijs dat de gebruiker het pakket heeft ontvangen |
| **Vrijgeven van informatie** | Omvat de blootstelling van gegevens aan personen die niet moeten toegang hebben tot het — bijvoorbeeld de mogelijkheid van gebruikers om te lezen van een bestand dat ze zijn geen toegang tot of de mogelijkheid van een indringer het lezen van gegevens tijdens de overdracht tussen twee computers |
| **Denial of Service** | DOS-aanvallen (DoS) service weigeren aan geldige gebruikers, bijvoorbeeld door het maken van een webserver tijdelijk niet beschikbaar of onbruikbaar. U moet beschermen tegen een bepaalde soorten bedreigingen DoS gewoon systeembeschikbaarheid en betrouwbaarheid te verbeteren |
| **Misbruik van bevoegdheden** | Een onbevoegde gebruiker bevoorrechte toegang te krijgen en waardoor voldoende toegangsrechten heeft voor beschadigen of vernietigen van het gehele systeem. Onrechtmatige uitbreiding van bevoegdheden bedreigingen situaties waarbij een aanvaller effectief alle systeem-beveiliging heeft gepenetreerd opnemen en deel uitmaken van het vertrouwde systeem zelf, een gevaarlijke situatie inderdaad |

## <a name="next-steps"></a>Volgende stappen

Ga door naar  **[Threat Modeling hulpprogramma oplossingen](./azure-security-threat-modeling-tool-mitigations.md)**  voor meer informatie over de verschillende manieren waarop u deze bedreigingen met Azure kunt verhelpen.