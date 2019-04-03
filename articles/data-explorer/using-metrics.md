---
title: De prestaties, de status en het gebruik met metrische gegevens over Azure Data Explorer controleren
description: Informatie over het gebruik van Azure Data Explorer metrische gegevens voor het bewaken van de prestaties, de status en het gebruik van het cluster.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: 5252ca8898439b63a8819f6abfd634de0786932b
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851731"
---
# <a name="monitor-azure-data-explorer-performance-health-and-usage-with-metrics"></a>De prestaties, de status en het gebruik met metrische gegevens over Azure Data Explorer controleren

Azure Data Explorer is een snelle, volledig beheerde service voor gegevensanalyses waarmee grote hoeveelheden gegevens van toepassingen, websites, IoT-apparaten en dergelijke in real-time kunnen worden geanalyseerd. Als u Azure Data Explorer wilt gebruiken, maakt u eerst een cluster. Daarna maakt u een of meer databases in het cluster. De volgende stap is het opnemen (laden) van gegevens in een database, zodat u er query's op kunt uitvoeren. Metrische gegevens van Azure Data Explorer bieden KPI's over de status en prestaties van de clusterresources. Gebruik de metrische gegevens die worden beschreven in dit artikel voor het bewaken van Azure Data Explorer clusterstatus en prestaties in uw specifieke scenario als zelfstandige metrische gegevens. U kunt metrische gegevens ook gebruiken als basis voor operationele [Azure-Dashboards](/azure/azure-portal/azure-portal-dashboards) en [Azure-waarschuwingen](/azure/azure-monitor/platform/alerts-metric-overview).

## <a name="prerequisites"></a>Vereisten

* Als u geen Azure-abonnement hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/).

* Maak een [cluster en de database](create-cluster-database-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="using-metrics"></a>Met metrische gegevens

Selecteer in het cluster Azure Data Explorer **metrische gegevens** voor het openen van het deelvenster met metrische gegevens en beginnen met de analyse op uw cluster.

![Metrische gegevens selecteren](media/using-metrics/select-metrics.png)

In het deelvenster met metrische gegevens:

![Deelvenster van de metrische gegevens](media/using-metrics/metrics-pane.png)

1. Voor het maken van een grafiek met metrische gegevens selecteert **Metric** naam en het relevante **aggregatie** per metrisch gegeven als gedetailleerde hieronder. De **Resource** en **metriek Namespace** kleurenkiezer zijn vooraf geselecteerde met uw Azure Data Explorer-cluster.

    **Gegevens** | **Eenheid** | **Aggregatie** | **Beschrijving van metrische gegevens**
    |---|---|---|---|
    | Cache-gebruik | Procent | Avg, Max, Min | Verhouding tussen de vereiste cachegrootte (op basis van het beleid gedefinieerde cache) en de grootte van de totale cache van het cluster (totale grootte van SSD voor gebruikersactiviteit gegeven). Een gemiddelde cache gebruik van 80% of minder is een duurzame status voor een cluster. Als de gemiddelde cache-gebruik hoger is dan 80%, het cluster moet [opgeschaald](manage-cluster-scale-up.md) naar een opslag geoptimaliseerd prijscategorie of [uitgeschaalde](manage-cluster-scale-out.md) meer exemplaren. U kunt ook aan te passen de cache-beleid (minder dagen in de cache). Als cache gebruik meer dan 100% is, de grootte van gegevens in de cache opgeslagen, op basis van het cachebeleid groter is dat de totale grootte van de cache op het cluster. |
    | CPU | Procent | Avg, Max, Min | Verhouding tussen de totale CPU-gebruik en de CPU beschikbaar is op het hele cluster. Er is een gemiddelde CPU van 80% of minder duurzame voor een cluster. De maximumwaarde van CPU is 100%, wat betekent dat er zijn geen extra rekenresources om gegevens te verwerken. Wanneer een cluster is niet goed presteert, controleert u de maximale waarde van de CPU om te bepalen of er specifieke CPU's die zijn geblokkeerd. |
    | Gebeurtenissen die worden verwerkt (voor Event Hubs) | Count | Max, Min, Sum | Totaal aantal gebeurtenissen die door Event Hubs worden verzonden en ontvangen door het cluster. De gebeurtenissen worden onderverdeeld in geweigerd en gebeurtenissen die worden geaccepteerd door de engine voor het cluster. |
    | Opnamelatentie | Seconden | Avg, Max, Min | Latentie van gegevens die zijn opgenomen vanaf het moment dat de gegevens in het cluster is ontvangen totdat deze klaar voor de query is. Opnamelatentie wordt gemeten in seconden. De opname latentieperiode is afhankelijk van het scenario voor gegevensopname. |
    | Opname-resultaat | Count | Count | Totaal aantal opname-bewerkingen die zijn mislukt en is voltooid. Gebruik **toepassen splitsen** maken buckets van slagen en mislukken resultaten.|
    | Opname-gebruik | Procent | Avg, Max, Min | Verhouding tussen de werkelijke hoeveelheid resources die wordt gebruikt voor opname van gegevens en het totaal aan resources toegewezen in het beleid van de capaciteit wilt opnemen. Het standaardbeleid voor capaciteit is niet meer dan 512 gelijktijdige opname bewerkingen of 75% van de clusterbronnen geïnvesteerd in de opname. Gem. opname gebruik van 80% of minder is een duurzame status voor een cluster. Max-waarde van het gebruik van de opname is 100%, wat betekent dat alle cluster opname-mogelijkheid wordt gebruikt en kan leiden tot een opname-wachtrij. |
    | Opname-volume (in MB) | Count | Max, Min, Sum | De totale grootte van de gegevens die worden opgenomen in het cluster (in MB). De eenheden zijn het aantal MB's opgenomen gegevens vóór compressie. |
    | Actief houden | Count | Gem | Houdt de reactiesnelheid van het cluster. Een cluster met heel snel reageert, retourneert de waarde 1 en een cluster geblokkeerd of niet-verbonden retourneert 0. |
    | Queryduur | Seconden | Count, Avg, Min, Max, Sum | Totale tijd totdat de queryresultaten worden ontvangen. |
    | | | |

    Aanvullende informatie met betrekking tot [ondersteunde metrische gegevens van Azure Data Explorer-cluster](/azure/azure-monitor/platform/metrics-supported#microsoftkustoclusters)

2. Selecteer de **metrische waarde toevoegen** om weer te geven meerdere metrische gegevens in dezelfde grafiek getekend.
3. Selecteer de **+ nieuwe grafiek** om weer te geven meerdere diagrammen in één weergave.
4. Gebruik de tijdkiezer om te wijzigen van het tijdsbereik (standaard: afgelopen 24 uur).
5. Gebruik [ **filter toevoegen** en **toepassen splitsen** ](/azure/azure-monitor/platform/metrics-getting-started#apply-dimension-filters-and-splitting) voor metrische gegevens die dimensies hebben.
6. Selecteer **vastmaken aan dashboard** uw configuratie van de grafiek toevoegen aan de dashboards, zodat u deze opnieuw kunt bekijken.
7. Stel **nieuwe waarschuwingsregel** voor het visualiseren van uw metrische gegevens met behulp van de set criteria. De nieuwe waarschuwingsregel bevat de doelresource, metrische gegevens, opsplitsen en filterdimensies van de grafiek. Wijzig deze instellingen in de [waarschuwingsregel maken van het deelvenster](/azure/azure-monitor/platform/metrics-charts#create-alert-rules).

Als u meer informatie over het gebruik van de [Metrics Explorer](/azure/azure-monitor/platform/metrics-getting-started).


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Snelstart: query's uitvoeren op gegevens in Azure Data Explorer](web-query-data.md)
