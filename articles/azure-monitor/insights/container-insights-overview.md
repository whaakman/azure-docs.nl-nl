---
title: Overzicht van Azure Monitor voor containers (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven van Azure Monitor voor containers die bewaakt AKS Container Insights-oplossing en de waarde die het biedt een door de bewaking van de status van uw AKS-clusters en exemplaren van de Container in Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: 0854361b262b1177d72a4ab9718698daff3c47fd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850054"
---
# <a name="azure-monitor-for-containers-preview-overview"></a>Azure Monitor voor een overzicht van containers (Preview)

Azure Monitor voor containers is een functie die is ontworpen voor het bewaken van de prestaties van containerworkloads die zijn geïmplementeerd op beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Controle van uw containers is kritiek, met name wanneer u een productiecluster op schaal, met meerdere toepassingen uitvoert.

Azure Monitor voor containers biedt u de zichtbaarheid van de prestaties door verzamelen geheugen en processors metrische gegevens van domeincontrollers, knooppunten en containers die beschikbaar in Kubernetes via de API voor metrische gegevens zijn. Er worden ook containerlogboeken verzameld.  Nadat u de bewaking van Kubernetes-clusters hebt ingeschakeld, deze metrische gegevens en logboeken worden automatisch verzameld voor u via een beperkte versie van de Log Analytics-agent voor Linux en opgeslagen in uw [Log Analytics](../../azure-monitor/log-query/log-query-overview.md) werkruimte. 
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Wat kost Azure Monitor voor containers bieden?

Azure Monitor voor containers bevat verschillende vooraf gedefinieerde weergaven die de die containerwerkbelastingen en wat van invloed is op de prestatiestatus van het bewaakte Kubernetes-cluster dat u kunt weergeven:  

* Identificeer de AKS-containers die worden uitgevoerd op het knooppunt en het gemiddelde gebruik van de processor en geheugen. Aan de hand van deze kennis kunt u knelpunten in de resource.
* Bepalen waar de container bevindt zich in een domeincontroller of een schil. Aan de hand van deze kennis kunt u de algehele prestaties van de van de domeincontroller of de schil weergeven. 
* Controleer het brongebruik van workloads die worden uitgevoerd op de host die niet gerelateerd zijn aan de standaard processen die ondersteuning bieden voor de schil.
* Begrijp het gedrag van het cluster bij gemiddelde en zwaarste belasting. Deze kennis kunt u identificeren behoeften aan capaciteit en bepaal de maximale belasting van het cluster kan tolereren. 

## <a name="how-do-i-access-this-feature"></a>Hoe krijg ik toegang tot deze functie?
U kunt toegang tot Azure Monitor voor twee manieren van Azure Monitor of rechtstreeks vanuit het geselecteerde AKS-cluster-containers. Van Azure Monitor die u hebt een globaal overzicht van alle containers geïmplementeerd, die worden bewaakt en die niet zo is, zodat u kunt zoeken en filteren in uw abonnementen en resourcegroepen, en vervolgens omlaag naar Azure Monitor voor containers op de geselecteerde de container.  Anders kunt u gewoon de functie openen rechtstreeks vanuit een geselecteerde AKS-container vanuit de AKS-pagina.  

![Overzicht van methoden voor toegang tot Azure Monitor voor containers](./media/container-insights-overview/azmon-containers-views.png)

Als u geïnteresseerd bent in controle en beheer van uw Docker- en Windows containerhosts configuratie weergeven, controle en gebruik van resources, Zie de [Container Monitoring solution](../../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Volgende stappen
Om te beginnen met controleren van uw AKS-cluster, Bekijk [hoe zorgen voor onboarding Azure controleren voor containers](container-insights-onboard.md) om te begrijpen van de vereisten en de beschikbare methoden bewaking wilt inschakelen.  
