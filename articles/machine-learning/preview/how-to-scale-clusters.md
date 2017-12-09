---
title: Een Azure Container Service-cluster schalen voor Machine Learning | Microsoft Docs
description: Schalen van een ACS-cluster - automatisch schalen en statische schalen; het aantal knooppunten schalen in het cluster
services: machine-learning
author: raymondl
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/04/2017
ms.openlocfilehash: 8d709936bfba5c89091d7f26449d165bddb930de
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="scaling-the-cluster-to-manage-web-service-throughput"></a>Het cluster voor het beheren van web service doorvoer schalen

## <a name="why-scale-the-cluster"></a>Waarom het cluster schalen?

Het Azure Container Service (ACS)-cluster schalen, is een effectieve manier om te optimaliseren van de service-doorvoer terwijl de clustergrootte tot een minimum om kosten te beperken. 

Houd rekening met het volgende voorbeeld van een cluster met drie webservices om automatisch schalen beter te begrijpen:

![Voorbeeld: Drie services op een cluster](media/how-to-scale-clusters/three-services.png)

De services hebben verschillende piek-vereisten: 1-Service (line-blauw) vereist 40 gehele product op pieken, 2-Service (line-oranje) vereist 38 bij piek en 3-Service (line-grijze) 50 bij piek vereist. Als u de benodigde piek-capaciteit voor elke service afzonderlijk reserveren, dit cluster moet minimaal 40 + 38 + 50 = 128 totale gehele product.

Maar overweeg het gebruik van de werkelijke schil op elk punt in tijd, vertegenwoordigd door de zwarte stippellijn in de grafiek. In dit geval de *hoogste aantal gehele product gebruikt op elk gewenst moment* is 64, die om 20:00 uur wanneer Service 3 bij piek is optreedt. Op dit moment Service 3 gebruikt 50 gehele product, maar Service 2 gebruikt slechts 9 gehele product en Service 1 alleen 5 gebruikt. Denk eraan dat dit *piekgebruik* voor dit cluster. Dit betekent dat op geen enkel moment het cluster meer dan 64 gehele product--de helft van de berekende vereiste van 128 gehele product voor de drie services geschaald onafhankelijk voor piekgebruik gebruiken wordt.

Door het gehele product in de cluster, dat wil zeggen door schaling aanpassen--opnieuw toe te wijzen om te voldoen aan de huidige vraag van elke service dan gewoon voldoende bronnen nodig voor de piek-vraag van alle services, kunt u de clustergrootte van uw verminderen. In dit eenvoudige voorbeeld verlaagt automatisch schalen het vereiste aantal gehele product van 128 aan de vereiste clustergrootte dubbel knippen 64.

Schalen van het aantal gehele product is een relatief snel bewerking, vereisen van minder dan een minuut, zodat de reactietijd van de service niet ernstig negatief beïnvloed.

> [!NOTE]
> Schalen van een cluster helpt niet bij de latentie van aanvraag. Ter uitoefening moet omhoog schalen Verhoog het aantal geslaagde en afnemen fouten Service niet beschikbaar. 

## <a name="how-to-scale-web-services-on-your-acs-cluster"></a>Webservices schalen op uw ACS-cluster

De cluster-installatieoptie van de model-management CLI standaard configureert twee agents en een schil in uw omgeving. Dit wordt ook de CLI Kubernetes geïnstalleerd.

U kunt de webservices die u hebt geïmplementeerd op ACS door aan te passen schalen:

* Het aantal knooppunten in het cluster agent
* Het aantal Kubernetes schil replica's die worden uitgevoerd op knooppunten van de agent

### <a name="scaling-the-number-of-nodes-in-the-cluster"></a>Het aantal knooppunten schalen in het cluster

De volgende opdracht stelt het aantal knooppunten van de agent in het cluster:

```
az acs scale -g <resource group> -n <cluster name> --new-agent-count <new scale>
```

Dit duurt enkele minuten om te voltooien. Zie voor meer informatie over het schalen van het aantal knooppunten in het cluster [agent knooppunten in een Container Service-cluster schalen](https://docs.microsoft.com/azure/container-service/container-service-scale).

### <a name="scaling-the-number-of-kubernetes-pod-replicas-in-a-cluster"></a>Het aantal Kubernetes schil replica's schalen in een cluster
 
U kunt het aantal schil replica's die zijn toegewezen aan het cluster met behulp van de Azure Machine Learning CLI of [Kubernetes dashboard] schalen (https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

Zie voor meer informatie over Kubernetes replica gehele product de [Kubernetes gehele product](https://kubernetes.io/docs/concepts/workloads/pods/pod/) documentatie.

#### <a name="scaling-a-cluster-with-the-azure-machine-learning-cli"></a>Schalen van een cluster met de Azure Machine Learning CLI

Er zijn twee manieren schalen van een cluster met behulp van de CLI:

- Automatisch schalen
- Statische schaal

Automatisch schalen is standaard actief wanneer de service wordt gemaakt en in de meeste gevallen de voorkeursmethode voor vergroten/verkleinen is.

##### <a name="autoscale"></a>Automatisch schalen

De volgende opdracht wordt automatisch schalen en stelt het minimum en maximum aantal replica's voor de service.

```
az ml service update realtime -i <service id> --autoscale-enabled true --autoscale-min-replicas <positive number> --autoscale-max-replicas <positive number>
```

Bijvoorbeeld: instelling `autoscale-min-replicas` tot en met 5 vijf replica's maakt. Om een optimale nummer voor de webservice aankomen, het aantal waarden zoals 10 instellen en het aantal foutberichten 503 bewaken. Pas het aantal dienovereenkomstig.


| Parameternaam | Type | Beschrijving |
|--------------------|--------------------|--------------------|
| `autoscale-enabled` | Booleaanse waarde | Hiermee geeft u op of automatisch schalen is ingeschakeld. Standaard: true |
| `autoscale-min-replicas` | geheel getal | Hiermee geeft u het minimum aantal gehele product. Moet 0 of hoger. Standaard: 1 |
| `autoscale-max-replicas` | geheel getal | Hiermee geeft u het maximum aantal gehele product. Moet 1 of hoger. Als u automatisch schalen-max-replica's kleiner is dan automatisch schalen-min-replica's, worden automatisch schalen-max-replica's genegeerd. Standaard: 10 |
| `autoscale-refresh-period-seconds` | geheel getal | Hiermee geeft u de duur in seconden tussen automatisch schalen wordt vernieuwd. Standaard: 1 |
| `autoscale-target-utilization` | geheel getal | Hiermee geeft u het percentage gebruik die doelen voor automatisch schalen, tussen 1 en 100 liggen. Standaard: 70 |

Automatisch schalen werkt om te controleren of de volgende twee omstandigheden:

1. Het gebruik van het doel wordt voldaan
2. Schalen nooit groter is dan de minimale en maximale instellingen

Services in een cluster zijn geconfigureerd voor het cluster-resources. Een service geschaald vergroten het Resourcegebruik van de cluster als het aantal aanvragen per tweede (RPS) toeneemt en resources naarmate de RPS wordt traag worden vrijgegeven. Cluster-resources wordt op verzoek worden verkregen, zolang deze middelen aanwezig zijn voor de service te verkrijgen.

Zie voor meer informatie over het gebruik van de parameters voor automatisch schalen die de [Model Management opdrachtregelinterface verwijzing](model-management-cli-reference.md) documentatie.

##### <a name="static-scale"></a>Statische schaal

In het algemeen moet statische schalen worden vermeden, omdat de cluster grootte vermindering van automatisch schalen niet mogelijk. Toch in sommige situaties kan statische schalen worden gesteld. Bijvoorbeeld, wanneer een cluster is toegewezen aan een service, biedt automatisch schalen geen voordeel; alle clusterbronnen moeten worden toegewezen aan die service.

Als u wilt schalen statisch een cluster, moet u automatisch schalen uitgeschakeld. Automatisch geschaald met de volgende opdracht uit te schakelen:

```
az ml service update realtime -i <service id> --autoscale-enabled false
```

Na het uitschakelen van automatisch schalen, wordt het aantal replica's voor een service rechtstreeks geschaald door de volgende opdracht.

```
az ml service update realtime -i <service id> -z <replica count>
```
 
Zie voor meer informatie over het schalen van het aantal knooppunten in het cluster Scale agent knooppunten in een Container Service-cluster.

#### <a name="scaling-number-of-replicas-using-the-kubernetes-dashboard"></a>Het aantal replica's met het dashboard Kubernetes is vergroten/verkleinen

Voer op de opdrachtregel:

```
kubectl proxy
```

In Windows, de installatielocatie Kubernetes niet automatisch toegevoegd aan het pad. Ga eerst naar de installatiemap:

```
c:\users\<user name>\bin
```

Zodra u de opdracht uitvoert, ziet u het volgende bericht:

```
Starting to serve on 127.0.0.1:8001
```

Als de poort al gebruikt wordt, ziet u een bericht dat lijkt op het volgende voorbeeld:

```
F0612 21:49:22.459111   59621 proxy.go:137] listen tcp 127.0.0.1:8001: bind: address already in use
```

Kunt u een alternatieve poort nummer met behulp van de *--poort* parameter.

```
kubectl proxy --port=8010
Starting to serve on 127.0.0.1:8010
```

Zodra u de dashboardserver hebt gestart, open een browser en voer de volgende URL:

```
127.0.0.1:<port number>/ui
```

Klik in het hoofdvenster van dashboard op **implementaties** op de linkernavigatiebalk. Als het navigatiedeelvenster niet wordt weergegeven, selecteert u dit pictogram ![Menu die bestaan uit drie korte horizontale lijnen](media/how-to-scale-clusters/icon-hamburger.png) in de linkerbovenhoek.

Zoek de implementatie als wilt wijzigen en klik op dit pictogram ![pictogram die bestaan uit drie verticale punten](media/how-to-scale-clusters/icon-kebab.png) aan de rechterkant en klik vervolgens op **weergeven/edi YAML**.

Zoek op het scherm van de implementatie bewerken de *spec* knooppunt wijzigen de *replica's* waarde en klik op **Update**.
