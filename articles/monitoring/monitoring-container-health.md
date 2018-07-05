---
title: Controleren van Azure Kubernetes Service (AKS)-status (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de prestaties van uw AKS-container om te zien van het gebruik van uw gehoste Kubernetes-omgeving snel kunt zien.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/02/2018
ms.author: magoedte
ms.openlocfilehash: e7d3fdf9e6f027ab1c23a057ad6e039d50cab9ad
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436419"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Containerstatus van Azure Kubernetes Service (AKS) (voorbeeld) bewaken

Dit artikel wordt beschreven hoe u kunt instellen en containerstatus van Azure Monitor gebruiken voor het bewaken van de prestaties van uw workloads in Kubernetes-omgevingen die worden gehost in Azure Kubernetes Service (AKS) geïmplementeerd.  Het is erg belangrijk om uw Kubernetes-cluster en -containers te bewaken, vooral wanneer u een productiecluster op schaal uitvoert met meerdere toepassingen.

Containerstatus biedt u de mogelijkheid door verzamelen geheugen en processors metrische gegevens van domeincontrollers, knooppunten en containers in Kubernetes via de API voor metrische gegevens beschikbaar voor prestatiebewaking.  Na het inschakelen van de containerstatus van deze metrische gegevens worden automatisch verzameld voor u gemaakt met een beperkte versie van de OMS-Agent voor Linux en opgeslagen in uw [Log Analytics](../log-analytics/log-analytics-overview.md) werkruimte.  De vooraf gedefinieerde weergaven die zijn opgenomen weergeven die container-workloads en wat invloed heeft op de prestatiestatus van het Kubernetes-cluster, zodat u inzicht krijgt in:  

* Welke containers worden uitgevoerd op het knooppunt en hun Gemiddeld gebruik voor de processor en geheugen aan de knelpunten in de resource
* Bepalen waar de container bevindt zich in een domeincontroller en/of schillen om te zien van de algehele prestaties voor een domeincontroller of een pod 
* Controleer het brongebruik van workloads die worden uitgevoerd op de host die geen verband houdt met de standaard processen ondersteunen de schil
* Informatie over het gedrag van het cluster onder belasting voor de gemiddelde en zwaarste op te sporen behoeften aan capaciteit en de maximale belasting die het bestand is tegen bepalen 

Als u geïnteresseerd bent in controle en beheer van uw Docker- en Windows containerhosts configuratie weergeven, controle en gebruik van resources, Zie de [Container Monitoring solution](../log-analytics/log-analytics-containers.md).

## <a name="requirements"></a>Vereisten 
Lees voordat u begint, de volgende informatie zodat u inzicht krijgt in de ondersteunde vereisten.

- Een nieuw of bestaand AKS-cluster
- Een beperkte OMS-agent voor Linux-versie microsoft / oms:ciprod04202018 en hoger. Het versienummer wordt vertegenwoordigd door een datum volgende de indeling - *mmddyyyy*.  Er wordt automatisch geïnstalleerd tijdens de voorbereiding van de containerstatus.  
- Een Log Analytics-werkruimte.  Het kan worden gemaakt wanneer u bewaking van het nieuwe cluster met AKS inschakelen of u via maken kunt [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), of vanuit de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md).
- Lid van de inzendersrol van Log Analytics om containerbewaking inschakelen.  Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Onderdelen 

Deze mogelijkheid is afhankelijk van een beperkte OMS-Agent voor Linux voor het verzamelen van prestaties en gebeurtenisgegevens uit alle knooppunten in het cluster.  De agent is geregistreerd met de opgegeven Log Analytics-werkruimte nadat u containerbewaking inschakelen en automatisch geïmplementeerd. 

>[!NOTE] 
>Als u al een AKS-cluster hebt geïmplementeerd, kunt u inschakelen bewaking met behulp van een opgegeven Azure Resource Manager-sjabloon, zoals verderop in dit artikel wordt gedemonstreerd. U kunt geen gebruiken `kubectl` als u wilt bijwerken, verwijderen, opnieuw implementeren of implementeren van de agent.  
>

## <a name="sign-in-to-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Inschakelen van de container en statusbewaking biedt voor een nieuw cluster
U kunt alleen inschakelen bewaking van uw AKS-cluster tijdens de implementatie van de Azure-portal.  Volg de stappen in dit artikel [een Azure Kubernetes Service (AKS)-cluster implementeren](../aks/kubernetes-walkthrough-portal.md).  Wanneer u bent op de **bewaking** weergeeft, schakelt **Ja** voor de optie **bewaking inschakelen** inschakelt, en selecteer een bestaande of maak een nieuwe Log Analytics-werkruimte.  

Nadat de controle is ingeschakeld alle configuratietaken zijn voltooid, kunt u de prestaties van uw cluster op basis van twee manieren controleren:

1. Rechtstreeks vanuit het AKS-cluster door te selecteren **Health** in het linkerdeelvenster.<br><br> 
2. Door te klikken op de **containerstatus bewaken** tegel op de pagina van de AKS-cluster voor het geselecteerde cluster.  Selecteer in Azure Monitor **Health** in het linkerdeelvenster.  

![Opties voor het selecteren van de containerstatus in AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Nadat de controle is ingeschakeld, kunnen duurt ongeveer 15 minuten voordat u zich kunt om te zien van operationele gegevens voor het cluster.  

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Inschakelen van de container en statusbewaking biedt voor bestaande beheerde clusters
Inschakelen van de controle van uw AKS-container al geïmplementeerd kan worden bereikt vanaf de Azure portal of met de opgegeven Azure Resource Manager-sjabloon met behulp van de PowerShell-cmdlet **New-AzureRmResourceGroupDeployment** of Azure CLI.  


### <a name="enable-from-azure-portal"></a>Inschakelen van Azure-portal
Voer de volgende stappen uit voor bewaking van uw AKS-container vanuit Azure portal.

1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Containers**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Kubernetes-services**.<br><br> ![Azure Portal](./media/monitoring-container-health/azure-portal-01.png)<br><br>  
2. Selecteer een container in uw lijst met containers.
3. Selecteer op de overzichtspagina van container **bewaken containerstatus** en de **Onboarding naar containerstatus en logboeken** pagina wordt weergegeven.
4. Op de **Onboarding naar containerstatus en logboeken** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  De lijst worden er de standaardwerkruimte en locatie van de AKS-container is geïmplementeerd op in het abonnement. Of u kunt selecteren **nieuw** en geeft u een nieuwe werkruimte in hetzelfde abonnement.<br><br> ![Statuscontrole van AKS container inschakelen](./media/monitoring-container-health/container-health-enable-brownfield.png) 

    Als u selecteert **nieuw**, wordt de **nieuwe werkruimte maken** deelvenster wordt weergegeven. De **regio** standaard ingesteld op de regio voor uw containerresource wordt gemaakt in en accepteer de standaardinstelling of Selecteer een andere regio en geef een naam op voor de werkruimte.  Klik op **maken** te accepteren van uw selectie.<br><br> ![Werkruimte voor container monintoring definiëren](./media/monitoring-container-health/create-new-workspace-01.png)  

    >[!NOTE]
    >Op dit moment die u geen nieuwe werkruimte in de regio West-Centraal VS maken kunt, kunt u alleen een bestaande werkruimte selecteren in die regio.  Hoewel u deze regio in de lijst selecteren kunt, de implementatie wordt gestart, maar deze mislukt kort daarna.  
    >
 
Nadat de controle is ingeschakeld, kunnen duurt ongeveer 15 minuten voordat u zich kunt om te zien van operationele gegevens voor het cluster. 

### <a name="enable-using-azure-resource-manager-template"></a>Inschakelen met behulp van Azure Resource Manager-sjabloon
Deze methode omvat twee JSON-sjablonen, één sjabloon Hiermee geeft u de configuratie voor bewaking en het JSON-sjabloon bevat parameterwaarden die u configureert om het volgende opgeven:

* AKS container resource-ID 
* Resourcegroep waarin het cluster wordt geïmplementeerd in 
* Log Analytics-werkruimte en de regio voor het maken van de werkruimte in 

De Log Analytics-werkruimte heeft handmatig worden gemaakt.  Voor het maken van de werkruimte, kunt u instellen een via [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), uit de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md).

Als u niet bekend met de concepten bent van de implementatie van resources met behulp van een sjabloon met PowerShell, raadpleegt u [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)of Azure CLI Zie, voor [resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Als u het gebruik van Azure CLI, moet u eerst installeren en de CLI lokaal gebruikt.  Dit is vereist dat u de Azure CLI versie 2.0.27 of hoger. Voer `az --version` voor het identificeren van de versie. Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

#### <a name="create-and-execute-template"></a>Maken en uitvoeren van sjabloon

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "aksResourceId": {
        "type": "string",
        "metadata": {
           "description": "AKS Cluster Resource ID"
           }
    },
    "aksResourceLocation": {
    "type": "string",
     "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
       }
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
       }
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
      }
     }
    },
    "resources": [
      {
    "name": "[split(parameters('aksResourceId'),'/')[8]]",
    "type": "Microsoft.ContainerService/managedClusters",
    "location": "[parameters('aksResourceLocation')]",
    "apiVersion": "2018-03-31",
    "properties": {
      "mode": "Incremental",
      "id": "[parameters('aksResourceId')]",
      "addonProfiles": {
        "omsagent": {
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
        }
       }
     ]
    }
    ```

2. Sla dit bestand als **existingClusterOnboarding.json** naar een lokale map.
3. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscroptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "East US"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "eastus"
       }
     }
    }
    ```

4. Bewerk de waarde voor **aksResourceId**, **aksResourceLocation** met de waarden, die u kunt vinden op de **overzicht van AKS** -pagina voor het AKS-cluster.  De waarde voor **workspaceResourceId** is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte.  Geef ook de locatie van de werkruimte in voor **workspaceRegion**.    
5. Sla dit bestand als **existingClusterParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

    * Gebruik de volgende PowerShell-opdrachten uit de map met de sjabloon:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Wijzigen van de configuratie kan een paar minuten duren. Als deze is voltooid, ziet u een bericht dat lijkt op de volgende mogelijkheden van het resultaat:

        ```powershell
        provisioningState       : Succeeded
        ```

    * De volgende opdracht uit met Azure CLI uitvoeren op Linux:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Als deze is voltooid, ziet u een bericht dat lijkt op de volgende mogelijkheden van het resultaat:

        ```azurecli
        provisioningState       : Succeeded
        ```
Nadat de controle is ingeschakeld, kunnen duurt ongeveer 15 minuten voordat u zich kunt om te zien van operationele gegevens voor het cluster.  

## <a name="verify-agent-deployed-successfully"></a>Controleer of de agent is geïmplementeerd

### <a name="agent-version-06072018-and-higher"></a>Agentversie 06072018 en hoger
Om te controleren of de versie van de OMS-agent *06072018* of hoger correct is geïmplementeerd, voer de volgende opdrachten uit: 

```
kubectl get ds omsagent --namespace=kube-system
```

De uitvoer moet eruitzien zoals in de volgende aangeeft dat deze correct hebt geïmplementeerd:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Als u wilt controleren of een nieuwe implementatie, moet u de volgende opdracht uitvoeren:

```
kubectl get deployment omsagent-rs -n=kube-system
```

De uitvoer moet eruitzien zoals in de volgende aangeeft dat deze correct hebt geïmplementeerd:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Agent-versie ouder is dan 06072018

Om te controleren of de versie van de OMS-agent uitgebracht vóór *06072018* correct is geïmplementeerd met de volgende opdracht uitvoeren:  

```
kubectl get ds omsagent --namespace=kube-system
```

De uitvoer moet eruitzien zoals in de volgende aangeeft dat deze correct hebt geïmplementeerd:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Gebruik van de prestaties weergeven
Wanneer u containerstatus opent, geeft de pagina onmiddellijk het gebruik van de prestaties van de clusterknooppunten.  Informatie bekijken over uw AKS-cluster is onderverdeeld in drie perspectieven:

- Knooppunten 
- Controllers  
- Containers

De rij-hiërarchie volgt het Kubernetes-objectmodel beginnen met een knooppunt in het cluster.  Vouw het knooppunt en ziet u een of meer schillen die worden uitgevoerd op het knooppunt en als er meer dan één container gegroepeerd op een schil, worden deze weergegeven als de laatste rij in de hiërarchie.<br><br> ![Voorbeeld van de Kubernetes-knooppunt-hiërarchie in de weergave van agentprestaties](./media/monitoring-container-health/container-performance-and-health-view-03.png)

U kunt domeincontrollers of containers selecteren vanaf de bovenkant van de pagina en het gebruik van de status en resource voor die objecten bekijken.  Gebruik de vervolgkeuzelijsten boven aan het scherm om te filteren op de naamruimte, service en knooppunt. Als in plaats daarvan u bekijken geheugengebruik wilt, uit de **Metric** vervolgkeuzelijst selecteren **geheugen RSS** of **geheugenwerkset**.  **Geheugen RSS** wordt alleen ondersteund voor Kubernetes versie 1.8 en hoger. Anders is, ziet u waarden voor **Gem. %** weergegeven als *NaN %*, die is een waarde van het numerieke gegevens die een niet-gedefinieerde of sjabloontaal waarde vertegenwoordigt. 

![Container prestaties knooppunten Prestatieweergave](./media/monitoring-container-health/container-performance-and-health-view-04.png)

Standaard prestatiegegevens is gebaseerd op de afgelopen zes uur, maar kunt u het venster met de **tijdsbereik** vervolgkeuzelijst gevonden in de rechterbovenhoek van de pagina. Op dit moment is de pagina wordt niet automatisch vernieuwen, dus u moet dit handmatig vernieuwen. 

In het volgende voorbeeld ziet u voor het knooppunt *aks-agentpool-3402399-0*, de waarde voor **Containers** is 10, die een samenvoeging van het totale aantal containers die zijn geïmplementeerd is.<br><br> ![Updatepakket van containers per knooppunt voorbeeld](./media/monitoring-container-health/container-performance-and-health-view-07.png)<br><br> Het kan helpen u snel vaststellen of u geen een goede balans tussen containers tussen knooppunten in uw cluster hebt.  

De volgende tabel beschrijft de informatie die wordt weergegeven wanneer u knooppunten weergeven.

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de host |
| Status | Kubernetes-weergave van de status van het knooppunt |
| GEM. % | Gemiddelde knooppunt percentage op basis van de geselecteerde metrische gegevens voor de geselecteerde duur. |
| GEMIDDELD | Gemiddelde werkelijke waarde van knooppunten op basis van de geselecteerde metrische gegevens voor de geselecteerde duur.  De gemiddelde waarde wordt gemeten vanuit de CPU/geheugen die is ingesteld voor een knooppunt. het is de avg-waarde die wordt gerapporteerd door de host voor schillen en containers. |
| Containers | Het aantal containers. |
| Actieve tijdsduur | Geeft de tijd sinds een knooppunt is gestart of opnieuw is opgestart. |
| Pod | Alleen voor containers. Hier ziet u welke pods deze die zich bevindt. |
| Controllers | Alleen voor containers en schillen. Hier ziet u welke domeincontroller dat deze zich bevindt. Niet alle schillen bevindt zich in een domeincontroller, zodat sommige N.V.T. kunnen weergeven. | 
| Trend Gem. % | Staafdiagram trend op basis van de container en knooppunt avg metrische %. |


Kies in de lijst met **Controllers**.<br><br> ![Selecteer controllers weergeven](./media/monitoring-container-health/container-performance-and-health-view-08.png)

Hier ziet u de prestatiestatus van uw domeincontrollers.<br><br> ![< naam > controllers Prestatieweergave](./media/monitoring-container-health/container-performance-and-health-view-05.png)

De rij-hiërarchie begint met een domeincontroller en breidt de controller en ziet u een of meer schillen of een of meer containers.  Vouw een schil en de laatste rij weergeven in de container de schil gegroepeerd.  

De volgende tabel beschrijft de informatie die wordt weergegeven wanneer u domeincontrollers weergeeft.

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de controller|
| Status | Status van de containers is voltooid met met de status, zoals *beëindigd*, *mislukt* *gestopt*, of *onderbroken*. Als de container wordt uitgevoerd, maar de status is niet correct weergegeven of niet is doorgevoerd door de agent als meer dan dertig minuten niet heeft gereageerd, de status is *onbekende*. |
| GEM. % | Gemiddelde van het gemiddelde percentage van elke entiteit voor de geselecteerde metrische gegevens getotaliseerd. |
| GEMIDDELD | Samenvouwen van de gemiddelde CPU millicore of geheugen prestaties van de container.  De gemiddelde waarde wordt van de CPU/geheugen die is ingesteld voor een schil gemeten. |
| Containers | Totaal aantal containers voor de controller of de schil. |
| Opnieuw opstarten | Getotaliseerde u van het aantal opnieuw opstarten van containers. |
| Actieve tijdsduur | Geeft de tijd sinds een container is gestart. |
| Pod | Alleen voor containers. Hier ziet u welke pods deze die zich bevindt. |
| Knooppunt | Alleen voor containers en schillen. Hier ziet u welke domeincontroller dat deze zich bevindt. | 
| Trend Gem. % | Staafdiagram trend gemiddelde metrische % van de container wordt gepresenteerd. |

Kies in de lijst met **Containers**.<br><br> ![Selecteer containers weergeven](./media/monitoring-container-health/container-performance-and-health-view-09.png)

Hier ziet de prestatiestatus van uw containers.<br><br> ![< naam > controllers Prestatieweergave](./media/monitoring-container-health/container-performance-and-health-view-06.png)

De volgende tabel beschrijft de informatie die wordt weergegeven wanneer u Containers weergeven.

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de controller|
| Status | Getotaliseerde status van de containers, indien van toepassing. |
| GEM. % | Gemiddelde van het gemiddelde percentage van elke entiteit voor de geselecteerde metrische gegevens getotaliseerd. |
| GEMIDDELD | Samenvouwen van de gemiddelde CPU millicore of geheugen prestaties van de container. De gemiddelde waarde wordt van de CPU/geheugen die is ingesteld voor een schil gemeten. |
| Containers | Totaal aantal containers voor de controller.|
| Opnieuw opstarten | Geeft de tijd sinds een container is gestart. |
| Actieve tijdsduur | Geeft de tijd sinds een container is gestart of opnieuw opgestart. |
| Pod | Pod gegevens waar deze zich bevindt. |
| Knooppunt |  Het knooppunt waarin de container zich bevindt.  | 
| Trend Gem. % | Staafdiagram trend gemiddelde metrische % van de container wordt gepresenteerd. |

## <a name="container-data-collection-details"></a>Details van container gegevens verzamelen
Containerstatus verzamelt verschillende metrische gegevens en logboekbestanden prestatiegegevens van de hosts van de container en containers. Gegevens worden verzameld om de drie minuten.

### <a name="container-records"></a>Container-records

De volgende tabel ziet u voorbeelden van records die zijn verzameld door de containerstatus en de gegevenstypen die worden weergegeven in de resultaten van de logboekzoekopdracht.

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
Log Analytics kunt u zoeken naar bestedingstrends en een diagnose stellen van knelpunten, prognose, of het correleren gegevens die u kan helpen te bepalen of de huidige configuratie van het cluster optimaal presteert.  Vooraf gedefinieerde zoekopdrachten in Logboeken vindt u onmiddellijk kunt beginnen met of om aan te passen om de gegevens van de manier waarop die u wilt retourneren. 

U kunt interactieve analyses van gegevens in de werkruimte uitvoeren door het selecteren van de **logboek weergeven** optie beschikbaar is op de helemaal rechts wanneer u een container.  **Zoeken in logboeken** pagina wordt weergegeven boven de pagina die u al had geopend in de portal.<br><br> ![Gegevens analyseren in Log Analytics](./media/monitoring-container-health/container-performance-and-health-view-logs-01.png)   

De container logboeken uitvoer doorgestuurd naar Log Analytics zijn STDOUT en STDERR. Omdat de containerstatus wordt bewaakt door Azure beheerde Kubernetes (AKS), Kube-systeem niet vandaag verzameld vanwege de grote hoeveelheid gegevens die zijn gegenereerd.     

### <a name="example-log-search-queries"></a>Voorbeeld van de logboekbestanden zoekquery 's
Vaak is het handig om te maken van query's beginnen met een voorbeeld of twee en vervolgens aanpassen aan uw behoeften. U kunt experimenteren met de volgende voorbeeldquery's waarmee u meer geavanceerde query's.

| Query’s uitvoeren | Beschrijving | 
|-------|-------------|
| ContainerInventory<br> &#124;Computer, de naam, afbeelding, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime project<br> &#124;tabel weergeven | Lijst van alle Container levenscyclus van gegevens| 
| KubeEvents_CL<br> &#124;waar not(isempty(Namespace_s))<br> &#124;sorteren op TimeGenerated desc<br> &#124;tabel weergeven | Kubernetes-gebeurtenissen|
| ContainerImageInventory<br> &#124;summarize AggregatedValue = count() by afbeelding, ImageTag, actief | Voorraad | 
| **Selecteer in de Advanced Analytics lijndiagrammen**:<br> Voor prestaties<br> &#124;waarbij ObjectName == 'Container' en CounterName == "% processortijd"<br> &#124;samenvatten AvgCPUPercent avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container CPU | 
| **Selecteer in de Advanced Analytics lijndiagrammen**:<br> Perf &#124; waarbij ObjectName == 'Container' en CounterName == "MB geheugen gebruik"<br> &#124;samenvatten AvgUsedMemory avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container-geheugen |

## <a name="how-to-stop-monitoring-with-container-health"></a>Bewaking met de containerstatus stoppen
Na het inschakelen van bewaking van uw AKS-container die u besluit niet langer wenst te bewaken, kunt u *opt-out* met behulp van de opgegeven Azure Resource Manager-sjablonen met de PowerShell-cmdlet  **Nieuwe-AzureRmResourceGroupDeployment** of Azure CLI.  Een JSON-sjabloon Hiermee geeft u de configuratie van *opt-out* en de andere JSON-sjabloon bevat parameterwaarden die u configureert om op te geven van de AKS-cluster-ID en resource resourcegroep in het cluster is geïmplementeerd.  Als u niet bekend met de concepten bent van de implementatie van resources met behulp van een sjabloon met PowerShell, raadpleegt u [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) of Azure CLI Zie, voor [resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Als u het gebruik van Azure CLI, moet u eerst installeren en de CLI lokaal gebruikt.  Dit is vereist dat u de Azure CLI versie 2.0.27 of hoger. Voer `az --version` voor het identificeren van de versie. Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

### <a name="create-and-execute-template"></a>Maken en uitvoeren van sjabloon

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       }
    },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": {
                "logAnalyticsWorkspaceResourceID": null
              }
            }
           }
         }
       }
      ]
    }
    ```

2. Sla dit bestand als **OptOutTemplate.json** naar een lokale map.
3. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Bewerk de waarde voor **aksResourceId** en **aksResourceLocation** met de waarden van het AKS-cluster dat u kunt vinden op de **eigenschappen** pagina voor het geselecteerde cluster.<br><br> ![De eigenschappenpagina van container](./media/monitoring-container-health/container-properties-page.png)<br>

    Als u zich op de **eigenschappen** pagina, kopieert u ook de **Resource-ID van werkruimte**.  Deze waarde is vereist als u besluit dat u wilt verwijderen van de Log Analytics-werkruimte later, dit wordt niet uitgevoerd als onderdeel van dit proces.  

5. Sla dit bestand als **OptOutParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

    * Gebruik de volgende PowerShell-opdrachten uit de map met de sjabloon:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze is voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * De volgende opdracht uit met Azure CLI uitvoeren op Linux:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze is voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Als de werkruimte alleen ter ondersteuning van de bewaking van het cluster is gemaakt en deze niet meer nodig hebt, hebt u deze handmatig te verwijderen. Als u niet bekend bent met het verwijderen van een werkruimte, Zie [verwijderen van een Azure Log Analytics-werkruimte met de Azure-portal](../log-analytics/log-analytics-manage-del-workspace.md).  Vergeet niet over de **Resource-ID van werkruimte** we eerder in stap 4 hebt gekopieerd, gaat u die nodig hebt.  

## <a name="troubleshooting"></a>Problemen oplossen
Deze sectie bevat informatie over het oplossen van problemen met de containerstatus.

Als de containerstatus is ingeschakeld en geconfigureerd, maar u bent niet ziet de van statusinformatie of resultaten in Log Analytics wanneer u een zoekopdracht in Logboeken uitvoert, kunt u de volgende stappen uit om het probleem vast te stellen uitvoeren.   

1. Controleer de status van de agent door het uitvoeren van de volgende opdracht uit: 

    `kubectl get ds omsagent --namespace=kube-system`

    De uitvoer moet eruitzien zoals in de volgende aangeeft dat deze correct hebt geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Controleer de status van de implementatie voor agentversie *06072018* of hoger met de volgende opdracht:

    `kubectl get deployment omsagent-rs -n=kube-system`

    De uitvoer moet eruitzien zoals in de volgende aangeeft dat deze correct hebt geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Controleer de status van de schil om te controleren of dat deze wordt uitgevoerd of niet door de volgende opdracht uit: `kubectl get pods --namespace=kube-system`

    De uitvoer is vergelijkbaar met het volgende met de status van *met* voor de omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Raadpleeg de logboeken van de agent. Wanneer de agent in containers wordt geïmplementeerd, wordt een snelle controle uitgevoerd door het uitvoeren van OMI opdrachten en toont de versie van de agent en de Docker-provider. Als u wilt zien dat de agent opnieuw toegevoegd is, moet u de volgende opdracht uitvoeren: `kubectl logs omsagent-484hw --namespace=kube-system`

    De status moet lijken op het volgende:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Volgende stappen

[Zoeken in logboeken](../log-analytics/log-analytics-log-search.md) om gedetailleerde container status en toepassing prestatiegegevens weer te geven.  