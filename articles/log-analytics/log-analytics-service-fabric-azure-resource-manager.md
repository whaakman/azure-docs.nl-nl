---
title: Service Fabric-toepassingen met Log Analytics met Azure portal vast | Microsoft Docs
description: U kunt de Service Fabric-oplossing in Log Analytics met Azure portal voor het evalueren van de risico's en de status van uw Service Fabric-toepassingen, micro-services, knooppunten en clusters.
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: nini
ms.openlocfilehash: 8c564c0dcbb2f9be286917b2f4d8a40da5406fae
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-the-azure-portal"></a>Evalueer Service Fabric-toepassingen en micro-services met de Azure-portal

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![Service Fabric symbool](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

In dit artikel wordt beschreven hoe de Service Fabric-oplossing in Log Analytics gebruiken om te identificeren en oplossen van problemen in uw Service Fabric-cluster.

De Service Fabric-oplossing gebruikt Azure Diagnostics-gegevens van uw virtuele machines van Service Fabric door het verzamelen van deze gegevens uit uw af van de Azure-tabellen. Log Analytics leest vervolgens de Service Fabric framework gebeurtenissen, met inbegrip van **betrouwbare servicegebeurtenissen**, **Actor gebeurtenissen**, **operationele gebeurtenissen**, en **aangepaste ETW-gebeurtenissen**. Met het dashboard van de oplossing bent u problemen die aandacht vereisen en relevante gebeurtenissen weergeven in uw omgeving Service Fabric.

Als u wilt beginnen met de oplossing, moet u verbinding maken met uw Service Fabric-cluster een werkruimte voor logboekanalyse. Hier volgen drie scenario's te overwegen:

1. Als u uw Service Fabric-cluster niet hebt geïmplementeerd, gebruikt u de stappen in ***implementeren een Service Fabric-Cluster met een werkruimte voor logboekanalyse verbonden*** implementeren van een nieuw cluster en dit wordt geconfigureerd om te rapporteren met logboekanalyse.
2. Als u verzamelen van prestatiemeteritems van uw hosts wilt te gebruiken van andere OMS-oplossingen zoals beveiliging op uw Service Fabric-Cluster, volgt u de stappen in ***implementeren een Service Fabric-Cluster met een werkruimte voor logboekanalyse met VM-extensie is geïnstalleerd verbonden.***
3. Als u al uw Service Fabric-cluster en u wilt verbinden met Log Analytics hebt geïmplementeerd, volg de stappen in ***toevoegen van een bestaand opslagaccount met logboekanalyse.***

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Een Service Fabric-Cluster verbonden met een werkruimte voor logboekanalyse implementeren.
Deze sjabloon doet het volgende:

1. Een Azure Service Fabric-cluster al verbonden met een werkruimte voor logboekanalyse implementeert. U hebt de optie voor het maken van een nieuwe werkruimte tijdens de implementatie van de sjabloon of de naam van een bestaande werkruimte voor logboekanalyse invoeren.
2. De diagnostische storage-account toevoegen aan de werkruimte voor logboekanalyse
3. Hiermee schakelt u de Service Fabric-oplossing in de werkruimte voor logboekanalyse.

[![Implementeren in Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

Zodra u de bovenstaande implementeren knop selecteert, wordt de Azure-portal wordt geopend met parameters die u kunt bewerken. Zorg dat u een nieuwe resourcegroep maken als u een nieuwe Log Analytics-Werkruimtenaam invoeren:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Accepteer de juridische voorwaarden en klik op **maken** implementatie te starten. Zodra de implementatie is voltooid, ziet u de nieuwe werkruimte en cluster gemaakt en de WADServiceFabric * gebeurtenissen, WADWindowsEventLogs en WADETWEvent tabellen die zijn toegevoegd:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace-with-vm-extension-installed"></a>Implementeer een Service Fabric-Cluster verbonden met een werkruimte voor logboekanalyse met VM-extensie is geïnstalleerd.

Deze sjabloon doet het volgende:

1. Een Azure Service Fabric-cluster al verbonden met een werkruimte voor logboekanalyse implementeert. U kunt een nieuwe werkruimte maken of een bestaande gebruiken.
2. De diagnostische storage-accounts toevoegt aan de werkruimte voor logboekanalyse.
3. Hiermee schakelt u de Service Fabric-oplossing in de werkruimte voor logboekanalyse.
4. Installeert de uitbreiding van de agent MMA in elke virtuele-machineschaalset ingesteld in het Service Fabric-cluster. Met de MMA agent is geïnstalleerd, bent u maatstaven voor prestaties over uw knooppunten te bekijken.

[![Implementeren in Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

De vereiste parameters voor invoer na de bovenstaande dezelfde stappen en ere van een implementatie. Weer ziet u de nieuwe werkruimte, cluster en alle gemaakte af-tabellen:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Weergeven van prestatiegegevens

Prestatiegegevens van de knooppunten weergeven:


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- Start de werkruimte voor logboekanalyse vanuit de Azure-portal.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Ga naar instellingen in het linkerdeelvenster en selecteer gegevens >> Windows-prestatiemeteritems >> 'De geselecteerde prestatiemeteritems toevoegen': ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- In logboek zoeken, gebruikt u de volgende query's moet worden uitgevoerd in de belangrijkste metrische gegevens over uw knooppunten:

    a. Het gemiddelde CPU-gebruik op alle knooppunten in uw vergelijken in het afgelopen uur voor een om te zien welke knooppunten hebben problemen met en met welke tijdsinterval een knooppunt dat opnieuw moest een piek:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Vergelijkbare lijndiagrammen voor het beschikbare geheugen op elk knooppunt in deze query weergeven:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    Gebruik deze query om een lijst met alle knooppunten, waarbij de exacte gemiddelde waarde voor beschikbare megabytes (MB) voor elk knooppunt weer te geven:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. In het geval dat u inzoomen wilt op een specifiek knooppunt door te controleren voor de per uur gemiddeld, minimum, maximum en 75 percentiel CPU-gebruik, kun je je dit doen met behulp van deze query (vervangen veld Computer):

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

Meer informatie lezen over maatstaven voor prestaties in logboekanalyse op de [Operations Management Suite-blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-to-log-analytics"></a>Een bestaand opslagaccount toevoegen aan Log Analytics

Deze sjabloon wordt uw bestaande opslagaccounts gewoon toegevoegd aan een nieuwe of bestaande werkruimte voor logboekanalyse.

[![Implementeren in Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Als u met een bestaande werkruimte voor logboekanalyse werkt, selecteert u bij het selecteren van een resourcegroep 'Bestaande gebruiken' en zoek naar de resourcegroep met de werkruimte voor logboekanalyse. Maak een nieuwe één als anders.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

Nadat u deze sjabloon is geïmplementeerd, kunt u zich kunt zien van het opslagaccount dat is verbonden met uw werkruimte voor logboekanalyse. In dit exemplaar ik één meer opslagaccount toegevoegd aan de Exchange-werkruimte ik die eerder is gemaakt.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Service Fabric-gebeurtenissen weergeven

Zodra de implementaties zijn voltooid en de Service Fabric-oplossing in uw werkruimte is ingeschakeld, selecteert u de **Service Fabric** tegel in de portal voor logboekanalyse voor het starten van het Service Fabric-dashboard. Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat de bovenste 10 gebeurtenissen op het aantal die overeenkomt met de criteria die kolom voor de opgegeven periode. U kunt een zoekopdracht logboek waarmee de volledige lijst door te klikken op uitvoeren **alle** rechts onder van elke kolom, of door te klikken op de kolomkop.

| **Service Fabric-gebeurtenis** | **Beschrijving** |
| --- | --- |
| Problemen die aandacht vereisen |Een weergave van de problemen zoals RunAsyncFailures RunAsynCancellations en keuzelijsten knooppunt. |
| Operationele gebeurtenissen |Operationele gebeurtenissen zoals upgrade van de toepassing en implementaties aandacht vereisen. |
| Gebeurtenissen van betrouwbare Service |Opmerkelijke betrouwbare servicegebeurtenissen dergelijke Runasyncinvocations. |
| Actor-gebeurtenissen |Opmerkelijke actor-gebeurtenissen die worden gegenereerd door uw micro-services, zoals uitzonderingen worden veroorzaakt door een actormethode, actor-activeringen en deactivations, enzovoort. |
| Toepassingsgebeurtenissen |Alle aangepaste ETW-gebeurtenissen die worden gegenereerd door uw toepassingen. |

![Service Fabric-dashboard](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-dashboard](./media/log-analytics-service-fabric/sf4.png)

De volgende tabel bevat de methoden van de collectie en andere informatie over hoe gegevens worden verzameld voor Service Fabric.

| Platform | Directe Agent | Operations Manager-agent | Azure Storage | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Frequentie van de verzameling |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 minuten |

> [!NOTE]
> U kunt het bereik van deze gebeurtenissen in de Service Fabric-oplossing wijzigen door te klikken op **gegevens op basis van de afgelopen 7 dagen** aan de bovenkant van het dashboard. U kunt ook gebeurtenissen die worden gegenereerd in de afgelopen zeven dagen, een dag of zes uur weergeven. U kunt ook selecteren **aangepaste** om op te geven van een aangepast datumbereik.
>
>

## <a name="next-steps"></a>Volgende stappen

* Gebruik [logboek van zoekopdrachten in logboekanalyse](log-analytics-log-searches.md) om gedetailleerde gegevens van de Service Fabric-gebeurtenissen weer te geven.
