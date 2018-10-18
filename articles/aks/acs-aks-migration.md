---
title: Migreren van Azure Containerservice (ACS) naar Azure Kubernetes Service (AKS)
description: Migreren van Azure Containerservice (ACS) naar Azure Kubernetes Service (AKS)
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: e42b0e7bd1bce40b7c58d75cb07f5a3f8afa5836
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49385038"
---
# <a name="migrating-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Migreren van Azure Containerservice (ACS) naar Azure Kubernetes Service (AKS)

Het doel van dit document is om te plannen en uitvoeren van een geslaagde migratie tussen Azure Container Service met Kubernetes (ACS) en Azure Kubernetes Service (AKS). Deze handleiding details van de verschillen tussen ACS en AKS, biedt een overzicht van het migratieproces en kunt u belangrijke beslissingen.

## <a name="differences-between-acs-and-aks"></a>Verschillen tussen ACS en AKS

ACS en AKS verschillen in enkele belangrijke onderwerpen die van invloed zijn op migratie. U moet bekijken en te plannen om de volgende punten voordat u een migratie.

* AKS-knooppunten gebruiken [Managed Disks](../virtual-machines/windows/managed-disks-overview.md)
    * Niet-beheerde schijven moet worden geconverteerd, voordat ze kunnen worden gekoppeld aan de AKS-knooppunten
    * Aangepaste `StorageClass` objecten voor Azure-schijven worden gewijzigd moet van `unmanaged` naar `managed`
    * Alle `PersistentVolumes` moet gebruiken `kind: Managed`
* Op dit moment ondersteunt slechts één agentpool AKS
* Windows Server-gebaseerde knooppunten zijn momenteel in [beperkte Preview-versie](https://azure.microsoft.com/blog/kubernetes-on-azure/)
* Controleer de lijst met AKS [ondersteunde regio's](https://docs.microsoft.com/azure/aks/container-service-quotas)
* AKS is een beheerde service met een gehoste controlelaag van Kubernetes. U moet mogelijk uw toepassingen aanpassen als u de configuratie van de ACS-modellen eerder hebt gewijzigd

### <a name="differences-between-kubernetes-versions"></a>Verschillen tussen de Kubernetes-versies

Als u naar een nieuwere versie van Kubernetes migreert waarnaar (ex: 1.7.x naar 1.9.x), er zijn enkele wijzigingen in de k8s-API die uw aandacht nodig hebt.

* [Migreren van een ThirdPartyResource naar CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/)
* [Workloads API-wijzigingen in versie 1.8 en 1.9](https://kubernetes.io/docs/reference/workloads-18-19/).

## <a name="migration-considerations"></a>Overwegingen bij migraties

### <a name="agent-pools"></a>Agentpools

Terwijl AKS de controlelaag van Kubernetes beheert, definieert u nog steeds de grootte en het aantal knooppunten dat u wilt opnemen in het nieuwe cluster. Ervan uitgaande dat u wilt dat een 1:1-toewijzing van ACS naar AKS, moet u voor het vastleggen van uw bestaande gegevens van de ACS-knooppunt. U gebruikt deze gegevens bij het maken van uw nieuwe AKS-cluster.

Voorbeeld:

| Naam | Count | VM-grootte | Besturingssysteem |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Omdat de extra virtuele machines worden geïmplementeerd in uw abonnement tijdens de migratie, moet u controleren of uw quota en limieten voldoende voor deze resources zijn. U kunt meer informatie aan de hand [Azure-abonnement en Servicelimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits). Om te controleren of uw huidige quota, gaat u naar de [blade abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal, selecteert u uw abonnement en vervolgens `Usage + quotas`.

### <a name="networking"></a>Netwerken

Voor complexe toepassingen, zult u doorgaans na verloop van tijd in plaats van in één keer migreren. Dit betekent dat dat de oude en nieuwe omgevingen om te communiceren via het netwerk mogelijk. Toepassingen die eerder konden gebruiken `ClusterIP` services om te communiceren moeten mogelijk worden weergegeven als type `LoadBalancer` en op de juiste wijze worden beveiligd.

Als u wilt de migratie is voltooid, moet u clients verwijzen naar de nieuwe services die worden uitgevoerd op AKS. De aanbevolen manier om het omleiden van verkeer is door het bijwerken van DNS om te verwijzen naar de Load Balancer die bevindt zich voor uw AKS-cluster.

### <a name="stateless-applications"></a>Stateless toepassingen

Stateless toepassingen migreren is het meest eenvoudige geval. U zult uw YAML-definities van toepassing op het nieuwe cluster, controleren of alles werkt zoals verwacht en omleiden van verkeer voor het activeren van het nieuwe cluster.

### <a name="stateful-applications"></a>Stateful toepassingen

Migreren stateful toepassingen vereist zorgvuldige planning om te voorkomen dat gegevens verloren gaan of onverwachte downtime.

#### <a name="highly-available-applications"></a>Maximaal beschikbare toepassingen

Sommige stateful toepassingen kunnen worden geïmplementeerd in een configuratie met hoge beschikbaarheid en gegevens kunnen kopiëren naar replica's. Als dit wordt de huidige implementatie beschreven, is het mogelijk dat het mogelijk te maken van een nieuw lid op het nieuwe AKS-cluster en migreren met minimale gevolgen voor downstream aanroepers. Er zijn in het algemeen de migratiestappen voor dit scenario:

1. Maak een nieuwe secundaire replica in AKS
2. Wachten op gegevens om te repliceren
3. Niet meer dan worden de nieuwe primaire voor secundaire replica maken
4. Bepaald verkeer naar het AKS-cluster

#### <a name="migrating-persistent-volumes"></a>Permanente Volumes migreren

Er zijn diverse factoren om te overwegen als u bestaande permanente Volumes AKS migreert waarnaar. Over het algemeen zijn de vereiste stappen:

1. (Optioneel) Stilleggen schrijfbewerkingen naar de toepassing (uitvaltijd vereist)
2. Momentopnameschijven
3. Nieuwe beheerde schijven maken van momentopnamen
4. Permanente Volumes in AKS maakt
5. Bijwerken van de schil specificaties die moeten worden [bestaande volumes gebruiken](https://docs.microsoft.com/azure/aks/azure-disk-volume) in plaats van PersistentVolumeClaims (vaste inrichting)
6. Toepassing implementeren naar AKS
7. Valideren
8. Bepaald verkeer naar het AKS-cluster

> **Belangrijke**: als u ervoor niet stilleggen schrijfbewerkingen kiest, moet u voor het repliceren van gegevens naar de nieuwe implementatie als u gegevens die sinds de schijfmomentopname van de is geschreven ontbreken

Open-source hulpprogramma's bestaan die kunnen helpen u bij het maken van beheerde schijven en volumes tussen Kubernetes-clusters migreren.

* [noelbundick/azure-cli-schijf-extension](https://github.com/noelbundick/azure-cli-disk-copy-extension) - kopiëren en schijven converteren tussen resourcegroepen en Azure-regio's
* [yaron2/azure-kube-cli](https://github.com/yaron2/azure-kube-cli) - ACS Kubernetes-volumes te inventariseren en migreren naar een AKS-cluster

#### <a name="azure-files"></a>Azure Files

In tegenstelling tot de schijven, kunnen Azure Files gelijktijdig worden gekoppeld aan meerdere hosts. Azure noch Kubernetes voorkomt dat u het maken van een schil in uw AKS-cluster die nog steeds wordt gebruikt door uw ACS-cluster. Om te voorkomen dat gegevens verloren gaan en onverwacht gedrag, moet u ervoor zorgen dat beide clusters worden niet naar dezelfde bestanden die op hetzelfde moment schrijven.

Als uw toepassing meerdere replica's die verwijst naar dezelfde bestandsshare hosten kan, kunt u de migratie van staatloze stappen en uw YAML-definities implementeren naar het nieuwe cluster.

Als dat niet het geval is, moet u mogelijk migratie benadering omvat de volgende stappen:

1. De toepassing implementeren naar AKS met een aantal replica's van 0
2. Schaal van de toepassing op ACS op 0 (uitvaltijd vereist)
3. De toepassing schalen in AKS maximaal 1
4. Valideren
5. Bepaald verkeer naar het AKS-cluster

In gevallen waar u wilt beginnen met een lege delen en vervolgens maakt u een kopie van de brongegevens bevinden, kunt u de [ `az storage file copy` ](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) opdrachten om uw gegevens te migreren.

### <a name="deployment-strategy"></a>Implementatiestrategie

De aanbevolen methode is het gebruik van uw bestaande CI/CD-pijplijn naar een bekende juiste configuratie implementeren naar AKS. U moet uw bestaande implementeren taken klonen en zorg ervoor dat uw `kubeconfig` verwijst naar het nieuwe AKS-cluster.

In gevallen waar dat is niet mogelijk is, moet u resource-uitbreiding van ACS exporteren en deze vervolgens toepassen op AKS. U kunt `kubectl` objecten exporteren.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Er zijn ook verschillende open-source-hulpprogramma's die, afhankelijk van uw behoeften helpen kunnen:

* [heptio/ark](https://github.com/heptio/ark) -k8s vereist 1.7
* [yaron2/azure-kube-cli](https://github.com/yaron2/azure-kube-cli)
* [mhausenblas/reshifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Migratiestappen

### <a name="1-create-an-aks-cluster"></a>1. Een AKS-cluster maken

U kunt de documenten te volgen [een AKS-cluster maken](https://docs.microsoft.com/azure/aks/create-cluster) via Azure portal, Azure CLI of Resource Manager-sjabloon.

> Vindt u voorbeeld van Azure Resource Manager-sjablonen voor AKS op de [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) -bibliotheek op GitHub

### <a name="2-modify-applications"></a>2. Wijzig de toepassingen

Brengt de benodigde wijzigingen aan uw YAML-definities. Bijvoorbeeld: vervangen `apps/v1beta1` met `apps/v1` voor `Deployments`

### <a name="3-optional-migrate-volumes"></a>3. (Optioneel) -Volumes migreren

Volumes uit uw ACS-cluster migreren naar uw AKS-cluster. Meer informatie vindt u de [permanente Volumes migreren](#Migrating-Persistent-Volumes) sectie.

### <a name="4-deploy-applications"></a>4. Toepassingen implementeren

Gebruik uw CI/CD-systeem te implementeren van toepassingen voor AKS kubectl gebruiken om toe te passen de YAML-definities.

### <a name="5-validate"></a>5. Valideren

Valideren dat uw toepassingen werken zoals verwacht en dat alle gemigreerde gegevens zijn gekopieerd.

### <a name="6-redirect-traffic"></a>6. Verkeer omleiden

Bijwerken van de DNS dat deze clients naar uw AKS-implementatie.

### <a name="7-post-migration-tasks"></a>7. Taken na migratie

Als u volumes gemigreerd waarbij niet stilleggen schrijfbewerkingen, moet u die gegevens kopiëren naar het nieuwe cluster.
