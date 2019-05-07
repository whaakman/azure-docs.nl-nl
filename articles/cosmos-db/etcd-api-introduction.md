---
title: Inleiding tot de Azure Cosmos DB-etcd API
description: In dit artikel biedt een overzicht en belangrijkste voordelen van etcd API in Azure Cosmos DB
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: 6c7fcb1429438ee024cb226b63cfcdcab05ed9f8
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205810"
---
# <a name="introduction-to-the-azure-cosmos-db-etcd-api-preview"></a>Inleiding tot de Azure Cosmos DB-etcd API (preview)

Azure Cosmos DB is wereldwijd gedistribueerde, multi-model database-service van Microsoft voor essentiële toepassingen. Het biedt kant en klare wereldwijde distributie, elastische schaalbaarheid van de doorvoer en opslag, slechts enkele milliseconden latentie in het 99e percentiel, en gegarandeerd hoge beschikbaarheid, ondersteund door marktleidende, toonaangevende SLA's.

[Etcd](https://github.com/etcd-io/etcd) is een gedistribueerde sleutel/waarde-archief. In [Kubernetes](https://kubernetes.io/), etcd wordt gebruikt voor het opslaan van de status en de configuratie van de Kubernetes-clusters. Ervoor te zorgen dat de beschikbaarheid, betrouwbaarheid en prestaties van etcd is essentieel om te kunnen de algehele clusterstatus, schaalbaarheid, flexibiliteit van de beschikbaarheid en prestaties van een Kubernetes-cluster. 

De etcd API in Azure Cosmos DB kunt u Azure Cosmos DB gebruiken als het archief met back-end voor [Azure Kubernetes](../aks/index.yml). etcd API in Azure Cosmos DB is momenteel in preview. Azure Cosmos DB implementeert de etcd wire-protocol. Met etcd API in Azure Cosmos DB, ontwikkelaars krijgen automatisch zeer betrouwbare [beschikbaar](high-availability.md), [wereldwijd gedistribueerde](distribute-data-globally.md) Kubernetes. Deze API kan ontwikkelaars Kubernetes statusbeheer schalen op een volledig beheerde cloud-native PaaS-service. 

> [!NOTE]
> In tegenstelling tot andere API's in Azure Cosmos DB, kunt u een etcd API-account via Azure portal, CLI of SDK's niet inrichten. U kunt een etcd API-account inrichten door het implementeren van de Resource Manager-sjabloon. Zie voor gedetailleerde stappen [inrichten Azure Kubernetes met Azure Cosmos DB](bootstrap-kubernetes-cluster.md) artikel. Azure Cosmos DB etcd API is momenteel in de beperkte Preview-versie. U kunt [Meld u aan voor de Preview-versie](https://aka.ms/cosmosetcdapi-signup), door het invullen van het aanmeldingsformulier hebt ingevuld.

## <a name="wire-level-compatibility"></a>Wire-level compatibiliteit

Azure Cosmos DB implementeert de wire-protocol van etcd versie 3, en maakt de [van hoofdknooppunt](https://kubernetes.io/docs/concepts/overview/components/) API-servers voor het gebruik van Azure Cosmos DB, net zoals deze in een omgeving met lokaal geïnstalleerde etcd zou doen. De etcd API biedt ondersteuning voor TLS wederzijdse verificatie. 

Het volgende diagram ziet u de onderdelen van een Kubernetes-cluster. In de cluster-master gebruikt de API-Server etcd-API van Azure Cosmos DB, in plaats van de lokaal geïnstalleerde etcd. 

![Azure Cosmos DB de etcd wire-protocol implementeren](./media/etcd-api-introduction/etcd-api-wire-protocol.png)

## <a name="key-benefits"></a>Belangrijkste voordelen

### <a name="no-etcd-operations-management"></a>Er is geen etcd operationeel management

Volledig beheerde, systeemeigen cloudservice elimineert Azure Cosmos DB de noodzaak van Kubernetes-ontwikkelaars kunnen instellen en beheren van etcd. De etcd API in Azure Cosmos DB is schaalbare, maximaal beschikbare, fouttolerant en hoge prestaties biedt. De overhead van het instellen van replicatie over meerdere knooppunten, uitvoeren van rolling-updates, beveiligingspatches, en de status van de etcd in de gaten worden verwerkt door Azure Cosmos DB.

### <a name="global-distribution--high-availability"></a>Wereldwijde distributie en hoge beschikbaarheid 

Met behulp van etcd API, Azure Cosmos DB garandeert 99,99% beschikbaarheid voor het lezen van gegevens en schrijft in een enkele regio en de beschikbaarheid van 99,999% voor meerdere regio's. 

### <a name="elastic-scalability"></a>Elastische schaalbaarheid

Azure Cosmos DB biedt elastische schaalbaarheid voor lezen en schrijven van aanvragen in verschillende regio's.
Als het Kubernetes-cluster groeit, wordt de etcd API-account in Azure Cosmos DB elastisch schalen zonder downtime. Etcd gegevens opslaan in Azure Cosmos DB, in plaats van de master Kubernetes-knooppunten kan ook meer flexibele schaling van hoofdknooppunt. 

### <a name="security--enterprise-readiness"></a>Gereedheid van de beveiligings- en enterprise

Wanneer etcd gegevens worden opgeslagen in Azure Cosmos DB, Kubernetes-ontwikkelaars automatisch krijgen de [ingebouwde versleuteling-at-rest](database-encryption-at-rest.md), [certificeringen en naleving](compliance.md), en [back-up en herstellen mogelijkheden](online-backup-and-restore.md) ondersteund door Azure Cosmos DB. 

## <a name="next-steps"></a>Volgende stappen

* [Het gebruik van Azure Kubernetes met Azure Cosmos DB](bootstrap-kubernetes-cluster.md)
* [Belangrijkste voordelen van Azure Cosmos DB](introduction.md)
* [Snelstartgids voor AKS-engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md)