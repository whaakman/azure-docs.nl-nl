---
title: Implementeer containers met Helm in Azure Kubernetes
description: Het Helm verpakking-hulpprogramma gebruiken voor het implementeren van containers in een Kubernetes-cluster in Azure Container Service
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 882e785968f94473e80c7a14e5a68498add37735
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38634150"
---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>Helm gebruiken voor het implementeren van containers in een Kubernetes-cluster

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

[Helm](https://github.com/kubernetes/helm/) is een open-source verpakking-hulpprogramma dat helpt u bij het installeren en beheren van de levenscyclus van toepassingen met Kubernetes. Net als bij Linux pakketmanagers zoals Apt-get en Yum, Helm wordt gebruikt voor het beheren van Kubernetes grafieken die pakketten van vooraf geconfigureerde Kubernetes-resources zijn. Dit artikel ziet u hoe u werkt met Helm op een Kubernetes-cluster geïmplementeerd in Azure Container Service.

Helm bestaat uit twee onderdelen: 
* De **Helm CLI** is een client die lokaal of in de cloud wordt uitgevoerd op uw computer  

* **Tiller** is een server die wordt uitgevoerd op het Kubernetes-cluster en beheert de levenscyclus van uw toepassingen met Kubernetes 
 
## <a name="prerequisites"></a>Vereisten

* [Maak een Kubernetes-cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service

* [Installeer en configureer `kubectl` ](../container-service-connect.md) op een lokale computer

* [Installeer Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) op een lokale computer

## <a name="helm-basics"></a>Helm-basisbeginselen 

Als u informatie over het Kubernetes-cluster dat u bent Tiller installeren en implementeren van uw toepassingen op, typ de volgende opdracht:

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Nadat u Helm hebt geïnstalleerd, installeert u Tiller in uw Kubernetes-cluster door de volgende opdracht te typen:

```bash
helm init --upgrade
```
Als deze voltooid is, ziet u de volgende uitvoer:

![Tiller-installatie](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Als u alle Helm-grafieken die beschikbaar in de opslagplaats, typt u de volgende opdracht uit:

```bash 
helm search 
```

Ziet u de volgende uitvoer:

![Helm search](./media/container-service-kubernetes-helm/helm-search.png)
 
Voor het bijwerken van de grafieken om op te halen van de meest recente versies, typt u:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Een diagram van Nginx inkomend controller implementeren 
 
Voor het implementeren van een grafiek van Nginx inkomend controller, typt u een enkele opdracht:

```bash
helm install stable/nginx-ingress 
```
![Controller voor binnenkomend verkeer implementeren](./media/container-service-kubernetes-helm/nginx-ingress.png)

Als u typt `kubectl get svc` als u wilt weergeven van alle services die worden uitgevoerd op het cluster, ziet u dat een IP-adres is toegewezen aan de controller voor binnenkomend verkeer. (Terwijl de toewijzing uitgevoerd wordt, ziet u `<pending>`. Het duurt enkele minuten om te voltooien.) 

Na de IP-adres wordt toegewezen, gaat u naar de waarde van het externe IP-adres om te zien van de back-end van Nginx uitgevoerd. 
 
![IP-adres voor inkomend verkeer](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Een lijst van grafieken geïnstalleerd in uw cluster wilt bekijken, typt u:

```bash
helm list 
```

U kunt ook opgeven de opdracht voor het `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Een MariaDB grafiek en een client implementeren

Implementeer nu een MariaDB-grafiek en een MariaDB-client verbinding maakt met de database.

Voor het implementeren van de grafiek MariaDB, typ de volgende opdracht:

```bash
helm install --name v1 stable/mariadb
```

waar `--name` is een code die wordt gebruikt voor releases.

> [!TIP]
> Als de implementatie mislukt, voert u `helm repo update` en probeer het opnieuw.
>
 
 
Als alle grafieken in uw cluster wordt geïmplementeerd, typt u:

```bash 
helm list
```
 
Als alle implementaties die worden uitgevoerd op uw cluster, typt u:

```bash
kubectl get deployments 
``` 
 
 
Ten slotte, als u wilt uitvoeren in een schil voor toegang tot de client, typt u:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
Voor verbinding met de client, typt u de volgende opdracht, vervangt `v1-mariadb` met de naam van uw implementatie:

```bash
sudo mysql –h v1-mariadb
```
 
 
U kunt nu standaard SQL-opdrachten gebruiken om te maken van databases, tabellen, enzovoort. Bijvoorbeeld, `Create DATABASE testdb1;` wordt een lege database gemaakt. 
 
 
 
## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het beheren van Kubernetes grafieken, de [Helm documentatie](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

