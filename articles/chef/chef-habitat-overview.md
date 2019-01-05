---
title: Habitat gebruiken voor het implementeren van uw toepassing in Azure
description: Meer informatie over het consistent implementeren van uw toepassing in Azure virtuele machines en containers
keywords: Azure, chef, devops, virtuele machines, overzicht, automatiseren, habitat
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 4847d9ce551c9acf1e4fb6325c770187b2cfd89f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052288"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Habitat gebruiken voor het implementeren van uw toepassing in Azure
[Habitat](https://www.habitat.sh/) is een eerste van de soort open source-project met een geheel nieuwe benadering van Toepassingsbeheer. Habitat maakt de toepassing en haar automation de implementatie-eenheid. Wanneer toepassingen zijn ingepakt in een lichtgewicht "habitat', de runtime-omgeving, ongeacht of het een container, bare-metalcomputers of PaaS, is niet langer de focus en legt geen beperkingen voor de toepassing. 

Dit artikel beschrijft de voordelen van het gebruik van Habitat.

## <a name="support-for-the-modern-application"></a>Ondersteuning voor de moderne toepassing
Habitat-pakketten omvatten alles wat u een toepassing moet worden uitgevoerd gedurende de levenscyclus. Habitat de belangrijkste onderdelen zijn:
- Verpakkingsindeling - toepassingen in een pakket met Habitat zijn atomisch, onveranderbaar en controleerbare.
- De supervisor Habitat wordt toepassingspakketten uitgevoerd met kennis van de pakketten peer, relaties, upgrade strategie en beveiligingsbeleid. De supervisor Habitat configureert en beheert de toepassing voor uw omgeving aanwezig is.
- Het ecosysteem Habitat biedt ook een buildservice die een Habitat-versieplan neemt, maakt u het pakket met Habitat en publiceert deze naar een opslagplaats.

## <a name="run-any-application-anywhere"></a>Elke toepassing overal uitvoeren
Met Habitat, toepassingen kunnen worden uitgevoerd in een runtime-omgeving ongewijzigd. De toepassing kan van alles zijn van bare-metalcomputers en virtuele machines voor containers (zoals Docker), cluster-management-systemen (zoals Mesosphere of Kubernetes) en PaaS-systemen (zoals Pivotal Cloud Foundry).

## <a name="easily-port-legacy-applications"></a>Eenvoudig poort oudere toepassingen
Als u oudere toepassingen zijn ingepakt in een pakket Habitat, zijn de toepassingen onafhankelijk van de omgeving waarvoor ze oorspronkelijk zijn ontworpen. De pakketten kunnen snel worden verplaatst naar moderne omgevingen, zoals de cloud of containers. Oudere toepassingen worden ook veel gemakkelijker te beheren omdat Habitat-pakketten een standaard, passieve gerichte interface hebben.

## <a name="improve-the-container-experience"></a>De containerervaring te verbeteren
Habitat vermindert de complexiteit van het beheer van containers in productie-omgevingen. Door het automatiseren van de configuratie van de toepassing in een container, adressen Habitat het gezicht van de ontwikkelaars uitdagingen bij het verplaatsen van containertoepassingen in ontwikkelomgevingen in productie.

## <a name="integrate-into-the-chef-devops-workflow"></a>Integreren met het Chef DevOps-werkstroom
Het project Habitat wordt gesponsord door Chef. Habitat maakt gebruik van de uitgebreide ervaring van Chef met automatisering van infrastructuur om ongekende automatiseringsmogelijkheden voor toepassingen. Chef bieden commerciële ondersteuning voor Habitat en naadloze integratie tussen Habitat en Chef-levering voor het automatiseren van de versie voor installatie van toepassingen, van ontwikkeling tot implementatie.

## <a name="next-steps"></a>Volgende stappen
* [Een Windows-machine maken op Azure met Chef](/azure/virtual-machines/windows/chef-automation)