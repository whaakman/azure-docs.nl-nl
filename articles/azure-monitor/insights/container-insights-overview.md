---
title: Overzicht van Azure Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven van Azure Monitor voor containers die bewaakt AKS Container Insights-oplossing en de waarde die het biedt een door de bewaking van de status van uw AKS-clusters en exemplaren van de Container in Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: d6321564672097fbf901d1d33afac9f606fcb63a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65521841"
---
# <a name="azure-monitor-for-containers-overview"></a>Azure Monitor voor containers: overzicht

Azure Monitor voor containers is een functie die is ontworpen voor het bewaken van de prestaties van containerworkloads die zijn geïmplementeerd in Azure Container Instances- of beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Controle van uw containers is kritiek, met name wanneer u een productiecluster op schaal, met meerdere toepassingen uitvoert.

Azure Monitor voor containers biedt u de zichtbaarheid van de prestaties door verzamelen geheugen en processors metrische gegevens van domeincontrollers, knooppunten en containers die beschikbaar in Kubernetes via de API voor metrische gegevens zijn. Er worden ook containerlogboeken verzameld.  Nadat u de bewaking van Kubernetes-clusters hebt ingeschakeld, worden logboeken en metrische gegevens automatisch verzameld voor u via een beperkte versie van de Log Analytics-agent voor Linux. Metrische gegevens worden geschreven naar het archief met metrische gegevens en logboekgegevens geschreven naar het archief met Logboeken die zijn gekoppeld aan uw [Log Analytics](../log-query/log-query-overview.md) werkruimte. 

![Azure Monitor voor containers-architectuur](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Wat kost Azure Monitor voor containers bieden?

Azure Monitor voor containers levert een uitgebreide controle-ervaring met behulp van verschillende functies van Azure Monitor zodat u inzicht krijgt in de prestaties en status van uw Kubernetes-cluster en de containerworkloads. Met Azure Monitor voor containers kunt u het volgende doen:

* Identificeer de AKS-containers die worden uitgevoerd op het knooppunt en het gemiddelde gebruik van de processor en geheugen. Aan de hand van deze kennis kunt u knelpunten in de resource.
* Identificeer de processor en geheugen gebruik van groepen met containers en de containers die worden gehost in Azure Container Instances.  
* Bepalen waar de container bevindt zich in een domeincontroller of een schil. Aan de hand van deze kennis kunt u de algehele prestaties van de van de domeincontroller of de schil weergeven.
* Controleer het brongebruik van workloads die worden uitgevoerd op de host die niet gerelateerd zijn aan de standaard processen die ondersteuning bieden voor de schil.
* Begrijp het gedrag van het cluster bij gemiddelde en zwaarste belasting. Deze kennis kunt u identificeren behoeften aan capaciteit en bepaal de maximale belasting van het cluster kan tolereren. 
* Waarschuwingen voor het proactief waarschuwen u of leg deze vast bij het gebruik van de CPU en geheugen op knooppunten of containers de drempelwaarden overschrijden configureren.  

## <a name="how-do-i-access-this-feature"></a>Hoe krijg ik toegang tot deze functie?
U kunt toegang tot Azure Monitor voor twee manieren van Azure Monitor of rechtstreeks vanuit het geselecteerde AKS-cluster-containers. Van Azure Monitor, hebt u een globale perspectief van alle de containers die worden bewaakt en die zijn niet zo is, zodat u kunt zoeken en filteren in uw abonnementen en resourcegroepen, geïmplementeerd en Ga omlaag naar Azure Monitor voor containers in de geselecteerde container.  Anders kunt u toegang tot de functie rechtstreeks vanuit een geselecteerde AKS-container vanuit de AKS-pagina.  

![Overzicht van methoden voor toegang tot Azure Monitor voor containers](./media/container-insights-overview/azmon-containers-experience.png)

Als u geïnteresseerd bent in controle en beheer van uw Docker- en Windows containerhosts configuratie weergeven, controle en gebruik van resources, Zie de [Container Monitoring solution](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Volgende stappen
Als u wilt bewaken van uw AKS-cluster, Controleer [het inschakelen van de Azure-Monitor voor containers](container-insights-onboard.md) om te begrijpen van de vereisten en de beschikbare methoden bewaking wilt inschakelen.  
