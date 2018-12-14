---
title: Slimme groepen
description: Slimme groepen worden aggregaties van waarschuwingen die u helpen verminderen waarschuwingsruis
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 960dc8f2908a20620a84113b27e474b553cd45d4
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346094"
---
# <a name="smart-groups"></a>Slimme groepen
Een algemene vraag met het afhandelen van waarschuwingen is het doorspitten van de ruis om erachter te komen wat werkelijk belangrijk is: slimme groepen zijn bedoeld om te worden van de oplossing voor dit probleem.  

Slimme groepen worden automatisch gemaakt met behulp van machine learning-algoritmen combineren van gerelateerde waarschuwingen die staan voor één probleem.  Wanneer een waarschuwing wordt gemaakt, wordt deze door de algoritme toegevoegd aan een nieuwe slimme groep of een bestaande slimme groep op basis van gegevens, zoals historische patronen, vergelijkbare eigenschappen en vergelijkbare structuur. Bijvoorbeeld, als het % CPU op meerdere virtuele machines in een abonnement gelijktijdig pieken, waardoor er veel afzonderlijke waarschuwingen en als deze waarschuwingen hebben samen is opgetreden op elk moment in het verleden deze waarschuwingen waarschijnlijk worden gegroepeerd in een enkele Smart-groep, doe een mogelijke hoofdoorzaken van veelvoorkomende aanpakken. Dit betekent dat voor iemand waarschuwingen oplossen, slimme groepen niet alleen ze in staat stelt om ruis te verminderen door het beheer van gerelateerde waarschuwingen als één eenheid samengevoegde, deze ook bevat stappen waarmee ze naar mogelijke algemene belangrijkste voor de waarschuwingen oorzaken.

Het algoritme beschouwt op dit moment alleen waarschuwingen van de dezelfde monitor-service binnen een abonnement. Slimme groepen kunnen maximaal 99% van de waarschuwing ruis via deze samenvoeging te verminderen. U kunt de reden dat er waarschuwingen zijn opgenomen in een groep in de pagina met slimme groep weergeven.

U kunt de details van slimme groepen weergeven en de status op dezelfde manier worden ingesteld op hoe u met waarschuwingen kunt. Elke waarschuwing is lid van slechts één slimme groep. 

## <a name="smart-group-state"></a>Slimme groepsstatus
Slimme groepsstatus is een vergelijkbare concept naar de waarschuwingsstatus, waarmee u voor het beheren van het omzettingsproces op het niveau van een slimme groep. Net als bij de waarschuwingsstatus wanneer een slimme groep wordt gemaakt, heeft de **nieuw** staat, die kan worden gewijzigd in een **bevestigd** of **gesloten**.

De volgende statussen van slimme groep worden ondersteund.

| Status | Description |
|:---|:---|
| Nieuw | Het probleem alleen is gedetecteerd en nog niet zijn beoordeeld. |
| Bevestigd | Een beheerder heeft de slimme groep bekeken en werken op deze gestart. |
| Gesloten | Het probleem is opgelost. Nadat een slimme groep is gesloten, kunt u deze opnieuw openen door deze te wijzigen naar een andere status. |

[Informatie over het wijzigen van de status van uw slimme groep.](https://aka.ms/managing-alert-smart-group-states)

> [!NOTE]
>  Als u de status van een slimme groep wijzigt, wordt de status van de waarschuwingen afzonderlijk lid niet gewijzigd.

## <a name="smart-group-details-page"></a>Pagina met details van slimme groep

De detailpagina van het Smart-groep wordt weergegeven wanneer u een slimme groep selecteert. Het bevat informatie over de slimme groep, met inbegrip van de redenering die is gebruikt voor het maken van de groep, en kunt u de status te veranderen.
 
![Details van slimme](media/alerts-smartgroups-overview/smart-group-detail.png)


De detailpagina van het smart groep bevat de volgende secties.

| Sectie | Description |
|:---|:---|
| Waarschuwingen | Geeft een lijst van de afzonderlijke waarschuwingen die zijn opgenomen in de slimme groep. Selecteer een waarschuwing om de Waarschuwingsdetails-pagina te openen. |
| Geschiedenis | Geeft een lijst van elke actie op die door de slimme groep en alle wijzigingen die zijn aangebracht. Dit is momenteel beperkt tot statuswijzigingen in de en waarschuwingen lidmaatschap. |

## <a name="smart-group-taxonomy"></a>Slimme groep taxonomie

De naam van een slimme groep is de naam van de eerste waarschuwing. U kunt maken of wijzig de naam van een slimme groep.

## <a name="next-steps"></a>Volgende stappen

- [Slimme groepen beheren](https://aka.ms/managing-smart-groups)
- [De groepsstatus van uw waarschuwingen en slimme wijzigen](https://aka.ms/managing-alert-smart-group-states)

