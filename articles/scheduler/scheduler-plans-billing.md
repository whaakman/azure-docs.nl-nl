---
title: Abonnementen en facturering - Azure Scheduler
description: Meer informatie over abonnementen en facturering voor Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 74f13628b62d53a84b4d19255a30a6bc4a7367ec
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974286"
---
# <a name="plans-and-billing-for-azure-scheduler"></a>Abonnementen en facturering voor Azure Scheduler

> [!IMPORTANT]
> [Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, is buiten gebruik gesteld. Voor het plannen van taken, [Azure Logic Apps in plaats daarvan probeert](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

## <a name="job-collection-plans"></a>Taak verzamelingsplannen

Een jobverzameling bevat in Azure Scheduler, een bepaald aantal taken. De taakverzameling is de factureerbare entiteit en is beschikbaar in standaard, P10 Premium of P20 Premium-abonnementen, die hier worden beschreven: 

| Plan voor het verzamelen van taak | Max. aantal taken per verzameling | Max. terugkeerpatroon | Maximum aantal taakcollecties per abonnement | Limieten | 
|:--- |:--- |:--- |:--- |:--- |
| **Standard** | 50 taken per verzameling | Een per minuut. Taken kan niet vaker dan één per minuut worden uitgevoerd. | Elk Azure-abonnement kan maximaal 100 Standard taakverzamelingen hebben. | Toegang tot de volledige functieset Scheduler | 
| **P10 Premium** | 50 taken per verzameling | Een per minuut. Taken kan niet vaker dan één per minuut worden uitgevoerd. | Elk Azure-abonnement kan maximaal 10.000 P10 Premium-taakverzamelingen hebben. Voor meer verzamelingen <a href="mailto:wapteams@microsoft.com">contact met ons opnemen</a>. | Toegang tot de volledige functieset Scheduler |
| **P20 Premium** | 1000 taken per verzameling | Een per minuut. Taken kan niet vaker dan één per minuut worden uitgevoerd. | Elk Azure-abonnement kan maximaal 5.000 P20 Premium-taakverzamelingen hebben. Voor meer verzamelingen <a href="mailto:wapteams@microsoft.com">contact met ons opnemen</a>. | Toegang tot de volledige functieset Scheduler |
|||||| 

## <a name="pricing"></a>Prijzen

Zie voor prijsgegevens [Scheduler prijzen](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="upgrade-or-downgrade-plans"></a>Upgraden en downgraden plannen

Op elk gewenst moment kunt u upgraden en downgraden van een plan voor het verzamelen van een taak in de standaard, P10 Premium of P20 Premium-abonnementen.

## <a name="active-status-and-billing"></a>Status van de actieve en facturering

Taakcollecties zijn altijd actief, tenzij uw gehele Azure-abonnement in een tijdelijk uitgeschakeld vanwege problemen met de facturering gaat. En hoewel alle taken in een taakverzameling via één bewerking, u uitschakelen kunt, deze actie facturering status van de taakverzameling niet wijzigen, zodat de taakverzameling is *nog steeds* kosten in rekening gebracht. Lege taakverzamelingen worden beschouwd als actief en worden in rekening gebracht.

Als u wilt controleren of dat een jobverzameling is niet in rekening gebracht, moet u de taakverzameling verwijderen.

## <a name="standard-billable-units"></a>Standaard-factureerbare eenheden

Een standaard factureerbare eenheid kan tot 10 Standard taakverzamelingen hebben. Omdat een standaard taakverzameling maximaal 50 taken per verzameling hebben kan, een standaard Factureringseenheid kunt uw Azure-abonnement hebt op 500 taken of tot bijna 22 *miljoen* taakuitvoeringen per maand. Deze lijst wordt uitgelegd hoe u wordt gefactureerd op basis van verschillende aantallen Standard taakverzamelingen:

* Als u tussen 1 en 10 Standard taakverzamelingen hebt, kunt u wordt gefactureerd voor een standaard Factureringseenheid. 

* Als u tussen 11 en 20 Standard taakverzamelingen hebt, kunt u wordt gefactureerd voor twee standaard facturering-eenheden. 

* Als u hebt tussen 21 en 30 Standard taakverzamelingen, u wordt gefactureerd voor drie eenheden van standard facturering, enzovoort.

## <a name="p10-premium-billable-units"></a>P10 premium factureerbare eenheden

Een P10 premium factureerbare eenheid kan maximaal 10.000 P10 Premium-taakverzamelingen hebben. Aangezien een taakverzameling P10 Premium maximaal 50 taken per verzameling hebben kan, een P10 premium Factureringseenheid kunt uw Azure-abonnement hebt op 500.000 taken of tot bijna 22 *miljard* taakuitvoeringen per maand. 

P10 Premium-taakverzamelingen bieden dezelfde mogelijkheden als standaard-taakverzamelingen, maar bieden een pauze van de prijs voor apps die veel taakverzamelingen vereisen en bieden meer schaalbaarheid. Deze lijst wordt uitgelegd hoe u wordt gefactureerd op basis van verschillende aantallen P10 Premium-taakverzamelingen:

* Als u tussen 1 en 10.000 P10 Premium-taakverzamelingen hebt, kunt u wordt gefactureerd voor een P10 premium Factureringseenheid. 

* Als u hebt tussen 10,001 en 20.000 P10 Premium-taakverzamelingen, u wordt in rekening gebracht voor 2 P10 premium factureringseenheden, enzovoort.

## <a name="p20-premium-billable-units"></a>Factureerbare P20 premium-eenheden

Een factureerbaar P20 premium-eenheid kan maximaal 5.000 P20 Premium-taakverzamelingen hebben. Omdat een taakverzameling P20 Premium maximaal 1000 taken per taakverzameling hebben kan, een P20 premium-Factureringseenheid kunt uw Azure-abonnement hebt op 5.000.000 taken of tot bijna 220 *miljard* taakuitvoeringen per maand.

P20 Premium-taakverzamelingen bieden dezelfde mogelijkheden als P10 Premium-taakverzamelingen, maar bieden ook ondersteuning voor een groter aantal taken per verzameling en een groter totale aantal taken algehele dan P10 Premium, bieden meer schaalbaarheid.

## <a name="plan-comparison"></a>Vergelijking van abonnementen

* Als u meer dan 100 Standard taakcollecties (10 facturering standaardeenheden) hebt, kunt u een betere deal ophalen door alle taakverzamelingen in een Premium-abonnement.

* Als u een Standard jobverzameling en een Premium-taakverzameling hebt, wordt in voor een standaard Factureringseenheid gebracht rekening *en* één facturering premium-eenheid.

  De facturen van de Scheduler-service is op basis van het aantal actieve taakverzamelingen die standard- of premium zijn.

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)
* [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)