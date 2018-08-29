---
title: Evalueren van Service Fabric-toepassingen met Log Analytics met Azure portal | Microsoft Docs
description: U kunt de Service Fabric-oplossing in Log Analytics met Azure portal om het risico en de status van uw Service Fabric-toepassingen, microservices, knooppunten en -clusters vast te stellen.
services: log-analytics
documentationcenter: ''
author: niniikhena
manager: jochan
editor: ''
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: nini
ms.component: na
ms.openlocfilehash: 7c86c1006d7139356426c0cfb8fc5e684a4c9be6
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125675"
---
# <a name="assess-service-fabric-applications-and-micro-services-with-the-azure-portal"></a>Beoordeling van Service Fabric-toepassingen en microservices met de Azure-portal

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![Service Fabric-symbool](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

In dit artikel wordt beschreven hoe u de Service Fabric-oplossing in Log Analytics gebruiken om te identificeren en oplossen van problemen in uw Service Fabric-cluster.

De Service Fabric-oplossing maakt gebruik van Azure Diagnostics-gegevens van uw virtuele machines van Service Fabric door het verzamelen van deze gegevens uit uw Azure-WAD-tabellen. Log Analytics leest vervolgens de Service Fabric framework gebeurtenissen, met inbegrip van **Reliable servicegebeurtenissen**, **Actor gebeurtenissen**, **operationele gebeurtenissen**, en **aangepaste ETW-gebeurtenissen**. Met het dashboard van de oplossing bent u weergeven van problemen die aandacht vereisen en relevante gebeurtenissen in uw Service Fabric-omgeving.

Als u wilt beginnen met de oplossing, moet u uw Service Fabric-cluster verbinden met Log Analytics-werkruimte. Hier volgen drie scenario's:

1. Als u niet uw Service Fabric-cluster hebt geïmplementeerd, gebruikt u de stappen in ***implementeren die een Service Fabric-Cluster met een Log Analytics-werkruimte verbonden*** naar een nieuw cluster implementeren en deze hebt geconfigureerd voor rapportage aan Log Analytics.
1. Als u nodig hebt voor het verzamelen van prestatiemeteritems van uw hosts andere beheeroplossingen, zoals beveiliging op uw Service Fabric-Cluster gebruiken, volgt u de stappen in ***implementeren die een Service Fabric-Cluster met een Log Analytics-werkruimte met VM-extensie verbonden geïnstalleerd.***
1. Als u uw Service Fabric-cluster en u wilt verbinden met Log Analytics al hebt geïmplementeerd, volgt u de stappen in ***een bestaand opslagaccount toe te voegen aan Log Analytics.***

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Implementeer een Service Fabric-Cluster verbonden met een Log Analytics-werkruimte.
Deze sjabloon doet het volgende:

1. Hiermee wordt een Azure Service Fabric-cluster al verbonden met een Log Analytics-werkruimte geïmplementeerd. U hebt de optie voor het maken van een nieuwe werkruimte tijdens het implementeren van de sjabloon of geef de naam van een bestaande Log Analytics-werkruimte.
1. Het opslagaccount voor diagnostische gegevens toevoegt aan de Log Analytics-werkruimte.
1. Kan de Service Fabric-oplossing in uw Log Analytics-werkruimte.

[![Implementeren in Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

Wanneer u de bovenstaande knop implementeren selecteert, wordt de Azure-portal wordt geopend met parameters voor u om te bewerken. Zorg ervoor dat een nieuwe resourcegroep maken wanneer u een nieuwe naam van de Log Analytics-werkruimte:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Accepteer de juridische voorwaarden en klik op **maken** implementatie te starten. Zodra de implementatie is voltooid, ziet u de nieuwe werkruimte en cluster hebt gemaakt en de WADServiceFabric * gebeurtenissen, WADWindowsEventLogs en WADETWEvent tabellen die zijn toegevoegd:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace-with-vm-extension-installed"></a>Implementeer een Service Fabric-Cluster verbonden met een Log Analytics-werkruimte met VM-extensie is geïnstalleerd.

Deze sjabloon doet het volgende:

1. Hiermee wordt een Azure Service Fabric-cluster al verbonden met een Log Analytics-werkruimte geïmplementeerd. U kunt een nieuwe werkruimte maken of gebruik een bestaande resourcegroep.
1. De diagnostische storage-accounts toevoegt aan de Log Analytics-werkruimte.
1. Kan de Service Fabric-oplossing in de Log Analytics-werkruimte.
1. Installeert de MMA-agent-extensie in elke virtuele-machineschaalset in uw Service Fabric-cluster. Met de MMA-agent is geïnstalleerd, bent u metrische gegevens over uw knooppunten voor prestaties weergeven.

[![Implementeren in Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

De bovenstaande dezelfde stappen de benodigde invoerparameters en een vliegende start een implementatie. Weer ziet u de nieuwe werkruimte, cluster en alle gemaakte WAD-tabellen:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Prestatiegegevens te bekijken

Om weer te geven prestatiegegevens van uw knooppunten:


- Start de Log Analytics-werkruimte van de Azure-portal.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Ga naar instellingen in het linkerdeelvenster en selecteer gegevens >> Windows-prestatiemeteritems >> 'De geselecteerde prestatiemeteritems toevoegen': ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- In zoeken in Logboeken, gebruikt u de volgende query's naar dieper ingaan op belangrijke metrische gegevens over uw knooppunten:

    a. Vergelijk het gemiddelde CPU-gebruik voor alle uw knooppunten in het afgelopen uur om te zien welke knooppunten problemen ondervindt en met welke tijdsinterval een knooppunt heeft een piek:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Vergelijkbare lijndiagrammen voor beschikbaar geheugen op elk knooppunt met deze query weergeven:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    Gebruik deze query om een lijst met alle knooppunten, met de exacte gemiddelde waarde voor beschikbare megabytes (MB) voor elk knooppunt weer te geven:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. In het geval dat u wilt inzoomen op een specifiek knooppunt door te controleren van de gemiddelde per uur, de minimale, maximale en 75 percentiel CPU-gebruik, u kunt dit doen met behulp van deze query (veld Computer vervangen):

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

Meer informatie over metrische gegevens voor prestaties in Log Analytics op de [Operations Management Suite-blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-to-log-analytics"></a>Een bestaand opslagaccount toe te voegen aan Log Analytics

Deze sjabloon wordt gewoon uw bestaande opslagaccounts toegevoegd aan een nieuwe of bestaande Log Analytics-werkruimte.

[![Implementeren in Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Als u met een bestaande Log Analytics-werkruimte werkt, selecteert u bij het selecteren van een resourcegroep 'Bestaande gebruiken' en zoek naar de resourcegroep met de Log Analytics-werkruimte. Maak een nieuwe één als anders.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

Nadat u deze sjabloon is geïmplementeerd, kunt u zich om te zien van het opslagaccount dat is verbonden met uw Log Analytics-werkruimte. In dit geval, ik heb één meer opslagaccount toegevoegd aan de Exchange-werkruimte die ik hierboven hebt gemaakt.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Service Fabric-gebeurtenissen weergeven

Zodra de implementaties zijn voltooid en de Service Fabric-oplossing is ingeschakeld in uw werkruimte, selecteert u de **Service Fabric** tegel in de Log Analytics-portal om de Service Fabric-dashboard te openen. Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat de bovenste 10 gebeurtenissen op aantal die overeenkomen met de criteria van die kolom voor de opgegeven periode. U kunt een logboekzoekopdracht waarmee de volledige lijst door te klikken op uitvoeren **alle** linksonder elke kolom, of door op de kolomkop te klikken.

| **Service Fabric-gebeurtenis** | **Beschrijving** |
| --- | --- |
| Problemen die aandacht vereisen |Een weergave van problemen, zoals RunAsyncFailures RunAsynCancellations en knooppunt meer details. |
| Operationele gebeurtenissen |Operationele gebeurtenissen die aandacht vereisen, zoals de upgrade van de toepassing en implementaties. |
| Gebeurtenissen van de betrouwbare Service |Gebeurtenissen van die aandacht vereisen betrouwbare service die een Runasyncinvocations. |
| Actor-gebeurtenissen |Die aandacht vereisen actor-gebeurtenissen die worden gegenereerd door uw micro-services, zoals uitzonderingen die door een actormethode, actor activeringen en deactivations, enzovoort. |
| Toepassingsgebeurtenissen |Alle aangepaste ETW-gebeurtenissen die worden gegenereerd door uw toepassingen. |

![Service Fabric-dashboard](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-dashboard](./media/log-analytics-service-fabric/sf4.png)

De volgende tabel bevat de methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor Service Fabric.

| Platform | Agent toewijzen | Operations Manager-agent | Azure Storage | Operations Manager vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 minuten |

> [!NOTE]
> U kunt het bereik van deze gebeurtenissen in de Service Fabric-oplossing wijzigen door te klikken op **gegevens op basis van de afgelopen 7 dagen** aan de bovenkant van het dashboard. U kunt ook gebeurtenissen die worden gegenereerd in de afgelopen zeven dagen, een dag of 6 uur weergeven. U kunt ook selecteren **aangepaste** om op te geven van een aangepast datumbereik.
>
>

## <a name="next-steps"></a>Volgende stappen

* Gebruik [zoekopdrachten in Logboeken in Log Analytics](log-analytics-log-searches.md) om gedetailleerde gegevens voor Service Fabric-gebeurtenis weer te geven.
