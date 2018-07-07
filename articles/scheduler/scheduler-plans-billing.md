---
title: Abonnementen en facturering in Azure Scheduler
description: Abonnementen en facturering in Azure Scheduler
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 03f335634b7ce1fe4aa6251d6ec21922ed9b84c8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887484"
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Abonnementen en facturering in Azure Scheduler
## <a name="job-collection-plans"></a>Taak Verzamelingsplannen
Verzamelingen van de taak is een factureerbare entiteit in Azure Scheduler. Taakcollecties bevatten een aantal taken en zijn verkrijgbaar in drie plannen: Standard, Premium P10 en P20 Premium-die hieronder beschreven worden.

| **Plan voor het verzamelen van taak** | **Maximum aantal taken per Taakverzameling** | **Max. terugkeerpatroon** | **Maximum aantal Taakcollecties per abonnement** | **Limieten** |
|:--- |:--- |:--- |:--- |:--- |
| **Standard** |50 taken per taakverzameling |Eenmaal per minuut. Kan niet vaker dan eens per minuut taken worden uitgevoerd |Een abonnement mag maximaal 100 standard taakcollecties |Toegang tot de volledige functieset van Scheduler |
| **P10 Premium** |50 taken per taakverzameling |Eenmaal per minuut. Kan niet vaker dan eens per minuut taken worden uitgevoerd |Een abonnement kan maximaal 10.000 P10 Premium-taakverzamelingen is toegestaan. <a href="mailto:wapteams@microsoft.com">Neem contact met ons</a> voor meer informatie. |Toegang tot de volledige functieset van Scheduler |
| **P20 Premium** |1000 taken per taakverzameling |Eenmaal per minuut. Kan niet vaker dan eens per minuut taken worden uitgevoerd |Een abonnement kan maximaal 10.000 P20 Premium-taakverzamelingen is toegestaan. <a href="mailto:wapteams@microsoft.com">Neem contact met ons</a> voor meer informatie. |Toegang tot de volledige functieset van Scheduler |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Upgrades en Downgrades van de taak Verzamelingsplannen
U kunt upgraden en downgraden van een taak verzamelingsplan op elk gewenst moment tussen de Standard, Premium P10 en P20 Premium-abonnementen.

## <a name="billing-and-azure-plans"></a>Facturering en Azure-abonnementen
Als u meer dan 100 standard taakcollecties (10 facturering standaardeenheden) hebt, is een betere deal alle taakcollecties in de premium-abonnement hebben.

Als u een standaard jobverzameling en een premium-taakverzameling hebt, bent u worden gefactureerd één standard Factureringseenheid *en* één facturering premium-eenheid. De facturen van de Scheduler-service uit op basis van het aantal actieve taakverzameling die zijn ingesteld op standaard of premium; Dit wordt verderop in de volgende twee secties.

## <a name="standard-billable-units"></a>Standaard-factureerbare eenheden
Een standaard factureerbare eenheid kunt tot 10 standard taakcollecties opnemen. Aangezien een standaard taakverzameling maximaal 50 taken per taakverzameling hebben kan, kan één standard Factureringseenheid een abonnement hebben van maximaal 500 taken – tot bijna 22 miljoen taakuitvoeringen per maand.

Als u tussen 1 en 10 standard taakverzamelingen hebt, wordt u gefactureerd voor een standaard Factureringseenheid. Als u tussen 11 en 20 standard taakverzamelingen hebt, wordt u gefactureerd voor de twee standaard facturering-eenheden. Als u hebt tussen 21 en 30 standard taakverzamelingen, u wordt gefactureerd voor drie facturering standaardeenheden, enzovoort.

## <a name="p10-premium-billable-units"></a>P10 Premium factureerbare eenheden
Een P10 premium factureerbare eenheid kan maximaal 10.000 P10 premium-taakverzamelingen bevatten. Omdat de taakverzameling van een P10 premium maximaal 50 taken per taakverzameling hebt kan, kan een premium-Factureringseenheid een abonnement te hebben tot maximaal 500.000 taken – tot bijna 22 miljard taakuitvoeringen per maand.

Als u tussen 1 en 10.000 premium-taakverzamelingen hebt, wordt u gefactureerd voor een P10 premium Factureringseenheid. Als u hebt tussen 10,001 en 20.000 premium-taakverzamelingen, u ontvangt een factuur voor 2 P10 premium factureringseenheden, enzovoort.

Dus P10 premium-taakverzamelingen hebben dezelfde functionaliteit als de standard-taakverzamelingen, maar bieden een prijsverschil als uw toepassing veel taakcollecties vereist.

## <a name="p20-premium-billable-units"></a>P20 Factureerbare Premium-eenheden
Een factureerbaar P20 premium-eenheid kan maximaal 5.000 P20 premium-taakverzamelingen bevatten. Aangezien een P20 premium-taakverzameling maximaal 1000 taken per taakverzameling hebben kan, kan een premium-Factureringseenheid een abonnement hebben tot 5.000.000 taken – tot bijna 220 miljard taakuitvoeringen per maand.

P20 premium-taakverzamelingen bieden dezelfde mogelijkheden als P10 premium-taakverzamelingen, maar biedt ook ondersteuning voor een groter aantal taken per taakverzameling en een groter totale aantal taken algehele dan P10 premium waarmee u meer schaalbaarheid.

## <a name="billing-and-active-status"></a>Facturering en actieve Status
Taakcollecties zijn altijd actief, tenzij uw volledige abonnement is geworden in sommige tijdelijk uitgeschakeld vanwege problemen met facturering. Er is de enige manier om ervoor te zorgen dat een jobverzameling is niet in rekening gebracht voor de taakverzameling verwijderen.

Hoewel u alle taken binnen de taakverzameling van een in één bewerking uitschakelen kunt, wijzigt de status van de facturering van de taakverzameling niet – de taakverzameling wordt *nog steeds* worden kosten in rekening gebracht. Op deze manier leeg taakverzamelingen worden beschouwd als actief en wordt in rekening gebracht.

## <a name="pricing"></a>Prijzen
Zie voor prijsgegevens [Scheduler prijzen](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Zie ook
 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met behulp van Scheduler in Azure Portal](scheduler-get-started-portal.md)

 [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Hoge beschikbaarheid en betrouwbaarheid van Azure Scheduler](scheduler-high-availability-reliability.md)

 [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)

