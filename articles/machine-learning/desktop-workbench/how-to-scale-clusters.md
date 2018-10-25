---
title: Hoe u een Azure Container Service-cluster kunt schalen voor Machine Learning | Microsoft Docs
description: Schalen van een ACS-cluster - automatische schalings- en statische schalen; het aantal knooppunten schalen in het cluster
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/04/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 9688b9ba305a2eb59b80b02c0b41a7f4855dd051
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024559"
---
# <a name="scaling-the-cluster-to-manage-web-service-throughput"></a>Het cluster voor het beheren van web service doorvoer schalen

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


## <a name="why-scale-the-cluster"></a>Waarom het cluster schalen?

Schalen van het cluster Azure Container Service (ACS) is een effectieve manier om te optimaliseren van de service-doorvoer terwijl de clustergrootte tot een minimum beperkt om kosten te beperken. 

Voor meer informatie over automatisch schalen, houd rekening met het volgende voorbeeld van een cluster met drie webservices:

![Voorbeeld: Drie services op een cluster](media/how-to-scale-clusters/three-services.png)

De services hebben verschillende piekvraag: 1-Service (blauwe lijn) 40 schillen aan grote vraag is vereist, Service 2 (oranje regel) vereist 38 bij piek en Service 3 (grijze lijn) 50 bij piek vereist. Als u de benodigde piek-capaciteit voor elke service afzonderlijk reserveren, dit cluster moeten er ten minste 40 + 38 + 50 = 128 schillen totaal.

Maar houd rekening met het gebruik van de werkelijke pod op elk gewenst moment in-time, vertegenwoordigd door de zwarte stippellijn in de grafiek. In dit geval de *hoogste aantal schillen gebruikt op elk willekeurig moment* is 64, die om 20:00 wanneer de Service 3 bij piek is optreedt. Op dit moment Service 3 50 schillen gebruikt, maar slechts 9 schillen maakt gebruik van Service 2 en 1-Service gebruikt alleen 5. Denk eraan dat dit is *piekgebruik* voor dit cluster. Dit betekent dat op geen enkel moment maakt het cluster gebruik van meer dan 64 schillen--de helft van de berekende vereiste van 128 schillen voor de drie services onafhankelijk worden geschaald voor piekgebruik.

Door het opnieuw toewijzen van schillen in de cluster, dat wil zeggen door schaling aanpassen, om te voldoen aan de huidige vraag van elke service in plaats van gewoon voldoende bronnen zijn vereist voor de grote vraag van alle services, kunt u de clustergrootte van uw verminderen. In dit eenvoudige voorbeeld wordt deze automatisch schalen het vereiste aantal schillen van 128 naar 64, de vereiste clustergrootte knippen in de helft.

Het aantal schillen schalen is een relatief snel bewerking, vereisen minder dan een minuut, zodat de reactietijd van de service is niet ernstig beïnvloed.

> [!NOTE]
> Een cluster schalen helpt niet met problemen met latentie. Voor uitoefening, moet omhoog verhogen van het aantal geslaagde pogingen en verlagen van de Service niet beschikbaar-fouten. 

## <a name="how-to-scale-web-services-on-your-acs-cluster"></a>Web-services op uw ACS-cluster schalen

De cluster-installatieoptie van het Modelbeheer-CLI standaard configureert twee agents en een schil in uw omgeving. Het installeert ook de Kubernetes-CLI.

De webservices die u hebt geïmplementeerd op ACS door aan te passen, kunt u schalen:

* Het aantal agentknooppunten in het cluster
* Het aantal Kubernetes pod replica's die worden uitgevoerd op de knooppunten van de agent

### <a name="scaling-the-number-of-nodes-in-the-cluster"></a>Het aantal knooppunten schalen in het cluster

De volgende opdracht stelt het aantal agentknooppunten in het cluster:

```
az acs scale -g <resource group> -n <cluster name> --new-agent-count <new scale>
```

Het duurt enkele minuten voordat dit is voltooid. Zie voor meer informatie over het aantal knooppunten in het cluster schalen [agentknooppunten in een Container Service-cluster schalen](https://docs.microsoft.com/azure/container-service/container-service-scale).

### <a name="scaling-the-number-of-kubernetes-pod-replicas-in-a-cluster"></a>Het aantal Kubernetes pod-replica's schalen in een cluster
 
U kunt het aantal schilreplica's die zijn toegewezen aan het cluster met behulp van de Azure Machine Learning CLI schalen of het [Kubernetes-dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

Zie voor meer informatie over Kubernetes-schillen voor replica's de [Kubernetes-schillen](https://kubernetes.io/docs/concepts/workloads/pods/pod/) documentatie.

#### <a name="scaling-a-cluster-with-the-azure-machine-learning-cli"></a>Schalen van een cluster met de Azure Machine Learning CLI

Er zijn twee manieren voor het schalen van een cluster met behulp van de CLI:

- Automatisch schalen
- Statische schaal

Automatisch schalen is standaard actief wanneer de service wordt gemaakt en in de meeste gevallen de voorkeursmethode voor vergroten/verkleinen is.

##### <a name="autoscale"></a>Automatisch schalen

De volgende opdracht kunt u automatisch schalen en stelt het minimum en maximum aantal replica's voor de service.

```
az ml service update realtime -i <service id> --autoscale-enabled true --autoscale-min-replicas <positive number> --autoscale-max-replicas <positive number>
```

Bijvoorbeeld, als `autoscale-min-replicas` tot en met 5 maakt vijf replica's. Als u wilt een optimale nummer voor de webservice aankomen, stelt de waarde in op de waarden zoals 10 en het aantal 503 foutberichten bewaken. Pas het aantal dienovereenkomstig.


| Parameternaam | Type | Beschrijving |
|--------------------|--------------------|--------------------|
| `autoscale-enabled` | booleaans | Hiermee geeft u op of voor automatisch schalen is ingeschakeld. Standaard: true |
| `autoscale-min-replicas` | geheel getal | Hiermee geeft u het minimale aantal schillen. Moet 0 of hoger zijn. Standaard: 1 |
| `autoscale-max-replicas` | geheel getal | Hiermee geeft u het maximum aantal schillen. Moet 1 of hoger. Als automatisch schalen-max-replica's is kleiner dan voor automatisch schalen-min-replica's, worden automatisch schalen-max-replica's genegeerd. Standaard: 10 |
| `autoscale-refresh-period-seconds` | geheel getal | Hiermee geeft u de duur in seconden tussen het vernieuwen van de functie voor automatisch schalen. Standaard: 1 |
| `autoscale-target-utilization` | geheel getal | Hiermee geeft u het processorpercentagegebruik deze doelen voor automatisch schalen, tussen 1 en 100 liggen. Standaard: 70 |

Automatisch schalen werkt om te controleren of de volgende twee omstandigheden:

1. Het gebruik van de doel wordt voldaan
2. Schalen nooit groter is dan de minimale en maximale-instellingen

Services in een cluster concurreren om resources-cluster. Een service autoscaled wordt het gebruik van cluster resource worden groter naarmate de aanvragen per seconde (RPS) toeneemt en resources als de RPS-vermindert langzaam brengt. Clusterbronnen wordt op aanvraag worden verkregen, zolang deze middelen bestaan voor de service aan te schaffen.

Zie voor meer informatie over het gebruik van de parameters voor automatisch schalen de [naslaginformatie over objectmodel Management Command Line Interface](model-management-cli-reference.md) documentatie.

##### <a name="static-scale"></a>Statische schaal

In het algemeen moet statische schalen worden vermeden, aangezien deze niet toe de cluster grootte vermindering van automatisch schalen dat staat. Zodat in sommige gevallen kan statische schalen worden gesteld. Bijvoorbeeld, als een cluster is toegewezen aan één service, biedt automatisch schalen geen voordelen; alle clusterresources moeten worden toegewezen aan die service.

Om te schalen statisch een cluster, moet u automatisch schalen uitschakelen. Automatisch schalen met de volgende opdracht uit te schakelen:

```
az ml service update realtime -i <service id> --autoscale-enabled false
```

Na het uitschakelen van automatisch schalen, wordt het aantal replica's voor een service rechtstreeks geschaald door de volgende opdracht uit.

```
az ml service update realtime -i <service id> -z <replica count>
```
 
Zie voor meer informatie over het aantal knooppunten in het cluster schalen agentknooppunten schalen in een Container Service-cluster.

#### <a name="scaling-number-of-replicas-using-the-kubernetes-dashboard"></a>Vergroten/verkleinen aantal replica's met behulp van het Kubernetes-dashboard

Voer op de opdrachtregel in:

```
kubectl proxy
```

Op Windows, is de installatielocatie van Kubernetes niet automatisch toegevoegd aan het pad. Ga eerst naar de installatiemap:

```
c:\users\<user name>\bin
```

Zodra u de opdracht uitvoert, ziet u het volgende bericht:

```
Starting to serve on 127.0.0.1:8001
```

Als de poort al in gebruik is is, ziet u een bericht dat lijkt op het volgende voorbeeld:

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

Klik in het hoofdscherm van dashboard op **implementaties** op de linker navigatiebalk. Als het navigatiedeelvenster niet wordt weergegeven, selecteert u dit pictogram ![Menu die bestaat uit drie korte horizontale lijnen](media/how-to-scale-clusters/icon-hamburger.png) in de linkerbovenhoek.

Zoek de implementatie als u wilt wijzigen en klik op dit pictogram ![menupictogram die bestaat uit drie verticale puntjes](media/how-to-scale-clusters/icon-kebab.png) aan de rechterkant en klik vervolgens op **weergeven/bewerken YAML**.

Zoek op het scherm voor bewerken-implementatie, de *spec* knooppunt, wijzigen de *replica's* waarde en klik op **Update**.
