---
title: Python 2-pakketten in Azure Automation beheren
description: In dit artikel wordt beschreven hoe u Python 2-pakketten in Azure Automation beheren.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: de0998dffeac54db5311bbcde1c9499488b23556
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434969"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Python 2-pakketten in Azure Automation beheren

Azure Automation kunt u Python 2-runbooks uitvoeren op Azure en op Linux Hybrid Runbook Workers. U kunt om te helpen bij de vereenvoudiging van runbooks, Python-pakketten gebruiken voor het importeren van de modules die u nodig hebt. Dit artikel wordt beschreven hoe u beheren en gebruiken van Python-pakketten in Azure Automation.

## <a name="import-packages"></a>Pakketten importeren

Selecteer in uw Automation-Account **Python 2-pakketten** onder **gedeelde bronnen**. Klik op **+ toevoegen van een Python 2-pakket**.

![Python-pakket toevoegen](media/python-packages/add-python-package.png)

Op de **Python 2-pakket toevoegen** pagina, selecteert u een lokale pakket te uploaden. Het pakket mag een `.whl` bestand of `.tar.gz` bestand. Wanneer u hebt geselecteerd, klikt u op **OK** voor het uploaden van het pakket.

![Python-pakket toevoegen](media/python-packages/upload-package.png)

Wanneer een pakket is geïmporteerd, wordt deze weergegeven op de **Python 2-pakketten** pagina in uw Automation-Account. Als u nodig hebt om een pakket te verwijderen, selecteert u het pakket en kies **verwijderen** op de pakketpagina.

![Lijst met](media/python-packages/package-list.png)

## <a name="use-a-package-in-a-runbook"></a>Een pakket gebruiken in een runbook

Wanneer u een pakket hebt geïmporteerd, kunt u deze nu in een runbook gebruiken. Het volgende voorbeeld wordt de [ pakket met het hulpprogramma Azure Automation](https://github.com/azureautomation/azure_automation_utility). Dit pakket wordt het gemakkelijker te Python gebruiken met Azure Automation. Volg de instructies in de GitHub-opslagplaats voor het gebruik van het pakket, en deze toevoegen aan het runbook met behulp van `from azure_automation_utility import get_automation_runas_credential` bijvoorbeeld voor het importeren van de functie voor het ophalen van het RunAs-Account.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Ontwikkelen en testen van runbooks offline

Als u wilt ontwikkelen en testen van uw Python 2-runbooks offline, kunt u de [Azure Automation python geëmuleerde activa](https://github.com/azureautomation/python_emulated_assets) module op GitHub. Deze module kunt u verwijzen naar de gedeelde bronnen zoals referenties, variabelen, verbindingen en certificaten.

## <a name="next-steps"></a>Volgende stappen

Als u wilt aan de slag met Python 2-runbooks, Zie [Mijn eerste Python 2-runbook](automation-first-runbook-textual-python2.md)
