---
title: Controle en diagnostische gegevens toevoegen aan een virtuele machine van Azure | Microsoft Docs
description: Gebruik een Azure Resource Manager-sjabloon maken van een nieuwe virtuele machine voor Windows Azure diagnostics-extensie.
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
ms.openlocfilehash: 792a3401c483327eb7fb9fcd88039bc09025b3ef
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>Controle en diagnostische gegevens met een virtuele machine van Windows en Azure Resource Manager-sjablonen gebruiken
De extensie voor diagnostische gegevens van Azure bieden de mogelijkheden voor controle en diagnostische gegevens op een op basis van Windows Azure virtuele machine. U kunt deze mogelijkheden op de virtuele machine inschakelen door de uitbreiding als onderdeel van de Azure Resource Manager-sjabloon. Zie [Azure Resource Manager-sjablonen ontwerpen met VM-extensies](../windows/template-description.md#extensions) voor meer informatie over het toevoegen van elke extensie als onderdeel van een sjabloon voor virtuele machines. Dit artikel wordt beschreven hoe u de extensie Azure Diagnostics kunt toevoegen aan een sjabloon voor virtuele machines van windows.  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>De extensie Azure Diagnostics aan de definitie van de VM-resource toevoegen
Om in te schakelen op de extensie voor diagnostische gegevens op een virtuele Windows-computer, moet u de extensie als een VM-resource in het Resource Manager-sjabloon toevoegen.

Voor een eenvoudige Resource Manager gebaseerde virtuele Machine toevoegen configuratie voor de uitbreiding aan de *resources* matrix voor de virtuele Machine: 

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

Een andere algemene conventie is het toevoegen van configuratie voor de uitbreiding in het hoofdknooppunt van de resources van de sjabloon in plaats van deze onder het knooppunt voor de virtuele machine-resources te definiëren. Met deze methode die u hebt het expliciet opgeven van een hiërarchische relatie tussen de uitbreiding en de virtuele machine met de *naam* en *type* waarden. Bijvoorbeeld: 

```json
"name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
"type": "Microsoft.Compute/virtualMachines/extensions",
```

De extensie is altijd gekoppeld aan de virtuele machine, kunt u ofwel rechtstreeks direct onder het knooppunt van de virtuele machine resource definiëren of definiëren op het niveau basis en gebruik van de hiërarchische naamconventie om te koppelen aan de virtuele machine.

Voor virtuele-Machineschaalsets de configuratie van de extensies is opgegeven in de *extensionProfile* eigenschap van de *VirtualMachineProfile*.

De *publisher* eigenschap met de waarde van **Microsoft.Azure.Diagnostics** en de *type* eigenschap met de waarde van **IaaSDiagnostics** unieke identificatie van de extensie Azure Diagnostics.

De waarde van de *naam* eigenschap kan worden gebruikt om te verwijzen naar de extensie in de resourcegroep. Specifiek instelling **Microsoft.Insights.VMDiagnosticsSettings** kunnen gemakkelijk worden geïdentificeerd door de Azure-portal ervoor te zorgen dat de bewaking weergeven van goed in de Azure portal grafieken.

De *typeHandlerVersion* geeft u de versie van de uitbreiding die u wilt gebruiken. Instelling *autoUpgradeMinorVersion* secundaire versie naar **true** zorgt ervoor dat u de nieuwste secundaire versie van de uitbreiding die beschikbaar is. Het is raadzaam dat u altijd ingesteld *autoUpgradeMinorVersion* altijd **true** zodat u altijd de meest recente beschikbare diagnostics uitbreiding gebruiken met de nieuwe functies en foutoplossingen ophalen. 

De *instellingen* element bevat eigenschappen van de configuraties voor de uitbreiding die kunnen worden ingesteld en kan worden gelezen door de uitbreiding (ook wel aangeduid als de openbare configuratie). De *xmlcfg* eigenschap bevat op basis van xml-configuratie voor de logboeken met diagnostische gegevens van prestatiemeteritems enzovoort die door de agent diagnostische gegevens worden verzameld. Zie [Diagnostics configuratieschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor meer informatie over het XML-schema zelf. Een gebruikelijk om de werkelijke xml-configuratie opslaan als een variabele in de Azure Resource Manager-sjabloon en vervolgens samenvoegen en base64 coderen beheerders bij het instellen van de waarde voor *xmlcfg*. Zie de sectie over [diagnostische gegevens van de variabelen](#diagnostics-configuration-variables) voor meer informatie over het opslaan van de xml in variabelen. De *storageAccount* eigenschap geeft de naam van het opslagaccount naar welke diagnostische gegevens worden overgedragen. 

De eigenschappen in *protectedSettings* (soms ook bedoeld als privé configuration) kan worden ingesteld, maar kan niet worden gelezen terug nadat wordt ingesteld. De alleen-schrijven aard van *protectedSettings* is daarom geschikt voor het opslaan van geheimen zoals de sleutel van het opslagaccount waarin de diagnostics-gegevens worden geschreven.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Opslagaccount voor diagnostische gegevens als parameters opgeven
De diagnostische gegevens extensie json-codefragment hierboven wordt ervan uitgegaan dat de twee parameters *existingdiagnosticsStorageAccountName* en *existingdiagnosticsStorageResourceGroup* om op te geven van de diagnostics-opslag het account waar diagnostische gegevens worden opgeslagen. Geven de opslagaccount voor diagnostische gegevens als een parameter kunt u gemakkelijk de opslagaccount voor diagnostische gegevens wijzigen in verschillende omgevingen, bijvoorbeeld u mogelijk wilt gebruiken een ander diagnostics storage-account voor het testen en een andere naam voor uw productie-implementatie.  

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

Het is best practice om op te geven van een opslagaccount voor diagnostische gegevens in een andere resourcegroep dan de resourcegroep voor de virtuele machine. Een resourcegroep kan worden beschouwd als een implementatie-eenheid met een eigen levensduur, een virtuele machine kan worden geïmplementeerd en geïmplementeerd als het nieuwe configuraties updates worden worden aangebracht in het maar u kunt doorgaan met het opslaan van de diagnostics-gegevens in hetzelfde opslagaccount in de implementaties van deze virtuele machine. Als het opslagaccount zich op een andere resource, kunt het opslagaccount om gegevens uit verschillende implementaties van virtuele machines zodat u problemen kunt oplossen door de verschillende versies te accepteren.

> [!NOTE]
> Als u een windows-virtuelemachinesjabloon vanuit Visual Studio maakt, kan het standaardopslagaccount zijn ingesteld op gebruik hetzelfde opslagaccount waar de VHD van de virtuele machine wordt geüpload. Dit is de eerste installatie van de virtuele machine te vereenvoudigen. Opnieuw rekening te houden met de sjabloon voor het gebruik van een ander opslagaccount die kan worden doorgegeven als parameter. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Variabelen voor de configuratie van diagnostische gegevens
De voorgaande diagnostics extensie json-codefragment definieert een *accountid* variabele voor het vereenvoudigen van het ophalen van de opslagaccountsleutel voor de opslag van diagnostische gegevens:   

```json
"accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"
```

De *xmlcfg* eigenschap voor de extensie voor diagnostische gegevens wordt gedefinieerd met meerdere variabelen die worden samengevoegd. De waarden van deze variabelen zijn in xml daarom ze moeten correct worden overgeslagen bij het instellen van de json-variabelen.

Het volgende voorbeeld wordt de diagnostische gegevens configuratie-xml die standaard niveau systeemprestatiemeteritems samen met enkele windows-gebeurtenislogboeken en diagnostische gegevens infrastructuur logboeken verzamelt beschreven. Het is escape-teken en de juiste indeling zodat de configuratie rechtstreeks in het gedeelte variabelen van uw sjabloon kan worden geplakt. Zie de [Diagnostics configuratieschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor een meer menselijke leesbare voorbeeld van de configuratie-xml.

```json
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
"wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
"wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"
```

De metrische gegevens definitie XML-knooppunt in de bovenstaande configuratie is een belangrijke configuratie-element wordt gedefinieerd hoe de prestatiemeteritems die eerder in de xml in gedefinieerd *PerformanceCounter* knooppunt zijn geaggregeerd en opgeslagen. 

> [!IMPORTANT]
> Deze metrische gegevens station de bewaking grafieken en waarschuwingen in de Azure portal.  De **metrische gegevens** knooppunt met de *resourceID* en **MetricAggregation** moeten worden opgenomen in de configuratie van de diagnostische gegevens voor uw virtuele machine als u wilt zien van de virtuele machine het bewaken van gegevens in de Azure portal. 
> 
> 

Het volgende voorbeeld ziet u de xml voor definities van de metrische gegevens: 

```xml
<Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
    <MetricAggregation scheduledTransferPeriod="PT1H"/>
    <MetricAggregation scheduledTransferPeriod="PT1M"/>
</Metrics>
```

De *resourceID* kenmerk een unieke identificatie van de virtuele machine in uw abonnement. Zorg ervoor dat de functies subscription() en resourceGroup() gebruiken zodat de sjabloon die waarden op basis van het abonnement en resourcegroep die u implementeert automatisch bijgewerkt.

Als u meerdere virtuele Machines in een lus maakt, hebt u voor het vullen van de *resourceID* waarde met een functie copyIndex() voor elke afzonderlijke virtuele machine correct te onderscheiden. De *xmlCfg* waarde kan worden bijgewerkt ter ondersteuning van dit als volgt:  

```json
"xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 
```

De waarde MetricAggregation van *PT1M* en *PT1H* geven een aggregatie in een minuut en een aggregatie ruim een uur, respectievelijk.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics tabellen in de opslag
De configuratie van de metrische gegevens bovenstaande genereert tabellen in uw opslagaccount voor diagnostische gegevens met de volgende naamconventies:

* **WADMetrics**: standaardvoorvoegsel voor alle WADMetrics tabellen
* **PT1H** of **PT1M**: geeft aan dat de tabel statistische gegevens meer dan 1 uur of 1 minuut bevat
* **P10D**: geeft aan dat de tabel gegevens bevat voor 10 dagen vanaf wanneer het verzamelen van gegevens van de tabel wordt gestart
* **V2S**: tekenreeksconstante
* **JJJJMMDD**: de datum waarop de tabel gestart verzamelen van gegevens

Voorbeeld: *WADMetricsPT1HP10DV2S20151108* bevat metrische gegevens geaggregeerd ruim een uur tien dagen starten op 11-november-2015    

Elke tabel WADMetrics bevat de volgende kolommen:

* **PartitionKey**: de partitiesleutel is samengesteld op basis van de *resourceID* waarde ter identificatie van de VM-resource. Bijvoorbeeld: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
* **RowKey**: de indeling heeft `<Descending time tick>:<Performance Counter Name>`. De berekening van de aflopende maatstreepjes is maximale tijd ticks minus de tijd van het begin van de aggregatie-periode. Als de voorbeeld-periode is gestart op november-10-2015 en 00:00Hrs UTC en vervolgens de berekening is bijvoorbeeld: `DateTime.MaxValue.Ticks - (new DateTime(2015,11,10,0,0,0,DateTimeKind.Utc).Ticks)`. Voor de beschikbare bytes geheugen eruit prestatiemeteritem de rijsleutel: `2519551871999999999__:005CMemory:005CAvailable:0020Bytes`
* **CounterName**: de naam van het prestatiemeteritem. Dit komt overeen met de *counterSpecifier* gedefinieerd in de XML-configuratie.
* **Maximale**: de maximumwaarde van het prestatiemeteritem gedurende de periode voor aggregatie.
* **Minimale**: de minimumwaarde van het prestatiemeteritem gedurende de periode voor aggregatie.
* **Totaal aantal**: de som van alle waarden van het prestatiemeteritem gerapporteerd gedurende de periode voor aggregatie.
* **Aantal**: het totale aantal waarden dat voor het prestatiemeteritem is gerapporteerd.
* **Gemiddelde**: de waarde (totaal/aantal) gemiddelde van het prestatiemeteritem gedurende de periode voor aggregatie.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een compleet codevoorbeeld-sjabloon van een virtuele Windows-computer met de extensie voor diagnostische gegevens [201-vm-bewaking--extensie voor diagnostische gegevens](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Implementeert de Azure Resource Manager-sjabloon met [Azure PowerShell](../windows/ps-template.md) of [Azure vanaf de opdrachtregel](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Meer informatie over [Azure Resource Manager-sjablonen ontwerpen](../../resource-group-authoring-templates.md)
