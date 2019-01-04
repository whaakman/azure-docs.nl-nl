---
title: Bewaking en diagnostische gegevens toevoegen aan een virtuele machine van Azure | Microsoft Docs
description: Gebruik Azure Resource Manager-sjabloon om een nieuwe virtuele machine voor Windows met Azure diagnostics-extensie.
services: virtual-machines-windows
documentationcenter: ''
author: sbtron
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 8cde8fe7-977b-43d2-be74-ad46dc946058
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: saurabh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85e9b49cb8be1a3f53ca0f3b4816e6165b68bde0
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993102"
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>Controle en diagnostische gegevens met een Windows-VM en Azure Resource Manager-sjablonen gebruiken
De Azure Diagnostics-extensie biedt de mogelijkheden voor controle en diagnostische gegevens op een op basis van Windows Azure-machine. U kunt deze mogelijkheden op de virtuele machine inschakelen door te nemen van de extensie als onderdeel van de Azure Resource Manager-sjabloon. Zie [Azure Resource Manager-sjablonen ontwerpen met VM-extensies](../windows/template-description.md#extensions) voor meer informatie over elke extensie opnemen als onderdeel van een virtuele-machinesjabloon. Dit artikel wordt beschreven hoe u de Azure Diagnostics-extensie kunt toevoegen aan een windows VM-sjabloon.  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>De Azure Diagnostics-extensie toevoegen aan de definitie van de VM-resource
Als u wilt de extensie voor diagnostische gegevens op een Windows-Machine inschakelen, moet u de extensie toevoegen als een VM-resource in het Resource Manager-sjabloon.

Voor een eenvoudige Resource Manager gebaseerde virtuele Machine toevoegen aan de configuratie van de extensie de *resources* matrix voor de virtuele Machine: 

```json
"resources": [
    {
        "name": "Microsoft.Insights.VMDiagnosticsSettings",
        "type": "extensions",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-06-15",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "tags": {
            "displayName": "AzureDiagnostics"
        },
        "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
                "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                "storageAccountEndPoint": "https://core.windows.net"
            }
        }
    }
]
```

Een andere algemene conventie is het toevoegen van de configuratie van de extensie op het hoofdknooppunt van de resources van de sjabloon in plaats van deze onder het knooppunt voor de virtuele machine-resources te definiëren. Met deze methode voert u moet expliciet opgeven van een hiërarchische relatie tussen de extensie en de virtuele machine met de *naam* en *type* waarden. Bijvoorbeeld: 

```json
"name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
"type": "Microsoft.Compute/virtualMachines/extensions",
```

De extensie is altijd gekoppeld aan de virtuele machine, kunt u ofwel rechtstreeks direct onder het knooppunt voor de virtuele machine-resource te bepalen of definiëren op het niveau basis en de hiërarchische naamconventie gebruiken om te koppelen aan de virtuele machine.

Voor Virtual Machine Scale Sets de configuratie van de extensies is opgegeven in de *extensionProfile* eigenschap van de *VirtualMachineProfile*.

De *publisher* eigenschap met de waarde van **Microsoft.Azure.Diagnostics** en de *type* eigenschap met de waarde van **IaaSDiagnostics**unieke identificatie van de Azure Diagnostics-extensie.

De waarde van de *naam* eigenschap kan worden gebruikt om te verwijzen naar de extensie in de resourcegroep. Instelling voor het specifiek **Microsoft.Insights.VMDiagnosticsSettings** kunnen eenvoudig worden geïdentificeerd door de Azure-portal ervoor te zorgen dat de bewaking weergeven van goed in de Azure-portal grafieken.

De *typeHandlerVersion* Hiermee geeft u de versie van de uitbreiding die u wilt gebruiken. Instellen van *autoUpgradeMinorVersion* secundaire versie **waar** zorgt ervoor dat u de nieuwste secundaire versie van de extensie die beschikbaar is. Het is raadzaam dat u altijd ingesteld *autoUpgradeMinorVersion* altijd **waar** zodat u altijd kunnen genieten van de meest recente beschikbare diagnostics-extensie gebruiken met de nieuwe functies en bugfixes. 

De *instellingen* element bevat configuratie-eigenschappen voor de extensie die u kunt instellen en lezen van de extensie (soms aangeduid als openbare configuratie). De *xmlcfg* eigenschap bevat op basis van xml-configuratie voor de logboeken met diagnostische gegevens van prestatiemeteritems enzovoort die door de diagnostics-agent worden verzameld. Zie [configuratieschema van Diagnostics](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor meer informatie over het xml-schema zelf. Een normaal worden bij de werkelijke xml-configuratie worden opgeslagen als een variabele in de Azure Resource Manager-sjabloon en vervolgens samen te voegen en base64 coderen ze om in te stellen de waarde voor *xmlcfg*. Zie de sectie over [variabelen voor de configuratie van diagnostische gegevens over](#diagnostics-configuration-variables) voor meer informatie over het opslaan van het XML-bestand in de variabelen. De *storageAccount* eigenschap geeft u de naam van het opslagaccount dat aan welke diagnostische gegevens worden overgebracht. 

De eigenschappen in *protectedSettings* (soms ook bedoeld als privé-configuratie) kan worden ingesteld, maar kan niet worden gelezen nadat wordt ingesteld. De alleen-schrijven aard van *protectedSettings* is het handig is voor het opslaan van geheimen zoals de opslagaccountsleutel waar de diagnostische gegevens worden geschreven.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Opslagaccount voor diagnostische gegevens op te geven als parameters
De diagnostics-extensie json-codefragment hierboven wordt ervan uitgegaan dat twee parameters *existingdiagnosticsStorageAccountName* en *existingdiagnosticsStorageResourceGroup* om op te geven van de opslag van diagnostische gegevens account waar de diagnostische gegevens worden opgeslagen. Opgeven van het opslagaccount voor diagnostische gegevens als een parameter kunt u eenvoudig wijzigen van het opslagaccount voor diagnostische gegevens in verschillende omgevingen, zo kunt u het gebruik van een opslagaccount voor andere diagnostische gegevens voor het testen van en een andere naam voor uw productie-implementatie.  

```json
"existingdiagnosticsStorageAccountName": {
    "type": "string",
    "metadata": {
"description": "The name of an existing storage account to which diagnostics data is transfered."
    }
},
"existingdiagnosticsStorageResourceGroup": {
    "type": "string",
    "metadata": {
"description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
    }
}
```

Het is best practice om op te geven van een opslagaccount voor diagnostische gegevens in een andere resourcegroep dan de resourcegroep voor de virtuele machine. Een resourcegroep kan worden beschouwd als een implementatie-eenheid met hun eigen levensduur, een virtuele machine kan worden geïmplementeerd en opnieuw wordt geïmplementeerd als de nieuwe configuratie-updates zijn worden aangebracht aan het maar mogelijk wilt u doorgaan met het opslaan van diagnostische gegevens in hetzelfde opslagaccount in deze virtuele machine-implementaties. Met de storage-account in een andere resource, kunt het opslagaccount om gegevens uit verschillende implementaties van virtuele machines zodat u eenvoudig kunt oplossen van problemen voor de verschillende versies te accepteren.

> [!NOTE]
> Als u een windows VM-sjabloon vanuit Visual Studio maken, kan het standaardaccount voor opslag worden ingesteld op hetzelfde opslagaccount waar de virtuele machine VHD is geüpload. Dit is voor het vereenvoudigen van de eerste installatie van de virtuele machine. Opnieuw rekening met de sjabloon voor het gebruik van een ander opslagaccount die kan worden doorgegeven als parameter. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Variabelen voor de configuratie van diagnostische gegevens
De voorgaande diagnostics-extensie json-fragment definieert een *accountid* variabele voor het vereenvoudigen van de opslagaccountsleutel voor de diagnostische gegevens van opslag aan:   

```json
"accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"
```

De *xmlcfg* eigenschap voor de diagnostics-extensie is gedefinieerd met behulp van meerdere variabelen die zijn samengevoegd. De waarden van deze variabelen zijn in XML-bestand, zodat ze nodig hebben om correct worden weergegeven bij het instellen van de json-variabelen.

Het volgende voorbeeld wordt de configuratie-xml voor diagnostische gegevens die worden verzameld standaardbesturingssysteem niveau prestatiemeteritems samen met enkele windows-gebeurtenislogboeken en logboeken met diagnostische gegevens over infrastructuur beschreven. Het is voorzien escape-tekens en juist opgemaakt, zodat de configuratie rechtstreeks in de sectie met variabelen van uw sjabloon kan worden geplakt. Zie de [configuratieschema van Diagnostics](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor een nog mensachtigere leesbare voorbeeld van de configuratie-xml.

```json
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
"wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
"wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"
```

De metrische gegevens over de definitie van xml-knooppunt in de bovenstaande configuratie is een belangrijke configuratie-element wordt gedefinieerd hoe de prestatiemeteritems die eerder in de xml in gedefinieerd *PerformanceCounter* knooppunt worden verzameld en opgeslagen. 

> [!IMPORTANT]
> Deze metrische gegevens station het controleren van grafieken en waarschuwingen in Azure portal.  De **metrische gegevens** knooppunt met de *resourceID* en **MetricAggregation** moeten worden opgenomen in de configuratie van diagnostische gegevens voor uw virtuele machine als u wilt zien van de virtuele machine in door gegevens te controleren de Azure-portal. 
> 
> 

Het volgende voorbeeld ziet u de xml voor de metrische definities: 

```xml
<Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
    <MetricAggregation scheduledTransferPeriod="PT1H"/>
    <MetricAggregation scheduledTransferPeriod="PT1M"/>
</Metrics>
```

De *resourceID* kenmerk unieke identificatie van de virtuele machine in uw abonnement. Zorg ervoor dat u de functies subscription() en resourceGroup() zodat de sjabloon werkt automatisch de waarden op basis van het abonnement en de resourcegroep die u implementeert.

Als u meerdere virtuele Machines in een lus maakt, hebt u voor het vullen van de *resourceID* waarde met een functie copyIndex() elke afzonderlijke virtuele machine correct onderscheid te maken. De *xmlCfg* waarde kan worden bijgewerkt ter ondersteuning van dit als volgt:  

```json
"xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 
```

De waarde MetricAggregation van *PT1M* en *PT1H* overheen een aggregatie in een minuut en een aggregatie binnen een uur, respectievelijk.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics tabellen in de opslag
De metrische gegevens van bovenstaande configuratie genereert tabellen in uw opslagaccount voor diagnostische gegevens met de volgende naamgevingsregels:

* **WADMetrics**: Standaardvoorvoegsel voor alle WADMetrics tabellen
* **PT1H** of **PT1M**: Geeft aan dat de tabel samengevoegde gegevens meer dan 1 uur of 1 minuut bevat
* **P10D**: Geeft aan dat de tabel bevat gegevens tien dagen van wanneer de tabel aan de slag verzamelen van gegevens
* **V2S**: Tekenreeksconstante
* **JJJJMMDD**: De datum waarop de tabel gestart verzamelen van gegevens

Voorbeeld: *WADMetricsPT1HP10DV2S20151108* bevat metrische gegevens die zijn samengevoegd tijdens een uur voor 10 dagen vanaf 11-november-2015    

Elke tabel WADMetrics bevat de volgende kolommen:

* **PartitionKey**: De partitiesleutel is gebouwd op basis van de *resourceID* waarde voor het aanduiden van de VM-resource. Bijvoorbeeld: `002Fsubscriptions:<subscriptionID>:002FresourceGroups:002F<ResourceGroupName>:002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>`  
* **RowKey**: De indeling volgt `<Descending time tick>:<Performance Counter Name>`. De berekening van de aflopende maatstreepjes is tikken maximale tijd minus het tijdstip van het begin van de aggregatie is verstreken. Als bijvoorbeeld de voorbeeld-periode is gestart op 10-november-2015 en 00:00Hrs UTC en vervolgens de berekening zou zijn: `DateTime.MaxValue.Ticks - (new DateTime(2015,11,10,0,0,0,DateTimeKind.Utc).Ticks)`. Voor de beschikbare bytes geheugen eruit prestatiemeteritem de rijsleutel: `2519551871999999999__:005CMemory:005CAvailable:0020Bytes`
* **CounterName**: Is de naam van het prestatiemeteritem. Dit komt overeen met de *counterSpecifier* gedefinieerd in de xml-configuratie.
* **Maximale**: De maximale waarde van het prestatiemeteritem gedurende de periode van aggregatie.
* **Minimale**: De minimale waarde van het prestatiemeteritem gedurende de periode van aggregatie.
* **Totaal aantal**: De som van alle waarden van het prestatiemeteritem heeft gerapporteerd gedurende de periode van aggregatie.
* **Aantal**: Het totale aantal waarden dat voor het prestatiemeteritem is gerapporteerd.
* **Gemiddelde**: De gemiddelde (totaal/aantal)-waarde van het prestatiemeteritem gedurende de periode van aggregatie.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een compleet voorbeeld van een Windows virtuele machine met de extensie voor diagnostische gegevens, [201-vm-bewaking--extensie voor diagnostische gegevens](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Implementatie van de Azure Resource Manager-sjabloon via [Azure PowerShell](../windows/ps-template.md) of [Azure vanaf de opdrachtregel](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Meer informatie over [Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md)
