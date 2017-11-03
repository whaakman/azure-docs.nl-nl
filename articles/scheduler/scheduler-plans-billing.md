---
title: Plannen en facturering in Azure Scheduler
description: Plannen en facturering in Azure Scheduler
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: f0662230c5d1663e37ee2be58f234934ec3d55dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Plannen en facturering in Azure Scheduler
## <a name="job-collection-plans"></a>Taak Verzamelingsplannen
Taakverzamelingen zijn de factureerbare entiteit in Azure Scheduler. Taakverzamelingen bevatten een aantal taken en komen in drie plannen – vrije, Standard en Premium-die hieronder worden beschreven.

| **Taakverzamelingsabonnement** | **Maximum aantal taken per Taakverzameling** | **Maximale terugkeerpatroon** | **Maximum aantal Taakverzamelingen per abonnement** | **Limieten** |
|:--- |:--- |:--- |:--- |:--- |
| **Gratis** |5 taken per taakverzameling |Eenmaal per uur. Kan niet vaker dan eens per uur taken worden uitgevoerd |Een abonnement mag maximaal 1 gratis taakverzameling |Kan niet worden gebruikt [HTTP uitgaande autorisatie-object](scheduler-outbound-authentication.md) |
| **Standard** |50 taken per taakverzameling |Eenmaal per minuut. Kan niet vaker dan eens per minuut taken worden uitgevoerd |Een abonnement mag maximaal 100 standaard taakverzamelingen |Toegang tot de volledige functieset van Scheduler |
| **P10 Premium** |50 taken per taakverzameling |Eenmaal per minuut. Kan niet vaker dan eens per minuut taken worden uitgevoerd |Een abonnement mag maximaal 10.000 P10 Premium taakverzamelingen. <a href="mailto:wapteams@microsoft.com">Neem contact met ons</a> voor meer informatie. |Toegang tot de volledige functieset van Scheduler |
| **P20 Premium** |1000 taken per taakverzameling |Eenmaal per minuut. Kan niet vaker dan eens per minuut taken worden uitgevoerd |Een abonnement mag maximaal 10.000 P20 Premium taakverzamelingen. <a href="mailto:wapteams@microsoft.com">Neem contact met ons</a> voor meer informatie. |Toegang tot de volledige functieset van Scheduler |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Upgrades en Downgrades van de taak Verzamelingsplannen
U kunt upgraden of downgraden van een taakverzamelingsabonnement op elk gewenst moment tussen de vrije, Standard en Premium-abonnementen. Wanneer downgraden naar een gratis taakverzameling, kan echter de downgrade voor een van de volgende redenen mislukken:

* Er bestaat al een gratis taakverzameling in het abonnement
* Een taak in de taakverzameling heeft een hogere terugkeer dan is toegestaan voor jobs in gratis jobverzamelingen. De maximale terugkeerpatroon dat is toegestaan in een gratis taakverzameling is één keer per uur
* Er zijn meer dan 5 taken in de taakverzameling
* Een taak in de taakverzameling heeft een HTTP of HTTPS-actie die gebruikmaakt van een [HTTP uitgaande autorisatie-object](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Facturering en Azure-abonnementen
Abonnementen zijn niet in rekening gebracht gratis taakverzamelingen. Als u meer dan 100 standaard taakverzamelingen (10 facturering standaardeenheden) hebt, is een betere koop alle taakverzamelingen hebben in de premium-abonnement.

Als u een standaard jobverzameling en verzameling van de taak één premium hebt, bent u gefactureerd één standaard facturering eenheid *en* één facturering premium-eenheid. De Scheduler-service van rekeningen op basis van het aantal actieve taken verzamelingen die zijn ingesteld op standaard of premium; Dit wordt verderop in de volgende twee secties.

## <a name="standard-billable-units"></a>Factureerbare standaardeenheden
Een factureerbaar standaardeenheid kan maximaal 10 standaard taakverzamelingen bevatten. Aangezien een standaard jobverzameling maximaal 50 taken per taakverzameling hebben kan, kan één facturering standaardeenheid een abonnement op maximaal 500 taken – tot bijna 22 miljoen taak uitvoeringen per maand.

Als u tussen 1 en 10 standaard taakverzamelingen hebt, zult u gefactureerd voor 1 standard facturering-eenheid. Als u tussen 11 en 20 standaard taakverzamelingen hebt, zult u gefactureerd voor 2 standaardeenheden facturering. Als u hebt tussen 21 en 30 standaard taakverzamelingen, u wordt gefactureerd voor 3 facturering standaardeenheden, enzovoort.

## <a name="p10-premium-billable-units"></a>P10 Premium factureerbare eenheden
Een factureerbaar P10 premium-eenheid kan maximaal 10.000 P10 premium taakverzamelingen bevatten. Aangezien een verzameling P10 premium taak maximaal 50 taken per taakverzameling hebben kan, kan één facturering premium-eenheid een abonnement te hebben tot 500.000 taken – tot bijna 22 miljard taak uitvoeringen per maand.

Als u tussen 1 en 10.000 premium taakverzamelingen hebt, hebt u gefactureerd voor 1 P10 premium facturering eenheid. Als u hebt tussen 10,001 en jobverzamelingen 20.000 premium, u wordt gefactureerd voor 2 P10 premium facturering eenheden, enzovoort.

Dus P10 premium taakverzamelingen hebben dezelfde functionaliteit als de standaard taakverzamelingen maar bieden een einde prijs als uw toepassing een groot aantal taakverzamelingen vereist.

## <a name="p20-premium-billable-units"></a>P20 Premium factureerbare eenheden
Een factureerbaar P20 premium-eenheid kan maximaal 5.000 P20 premium taakverzamelingen bevatten. Aangezien een verzameling P20 premium taak maximaal 1000 taken per taakverzameling hebben kan, kan één facturering premium-eenheid een abonnement te hebben tot 5,000,000 taken – tot bijna 220 miljard taak uitvoeringen per maand.

P20 premium taakverzamelingen biedt dezelfde mogelijkheden als P10 premium taakverzamelingen maar ondersteunt ook een groter aantal taken per taakverzameling en een groter aantal taken algehele dan P10 premium waarmee u meer schaalbaarheid.

## <a name="billing-and-active-status"></a>Facturering en actieve Status
Taakverzamelingen zijn altijd actief tenzij uw volledige abonnement is overgegaan op een aantal tijdelijke uitgeschakelde status bereikt vanwege problemen facturering. De enige manier om ervoor te zorgen dat een jobverzameling wordt niet in rekening gebracht is ofwel instellen op de *vrije* plan of te verwijderen van de taakverzameling.

Hoewel u alle taken binnen een jobverzameling in één bewerking uitschakelt mogelijk, worden de facturering status van de taakverzameling niet gewijzigd – de taakverzameling wordt *nog steeds* in rekening gebracht. Op deze manier leeg taakverzamelingen worden beschouwd als actief en wordt gefactureerd.

## <a name="pricing"></a>Prijzen
Raadpleeg voor prijsinformatie, [Scheduler prijzen](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Zie ook
 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met behulp van Scheduler in Azure Portal](scheduler-get-started-portal.md)

 [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Hoge beschikbaarheid en betrouwbaarheid van Azure Scheduler](scheduler-high-availability-reliability.md)

 [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)

