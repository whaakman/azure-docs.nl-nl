---
title: Concepten - Scale applications in Azure Kubernetes Services (AKS)
description: Meer informatie over het schalen in Azure Kubernetes Service (AKS), met inbegrip van horizontale schillen automatisch schalen, cluster automatisch schalen en de Azure Container Instances-connector.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: c7019eac4edc530de5ef64ba9eb32e8e4994e75b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57245196"
---
# <a name="scaling-options-for-applications-in-azure-kubernetes-service-aks"></a>Schaalopties voor toepassingen in Azure Kubernetes Service (AKS)

Als u toepassingen in Azure Kubernetes Service (AKS) uitvoert, moet u het vergroten of verkleinen van de hoeveelheid rekenresources. Als het aantal exemplaren van de toepassing moet u het aantal onderliggende Kubernetes knooppunten moet mogelijk ook wijzigen wijzigen. U moet mogelijk ook een groot aantal exemplaren van een extra toepassing snel inrichten.

Dit artikel bevat de belangrijkste concepten waarmee u kunt toepassingen schalen in AKS:

- [Handmatig schalen](#manually-scale-pods-or-nodes)
- [Horizontale schillen automatisch schalen (HPA)](#horizontal-pod-autoscaler)
- [Automatisch schalen van cluster](#cluster-autoscaler)
- [Integratie van Azure Container exemplaar (ACI) met AKS](#burst-to-azure-container-instances)

## <a name="manually-scale-pods-or-nodes"></a>Handmatig schalen van schillen of knooppunten

U kunt handmatig schalen replica's (schillen) en knooppunten te testen hoe uw toepassing reageert op een wijziging in de beschikbare resources en status. Handmatig schalen van resources kunt u een vaste hoeveelheid resources om te gebruiken voor het onderhouden van een vaste prijs, zoals het aantal knooppunten definiëren. Als u wilt handmatig schalen, definieert u de replica of aantal knooppunten en de planning van de Kubernetes API extra schillen of leegmaken van knooppunten.

Aan de slag met het handmatig schalen van schillen en knooppunten zien [toepassingen schalen in AKS][aks-scale].

## <a name="horizontal-pod-autoscaler"></a>Horizontale schillen automatisch schalen

Kubernetes gebruikt het horizontale schillen automatisch schalen (HPA) om de vraag naar resources controleren en automatisch schalen van het aantal replica's. Standaard controleert het horizontale schillen automatisch schalen de metrische gegevens API elke 30 seconden voor eventuele vereiste wijzigingen in aantal replica's. Wanneer er wijzigingen nodig zijn, het aantal replica's of afneemt dienovereenkomstig. Horizontale schillen automatisch schalen werkt met AKS-clusters die de Server van de metrische gegevens voor Kubernetes 1.8 + hebt geïmplementeerd.

![Kubernetes horizontale u automatisch schalen](media/concepts-scale/horizontal-pod-autoscaling.png)

Wanneer u het horizontale schillen automatisch schalen voor een bepaalde implementatie configureert, definieert u het minimum en maximum aantal replica's die kunnen worden uitgevoerd. U definieert ook de metrische gegevens om te controleren en eventuele vergroten/verkleinen besluiten baseren op, zoals CPU-gebruik.

Als u wilt beginnen met het horizontale schillen automatisch schalen in AKS, Zie [schillen automatisch schalen in AKS][aks-hpa].

### <a name="cooldown-of-scaling-events"></a>Afkoeltijd van het schalen van gebeurtenissen

Als het horizontale schillen automatisch schalen de metrische gegevens-API elke 30 seconden wordt gecontroleerd, vorige schaalgebeurtenissen mogelijk niet hebt voltooid voordat een andere op uitgevoerd. Dit gedrag kan ertoe leiden dat het horizontale pod automatisch schalen om te wijzigen van het aantal replica's voordat bleek in de vorige schaalgebeurtenis staat voor het ontvangen van de workload van toepassing en de resource-eisen aan overeenkomstig aanpassen.

Als u wilt deze gebeurtenissen race minimaliseren, kunnen afkoeltijd of vertraging waarden worden ingesteld. Deze waarden bepalen hoe lang het horizontale schillen automatisch schalen moet wachten na een schaalgebeurtenis voordat een andere schaalgebeurtenis kan worden geactiveerd. Dit gedrag kunt de nieuwe replica tellen te laten treden, en de API voor metrische gegevens geven de gedistribueerde werkbelasting. Standaard de vertraging op schaal gebeurtenissen is 3 minuten en de vertraging op schaal omlaag gebeurtenissen is 5 minuten

Mogelijk moet u deze waarden afkoeltijd af te stemmen. De standaardwaarden van de afkoeltijd geven de indruk dat de horizontale schillen automatisch schalen het aantal replica's snel genoeg is niet schalen. Bijvoorbeeld, als u wilt sneller verhoogt het aantal replica's gebruikt, verminderen de `--horizontal-pod-autoscaler-upscale-delay` bij het maken van uw schil horizontale definities van automatisch schalen met behulp van `kubectl`.

## <a name="cluster-autoscaler"></a>Automatisch schalen van cluster

Kubernetes is om te reageren op veranderende vraag pod, een cluster automatisch schalen die Hiermee past u het aantal knooppunten op basis van de aangevraagde compute-resources in de knooppuntgroep. Het cluster automatisch schalen gebruikt opstartparameters voor zaken zoals tijdsintervallen tussen schaalgebeurtenissen en resourcedrempelwaarden voor de. Als de cluster voor automatisch schalen wordt vastgesteld dat er een wijziging vereist is, het aantal knooppunten in uw AKS-cluster- of afneemt dienovereenkomstig. AKS-clusters die gebruikmaken van het cluster automatisch schalen worden uitgevoerd op virtuele-machineschaalsets beheren van de schaal omhoog en omlaag gebeurtenissen van de AKS-knooppunten schalen.

![Kubernetes-cluster automatisch schalen](media/concepts-scale/cluster-autoscaler.png)

Cluster automatisch schalen wordt meestal gebruikt samen met het horizontale schillen automatisch schalen. In combinatie, automatisch schalen horizontaal pod vergroot of verkleint u het aantal schillen op basis van de vraag van toepassing, en het aantal knooppunten van het cluster automatisch schalen wordt aangepast behoefte om uit te voeren die extra schillen dienovereenkomstig.

Als u wilt beginnen met het cluster automatisch schalen in AKS, Zie [Cluster automatisch schalen in AKS][aks-cluster-autoscaler].

### <a name="scale-up-events"></a>Gebeurtenissen opschalen

Als een knooppunt niet voldoende rekenresources om uit te voeren van een aangevraagde schil heeft, kan niet deze pod voortgang van het planningsproces. De schil kan niet worden gestart als aanvullende rekenbronnen beschikbaar binnen de pool knooppunt zijn.

Wanneer het cluster automatisch schalen schillen die door resourcebeperkingen voor knooppunt groep kunnen niet worden gepland kennisgevingen, wordt het aantal knooppunten in het knooppunt pool verhoogd voor de extra rekenresources. Wanneer deze extra knooppunten is geïmplementeerd en beschikbaar zijn voor gebruik binnen de pool knooppunt zijn, worden vervolgens de schillen gepland om uit te voeren op deze.

Als uw toepassing moet schalen, blijven sommige schillen in een status die moet worden gepland totdat de extra knooppunten geïmplementeerd door het cluster automatisch schalen kunnen de geplande schillen accepteren. Voor toepassingen die hoge burst vraag hebt, kunt u schalen met virtuele knooppunten en Azure Container Instances.

### <a name="scale-down-events"></a>Gebeurtenissen verkleinen

Het cluster automatisch schalen controleert ook de schil planning van de status van knooppunten die niet onlangs nieuwe planning aanvragen ontvangen. In dit scenario geeft aan dat de knooppuntgroep meer rekenresources dan vereist is, en dat het aantal knooppunten kan worden verlaagd.

Een knooppunt dat wordt doorgegeven een drempelwaarde niet langer wordt standaard meer 10 minuten nodig is gepland voor verwijdering. Als deze situatie zich voordoet, schillen zijn gepland voor uitvoering op andere knooppunten in de pool knooppunt en het cluster automatisch schalen vermindert het aantal knooppunten.

Uw toepassingen kunnen sommige onderbreking optreden als schillen zijn gepland op verschillende knooppunten wanneer het cluster automatisch schalen het aantal knooppunten vermindert. Om te beperken wordt onderbroken, te voorkomen dat toepassingen die gebruikmaken van een één pod-exemplaar.

## <a name="burst-to-azure-container-instances"></a>Stap over op Azure Container Instances

Als u wilt snel uw AKS-cluster schalen, kunt u met Azure Container Instances (ACI) integreren. Kubernetes heeft ingebouwde onderdelen voor het schalen van het aantal replica- en knooppunt. Als uw toepassing kunnen snel worden geschaald moet, kan het horizontale schillen automatisch schalen echter meer schillen dan kan worden geleverd door de bestaande compute-resources in het knooppunt toepassingen plannen. Als geconfigureerd, in dit scenario vervolgens het cluster automatisch schalen voor het implementeren van extra knooppunten in het knooppunt toepassingen activeren, maar het duurt een paar minuten voor die knooppunten is inrichten en de scheduler Kubernetes schillen uitgevoerd op deze toestaan.

![Kubernetes-burst schalen naar ACI](media/concepts-scale/burst-scaling.png)

ACI kunt u snel containerinstanties zonder de overhead van het aanvullende infrastructuur te implementeren. Wanneer u verbinding met AKS maakt, wordt ACI een beveiligde, logische verlengstuk van uw AKS-cluster. De Virtual Kubelet-onderdeel wordt geïnstalleerd in uw AKS-cluster met daarin ACI als een virtuele Kubernetes-knooppunt. Kubernetes kunt vervolgens schillen die worden uitgevoerd als virtuele knooppunten ACI-instanties, niet als schillen op VM-knooppunten rechtstreeks in uw AKS-cluster plannen.

Uw toepassing vereist geen wijzigingen aan de virtuele-knooppunten gebruiken. Implementaties kunnen schalen via AKS en ACI en zonder vertraging als cluster met automatisch schalen nieuwe knooppunten in uw AKS-cluster implementeert.

Virtuele knooppunten worden geïmplementeerd naar een ander subnet in hetzelfde virtuele netwerk bevinden als uw AKS-cluster. Deze configuratie van het virtuele netwerk kan het verkeer tussen ACI en AKS te worden beveiligd. Als een AKS-cluster is een ACI-exemplaar een veilige, logische compute-resource die is geïsoleerd van andere gebruikers.

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen met het schalen van toepassingen, volgt u eerst de [Snelstartgids voor het maken van een AKS-cluster met de Azure CLI][aks-quickstart]. Vervolgens kunt u beginnen met het handmatig of automatisch schalen van toepassingen in uw AKS-cluster:

- Handmatig schalen [schillen] [ aks-manually-scale-pods] of [knooppunten][aks-manually-scale-nodes]
- Gebruik de [horizontale schillen automatisch schalen][aks-hpa]
- Gebruik de [automatisch schalen van cluster][aks-cluster-autoscaler]

Zie de volgende artikelen voor meer informatie over core Kubernetes en AKS concepten:

- [Kubernetes / AKS-clusters en workloads][aks-concepts-clusters-workloads]
- [Kubernetes / AKS toegang en identiteit][aks-concepts-identity]
- [Kubernetes / AKS-beveiliging][aks-concepts-security]
- [Kubernetes / AKS virtuele netwerken][aks-concepts-network]
- [Kubernetes / AKS-opslag][aks-concepts-storage]

<!-- LINKS - external -->

<!-- LINKS - internal -->
[aks-quickstart]: kubernetes-walkthrough.md
[aks-hpa]: tutorial-kubernetes-scale.md#autoscale-pods
[aks-scale]: tutorial-kubernetes-scale.md
[aks-manually-scale-pods]: tutorial-kubernetes-scale.md#manually-scale-pods
[aks-manually-scale-nodes]: tutorial-kubernetes-scale.md#manually-scale-aks-nodes
[aks-cluster-autoscaler]: autoscaler.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-network]: concepts-network.md
