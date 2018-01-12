---
title: Azure Container-exemplaren en container orchestration
description: Begrijpen hoe Azure-container exemplaren communiceren met de container orchestrators.
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/09/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4954dcb4cb03407b85ad35aec94920e39644844b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Container-exemplaren en container orchestrators

Vanwege de klein en de afdrukstand van de toepassing zijn containers uitermate geschikt is voor flexibele levering omgevingen en op basis van microservice-architecturen. De taak van automatiseren en beheren van een groot aantal containers en hun werking wordt ook wel *orchestration*. Populaire container orchestrators zijn Kubernetes DC/OS en Docker Swarm dit zijn allemaal beschikbaar in de [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

Azure Containerexemplaren biedt enkele van de mogelijkheden voor basic planning van de orchestration-platforms, maar dit geldt niet voor de services van de hogere waarde deze platforms bieden en kunnen in feite worden aanvullende met hen. Dit artikel wordt beschreven voor het bereik van wat Azure Containerexemplaren verwerkt en hoe vol container orchestrators ermee kunnen communiceren.

## <a name="traditional-orchestration"></a>Traditionele orchestration

De definitie van het standaard van orchestration omvat de volgende taken:

- **Planning**: een geschikte machine waarop moet worden uitgevoerd van de container gezien een installatiekopie van een container en een resource-aanvraag, vinden.
- **Affiniteit /-affinity**: opgeven dat een set van containers moet worden uitgevoerd in de buurt van elkaar (prestaties) of voldoende ver uit elkaar (voor beschikbaarheid).
- **Statuscontrole**: Bekijk bij fouten van de container en automatisch opnieuw te plannen.
- **Failover**: wat wordt uitgevoerd op elke machine en bijhouden van mislukte machines containers in orde knooppunten plannen.
- **Schalen**: toevoegen of verwijderen van containerexemplaren zodat deze overeenkomen met de vraag, handmatig of automatisch.
- **Networking**: een overlaynetwerk bieden voor het coördineren van containers om te communiceren over meerdere hostmachines.
- **Servicedetectie**: containers naar elkaar automatisch vinden zelfs als ze worden verplaatst tussen hostmachines en wijzigen van IP-adressen inschakelen.
- **Coördinatie toepassingsupgrades**: upgrades van de container voor de toepassing uitvaltijd voorkomen en terugdraaien als er iets mis gaat beheren.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orchestration met exemplaren van Azure-Container: een gelaagde benadering.

Azure Container-exemplaren kunnen een gelaagde benadering orchestration, alle vereist voor het uitvoeren van een enkele container en tegelijkertijd orchestrator platforms voor het beheren van taken met meerdere container boven op het plannings- en mogelijkheden bieden.

Omdat alle van de onderliggende infrastructuur voor exemplaren van de Container wordt beheerd door Azure, hoeft een orchestrator-platform niet naar zichzelf betrekking hebben op bij het zoeken van een geschikte host-machine waarop een enkele container uitgevoerd. De elasticiteit van de cloud zorgt ervoor dat een altijd beschikbaar is. In plaats daarvan kunt de orchestrator zich richten op de taken die de ontwikkeling van meerdere container architecturen, met inbegrip van schalen en gecoördineerde upgrades vereenvoudigen.

## <a name="potential-scenarios"></a>Mogelijke scenario 's

Orchestrator-integratie met Azure Containerexemplaren is nog steeds opkomende, verwachten we dat er een paar verschillende omgevingen kunnen ontstaan:

### <a name="orchestration-of-container-instances-exclusively"></a>Orchestration van Containerexemplaren uitsluitend

Omdat ze snel starten en door het tweede factureren, biedt een omgeving op basis van exemplaren van Azure-Container uitsluitend de snelste manier om te beginnen en om te gaan met maximaal variabele werkbelastingen.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Combinatie van Containerexemplaren en containers in virtuele Machines

Voor langdurige, stabiele werkbelastingen wordt organiseren containers in een cluster toegewezen virtuele machines doorgaans goedkoper dan het uitvoeren van de dezelfde containers met exemplaren van de Container. Containerexemplaren bieden echter een uitstekende oplossing voor het snel uitbreiden en verdragsluitende uw algehele capaciteit om te gaan met onverwachte of tijdelijke pieken in gebruik. In plaats van het aantal virtuele machines in uw cluster uitbreiden, vervolgens extra containers op die computers implementeert, de orchestrator kunt gewoon de extra containers met instanties van de Container plannen en verwijderen wanneer ze niet meer zijn nodig.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Voorbeeldimplementatie: Azure Container exemplaren-Connector voor Kubernetes

Om te demonstreren hoe container orchestration platforms kunnen integreren met Azure Containerexemplaren, hebben we gebouw gestart een [voorbeeld-connector voor Kubernetes][aci-connector-k8s].

De connector voor Kubernetes lijkt de [kubelet] [ kubelet-doc] door te registreren als een knooppunt met onbeperkte capaciteit en tijdens het verzenden van het maken van [gehele product] [ pod-doc] als containergroepen in Azure Containerexemplaren.

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

Connectors voor andere orchestrators kunnen die op dezelfde manier worden geïntegreerd met platform primitieven combineren de kracht van de orchestrator-API met de snelheid en eenvoud van het beheer van containers in Azure Containerexemplaren te worden gebouwd.

> [!WARNING]
> De connector ACI voor Kubernetes is *experimentele* en mag niet worden gebruikt in productie.

## <a name="next-steps"></a>Volgende stappen

Uw eerste container maken met het gebruik van Azure Containerexemplaren de [Quick Start guide](container-instances-quickstart.md).

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/