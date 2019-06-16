---
title: Migreren van Azure Containerservice (ACS) naar Azure Kubernetes Service (AKS)
description: Van Azure Containerservice (ACS) migreren naar Azure Kubernetes Service (AKS).
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: dcee8da943603fb0978caf9992be76347ca197d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65977720"
---
# <a name="migrate-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Migreren van Azure Containerservice (ACS) naar Azure Kubernetes Service (AKS)

Dit artikel helpt u bij het plannen en uitvoeren van een geslaagde migratie tussen Azure Container Service (ACS) met Kubernetes en Azure Kubernetes Service (AKS). Als u wilt, kunt u belangrijke beslissingen in deze handleiding details van de verschillen tussen ACS en AKS en biedt een overzicht van het migratieproces.

## <a name="differences-between-acs-and-aks"></a>Verschillen tussen ACS en AKS

ACS en AKS verschillen in enkele belangrijke onderwerpen die invloed hebben op migratie. U moet vóór de migratie, controleren en van plan bent om de volgende verschillen:

* AKS-knooppunten gebruiken [beheerde schijven](../virtual-machines/windows/managed-disks-overview.md).
    * Niet-beheerde schijven moeten worden geconverteerd, voordat u ze aan de AKS-knooppunten koppelen kunt.
    * Aangepaste `StorageClass` objecten voor Azure-schijven moeten worden gewijzigd van `unmanaged` naar `managed`.
    * Alle `PersistentVolumes` moet gebruiken `kind: Managed`.
* Biedt ondersteuning voor AKS [meerdere knooppuntgroepen](https://docs.microsoft.com/azure/aks/use-multiple-node-pools) (momenteel in Preview-versie).
* Op basis van Windows Server-knooppunten zijn momenteel in [preview in AKS](https://azure.microsoft.com/blog/kubernetes-on-azure/).
* AKS ondersteunt een beperkte set [regio's](https://docs.microsoft.com/azure/aks/quotas-skus-regions).
* AKS is een beheerde service met een gehoste controlelaag van Kubernetes. Mogelijk moet u uw toepassingen aanpassen als u de configuratie van de ACS-modellen eerder hebt gewijzigd.

## <a name="differences-between-kubernetes-versions"></a>Verschillen tussen de Kubernetes-versies

Als waarnaar u migreert naar een nieuwere versie van Kubernetes (bijvoorbeeld van 1.7.x naar 1.9.x), raadpleegt u de volgende bronnen voor meer informatie over een aantal wijzigingen aan de Kubernetes-API:

* [Migreren van een ThirdPartyResource naar CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/)
* [Workloads API-wijzigingen in versie 1.8 en 1.9](https://kubernetes.io/docs/reference/workloads-18-19/)

## <a name="migration-considerations"></a>Overwegingen bij migraties

### <a name="agent-pools"></a>Agentpools

Hoewel AKS de controlelaag van Kubernetes beheert, definieert u nog steeds de grootte en het aantal knooppunten in het nieuwe cluster wilt opnemen. Ervan uitgaande dat u wilt dat een 1:1-toewijzing van ACS naar AKS, moet u voor het vastleggen van uw bestaande gegevens van de ACS-knooppunt. Deze gegevens gebruiken bij het maken van uw nieuwe AKS-cluster.

Voorbeeld:

| Name | Count | VM-grootte | Besturingssysteem |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Omdat de extra virtuele machines worden geïmplementeerd in uw abonnement tijdens de migratie, moet u controleren of uw quota en limieten voldoende voor deze resources zijn. 

Zie voor meer informatie, [Azure-abonnement en Servicelimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits). Om te controleren of uw huidige quota's in Azure portal, gaat u naar de [blade abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecteer uw abonnement en selecteer vervolgens **gebruik + quota**.

### <a name="networking"></a>Netwerken

Voor complexe toepassingen, zult u doorgaans na verloop van tijd in plaats van in één keer migreren. Dit betekent dat dat de oude en nieuwe omgevingen om te communiceren via het netwerk mogelijk. Toepassingen die eerder is gebruikt `ClusterIP` services om te communiceren moeten mogelijk worden weergegeven als type `LoadBalancer` en op de juiste wijze worden beveiligd.

Als u wilt de migratie is voltooid, moet u clients verwijzen naar de nieuwe services die worden uitgevoerd op AKS. Het is raadzaam dat u verkeer omleiden door bij te werken DNS om te verwijzen naar de Load Balancer die bevindt zich voor uw AKS-cluster.

### <a name="stateless-applications"></a>Stateless toepassingen

Stateless toepassingen migreren is het meest eenvoudige geval. U zult uw YAML-definities van toepassing op het nieuwe cluster, zorg ervoor dat alles werkt zoals verwacht en omleiden van verkeer voor het activeren van het nieuwe cluster.

### <a name="stateful-applications"></a>Stateful toepassingen

De migratie van stateful toepassingen om te voorkomen dat gegevens verloren gaan of onverwachte downtime zorgvuldig plannen.

#### <a name="highly-available-applications"></a>Maximaal beschikbare toepassingen

U kunt sommige stateful toepassingen in een configuratie met hoge beschikbaarheid implementeren. Deze toepassingen kunnen u gegevens kopiëren voor replica's. Als u momenteel gebruikmaakt van dit soort implementatie, is het mogelijk dat u kunt maken van een nieuw lid op het nieuwe AKS-cluster en vervolgens migreren met minimale invloed op downstream aanroepers. Over het algemeen zijn de migratiestappen voor dit scenario:

1. Maak een nieuwe secundaire replica in AKS.
2. Wachten op gegevens om te repliceren.
3. Kan niet meer dan een secundaire replica van de nieuwe primaire maken.
4. Verkeer verwijzen naar het AKS-cluster.

#### <a name="migrating-persistent-volumes"></a>Permanente volumes migreren

Als u een bestaande permanente volumes naar AKS migreert bent, volgt u in het algemeen als volgt:

1. Stilleggen schrijft naar de toepassing. (Deze stap is optioneel en te worden stilgelegd.)
2. Momentopnamen van de schijven.
3. Maak nieuwe beheerde schijven van de momentopnamen.
4. Permanente volumes in AKS maakt.
5. Bijwerken van de schil specificaties die moeten worden [bestaande volumes gebruiken](https://docs.microsoft.com/azure/aks/azure-disk-volume) in plaats van PersistentVolumeClaims (statische provisioning genoemd).
6. De toepassing implementeren naar AKS.
7. Valideren.
8. Verkeer verwijzen naar het AKS-cluster.

> [!IMPORTANT]
> Als u ervoor niet stilleggen schrijfbewerkingen kiest, moet u voor het repliceren van gegevens naar de nieuwe implementatie. Anders zult u de gegevens die is geschreven nadat u de schijf-momentopnamen heeft gemist.

Aantal open-source hulpprogramma's kunt u bij het maken van beheerde schijven en volumes tussen Kubernetes-clusters migreren:

* [De extensie van Azure CLI schijf kopiëren](https://github.com/noelbundick/azure-cli-disk-copy-extension) kopieert en zet de schijven in resourcegroepen en Azure-regio's.
* [De extensie van Azure CLI Kube](https://github.com/yaron2/azure-kube-cli) ACS Kubernetes-volumes te inventariseren en ze worden gemigreerd naar een AKS-cluster.

#### <a name="azure-files"></a>Azure Files

In tegenstelling tot de schijven, kunnen Azure Files gelijktijdig worden gekoppeld aan meerdere hosts. In uw AKS-cluster, Azure en Kubernetes niet voorkomen dat u het maken van een schil die nog steeds gebruikmaakt van uw ACS-cluster. Zorg ervoor dat de clusters niet naar dezelfde bestanden die op hetzelfde moment schrijven om te voorkomen dat gegevens verloren gaan en onverwacht gedrag.

Als uw toepassing meerdere replica's die naar dezelfde bestandsshare verwijzen hosten kan, gaat u als volgt de stateless migratiestappen uitvoeren en uw YAML-definities implementeren naar het nieuwe cluster. Als dat niet het geval is, moet u mogelijk migratie benadering omvat de volgende stappen:

1. De toepassing implementeren naar AKS met een aantal replica's van 0.
2. Schaal de toepassing op de ACS op 0. (Deze stap worden stilgelegd.)
3. De toepassing schalen in AKS maximaal 1.
4. Valideren.
5. Verkeer verwijzen naar het AKS-cluster.

Als u beginnen met een lege share- en een kopie maken van de brongegevens bevinden wilt, kunt u de [ `az storage file copy` ](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) opdrachten om uw gegevens te migreren.

### <a name="deployment-strategy"></a>Implementatiestrategie

U wordt aangeraden dat u uw bestaande CI/CD-pijplijn gebruiken voor het implementeren van een bekende juiste configuratie in AKS. Uw bestaande implementatietaken klonen en zorg ervoor dat `kubeconfig` verwijst naar het nieuwe AKS-cluster.

Als dat niet mogelijk is, resourcedefinities van ACS exporteren en vervolgens toegepast op AKS. U kunt `kubectl` objecten exporteren.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Aantal open-source-hulpprogramma's kunt, afhankelijk van de implementatiebehoeften van uw:

* [Velero](https://github.com/heptio/ark) (dit hulpprogramma moet Kubernetes 1.7.)
* [Azure CLI Kube-extensie](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Migratiestappen

1. [Een AKS-cluster maken](https://docs.microsoft.com/azure/aks/create-cluster) via Azure portal, Azure CLI of Azure Resource Manager-sjabloon.

   > [!NOTE]
   > Voorbeeld van Azure Resource Manager-sjablonen voor AKS op vinden de [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) -bibliotheek op GitHub.

2. Breng alle benodigde wijzigingen aan uw YAML-definities. Vervang bijvoorbeeld `apps/v1beta1` met `apps/v1` voor `Deployments`.

3. [-Volumes migreren](#migrating-persistent-volumes) (optioneel) van uw ACS-cluster naar uw AKS-cluster.

4. Uw CI/CD-systeem gebruiken om toepassingen met AKS te implementeren. Of kubectl gebruiken om toe te passen de YAML-definities.

5. Valideren. Zorg ervoor dat uw toepassingen werken zoals verwacht en dat alle gemigreerde gegevens zijn gekopieerd.

6. Verkeer omleiden. Bijwerken van de DNS dat deze clients naar uw AKS-implementatie.

7. Taken na de migratie voltooien. Als u volumes gemigreerd waarbij niet stilleggen schrijfbewerkingen, kopieert u deze gegevens naar het nieuwe cluster.
