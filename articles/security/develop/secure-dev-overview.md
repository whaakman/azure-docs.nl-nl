---
title: Secure development best practices voor Microsoft Azure
description: Aanbevolen procedures voor het ontwikkelen van veiligere code en een beter beveiligde toepassing in de cloud implementeren.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/11/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: ee5c043038fbb747e90bca9530cbe2e94c8a95c2
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144462"
---
# <a name="secure-development-best-practices-on-azure"></a>Secure development best practices voor Azure
Deze reeks artikelen geeft beveiligingsactiviteiten en besturingselementen om te overwegen bij het ontwikkelen van toepassingen voor de cloud. De fasen van de vragen over Microsoft Security Development Lifecycle (SDL) en beveiliging en concepten te houden tijdens elke fase van de levenscyclus worden besproken. Het doel is voor het definiëren van activiteiten en Azure-services die u in elke fase van de levenscyclus gebruiken kunt te ontwerpen, ontwikkelen en implementeren van een beter beveiligde toepassing.

De aanbevelingen in de artikelen zijn afkomstig uit onze ervaring met Azure-beveiliging en de ervaringen van onze klanten. U kunt deze artikelen gebruiken als uitgangspunt voor wat u tijdens een specifieke fase van uw project voor de ontwikkeling overwegen moet, maar het is raadzaam dat u ook via alle artikelen lezen van begin tot eind ten minste één keer. Lezen van alle artikelen, maakt u kennis met concepten die u mogelijk hebt gemist in eerdere fasen van het project. Implementatie van deze concepten voordat u uw product een release kunt u bouwen van veilige software, adres nalevingsvereisten voor beveiliging en ontwikkelingskosten te verlagen.

Deze artikelen zijn bedoeld om te worden van een resource voor softwareontwerpers van, ontwikkelaars en testers op alle niveaus die bouwen en implementeren van veilige Azure-toepassingen.

## <a name="overview"></a>Overzicht

Security is een van de belangrijkste aspecten van elke toepassing en het is niet heel eenvoudig meteen. Gelukkig biedt Azure veel services die u kunnen helpen uw toepassing in de cloud beveiligen. Deze artikelen adres activiteiten en Azure-services kunt u implementeren in elke fase van de levenscyclus van uw software-ontwikkeling voor het ontwikkelen van veiligere code en een beter beveiligde toepassing in de cloud implementeren.

## <a name="security-development-lifecycle"></a>Security development lifecycle

Aanbevolen procedures te volgen voor ontwikkeling van veilige software vereist integreren beveiliging in elke fase van de software development lifecycle, van een analyse van de vereiste onderhoud, ongeacht de methodologie van het project ([waterval](https://en.wikipedia.org/wiki/Waterfall_model), [flexibele](https://en.wikipedia.org/wiki/Agile_software_development), of [DevOps](https://en.wikipedia.org/wiki/DevOps)). Meer ontwikkelaars zijn in het kielzog van schendingen van belangrijke gegevens en de exploitatie van operationele beveiligingsfouten begrijpen dat beveiliging moet worden behandeld in het ontwikkelingsproces inbouwt.

Hoe hoger het oplossen van problemen in uw ontwikkelcyclus, hoe meer die zal verhelpen u de kosten. Beveiligingsproblemen met zich mee zijn geen uitzondering. Als u beveiligingsproblemen met zich mee in de eerste fasen van de softwareontwikkeling van uw negeren, kan elke fase die volgt op de kwetsbaarheid van de vorige fase overnemen. Uw uiteindelijke product wordt hebben verzameld, meerdere beveiligingsproblemen met zich mee en de mogelijkheid van een schending. Het inbouwen van beveiliging in elke fase van de ontwikkelingscyclus helpt u bij problemen vroegtijdig variabel en Hiermee kunt u uw ontwikkeling te verlagen.

Volgen we de fasen van de Microsoft [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) om activiteiten en Azure-services die u gebruiken kunt om te voldoen aan beveiligde softwareontwikkelingsprocedures in elke fase van de levenscyclus te introduceren.

De SDL-fasen zijn:

  - Training
  - Vereisten
  - Ontwerp
  - Implementatie
  - Verificatie
  - Release
  - Antwoord

![Security Development Lifecycle](./media/secure-dev-overview/01-sdl-phase.png)

In deze artikelen die we de SDL-fasen te in ontwerpen groeperen, ontwikkelen en implementeren.

## <a name="engage-your-organizations-security-team"></a>Uw organisatie beveiligingsteam betrekken

Uw organisatie mogelijk heeft een formele aanvraag beveiligingsprogramma die u met beveiligingsactiviteiten van begin helpt tot eind tijdens de ontwikkelfase. Als uw organisatie heeft de beveiliging en naleving van teams, moet u contact opnemen met deze voordat u begint met het ontwikkelen van uw toepassing. Hierover in elke fase van de SDL of er nog andere taken die u hebt gemist.

We begrijpen dat veel lezers een beveiligings- of -team contact opnemen met niet mogelijk. Deze artikelen kunnen u te helpen in de beveiligingsvragen en de beslissingen die u moet rekening houden met in elke fase van de SDL.

## <a name="resources"></a>Resources

Gebruik de volgende bronnen voor meer informatie over het ontwikkelen van beveiligde toepassingen en om te helpen beveiligen van uw toepassingen op Azure:

[Microsoft Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) – de SDL is een softwareontwikkelingsproces van Microsoft waarmee ontwikkelaars kunnen het bouwen van beter beveiligde software. Hiermee kunt u adres nalevingsvereisten voor beveiliging bij het terugdringen van de ontwikkeling.

[Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page) – OWASP is een onlinecommunity die verkrijgbare artikelen, methodologieën, documentatie, hulpprogramma's en technologieën op het gebied van beveiliging van webtoepassingen produceert.

[Pushen naar links, zoals een baas](https://code.likeagirl.io/pushing-left-like-a-boss-part-1-80f1f007da95?WT.mc_id=docs-blog-tajanca) : een reeks online artikelen die geeft een overzicht van verschillende typen toepassing beveiligingsactiviteiten die ontwikkelaars voor het maken van veiligere code moeten worden voltooid.

[Microsoft identity-platform](https://docs.microsoft.com/azure/active-directory/develop/) – het Microsoft identity-platform is een verbetering van de Azure AD identity-service en developer-platform. Het is een volledig functionele-platform, dat uit een authentication-service, open source-bibliotheken, registratie van toepassingen en configuratie, volledige ontwikkelaarsdocumentatie, codevoorbeelden en andere inhoud voor ontwikkelaars bestaat. Het Microsoft identity-platform biedt ondersteuning voor standaardprotocollen zoals OAuth 2.0 en OpenID Connect.

[Aanbevolen beveiligingsprocedures voor Azure-oplossingen](https://azure.microsoft.com/resources/security-best-practices-for-azure-solutions/) : een verzameling van aanbevolen procedures voor beveiliging moet worden gebruikt bij het ontwerpen, implementeren en beheren van oplossingen voor cloudopslag met behulp van Azure. Dit document is bedoeld als een bron voor IT-professionals. Dit kan dus ook ontwerpers, architecten, ontwikkelaars en testers die bouwen en implementeren van veilige Azure-oplossingen.

[Beveiliging en naleving blauwdrukken op Azure](https://servicetrust.microsoft.com/ViewPage/BlueprintOverview) – Azure-beveiliging en naleving blauwdrukken zijn resources waarmee u kunt bouwen en toepassingen met behulp van de cloud die aan de strenge regelgeving en standaarden voldoen starten.

## <a name="next-steps"></a>Volgende stappen
In de volgende artikelen, is het raadzaam beveiligingsmaatregelen en activiteiten waarmee u kunnen ontwerpen, ontwikkelen en implementeren van veilige toepassingen.

- [Beveiligde toepassingen ontwerpen](secure-design.md)
- [Beveiligde toepassingen ontwikkelen](secure-develop.md)
- [Beveiligde toepassingen implementeren](secure-deploy.md)
