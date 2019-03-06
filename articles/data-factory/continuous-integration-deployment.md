---
title: Continue integratie en levering in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van continue integratie en levering Data Factory-pijplijnen verplaatsen van de ene omgeving (ontwikkeling, testen, productie) naar een andere.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: douglasl
ms.openlocfilehash: bfab3c94892b94eaf1c0585ee47a6dcbdb161776
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57452722"
---
# <a name="continuous-integration-and-delivery-cicd-in-azure-data-factory"></a>Continue integratie en levering (CI/CD) in Azure Data Factory

Continue integratie is het testen van elke wijziging gereed om terug te uw codebasis automatisch en zo vroeg mogelijk. Continue levering volgt de tests die plaatsvindt tijdens de continue integratie en verstuurd van wijzigingen naar een systeem fasering of productie.

Betekent verplaatsen Data Factory-pijplijnen van de ene omgeving (ontwikkeling, testen, productie) naar een andere voor Azure Data Factory, continue integratie en levering. Als u wilt doen continue integratie en levering, kunt u Data Factory-UI-integratie met Azure Resource Manager-sjablonen. De gebruikersinterface van Data Factory een Resource Manager-sjabloon kunt genereren wanneer u selecteert de **ARM-sjabloon** opties. Wanneer u selecteert **exporteren ARM-sjabloon**, de portal voor het Resource Manager-sjabloon voor de data factory en een configuratiebestand met alle tekenreeksen in uw verbindingen en andere parameters wordt gegenereerd. Vervolgens moet u maken van een configuratiebestand voor elke omgeving (ontwikkeling, testen, productie). Het belangrijkste Resource Manager-sjabloonbestand blijft hetzelfde voor alle omgevingen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bekijk de volgende video voor een 9 minuten durende inleiding en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

## <a name="create-a-resource-manager-template-for-each-environment"></a>Een Resource Manager-sjabloon voor elke omgeving maken
Selecteer **exporteren ARM-sjabloon** voor het exporteren van de Resource Manager-sjabloon voor uw gegevensfactory in de ontwikkelomgeving.

![](media/continuous-integration-deployment/continuous-integration-image1.png)

Vervolgens gaat u naar uw gegevensfactory testen en productie data factory en selecteer **importeren ARM-sjabloon**.

![](media/continuous-integration-deployment/continuous-integration-image2.png)

Hiermee gaat u naar de Azure-portal, waar u de geëxporteerde sjabloon kunt importeren. Selecteer **bouw uw eigen sjabloon in de editor** en vervolgens **bestand laden** en selecteer de gegenereerde Resource Manager-sjabloon. Geef de instellingen en de data factory en de hele pijplijn wordt geïmporteerd in uw productieomgeving.

![](media/continuous-integration-deployment/continuous-integration-image3.png)

![](media/continuous-integration-deployment/continuous-integration-image4.png)

Selecteer **bestand laden** voor het selecteren van de geëxporteerde Resource Manager-sjabloon en geef alle configuratiewaarden (bijvoorbeeld, gekoppelde services).

![](media/continuous-integration-deployment/continuous-integration-image5.png)

**Tekenreeksen voor databaseverbindingen**. U kunt de informatie die is vereist voor het maken van verbindingsreeksen in de artikelen over de afzonderlijke connectors vinden. Bijvoorbeeld, voor Azure SQL Database, Zie [gegevens kopiëren naar of van Azure SQL Database met behulp van Azure Data Factory](connector-azure-sql-database.md). Om te controleren of de juiste verbindingsreeks - voor een gekoppelde service, bijvoorbeeld: kunt u ook openen codeweergave voor de resource in de gebruikersinterface van Data Factory. In de weergave van code, maar het wachtwoord of account sleutel gedeelte van de verbindingsreeks verwijderd. Als codeweergave wilt openen, selecteert u het pictogram gemarkeerd in de volgende schermafbeelding.

![Open-code weer te geven van de verbindingsreeks](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="continuous-integration-lifecycle"></a>Levenscyclus van continue integratie
Hier wordt de hele levenscyclus voor continue integratie en levering die u kunt gebruiken nadat u Azure-opslagplaatsen Git-integratie in de Data Factory-gebruikersinterface ingeschakeld:

1.  Instellen van een development data factory met Azure-opslagplaatsen waarin alle ontwikkelaars Data Factory-resources, zoals de pijplijnen en gegevenssets kunnen maken.

1.  Ontwikkelaars kunnen vervolgens resources zoals pijplijnen wijzigen. Wanneer ze hun wijzigingen aanbrengt, kunnen ze selecteren **Debug** om te zien hoe de pijplijn wordt uitgevoerd met de meest recente wijzigingen.

1.  Nadat ontwikkelaars tevreden met hun wijzigingen bent, kunnen ze een pull-aanvraag maken van de vertakking naar de master-vertakking (of de vertakking samenwerking) om op te halen van hun wijzigingen gecontroleerd door de peers.

1.  Nadat wijzigingen in de master-vertakking zijn, kunnen ze publiceren naar de factory ontwikkeling door te selecteren **publiceren**.

1.  Wanneer het team gereed om te promoten wijzigingen in de fabriek test en de productie-factory is, kunnen zij de Resource Manager-sjabloon exporteren vanuit de master-vertakking of vanuit een andere vertakking in het geval de master-vertakking back-ups de live Data Factory-ontwikkeling.

1.  De geëxporteerde Resource Manager-sjabloon kan worden geïmplementeerd met andere parameterbestanden voor de factory test en de productie-factory.

## <a name="automate-continuous-integration-with-azure-pipelines-releases"></a>Continue integratie met Azure-pijplijnen releases automatiseren

Hier volgen de stappen voor het instellen van een Azure-pijplijnen release, zodat u kunt de implementatie van een data factory om meerdere omgevingen te automatiseren.

![Diagram van continue integratie met Azure-pijplijnen](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Vereisten

-   Een Azure-abonnement is gekoppeld voor het gebruik van Team Foundation Server of Azure-opslagplaatsen de [*Azure Resource Manager-service-eindpunt*](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

-   Een Data Factory met Azure-opslagplaatsen Git-integratie is geconfigureerd.

-   Een [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) met de geheimen.

### <a name="set-up-an-azure-pipelines-release"></a>Instellen van een Azure-pijplijnen-versie

1.  Ga naar de pagina van uw Azure-opslagplaatsen in hetzelfde project als het account dat is geconfigureerd met de Data Factory.

1.  Klik op het bovenste menu **Azure pijplijnen** &gt; **Releases** &gt; **release-definitie maken**.

    ![](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  Selecteer de **leeg proces** sjabloon.

1.  Voer de naam van uw omgeving.

1.  Een Git-artefact toevoegen en selecteer de dezelfde opslagplaats is geconfigureerd met de Data Factory. Kies `adf_publish` als de standaardvertakking met de meest recente standaardversie.

    ![](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Een implementatie van Azure Resource Manager-taak toevoegen:

    a.  Nieuwe taak maken, te zoeken naar **Azure Resource Group Deployment**, en voeg deze toe.

    b.  In de implementatie-taak, kies het abonnement, resourcegroep en locatie voor het doel van Data Factory en referenties opgeven indien nodig.

    c.  Selecteer de **resourcegroep maken of bijwerken** actie.

    d.  Selecteer **...** in de **sjabloon** veld. Bladeren naar het Resource Manager-sjabloon (*ARMTemplateForFactory.json*) die is gemaakt door de actie publiceren in de portal. Zoek naar dit bestand in de map `<FactoryName>` van de `adf_publish` vertakking.

    e.  Doe hetzelfde voor de parameterbestand. Kies het juiste bestand, afhankelijk van of u een kopie gemaakt of u het standaardbestand *ARMTemplateParametersForFactory.json*.

    f.  Selecteer **...** naast de **overschrijven Sjabloonparameters** veld en vul de gegevens voor het doel van Data Factory. Voor de referenties die worden geleverd vanuit key vault, gebruikt u dezelfde naam voor het geheim in de volgende indeling: ervan uitgaande dat de naam van het geheim is `cred1`, voer `"$(cred1)"` (tussen aanhalingstekens).

    ![](media/continuous-integration-deployment/continuous-integration-image9.png)

    g. Selecteer de **incrementele** Implementatiemodus.

    > [!WARNING]
    > Als u selecteert **voltooid** implementatiemodus, bestaande bronnen kan worden verwijderd, met inbegrip van alle resources in de doelresourcegroep die niet zijn gedefinieerd in het Resource Manager-sjabloon.

1.  Sla de release-pijplijn.

1.  Maak een nieuwe versie van deze release-pijplijn.

    ![](media/continuous-integration-deployment/continuous-integration-image10.png)

### <a name="optional---get-the-secrets-from-azure-key-vault"></a>Optioneel: de geheimen ophalen uit Azure Key Vault

Als u geheimen om door te geven in een Azure Resource Manager-sjabloon hebt, raden wij met behulp van Azure Key Vault met de release van Azure-pijplijnen.

Er zijn twee manieren voor het afhandelen van de geheimen:

1.  De geheimen toevoegen aan het parameterbestand. Zie voor meer informatie, [Azure Key Vault gebruikt om door te geven beveiligde parameterwaarde tijdens de implementatie van](../azure-resource-manager/resource-manager-keyvault-parameter.md).

    -   Maak een kopie van de parameterbestand dat is geüpload naar de vertakking publiceren en stel de waarden van de parameters die u wilt ophalen uit key vault met de volgende indeling:

    ```json
    {
        "parameters": {
            "azureSqlReportingDbPassword": {
                "reference": {
                    "keyVault": {
                        "id": "/subscriptions/<subId>/resourceGroups/<resourcegroupId> /providers/Microsoft.KeyVault/vaults/<vault-name> "
                    },
                    "secretName": " < secret - name > "
                }
            }
        }
    }
    ```

    -   Wanneer u deze methode gebruikt, wordt het geheim automatisch opgehaald uit de key vault.

    -   Het parameterbestand moet zich in de vertakking publiceren ook.

1.  Voeg een [Azure Key Vault-taak](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) voordat u de Azure Resource Manager-implementatie in de vorige sectie beschreven:

    -   Selecteer de **taken** tabblad, een nieuwe taak maken, zoekt u **Azure Key Vault** en toe te voegen.

    -   Kies in de Key Vault-taak, het abonnement waarin u de sleutelkluis hebt gemaakt, geeft u indien nodig referenties in en kies vervolgens de key vault.

    ![](media/continuous-integration-deployment/continuous-integration-image8.png)

### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Machtigingen verlenen voor de Azure-pijplijnen-agent
De Azure Key Vault-taak mislukken de tijd van de Runtime fIntegration met een foutbericht over geweigerde toegang. De logboeken voor de versie te downloaden, en Ga naar de `.ps1` bestand met de opdracht machtigingen geven tot de Azure-pijplijnen-agent. U kunt de opdracht rechtstreeks uitvoeren, of u kunt kopiëren van de principal-ID uit het bestand en het handmatig toevoegen van het toegangsbeleid in Azure portal. (*Ophalen* en *lijst* zijn de minimale machtigingen die zijn vereist).

### <a name="update-active-triggers"></a>Actieve triggers bijwerken
Implementatie kan mislukken als u probeert active triggers bijwerken. Voor het actieve triggers bijwerken, moet u handmatig ze stopt en start deze na de implementatie. U kunt een Azure Powershell-taak toevoegen voor dit doel, zoals wordt weergegeven in het volgende voorbeeld:

1.  Zoek in het tabblad taken van de release van **Azure Powershell** en toe te voegen.

1.  Kies **Azure Resource Manager** als de verbinding typt en selecteer uw abonnement.

1.  Kies **Inline Script** als het script typt en geef vervolgens uw code. Het volgende voorbeeld stopt de triggers:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

    ![](media/continuous-integration-deployment/continuous-integration-image11.png)

U kunt uitvoeren van gelijksoortige stappen en vergelijkbare code gebruiken (met de `Start-AzDataFactoryV2Trigger` functie) opnieuw op te starten van de triggers na de implementatie.

> [!IMPORTANT]
> Continue integratie en implementatie-scenario's moet het type Integratieruntime in verschillende omgevingen hetzelfde zijn. Als u hebt bijvoorbeeld een *zelf-Hostend* Integration Runtime (IR) in de ontwikkelomgeving, dezelfde IR moet van het type *zelf-Hostend* in andere omgevingen zoals test- en productieomgevingen ook. Op dezelfde manier als u integratieruntimes in meerdere fasen deelt, u moet configureren de Integratieruntimes als *gekoppelde zelf-Hostend* in alle omgevingen, zoals ontwikkeling, testen en productie.

## <a name="sample-deployment-template"></a>Voorbeeldsjabloon voor implementatie

Hier volgt een voorbeeld-implementatiesjabloon die u in Azure-pijplijnen importeren kunt.

```json
{
    "source": 2,
    "id": 1,
    "revision": 51,
    "name": "Data Factory Prod Deployment",
    "description": null,
    "createdBy": {
        "displayName": "Sample User",
        "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "uniqueName": "sampleuser@microsoft.com",
        "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
    },
    "createdOn": "2018-03-01T22:57:25.660Z",
    "modifiedBy": {
        "displayName": "Sample User",
        "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "uniqueName": "sampleuser@microsoft.com",
        "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
    },
    "modifiedOn": "2018-03-14T17:58:11.643Z",
    "isDeleted": false,
    "path": "\\",
    "variables": {},
    "variableGroups": [],
    "environments": [{
        "id": 1,
        "name": "Prod",
        "rank": 1,
        "owner": {
            "displayName": "Sample User",
            "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "uniqueName": "sampleuser@microsoft.com",
            "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
        },
        "variables": {
            "factoryName": {
                "value": "sampleuserprod"
            }
        },
        "variableGroups": [],
        "preDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 1
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 1
            }
        },
        "deployStep": {
            "id": 2
        },
        "postDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 3
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 2
            }
        },
        "deployPhases": [{
            "deploymentInput": {
                "parallelExecution": {
                    "parallelExecutionType": "none"
                },
                "skipArtifactsDownload": false,
                "artifactsDownloadInput": {
                    "downloadInputs": []
                },
                "queueId": 19,
                "demands": [],
                "enableAccessToken": false,
                "timeoutInMinutes": 0,
                "jobCancelTimeoutInMinutes": 1,
                "condition": "succeeded()",
                "overrideInputs": {}
            },
            "rank": 1,
            "phaseType": 1,
            "name": "Run on agent",
            "workflowTasks": [{
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "param\n(\n    [parameter(Mandatory = $false)] [String] $rootFolder=\"C:\\Users\\sampleuser\\Downloads\\arm_template\",\n    [parameter(Mandatory = $false)] [String] $armTemplate=\"$rootFolder\\arm_template.json\",\n    [parameter(Mandatory = $false)] [String] $armTemplateParameters=\"$rootFolder\\arm_template_parameters.json\",\n    [parameter(Mandatory = $false)] [String] $domain=\"microsoft.onmicrosoft.com\",\n    [parameter(Mandatory = $false)] [String] $TenantId=\"72f988bf-86f1-41af-91ab-2d7cd011db47\",\n    [parame",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $true",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": "5.*"
                }
            }, {
                "taskId": "1e244d32-2dd4-4165-96fb-b7441ca9331e",
                "version": "1.*",
                "name": "Azure Key Vault: sampleuservault",
                "refName": "secret1",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "KeyVaultName": "sampleuservault",
                    "SecretsFilter": "*"
                }
            }, {
                "taskId": "94a74903-f93f-4075-884f-dc11f34058b4",
                "version": "2.*",
                "name": "Azure Deployment:Create Or Update Resource Group action on sampleuser-datafactory",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "action": "Create Or Update Resource Group",
                    "resourceGroupName": "sampleuser-datafactory",
                    "location": "East US",
                    "templateLocation": "Linked artifact",
                    "csmFileLink": "",
                    "csmParametersFileLink": "",
                    "csmFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json",
                    "csmParametersFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateParametersForFactory.json",
                    "overrideParameters": "-factoryName \"$(factoryName)\" -linkedService1_connectionString \"$(linkedService1-connectionString)\" -linkedService2_connectionString \"$(linkedService2-connectionString)\"",
                    "deploymentMode": "Incremental",
                    "enableDeploymentPrerequisites": "None",
                    "deploymentGroupEndpoint": "",
                    "project": "",
                    "deploymentGroupName": "",
                    "copyAzureVMTags": "true",
                    "outputVariable": "",
                    "deploymentOutputs": ""
                }
            }, {
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "# You can write your azure powershell scripts inline here. \n# You can also pass predefined and custom variables to this script using arguments",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $false",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }]
        }],
        "environmentOptions": {
            "emailNotificationType": "OnlyOnFailure",
            "emailRecipients": "release.environment.owner;release.creator",
            "skipArtifactsDownload": false,
            "timeoutInMinutes": 0,
            "enableAccessToken": false,
            "publishDeploymentStatus": true,
            "badgeEnabled": false,
            "autoLinkWorkItems": false
        },
        "demands": [],
        "conditions": [{
            "name": "ReleaseStarted",
            "conditionType": 1,
            "value": ""
        }],
        "executionPolicy": {
            "concurrencyCount": 1,
            "queueDepthCount": 0
        },
        "schedules": [],
        "retentionPolicy": {
            "daysToKeep": 30,
            "releasesToKeep": 3,
            "retainBuild": true
        },
        "processParameters": {
            "dataSourceBindings": [{
                "dataSourceName": "AzureRMWebAppNamesByType",
                "parameters": {
                    "WebAppKind": "$(WebAppKind)"
                },
                "endpointId": "$(ConnectedServiceName)",
                "target": "WebAppName"
            }]
        },
        "properties": {},
        "preDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "postDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "badgeUrl": "https://sampleuser.vsrm.visualstudio.com/_apis/public/Release/badge/19749ef3-2f42-49b5-9696-f28b49faebcb/1/1"
    }, {
        "id": 2,
        "name": "Staging",
        "rank": 2,
        "owner": {
            "displayName": "Sample User",
            "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "uniqueName": "sampleuser@microsoft.com",
            "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
        },
        "variables": {
            "factoryName": {
                "value": "sampleuserstaging"
            }
        },
        "variableGroups": [],
        "preDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 4
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 1
            }
        },
        "deployStep": {
            "id": 5
        },
        "postDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 6
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 2
            }
        },
        "deployPhases": [{
            "deploymentInput": {
                "parallelExecution": {
                    "parallelExecutionType": "none"
                },
                "skipArtifactsDownload": false,
                "artifactsDownloadInput": {
                    "downloadInputs": []
                },
                "queueId": 19,
                "demands": [],
                "enableAccessToken": false,
                "timeoutInMinutes": 0,
                "jobCancelTimeoutInMinutes": 1,
                "condition": "succeeded()",
                "overrideInputs": {}
            },
            "rank": 1,
            "phaseType": 1,
            "name": "Run on agent",
            "workflowTasks": [{
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "# You can write your azure powershell scripts inline here. \n# You can also pass predefined and custom variables to this script using arguments",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $true",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }, {
                "taskId": "1e244d32-2dd4-4165-96fb-b7441ca9331e",
                "version": "1.*",
                "name": "Azure Key Vault: sampleuservault",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "KeyVaultName": "sampleuservault",
                    "SecretsFilter": "*"
                }
            }, {
                "taskId": "94a74903-f93f-4075-884f-dc11f34058b4",
                "version": "2.*",
                "name": "Azure Deployment:Create Or Update Resource Group action on sampleuser-datafactory",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "action": "Create Or Update Resource Group",
                    "resourceGroupName": "sampleuser-datafactory",
                    "location": "East US",
                    "templateLocation": "Linked artifact",
                    "csmFileLink": "",
                    "csmParametersFileLink": "",
                    "csmFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json",
                    "csmParametersFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateParametersForFactory.json",
                    "overrideParameters": "-factoryName \"$(factoryName)\" -linkedService1_connectionString \"$(linkedService1-connectionString)\" -linkedService2_connectionString \"$(linkedService2-connectionString)\"",
                    "deploymentMode": "Incremental",
                    "enableDeploymentPrerequisites": "None",
                    "deploymentGroupEndpoint": "",
                    "project": "",
                    "deploymentGroupName": "",
                    "copyAzureVMTags": "true",
                    "outputVariable": "",
                    "deploymentOutputs": ""
                }
            }, {
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "16a37943-8b58-4c2f-a3d6-052d6f032a07",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "param(\n$x,\n$y,\n$z)\nwrite-host \"----------\"\nwrite-host $x\nwrite-host $y\nwrite-host $z | ConvertTo-SecureString\nwrite-host \"----------\"",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $false",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }]
        }],
        "environmentOptions": {
            "emailNotificationType": "OnlyOnFailure",
            "emailRecipients": "release.environment.owner;release.creator",
            "skipArtifactsDownload": false,
            "timeoutInMinutes": 0,
            "enableAccessToken": false,
            "publishDeploymentStatus": true,
            "badgeEnabled": false,
            "autoLinkWorkItems": false
        },
        "demands": [],
        "conditions": [{
            "name": "ReleaseStarted",
            "conditionType": 1,
            "value": ""
        }],
        "executionPolicy": {
            "concurrencyCount": 1,
            "queueDepthCount": 0
        },
        "schedules": [],
        "retentionPolicy": {
            "daysToKeep": 30,
            "releasesToKeep": 3,
            "retainBuild": true
        },
        "processParameters": {
            "dataSourceBindings": [{
                "dataSourceName": "AzureRMWebAppNamesByType",
                "parameters": {
                    "WebAppKind": "$(WebAppKind)"
                },
                "endpointId": "$(ConnectedServiceName)",
                "target": "WebAppName"
            }]
        },
        "properties": {},
        "preDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "postDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "badgeUrl": "https://sampleuser.vsrm.visualstudio.com/_apis/public/Release/badge/19749ef3-2f42-49b5-9696-f28b49faebcb/1/2"
    }],
    "artifacts": [{
        "sourceId": "19749ef3-2f42-49b5-9696-f28b49faebcb:a6c88f30-5e1f-4de8-b24d-279bb209d85f",
        "type": "Git",
        "alias": "Dev",
        "definitionReference": {
            "branches": {
                "id": "adf_publish",
                "name": "adf_publish"
            },
            "checkoutSubmodules": {
                "id": "",
                "name": ""
            },
            "defaultVersionSpecific": {
                "id": "",
                "name": ""
            },
            "defaultVersionType": {
                "id": "latestFromBranchType",
                "name": "Latest from default branch"
            },
            "definition": {
                "id": "a6c88f30-5e1f-4de8-b24d-279bb209d85f",
                "name": "Dev"
            },
            "fetchDepth": {
                "id": "",
                "name": ""
            },
            "gitLfsSupport": {
                "id": "",
                "name": ""
            },
            "project": {
                "id": "19749ef3-2f42-49b5-9696-f28b49faebcb",
                "name": "Prod"
            }
        },
        "isPrimary": true
    }],
    "triggers": [{
        "schedule": {
            "jobId": "b5ef09b6-8dfd-4b91-8b48-0709e3e67b2d",
            "timeZoneId": "UTC",
            "startHours": 3,
            "startMinutes": 0,
            "daysToRelease": 31
        },
        "triggerType": 2
    }],
    "releaseNameFormat": "Release-$(rev:r)",
    "url": "https://sampleuser.vsrm.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_apis/Release/definitions/1",
    "_links": {
        "self": {
            "href": "https://sampleuser.vsrm.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_apis/Release/definitions/1"
        },
        "web": {
            "href": "https://sampleuser.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_release?definitionId=1"
        }
    },
    "tags": [],
    "properties": {
        "DefinitionCreationSource": {
            "$type": "System.String",
            "$value": "ReleaseNew"
        }
    }
}
```

## <a name="sample-script-to-stop-and-restart-triggers-and-clean-up"></a>Voorbeeld van een script om te stoppen en opnieuw opstarten van triggers en opschonen

Hier volgt een voorbeeld van een script om te stoppen triggers vóór de implementatie en triggers daarna opnieuw starten. Het script bevat ook een code voor het verwijderen van resources die zijn verwijderd. Zie voor het installeren van de meest recente versie van Azure PowerShell, [Azure PowerShell installeren op Windows met PowerShellGet](https://docs.microsoft.com/powershell/azure/install-az-ps).

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $rootFolder,
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and ($_.properties.pipelines.Count -gt 0 -or $_.properties.pipeline.pipelineReference -ne $null)} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { 
        Write-host "Disabling trigger " $_
        Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-AzDataFactoryV2LinkedService -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #Delete resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { 
        Write-Host "Deleting trigger "  $_.Name
        $trig = Get-AzDataFactoryV2Trigger -name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
        if ($trig.RuntimeState -eq "Started") {
            Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.Name -Force 
        }
        Remove-AzDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { 
        Write-Host "Deleting pipeline " $_.Name
        Remove-AzDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { 
        Write-Host "Deleting dataset " $_.Name
        Remove-AzDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { 
        Write-Host "Deleting Linked Service " $_.Name
        Remove-AzDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { 
        Write-Host "Deleting integration runtime " $_.Name
        Remove-AzDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }

    if ($deleteDeployment -eq $true) {
        Write-Host "Deleting ARM deployment ... under resource group: " $ResourceGroupName
        $deployments = Get-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName
        $deploymentsToConsider = $deployments | Where { $_.DeploymentName -like "ArmTemplate_master*" -or $_.DeploymentName -like "ArmTemplateForFactory*" } | Sort-Object -Property Timestamp -Descending
        $deploymentName = $deploymentsToConsider[0].DeploymentName

       Write-Host "Deployment to be deleted: " $deploymentName
        $deploymentOperations = Get-AzResourceGroupDeploymentOperation -DeploymentName $deploymentName -ResourceGroupName $ResourceGroupName
        $deploymentsToDelete = $deploymentOperations | Where { $_.properties.targetResource.id -like "*Microsoft.Resources/deployments*" }

        $deploymentsToDelete | ForEach-Object { 
            Write-host "Deleting inner deployment: " $_.properties.targetResource.id
            Remove-AzResourceGroupDeployment -Id $_.properties.targetResource.id
        }
        Write-Host "Deleting deployment: " $deploymentName
        Remove-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName -Name $deploymentName
    }

    #Start Active triggers - After cleanup efforts
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { 
        Write-host "Enabling trigger " $_
        Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Aangepaste parameters met de Resource Manager-sjabloon gebruiken

Als u in de GIT-modus, kunt u de standaardeigenschappen overschrijven in de Resource Manager-sjabloon voor het instellen van eigenschappen die als parameters worden gebruikt in de sjabloon en de eigenschappen die vastgelegd zijn. Het is raadzaam voor de onderdrukking van de standaardsjabloon voor parameterisering in deze scenario's:

* Gebruik van geautomatiseerde CI/CD en u wilt wijzigen van bepaalde eigenschappen tijdens de implementatie van Resource Manager, maar de eigenschappen worden niet standaard parameters.
* Uw gegevensfactory is zo groot is dat de standaard-Resource Manager-sjabloon ongeldig is omdat het meer dan de maximaal toegestane parameters (256).

Onder deze omstandigheden, als u wilt overschrijven de standaardsjabloon voor parameterisering, maak een bestand genaamd *arm-sjabloon-parameters-definition.json* in de hoofdmap van de opslagplaats. De bestandsnaam moet exact overeenkomen. Data Factory probeert te lezen van dit bestand, vanuit welke vertakking u bevindt zich op in de portal voor Azure Data Factory, niet alleen de vertakking samenwerking. U kunt maken of bewerken van het bestand van een privé-vertakking, waarin u uw wijzigingen testen met behulp van kunt de **exporteren ARM-sjabloon** in de gebruikersinterface. Vervolgens kunt u het bestand naar de vertakking samenwerking samenvoegen. Als er geen bestand wordt gevonden, wordt de standaardsjabloon wordt gebruikt.


### <a name="syntax-of-a-custom-parameters-file"></a>Syntaxis van een bestand met aangepaste parameters

Hier vindt u enkele richtlijnen te gebruiken wanneer u het bestand met aangepaste parameters ontwerpt. Het bestand bestaat uit een sectie voor elk entiteitstype: trigger, pijplijn, linkedservice, dataset, integrationruntime, enzovoort.
* Geef het eigenschapspad onder het type relevante entiteit.
* Als u de naam van een eigenschap instelt op '\*'', geeft u aan dat u wilt om te voorzien van alle eigenschappen daaronder (alleen op het eerste niveau, niet recursief). U kunt ook eventuele uitzonderingen op deze opgeven.
* Als u de waarde van een eigenschap als een tekenreeks instelt, kunt u aangeven dat u wilt parameter van de eigenschap. Gebruik de indeling `<action>:<name>:<stype>`.
   *  `<action>` een van de volgende tekens kan zijn:
      * `=` de huidige waarde behouden betekent als de standaardwaarde voor de parameter.
      * `-` de standaardwaarde voor de parameter bijhouden betekent niet.
      * `|` is een speciaal geval voor geheimen in Azure Key Vault voor tekenreeksen voor databaseverbindingen of sleutels.
   * `<name>` is de naam van de parameter. Als deze leeg is, heeft deze de naam van de eigenschap. Als de waarde met met begint een `-` teken, de naam is verkort. Bijvoorbeeld, `AzureStorage1_properties_typeProperties_connectionString` zou worden ingekort tot `AzureStorage1_connectionString`.
   * `<stype>` is het type parameter. Als `<stype>` is leeg, het type is `string`. Ondersteunde waarden: `string`, `bool`, `number`, `object`, en `securestring`.
* Wanneer u een matrix in het definitiebestand opgeeft, kunt u aangeven dat de overeenkomende eigenschap in de sjabloon een matrix is. Data Factory doorloopt de objecten in de matrix met behulp van de definitie die opgegeven in de Integration Runtime-object van de matrix. Het tweede object, een tekenreeks, wordt de naam van de eigenschap, die wordt gebruikt als de naam voor de parameter voor elke iteratie.
* Het is niet mogelijk om een definitie die specifiek is voor een exemplaar van de resource. Een definitie is van toepassing op alle resources van dit type.
* Standaard worden alle beveiligde tekenreeksen, zoals de geheimen van de Key Vault en beveiligde tekenreeksen, zoals verbindingsreeksen, sleutels en tokens, parameters.
 
## <a name="sample-parameterization-template"></a>De parameterisering voorbeeldsjabloon

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "activities": [{
                "typeProperties": {
                    "waitTimeInSeconds": "-::number",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.DataFactory/factories/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:number",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "secretAccessKey": "|"
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```

### <a name="explanation"></a>Uitleg:

#### <a name="pipelines"></a>Pijplijnen
    
* Er is een eigenschap in het pad activiteiten/typeProperties/waitTimeInSeconds parameters. Dit betekent dat elke activiteit in een pijplijn met een code-niveau eigenschap met de naam `waitTimeInSeconds` (bijvoorbeeld de `Wait` activiteit) als parameters als een getal, met een standaardnaam wordt gebruikt. Maar deze bewerking dit geen een standaardwaarde is opgegeven in het Resource Manager-sjabloon. Dit is een verplichte invoer tijdens de implementatie van Resource Manager.
* Op deze manier een eigenschap met de naam `headers` (bijvoorbeeld in een `Web` activiteit) met parameters met het type `object` (JObject). Er is een standaardwaarde, die de dezelfde waarde als in de fabriek bron is.

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Alleen de eigenschappen en alle eigenschappen, onder het pad `typeProperties` bestaan parameters met hun respectieve standaardwaarden. Bijvoorbeeld, vanaf vandaag schema, zijn er twee eigenschappen onder **IntegrationRuntimes** eigenschappen van het type: `computeProperties` en `ssisProperties`. Beide typen eigenschappen zijn gemaakt met hun respectieve standaardwaarden en -typen (Object).

#### <a name="triggers"></a>Triggers

* Onder `typeProperties`, twee eigenschappen als parameters worden gebruikt. Ten eerste `maxConcurrency`, die is opgegeven voor een standaardwaarde hebben en het type zou worden `string`. Dit is de standaardnaam van de parameter van `<entityName>_properties_typeProperties_maxConcurrency`.
* De `recurrence` eigenschap ook met parameters. Alle eigenschappen op dat niveau worden Klik hieronder op als parameters worden gebruikt als tekenreeksen met standaardwaarden en namen van parameters opgegeven. Een uitzondering hierop is de `interval` eigenschap, die als type als parameters gebruikt, en met de parameternaam voorafgegaan door `<entityName>_properties_typeProperties_recurrence_triggerSuffix`. Op dezelfde manier de `freq` eigenschap is een tekenreeks en parameters wordt gebruikt als een tekenreeks. Echter, de `freq` eigenschap parameters zonder een standaardwaarde wordt gebruikt. De naam is ingekort en volgen. Bijvoorbeeld `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Gekoppelde services is uniek. Omdat de gekoppelde services en gegevenssets van verschillende typen mogelijk zijn kunnen, kunt u specifieke aanpassingen kunt opgeven. U kunt bijvoorbeeld zeggen dat voor alle services van het type gekoppelde `AzureDataLakeStore`, een specifieke sjabloon kan worden toegepast, en voor alle andere (via \*) een andere sjabloon worden toegepast.
* In het voorgaande voorbeeld wordt de `connectionString` eigenschap wordt als parameters worden gebruikt als een `securestring` -waarde, met deze bewerking dit geen standaardwaarde en er een korte parameternaam op die wordt voorafgegaan door `connectionString`.
* De eigenschap `secretAccessKey`, echter gebeurt er moet een `AzureKeyVaultSecret` (bijvoorbeeld een `AmazonS3` gekoppelde service). Deze automatisch als een Azure Key Vault-geheim met parameters, en dus wordt het opgehaald uit de sleutelkluis die geconfigureerd met in de fabriek bron. U kunt ook parameter van de key vault zelf.

#### <a name="datasets"></a>Gegevenssets

* Hoewel typespecifieke aanpassing beschikbaar voor gegevenssets is, configuratie kan worden opgegeven zonder expliciet een \*-configuratie. In het voorgaande voorbeeld, alle eigenschappen van gegevensset onder `typeProperties` bestaan parameters.

De parameterisering standaardsjabloon kunt wijzigen, maar dit is de huidige sjabloon. Dit is handig als u slechts één extra eigenschap wordt toevoegen als een parameter, maar ook als u niet wilt verliezen van de bestaande parameteriseringen en moeten ze opnieuw maken.


```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }

        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

## <a name="linked-resource-manager-templates"></a>Gekoppelde Resource Manager-sjablonen

Als u continue integratie en implementatie (CI/CD) voor uw Data Factory's hebt ingesteld, kunt u zien dat, als uw factory groeit, u uitgevoerd in de Resource Manager-sjabloon-limieten, zoals het maximum aantal resources of de maximale nettolading in een Resource Manager-sjabloon. Voor scenario's zoals deze, samen met het genereren van de volledige Resource Manager-sjabloon voor een fabriek, genereert de Data Factory nu ook Gekoppelde Resource Manager-sjablonen. Als gevolg hiervan hebben de nettolading van de gehele factory onderverdeeld in verschillende bestanden, zodat u de genoemde limieten niet uitvoeren.

Als u Git geconfigureerd hebt, de gekoppelde sjablonen worden gegenereerd en opgeslagen samen met de volledige Resource Manager-sjablonen de `adf_publish` vertakking, onder een nieuwe map met de naam `linkedTemplates`.

![Gekoppelde map voor Resource Manager-sjablonen](media/continuous-integration-deployment/linked-resource-manager-templates.png)

De gekoppelde Resource Manager-sjablonen hebben meestal een master sjabloon en een set van onderliggende sjablonen die zijn gekoppeld aan het model. De bovenliggende sjabloon heet `ArmTemplate_master.json`, en de onderliggende sjablonen hebben een naam met het patroon `ArmTemplate_0.json`, `ArmTemplate_1.json`, enzovoort. Als u wilt verplaatsen via de volledige Resource Manager-sjabloon voor het gebruik van de gekoppelde sjablonen gebruiken, werken uw CI/CD-taak om te verwijzen naar `ArmTemplate_master.json` in plaats van dat verwijst naar `ArmTemplateForFactory.json` (dat wil zeggen, de volledige Resource Manager-sjabloon). Resource Manager moet u de gekoppelde sjablonen uploaden naar een opslagaccount, zodat ze tijdens de implementatie kunnen worden geopend door Azure. Zie voor meer informatie, [gekoppelde ARM-sjablonen met VSTS implementeren](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/).

Vergeet niet om toe te voegen van de Data Factory-scripts in uw CI/CD-pijplijn voor en na de Implementatietaak.

Als u geen Git geconfigureerd, de gekoppelde sjablonen zijn toegankelijk via de **exporteren ARM-sjabloon** gebaar.

## <a name="best-practices-for-cicd"></a>Aanbevolen procedures voor CI/CD

Als u met behulp van Git-integratie met uw data factory en u een CI/CD-pijplijn die de wijzigingen van ontwikkeling hebt, testen en vervolgens naar productie verplaatst, wordt aangeraden de volgende aanbevolen procedures:

-   **GIT-integratie**. U alleen moet configureren van uw gegevensfactory ontwikkeling met Git-integratie. Wijzigingen in de Test en productie zijn geïmplementeerd via CI/CD en niet hoeven te hebben van Git-integratie.

-   **Data Factory CI/CD-script**. Voordat u de implementatiestap van Resource Manager in CI/CD moet u zorgen van items zoals het stoppen van de triggers en een ander soort factory opruimen. Wordt u aangeraden [met dit script](#sample-script-to-stop-and-restart-triggers-and-clean-up) als dit zorgt dat al deze dingen. Voer het script in één keer voor de implementatie, en eenmaal na, met behulp van de juiste vlaggen.

-   **Integratieruntimes en het delen van**. Integratieruntimes vormen een van de infrastructurele componenten in uw data factory, die minder vaak wijzigingen ondergaan en zijn vergelijkbaar in alle fasen van uw CI/CD. Als gevolg hiervan Data Factory wordt verwacht dat u hebt dezelfde naam en hetzelfde type Integratieruntimes in alle fasen van CI/CD. Als u wilt delen Integratieruntimes in alle fasen - bijvoorbeeld: de zelfgehoste Integration Runtimes - is één manier om te delen die als host fungeert voor de zelfgehoste IR in een fabriek Ternair alleen voor dat de gedeelde Integratieruntimes bevat. Vervolgens kunt u ze in Dev/Test/Prod als een gekoppelde IR-type.

-   **Key Vault**. Als u de aanbevolen Azure Key Vault op basis van gekoppelde services, kunt u de voordelen één niveau verder in afzonderlijke sleutelkluizen mogelijk te houden voor ontwikkelen/testen/productie nemen. U kunt ook afzonderlijke machtigingen configureren voor elk van deze. U kunt niet uw teamleden machtigingen hebben voor de productie-geheimen. We raden aan dezelfde geheime namen houdt in alle fasen. Als u dezelfde namen wilt houden, bevindt u zich hoeft te wijzigen van uw Resource Manager-sjablonen voor CI/CD, omdat het enige wat die moet worden gewijzigd, is de key vault-naam een van de parameters van de Resource Manager-sjabloon is.

## <a name="unsupported-features"></a>Niet-ondersteunde functies

-   U kunt afzonderlijke resources, niet publiceren omdat de data factory-entiteiten van elkaar afhankelijk. Bijvoorbeeld, triggers afhankelijk zijn van pijplijnen, pijplijnen zijn afhankelijk van gegevenssets en andere pijplijnen, enzovoort. Tracering wijzigen afhankelijkheden is moeilijk. Als deze mogelijk om de resources handmatig publiceren te selecteren is, zou het mogelijk zijn om op te halen, alleen een subset van de volledige set van wijzigingen, die tot onverwacht gedrag dingen nadat deze is gepubliceerd leiden kunnen.

-   U kunt niet publiceren van de persoonlijke vertakkingen.

-   U kunt geen projecten in Bitbucket hosten.
