---
title: Query uitvoeren op gegevens in Azure Monitor met Azure Data Explorer (Preview)
description: In dit onderwerp, kunt u gegevens in Azure Monitor opvragen door te maken van een Azure Data Explorer-proxy voor vectorproduct query's met Application Insights en Log Analytics
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: f363e59e6faa6b115eb40a2a5d35432f02299d52
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800209"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Query uitvoeren op gegevens in Azure Monitor met Azure Data Explorer (Preview)

Het cluster in Azure Data Explorer-proxy (ADX Proxy) is een entiteit waarmee u kunt het uitvoeren van query's vectorproduct tussen Azure-Data Explorer, [Application Insights (AI)](/azure/azure-monitor/app/app-insights-overview), en [Log Analytics (LA)](/azure/azure-monitor/platform/data-platform-logs) in de [Azure Monitor](/azure/azure-monitor/) service. U kunt Azure Monitor Log Analytics-werkruimten of Application Insights-apps toewijzen als een proxy-cluster. U kunt vervolgens de proxy-cluster met behulp van hulpprogramma's voor Azure Data Explorer query en hiernaar wordt verwezen in een cross-cluster-query. Het artikel wordt beschreven hoe u verbinding maken met een proxy-cluster, een proxy-cluster toevoegen aan Azure Data Explorer Web-UI en query's uitvoeren op uw AI-apps of LA werkruimten in Azure Data Explorer.

De stroom van Azure Data Explorer-proxy: 

![ADX proxy stroom](media/adx-proxy/adx-proxy-flow.png)

## <a name="prerequisites"></a>Vereisten

> [!NOTE]
> De Proxy ADX is in preview-modus. Als u wilt deze functie inschakelt, neem contact op met de [ADXProxy](mailto:adxproxy@microsoft.com) team.

## <a name="connect-to-the-proxy"></a>Verbinding maken met de proxy

1. Controleer of uw eigen cluster Azure Data Explorer (zoals *help* cluster) wordt weergegeven in het menu links, voordat u verbinding met uw Log Analytics of Application Insights-cluster maakt.

    ![Systeemeigen ADX-cluster](media/adx-proxy/web-ui-help-cluster.png)

1. In de gebruikersinterface van Azure Data Explorer (https://dataexplorer.azure.com/clusters), selecteer **Cluster toevoegen**.

1. In de **Cluster toevoegen** venster:

    * De URL toevoegen aan de LA of AI-cluster. Bijvoorbeeld: `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`

    * Selecteer **Toevoegen**.

    ![Cluster toevoegen](media/adx-proxy/add-cluster.png)

    Als u een verbinding aan meer dan één proxy-cluster toevoegen, geeft u elk een andere naam. Anders hebt ze alle dezelfde naam in het linkerdeelvenster.

1. Nadat de verbinding tot stand is gebracht, wordt uw LA of AI-cluster wordt weergegeven in het linkerdeelvenster met de systeemeigen ADX-cluster. 

    ![Log Analytics en Azure Data Explorer-clusters](media/adx-proxy/la-adx-clusters.png)

## <a name="run-queries"></a>Query's uitvoeren

U kunt Kusto Explorer, ADX Internet Explorer, Jupyter Kqlmagic of REST-API gebruiken om op te vragen van de proxy-clusters. 

> [!TIP]
> * De naam van database moet dezelfde naam als de resource die is opgegeven in het cluster proxy. Namen zijn hoofdlettergevoelig.
> * Cross-cluster query's in, zorg ervoor dat de [naamgeving van apps en werkruimten](#application-insights-app-and-log-analytics-workspace-names) juist is.

### <a name="query-against-the-native-azure-data-explorer-cluster"></a>Query op basis van het cluster met systeemeigen Azure-Data Explorer 

Query's uitvoeren op uw Azure Data Explorer-cluster (zoals *StormEvents* in tabel *help* cluster). Wanneer de query wordt uitgevoerd, controleert u of dat uw systeemeigen Azure-Data Explorer-cluster is geselecteerd in het linkerdeelvenster.

```kusto
StormEvents | take 10 // Demonstrate query through the native ADX cluster
```

![Query uitvoeren op tabel StormEvents](media/adx-proxy/query-adx.png)

### <a name="query-against-your-la-or-ai-cluster"></a>Query op basis van uw LA of AI-cluster

Wanneer u query's op uw cluster LA of AL hebt uitgevoerd, controleert u of dat uw LA of AI-cluster is geselecteerd in het linkerdeelvenster. 

```kusto
Perf | take 10 // Demonstrate query through the proxy on the LA workspace
```

![Query LA werkruimte](media/adx-proxy/query-la.png)

### <a name="query-your-la-or-ai-cluster-from-the-adx-proxy"></a>Query uitvoeren op uw LA of AI-cluster van de proxy ADX  

Wanneer u query's op uw LA of AI-cluster van de proxy uitvoeren, controleert u of dat uw eigen cluster ADX is geselecteerd in het linkerdeelvenster. Het volgende voorbeeld ziet u een query van de LA-werkruimte met behulp van de systeemeigen ADX-cluster

```kusto
cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name').Perf
| take 10 
```

![Query van Azure Data Explorer-proxy](media/adx-proxy/query-adx-proxy.png)

### <a name="cross-query-of-la-or-ai-cluster-and-the-adx-cluster-from-the-adx-proxy"></a>Databaseoverschrijdende query van LA of AI-cluster en het cluster ADX van de proxy ADX 

Wanneer u meerdere cluster query's van de proxy uitvoeren, controleert u of dat uw eigen cluster ADX is geselecteerd in het linkerdeelvenster. De volgende voorbeelden demonstreren combineren ADX tabellen cluster (met behulp van `union`) met LA werkruimte.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10 
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <ADX table>, cluster(CL1).database(<workspace-name>).<table name>
```

![Databaseoverschrijdende query van de Azure Data Explorer-proxy](media/adx-proxy/cross-query-adx-proxy.png)

Met behulp van de [ `join` operator](/azure/kusto/query/joinoperator), in plaats van union, mogelijk een hint uit te voeren op een systeemeigen Azure-Data Explorer-cluster (en niet op de proxy). 

## <a name="additional-syntax-examples"></a>Aanvullende syntaxis voor voorbeelden

De volgende syntaxis van de opties zijn beschikbaar bij het aanroepen van de clusters Application Insights (AI) of Log Analytics (LA):

|Van de syntaxisbeschrijving  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Database binnen een cluster met alleen de gedefinieerde resources in dit abonnement (**aanbevolen voor cross-query's voor cluster**) |   cluster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | cluster (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| Cluster met alle apps/werkruimten in dit abonnement    |     cluster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    cluster (`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|Cluster met alle apps/werkruimten in het abonnement en lid zijn van deze resourcegroep    |   cluster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    cluster (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|Cluster met alleen de gedefinieerde resources in dit abonnement      |    cluster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  cluster (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

### <a name="application-insights-app-and-log-analytics-workspace-names"></a>Application Insights-app en de namen van de Log Analytics-werkruimte

* Als namen van speciale tekens bevatten, worden ze vervangen door URL-codering in de naam van de proxy-cluster. 
* Als de namen bevatten tekens die niet voldoen aan [KQL id naam regels](/azure/kusto/query/schema-entities/entity-names), de zijn vervangen door de streepje **-** teken.

## <a name="next-steps"></a>Volgende stappen

[Query's schrijven](write-queries.md)