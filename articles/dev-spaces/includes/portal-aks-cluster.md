---
title: bestand opnemen
description: bestand opnemen
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 4c4a5b66fe35da01a3661715e17a9fda20bc2411
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43184727"
---
## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Een Kubernetes-cluster maken dat is ingeschakeld voor Azure Dev Spaces

1. Meld u aan bij Azure Portal op http://portal.azure.com.
1. Kies **Een resource maken** > ga naar **Kubernetes** > selecteer **Kubernetes-service** > **Maken**.

   Voer de volgende stappen uit onder elke kop van het formulier AKS-cluster maken.

    - **PROJECTDETAILS**: selecteer een Azure-abonnement en een nieuwe of bestaande Azure-resourcegroep.
    - **CLUSTERDETAILS**: voer een naam, regio (momenteel moet u EastUS, Central US, WestEurope, WestUS2, CanadaCentral of CanadaEast), versie en DNS-voorvoegsel in voor het AKS-cluster.
    - **SCHAAL**: selecteer een VM-grootte voor de AKS-agentknooppunten en het aantal knooppunten. Als u begint met Azure Dev Spaces, is één knooppunt voldoende voor het verkennen van alle functies. Het aantal knooppunten kan op elk gewenst moment na de implementatie van het cluster gemakkelijk worden aangepast. Let op: de VM-grootte kan niet meer worden gewijzigd als een AKS-cluster eenmaal is gemaakt. Zodra een AKS-cluster is geïmplementeerd, kunt u echter eenvoudig een nieuw AKS-cluster met grotere virtuele machines maken en Dev Spaces gebruiken om dat grotere cluster opnieuw te implementeren als u wilt opschalen.

   Zorg ervoor dat u Kubernetes versie 1.9.6 of hoger kiest.

   ![Configuratie-instellingen voor Kubernetes](../media/common/Kubernetes-Create-Cluster-2.PNG)

   Selecteer **Volgende: verificatie** wanneer u klaar bent.

1. Kies de gewenste instelling voor RBAC (op rollen gebaseerd toegangsbeheer). Azure Dev Spaces biedt ondersteuning voor clusters waarop RBAC is ingeschakeld of uitgeschakeld.

    ![RBAC-instelling](../media/common/k8s-RBAC.PNG)

1. Zorg ervoor dat routering van HTTP-toepassingen is ingeschakeld.

   ![Routering van HTTP-toepassingen inschakelen](../media/common/Kubernetes-Create-Cluster-3.PNG)

    > [!Note]
    > Om [Routering van HTTP-toepassingen](/azure/aks/http-application-routing) in te schakelen op een bestaande cluster, gebruikt u de opdracht: `az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing`

1. Selecteer **Controleren + maken** en vervolgens **Maken** wanneer u klaar bent.
