---
title: AKS-cluster-prestaties bewaken met Azure Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt bekijken en analyseren van de prestaties en het logboek gegevens met Azure Monitor voor containers.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2018
ms.author: magoedte
ms.openlocfilehash: 588dcabb35660c860f3d96dd03c82ed95a1d4d5b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087203"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Inzicht in prestaties in een AKS-cluster met Azure Monitor voor containers 
Met Azure Monitor voor containers, kunt u de van prestatiegrafieken en de status voor het bewaken van de werkbelasting van uw Azure Kubernetes Service (AKS)-clusters vanuit twee perspectieven, rechtstreeks vanuit een AKS-cluster of alle AKS-clusters in een abonnement van Azure Monitor. Weergeven van Azure Container Instances (ACI) is ook mogelijk wanneer u een specifieke AKS-cluster bewaken.

Dit artikel krijgt u inzicht in de ervaring tussen de twee perspectieven en hoe Hiermee kunt u snel beoordelen, onderzoeken en oplossen van problemen die zijn gedetecteerd.

Zie voor meer informatie over het inschakelen van Azure Monitor voor containers [ingebouwde Azure-Monitor voor containers](container-insights-onboard.md).

Azure Monitor biedt een cluster met meerdere weergave voor de status van alle bewaakte AKS clusters die zijn geïmplementeerd op verschillende resourcegroepen in uw abonnementen.  Hier ziet u AKS clusters gedetecteerd die niet worden bewaakt door de oplossing. U kunt onmiddellijk clusterstatus begrijpen en hier kunt u inzoomen op de pagina van de prestaties knooppunt en de domeincontroller of Ga om te zien worden prestatiegrafieken weergegeven voor het cluster.  Voor clusters met AKS gedetecteerd en geïdentificeerd als niet-bewaakt, kunt u de bewaking voor dat cluster op elk gewenst moment inschakelen.  

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com). 

## <a name="multi-cluster-view-from-azure-monitor"></a>Weergave van meerdere cluster van Azure Monitor 
Als u wilt weergeven van de status van alle AKS clusters die zijn geïmplementeerd, selecteert u **Monitor** in het linkervenster in Azure portal.  Onder de **Insights** sectie, selecteer **Containers**.  

![Voorbeeld van meerdere clusterdashboard met Azure Monitor](./media/container-insights-analyze/azmon-containers-multiview-1018.png)

Op de **bewaakt clusters** tabblad, kunt u de volgende informatie:

1. Het aantal clusters hebben een status Kritiek of slecht ten opzichte van het aantal zijn in orde is of niet reporting (aangeduid als een onbekende status)?
1. Zijn alle mijn [Azure Kubernetes-Engine (AKS-engine)](https://github.com/Azure/aks-engine) implementaties in orde?
1. Het aantal knooppunten, gebruiker en system schillen per cluster worden geïmplementeerd.  

De health-statussen inbegrepen zijn: 

* **In orde** : Er zijn geen problemen gedetecteerd voor de virtuele machine en deze werkt zoals vereist. 
* **Kritieke** : een of meer kritieke problemen worden ontdekt, die moeten worden opgelost als u wilt herstellen van normale operationele status, zoals verwacht.
* **Waarschuwing** -worden een of meer problemen gedetecteerd die moeten worden opgelost of de voorwaarde van de status kritiek kan worden.
* **Onbekende** : als de service kan niet een verbinding met het knooppunt of pod, de status is gewijzigd in een onbekende status te maken.
* **Niet gevonden** : een van beide de werkruimte, de resourcegroep of abonnement met de werkruimte voor deze oplossing, is verwijderd.
* **Niet-geautoriseerde** -gebruiker beschikt niet over vereiste machtigingen om de gegevens in de werkruimte te lezen.
* **Fout** -er is een fout opgetreden tijdens het lezen van gegevens van de werkruimte.
* **Mis geconfigureerd** -Azure-Monitor voor containers is niet juist geconfigureerd in de opgegeven werkruimte.
* **Er zijn geen gegevens** -gegevens niet naar de werkruimte heeft gerapporteerd in de laatste 30 minuten.

Status berekent de algehele status van het cluster als *slechtste van*"de drie statussen, met één uitzondering: als een van de drie statussen *onbekende*, ziet u algemene clusterstatus **onbekend**.  

De volgende tabel bevat een verdeling van de berekening van de statussen voor een bewaakte-cluster in de weergave meerdere cluster beheren.

| |Status |Beschikbaarheid |  
|-------|-------|-----------------|  
|**Gebruiker Pod**| | |  
| |In orde |100% |  
| |Waarschuwing |90 - 99% |  
| |Kritiek |< 90% |  
| |Onbekend |Als niet wordt vermeld in de afgelopen 30 minuten |  
|**Systeem-schil**| | |  
| |In orde |100% |
| |Waarschuwing |N/A |
| |Kritiek |< 100% |
| |Onbekend |Als niet wordt vermeld in de afgelopen 30 minuten |
|**Node** | | |
| |In orde |> 85% |
| |Waarschuwing |60 - 84% |
| |Kritiek |< 60% |
| |Onbekend |Als niet wordt vermeld in de afgelopen 30 minuten |

In de lijst van clusters, kunt u inzoomen op de **Cluster** pagina door te klikken op de naam van het cluster, naar de **knooppunten** prestaties pagina door te klikken op het totaal van knooppunten in de **knooppunten** kolom voor die specifieke cluster of inzoomen omlaag naar de **Controllers** prestaties pagina door te klikken op het totaal van **gebruiker schillen** of **System schillen**kolom.   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Prestaties weergeven rechtstreeks vanuit een AKS-cluster
Toegang tot Azure Monitor voor containers is beschikbaar rechtstreeks vanuit een AKS-cluster door te selecteren **Insights** in het linkerdeelvenster. Informatie bekijken over uw AKS-cluster is onderverdeeld in vier perspectieven:

- Cluster
- Knooppunten 
- Controllers  
- Containers

De standaardpagina geopend wanneer u klikt op **Insights** is **Cluster**, en omvat vier prestaties lijndiagrammen weergeven van de van de belangrijkste metrische prestatiegegevens van uw cluster. 

![Voorbeeld van de prestatiegrafieken weergegeven op het tabblad Cluster](./media/container-insights-analyze/containers-cluster-perfview.png)

Het diagram van de prestaties worden vier metrische gegevens voor prestaties weergegeven:

- **Knooppunt CPU-gebruik&nbsp;%**: een samengevoegde perspectief van de CPU-gebruik voor het hele cluster. U kunt filteren dat de resultaten voor het tijdsbereik selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de kiezer percentielen boven de grafiek, ofwel afzonderlijk of gecombineerd. 
- **Geheugengebruik knooppunt&nbsp;%**: een samengevoegde perspectief van geheugengebruik voor het hele cluster. U kunt filteren dat de resultaten voor het tijdsbereik selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de kiezer percentielen boven de grafiek, ofwel afzonderlijk of gecombineerd. 
- **Aantal knooppunten**: een aantal knooppunten en de status van Kubernetes. Status van de clusterknooppunten weergegeven zijn *alle*, *gereed*, en *niet gereed* en kunnen worden gefilterd afzonderlijk of in de kiezer boven de grafiek worden gecombineerd. 
- **Activiteit pod aantal**: een pod aantal en de status van Kubernetes. Status van de vertegenwoordigd schillen zijn *alle*, *in behandeling*, *uitgevoerd*, en *onbekende* en kunnen worden gefilterd, afzonderlijk of gecombineerd de kiezer boven de grafiek. 

Als u overschakelt naar **knooppunten**, **Controllers**, en **Containers** tabblad automatisch weergegeven aan de rechterkant van de pagina is het deelvenster met de eigenschap.  Het bevat de eigenschappen van het item dat is geselecteerd, met inbegrip van de labels die u definieert om te organiseren van Kubernetes-objecten. Klik op de **>>** koppelen in het deelvenster aan view\hide het deelvenster.  

![Voorbeeld van Kubernetes perspectieven het deelvenster met eigenschappen](./media/container-insights-analyze/perspectives-preview-pane-01.png)

Als u de objecten in de hiërarchie wilt uitbreiden, de eigenschappen van deelvenster updates op basis van het geselecteerde object. In het deelvenster kunt u de Kubernetes-gebeurtenissen met vooraf gedefinieerde zoekopdrachten ook weergeven door te klikken op de **weergave Kubernetes gebeurtenislogboeken** koppelen aan de bovenkant van het deelvenster. Zie voor meer informatie over het weergeven van Kubernetes-logboekgegevens [zoeken in Logboeken om gegevens te analyseren](#search-logs-to-analyze-data). Wanneer u uw containers in de **Containers** weergave ziet u container-Logboeken in realtime. Zie voor meer informatie over deze functie en de vereiste configuratie voor het verlenen en beheren van toegang, [container logboeken realtime met Azure Monitor voor containers weergeven](container-insights-live-logs.md). 

Gebruik de **+ Filter toevoegen** optie vanaf de bovenkant van de pagina voor het filteren van de resultaten voor de weergave door **Service**, **knooppunt**, of **Namespace** en na u selecteert de filterbereik, u vervolgens selecteren van een van de waarden in de **waard(en) selecteren** veld.  Nadat het filter is geconfigureerd, wordt deze wereldwijd toegepast tijdens het bekijken van een perspectief van het AKS-cluster.  De formule ondersteunt alleen het gelijkteken.  U kunt extra filters op de eerste verder beperken van de resultaten kunt toevoegen.  Bijvoorbeeld, als u een filter door opgegeven **knooppunt**, het tweede filter zou alleen kunt u selecteren **Service** of **Namespace**.  

![Voorbeeld met behulp van het filter verfijnen resultaten](./media/container-insights-analyze/add-filter-option-01.png)

Een filter op te geven op een tabblad blijft te worden toegepast wanneer u een andere selecteert en wordt verwijderd nadat u op de **x** symbool naast het opgegeven filter.   

Schakel over naar de **knooppunten** tabblad en de hiërarchie rij volgt het Kubernetes-objectmodel, beginnen met een knooppunt in het cluster. Vouw het knooppunt en vindt u een of meer schillen die worden uitgevoerd op het knooppunt. Als meer dan één container is gegroepeerd op een schil, worden ze weergegeven als de laatste rij in de hiërarchie. U kunt ook bekijken hoeveel niet-schil gerelateerde workloads worden uitgevoerd op de host als de host processor of geheugen.

![Voorbeeld van de Kubernetes-knooppunt-hiërarchie in de weergave van agentprestaties](./media/container-insights-analyze/containers-nodes-view.png)

Azure Container Instances virtuele knooppunten met het Linux-besturingssysteem worden weergegeven na de laatste AKS-cluster-knooppunt in de lijst.  Wanneer u een virtuele-ACI-knooppunt uitvouwt, kunt u een of meer pods in ACI en containers die worden uitgevoerd op het knooppunt weergeven.  Metrische gegevens zijn niet verzameld en gerapporteerd voor knooppunten, alleen schillen.

![Voorbeeld van knooppunt-hiërarchie met Container Instances vermeld](./media/container-insights-analyze/nodes-view-aci.png)

In een uitgevouwen knooppunt, kunt u inzoomen uit de schil of de container die wordt uitgevoerd op het knooppunt naar de controller om prestatiegegevens gefilterd voor controller weer te geven. Klik op de waarde onder de **Controller** kolom voor het specifieke knooppunt.   
![Voorbeeld van inzoomen naar controller in de weergave van agentprestaties](./media/container-insights-analyze/drill-down-node-controller.png)

U kunt selecteren controllers of containers aan de bovenkant van de pagina en het gebruik van de status en resource voor die objecten bekijken.  Als in plaats daarvan u bekijken geheugengebruik wilt, in de **Metric** vervolgkeuzelijst, selecteer **geheugen RSS** of **geheugenwerkset**. **Geheugen RSS** wordt alleen ondersteund voor Kubernetes versie 1.8 en hoger. Anders wordt het weergeven van waarden voor **Min&nbsp; %**  als *NaN&nbsp;%*, dit is een waarde voor het type van numerieke gegevens die een niet-gedefinieerde vertegenwoordigt of Sjabloontaal waarde. 

![Prestatieweergave container-knooppunten](./media/container-insights-analyze/containers-node-metric-dropdown.png)

Standaard prestatiegegevens is gebaseerd op de afgelopen zes uur, maar u kunt het venster wijzigen met behulp van de **TimeRange** optie in de linkerbovenhoek. U kunt ook de resultaten in het tijdsbereik filteren door te selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de lijst met percentiel. 

![Percentiel selectie voor het filteren van gegevens](./media/container-insights-analyze/containers-metric-percentile-filter.png)

Wanneer u met de muisaanwijzer boven het staafdiagram onder de **Trend** kolom, elke balk ziet u gebruik van CPU of geheugen, afhankelijk van welke waarde is geselecteerd, binnen de periode van een voorbeeld van 15 minuten.  

![Trend balk voorbeeldgrafiek bij aanwijzen](./media/container-insights-analyze/containers-metric-trend-bar-01.png)    

In het volgende voorbeeld wordt de opmerking voor de eerste in de lijst - knooppunt *aks-nodepool1 -*, de waarde voor **Containers** is 9, die een samenvoeging van het totale aantal containers die zijn geïmplementeerd is.

![Updatepakket van containers per knooppunt voorbeeld](./media/container-insights-analyze/containers-nodes-containerstotal.png)

U kunt snel bepalen of u een goede balans tussen containers tussen knooppunten in uw cluster hebt. 

De informatie die wordt weergegeven wanneer u knooppunten weergeven wordt in de volgende tabel beschreven:

| Kolom | Description | 
|--------|-------------|
| Name | De naam van de host. |
| Status | Kubernetes-weergave van de status van het knooppunt. |
| Gem.&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Gemiddelde knooppunt percentage is gebaseerd op percentiel tijdens de geselecteerde periode. |
| Avg, Min, Max, 50e, 90e | Gemiddelde knooppunten werkelijke waarde op basis van percentiel gedurende deze tijdsduur geselecteerd. De gemiddelde waarde wordt gemeten vanuit de CPU/geheugen die is ingesteld voor een knooppunt. het is de avg-waarde die wordt gerapporteerd door de host voor schillen en containers. |
| Containers | Het aantal containers. |
| Actieve tijdsduur | Geeft de tijd sinds een knooppunt is gestart of opnieuw is opgestart. |
| Controllers | Alleen voor containers en schillen. Hier ziet u welke domeincontroller is die zich bevinden in. Niet alle schillen zijn in een domeincontroller, zodat sommige mogelijk weergegeven **N.V.T**. | 
| Gem. trend&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Staafdiagram trend geeft het gemiddelde percentiel metrische percentage van de controller. |

Selecteer in de lijst met **Controllers**.

![Selecteer controllers weergeven](./media/container-insights-analyze/containers-controllers-tab.png)

Hier vindt u de prestatiestatus van uw domeincontrollers en domeincontrollers ACI virtueel knooppunt of virtueel knooppunt schillen niet is verbonden met een domeincontroller.

![< naam > controllers Prestatieweergave](./media/container-insights-analyze/containers-controllers-view.png)

De hiërarchie van de rij wordt gestart met een domeincontroller en wanneer u een domeincontroller wilt uitbreiden, u een of meer pods weergeven.  Pod uitvouwen en de laatste rij geeft de container de schil gegroepeerd. Uit een uitgebreide controller, kunt u inzoomen op het knooppunt waarop het wordt uitgevoerd om prestatiegegevens gefilterd voor dat knooppunt weer te geven. ACI schillen niet is verbonden met een domeincontroller worden laatst weergegeven in de lijst.

![Voorbeeld van de Controllers hiërarchie met Container Instances schillen vermeld](./media/container-insights-analyze/controllers-view-aci.png)

Klik op de waarde onder de **knooppunt** kolom voor de specifieke controller.   

![Voorbeeld inzoomen van knooppunt naar controller in de weergave van agentprestaties](./media/container-insights-analyze/drill-down-controller-node.png)

De informatie die wordt weergegeven wanneer u de controllers weergeven wordt in de volgende tabel beschreven:

| Kolom | Description | 
|--------|-------------|
| Name | De naam van de controller.|
| Status | De status rollup van de containers is voltooid met met de status, zoals *OK*, *beëindigd*, *mislukt* *gestopt*, of *Onderbroken*. Als de container wordt uitgevoerd, maar de status is niet correct weergegeven of niet is doorgevoerd door de agent als meer dan dertig minuten niet heeft gereageerd, de status is *onbekende*. Aanvullende details van de statuspictogram zijn opgegeven in de onderstaande tabel.|
| Gem.&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Updatepakket gemiddelde van het gemiddelde percentage van elke entiteit voor de geselecteerde metrische gegevens en percentiel. |
| Avg, Min, Max, 50e, 90e  | Totalisering van de gemiddelde CPU millicore of geheugen prestaties van de container voor het geselecteerde percentiel. De gemiddelde waarde wordt van de CPU/geheugen die is ingesteld voor een schil gemeten. |
| Containers | Totaal aantal containers voor de controller of de schil. |
| Opnieuw opstarten | Updatepakket van het aantal opnieuw opstarten van containers. |
| Actieve tijdsduur | Geeft de tijd sinds een container is gestart. |
| Knooppunt | Alleen voor containers en schillen. Hier ziet u welke domeincontroller dat deze zich bevindt. | 
| Gem. trend&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;%| Trend staafdiagram vertegenwoordigt de metriek gemiddelde percentiel van de controller. |

De pictogrammen in het statusveld geven aan de online status van de containers:
 
| Pictogram | Status | 
|--------|-------------|
| ![Pictogram voor actieve status gereed](./media/container-insights-analyze/containers-ready-icon.png) | Wordt uitgevoerd (klaar)|
| ![In afwachting van ' of ' onderbroken statuspictogram](./media/container-insights-analyze/containers-waiting-icon.png) | In afwachting van ' of ' onderbroken|
| ![De laatste keer doorgegeven met statuspictogram](./media/container-insights-analyze/containers-grey-icon.png) | Laatst gerapporteerd uitgevoerd maar meer dan 30 minuten nog niet is gereageerd|
| ![Geslaagde statuspictogram](./media/container-insights-analyze/containers-green-icon.png) | Is gestopt of niet stoppen|

De van het statuspictogram wordt weergegeven een aantal op basis van wat de schil biedt. De slechtste twee statussen worden weergegeven, en wanneer u de muisaanwijzer over de status, wordt de status van een updatepakket van alle schillen weergegeven in de container. Als er geen een status gereed heeft, wordt de statuswaarde weergegeven **(0)**. 

Selecteer in de lijst met **Containers**.

![Selecteer containers weergeven](./media/container-insights-analyze/containers-containers-tab.png)

Hier vindt u de prestatiestatus van uw Azure Kubernetes en Azure Container Instances-containers.  

![< naam > controllers Prestatieweergave](./media/container-insights-analyze/containers-containers-view.png)

Uit een container, kunt u inzoomen op een pod of het knooppunt om prestatiegegevens gefilterd voor dat object weer te geven. Klik op de waarde onder de **Pod** of **knooppunt** kolom voor de specifieke container.   

![Voorbeeld inzoomen van knooppunt naar controller in de weergave van agentprestaties](./media/container-insights-analyze/drill-down-controller-node.png)

De informatie die wordt weergegeven wanneer u containers weergeven wordt in de volgende tabel beschreven:

| Kolom | Description | 
|--------|-------------|
| Name | De naam van de controller.|
| Status | De status van de containers, indien van toepassing. Meer informatie over het statuspictogram vindt u in de volgende tabel.|
| Gem.&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Het totaal van het gemiddelde percentage van elke entiteit voor de geselecteerde metrische gegevens en percentiel. |
| Avg, Min, Max, 50e, 90e  | Het totaal van de gemiddelde CPU millicore of geheugen prestaties van de container voor het geselecteerde percentiel. De gemiddelde waarde wordt van de CPU/geheugen die is ingesteld voor een schil gemeten. |
| Pod | De container waarin de schil zich bevindt.| 
| Knooppunt |  Het knooppunt waarin de container zich bevindt. | 
| Opnieuw opstarten | Geeft de tijd sinds een container is gestart. |
| Actieve tijdsduur | Geeft de tijd sinds een container is gestart of opnieuw opgestart. |
| Gem. trend&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Staafdiagram trend geeft het gemiddelde percentiel metrische percentage van de container. |

De pictogrammen in het statusveld geven aan de online status van schillen, zoals beschreven in de volgende tabel:
 
| Pictogram | Status |  
|--------|-------------|  
| ![Pictogram voor actieve status gereed](./media/container-insights-analyze/containers-ready-icon.png) | Wordt uitgevoerd (klaar)|  
| ![In afwachting van ' of ' onderbroken statuspictogram](./media/container-insights-analyze/containers-waiting-icon.png) | In afwachting van ' of ' onderbroken|  
| ![De laatste keer doorgegeven met statuspictogram](./media/container-insights-analyze/containers-grey-icon.png) | Laatst uitgevoerd gemeld, maar nog niet is gereageerd in meer dan 30 minuten|  
| ![Beëindigde statuspictogram](./media/container-insights-analyze/containers-terminated-icon.png) | Is gestopt of niet stoppen|  
| ![Pictogram van de status mislukt](./media/container-insights-analyze/containers-failed-icon.png) | Status mislukt |  


## <a name="container-data-collection-details"></a>Details van de gegevensverzameling container
Container Insights verzamelt verschillende metrische gegevens en logboekbestanden prestatiegegevens van de hosts van de container en containers. Gegevens worden verzameld om de drie minuten.

### <a name="container-records"></a>Container-records

Voorbeelden van records die door Azure Monitor worden verzameld voor containers en de gegevenstypen die worden weergegeven in de resultaten van de logboekzoekopdracht worden weergegeven in de volgende tabel:

| Gegevenstype | Het gegevenstype in zoeken in Logboeken | Velden |
| --- | --- | --- |
| Prestaties voor hosts en -containers | `Perf` | Computer, ObjectName, CounterName &#40;% processortijd, schijf leest MB, schijf schrijft MB, gebruik van geheugen (MB), netwerk ontvangen Bytes, netwerk verzenden Bytes, Processor gebruik seconde, netwerk&#41;, CounterValue, TimeGenerated, itempad, SourceSystem |
| Container-inventaris | `ContainerInventory` | TimeGenerated, de Computer, de containernaam, ContainerHostname, afbeelding, ImageTag, ContainerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Inventarisatie van container-installatiekopie | `ContainerImageInventory` | TimeGenerated, Computer, afbeelding, ImageTag, ImageSize, VirtualSize, die wordt uitgevoerd, is onderbroken, gestopt, is mislukt, SourceSystem, ImageID, TotalContainer |
| Container-logboek | `ContainerLog` | TimeGenerated, Computer, afbeeldings-ID, containernaam van de, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Container service-logboek | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, afbeelding, opdracht, SourceSystem, ContainerID |
| Inventaris van containerknooppunten | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Containerverwerking | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventaris van schillen in een Kubernetes-cluster | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, servicenaam, ControllerKind, controllernaam, ContainerStatus, ContainerID, ContainerName, naam, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventarisatie van een deel van de knooppunten van een Kubernetes-cluster | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes-gebeurtenissen | `KubeEvents_CL` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, bericht, SourceSystem | 
| Services in het Kubernetes-cluster | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Metrische gegevens voor prestaties voor een deel van de knooppunten van het Kubernetes-cluster | Perf &#124; waarbij ObjectName == "K8SNode" | Computer, ObjectName, CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, CounterValue, TimeGenerated, itempad, SourceSystem | 
| Metrische gegevens voor prestaties voor containers die deel uitmaken van het Kubernetes-cluster | Perf &#124; waarbij ObjectName == "K8SContainer" | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, CounterValue, TimeGenerated, itempad, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Zoeken in Logboeken om gegevens te analyseren
Log Analytics kunt u zoeken naar bestedingstrends en een diagnose stellen van knelpunten, prognose, of het correleren gegevens die u kan helpen te bepalen of de huidige configuratie van het cluster optimaal presteert. Vooraf gedefinieerde zoekopdrachten in Logboeken vindt u onmiddellijk kunt beginnen met of om aan te passen als u wilt de gegevens van de manier waarop die u wilt retourneren. 

U kunt uitvoeren interactieve analyses van gegevens in de werkruimte selecteren de **weergave Kubernetes gebeurtenislogboeken** of **containerlogboeken bekijken** optie in het voorbeeldvenster. De **zoeken in logboeken** pagina wordt weergegeven aan de rechterkant van de Azure portal-pagina die u al had geopend.

![Gegevens analyseren in Log Analytics](./media/container-insights-analyze/container-health-log-search-example.png)   

De uitvoer van de container-logboeken die wordt doorgestuurd naar Log Analytics zijn STDOUT en STDERR. Omdat Azure Monitor wordt bewaakt door Azure beheerde Kubernetes (AKS), Kube-systeem niet vandaag verzameld vanwege het grote aantal gegenereerde gegevens. 

### <a name="example-log-search-queries"></a>Voorbeeld van de logboekbestanden zoekquery 's
Vaak is het handig om te maken van query's die beginnen met een voorbeeld of twee en vervolgens kan aanpassen aan uw behoeften. Om te helpen meer geavanceerde query's, kunt u experimenteren met de volgende voorbeeldquery's:

| Query’s uitvoeren | Description | 
|-------|-------------|
| ContainerInventory<br> &#124;Computer, de naam, afbeelding, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime project<br> &#124;tabel weergeven | Lijst met alle van de container levenscyclus van gegevens| 
| KubeEvents_CL<br> &#124;waar not(isempty(Namespace_s))<br> &#124;sorteren op TimeGenerated desc<br> &#124;tabel weergeven | Kubernetes-gebeurtenissen|
| ContainerImageInventory<br> &#124;summarize AggregatedValue = count() by afbeelding, ImageTag, actief | Voorraad | 
| **Selecteer de optie lijndiagram weergeven**:<br> Prestaties<br> &#124;waarbij ObjectName == "K8SContainer" en CounterName == "cpuUsageNanoCores" &#124; AvgCPUUsageNanoCores samenvatten avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container CPU | 
| **Selecteer de optie lijndiagram weergeven**:<br> Prestaties<br> &#124;waarbij ObjectName == "K8SContainer" en CounterName == "memoryRssBytes" &#124; AvgUsedRssMemoryBytes samenvatten avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container-geheugen |

## <a name="alerting"></a>Waarschuwingen
Azure Monitor voor containers bevat geen een vooraf gedefinieerde set waarschuwingen die u kunt kopiëren en wijzigen op basis van uw ondersteunende processen en procedures. Bekijk in de tussentijd [waarschuwingen maken met Azure Monitor](../../monitoring-and-diagnostics/alert-log.md?toc=/azure/azure-monitor/toc.json) en informatie over het maken van uw eigen set waarschuwingen.  