---
title: Python 2-pakketten in Azure Automation beheren
description: In dit artikel wordt beschreven hoe u Python 2-pakketten in Azure Automation beheren.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 81ce9cb2667ce9f21d7c18a92e417e47768d7efb
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57407931"
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

## <a name="import-packages-with-dependencies"></a>Importeren van pakketten met afhankelijkheden

Azure automation tijdens het importproces kan afhankelijkheden voor python-pakketten niet worden omgezet. Er zijn twee manieren voor het importeren van een pakket met alle afhankelijkheden ervan. Slechts één van de volgende stappen moet worden gebruikt voor het importeren van de pakketten in uw Automation-Account.

### <a name="manually-download"></a>Handmatig downloaden

Op een Windows 64-bits computer met [python2.7](https://www.python.org/downloads/release/latest/python2) en [pip](https://pip.pypa.io/en/stable/) geïnstalleerd, voer de volgende opdracht om een pakket en alle afhankelijkheden ervan te downloaden:

```
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Nadat de pakketten zijn gedownload, kunt u ze kunt importeren in uw automation-account.

### <a name="runbook"></a>Runbook

Het python-runbook importeren [importeren Python 2-pakketten uit pypi in Azure Automation-account](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) uit de galerie in uw Automation-Account. Zorg ervoor dat de instellingen worden uitgevoerd zijn ingesteld op **Azure** en het runbook start met de parameters. Het runbook moet een Run As-Account voor het Automation-Account om te werken. Voor elke parameter Zorg ervoor dat starten u deze met de switch zoals in de volgende lijst en de volgende afbeelding:

* -s \<abonnements-id\>
* -g \<resourceGroup\>
* -a \<automationAccount\>
* -m \<modulePackage\>

![Lijst met](media/python-packages/import-python-runbook.png)

Het runbook kunt u opgeven welk pakket te downloaden, bijvoorbeeld `Azure` (de vierde parameter) downloadt alle Azure-modules en alle bijbehorende afhankelijkheden, die ongeveer 105 is.

Als het runbook is voltooid. u kunt controleren de **Python 2-pakketten** pagina onder **gedeelde bronnen** in uw Automation-Account om te controleren of dat ze het pakket is correct geïmporteerd.

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
