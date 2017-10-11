---
title: Azure Automation-resources in OMS-oplossingen | Microsoft Docs
description: Oplossingen in OMS omvatten meestal runbooks in Azure Automation voor het automatiseren van processen, zoals het verzamelen en bewakingsgegevens verwerken.  Dit artikel wordt beschreven hoe u runbooks en hun bijbehorende resources opnemen in een oplossing.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1909183a33ed03d8165671cff25cc8b83b77733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="adding-azure-automation-resources-to-an-oms-management-solution-preview"></a>Azure Automation-resources toe te voegen aan een OMS-beheeroplossing (Preview)
> [!NOTE]
> Dit is voorlopige documentatie voor het maken van oplossingen voor het beheer in OMS die zich momenteel in preview. De hieronder beschreven schema kan worden gewijzigd.   


[Oplossingen voor het beheer in OMS](operations-management-suite-solutions.md) omvatten meestal runbooks in Azure Automation voor het automatiseren van processen, zoals het verzamelen en bewakingsgegevens verwerken.  Automation-accounts bevat naast runbooks, assets, zoals variabelen en schema's die ondersteuning bieden voor de runbooks gebruikt in de oplossing.  Dit artikel wordt beschreven hoe u runbooks en hun bijbehorende resources opnemen in een oplossing.

> [!NOTE]
> De voorbeelden in dit artikel gebruiken parameters en variabelen die zijn vereist of gemeenschappelijke voor beheeroplossingen en wordt beschreven in [beheeroplossingen maken in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al bekend met de volgende informatie bent.

- Hoe [maken van een beheeroplossing](operations-management-suite-solutions-creating.md).
- De structuur van een [oplossingsbestand](operations-management-suite-solutions-solution-file.md).
- Hoe [Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md)

## <a name="automation-account"></a>Automation-account
Alle resources in Azure Automation zijn opgenomen in een [Automation-account](../automation/automation-security-overview.md#automation-account-overview).  Zoals beschreven in [OMS werkruimte en de Automation-account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) het Automation-account niet is opgenomen in de oplossing voor beheer, maar moet bestaan voordat de oplossing is geïnstalleerd.  De installatie van de oplossing zal mislukken als het is niet beschikbaar.

De naam van elke resource Automation bevat de naam van de Automation-account.  Dit doet u in de oplossing met de **accountName** parameter zoals in het volgende voorbeeld van een runbook-resource.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
Alle runbooks die door de oplossing in het oplossingsbestand gebruikt zodat ze worden gemaakt wanneer de oplossing is geïnstalleerd, moet u opnemen.  U mag niet de hoofdtekst van het runbook in de sjabloon, dus u moet het runbook publiceren naar een openbaar toegankelijke locatie waar deze kan worden geopend door elke gebruiker installeren van uw oplossing.

[Azure Automation-runbook](../automation/automation-runbook-types.md) resources zijn een type **Microsoft.Automation/automationAccounts/runbooks** en hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


De eigenschappen voor runbooks worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--- |:--- |
| runbookType |Hiermee geeft u de typen van het runbook. <br><br> Script - PowerShell-script <br>PowerShell - PowerShell-werkstroom <br> GraphPowerShell - grafische PowerShell-script runbook <br> GraphPowerShellWorkflow - grafische PowerShell workflow-runbook |
| logProgress |Hiermee geeft u op of [records voortgang](../automation/automation-runbook-output-and-messages.md) voor het runbook moet worden gegenereerd. |
| logVerbose |Hiermee geeft u op of [uitgebreide records](../automation/automation-runbook-output-and-messages.md) voor het runbook moet worden gegenereerd. |
| Beschrijving |Optionele beschrijving voor het runbook. |
| publishContentLink |Hiermee geeft u de inhoud van het runbook. <br><br>URI - Uri met de inhoud van het runbook.  Dit is een ps1-bestand voor runbooks met PowerShell en het Script en een geëxporteerde grafisch runbook-bestand voor een Graph-runbook.  <br> versie - versie van het runbook voor uw eigen bijhouden. |


## <a name="automation-jobs"></a>Automation-taken
Wanneer u een runbook in Azure Automation start, wordt een automation-taak gemaakt.  U kunt een automation-taak resource toevoegen aan uw oplossing automatisch een runbook wordt gestart wanneer het systeem is geïnstalleerd.  Deze methode wordt doorgaans gebruikt voor het starten van runbooks die worden gebruikt voor de initiële configuratie van de oplossing.  Maak eerst een runbook met regelmatige tussenpozen een [planning](#schedules) en een [taakschema](#job-schedules)

Taak resources zijn een type **Microsoft.Automation/automationAccounts/jobs** en hebben de volgende structuur.  Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

De eigenschappen voor automatisering taken worden beschreven in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| runbook |De naam van één entiteit met de naam van het runbook te starten. |
| Parameters |De entiteit voor elke parameterwaarde vereist voor het runbook. |

De taak bevat de runbooknaam en parameterwaarden worden verzonden naar het runbook.  De taak moet [afhankelijk](operations-management-suite-solutions-solution-file.md#resources) het runbook dat deze wordt gestart nadat het runbook moet worden gemaakt voordat de taak.  Als er meerdere runbooks die moet worden gestart, kunt u de volgorde definiëren wanneer er een taak die afhankelijk zijn van andere taken die eerst moeten worden uitgevoerd.

De naam van de resource van een taak moet een GUID die doorgaans wordt toegewezen door een parameter bevatten.  Meer informatie over parameters in GUID [om oplossingen te maken in Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).  


## <a name="certificates"></a>Certificaten
[Azure Automation-certificaten](../automation/automation-certificates.md) een soort **Microsoft.Automation/automationAccounts/certificates** en hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



De eigenschappen van bronnen van de certificaten worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--- |:--- |
| base64Value |Base 64-waarde voor het certificaat. |
| vingerafdruk |Vingerafdruk van het certificaat. |



## <a name="credentials"></a>Referenties
[Azure Automation-referenties](../automation/automation-credentials.md) een soort **Microsoft.Automation/automationAccounts/credentials** en hebben de volgende structuur.  Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

De eigenschappen voor de referentie-resources worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Gebruikersnaam |De naam van de gebruiker voor de referentie. |
| wachtwoord |Wachtwoord voor de referentie. |


## <a name="schedules"></a>Planningen
[Azure Automation-planningen](../automation/automation-schedules.md) een soort **Microsoft.Automation/automationAccounts/schedules** en hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

De eigenschappen voor resources plannen worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Beschrijving |Optionele beschrijving voor de planning. |
| startTime |Hiermee geeft u de begintijd van een planning als een datum/tijd-object. Een tekenreeks kan worden opgegeven als kan worden geconverteerd naar een geldige DateTime. |
| IsEnabled |Hiermee geeft u op of de planning is ingeschakeld. |
| Interval |Het type interval voor de planning.<br><br>dag<br>uur |
| Frequentie |De frequentie waarmee de planning moet worden geactiveerd in aantal dagen of uren. |

Schema's moeten een begintijd met een waarde groter is dan de huidige tijd hebben.  U kunt deze waarde kan niet opgeven met een variabele, omdat u er niet toe van weten wanneer deze het wil worden geïnstalleerd.

Gebruik een van de volgende twee strategieën wanneer resources plannen in een oplossing.

- Gebruik een parameter voor de begintijd van de planning.  Dit wordt de gebruiker gevraagd om een waarde opgeven bij de installatie van de oplossing.  Als u meerdere schema's hebt, kunt u een enkele parameterwaarde kunt gebruiken voor meer dan één.
- Maak de schema's met behulp van een runbook dat wordt gestart wanneer de oplossing is geïnstalleerd.  Hiermee verwijdert u de vereisten van de gebruiker een tijdstip opgeven, maar u mag niet de planning in uw oplossing zodat wordt verwijderd wanneer de oplossing wordt verwijderd.


### <a name="job-schedules"></a>Jobplanningen
Taak schema resources koppelen een runbook met een schema.  Ze hebben een soort **Microsoft.Automation/automationAccounts/jobSchedules** en hebben de volgende structuur.  Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


De eigenschappen voor taakschema's worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--- |:--- |
| de naam van de planning |Één **naam** entiteit met de naam van de planning. |
| Runbooknaam  |Één **naam** entiteit met de naam van het runbook.  |



## <a name="variables"></a>Variabelen
[Azure Automation-variabelen](../automation/automation-variables.md) een soort **Microsoft.Automation/automationAccounts/variables** en hebben de volgende structuur.  Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

De eigenschappen voor de variabele resources worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Beschrijving | Optionele beschrijving voor de variabele. |
| isEncrypted | Hiermee geeft u op of de variabele moet worden versleuteld. |
| type | Deze eigenschap heeft momenteel geen effect.  Het gegevenstype van de variabele wordt bepaald door de initiële waarde. |
| waarde | De waarde voor de variabele. |

> [!NOTE]
> De **type** eigenschap heeft momenteel geen effect op de variabele die wordt gemaakt.  Het gegevenstype voor de variabele wordt bepaald door de waarde.  

Als u de beginwaarde voor de variabele instelt, moet deze worden geconfigureerd als het juiste gegevenstype.  De volgende tabel bevat de verschillende gegevenstypen toegestane en hun syntaxis.  Houd er rekening mee dat de waarden in JSON moeten altijd tussen aanhalingstekens met de speciale tekens binnen de aanhalingstekens worden geplaatst.  Bijvoorbeeld, zou een string-waarde worden opgegeven door aanhalingstekens rond de tekenreeks (met behulp van het escape-teken (\\)) tijdens een numerieke waarde zou worden opgegeven met een set van aanhalingstekens.

| Gegevenstype | Beschrijving | Voorbeeld | Wordt omgezet in |
|:--|:--|:--|:--|
| Tekenreeks   | Waarde moet tussen dubbele aanhalingstekens.  | '\"Hallo wereld\"' | "Hallo wereld" |
| numerieke  | Numerieke waarde met enkele aanhalingstekens.| "64" | 64 |
| Booleaanse waarde  | **de waarde True** of **false** tussen aanhalingstekens.  Houd er rekening mee dat deze waarde een kleine letter moet. | 'true' | De waarde True |
| Datum/tijd | Geserialiseerde date-waarde.<br>U kunt de ConvertTo-Json-cmdlet in PowerShell gebruiken voor het genereren van deze waarde voor een bepaalde datum.<br>Voorbeeld: get-date ' 24/5/2017 13:14:57 "\| ConvertTo-Json | "\\/Date(1495656897378)\\/" | 2017-05-24 13:14:57 |

## <a name="modules"></a>Modules
Uw oplossing voor het beheer niet hoeft te definiëren [globale modules](../automation/automation-integration-modules.md) gebruikt door uw runbooks omdat ze altijd beschikbaar zijn in uw Automation-account.  U hoeft een resource voor elke andere module die wordt gebruikt door uw runbooks bevatten.

[Integratiemodules](../automation/automation-integration-modules.md) een soort **Microsoft.Automation/automationAccounts/modules** en hebben de volgende structuur.  Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


De eigenschappen voor de module resources worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--- |:--- |
| contentLink |Hiermee geeft u de inhoud van de module. <br><br>URI - Uri met de inhoud van de module.  Dit is een ps1-bestand voor runbooks met PowerShell en het Script en een geëxporteerde grafisch runbook-bestand voor een Graph-runbook.  <br> versie - versie van de module voor uw eigen bijhouden. |

Het runbook moet afhankelijk zijn van de bron van de module om ervoor te zorgen dat deze gemaakt voordat het runbook.

### <a name="updating-modules"></a>Bijwerken van modules
Als u een oplossing met een runbook dat gebruikmaakt van een schema bijwerken en de nieuwe versie van uw oplossing een nieuwe module door dat runbook gebruikt is, kan het runbook de oude versie van de module gebruiken.  U moet de volgende runbooks opnemen in uw oplossing en taak maken voor deze uitvoeren vóór alle andere runbooks.  Dit zorgt ervoor dat alle modules die worden bijgewerkt als vereist voordat de runbooks worden geladen.

* [Update ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) zorgt ervoor dat alle modules die worden gebruikt door runbooks in uw oplossing de meest recente versie zijn.  
* [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) alle resources schema om ervoor te zorgen dat de runbooks is gekoppeld aan deze bij gebruik de meest recente modules opnieuw registreren.




## <a name="sample"></a>Voorbeeld
Hier volgt een voorbeeld van een oplossing die opnemen met de volgende bronnen:

- Runbook.  Dit is een voorbeeldrunbook dat is opgeslagen in een openbare GitHub-opslagplaats.
- Automation-taak die het runbook wordt gestart wanneer de oplossing is geïnstalleerd.
- Planning en het schema voor het runbook starten met regelmatige tussenpozen.
- Certificaat.
- De referentie.
- Variabele.
- Module.  Dit is de [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) voor het schrijven van gegevens met logboekanalyse. 

In dit voorbeeld worden [standaardoplossing parameters](operations-management-suite-solutions-solution-file.md#parameters) variabelen die meestal in een oplossing in plaats van hardcoderen waarden in de resourcedefinities gebruikt wordt.


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the schedule link to runbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a>Volgende stappen
* [Een weergave toevoegen aan uw oplossing](operations-management-suite-solutions-resources-views.md) om verzamelde gegevens te visualiseren.
