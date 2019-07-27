---
title: Wat is Azure Australië? | Microsoft Docs
description: Richt lijnen voor het configureren van Azure in de Australische regio's om te voldoen aan de specifieke vereisten van het beleid voor de Australische overheid, de regelgeving en de wetgeving.
author: Galey801
ms.service: azure-australia
ms.topic: overview
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: a6bd4a8747ba4aa592914fc970acd756803dd15e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68570878"
---
# <a name="what-is-azure-australia"></a>Wat is Azure Australië?

In 2014 werd Azure geïntroduceerd in Australië, met twee regio's. Australië-oost (Sydney) en Australië-zuidoost (Melbourne). In april 2018 zijn twee nieuwe Azure-regio's in Canberra-Australië-centraal en Australië-centraal 2 gestart. De Australië-centraal-en Australië-centraal 2 regio's zijn bedoeld om te voldoen aan de behoeften van de overheid en kritieke nationale infra structuur en bieden gespecialiseerde connectiviteit en flexibiliteit zodat u uw systemen naast de cloud kunt vinden, met niveaus van beveiliging en tolerantie worden alleen verwacht van in het geheim geclassificeerde netwerken. Azure Australia is een platform voor de digitale trans formatie van de overheid en kritieke nationale infra structuur, en de enige essentiële Cloud die in Australië speciaal is ontworpen voor die behoeften.

Er zijn specifieke vereisten voor de Australische overheid voor het maken van verbinding met, verbruikt en werken in [Microsoft Azure Australië](https://azure.microsoft.com/global-infrastructure/australia/) voor Australische overheids gegevens en-systemen. De resources op deze pagina bieden ook algemene richt lijnen die van toepassing zijn op alle klanten met een specifieke focus op veilige configuratie en werking.

Raadpleeg de pagina Australië van de [micro soft service Trust-Portal](https://aka.ms/au-irap) voor actuele informatie over de Azure Australia Information Security geregistreerde beoordelaar (IRAP)-evaluaties, certificering en insluiting op de gecertificeerde Cloud Services lijst (CCSL). Op de pagina Australië vindt u ook andere micro soft-adviezen die specifiek zijn voor overheden en kritieke infrastructuur providers.

## <a name="principles-for-securing-customer-data-in-azure-australia"></a>Principes voor het beveiligen van klant gegevens in azure Australië

Azure Australië biedt een scala aan functies en services die u kunt gebruiken om cloud oplossingen te bouwen om te voldoen aan uw gereguleerde/beheerde gegevens behoeften. Een compatibele klant oplossing is niets meer dan de daad werkelijke implementatie van out-of-the-box Azure Australia-mogelijkheden, in combi natie met een solide procedure voor het beveiligen van gegevens.

Wanneer u een oplossing in azure Australia host, verwerkt micro soft veel van deze vereisten op het niveau van de Cloud infrastructuur.

Het volgende diagram toont het Azure-verdedigings model. Micro soft biedt bijvoorbeeld een basis DDoS voor de Cloud infrastructuur, samen met de mogelijkheden van de klant, zoals beveiligings apparaten of Premium DDoS-Services, voor klantspecifieke toepassings behoeften.

![alternatieve tekst](media/defenceindepth.png)

Deze artikelen geven een overzicht van de basis principes voor het beveiligen van uw services en toepassingen, met begeleiding en aanbevolen procedures voor het Toep assen van deze principes. Met andere woorden, hoe klanten het slimme gebruik van Azure Australië moeten maken om te voldoen aan de verplichtingen en verantwoordelijkheden die vereist zijn voor een oplossing die de overheid gevoelige en geclassificeerde informatie verwerkt.

Er zijn twee categorieën documentatie beschikbaar die door Australische overheids instanties worden gemigreerd naar Azure.

## <a name="security-in-azure-australia"></a>Beveiliging in azure Australië

Identiteit, op rollen gebaseerd toegangs beheer, gegevens beveiliging via versleuteling en Rights Management, en effectief bewakings-en configuratie beheer zijn belang rijke elementen die u moet implementeren. In deze sectie vindt u een reeks artikelen waarin de ingebouwde mogelijkheden van Azure worden uitgelegd en hoe deze zich verhouden tot de ISM en ASD essentiële 8.

Deze artikelen zijn toegankelijk via het menu onder *concepten-> beveiliging in azure Australië*.

## <a name="gateways-in-azure-australia"></a>Gateways in azure Australië

Een andere belang rijke stap voor overheids instanties is het opstellen van de beveiligings mogelijkheden van een perimeter netwerk. Deze mogelijkheden worden aangeduid als beveiligde Internet gateways (SIG) en wanneer u Azure gebruikt, is het uw verantwoordelijkheid om ervoor te zorgen dat deze beveiligingen aanwezig zijn. Micro soft werkt niet met een SIG; door onze Edge-netwerk services te combi neren die alle klanten beveiligen, en specifieke services die binnen uw Azure-omgeving zijn geïmplementeerd, kunt u echter een vergelijk bare mogelijkheid uitoefenen.

Deze artikelen zijn toegankelijk via het menu onder *concepten-> gateways in azure Australië*.

## <a name="next-steps"></a>Volgende stappen

* Als uw sleutel focus uw gegevens in azure beveiligt, kunt u beginnen met [gegevens beveiliging](secure-your-data.md)
* Als uw hoofd focus een gateway in azure bouwt, begint u met het controleren van de [gateway, logboek registratie en zicht baarheid](gateway-log-audit-visibility.md).
