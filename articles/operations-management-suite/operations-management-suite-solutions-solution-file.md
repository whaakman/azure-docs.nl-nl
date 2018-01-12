---
title: Maken van oplossingen voor het beheer in de Operations Management Suite (OMS) | Microsoft Docs
description: De functionaliteit van Operations Management Suite (OMS) uitbreiden beheeroplossingen door verpakte beheerscenario die klanten aan hun OMS-werkruimte toevoegen kunnen.  Dit artikel biedt details over hoe u oplossingen voor het beheer moet worden gebruikt in uw eigen omgeving kunt maken of beschikbaar gesteld aan uw klanten.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2018
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1ace3042cc00cedd005955cdfb82c557fd4a8fb2
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a>Maken van een bestand van de oplossing voor beheer in Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Dit is voorlopige documentatie voor het maken van oplossingen voor het beheer in OMS die zich momenteel in preview. De hieronder beschreven schema kan worden gewijzigd.  

Oplossingen voor het beheer in de Operations Management Suite (OMS) worden geïmplementeerd als [Resource Manager-sjablonen](../azure-resource-manager/resource-manager-template-walkthrough.md).  Leren van de belangrijkste taak bij het leren van het ontwerpen van oplossingen voor het [ontwerpen van een sjabloon](../azure-resource-manager/resource-group-authoring-templates.md).  Dit artikel bevat een unieke gegevens van sjablonen die worden gebruikt voor oplossingen en het configureren van standaardoplossing resources.


## <a name="tools"></a>Hulpprogramma's

Gebruik een teksteditor kunt u werken met oplossingsbestanden, maar we raden gebruik van de functies van Visual Studio of Visual Studio Code zoals beschreven in de volgende artikelen.

- [Maken en implementeren van Azure-resourcegroepen met Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [Werken met Azure Resource Manager-sjablonen in Visual Studio Code](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a>structuur
De basisstructuur van een bestand van management-oplossing is hetzelfde als een [Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md#template-format), die is als volgt.  Elk van de volgende secties wordt beschreven in de bovenste elementen en de inhoud in een oplossing.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameters
[Parameters](../azure-resource-manager/resource-group-authoring-templates.md#parameters) zijn waarden die u nodig van de gebruiker hebt bij de installatie van de oplossing voor beheer.  Er zijn standaard parameters die alle oplossingen hebben en u kunt zo nodig extra parameters toevoegen voor uw specifieke oplossing.  Hoe gebruikers parameterwaarden wordt opgeven bij de installatie van uw oplossing afhankelijk van de specifieke parameter en hoe de oplossing wordt geïnstalleerd.

Wanneer een gebruiker uw beheeroplossing voor via installeert de [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) of [Azure-snelstartsjablonen](operations-management-suite-solutions.md#finding-and-installing-management-solutions) wordt ze gevraagd om te selecteren een [OMS-werkruimteendeAutomation-account](operations-management-suite-solutions.md#oms-workspace-and-automation-account).  Deze worden gebruikt voor het vullen van de waarden van elk van de standaard parameters.  De gebruiker niet gevraagd rechtstreeks waarden opgeven voor de standaard parameters, maar wordt ze gevraagd waarden opgeven voor elke extra parameters.

Wanneer de gebruiker geïnstalleerd in uw oplossing [een andere methode](operations-management-suite-solutions.md#finding-and-installing-management-solutions), moeten deze een waarde opgeven voor alle standaardparameters en alle extra parameters.

Een voorbeeld-parameter worden hieronder weergegeven.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

De volgende tabel beschrijft de kenmerken van een parameter.

| Kenmerk | Beschrijving |
|:--- |:--- |
| type |Het gegevenstype voor de parameter. Het ingevoerde besturingselement weergegeven voor de gebruiker is afhankelijk van het gegevenstype.<br><br>Boole - vervolgkeuzelijst<br>String - tekstvak<br>int - tekstvak<br>SecureString - wachtwoordveld<br> |
| category |De categorie voor de parameter is optioneel.  Parameters in dezelfde categorie zijn gegroepeerd. |
| Besturingselement |Aanvullende functionaliteit voor string-parameters.<br><br>datum/tijd - datum/tijd-besturingselement wordt weergegeven.<br>GUID - Guid-waarde wordt automatisch gegenereerd en de parameter niet wordt weergegeven. |
| description |Optionele beschrijving voor de parameter.  Weergegeven van een ballon met informatie naast de parameter. |

### <a name="standard-parameters"></a>Standaard-parameters
De volgende tabel bevat de standaardparameters voor alle beheeroplossingen voor.  Deze waarden worden ingevuld voor de gebruiker in plaats van het betreffende bevestigingsvenster wanneer uw oplossing wordt geïnstalleerd vanuit de Quick Start-sjablonen of Azure Marketplace.  De gebruiker moet waarden opgeven voor deze als de oplossing is geïnstalleerd met een andere methode.

> [!NOTE]
> De gebruikersinterface in de Azure Marketplace en Quick Start-sjablonen worden de namen van parameters verwacht in de tabel.  Als u verschillende parameternamen gebruikt vervolgens de gebruiker wordt gevraagd om deze en ze niet automatisch ingevuld.
>
>

| Parameter | Type | Beschrijving |
|:--- |:--- |:--- |
| Accountnaam |tekenreeks |Azure Automation-accountnaam. |
| pricingTier |tekenreeks |De prijscategorie van de werkruimte voor logboekanalyse zowel Azure Automation-account. |
| regionId |tekenreeks |De regio van het Azure Automation-account. |
| SolutionName |tekenreeks |Naam van de oplossing.  Als u uw oplossing via snelstartsjablonen implementeert, moet vervolgens u definiëren solutionName als een parameter zodat definieert u een tekenreeks in plaats daarvan vereisen dat de gebruiker een opgeven. |
| workspaceName |tekenreeks |Meld u aan de naam van de werkruimte Analytics. |
| workspaceRegionId |tekenreeks |De regio van de werkruimte voor logboekanalyse. |


Hieronder volgt de structuur van de standaard parameters die u kunt kopiëren en plakken in uw oplossingsbestand.  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


Raadpleeg de parameterwaarden in andere elementen van de oplossing met de syntaxis **parameters (parameter name)**.  Bijvoorbeeld, als u de naam van de werkruimte, gebruikt u **parameters('workspaceName')**

## <a name="variables"></a>Variabelen
[Variabelen](../azure-resource-manager/resource-group-authoring-templates.md#variables) zijn waarden die u in de rest van de oplossing voor beheer gebruiken wilt.  Deze waarden zijn niet blootgesteld aan de gebruiker die de oplossing installeert.  Ze zijn bedoeld als de auteur van het met één locatie waar ze de waarden die u mogelijk meerdere keren worden gebruikt in de oplossing kunnen beheren. U moet alle waarden specifieke naar plaatsen uw oplossing in variabelen in harde code in plaats van de **resources** element.  Dit maakt de code beter leesbaar en kunt u eenvoudig wijzigen van deze waarden in latere versies.

Hieronder volgt een voorbeeld van een **variabelen** element met de gangbare parameters die worden gebruikt in oplossingen.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

U verwijzen naar variabelen binnen de oplossing met de syntaxis **variabelen ('variabele name')**.  Bijvoorbeeld, als u de variabele SolutionName, gebruikt u **variables('SolutionName')**.

U kunt ook complexe variabelen definiëren die meerdere sets met waarden.  Dit zijn vooral handig beheersystemen waar het definiëren van meerdere eigenschappen voor verschillende soorten resources.  U kan bijvoorbeeld de oplossing variabelen hierboven weergegeven met de volgende herstructureren.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

In dit geval u verwijzen naar variabelen binnen de oplossing met de syntaxis **variables('variable name').property**.  Bijvoorbeeld, als u de naam van de oplossing variabele, gebruikt u **variables('Solution'). Naam**.

## <a name="resources"></a>Resources
[Resources](../azure-resource-manager/resource-group-authoring-templates.md#resources) definiëren van de andere resources die uw beheeroplossing voor installeren en configureren.  Dit is het grootste en meest complexe gedeelte van de sjabloon.  U krijgt de structuur en een volledige beschrijving van de resource-elementen in [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md#resources).  Verschillende bronnen die u doorgaans definieert zijn aangegeven in de andere artikelen in deze documentatie. 


### <a name="dependencies"></a>Afhankelijkheden
De **dependsOn** element geeft een [afhankelijkheid](../azure-resource-manager/resource-group-define-dependencies.md) op een andere resource.  Wanneer de oplossing is geïnstalleerd, wordt een bron is niet gemaakt totdat alle afhankelijkheden ervan zijn gemaakt.  Bijvoorbeeld: uw oplossing mogelijk [een runbook start](operations-management-suite-solutions-resources-automation.md#runbooks) wanneer deze geïnstalleerd met behulp van een [taak resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).  De bron van de taak is afhankelijk van de runbook-bron om ervoor te zorgen dat het runbook is gemaakt voordat de taak is gemaakt.

### <a name="oms-workspace-and-automation-account"></a>OMS-werkruimte en de Automation-account
Oplossingen vereisen een [OMS-werkruimte](../log-analytics/log-analytics-manage-access.md) bevat weergaven en een [Automation-account](../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat.  Deze moeten worden voldaan voordat de resources in de oplossing zijn gemaakt en moeten niet worden gedefinieerd in de oplossing zelf.  De gebruiker wordt [een werkruimte en de account opgeven](operations-management-suite-solutions.md#oms-workspace-and-automation-account) wanneer ze uw oplossing implementeren, maar als de auteur moet u rekening houden met de volgende punten.

## <a name="solution-resource"></a>Oplossing resource
Elke oplossing vereist een resource-vermelding in de **resources** element dat de oplossing zelf definieert.  Dit heeft een type **Microsoft.OperationsManagement/solutions** en hebben de volgende structuur. Dit omvat [standaardparameters](#parameters) en [variabelen](#variables) die meestal worden gebruikt om de eigenschappen van de oplossing te definiëren.


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a>Afhankelijkheden
De bron van de oplossing moet hebben, een [afhankelijkheid](../azure-resource-manager/resource-group-define-dependencies.md) op alle andere bronnen in de oplossing omdat ze bestaan moeten voordat de oplossing kan worden gemaakt.  U dit doen door het toevoegen van een vermelding voor elke resource in de **dependsOn** element.

### <a name="properties"></a>Eigenschappen
De oplossing-resource heeft de eigenschappen in de volgende tabel.  Dit omvat de resources waarnaar wordt verwezen en die deel uitmaken van de oplossing waarmee wordt gedefinieerd hoe de resource wordt beheerd nadat de oplossing is geïnstalleerd.  Elke resource in de oplossing moet worden weergegeven in ofwel de **referencedResources** of de **containedResources** eigenschap.

| Eigenschap | Beschrijving |
|:--- |:--- |
| workspaceResourceId |ID van de werkruimte voor logboekanalyse in het formulier  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Werkruimtenaam\>*. |
| referencedResources |Lijst met resources in de oplossing die mogen niet worden verwijderd wanneer de oplossing wordt verwijderd. |
| containedResources |Lijst met resources in de oplossing die moet worden verwijderd wanneer de oplossing wordt verwijderd. |

Het bovenstaande voorbeeld is voor een oplossing met een runbook, een schema en de weergave.  De planning en runbook zijn *waarnaar wordt verwezen* in de **eigenschappen** element zodat ze worden niet verwijderd wanneer de oplossing is verwijderd.  De weergave is *opgenomen* zodat het wordt verwijderd wanneer de oplossing wordt verwijderd.

### <a name="plan"></a>Plannen
De **plan** entiteit van de oplossing-bron heeft de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| naam |Naam van de oplossing. |
| versie |De versie van de oplossing zoals wordt bepaald door de auteur. |
| product |Unieke tekenreeks voor het identificeren van de oplossing. |
| Uitgever |Uitgever van de oplossing. |



## <a name="sample"></a>Voorbeeld
Hier vindt u voorbeelden van oplossingsbestanden met de bron van een oplossing op de volgende locaties.

- [Automation-resources](operations-management-suite-solutions-resources-automation.md#sample)
- [Resources zoeken en waarschuwingen](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a>Volgende stappen
* [Opgeslagen zoekopdrachten en waarschuwingen toevoegen](operations-management-suite-solutions-resources-searches-alerts.md) bij uw systeem.
* [Weergaven toevoegen](operations-management-suite-solutions-resources-views.md) bij uw systeem.
* [Runbooks en andere Automation-resources toevoegen](operations-management-suite-solutions-resources-automation.md) bij uw systeem.
* Lees meer over [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md).
* Search [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van andere Resource Manager-sjablonen.
