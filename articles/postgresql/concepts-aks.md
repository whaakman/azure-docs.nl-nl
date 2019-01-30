---
title: Verbinding maken met Azure Kubernetes Service (AKS) met Azure Database for PostgreSQL
description: Meer informatie over Azure Kubernetes Service te verbinden met Azure Database for PostgreSQL
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.date: 11/27/2018
ms.topic: conceptual
ms.openlocfilehash: f25d87c7c557404071d777f4efcf22e53886d96d
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242617"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-postgresql"></a>Verbinden van Azure Kubernetes Service en Azure Database for PostgreSQL

Azure Kubernetes Service (AKS) biedt een beheerde Kubernetes-cluster die kunt u in Azure. Hieronder vindt u enkele opties om te overwegen bij het gebruik van AKS en Azure Database for PostgreSQL samen om een toepassing te maken.


## <a name="accelerated-networking"></a>Versneld netwerken
Versneld netwerken is ingeschakeld onderliggende VM's in uw AKS-cluster gebruiken. Wanneer versneld netwerken is ingeschakeld op een virtuele machine, is er lagere latentie, minder jitter en minder CPU-gebruik op de virtuele machine. Meer informatie over hoe u versnelde netwerken works, de ondersteunde versies van het besturingssysteem en VM-exemplaren voor ondersteund [Linux](../virtual-network/create-vm-accelerated-networking-cli.md).

Vanaf November 2018 ondersteunt AKS versneld netwerken op deze ondersteunde VM-exemplaren. Versneld netwerken is standaard ingeschakeld op nieuwe AKS-clusters die gebruikmaken van deze virtuele machines.

U kunt controleren of uw AKS-cluster netwerken versnelde:
1. Ga naar de Azure-portal en selecteer uw AKS-cluster.
2. Selecteer het tabblad Eigenschappen.
3. Kopieer de naam van de **resourcegroep voor infrastructuur**.
4. Gebruik de zoekbalk van de portal om te zoeken en openen van de resourcegroep voor infrastructuur.
5. Selecteer een virtuele machine in die resourcegroep.
6. Ga naar de VM's **netwerken** tabblad.
7. Controleer of **versnelde netwerken** is 'ingeschakeld.'

Of via de Azure CLI met behulp van de volgende twee opdrachten:
```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query "nodeResourceGroup"
```
De uitvoer is de gegenereerde resourcegroep AKS wordt gemaakt waarin de netwerkinterface. Nemen de naam 'nodeResourceGroup' en deze gebruiken in de volgende opdracht. **EnableAcceleratedNetworking** wordt een true of false zijn:
```azurecli
az network nic list --resource-group nodeResourceGroup -o table
```

## <a name="open-service-broker-for-azure"></a>Open Service Broker voor Azure 
[Open Service Broker for Azure](https://github.com/Azure/open-service-broker-azure/blob/master/README.md) (OSBA) kunt u Azure-services rechtstreeks via Kubernetes of Cloud Foundry inrichten. Het is een [Open Service Broker API](https://www.openservicebrokerapi.org/) implementatie voor Azure.

U kunt met OSBA, een Azure Database for PostgreSQL-server maken en koppelen aan uw AKS-cluster met behulp van Kubernetes de eigen taal. Meer informatie over het gebruik van OSBA en Azure Database for PostgreSQL samen op de [OSBA GitHub-pagina](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md). 


## <a name="connection-pooling"></a>Groepsgewijze verbinding
Een verbindingsgroepering minimaliseert de kosten en de tijd die is gekoppeld met het maken en nieuwe verbindingen met de database wordt gesloten. De pool is een verzameling van verbindingen die kunnen worden hergebruikt. 

Er zijn meerdere verbinding poolers die kunt u met PostgreSQL. Een van deze is [PgBouncer](https://pgbouncer.github.io/). In het Containerregister van Microsoft bieden we een lichtgewicht containers PgBouncer die kan worden gebruikt in een sidecar pool verbindingen van AKS met Azure Database voor PostgreSQL. Ga naar de [docker hub-pagina](https://hub.docker.com/r/microsoft/azureossdb-tools-pgbouncer/) voor informatie over het openen en gebruiken van deze installatiekopie. 


## <a name="next-steps"></a>Volgende stappen
-  [Een Azure Kubernetes Service-cluster maken](../aks/kubernetes-walkthrough.md)
