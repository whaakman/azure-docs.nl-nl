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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/22/2019
ms.author: magoedte
ms.openlocfilehash: bbfc8cc61571de8b76ef1f7f0216501ef6d2cdee
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377469"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Inzicht in prestaties in een AKS-cluster met Azure Monitor voor containers 
Met Azure Monitor voor containers, kunt u de van prestatiegrafieken en de status voor het bewaken van de werkbelasting van uw Azure Kubernetes Service (AKS)-clusters vanuit twee perspectieven, rechtstreeks vanuit een AKS-cluster of alle AKS-clusters in een abonnement van Azure Monitor. Weergeven van Azure Container Instances (ACI) is ook mogelijk wanneer u een specifieke AKS-cluster bewaken.

Dit artikel krijgt u inzicht in de ervaring tussen de twee perspectieven en hoe Hiermee kunt u snel beoordelen, onderzoeken en oplossen van problemen die zijn gedetecteerd.

Zie voor meer informatie over het inschakelen van Azure Monitor voor containers [ingebouwde Azure-Monitor voor containers](container-insights-onboard.md).

Azure Monitor biedt een multi-cluster weergave met de status van alle bewaakte AKS-clusters met Linux en Windows Server 2019 die zijn geïmplementeerd in alle resource groepen in uw abonnementen.  Hier ziet u AKS clusters gedetecteerd die niet worden bewaakt door de oplossing. U kunt onmiddellijk clusterstatus begrijpen en hier kunt u inzoomen op de pagina van de prestaties knooppunt en de domeincontroller of Ga om te zien worden prestatiegrafieken weergegeven voor het cluster.  Voor clusters met AKS gedetecteerd en geïdentificeerd als niet-bewaakt, kunt u de bewaking voor dat cluster op elk gewenst moment inschakelen.  

De belangrijkste verschillen bij het bewaken van een Windows Server-cluster met Azure Monitor voor containers in vergelijking met een Linux-cluster zijn de volgende:

- De metrische gegevens van het geheugen zijn niet beschikbaar voor Windows-knoop punten en-containers.
- Informatie over capaciteit van schijf opslag is niet beschikbaar voor Windows-knoop punten.
- Ondersteuning voor Live-Logboeken is beschikbaar met uitzonde ring van Windows-container Logboeken.
- Alleen pod omgevingen worden bewaakt, niet-docker-omgevingen.
- Met de preview-versie worden Maxi maal 30 Windows Server-containers ondersteund. Deze beperking is niet van toepassing op Linux-containers.  

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com). 

## <a name="multi-cluster-view-from-azure-monitor"></a>Weergave van meerdere cluster van Azure Monitor

Als u wilt weergeven van de status van alle AKS clusters die zijn geïmplementeerd, selecteert u **Monitor** in het linkervenster in Azure portal.  Onder de **Insights** sectie, selecteer **Containers**.  

![Voorbeeld van meerdere clusterdashboard met Azure Monitor](./media/container-insights-analyze/azmon-containers-multiview.png)

Op de **bewaakt clusters** tabblad, kunt u de volgende informatie:

1. Het aantal clusters hebben een status Kritiek of slecht ten opzichte van het aantal zijn in orde is of niet reporting (aangeduid als een onbekende status)?
2. Zijn alle mijn [Azure Kubernetes-Engine (AKS-engine)](https://github.com/Azure/aks-engine) implementaties in orde?
3. Hoeveel knoop punten, het aantal gebruikers en het systeem worden er per cluster geïmplementeerd?
4. Hoeveel schijf ruimte is er beschikbaar en is er sprake van een capaciteits probleem?

De health-statussen inbegrepen zijn: 

* **In orde** : Er zijn geen problemen gedetecteerd voor de virtuele machine en deze werkt zoals vereist. 
* **Kritieke** : een of meer kritieke problemen worden ontdekt, die moeten worden opgelost als u wilt herstellen van normale operationele status, zoals verwacht.
* **Waarschuwing** -worden een of meer problemen gedetecteerd die moeten worden opgelost of de voorwaarde van de status kritiek kan worden.
* **Onbekende** : als de service kan niet een verbinding met het knooppunt of pod, de status is gewijzigd in een onbekende status te maken.
* **Niet gevonden** : een van beide de werkruimte, de resourcegroep of abonnement met de werkruimte voor deze oplossing, is verwijderd.
* **Niet-geautoriseerde** -gebruiker beschikt niet over vereiste machtigingen om de gegevens in de werkruimte te lezen.
* **Fout** -er is een fout opgetreden tijdens het lezen van gegevens van de werkruimte.
* Verkeerd geconfigureerde **-Azure monitor** voor containers is niet juist geconfigureerd in de opgegeven werk ruimte.
* **Er zijn geen gegevens** -gegevens niet naar de werkruimte heeft gerapporteerd in de laatste 30 minuten.

De status van het hele cluster wordt met één uitzonde ring berekend als *slechtste van* de drie statussen. als een van de drie statussen *onbekend*is, wordt in de algehele cluster status **onbekende**weer gegeven.  

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

- **CPU-&nbsp;gebruik%van knoop punt**: Een geaggregeerd perspectief van CPU-gebruik voor het hele cluster. U kunt filteren dat de resultaten voor het tijdsbereik selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de kiezer percentielen boven de grafiek, ofwel afzonderlijk of gecombineerd. 
- **Geheugen gebruik&nbsp;vanknooppunt:%** Een geaggregeerd perspectief van geheugen gebruik voor het hele cluster. U kunt filteren dat de resultaten voor het tijdsbereik selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de kiezer percentielen boven de grafiek, ofwel afzonderlijk of gecombineerd. 
- **Aantal knoop punten**: Het aantal knoop punten en de status van Kubernetes. Status van de clusterknooppunten weergegeven zijn *alle*, *gereed*, en *niet gereed* en kunnen worden gefilterd afzonderlijk of in de kiezer boven de grafiek worden gecombineerd. 
- **Aantal pod activiteiten**: Een aantal pod en een status van Kubernetes. Status van de vertegenwoordigd schillen zijn *alle*, *in behandeling*, *uitgevoerd*, en *onbekende* en kunnen worden gefilterd, afzonderlijk of gecombineerd de kiezer boven de grafiek. 

U kunt de toetsen pijl links/rechts gebruiken om door elk gegevens punt in de grafiek te bladeren en de pijl-omhoog/omlaag-toetsen om de percentiel lijnen te door lopen. Als u op het speld pictogram in de rechter bovenhoek van een van de grafieken klikt, wordt de geselecteerde grafiek vastgemaakt aan het laatste Azure-dash board dat u het laatst hebt bekeken. Vanuit het dash board kunt u het formaat en de positie van de grafiek wijzigen. Als u de grafiek in het dash board selecteert, wordt u omgeleid naar Azure Monitor voor containers en het juiste bereik en de weer gave te laden.

Azure Monitor voor containers biedt ook ondersteuning voor Azure Monitor [Metrics Explorer](../platform/metrics-getting-started.md), waar u uw eigen plot grafieken kunt maken, trends en onderzoeken en vastmaakt aan dash boards. Vanuit Metrics Explorer kunt u ook de criteria gebruiken die u hebt ingesteld voor het visualiseren van uw metrische gegevens als basis voor een [waarschuwings regel op basis van metrische gegevens](../platform/alerts-metric.md).  

## <a name="view-container-metrics-in-metrics-explorer"></a>Metrische container gegevens weer geven in Metrics Explorer

In Metrics Explorer kunt u geaggregeerde gegevens over knoop punt-en pod-gebruik weer geven van Azure Monitor voor containers. De volgende tabel bevat een overzicht van de Details om u te helpen begrijpen hoe u de metrische grafieken kunt gebruiken voor het visualiseren van metrische gegevens over containers.

|Naamruimte | Gegevens |
|----------|--------|
| insights.container/nodes | |
| | cpuUsageMillicores |
| | cpuUsagePercentage |
| | memoryRssBytes |
| | memoryRssPercentage |
| | memoryWorkingSetBytes |
| | memoryWorkingSetPercentage |
| | nodesCount |
| insights.container/pods | |
| | PodCount |

U kunt de functie voor het [splitsen](../platform/metrics-charts.md#apply-splitting-to-a-chart) van een metriek Toep assen om deze per dimensie weer te geven en te visualiseren hoe verschillende segmenten van de IT met elkaar worden vergeleken. Voor een knoop punt kunt u het diagram segmenteren op basis van de dimensie *host* en van een pod kunt u dit segmenteren op basis van de volgende dimensies:

* Controller
* Kubernetes-naam ruimte
* Knooppunt
* Fase

## <a name="analyze-nodes-controllers-and-container-health"></a>Knoop punten, controllers en container status analyseren

Als u overschakelt naar **knooppunten**, **Controllers**, en **Containers** tabblad automatisch weergegeven aan de rechterkant van de pagina is het deelvenster met de eigenschap. Hierin worden de eigenschappen van het geselecteerde item weer gegeven, met inbegrip van labels die u definieert om Kubernetes-objecten te organiseren. Wanneer een Linux-knoop punt is geselecteerd, wordt het ook weer gegeven onder de sectie **lokale schijf capaciteit** beschik bare schijf ruimte en percentage gebruikt voor elke schijf die wordt weer gegeven aan het knoop punt. Klik op de **>>** koppelen in het deelvenster aan view\hide het deelvenster. 

![Voorbeeld van Kubernetes perspectieven het deelvenster met eigenschappen](./media/container-insights-analyze/perspectives-preview-pane-01.png)

Als u de objecten in de hiërarchie wilt uitbreiden, de eigenschappen van deelvenster updates op basis van het geselecteerde object. In het deelvenster kunt u de Kubernetes-gebeurtenissen met vooraf gedefinieerde zoekopdrachten ook weergeven door te klikken op de **weergave Kubernetes gebeurtenislogboeken** koppelen aan de bovenkant van het deelvenster. Zie voor meer informatie over het weergeven van Kubernetes-logboekgegevens [zoeken in Logboeken om gegevens te analyseren](container-insights-log-search.md). Terwijl u cluster bronnen bekijkt, kunt u container logboeken en gebeurtenissen in realtime zien. Zie [Logboeken realtime weer geven met Azure monitor voor containers](container-insights-live-logs.md)voor meer informatie over deze functie en de configuratie die vereist is om toegang te verlenen en te beheren. 

Gebruik de **+ filter optie toevoegen** boven aan de pagina om de resultaten voor de weer gave per **service**, **knoop punt**, **naam ruimte**of **knooppunt groep** te filteren en nadat u het filter bereik hebt geselecteerd, selecteert u een van de waarden die worden weer gegeven in de **Selecteer een veld waarde (n)** .  Nadat het filter is geconfigureerd, wordt deze wereldwijd toegepast tijdens het bekijken van een perspectief van het AKS-cluster.  De formule ondersteunt alleen het gelijkteken.  U kunt extra filters op de eerste verder beperken van de resultaten kunt toevoegen.  Bijvoorbeeld, als u een filter door opgegeven **knooppunt**, het tweede filter zou alleen kunt u selecteren **Service** of **Namespace**.  

![Voorbeeld met behulp van het filter verfijnen resultaten](./media/container-insights-analyze/add-filter-option-01.png)

Een filter op te geven op een tabblad blijft te worden toegepast wanneer u een andere selecteert en wordt verwijderd nadat u op de **x** symbool naast het opgegeven filter.   

Schakel over naar de **knooppunten** tabblad en de hiërarchie rij volgt het Kubernetes-objectmodel, beginnen met een knooppunt in het cluster. Vouw het knooppunt en vindt u een of meer schillen die worden uitgevoerd op het knooppunt. Als meer dan één container is gegroepeerd op een schil, worden ze weergegeven als de laatste rij in de hiërarchie. U kunt ook bekijken hoeveel niet-schil gerelateerde workloads worden uitgevoerd op de host als de host processor of geheugen.

![Voorbeeld van de Kubernetes-knooppunt-hiërarchie in de weergave van agentprestaties](./media/container-insights-analyze/containers-nodes-view.png)

Windows Server-containers waarop het besturings systeem Windows Server 2019 wordt uitgevoerd, worden weer gegeven na alle op Linux gebaseerde knoop punten in de lijst. Wanneer u een Windows Server-knoop punt uitvouwt, kunt u een of meer peulen en containers die op het knoop punt worden uitgevoerd, weer geven. Wanneer een knoop punt is geselecteerd, worden in het deel venster Eigenschappen versie-informatie weer gegeven, met uitzonde ring van agent informatie, omdat er geen agent is geïnstalleerd op Windows Server-knoop punten.  

![Voor beeld van knooppunt hiërarchie met Windows Server-knoop punten in de lijst](./media/container-insights-analyze/nodes-view-windows.png) 

Azure Container Instances virtuele knooppunten met het Linux-besturingssysteem worden weergegeven na de laatste AKS-cluster-knooppunt in de lijst.  Wanneer u een virtuele-ACI-knooppunt uitvouwt, kunt u een of meer pods in ACI en containers die worden uitgevoerd op het knooppunt weergeven.  Metrische gegevens zijn niet verzameld en gerapporteerd voor knooppunten, alleen schillen.

![Voorbeeld van knooppunt-hiërarchie met Container Instances vermeld](./media/container-insights-analyze/nodes-view-aci.png)

In een uitgevouwen knooppunt, kunt u inzoomen uit de schil of de container die wordt uitgevoerd op het knooppunt naar de controller om prestatiegegevens gefilterd voor controller weer te geven. Klik op de waarde onder de **Controller** kolom voor het specifieke knooppunt.   
![Voorbeeld van inzoomen naar controller in de weergave van agentprestaties](./media/container-insights-analyze/drill-down-node-controller.png)

U kunt selecteren controllers of containers aan de bovenkant van de pagina en het gebruik van de status en resource voor die objecten bekijken.  Als in plaats daarvan u bekijken geheugengebruik wilt, in de **Metric** vervolgkeuzelijst, selecteer **geheugen RSS** of **geheugenwerkset**. **Geheugen RSS** wordt alleen ondersteund voor Kubernetes versie 1.8 en hoger. Anders wordt het weergeven van waarden voor **Min&nbsp; %**  als *NaN&nbsp;%* , dit is een waarde voor het type van numerieke gegevens die een niet-gedefinieerde vertegenwoordigt of Sjabloontaal waarde.

Geheugen werkset toont zowel het residente geheugen als het virtuele geheugen (cache), en is in totaal wat de toepassing gebruikt. In het geheugen RSS wordt alleen het hoofd geheugen weer gegeven. Dit is het residente geheugen. Met deze metriek wordt de werkelijke capaciteit van het beschik bare geheugen weer gegeven.

![Prestatieweergave container-knooppunten](./media/container-insights-analyze/containers-node-metric-dropdown.png)

Standaard prestatiegegevens is gebaseerd op de afgelopen zes uur, maar u kunt het venster wijzigen met behulp van de **TimeRange** optie in de linkerbovenhoek. U kunt ook de resultaten in het tijdsbereik filteren door te selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de lijst met percentiel. 

![Percentiel selectie voor het filteren van gegevens](./media/container-insights-analyze/containers-metric-percentile-filter.png)

Wanneer u met de muisaanwijzer boven het staafdiagram onder de **Trend** kolom, elke balk ziet u gebruik van CPU of geheugen, afhankelijk van welke waarde is geselecteerd, binnen de periode van een voorbeeld van 15 minuten. Nadat u het trend diagram via een toetsen bord hebt geselecteerd, kunt u de toetsen Alt + PageUp of ALT + PGDN gebruiken om elke balk afzonderlijk te door lopen en dezelfde details te verkrijgen als bij mouseover.

![Een trend balk diagram met de muis aanwijzer over voor beeld](./media/container-insights-analyze/containers-metric-trend-bar-01.png)    

In het volgende voorbeeld wordt de opmerking voor de eerste in de lijst - knooppunt *aks-nodepool1 -* , de waarde voor **Containers** is 9, die een samenvoeging van het totale aantal containers die zijn geïmplementeerd is.

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

![\<Prestatie weergave van de groeps naam > controllers](./media/container-insights-analyze/containers-controllers-view.png)

De hiërarchie van de rij wordt gestart met een domeincontroller en wanneer u een domeincontroller wilt uitbreiden, u een of meer pods weergeven.  Pod uitvouwen en de laatste rij geeft de container de schil gegroepeerd. Uit een uitgebreide controller, kunt u inzoomen op het knooppunt waarop het wordt uitgevoerd om prestatiegegevens gefilterd voor dat knooppunt weer te geven. ACI schillen niet is verbonden met een domeincontroller worden laatst weergegeven in de lijst.

![Voorbeeld van de Controllers hiërarchie met Container Instances schillen vermeld](./media/container-insights-analyze/controllers-view-aci.png)

Klik op de waarde onder de **knooppunt** kolom voor de specifieke controller.   

![Voorbeeld inzoomen van knooppunt naar controller in de weergave van agentprestaties](./media/container-insights-analyze/drill-down-controller-node.png)

De informatie die wordt weergegeven wanneer u de controllers weergeven wordt in de volgende tabel beschreven:

| Kolom | Description | 
|--------|-------------|
| Name | De naam van de controller.|
| Status | De status rollup van de containers is voltooid met met de status, zoals *OK*, *beëindigd*, *mislukt* *gestopt*, of *Onderbroken*. Als de container wordt uitgevoerd, maar de status is niet correct weergegeven of niet is doorgevoerd door de agent als meer dan dertig minuten niet heeft gereageerd, de status is *onbekende*. Aanvullende details van de statuspictogram zijn opgegeven in de onderstaande tabel.|
| Gem.&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Het gemiddelde van het gemiddelde percentage van elke entiteit voor de geselecteerde metriek en percentiel totaliseren. |
| Avg, Min, Max, 50e, 90e  | Totaliseren van de gemiddelde CPU-millicore of-geheugen prestaties van de container voor het geselecteerde percentiel. De gemiddelde waarde wordt van de CPU/geheugen die is ingesteld voor een schil gemeten. |
| Containers | Totaal aantal containers voor de controller of de schil. |
| Opnieuw opstarten | Totaliseren van het aantal opnieuw opstarten vanuit containers. |
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

De van het statuspictogram wordt weergegeven een aantal op basis van wat de schil biedt. De slechtste twee statussen worden weergegeven, en wanneer u de muisaanwijzer over de status, wordt de status van een updatepakket van alle schillen weergegeven in de container. Als er geen een status gereed heeft, wordt de statuswaarde weergegeven **(0)** . 

Selecteer in de lijst met **Containers**.

![Selecteer containers weergeven](./media/container-insights-analyze/containers-containers-tab.png)

Hier vindt u de prestatiestatus van uw Azure Kubernetes en Azure Container Instances-containers.  

![\<Prestatie weergave van de groeps naam > controllers](./media/container-insights-analyze/containers-containers-view.png)

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

## <a name="workbooks"></a>Workbooks

Werkmappen combi neren tekst, [logboek query's](../log-query/query-language.md), [metrische gegevens](../platform/data-platform-metrics.md)en para meters in uitgebreide interactieve rapporten. Werkmappen kunnen worden bewerkt door andere team leden die toegang hebben tot dezelfde Azure-resources.

Azure Monitor voor containers bevat vier werkmappen om aan de slag te gaan:

- **Schijf capaciteit**: Geeft interactieve schijf gebruiks grafieken voor elke schijf die wordt weer gegeven in het knoop punt binnen een container in de volgende perspectieven:

    - Schijf% gebruik voor alle schijven
    - Beschik bare schijf ruimte voor alle schijven
    - Een raster dat voor elke knoop punt schijf, het percentage gebruikte ruimte, de trend van% gebruikte ruimte, vrije schijf ruimte (GiB) en trend van vrije schijf ruimte (GiB) wordt weer gegeven. Wanneer een rij wordt geselecteerd in de tabel, wordt er% gebruikte ruimte en vrije schijf ruimte (GiB) hieronder weer gegeven 

- **Schijf-IO**: Geeft interactieve schijf gebruiks grafieken voor elke schijf die wordt weer gegeven in het knoop punt binnen een container in de volgende perspectieven:

    - Schijf-I/O wordt op alle schijven samenvatten door gelezen bytes per seconde, geschreven bytes per seconde en lees-en schrijf bewerkingen in bytes per seconde 
    - Acht prestatie grafieken met belang rijke prestatie-indica toren om schijf-I/O-knel punten te meten en te identificeren.

- **Kubelet**: Bevat twee rasters die de bedrijfs statistieken van het sleutel knooppunt weer geven:

    - Overzicht per knooppunt raster geeft een overzicht van de totale bewerking, het totale aantal fouten en geslaagde bewerkingen op percentage en trend voor elk knoop punt.
    - Overzicht per bewerkings type geeft een samen vatting van de totale bewerking, het totale aantal fouten en geslaagde bewerkingen op percentage en trend.

- **Netwerk**: Presenteert interactieve netwerken voor netwerk gebruik voor elke knoop punten netwerk adapter en een raster met de belangrijkste prestatie-indica toren om de prestaties van uw netwerk adapters te meten.  

U opent deze werkmappen door elk ervan te selecteren in de vervolg keuzelijst **werkmappen weer geven** .  

![Vervolg keuzelijst werkmappen weer geven](./media/container-insights-analyze/view-workbooks-dropdown-list.png)

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de procedures [voor het maken van prestatie waarschuwingen met Azure monitor voor containers](container-insights-alerts.md) voor meer informatie over het maken van waarschuwingen voor hoog CPU-en geheugen gebruik ter ondersteuning van uw DevOps-of operationele processen en procedures. 

- Bekijk de [voor beelden van logboek query's](container-insights-log-search.md#search-logs-to-analyze-data) om vooraf gedefinieerde query's en voor beelden te bekijken voor het evalueren of aanpassen van waarschuwingen, het visualiseren of analyseren van uw clusters.
