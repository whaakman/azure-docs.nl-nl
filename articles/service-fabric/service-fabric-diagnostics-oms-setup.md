---
title: Azure Service Fabric - bewaking met OMS Log Analytics instellen | Microsoft Docs
description: Informatie over het instellen van OMS voor het visualiseren en analyseren van gebeurtenissen voor het controleren van uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2017
ms.author: dekapur
ms.openlocfilehash: 53b06c5a1395f34c96d4011366835a920d5c670b
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="set-up-oms-log-analytics-for-a-cluster"></a>OMS Log Analytics voor een cluster instellen

U kunt instellen wanneer er een OMS-werkruimte via Azure Resource Manager, PowerShell of via Azure Marketplace. Als u een bijgewerkte sjabloon voor Resource Manager van uw implementatie, dezelfde sjabloon voor toekomstig gebruik, gebruiken voor het instellen van uw omgeving OMS. Implementeren via de Marketplace is gemakkelijker als u al een cluster dat is geïmplementeerd met diagnostische gegevens die zijn ingeschakeld hebt. PowerShell gebruiken in het geval dat u bent niet gemachtigd abonnement niveau in het account waarmee u OMS implementeert, of via de Resource Manager-sjabloon implementeren.

> [!NOTE]
> Moet u beschikken over diagnostische gegevens ingeschakeld voor uw cluster om weer te geven van de cluster-platform niveau gebeurtenissen te kunnen instellen met succes van OMS om te controleren van uw cluster.

## <a name="deploying-oms-using-azure-marketplace"></a>Met behulp van Azure Marketplace OMS implementeren

Als u liever een OMS-werkruimte toevoegen nadat u een cluster hebt geïmplementeerd, Ga naar Azure Marketplace (in de Portal) en zoekt u *'Service Fabric Analytics'.*

1. Klik op **nieuw** in het navigatiemenu links. 

2. Zoeken naar *Service Fabric Analytics*. Klik op de resource die wordt weergegeven.

3. Klik op **maken**

    ![OMS SF analyses in de Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Klik in het venster van het maken van Service Fabric Analytics **Selecteer een werkruimte** voor de *OMS-werkruimte* veld en vervolgens **Maak een nieuwe werkruimte**. Vul de vereiste vermeldingen - hier de enige vereiste is dat het abonnement voor de Service Fabric-cluster en de OMS-werkruimte moet gelijk zijn. Nadat de gegevens zijn geverifieerd, wordt de OMS-werkruimte gaat implementeren. Dit moet alleen een paar minuten duren.

5. Wanneer u klaar bent, klikt u op **maken** opnieuw op de onderkant van het Service Fabric Analytics-venster maken. Zorg ervoor dat de nieuwe werkruimte wordt weergegeven onder *OMS-werkruimte*. De oplossing wordt toegevoegd aan de werkruimte die u zojuist hebt gemaakt.

Als u van Windows gebruikmaakt, gaat u verder met de volgende stappen OMS aansluiten op het opslagaccount waar uw Clustergebeurtenissen worden opgeslagen. Het inschakelen van deze ervaring voor Linux-clusters correct is nog in voortgang. Ondertussen, gaat u verder met de OMS-Agent aan het cluster toe te voegen.  

1. De werkruimte moet nog steeds zijn verbonden met de diagnostics-gegevens die afkomstig zijn van uw cluster. Navigeer naar de resourcegroep die u hebt gemaakt met de Service Fabric Analytics-oplossing in. U ziet een *ServiceFabric (\<nameOfOMSWorkspace\>)*. Klik op de oplossing om te navigeren naar de overzichtspagina van waar u Oplossingsinstellingen, werkruimte-instellingen wijzigen en navigeer naar de OMS-portal.

2. Klik in het navigatiemenu links op **opslagaccounts logboeken**onder *werkruimte gegevensbronnen*.

3. Op de *Storage account logboeken* pagina, klikt u op **toevoegen** aan de bovenkant van uw cluster logboeken toevoegen aan de werkruimte.

4. Klik op **opslagaccount** de juiste account gemaakt in het cluster toe te voegen. Als u de standaardnaam gebruikt, het storage-account heet *sfdg\<resourceGroupName\>*. U kunt dit ook bevestigen door het controleren van de Azure Resource Manager-sjabloon gebruikt voor het implementeren van uw cluster, door de waarde die wordt gebruikt voor de `applicationDiagnosticsStorageAccountName`. Misschien hebt u ook Schuif naar beneden en klik op **laden meer** als de naam niet wordt weergegeven. Klik op de juiste opslagaccountnaam omhoog om deze te selecteren.

5. Vervolgens hebt u om op te geven de *gegevenstype*, die moet worden ingesteld op **gebeurtenissen van de Service Fabric**.

6. De *bron* automatisch moet worden ingesteld op *WADServiceFabric\*EventTable*.

7. Klik op **OK** verbinding maken met uw werkruimte logboeken voor uw cluster.

    ![Logboeken voor storage-account toevoegen aan OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Het account moet nu worden weergegeven als onderdeel van uw *Storage account logboeken* in uw werkruimte-gegevensbronnen.

U hebt nu de Service Fabric Analytics-oplossing in een logboekanalyse OMS-werkruimte nu goed met uw cluster-platform en de logboektabel van toepassing verbonden is met dit toegevoegd. U kunt aanvullende bronnen toevoegen aan de werkruimte op dezelfde manier.


## <a name="deploying-oms-using-a-resource-manager-template"></a>OMS met een Resource Manager-sjabloon implementeren

Bij het implementeren van een cluster met behulp van een Resource Manager-sjabloon, het Service Fabric-oplossing aan de sjabloon moet een nieuwe OMS-werkruimte maken toevoegen en configureren voor het lezen van gegevens uit de tabellen met de juiste opslag.

[Hier](https://azure.microsoft.com/resources/templates/service-fabric-oms/) is een voorbeeldsjabloon die u kunt gebruiken en aanpassen volgens de vereisten. Meer sjablonen waarmee u verschillende opties voor het instellen van een OMS-werkruimte kan worden gevonden op [Service Fabric en OMS sjablonen](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

De belangrijkste wijzigingen zijn aangebracht, zijn de volgende:

1. Voeg `omsWorkspaceName` en `omsRegion` aan uw parameters. Dit betekent het volgende codefragment toe te voegen aan de gedefinieerde parameters in uw *template.json* bestand. U kunt de standaardwaarden wijzigen wens naar. U moet ook toevoegen met de twee nieuwe parameters in uw *parameters.json* om hun waarden voor de resource-implementatie te definiëren:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
    ```

    De `omsRegion` waarden moeten voldoen aan een specifieke set van de waarden. U moet kiezen die het dichtst bij de implementatie van het cluster.

2. Als u de logboekbestanden van de toepassing worden sturen met OMS, Controleer of de `applicationDiagnosticsStorageAccountType` en `applicationDiagnosticsStorageAccountName` zijn opgenomen als parameters in de sjabloon. Als dat niet het geval is, toe te voegen aan het gedeelte variabelen als volgt te werk en hun waarden indien nodig bewerken. U kunt ook deze opnemen als parameters, indien gewenst, de indeling die hierboven worden gebruikt na.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. De Service Fabric OMS-oplossing toevoegen aan uw sjabloon variabelen:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. De volgende wordt toegevoegd aan het einde van uw bronnensectie na waar de Service Fabric-clusterbron is gedeclareerd.

    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', parameters('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "id": "[resourceId('Microsoft.OperationsManagement/solutions/', variables('solution'))]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
    ```
    
    > [!NOTE]
    > Als u hebt toegevoegd de `applicationDiagnosticsStorageAccountName` als een variabele, zorg ervoor dat elke verwijzing naar deze wijzigen `variables('applicationDiagnosticsStorageAccountName')` in plaats van `parameters('applicationDiagnosticsStorageAccountName')`.

5. De sjabloon implementeren als een Resource Manager-upgrade voor uw cluster. Dit wordt gedaan met behulp van de `New-AzureRmResourceGroupDeployment` API in de AzureRM PowerShell-module. Een van de voorbeeldopdracht zou zijn:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager zich voor het detecteren van dit is een update aan voor een bestaande resource. Alleen wordt de wijzigingen tussen de sjabloon voor het besturen van de bestaande implementatie en de nieuwe sjabloon die verwerkt.

## <a name="deploying-oms-using-azure-powershell"></a>Met Azure PowerShell OMS implementeren

U kunt ook uw resource OMS Log Analytics via PowerShell implementeren. Dit wordt bereikt met behulp van de `New-AzureRmOperationalInsightsWorkspace` opdracht. Om dit doet, zorg ervoor dat u hebt geïnstalleerd [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Dit script gebruiken voor het maken van een nieuwe OMS Log Analytics-werkruimte en de Service Fabric-oplossing aan toe te voegen: 

```ps

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<OMS Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Zodra dit is voltooid, als uw cluster zich in een Windows-cluster, de stappen in de bovenstaande sectie OMS Log Analytics aansluiten op de juiste opslagaccount.

U kunt ook andere oplossingen toevoegen of andere wijzigingen aanbrengen in de OMS-werkruimte met behulp van PowerShell. Voor meer informatie over deze, Zie [Log Analytics beheren met behulp van PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md)

## <a name="next-steps"></a>Volgende stappen
* [De OMS-Agent implementeren](service-fabric-diagnostics-oms-agent.md) naar uw knooppunten om te verzamelen prestatiemeteritems en verzamelen van Logboeken voor uw containers en docker-statistieken
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse
* [Weergave Designer gebruiken voor het maken van aangepaste weergaven in Log Analytics](../log-analytics/log-analytics-view-designer.md)
