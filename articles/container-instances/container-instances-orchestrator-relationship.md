---
title: Azure Container Instances en indeling met containers
description: Begrijpen hoe Azure container instances communiceren met de container-orchestrators.
services: container-instances
author: seanmck
ms.service: container-instances
ms.topic: article
ms.date: 11/30/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 08bc344a20ade3d8bb0f7dd23a854fd03ddac006
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52845798"
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Container Instances en container-orchestrators

Vanwege de kleine grootte en richting van de toepassing zijn-containers zeer geschikt voor omgevingen met flexibele levering en op basis van microservice-architecturen. De taak van het automatiseren en beheren van een groot aantal containers en hun werking wordt ook wel *orchestration*. Populaire containerorchestrators bevatten Kubernetes, DC/OS en Docker Swarm.

Azure Container Instances biedt enkele van de mogelijkheden voor eenvoudige planning van de orchestration-platforms. En terwijl dit geldt niet voor de hogere waarde-services die deze platforms bieden, Azure Container Instances aanvullende aan hen kan worden gemaakt. Dit artikel wordt het bereik van wat Azure Container Instances worden verwerkt, en hoe volledige containerorchestrators mogelijk ermee beschreven.

## <a name="traditional-orchestration"></a>Traditionele orchestration

De standaarddefinitie van orchestration omvat de volgende taken:

- **Planning**: een geschikte virtuele machine op voor het uitvoeren van de container gezien een containerinstallatiekopie en een resource-aanvraag, vinden.
- **Affiniteit/Anti-affinity**: opgeven dat een set van containers in de buurt elkaar (voor prestaties) of voldoende ver uit elkaar wordt uitgevoerd (voor beschikbaarheid).
- **Statuscontrole**: Bekijk de fouten van de container en automatisch ze opnieuw plannen.
- **Failover**: bijhouden van wat wordt uitgevoerd op elke machine en plannen van containers van de mislukte computers op in orde knooppunten.
- **Schalen**: toevoegen of verwijderen van containerinstanties zodat deze overeenkomt met vraag, handmatig of automatisch.
- **Netwerken**: een overlaynetwerk bieden voor het coördineren van containers om te communiceren tussen meerdere host-computers.
- **Servicedetectie**: containers elkaar automatisch vinden, zelfs als ze tussen hostmachines verplaatsen en wijzigen van IP-adressen inschakelen.
- **Gecoördineerde toepassingsupgrades**: container upgrades om te voorkomen, downtime van toepassingen beheren en terugdraaien als er iets misgaat.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orchestration met Azure Container Instances: een gelaagde benadering

Azure Container Instances kunt u een gelaagde benadering voor orchestration, biedt alle mogelijkheden plannings- en management vereist voor het uitvoeren van een enkele container, terwijl de orchestrator-platforms om meerdere containers taken onder controle te beheren.

Omdat de onderliggende infrastructuur voor container instances wordt beheerd door Azure, hoeft een orchestrator-platform niet naar zichzelf betrekking hebben op een geschikte host virtuele machine op voor het uitvoeren van een enkele container te zoeken. De flexibiliteit van de cloud zorgt ervoor dat een altijd beschikbaar is. In plaats daarvan kunt de orchestrator concentreren op de taken die de ontwikkeling van meerdere containers architecturen, met inbegrip van schalen en gecoördineerde upgrades te vereenvoudigen.

## <a name="scenarios"></a>Scenario's

Orchestrator-integratie met Azure Container Instances is nog steeds ontstane, verwachten we dat er een paar verschillende omgevingen ontstaan:

### <a name="orchestration-of-container-instances-exclusively"></a>Indeling van de container uitsluitend-exemplaren

Omdat ze snel aan de slag en per seconde factureren, biedt een omgeving op basis van Azure Container Instances uitsluitend de snelste manier om te beginnen en te maken met zeer variabele werkbelastingen.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Combinatie van containerinstanties en containers in virtuele Machines

Voor langlopende, stabiele workloads is het doorgaans goedkoper dan de dezelfde containers uitvoeren met Azure Container Instances indelen van containers in een cluster met eigen virtuele machines. Containerinstanties bieden echter een uitstekende oplossing voor het snel uitvouwen en contracteren van de totale capaciteit te bekommeren om onverwachte of tijdelijke pieken in gebruik.

In plaats van het aantal virtuele machines in uw cluster schalen, en vervolgens extra containers op deze machines implementeren, de orchestrator kunt gewoon de meer containers in Azure Container Instances plannen en te verwijderen wanneer ze niet meer zijn nodig.

## <a name="sample-implementation-virtual-nodes-for-azure-kubernetes-service-aks"></a>Voorbeeldimplementatie: virtuele knooppunten voor Azure Kubernetes Service (AKS)

Snel schalen werkbelastingen van toepassingen in een [Azure Kubernetes Service](../aks/intro-kubernetes.md) (AKS)-cluster, kunt u *virtuele knooppunten* dynamisch gemaakt in Azure Container Instances. Virtuele knooppunten inschakelen momenteel in preview, netwerkcommunicatie tussen pods in ACI met en het AKS-cluster. 

Virtuele knooppunten ondersteuning momenteel voor Linux-container instances. Aan de slag met virtuele knooppunten met behulp van de [Azure CLI](https://go.microsoft.com/fwlink/?linkid=2047538) of [Azure-portal](https://go.microsoft.com/fwlink/?linkid=2047545).

Virtuele knooppunten maken gebruik van de open-source [Virtual Kubelet] [ aci-connector-k8s] om na te bootsen de Kubernetes [kubelet] [ kubelet-doc] door te registreren als een knooppunt met onbeperkte de capaciteit. De Virtual Kubelet verzendt het maken van [schillen] [ pod-doc] als containergroepen in Azure Container Instances.

Zie de [Virtual Kubelet](https://github.com/virtual-kubelet/virtual-kubelet) project voor meer voorbeelden van de Kubernetes-API uit te breiden naar serverloze containerplatforms.

## <a name="next-steps"></a>Volgende stappen

Uw eerste container maken met Azure Container Instances met behulp van de [snelstartgids](container-instances-quickstart.md).

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/