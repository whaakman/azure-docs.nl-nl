---
title: De extensie Azure Diagnostics voor Windows | Microsoft Docs
description: Azure VM's van Windows met behulp van de extensie voor diagnostische gegevens van Windows Azure bewaken
services: virtual-machines-windows
documentationcenter: ''
author: johnkemnetz
manager: ashwink
editor: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: a9621f6f93d8e14e53cfe05ca4a714e88b9d8572
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="windows-azure-diagnostics-extension"></a>Windows Azure Diagnostics-uitbreiding

## <a name="overview"></a>Overzicht

De Windows Azure Diagnostics VM-extensie kunt u bewakingsgegevens zoals prestatiemeteritems en gebeurtenislogboeken, verzamelen van uw VM van Windows. Granulair kunt u opgeven welke gegevens u wenst te verzamelen en waar u wilt dat de gegevens worden geplaatst, zoals een Azure Storage-account of een Azure Event Hub. U kunt deze gegevens ook gebruiken om te grafieken in de Azure portal bouwen of metrische waarschuwingen maken.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De extensie voor diagnostische gegevens van Windows Azure kan worden uitgevoerd op basis van Windows 10-Client, Windows Server 2008 R2, 2012, 2012 R2 en 2016.

### <a name="internet-connectivity"></a>Internetconnectiviteit

De extensie voor diagnostische gegevens van Windows Azure is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Uitbreidingsschema

[De extensie voor diagnostische gegevens van Windows Azure-schema- en -waarden worden beschreven in dit document.](../../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md)

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de Windows Azure Diagnostics-extensie uitgevoerd tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt. Zie [Gebruik controle en diagnostische gegevens met een virtuele machine van Windows en Azure Resource Manager-sjablonen](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt voor het implementeren van de Windows Azure Diagnostics-extensie op een bestaande virtuele machine. Vervangen door de beveiligde instellingen en eigenschappen voor geldige JSON uit de bovenstaande Uitbreidingsschema. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureRmVMDiagnosticsExtension` opdracht kan worden gebruikt voor de Windows Azure Diagnostics-extensie toevoegen aan een bestaande virtuele machine. Zie ook [Gebruik PowerShell voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows](ps-extensions-diagnostics.md).
```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure CLI. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

[Raadpleeg dit artikel](../../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) voor een uitgebreidere gids voor probleemoplossing voor de Windows Azure Diagnostics-extensie.

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de Windows Azure Diagnostics-uitbreiding](../../monitoring-and-diagnostics/azure-diagnostics.md)
* [Raadpleeg de Uitbreidingsschema en versies](../../monitoring-and-diagnostics/azure-diagnostics-schema.md)
