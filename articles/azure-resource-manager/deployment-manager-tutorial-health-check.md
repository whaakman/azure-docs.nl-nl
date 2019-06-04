---
title: Azure Deployment Manager gebruiken met Resource Manager-sjablonen | Microsoft Docs
description: Resource Manager-sjablonen met Azure Deployment Manager gebruiken om Azure-resources te implementeren.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/31/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d43a0e7c48db9dd42c7cf3b52e5d4072a4827898
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479180"
---
# <a name="tutorial-use-health-check-in-azure-deployment-manager-public-preview"></a>Zelfstudie: Controle van gatewayservicestatus gebruiken in Azure Deployment Manager (preview-versie)

Meer informatie over het integreren van statuscontrole in [Azure Deployment Manager](./deployment-manager-overview.md). In deze zelfstudie is gebaseerd op de [gebruik Azure Deployment Manager met Resource Manager-sjablonen](./deployment-manager-tutorial.md) zelfstudie. U moet deze zelfstudie hebt voltooid voordat u met deze doorgaat.

In de implementatie-sjabloon die wordt gebruikt [gebruik Azure Deployment Manager met Resource Manager-sjablonen](./deployment-manager-tutorial.md), een ogenblik geduld stap die wordt gebruikt. In deze zelfstudie, kunt u de stap wacht vervangen door een stap status controleren.

> [!IMPORTANT]
> Als uw abonnement is gemarkeerd voor Canarische voor het testen van nieuwe functies van Azure, kunt u alleen Azure Deployment Manager gebruiken om te implementeren naar de Canary regio's. 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een simulator health check-service maken
> * De sjabloon voor implementatie wijzigen
> * De topologie implementeren
> * Implementeren van de implementatie met een slechte status
> * Controleer of de implementatie-implementatie
> * Implementeren van de implementatie met de status in orde
> * Controleer of de implementatie-implementatie
> * Resources opschonen

De Azure Deployment Manager REST API-verwijzing vindt u [hier](https://docs.microsoft.com/rest/api/deploymentmanager/).

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* Volledige [gebruik Azure Deployment Manager met Resource Manager-sjablonen](./deployment-manager-tutorial.md).
* Download [de sjablonen en de artefacten](https://armtutorials.blob.core.windows.net/admtutorial/ADMTutorial.zip) die wordt gebruikt door deze zelfstudie.

## <a name="create-a-health-check-service-simulator"></a>Een simulator health check-service maken

In de productieomgeving, moet u doorgaans een of meer controle providers gebruiken. Als u wilt de gezondheid van integratie zo eenvoudig mogelijk te maken, is Microsoft gewerkt met enkele van de belangrijkste service voor health monitoring bedrijven om te voorzien van een oplossing voor eenvoudige kopiëren/plakken statuscontroles integreren in uw implementaties. Zie voor een lijst van deze bedrijven, [voor Health monitoring providers](./deployment-manager-health-check.md#health-monitoring-providers). In deze zelfstudie maakt u een [Azure-functie](/azure/azure-functions/) voor het simuleren van een health-service te controleren. Deze functie gebruikt een statuscode, en de dezelfde code retourneert. Uw Azure Deployment Manager-sjabloon maakt gebruik van de statuscode om te bepalen hoe om door te gaan met de implementatie.

De volgende twee bestanden worden gebruikt voor het implementeren van de Azure-functie. U hoeft niet te deze bestanden downloaden naar de zelfstudie te volgen.

* Een Resource Manager-sjabloon bevindt zich op [ https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json ](https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json). U implementeert deze sjabloon voor het maken van een Azure-functie.
* Een zip-bestand van de broncode van de Azure-functie [ http://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip ](http://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip). Dit zip met de naam wordt aangeroepen door de Resource Manager-sjabloon.

Selecteer voor het implementeren van de Azure-functie, **uitproberen** Azure Cloud shell openen en plak het volgende script in de shell-venster.  Als u de code, met de rechtermuisknop op de shell-venster en selecteer vervolgens **plakken**.

> [!IMPORTANT]
> **projectName** in de PowerShell-script voor het genereren van namen voor de Azure-services die zijn geïmplementeerd in deze zelfstudie wordt gebruikt. Verschillende Azure-services hebben verschillende vereisten voor de namen van. Om ervoor te zorgen voor dat de implementatie is gelukt, kies een naam in van minder dan 12 tekens met alleen kleine letters en cijfers.
> Sla een kopie van de naam van het project. U dezelfde projectName de zelfstudie.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json" -projectName $projectName
```

Om te controleren en testen van de Azure-functie:

1. Open de [Azure Portal](https://portal.azure.com).
1. Open de resourcegroep.  De standaardnaam is de naam van het project met **rg** toegevoegd.
1. Selecteer de appservice in de resourcegroep.  De standaardnaam van de app-service is de naam van het project met **webapp** toegevoegd.
1. Vouw **functies**, en selecteer vervolgens **HttpTrigger1**.

    ![Azure Deployment Manager statuscontrole van de Azure-functie](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-function.png)

1. Selecteer  **&lt;/ > functie-URL ophalen**.
1. Selecteer **kopie** naar de URL naar het Klembord kopiëren.  De URL is vergelijkbaar met:

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/{healthStatus}?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    Vervang `{healthStatus}` in de URL met een statuscode. In deze zelfstudie gebruikt **niet in orde** testen van het scenario niet in orde en gebruik een **in orde** of **waarschuwing** om de goede scenario te testen. Maak twee URL's, één met de slechte status en de andere met de status in orde. Voor voorbeelden:

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/healthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    U moet beide URL's in deze zelfstudie voltooid.

1. Als u wilt testen van de health monitoring simulator, opent u de URL's die u in de vorige stap hebt gemaakt.  De resultaten van de status niet in orde zijn vergelijkbaar met:

    ```
    Status: unhealthy
    ```

## <a name="revise-the-rollout-template"></a>De sjabloon voor implementatie wijzigen

Er is het doel van deze sectie leert u hoe u een stap status controleren in de sjabloon rollout opnemen. U hoeft te maken van uw eigen bestand CreateADMRollout.json om deze zelfstudie te voltooien. De sjabloon herziene rollout wordt gedeeld in een opslagaccount dat wordt gebruikt in de volgende secties worden uitgelegd.

1. Open **CreateADMRollout.json**. Deze JSON-bestand is een onderdeel van de download.  Zie [Vereisten](#prerequisites).
1. Voeg nog twee parameters:

    ```json
    "healthCheckUrl": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check URL."
        }
    },
    "healthCheckAuthAPIKey": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check Azure Function function authorization key."
        }
    }
    ```

1. Vervang de wachttijd stap resource-uitbreiding met een resourcedefinitie health check stap:

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT0M",
          "maxElasticDuration": "PT0M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "ApiKey",
                    "name": "code",
                    "in": "Query",
                    "value": "[parameters('healthCheckAuthAPIKey')]"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "Status: healthy",
                      "Status: warning"
                    ],
                    "matchQuantifier": "Any"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

    Op basis van de definitie, verloopt de implementatie als de status *in orde* of *waarschuwing*.

1. Update de **dependsON** van de definitie van de implementatie om op te nemen van de stap van de gedefinieerde status controleren:

    ```json
    "dependsOn": [
        "[resourceId('Microsoft.DeploymentManager/artifactSources', variables('rolloutArtifactSource').name)]",
        "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
    ],
    ```

1. Update **stepGroups** om op te nemen van de stap van de status controleren. De **healthCheckStep** heet in **postDeploymentSteps** van **stepGroup2**. **stepGroup3** en **stepGroup4** alleen worden geïmplementeerd als de status in orde *in orde* of *waarschuwing*.

    ```json
    "stepGroups": [
        {
            "name": "stepGroup1",
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup2",
            "dependsOnStepGroups": ["stepGroup1"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit1.name)]",
            "postDeploymentSteps": [
                {
                    "stepId": "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
                }
            ]
        },
        {
            "name": "stepGroup3",
            "dependsOnStepGroups": ["stepGroup2"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup4",
            "dependsOnStepGroups": ["stepGroup3"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit1.name)]",
            "postDeploymentSteps": []
        }
    ]
    ```

    Als u vergelijkt de **stepGroup3** sectie voordat en nadat deze is gewijzigd in deze sectie nu afhankelijk is van **stepGroup2**.  Dit is noodzakelijk wanneer **stepGroup3** en de groepen van de volgende stap is afhankelijk van de resultaten van de statuscontrole.

    De volgende schermafbeelding ziet u de gebieden die zijn gewijzigd, en hoe de stap van de status controleren wordt gebruikt:

    ![Azure Deployment Manager health check-sjabloon](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-rollout-template.png)

## <a name="deploy-the-topology"></a>De topologie implementeren

Ter vereenvoudiging van de zelfstudie, worden de sjabloon van de topologie en de artefacten gedeeld op de volgende locaties zodat u hoeft uw eigen kopie voorbereiden. Als u gebruiken uw eigen wilt, volgt u de instructies in [zelfstudie: Azure Deployment Manager gebruiken met Resource Manager-sjablonen](./deployment-manager-tutorial.md).

* Topologie sjabloon: https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplates/CreateADMServiceTopology.json
* Artefacten store: https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore

Selecteer voor het implementeren van de topologie **uitproberen** naar de cloudshell openen en plakt u het PowerShell-script.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the service topology
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMServiceTopology.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation
```

Controleer of de servicetopologie en de onderstreepte resources zijn gemaakt met behulp van Azure Portal:

![Geïmplementeerde servicetopologieresources in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

**Verborgen typen weergeven** moet worden geselecteerd om de resources weer te geven.

## <a name="deploy-the-rollout-with-the-unhealthy-status"></a>Implementeren van de implementatie met de slechte status

De sjabloon herziene rollout wordt gedeeld op de volgende locaties ter vereenvoudiging van de zelfstudie, zodat u hoeft uw eigen kopie voorbereiden. Als u gebruiken uw eigen wilt, volgt u de instructies in [zelfstudie: Azure Deployment Manager gebruiken met Resource Manager-sjablonen](./deployment-manager-tutorial.md).

* Topologie sjabloon: https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json
* Artefacten store: https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore

Gebruik de slechte status-URL die u hebt gemaakt in [maken van een health check service simulator](#create-a-health-check-service-simulator). Voor **managedIdentityID**, Zie [maken van de gebruiker toegewezen beheerde identiteit](./deployment-manager-tutorial.md#create-the-user-assigned-managed-identity).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$managedIdentityID = Read-Host -Prompt "Enter a user-assigned managed identity"
$healthCheckUrl = Read-Host -Prompt "Enter the health check Azure function URL"
$healthCheckAuthAPIKey = $healthCheckUrl.Substring($healthCheckUrl.IndexOf("?code=")+6, $healthCheckUrl.Length-$healthCheckUrl.IndexOf("?code=")-6)
$healthCheckUrl = $healthCheckUrl.Substring(0, $healthCheckUrl.IndexOf("?"))

$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the rollout
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation `
    -managedIdentityID $managedIdentityID `
    -healthCheckUrl $healthCheckUrl `
    -healthCheckAuthAPIKey $healthCheckAuthAPIKey
```

> [!NOTE]
> `New-AzResourceGroupDeployment` is een asynchrone aanroep. Het succes bericht betekent dat de implementatie met succes is gestart. Gebruiken om te controleren of de implementatie, `Get-AZDeploymentManagerRollout`.  Zie de volgende procedure.

Controleer de voortgang van de implementatie met behulp van de volgende PowerShell-script:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$resourceGroupName = "${projectName}rg"
$rolloutName = "${projectName}Rollout"

# Get the rollout status
Get-AzDeploymentManagerRollout `
    -ResourceGroupName $resourceGroupName `
    -Name $rolloutName `
    -Verbose
```

De volgende voorbeelduitvoer ziet u de implementatie is mislukt vanwege een slechte status:

```output
Service: myhc0417ServiceWUSrg
    TargetLocation: WestUS
    TargetSubscriptionId: <Subscription ID>

    ServiceUnit: myhc0417ServiceWUSWeb
        TargetResourceGroup: myhc0417ServiceWUSrg

        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
Get-AzDeploymentManagerRollout :
Service: myhc0417ServiceWUSrg
    ServiceUnit: myhc0417ServiceWUSWeb
        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
At line:1 char:1
+ Get-AzDeploymentManagerRollout `
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Get-AzDeploymentManagerRollout], Exception
+ FullyQualifiedErrorId : RolloutFailed,Microsoft.Azure.Commands.DeploymentManager.Commands.GetRollout


ResourceGroupName       : myhc0417rg
BuildVersion            : 1.0.0.0
ArtifactSourceId        : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/artifactSources/myhc0417ArtifactSourceRollout
TargetServiceTopologyId : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/serviceTopologies/myhc0417ServiceTopology
Status                  : Failed
TotalRetryAttempts      : 0
Identity                : Microsoft.Azure.Commands.DeploymentManager.Models.PSIdentity
OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
Services                : {myhc0417ServiceWUS, myhc0417ServiceWUSrg}
Name                    : myhc0417Rollout
Type                    : Microsoft.DeploymentManager/rollouts
Location                : centralus
Id                      : /subscriptions/<Subscription ID>/resourcegroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/rollouts/myhc0417Rollout
Tags                    :
```

Nadat de implementatie is voltooid, ziet u een extra resourcegroep gemaakt voor VS-West.

## <a name="deploy-the-rollout-with-the-healthy-status"></a>Implementeren van de implementatie met de status in orde

Herhaal deze sectie voor het implementeren van de implementatie met de URL van de status in orde.  Nadat de implementatie is voltooid, ziet u een meer resourcegroep gemaakt voor VS-Oost.

## <a name="verify-the-deployment"></a>De implementatie controleren

1. Open de [Azure Portal](https://portal.azure.com).
2. Blader naar de zojuist gemaakte webtoepassingen onder de nieuwe resourcegroepen die zijn gemaakt door de implementatie.
3. Open de webtoepassing in een webbrowser. Controleer de locatie en de versie in het index.html-bestand.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Gebruik het veld **Filteren op naam** om u te beperken tot de resourcegroepen die u in deze zelfstudie hebt gemaakt. Er zijn er 3-4:

    * **&lt;namePrefix > rg**: bevat de Deployment Manager-resources.
    * **&lt;namePrefix > ServiceWUSrg**: bevat de resources die zijn gedefinieerd door ServiceWUS.
    * **&lt;namePrefix>ServiceEUSrg**: bevat de resources die zijn gedefinieerd door ServiceEUS.
    * De resourcegroep voor de door de gebruiker gedefinieerde beheerde identiteit.
3. Selecteer de naam van de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.
5. Herhaal de laatste twee stappen als u andere resourcegroepen wilt verwijderen die zijn gemaakt in deze zelfstudie.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de functie voor status van Azure Deployment Manager. Zie [de documentatie bij Azure Resource Manager](/azure/azure-resource-manager/) voor meer informatie.
