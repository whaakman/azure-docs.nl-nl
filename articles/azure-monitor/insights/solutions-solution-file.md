---
title: Het maken van een oplossingsbestand management in Azure | Microsoft Docs
description: Beheeroplossingen bevatten verpakte beheerscenario's die klanten aan hun Azure-omgeving toevoegen kunnen.  In dit artikel vindt u informatie over hoe u oplossingen voor moet worden gebruikt in uw eigen omgeving kunt maken of beschikbaar wordt gesteld aan uw klanten.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2018
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 998c3f1fcfbf53f6f0dfb5ddf34d420f2b31a0b2
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001877"
---
# <a name="creating-a-management-solution-file-in-azure-preview"></a>Het maken van een oplossingsbestand management in Azure (Preview)
> [!NOTE]
> Dit is een voorlopige documentatie voor het maken van oplossingen in Azure die zich momenteel in preview. Er is geen schema die hieronder worden beschreven kan worden gewijzigd.  

Management-oplossingen in Azure worden geïmplementeerd als [Resource Manager-sjablonen](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md).  Leren van de belangrijkste taak leren over het ontwerpen van oplossingen voor het [ontwerpen van een sjabloon](../../azure-resource-manager/resource-group-authoring-templates.md).  Dit artikel bevat de unieke informatie van sjablonen voor oplossingen en het configureren van resources van de typische oplossing gebruikt.


## <a name="tools"></a>Hulpprogramma's

Gebruik een teksteditor kunt u werken met oplossingsbestanden, maar het is raadzaam gebruik te maken van de functies die beschikbaar in Visual Studio of Visual Studio Code, zoals beschreven in de volgende artikelen.

- [Het maken en implementeren van Azure-resourcegroepen met Visual Studio](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [Werken met Azure Resource Manager-sjablonen in Visual Studio Code](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)




## <a name="structure"></a>structuur
De basisstructuur van een management-oplossingsbestand is hetzelfde als een [Resource Manager-sjabloon](../../azure-resource-manager/resource-group-authoring-templates.md#template-format), die is als volgt.  Elk van de volgende secties wordt beschreven in de bovenste elementen en de inhoud ervan in een oplossing.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameters
[Parameters](../../azure-resource-manager/resource-group-authoring-templates.md#parameters) zijn waarden die u nodig van de gebruiker hebt bij de installatie van de oplossing voor beheer.  Er zijn standaard parameters die alle oplossingen hebben, en kunt u aanvullende parameters zoals vereist voor uw specifieke oplossing toevoegen.  Hoe gebruikers parameterwaarden wordt opgeven bij de installatie van uw oplossing afhankelijk van de specifieke parameter en hoe de oplossing wordt geïnstalleerd.

Wanneer een gebruiker [installeert uw beheeroplossing voor](solutions.md#install-a-management-solution) via de Azure Marketplace of een Azure-QuickStart-sjablonen selecteren ze wordt gevraagd een [Log Analytics-werkruimte en het Automation-account](solutions.md#log-analytics-workspace-and-automation-account).  Deze worden gebruikt voor het vullen van de waarden van elk van de standaard parameters.  De gebruiker niet gevraagd naar rechtstreeks waarden opgeven voor de standard-parameters, maar ze wordt gevraagd waarden op te geven voor elke extra parameters.


Hieronder ziet u een voorbeeld-parameter.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

De volgende tabel beschrijft de kenmerken van een parameter.

| Kenmerk | Description |
|:--- |:--- |
| type |Het gegevenstype voor de parameter. Het besturingselement voor tekstinvoer weergegeven voor de gebruiker, is afhankelijk van het gegevenstype.<br><br>Boole - vervolgkeuzelijst<br>tekenreeks - tekstvak<br>int - tekstvak<br>SecureString - veld wachtwoord<br> |
| category |De categorie voor de parameter is optioneel.  Parameters in dezelfde categorie zijn gegroepeerd. |
| Besturingselement |Aanvullende functionaliteit voor tekenreeksparameters.<br><br>datum/tijd - datum/tijd-besturingselement wordt weergegeven.<br>GUID - Guid-waarde wordt automatisch gegenereerd en de parameter niet wordt weergegeven. |
| description |Optionele beschrijving voor de parameter.  In een ballon naast de parameter van de gegevens weergegeven. |

### <a name="standard-parameters"></a>Standard parameters
De volgende tabel bevat de standaardparameters voor alle beheeroplossingen.  Deze waarden worden ingevuld voor de gebruiker in plaats van dat wordt gevraagd om ze als uw oplossing wordt geïnstalleerd vanuit de Azure Marketplace of Quickstart-sjablonen.  De gebruiker moet waarden opgeven voor deze als de oplossing wordt geïnstalleerd in een andere methode.

> [!NOTE]
> De gebruikersinterface in de Azure Marketplace en snelstartsjablonen verwacht dat de namen van parameters in de tabel.  Als u de namen van andere parameters gebruikt vervolgens de gebruiker wordt gevraagd voor hen en ze niet automatisch worden ingevuld.
>
>

| Parameter | Type | Description |
|:--- |:--- |:--- |
| accountName |string |Azure Automation-accountnaam. |
| pricingTier |string |De prijscategorie van Log Analytics-werkruimte en Azure Automation-account. |
| regionId |string |De regio van de Azure Automation-account. |
| solutionName |string |Naam van de oplossing.  Als u uw oplossing via Quickstart-sjablonen implementeren wilt, moet klikt u vervolgens u definiëren solutionName als een parameter zodat u kunt een tekenreeks in plaats daarvan vereisen dat de gebruiker om op te geven een definiëren. |
| Werkruimtenaam |string |Naam van log Analytics-werkruimte. |
| workspaceRegionId |string |De regio van de Log Analytics-werkruimte. |


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


U verwijst naar de parameterwaarden in andere elementen van de oplossing met de syntaxis van de **parameters ('parameternaam ')**.  Bijvoorbeeld, voor toegang tot de naam van de werkruimte, zou u **parameters('workspaceName')**

## <a name="variables"></a>Variabelen
[Variabelen](../../azure-resource-manager/resource-group-authoring-templates.md#variables) zijn waarden die u in de rest van de oplossing voor beheer gebruiken wilt.  Deze waarden worden niet blootgesteld aan de gebruiker installeren van de oplossing.  Ze zijn bedoeld om te voorzien van de auteur van één locatie waar ze de waarden die mogelijk meerdere keren wordt gebruikt in de oplossing kunnen beheren. U geen waarden specifieke moet plaatsen voor uw oplossing in de variabelen in plaats van code in de **resources** element.  Dit maakt de code beter leesbare en kunt u eenvoudig wijzigen van deze waarden in latere versies.

Hieronder volgt een voorbeeld van een **variabelen** element met normale parameters die worden gebruikt in oplossingen.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

U kunt verwijzen naar waarden van variabelen met de oplossing met de syntaxis van de **variabelen ('variabele naam')**.  Bijvoorbeeld, voor toegang tot de SolutionName-variabele, zou u **variables('SolutionName')**.

U kunt ook complexe variabelen definiëren die meerdere sets met waarden.  Dit zijn vooral nuttig in oplossingen voor het beheer wanneer u meerdere eigenschappen voor verschillende soorten resources wilt definiëren.  Bijvoorbeeld, kan u de oplossing voor variabelen die wordt weergegeven boven aan de volgende herstructureren.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

In dit geval u verwijzen naar waarden van variabelen met de oplossing met de syntaxis van de **variables('variable name').property**.  Bijvoorbeeld, voor toegang tot de variabele de naam van oplossing, zou u **variables('Solution'). Naam**.

## <a name="resources"></a>Resources
[Resources](../../azure-resource-manager/resource-group-authoring-templates.md#resources) definiëren van de verschillende bronnen die uw beheeroplossing voor installeren en configureren.  Dit is het grootste en meest complexe gedeelte van de sjabloon.  U krijgt de structuur en de volledige beschrijving van de resource-elementen in [Authoring Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md#resources).  Verschillende bronnen die u normaal gesproken definieert, worden beschreven in de andere artikelen in deze documentatie. 


### <a name="dependencies"></a>Afhankelijkheden
De **dependsOn** element Hiermee geeft u een [afhankelijkheid](../../azure-resource-manager/resource-group-define-dependencies.md) op een andere resource.  Wanneer de oplossing is geïnstalleerd, wordt een resource wordt niet gemaakt totdat alle bijbehorende afhankelijkheden zijn gemaakt.  Bijvoorbeeld: uw oplossing kan [een runbook starten](solutions-resources-automation.md#runbooks) wanneer deze geïnstalleerd met behulp van een [taak resource](solutions-resources-automation.md#automation-jobs).  De resource van de taak zou zijn afhankelijk van de runbook-resource om ervoor te zorgen dat het runbook is gemaakt voordat de taak is gemaakt.

### <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics-werkruimte en het Automation-account
Managementoplossingen vereisen een [Log Analytics-werkruimte](../../azure-monitor/platform/manage-access.md) weergaven bevatten en een [Automation-account](../../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat.  Deze moeten beschikbaar zijn voordat de resources in de oplossing worden gemaakt en moeten niet worden gedefinieerd in de oplossing zelf.  De gebruiker wordt [geeft u een werkruimte en account](solutions.md#log-analytics-workspace-and-automation-account) wanneer ze uw oplossing implementeren, maar u moet de volgende punten overwegen als de auteur.


## <a name="solution-resource"></a>Oplossing voor resource
Elke oplossing vereist een resource-vermelding in de **resources** element waarin de oplossing zelf.  Dit is een type **Microsoft.OperationsManagement/solutions** en hebben de volgende structuur. Dit omvat [standaardparameters](#parameters) en [variabelen](#variables) die meestal worden gebruikt om de eigenschappen van de oplossing te definiëren.


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
De bron van de oplossing moet hebben een [afhankelijkheid](../../azure-resource-manager/resource-group-define-dependencies.md) voor elke andere resources in de oplossing omdat ze bestaan moeten voordat de oplossing kan worden gemaakt.  U dit doen door het toevoegen van een vermelding voor elke resource in de **dependsOn** element.

### <a name="properties"></a>Properties
De resource van de oplossing heeft de eigenschappen in de volgende tabel.  Dit omvat de resources waarnaar wordt verwezen en opgenomen in de oplossing waarmee wordt gedefinieerd hoe de resource wordt beheerd nadat de oplossing is geïnstalleerd.  Elke resource in de oplossing moet worden vermeld in ofwel de **referencedResources** of de **containedResources** eigenschap.

| Eigenschap | Description |
|:--- |:--- |
| workspaceResourceId |ID van de Log Analytics-werkruimte in de vorm  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Werkruimtenaam\>*. |
| referencedResources |Lijst met resources in de oplossing die mogen niet worden verwijderd wanneer de oplossing wordt verwijderd. |
| containedResources |Lijst met resources in de oplossing die moeten worden verwijderd wanneer de oplossing wordt verwijderd. |

Het bovenstaande voorbeeld is voor een oplossing met een runbook, een planning en de weergave.  De planning en het runbook zijn *waarnaar wordt verwezen,* in de **eigenschappen** element zodat ze niet worden verwijderd wanneer de oplossing wordt verwijderd.  De weergave is *opgenomen* , zodat deze wordt verwijderd wanneer de oplossing wordt verwijderd.

### <a name="plan"></a>Plannen
De **plan** entiteit van de resource van de oplossing heeft de eigenschappen in de volgende tabel.

| Eigenschap | Description |
|:--- |:--- |
| naam |Naam van de oplossing. |
| versie |De versie van de oplossing zoals wordt bepaald door de auteur. |
| product |De unieke tekenreeks voor het identificeren van de oplossing. |
| Uitgever |Uitgever van de oplossing. |



## <a name="sample"></a>Voorbeeld
Hier vindt u voorbeelden van een voor oplossingsbestanden met een resource van de oplossing op de volgende locaties.

- [Automation-resources](solutions-resources-automation.md#sample)
- [Resources zoeken en waarschuwingen](solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a>Volgende stappen
* [Opgeslagen zoekopdrachten en waarschuwingen toevoegen](solutions-resources-searches-alerts.md) aan uw oplossing.
* [Weergaven toevoegen](solutions-resources-views.md) aan uw oplossing.
* [Runbooks en andere Automation-resources toevoegen](solutions-resources-automation.md) aan uw oplossing.
* Lees meer over van [Authoring Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md).
* Search [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van verschillende Resource Manager-sjablonen.
