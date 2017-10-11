---
title: Implementeer containers met Helm in Azure Kubernetes | Microsoft Docs
description: Het Helm verpakking-hulpprogramma gebruiken om de containers op een cluster Kubernetes in Azure Container Service implementeren
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3cfcc5abbee03ca8fbbec4e4eae711e7c2d9deae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>Helm gebruiken voor het implementeren van containers op een cluster Kubernetes 

[Helm](https://github.com/kubernetes/helm/) is een open source verpakking hulpprogramma waarmee u kunt installeren en beheren van de levenscyclus van Kubernetes toepassingen. Net als bij Linux pakket managers zoals Apt get- en Yum, Helm wordt gebruikt voor het beheren van Kubernetes grafieken die pakketten van vooraf geconfigureerde Kubernetes resources zijn. In dit artikel leest u hoe werken met Helm op een cluster Kubernetes is geïmplementeerd in Azure Container Service.

Helm bestaat uit twee onderdelen: 
* De **Helm CLI** een client die lokaal of in de cloud wordt uitgevoerd op uw computer  

* **Helmstok** is een server die wordt uitgevoerd op het cluster Kubernetes en beheert de levenscyclus van uw toepassingen Kubernetes 
 
## <a name="prerequisites"></a>Vereisten

* [Maak een cluster Kubernetes](container-service-kubernetes-walkthrough.md) in Azure Container Service

* [Installeer en configureer `kubectl` ](../container-service-connect.md) op een lokale computer

* [Installeer Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) op een lokale computer

## <a name="helm-basics"></a>Helm basisbeginselen 

Als u informatie over het Kubernetes-cluster dat u helmstok installeren en implementeren van uw toepassingen op, typ de volgende opdracht:

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Nadat u Helm hebt geïnstalleerd, installeert u helmstok op uw cluster Kubernetes door de volgende opdracht te typen:

```bash
helm init --upgrade
```
Wanneer het is voltooid, ziet u de volgende uitvoer:

![Helmstok installatie](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Als u wilt weergeven van alle Helm grafieken die beschikbaar is in de opslagplaats, typ de volgende opdracht:

```bash 
helm search 
```

Ziet u de volgende uitvoer:

![Helm zoeken](./media/container-service-kubernetes-helm/helm-search.png)
 
Voor het bijwerken van de grafieken om op te halen van de meest recente versies, typt u:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Een grafiek Nginx inkomend domeincontroller implementeren 
 
Typ één opdracht voor het implementeren van een grafiek Nginx inkomend domeincontroller:

```bash
helm install stable/nginx-ingress 
```
![Inkomend domeincontroller implementeren](./media/container-service-kubernetes-helm/nginx-ingress.png)

Als u typt `kubectl get svc` om weer te geven van alle services die worden uitgevoerd op het cluster, u zien dat een IP-adres aan de domeincontroller inkomend is toegewezen. (Terwijl de toewijzing uitgevoerd wordt, ziet u `<pending>`. Het duurt een paar minuten duren.) 

Na het IP-adres wordt toegewezen, gaat u naar de waarde van het externe IP-adres voor een overzicht van de Nginx back-end uitgevoerd. 
 
![Inkomend IP-adres](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Een lijst van grafieken geïnstalleerd op het cluster wilt bekijken, typt u:

```bash
helm list 
```

U kunt ook de opdracht voor het opgeven `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Een grafiek MariaDB en de client implementeren

Een grafiek MariaDB en een client MariaDB verbinding maken met de database nu implementeren.

Typ de volgende opdracht voor het implementeren van de grafiek MariaDB:

```bash
helm install --name v1 stable/mariadb
```

waar `--name` een label dat is gebruikt voor versies.

> [!TIP]
> Als de implementatie mislukt, voert u `helm repo update` en probeer het opnieuw.
>
 
 
Als u wilt de diagrammen die zijn geïmplementeerd op het cluster weergeven, typt u:

```bash 
helm list
```
 
Als u wilt weergeven van alle implementaties die worden uitgevoerd op uw cluster, typt u:

```bash
kubectl get deployments 
``` 
 
 
Ten slotte voor het uitvoeren van een schil om toegang tot de client, typt u:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
Voor verbinding met de client, typt u de volgende opdracht, vervangen `v1-mariadb` met de naam van uw implementatie:

```bash
sudo mysql –h v1-mariadb
```
 
 
U kunt de standaard SQL-opdrachten nu gebruiken voor het maken van databases, tabellen, enzovoort. Bijvoorbeeld: `Create DATABASE testdb1;` maakt een lege database. 
 
 
 
## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het beheren van Kubernetes grafieken de [Helm documentatie](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

