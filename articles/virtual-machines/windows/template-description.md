---
title: Virtuele machines in een Azure Resource Manager-sjabloon | Microsoft Azure
description: Meer informatie over hoe de bron van de virtuele machine wordt gedefinieerd in een Azure Resource Manager-sjabloon.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f63ab5cc-45b8-43aa-a4e7-69dc42adbb99
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: cynthn
ms.openlocfilehash: eb88501c5daf0b79d22f4407a372c4606a173db1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987693"
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a>Virtuele machines in een Azure Resource Manager-sjabloon

Dit artikel wordt beschreven aspecten van een Azure Resource Manager-sjabloon die betrekking hebben op virtuele machines. In dit artikel beschrijft niet een volledige sjabloon voor het maken van een virtuele machine. Hiervoor moet u resourcedefinities voor opslagaccounts, netwerkinterfaces, openbare IP-adressen en virtuele netwerken. Zie voor meer informatie over hoe deze resources samen kunnen worden gedefinieerd, de [walkthrough voor Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Er zijn veel [sjablonen in de galerie](https://azure.microsoft.com/documentation/templates/?term=VM) die de VM-resource bevatten. Niet alle elementen die kunnen worden opgenomen in een sjabloon worden hier beschreven.

In dit voorbeeld toont een typische resource-gedeelte van een sjabloon voor het maken van een opgegeven aantal virtuele machines:

```json
"resources": [
  { 
    "apiVersion": "2016-04-30-preview", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[concat('myVM', copyindex())]", 
    "location": "[resourceGroup().location]",
    "copy": {
      "name": "virtualMachineLoop", 
      "count": "[parameters('numberOfInstances')]"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/myNIC', copyindex())]" 
    ], 
    "properties": { 
      "hardwareProfile": { 
        "vmSize": "Standard_DS1" 
      }, 
      "osProfile": { 
        "computername": "[concat('myVM', copyindex())]", 
        "adminUsername": "[parameters('adminUsername')]", 
        "adminPassword": "[parameters('adminPassword')]" 
      }, 
      "storageProfile": { 
        "imageReference": { 
          "publisher": "MicrosoftWindowsServer", 
          "offer": "WindowsServer", 
          "sku": "2012-R2-Datacenter", 
          "version": "latest" 
        }, 
        "osDisk": { 
          "name": "[concat('myOSDisk', copyindex())]",
          "caching": "ReadWrite", 
          "createOption": "FromImage" 
        },
        "dataDisks": [
          {
            "name": "[concat('myDataDisk', copyindex())]",
            "diskSizeGB": "100",
            "lun": 0,
            "createOption": "Empty"
          }
        ] 
      }, 
      "networkProfile": { 
        "networkInterfaces": [ 
          { 
            "id": "[resourceId('Microsoft.Network/networkInterfaces',
              concat('myNIC', copyindex()))]" 
          } 
        ] 
      },
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('https://', variables('storageName'), '.blob.core.windows.net')]"
        }
      } 
    },
    "resources": [ 
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
      {
        "name": "MyCustomScriptExtension",
        "type": "extensions",
        "apiVersion": "2016-03-30",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
        ],
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.7",
          "autoUpgradeMinorVersion": true,
          "settings": {
            "fileUris": [
              "[concat('https://', variables('storageName'),
                '.blob.core.windows.net/customscripts/start.ps1')]" 
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
          }
        }
      } 
    ]
  } 
]
``` 

> [!NOTE] 
>In dit voorbeeld is afhankelijk van een storage-account dat eerder is gemaakt. U kunt het storage-account maken door deze te implementeren uit de sjabloon. Het voorbeeld is ook afhankelijk van een netwerkinterface en de bijbehorende afhankelijke bronnen die in de sjabloon wordt gedefinieerd. Deze resources worden niet weergegeven in het voorbeeld.
>
>

## <a name="api-version"></a>API-versie

Wanneer u resources met behulp van een sjabloon implementeert, hebt u om op te geven van een versie van de API te gebruiken. Het voorbeeld ziet de bron van de virtuele machine met behulp van dit element apiVersion:

```
"apiVersion": "2016-04-30-preview",
```

De versie van de API die u in de sjabloon opgeeft is van invloed op de eigenschappen die u in de sjabloon definiëren kunt. In het algemeen moet u de meest recente versie van de API selecteren bij het maken van sjablonen. Voor bestaande sjablonen kunt u beslissen of u wilt doorgaan met behulp van een eerdere versie van de API of de sjabloon voor de meest recente versie om te profiteren van nieuwe functies bijwerkt.

Gebruik deze mogelijkheden voor het ophalen van de meest recente API-versies:

- REST-API - [alle resourceproviders vermelden](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)
- PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)
- Azure CLI - [az provider show](https://docs.microsoft.com/cli/azure/provider#az_provider_show)

## <a name="parameters-and-variables"></a>Parameters en variabelen

[Parameters](../../resource-group-authoring-templates.md) gemakkelijk voor u waarden op te geven voor de sjabloon wanneer u deze uitvoert. Deze parametersectie wordt gebruikt in het voorbeeld:

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

Wanneer u de voorbeeldsjabloon implementeert, voert u de waarden voor de naam en het wachtwoord van het administrator-account op elke virtuele machine en het aantal virtuele machines te maken. U hebt de mogelijkheid van het opgeven van parameterwaarden in een afzonderlijk bestand dat wordt beheerd met behulp van de sjabloon of geef dezelfde waarden als u wordt gevraagd.

[Variabelen](../../resource-group-authoring-templates.md) gemakkelijk voor u het instellen van waarden in de sjabloon die in het herhaaldelijk worden gebruikt of die na verloop van tijd kunt wijzigen. Deze sectie met variabelen wordt in het voorbeeld gebruikt:

```
"variables": { 
  "storageName": "mystore1",
  "accountid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name,
  '/providers/','Microsoft.Storage/storageAccounts/', variables('storageName'))]", 
  "wadlogs": "<WadCfg> 
    <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> 
      <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> 
      <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > 
        <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" />
      </WindowsEventLog>", 
  "wadperfcounters": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\">
      <PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\">
        <annotation displayName=\"Threads\" locale=\"en-us\"/>
      </PerformanceCounterConfiguration>
    </PerformanceCounters>", 
  "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters'), 
    '<Metrics resourceId=\"')]", 
  "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name , 
    '/providers/', 'Microsoft.Compute/virtualMachines/')]", 
  "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/>
    <MetricAggregation scheduledTransferPeriod=\"PT1M\"/>
    </Metrics></DiagnosticMonitorConfiguration>
    </WadCfg>"
}, 
```

Wanneer u de voorbeeldsjabloon implementeert, worden de waarden van variabelen worden gebruikt voor de naam en id van het eerder gemaakte opslagaccount. Variabelen worden ook gebruikt om de instellingen voor de extensie voor diagnostische te geven. Gebruik de [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](../../resource-manager-template-best-practices.md) om te bepalen hoe wilt u de parameters en variabelen in de sjabloon voor het structureren.

## <a name="resource-loops"></a>Resource-lussen

Als u meer dan één virtuele machine nodig hebt voor uw toepassing, kunt u een kopie-element in een sjabloon. Deze optionele element wordt uitgevoerd bij het maken van het aantal virtuele machines die u hebt opgegeven als parameter:

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

U ziet ook in het voorbeeld dat de lusindex wordt gebruikt bij het opgeven van sommige van de waarden voor de resource. Als u een aantal exemplaren van drie hebt ingevoerd, worden de namen van de schijven van het besturingssysteem bijvoorbeeld myOSDisk1 myOSDisk2 en myOSDisk3:

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
>In dit voorbeeld maakt gebruik van beheerde schijven voor de virtuele machines.
>
>

Houd er rekening mee dat het maken van een lus voor één resource in de sjabloon u vereisen u dat gebruik van de lus bij het maken of toegang tot andere bronnen. Bijvoorbeeld, meerdere virtuele machines die dezelfde netwerkinterface niet gebruiken, zodat als de sjabloon wordt uitgevoerd bij het maken van drie virtuele machines ook herhalen moet bij het maken van drie netwerkinterfaces. Bij het toewijzen van een netwerkinterface aan een virtuele machine, wordt de lusindex wordt gebruikt om te kunnen identificeren:

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a>Afhankelijkheden

De meeste resources, is afhankelijk van andere bronnen correct te laten werken. Virtuele machines moet worden gekoppeld met een virtueel netwerk en dat het moet een netwerkinterface. De [dependsOn](../../resource-group-define-dependencies.md) element wordt gebruikt om ervoor te zorgen dat de netwerkinterface gereed om te worden gebruikt is voordat de virtuele machines worden gemaakt:

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

Resource Manager implementeert parallel alle resources die zijn niet afhankelijk van een andere resource wordt geïmplementeerd. Wees voorzichtig bij het instellen van afhankelijkheden, omdat u uw implementatie per ongeluk verminderen kunt door onnodige afhankelijkheden op te geven. Afhankelijkheden kunnen koppelen via meerdere bronnen. Bijvoorbeeld, de netwerkinterface is afhankelijk van het openbare IP-adres en de virtuele-netwerkbronnen.

Hoe weet u als een afhankelijkheid vereist is? Bekijk de waarden die u in de sjabloon is ingesteld. Als een element in de virtuele machine-resource definition verwijst naar een andere resource die is geïmplementeerd in dezelfde sjabloon, moet u een afhankelijkheid. Uw voorbeeld van de virtuele machine wordt bijvoorbeeld een netwerkprofiel gedefinieerd:

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

Deze eigenschap wilt instellen, moet de netwerkinterface bestaat. Daarom moet u een afhankelijkheid. U moet ook een afhankelijkheid instellen wanneer één resource (een onderliggende) wordt gedefinieerd in een andere resource (een bovenliggend). Bijvoorbeeld, de diagnostische instellingen en aangepaste scriptextensies zijn gedefinieerd als de onderliggende resources van de virtuele machine. Ze kunnen niet worden gemaakt totdat de virtuele machine bestaat. Beide resources zijn daarom gemarkeerd als afhankelijk van de virtuele machine.

## <a name="profiles"></a>Profielen

Verschillende elementen van het profiel worden gebruikt bij het definiëren van de bron van een virtuele machine. Sommige zijn vereist en sommige zijn optioneel. Bijvoorbeeld, de hardwareProfile, osProfile storageProfile en networkProfile elementen zijn vereist, maar de diagnosticsProfile is optioneel. Deze profielen definiëren de instellingen zoals:
   
- [Grootte](sizes.md)
- [naam](/azure/architecture/best-practices/naming-conventions) en referenties
- schijf en [instellingen van besturingssysteem](cli-ps-findimage.md)
- [Netwerkinterface](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md) 
- Diagnostische gegevens over opstarten

## <a name="disks-and-images"></a>Schijven en installatiekopieën
   
In Azure, vhd-bestanden kunnen vertegenwoordigen [schijven of installatiekopieën](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Wanneer het besturingssysteem in een vhd-bestand is speciaal bedoeld om te worden van een specifieke virtuele machine, wordt dit aangeduid als een schijf. Wanneer het besturingssysteem in een vhd-bestand is gegeneraliseerd moet worden gebruikt om u te veel virtuele machines maken, is deze aangeduid als een installatiekopie.   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a>Nieuwe virtuele machines en nieuwe schijven maken van een platforminstallatiekopie

Wanneer u een virtuele machine maakt, moet u bepalen welk besturingssysteem te gebruiken. Het element imageReference wordt gebruikt voor het definiëren van het besturingssysteem van een nieuwe virtuele machine. Het voorbeeld ziet een definitie voor een Windows Server-besturingssysteem:

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

Als u maken van een Linux-besturingssysteem wilt, kunt u deze definitie gebruiken:

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

Configuratie-instellingen voor de besturingssysteemschijf zijn met het element osDisk toegewezen. In het voorbeeld wordt een nieuwe beheerde schijf met de cache-modus ingesteld op **ReadWrite** en dat de schijf wordt gemaakt van een [platforminstallatiekopie](cli-ps-findimage.md):

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a>Nieuwe virtuele machines maken van bestaande beheerde schijven

Als u maken van virtuele machines van de bestaande schijven wilt, verwijder de imageReference en de osProfile-elementen en definieert u de Schijfinstellingen van deze:

```
"osDisk": { 
  "osType": "Windows",
  "managedDisk": { 
    "id": "[resourceId('Microsoft.Compute/disks', [concat('myOSDisk', copyindex())])]" 
  }, 
  "caching": "ReadWrite",
  "createOption": "Attach" 
},
```

### <a name="create-new-virtual-machines-from-a-managed-image"></a>Nieuwe virtuele machines van een beheerde installatiekopie maken

Als u een virtuele machine van een beheerde installatiekopie maken wilt, wijzigen van het element imageReference en definieer de Schijfinstellingen van deze:

```
"storageProfile": { 
  "imageReference": {
    "id": "[resourceId('Microsoft.Compute/images', 'myImage')]"
  },
  "osDisk": { 
    "name": "[concat('myOSDisk', copyindex())]",
    "osType": "Windows",
    "caching": "ReadWrite", 
    "createOption": "FromImage" 
  }
},
```

### <a name="attach-data-disks"></a>Gegevensschijven koppelen

U kunt eventueel gegevensschijven toevoegen aan de virtuele machines. De [aantal schijven](sizes.md) is afhankelijk van de grootte van de schijf van het besturingssysteem die u gebruikt. Met de grootte van de virtuele machines ingesteld op Standard_DS1_v2, is het maximum aantal gegevensschijven dat kan worden toegevoegd aan deze twee. In het voorbeeld is een beheerde gegevensschijf wordt toegevoegd aan elke virtuele machine:

```
"dataDisks": [
  {
    "name": "[concat('myDataDisk', copyindex())]",
    "diskSizeGB": "100",
    "lun": 0, 
    "caching": "ReadWrite",
    "createOption": "Empty"
  }
],
```

## <a name="extensions"></a>Extensies

Hoewel [extensies](extensions-features.md) zijn van een afzonderlijke resource, worden ze nauw zijn gekoppeld aan virtuele machines. Extensies kunnen worden toegevoegd als een onderliggende resource van de virtuele machine of als een afzonderlijke resource. Het voorbeeld wordt de [Diagnostics-extensie](extensions-diagnostics-template.md) wordt toegevoegd aan de virtuele machines:

```
{ 
  "name": "Microsoft.Insights.VMDiagnosticsSettings", 
  "type": "extensions", 
  "location": "[resourceGroup().location]", 
  "apiVersion": "2016-03-30", 
  "dependsOn": [ 
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
  ], 
  "properties": { 
    "publisher": "Microsoft.Azure.Diagnostics", 
    "type": "IaaSDiagnostics", 
    "typeHandlerVersion": "1.5", 
    "autoUpgradeMinorVersion": true, 
    "settings": { 
      "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
      variables('wadmetricsresourceid'), 
      concat('myVM', copyindex()),
      variables('wadcfgxend')))]", 
      "storageAccount": "[variables('storageName')]" 
    }, 
    "protectedSettings": { 
      "storageAccountName": "[variables('storageName')]", 
      "storageAccountKey": "[listkeys(variables('accountid'), 
        '2015-06-15').key1]", 
      "storageAccountEndPoint": "https://core.windows.net" 
    } 
  } 
},
```

Deze extensie-resource maakt gebruik van de variabelen voor de diagnostische en de variabele storageName waarden op te geven. Als u wilt de gegevens die worden verzameld door deze extensie te wijzigen, kunt u meer prestatiemeteritems toevoegen aan de variabele wadperfcounters. U kunt er ook voor kiezen om de diagnostische gegevens in een ander opslagaccount dan waar de VM-schijven zijn opgeslagen.

Er zijn veel extensies die u op een virtuele machine installeren kunt, maar het meest geschikt is waarschijnlijk de [Custom Script Extension](extensions-customscript.md). In het voorbeeld wordt een PowerShell-script met de naam start.ps1 wordt uitgevoerd op elke virtuele machine wanneer deze eerst wordt gestart:

```
{
  "name": "MyCustomScriptExtension",
  "type": "extensions",
  "apiVersion": "2016-03-30",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
  ],
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "[concat('https://', variables('storageName'),
          '.blob.core.windows.net/customscripts/start.ps1')]" 
      ],
      "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
    }
  }
}
```

Het script start.ps1 kunt veel configuratietaken uitvoeren. Bijvoorbeeld, zijn de gegevensschijven die zijn toegevoegd aan de virtuele machines in het voorbeeld niet geïnitialiseerd; u kunt een aangepast script gebruiken ze worden geïnitialiseerd. Als u meerdere opstarttaken hebt te doen, kunt u het bestand start.ps1 om aan te roepen andere PowerShell-scripts in Azure storage. Het voorbeeld wordt PowerShell gebruikt, maar u kunt elke scripting methode gebruiken die beschikbaar is op het besturingssysteem die u gebruikt.

U ziet de status van de geïnstalleerde uitbreidingen van de instellingen van de extensies in de portal:

![Status van de extensie ophalen](./media/template-description/virtual-machines-show-extensions.png)

U kunt ook informatie over extensies ophalen met behulp van de **Get-AzureRmVMExtension** PowerShell-opdracht, de **vm-extensie get** Azure CLI-opdracht, of de **extensie informatie**REST-API.

## <a name="deployments"></a>Implementaties

Wanneer u een sjabloon implementeert, worden de resources dat u als een groep hebt geïmplementeerd en wordt automatisch een naam aan deze geïmplementeerde groep toegewezen bijgehouden in Azure. De naam van de implementatie is hetzelfde als de naam van de sjabloon.

Als u meer wilt weten over de status van resources in de implementatie, kunt u de resourcegroep-blade in Azure portal:

![Ophalen van informatie over de implementatie](./media/template-description/virtual-machines-deployment-info.png)
    
Het is niet een probleem aan dezelfde sjabloon gebruiken om resources te maken of bijwerken van bestaande resources. Wanneer u opdrachten gebruiken om sjablonen te implementeren, hebt u de mogelijkheid om in te spreken die [modus](../../resource-group-template-deploy.md) u wilt gebruiken. De modus kan worden ingesteld op **voltooid** of **incrementele**. De standaardwaarde is om te doen incrementele updates. Wees voorzichtig bij het gebruik van de **voltooid** modus omdat u per ongeluk de resources verwijdert mogelijk. Als u de modus instelt op **voltooid**, Resource Manager Hiermee verwijdert u alle resources in de resourcegroep die zich niet in de sjabloon.

## <a name="next-steps"></a>Volgende stappen

- Maak uw eigen sjabloon met [Authoring Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md).
- Implementeer de sjabloon die u hebt gemaakt met behulp van [een Windows-machine maken met Resource Manager-sjabloon](ps-template.md).
- Informatie over het beheren van de virtuele machines die u hebt gemaakt, vindt [maken en beheren van Windows-VM's met de Azure PowerShell-module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
