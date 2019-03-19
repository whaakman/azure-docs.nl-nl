---
title: Een Log Analytics-werkruimte met behulp van Azure PowerShell maken | Microsoft Docs
description: Informatie over het maken van een Log Analytics-werkruimte om het beheer van oplossingen en gegevens verzamelen van uw cloud en on-premises omgevingen met Azure PowerShell mogelijk.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: magoedte
ms.openlocfilehash: 055daebb28131268e517845a47d4c39aba90f201
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57871208"
---
# <a name="create-a-log-analytics-workspace-with-azure-powershell"></a>Een Log Analytics-werkruimte maken met Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. Deze snelstartgids leest u hoe de Azure PowerShell-module gebruiken om te implementeren van een Log Analytics-werkruimte in Azure Monitor. Een Log Analytics-werkruimte is een unieke omgeving voor Azure Monitor-logboekgegevens. Elke werkruimte heeft een eigen gegevensopslagplaats en de configuratie en gegevensbronnen en oplossingen zijn geconfigureerd voor het opslaan van hun gegevens in een bepaalde werkruimte. Hebt u nodig een Log Analytics-werkruimte als u van plan bent voor het verzamelen van gegevens uit de volgende bronnen:


* Azure-resources in uw abonnement  
* On-premises computers bewaakt door System Center Operations Manager  
* Apparaatverzamelingen van System Center Configuration Manager  
* Diagnose- of logboekgegevens van Azure Storage  
 
Zie de volgende onderwerpen voor andere bronnen, zoals Azure-VM's en Windows of Linux-VM's in uw omgeving:

* [Verzamelen van gegevens van virtuele machines van Azure](../learn/quick-collect-azurevm.md)
* [Gegevens verzamelen van hybride Linux-computer](../learn/quick-collect-linux-computer.md)
* [Gegevens verzamelen van hybride Windows-computer](quick-collect-windows-computer.md)

Als u geen Azure-abonnement hebt, maakt u [een gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de PowerShell lokaal gebruikt, deze zelfstudie vereist de Azure PowerShell-module AzureRM versie 5.7.0 of Az-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Zie [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps) als u een upgrade wilt uitvoeren. Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-workspace"></a>Een werkruimte maken
Maken van een werkruimte met [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Het volgende voorbeeld wordt een werkruimte met de naam *TestWorkspace* in de resourcegroep *Lab* in de *eastus* locatie met behulp van Resource Manager-sjabloon vanuit uw lokale opslag machine. De JSON-sjabloon is geconfigureerd om te vragen u alleen voor de naam van de werkruimte en een standaardwaarde opgegeven voor de andere parameters die waarschijnlijk moet worden gebruikt als een standaardconfiguratie in uw omgeving. 

Zie voor meer informatie over regio's ondersteund [Log Analytics is beschikbaar in regio's](https://azure.microsoft.com/regions/services/) en zoek naar Azure Monitor uit de **zoeken voor een product** veld. 

De volgende parameters instelt een standaardwaarde:

* locatie - standaard ingesteld op VS-Oost
* SKU - standaard ingesteld op de nieuwe prijzen Per GB-laag met de uitgebracht in het prijsmodel van April 2018

>[!WARNING]
>Als het maken of configureren van een Log Analytics-werkruimte in een abonnement dat is deelneemt aan het nieuwe prijsmodel April 2018, is het de enige geldige Log Analytics prijscategorie **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Sjabloon maken en implementeren

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. De sjabloon bijwerken om aan uw eisen voldoen. Beoordeling [Microsoft.OperationalInsights/workspaces sjabloon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) verwijzing voor meer informatie over welke eigenschappen en waarden worden ondersteund. 
3. Sla dit bestand als **deploylaworkspacetemplate.json** naar een lokale map.   
4. U kunt deze sjabloon nu implementeren. Gebruik de volgende opdrachten uit de map met de sjabloon:

    ```powershell
        New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
    ```

De implementatie kan enkele minuten duren. Als deze is voltooid, ziet u een bericht dat lijkt op de volgende mogelijkheden van het resultaat:

![Voorbeeld van resultaat wanneer de implementatie is voltooid](media/quick-create-workspace-posh/template-output-01.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u een werkruimte beschikbaar hebt, kunt u verzamelen van telemetrie bewaking configureren, uitvoeren van zoekopdrachten in Logboeken om die gegevens te analyseren en toevoegen van een oplossing om aanvullende gegevens en analytische informatie te bieden.  

* Zie voor het inschakelen van verzamelen van gegevens uit Azure-resources met Azure Diagnostics of Azure storage [Azure verzamelen van Logboeken en metrische gegevens voor gebruik in Azure Monitor](../platform/collect-azure-metrics-logs.md).  
* Voeg [System Center Operations Manager als een gegevensbron](../platform/om-agents.md) voor het verzamelen van gegevens van agents die rapporteren van uw Operations Manager-beheergroep en sla deze op in uw Log Analytics-werkruimte.  
* Verbinding maken met [Configuration Manager](../platform/collect-sccm.md) voor het importeren van computers die lid van verzamelingen in de hiërarchie zijn.  
* Controleer de [bewakingsoplossingen](../insights/solutions.md) beschikbaar en hoe u kunt toevoegen of verwijderen van een oplossing uit uw werkruimte.