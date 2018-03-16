---
title: Azure Service Fabric - bewaking met OMS Log Analytics ingesteld | Microsoft Docs
description: Informatie over het instellen van Operations Management Suite voor het visualiseren en analyseren van gebeurtenissen voor het bewaken van uw Azure Service Fabric-clusters.
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
ms.openlocfilehash: 98ac32b011744ce388762322edd538b467f93494
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="set-up-operations-management-suite-log-analytics-for-a-cluster"></a>Operations Management Suite Log Analytics voor een cluster instellen

U kunt instellen wanneer er een werkruimte van Operations Management Suite (OMS) via Azure Resource Manager, PowerShell of Azure Marketplace. Als u een bijgewerkte sjabloon voor Resource Manager van uw implementatie voor toekomstig gebruik onderhoudt, moet u dezelfde sjabloon gebruiken voor het instellen van uw omgeving OMS. Implementatie via de Marketplace is eenvoudiger als u al een cluster dat is geïmplementeerd met diagnostische gegevens die zijn ingeschakeld. Als u geen abonnement toegang in het account waarmee u OMS implementeert, implementeert u met behulp van PowerShell of de Resource Manager-sjabloon.

> [!NOTE]
> Als u OMS instelt voor het bewaken van uw cluster, moet u diagnostische gegevens die zijn ingeschakeld voor de weergave van gebeurtenissen voor cluster- of platform-niveau hebben.

## <a name="deploy-oms-by-using-azure-marketplace"></a>OMS implementeren met behulp van Azure Marketplace

Als u een OMS-werkruimte toevoegen wilt nadat u een cluster hebt geïmplementeerd, Ga naar Azure Marketplace in de portal en zoek naar **Service Fabric Analytics**:

1. Selecteer **nieuw** in het navigatiemenu links. 

2. Zoeken naar **Service Fabric Analytics**. Selecteer de resource die wordt weergegeven.

3. Selecteer **Maken**.

    ![OMS SF analyses in de Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Selecteer in het venster van het maken van Service Fabric Analytics **Selecteer een werkruimte** voor de **OMS-werkruimte** veld en vervolgens **Maak een nieuwe werkruimte**. Vul de vereiste vermeldingen. De enige vereiste hier is het abonnement voor de Service Fabric-cluster en de OMS-werkruimte is hetzelfde. Als uw vermeldingen zijn geverifieerd, wordt de OMS-werkruimte gestart om te implementeren. De implementatie duurt slechts enkele minuten.

5. Wanneer u klaar bent, selecteert u **maken** opnieuw op de onderkant van het Service Fabric Analytics-venster maken. Zorg ervoor dat de nieuwe werkruimte wordt weergegeven onder **OMS-werkruimte**. Deze actie worden de oplossing toegevoegd aan de werkruimte die u hebt gemaakt.

Als u van Windows gebruikmaakt, gaat u verder met de volgende stappen uit om met OMS het opslagaccount waar uw Clustergebeurtenissen worden opgeslagen. 

>[!NOTE]
>Inschakelen van deze ervaring voor Linux-clusters is nog niet beschikbaar. 

### <a name="connect-the-oms-workspace-to-your-cluster"></a>Verbinding maken met de OMS-werkruimte van het cluster 

1. De werkruimte moet worden verbonden met de diagnostics-gegevens die afkomstig zijn van uw cluster. Ga naar de resourcegroep waarin u de Service Fabric Analytics-oplossing hebt gemaakt. Selecteer **ServiceFabric\<nameOfOMSWorkspace\>**  en Ga naar de overzichtspagina. U kunt daar instellingen, werkruimte-instellingen wijzigen en toegang tot de OMS-portal.

2. In het menu linkernavigatievenster onder **werkruimte gegevensbronnen**, selecteer **opslagaccounts logboeken**.

3. Op de **Storage account logboeken** pagina **toevoegen** aan de bovenkant van uw cluster logboeken toevoegen aan de werkruimte.

4. Selecteer **opslagaccount** de juiste account gemaakt in het cluster toe te voegen. Als u de standaardnaam gebruikt, het opslagaccount is **sfdg\<resourceGroupName\>**. U kunt dit ook bevestigen met de Azure Resource Manager-sjabloon gebruikt voor het implementeren van uw cluster, door de waarde die wordt gebruikt voor **applicationDiagnosticsStorageAccountName**. Als de naam niet wordt weergegeven, schuif naar beneden en selecteer **laden meer**. Selecteer de naam van het opslagaccount.

5. Geef het gegevenstype. Stel deze in op **gebeurtenissen van de Service Fabric**.

6. Zorg ervoor dat de bron wordt automatisch ingesteld op **WADServiceFabric\*EventTable**.

7. Selecteer **OK** verbinding maken met uw werkruimte logboeken voor uw cluster.

    ![Logboeken voor storage-account toevoegen aan OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Het account nu worden weergegeven als onderdeel van uw opslagaccount wordt geregistreerd in uw werkruimte-gegevensbronnen.

U kunt de Service Fabric Analytics-oplossing hebt toegevoegd in een logboekanalyse OMS-werkruimte nu goed met uw cluster-platform en de logboektabel van toepassing verbonden is. U kunt aanvullende bronnen toevoegen aan de werkruimte op dezelfde manier.


## <a name="deploy-oms-by-using-a-resource-manager-template"></a>OMS implementeren met behulp van een Resource Manager-sjabloon

Wanneer u een cluster met een Resource Manager-sjabloon implementeert, de sjabloon is, wordt een nieuwe OMS-werkruimte maakt, voegt de Service Fabric-oplossing naar de werkruimte en geconfigureerd voor het lezen van gegevens uit de tabellen met de juiste opslag.

U kunt gebruiken en wijzigen [deze voorbeeldsjabloon](https://azure.microsoft.com/resources/templates/service-fabric-oms/) om te voldoen aan uw vereisten. Sjablonen waarmee u verschillende opties voor het instellen van een OMS-werkruimte kan worden gevonden op [Service Fabric en OMS sjablonen](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

De volgende wijzigingen aanbrengen:
1. Voeg `omsWorkspaceName` en `omsRegion` aan uw parameters door het volgende codefragment toe te voegen aan de gedefinieerde parameters in uw *template.json* bestand. U kunt de standaardwaarden wijzigen wens naar. Voeg ook de twee nieuwe parameters in uw *parameters.json* bestand om hun waarden voor de resource-implementatie te definiëren:
    
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

    De `omsRegion` waarden moeten voldoen aan een specifieke set van de waarden. Kiezen die het dichtst bij de implementatie van het cluster.

2. Als u de logboekbestanden van de toepassing met OMS verzendt, eerst bevestigen dat de `applicationDiagnosticsStorageAccountType` en `applicationDiagnosticsStorageAccountName` zijn opgenomen als parameters in de sjabloon. Als ze niet opgenomen zijn, toe te voegen aan het gedeelte variabelen en hun waarden indien nodig bewerken. U kunt ze ook opnemen als parameters aan de hand van de voorgaande indeling.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. De Service Fabric OMS-oplossing toevoegen aan uw sjabloon variabelen:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Voeg het volgende toe aan het einde van uw bronnensectie na waar de Service Fabric-clusterbron is gedeclareerd:

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

5. De sjabloon als een Resource Manager-upgrade voor uw cluster implementeren met behulp van de `New-AzureRmResourceGroupDeployment` API in de AzureRM PowerShell-module. Een van de voorbeeldopdracht zou zijn:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager heeft vastgesteld dat deze opdracht een update aan voor een bestaande resource. Wordt alleen de wijzigingen tussen de sjabloon voor het besturen van de bestaande implementatie en de nieuwe sjabloon opgegeven verwerkt.

## <a name="deploy-oms-by-using-azure-powershell"></a>OMS implementeren met behulp van Azure PowerShell

U kunt ook uw resource OMS Log Analytics via PowerShell implementeren met behulp van de `New-AzureRmOperationalInsightsWorkspace` opdracht. Als u deze methode gebruikt, zorg ervoor dat u hebt geïnstalleerd [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Dit script gebruiken voor het maken van een nieuwe OMS Log Analytics-werkruimte en de Service Fabric-oplossing aan toe te voegen: 

```PowerShell

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

Wanneer u bent klaar, de stappen in de vorige sectie OMS Log Analytics verbinding met het juiste storage-account.

U kunt ook andere oplossingen toevoegen of andere wijzigingen aanbrengen in de OMS-werkruimte met behulp van PowerShell. Zie voor meer informatie, [Log Analytics beheren met behulp van PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Volgende stappen
* [De OMS-Agent implementeren](service-fabric-diagnostics-oms-agent.md) naar uw knooppunten om te verzamelen prestatiemeteritems en verzamelen van Logboeken voor uw containers en docker-statistieken
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse
* [Weergave Designer gebruiken voor het maken van aangepaste weergaven in Log Analytics](../log-analytics/log-analytics-view-designer.md)
