---
title: Een Log Analytics-werkruimte met behulp van Azure CLI maken | Microsoft Docs
description: Informatie over het maken van een werkruimte voor logboekanalyse voor het beheer van oplossingen en gegevens te verzamelen van uw cloud en on-premises omgevingen met Azure CLI.
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
ms.date: 10/02/2018
ms.author: magoedte
ms.openlocfilehash: bc3b96ee55ccd28cce89b1f37494b836851977f1
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259244"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Een Log Analytics-werkruimte maken met Azure CLI 2.0

De Azure CLI 2.0 wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze quickstart laat zien hoe u Azure CLI 2.0 gebruiken voor het implementeren van een Log Analytics-werkruimte in Azure, dit een unieke omgeving met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen is.  De stappen in dit artikel zijn vereist als u van plan bent voor het verzamelen van gegevens uit de volgende bronnen:

* Azure-resources in uw abonnement  
* On-premises computers bewaakt door System Center Operations Manager  
* Apparaatverzamelingen van System Center Configuration Manager  
* Diagnose- of logboekgegevens van Azure Storage  
 
Zie de volgende onderwerpen voor andere bronnen, zoals Azure-VM's en Windows of Linux-VM's in uw omgeving:

* [Verzamelen van gegevens van virtuele machines van Azure](../../azure-monitor/learn/quick-collect-azurevm.md)
* [Gegevens verzamelen van hybride Linux-computer](../../azure-monitor/learn/quick-collect-linux-computer.md)
* [Gegevens verzamelen van hybride Windows-computer](quick-collect-windows-computer.md)

Als u geen Azure-abonnement hebt, maakt u [een gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.30 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Een werkruimte maken
Maken van een werkruimte met [az group deployment maken](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). Het volgende voorbeeld wordt een werkruimte met de naam *TestWorkspace* in de resourcegroep *Lab* in de *eastus* locatie met behulp van Resource Manager-sjabloon vanuit uw lokale opslag machine. De JSON-sjabloon is geconfigureerd om te vragen u alleen voor de naam van de werkruimte en een standaardwaarde opgegeven voor de andere parameters die waarschijnlijk moet worden gebruikt als een standaardconfiguratie in uw omgeving. Of u kunt de sjabloon opslaan in Azure storage-account voor gedeelde toegang in uw organisatie. Zie voor meer informatie over het werken met sjablonen [resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

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
            "apiVersion": "2017-03-15-preview",
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

2. De sjabloon bijwerken om aan uw eisen voldoen.  Beoordeling [Microsoft.OperationalInsights/workspaces sjabloon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) verwijzing voor meer informatie over welke eigenschappen en waarden worden ondersteund. 
3. Sla dit bestand als **deploylaworkspacetemplate.json** naar een lokale map.   
4. U kunt deze sjabloon nu implementeren. Gebruik de volgende opdrachten uit de map met de sjabloon:

    ```azurecli
    azure group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

De implementatie kan enkele minuten duren. Als deze is voltooid, ziet u een bericht dat lijkt op de volgende mogelijkheden van het resultaat:

![Voorbeeld van resultaat wanneer de implementatie is voltooid](media/quick-create-workspace-cli/template-output-01.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u een werkruimte beschikbaar hebt, kunt u verzamelen van telemetrie bewaking configureren, uitvoeren van zoekopdrachten in Logboeken om die gegevens te analyseren en toevoegen van een oplossing om aanvullende gegevens en analytische informatie te bieden.  

* Zie voor het inschakelen van verzamelen van gegevens uit Azure-resources met Azure Diagnostics of Azure storage [Azure verzamelen van Logboeken en metrische gegevens voor gebruik in Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md).  
* Voeg [System Center Operations Manager als een gegevensbron](../../azure-monitor/platform/om-agents.md) voor het verzamelen van gegevens van agents die rapporteren van uw Operations Manager-beheergroep en sla deze op in uw Log Analytics-werkruimte.  
* Verbinding maken met [Configuration Manager](../../azure-monitor/platform/collect-sccm.md) voor het importeren van computers die lid van verzamelingen in de hiërarchie zijn.  
* Controleer de [beheeroplossingen](../../azure-monitor/insights/solutions.md) beschikbaar en hoe u kunt toevoegen of verwijderen van een oplossing uit uw werkruimte.
