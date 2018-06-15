---
title: Habitat gebruiken voor het implementeren van uw toepassing in Azure
description: Meer informatie over het implementeren van uw toepassing in Azure virtual machines en containers consistent
keywords: Azure, chef, devops, virtuele machines, overzicht automatiseren, habitat
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 73c6834eb53c0496fa673dd25ab90abc18a6a139
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267407"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Habitat gebruiken voor het implementeren van uw toepassing in Azure
[Habitat](https://www.habitat.sh/) een eerste dag van het kind open source-project die een geheel nieuwe benadering van Toepassingsbeheer biedt is. Habitat maakt de toepassing en haar automation de implementatie-eenheid. Wanneer toepassingen zijn ingepakt in een lichtgewicht 'habitat', de runtime-omgeving of dit een container, bare-metal of PaaS is, is niet meer de focus en geen beperkingen van de toepassing. 

In dit artikel beschrijft de voordelen van habitats.

## <a name="support-for-the-modern-application"></a>Ondersteuning voor moderne toepassingen
Habitat pakketten bevatten alles wat u een toepassing moet worden uitgevoerd gedurende de levenscyclus. Kernonderdelen van habitats zijn:
- Indeling van de verpakking - toepassingen in een pakket Habitat zijn atomic onveranderbaar en controleerbare.
- De leidinggevende Habitat uitgevoerd toepassingspakketten met awareness van de pakketten peer relaties, upgrade strategie en beveiligingsbeleid. De leidinggevende Habitat configureert en beheert de toepassing voor elke omgeving aanwezig is.
- Het ecosysteem Habitat biedt ook een buildservice die een Habitat build plan duurt, maakt u het pakket Habitat en publiceert deze naar een depot.

## <a name="run-any-application-anywhere"></a>Alle toepassingen overal uitvoeren
Met Habitat, toepassingen kunnen worden uitgevoerd in een runtimeomgeving ongewijzigd. Dit omvat alles van bare-metal en virtuele machines voor containers (zoals Docker), cluster-management systems (zoals Mesosphere of Kubernetes) en PaaS-systemen (zoals belangrijke Cloud Foundry).

## <a name="easily-port-legacy-applications"></a>Eenvoudig poort oudere toepassingen
Bij oudere toepassingen zijn ingepakt in een pakket Habitat, zijn de toepassingen onafhankelijk van de omgeving waarvoor ze oorspronkelijk zijn gemaakt. De pakketten kunnen snel worden verplaatst naar moderne omgevingen zoals de cloud of containers. Bovendien omdat Habitat pakketten een standaard, passieve gerichte interface hebben, oudere toepassingen, worden veel gemakkelijker te beheren.

## <a name="improve-the-container-experience"></a>De container-ervaring te verbeteren
Habitat vermindert de complexiteit van het beheer van containers in productieomgevingen. Door automatisering van de configuratie van de toepassing binnen een container adressen Habitat het vlak van de ontwikkelaars uitdagingen bij het verplaatsen van toepassingen op basis van een container van ontwikkelomgevingen in productie.

## <a name="integrate-into-the-chef-devops-workflow"></a>Integreren in de werkstroom Chef DevOps
Het project Habitat is gesponsorde door Chef. Habitat maakt gebruik van Chef van grondige ervaring bij automation infrastructuur ongekende automatiseringsmogelijkheden om toepassingen te brengen. Chef commerciële ondersteuning voor Habitat en naadloze integratie tussen Habitat en Chef levering van ontwikkeling naar de implementatie van het releasecyclus toepassing volledig automatiseren.

## <a name="next-steps"></a>Volgende stappen
* [Een virtuele Windows-machine in Azure met behulp van Chef maken](/azure/virtual-machines/windows/chef-automation)