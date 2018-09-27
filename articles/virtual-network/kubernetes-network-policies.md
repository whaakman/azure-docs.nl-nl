---
title: Azure Kubernetes netwerkbeleidsregels | Microsoft Docs
description: Meer informatie over over Kubernetes netwerkbeleid voor het beveiligen van uw Kubernetes-cluster.
services: virtual-network
documentationcenter: na
author: aanandr
manager: NarayanAnnamalai
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/25/2018
ms.author: aanandr
ms.custom: ''
ms.openlocfilehash: b4f8577724781e5df10846a5fc4e30c8320403f2
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219767"
---
# <a name="azure-kubernetes-network-policies-overview"></a>Overzicht van Azure Kubernetes netwerk beleid

Beleid voor netwerken bieden micro-segmentatie voor schillen, net zoals Netwerkbeveiligingsgroepen (nsg's) micro-segmentatie voor virtuele machines bieden. De implementatie van beleid voor Azure-netwerken ondersteunt de standard netwerkbeleid Kubernetes-specificatie. U kunt labels gebruiken voor het selecteren van een groep van schillen en een lijst van inkomende en uitgaande regels die specificeren van het type van het verkeer dat is toegestaan en naar deze schillen definiëren. Meer informatie over het netwerkbeleid voor Kubernetes in de [Kubernetes-documentatie](https://kubernetes.io/docs/concepts/services-networking/network-policies/).

![Overzicht van de beleidsregels van de Kubernetes-netwerk](./media/kubernetes-network-policies/kubernetes-network-policies-overview.png)

Azure-netwerkbeleid werkt in combinatie met de Azure CNI die zorgt voor een VNet-integratie voor containers. Dit wordt alleen ondersteund op Linux-knooppunten vandaag nog. De implementaties configureren Linux IP tabel regels op basis van de gedefinieerde beleid af te dwingen wordt verkeer gefilterd.

## <a name="planning-security-for-your-kubernetes-cluster"></a>Planning van beveiliging voor uw Kubernetes-cluster
Bij het implementeren van beveiliging voor uw cluster, gebruik netwerkbeveiligingsgroepen (nsg's) om te filteren, Noord-Zuid-verkeer, dat wil zeggen, verkeer binnenkomende en uitgaande aanvragen van uw clustersubnet, en Kubernetes-netwerkbeleid voor Oost-West-verkeer dat is. verkeer tussen de schillen in uw cluster.

## <a name="using-azure-kubernetes-network-policies"></a>Met behulp van Azure Kubernetes-netwerkbeleid
Beleid voor Azure-netwerk kan worden gebruikt in de volgende manieren voor micro-segmentatie voor schillen.

### <a name="acs-engine"></a>ACS-engine
ACS-Engine is een hulpprogramma dat genereert een Azure Resource Manager-sjabloon voor de implementatie van een Kubernetes-cluster in Azure. De configuratie van het cluster is opgegeven in een JSON-bestand dat wordt doorgegeven aan het hulpprogramma bij het genereren van de sjabloon. Zie voor meer informatie over de volledige lijst van ondersteunde clusterinstellingen en de bijbehorende beschrijvingen, Microsoft Azure Container Service Engine - definitie van het Cluster.

Om in te schakelen voor clusters die zijn geïmplementeerd met behulp van de acs-engine beleidsregels, moet u de waarde van de instelling networkPolicy opgeven in het cluster-definitiebestand moet 'azure'.

#### <a name="example-configuration"></a>Voorbeeld van een configuratie

De onderstaande JSON voorbeeldconfiguratie maakt een nieuw virtueel netwerk en een subnet, en implementeert een Kubernetes-cluster in deze met Azure CNI. U wordt aangeraden dat u "Kladblok" gebruiken om de JSON-bestand te bewerken. 
```json
{
  "apiVersion": "vlabs",
  "properties": {
    "orchestratorProfile": {
      "orchestratorType": "Kubernetes",
      “kubernetesConfig”: {
         "networkPolicy": "azure"
       }
    },
    "masterProfile": {
      "count": 1,
      "dnsPrefix": "<specify a cluster name>",
      "vmSize": "Standard_D2s_v3"
    },
    "agentPoolProfiles": [
      {
        "name": "agentpool",
        "count": 2,
        "vmSize": "Standard_D2s_v3",
        "availabilityProfile": "AvailabilitySet"
      }
    ],
   "linuxProfile": {
      "adminUsername": "<specify admin username>",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "<cut and paste your ssh key here>"
          }
        ]
      }
    },
    "servicePrincipalProfile": {
      "clientId": "<enter the client ID of your service principal here >",
      "secret": "<enter the password of your service principal here>"
    }
  }
}

```
### <a name="creating-your-own-kubernetes-cluster-in-azure"></a>Het maken van uw eigen Kubernetes-cluster in Azure
De implementatie kan worden gebruikt voor beleid voor netwerken voor schillen in Kubernetes-clusters die u zelf, zonder afhankelijkheid van hulpprogramma's zoals de ACS-Engine implementeren. In dit geval u eerst de invoegtoepassing CNI installeren en inschakelen op elke virtuele machine in een cluster. Zie voor gedetailleerde instructies [implementeren van de invoegtoepassing voor een Kubernetes-cluster dat u zelf implementeren](deploy-container-networking.md#deploy-plug-in-for-a-kubernetes-cluster).

Zodra het cluster is geïmplementeerd, voert u de volgende `kubectl` opdracht uit om te downloaden en toepassen van beleid voor de Azure-netwerk *daemonset* aan het cluster.

  ```
  kubectl apply -f https://raw.githubusercontent.com/Azure/acs-engine/master/parts/k8s/addons/kubernetesmasteraddons-azure-npm-daemonset.yaml

  ```
De oplossing is ook open-source en de code is beschikbaar op de [opslagplaats Azure Container netwerken](https://github.com/Azure/azure-container-networking/tree/master/npm).



## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure Kubernetes Service](../aks/intro-kubernetes.md).
-  Meer informatie over [containernetwerken](container-networking-overview.md).
- [Implementatie van de invoegtoepassing](deploy-container-networking.md) voor Kubernetes-clusters of Docker-containers.