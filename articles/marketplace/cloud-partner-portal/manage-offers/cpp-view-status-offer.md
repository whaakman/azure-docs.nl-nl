---
title: 'De status van Marketplace-aanbiedingen: Azure Marketplace bekijken | Microsoft Docs'
description: Bekijk de status van aanbiedingen op Azure en AppSource marktplaatsen, met behulp van de Cloud Partner-Portal
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: pbutlerm
ms.openlocfilehash: d7c2909e59643378e765fa51e2d261cbdc106822
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355532"
---
# <a name="view-the-publishing-status-of-azure-marketplace-and-appsource-offers"></a>De publicatiestatus van aanbiedingen voor Azure Marketplace en AppSource weergeven

Nadat u een aanbieding hebt gemaakt, en met name tijdens het publicatieproces, kunt u de status van uw aanbieding weergeven in de Cloud Partner-Portal.  Algemene publicatiestatus is beschikbaar in de [ **alle biedt** ](../portal-tour/cpp-all-offers-page.md) en [ **goedkeuringen** ](../portal-tour/cpp-approvals-page.md) pagina's van de portal.  Een van de volgende statusindicatoren moet worden weergegeven voor elke aanbieding.  

|            Status              |   Description                                                           |
|            ------              |   -----------                                                           |
| **-**                          | Aanbieding is gemaakt maar publicatieproces nog niet begonnen.            |
| **Publiceren wordt uitgevoerd**        | Aanbieding werkt eraan door de stappen van het publicatieproces.   |
| **Publiceren is mislukt**             | Een belangrijk probleem is gedetecteerd tijdens het valideren of controle door Microsoft. |
| **Geannuleerde publiceren**           | De uitgever heeft de aanbieding publicatieproces geannuleerd.  Deze status is niet een bestaande aanbieding op marketplace noteringslijst. | 
| **Wacht op publisher-Meld u af** | Aanbieding door Microsoft is beoordeeld en nu wacht op de laatste controle door de uitgever. |
| **Delisted**                   | Een eerder gepubliceerde aanbieding op marketplace is verwijderd.      | 
|  |  |


## <a name="publishing-status-details"></a>Details van status van publiceren 

Meer informatie over de status van een aanbieding wanneer deze via het publicatieproces niet is gevonden in de **Status** tabblad van de **nieuwe aanbieding** pagina.  Deze pagina geeft een lijst van alle stappen voor dat aanbiedingtype publiceren.  *Houd er rekening mee dat het aantal en de specifieke stappen vaak tussen aanbiedingstypen verschillen.*  Deze pagina geeft ook eventuele openstaande problemen die worden gegenereerd door de validatie van Microsoft en de controlestappen, die vaak actie door de uitgever is vereist voordat het publicatieproces verder kunt gaan.  Bijvoorbeeld, de volgende afbeelding toont de **Status** tabblad voor een nieuwe virtuele machine-aanbieding. 

![Tabblad van de status voor VM-aanbieding](./media/vm-offer-pub-steps1.png)

Het volgende voorbeeld **Status** tabblad voor een consulting services, met een fout opgetreden in de instellingen voor potentiële klanten.  Omdat lead management vereist is voor adviesservices, kan deze fout moet worden gecorrigeerd voordat publicatie kan worden voortgezet.

![Tabblad van de status voor consulting servicefout weergeven](./media/consulting-service-error.png)

De status van het laatste voorbeeld van een Azure-toepassing bevat een cruciaal voor beoordeling van Microsoft.  Het bevat een hot koppeling naar het VSTS-item dat gedetailleerde informatie over dit probleem controleren bevat.  Zie voor meer informatie, [Azure publiceren toepassingsbieding]().

![Tabblad van de status voor Azure-app controleren probleem weergeven](../azure-applications/media/status-tab-ms-review.png)


## <a name="next-steps"></a>Volgende stappen

Openstaande problemen te corrigeren of bijwerken van instellingen van de aanbieding, moet u [bijwerken van een aanbieding](./cpp-update-offer.md). 
