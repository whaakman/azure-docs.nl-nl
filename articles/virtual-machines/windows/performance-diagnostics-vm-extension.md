---
title: Prestaties van Azure Diagnostics VM-extensie voor Windows | Microsoft Docs
description: Introduceert VM-extensie voor prestaties van Azure Diagnostics voor Windows.
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/29/2017
ms.author: genli
ms.openlocfilehash: d9384af2cf1d8b3f55f9ec2316046536634c124e
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Prestaties van Azure Diagnostics VM-extensie voor Windows

## <a name="summary"></a>Samenvatting
Helpt diagnostische prestatiegegevens verzamelen van VM's van Windows, voert analyse en biedt een rapport van de resultaten en aanbevelingen voor het identificeren en oplossen van prestatieproblemen met voor de virtuele machine van Azure Diagnostics VM-extensie van prestaties. Deze uitbreiding is geïnstalleerd voor probleemoplossing hulpprogramma [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Vereisten
### <a name="operating-systems"></a>Besturingssystemen
Deze uitbreiding kan worden geïnstalleerd op Windows Server 2008 R2, 2012 R2, 2012 2016; Windows 8.1 en Windows 10-besturingssystemen.

## <a name="extension-schema"></a>Uitbreidingsschema
De volgende JSON vindt u het schema voor de extensie voor diagnostische gegevens van Azure prestaties. Deze uitbreiding is vereist voor de naam en de sleutel voor een Opslagaccount naar de opslag van de uitvoer van de diagnostische gegevens en het rapport. Deze waarden gevoelig zijn en moeten worden opgeslagen in de configuratie van beveiligde instelling. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld. Houd er rekening mee dat storageAccountName en storageAccountKey hoofdlettergevoelig zijn. Andere vereiste parameters zijn vermeld in de onderstaande sectie.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
```

### <a name="property-values"></a>Eigenschapswaarden

|   **Naam**   |**Waarde / voorbeeld**|       **Beschrijving**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|Versie van de API
|Uitgever|Microsoft.Azure.Performance.Diagnostics|Publisher naamruimte voor de extensie
|type|AzurePerformanceDiagnostics|Type van de VM-extensie
|typeHandlerVersion|1.0|Versie van de extensie-Handler
|performanceScenario|standaard|Prestaties scenario voor het vastleggen van de gegevens voor. Geldige waarden zijn: **basic**, **vmslow**, **azurefiles**, en **aangepaste**.
|traceDurationInSeconds|300|De duur van de traceringen als een van de traceeropties zijn geselecteerd.
|perfCounterTrace|P|De optie voor het inschakelen van de teller prestatietracering. Geldige waarden zijn **p** of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde leeg.
|networkTrace|n|De optie voor het inschakelen van Netmon Trace. Geldige waarden zijn  **n**  of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde leeg.
|xperfTrace|x|De optie voor het inschakelen van XPerf Trace. Geldige waarden zijn **x** of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde leeg.
|storPortTrace|s|De optie StorPort-tracering inschakelen. Geldige waarden zijn s of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde leeg.
|srNumber|123452016365929|Ondersteuning voor Ticketnummer indien beschikbaar. Laat leeg als u niet hebt.
|storageAccountName|mystorageaccount|De naam van het opslagaccount voor het opslaan van de logboeken met diagnostische gegevens en de resultaten.
|storageAccountKey|lDuVvxuZB28NNP... hAiRF3voADxLBTcc ==|Sleutel voor het opslagaccount.

## <a name="install-the-extension"></a>De extensie installeren

Volg deze stappen voor het installeren van de VM-extensie op Windows virtuele Machines:

1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Selecteer de virtuele machine waarop u wilt installeren van deze extensie.

    ![Selecteer de virtuele machine](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Selecteer **extensies** blade en klik op de **toevoegen** knop.

    ![Extensies selecteren](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Selecteer de extensie voor diagnostische gegevens van Azure-prestaties bekijkt u de voorwaarden en bepalingen en op de **maken** knop.

    ![Maken van de prestaties van de Azure-extensie voor diagnostische gegevens](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. De parameterwaarden opgeven voor de installatie en klik op **OK** voor het installeren van de extensie. U vindt meer informatie over de ondersteunde scenario's voor het oplossen van problemen [hier](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![De extensie installeren](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Wanneer de installatie voltooid is, ziet u een bericht weergegeven met het inrichten is voltooid.

    ![Bericht inrichting voltooid](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > De uitvoering van de uitbreiding wordt gestart nadat het inrichten is voltooid en het duurt enkele minuten of minder uitgevoerd voor eenvoudige scenario. Voor andere scenario's, deze wordt uitgevoerd door de duur van de opgegeven tijdens de installatie.

## <a name="remove-the-extension"></a>Verwijder de extensie
Als u wilt verwijderen van de uitbreiding van een virtuele machine, de volgende stappen uit:

1. Meld u aan bij de [Azure-portal](http://portal.azure.com), selecteer de virtuele machine waar u deze extensie verwijderen en selecteer vervolgens extensies blade. 
2. Klik op de (**...** ) voor de vermelding prestaties-extensie voor diagnostische gegevens uit de lijst en selecteer verwijderen.

    ![De uitbreiding verwijderen](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > U kunt ook het extensie-item selecteren en selecteer de optie verwijderen niet selecteren.

## <a name="template-deployment"></a>Sjabloonimplementatie
Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de extensie Azure prestatiecontrole uitvoeren tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt. Hier volgt een voorbeeldsjabloon die kan worden gebruikt bij de implementatie van sjabloon:

````
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>PowerShell-implementatie
De `Set-AzureRmVMExtension` opdracht kan worden gebruikt voor het implementeren van de extensie van de prestaties diagnostische Azure-gegevens virtuele machine op een bestaande virtuele machine. Voordat u de opdracht uitvoert, moeten de openbare en persoonlijke configuraties worden opgeslagen in een PowerShell-hash-tabel.

PowerShell

````
$PublicSettings = @{ "performanceScenario":"basic","traceDurationInSeconds":300,"perfCounterTrace":"p","networkTrace":"","xperfTrace":"","storPortTrace":"","srNumber":"","requestTimeUtc":"2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountName":"mystorageaccount","storageAccountKey":"mystoragekey"}

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
````

## <a name="information-on-the-data-captured"></a>Informatie over de gegevens die zijn vastgelegd
PerfInsights hulpprogramma verzamelt verschillende logboeken, configuratie, enz., afhankelijk van het geselecteerde scenario voor diagnostische gegevens. Ga voor meer informatie over de gegevens die worden verzameld per scenario naar [PerfInsights documentatie](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Bekijken en delen van de resultaten

Uitvoer van de uitbreiding wordt opgeslagen in een map met de naam log_collection onder Temp-station (meestal D:\log_collection) standaard. Onder deze map ziet u het zip-bestanden met de logboeken met diagnostische gegevens en een rapport met bevindingen en aanbevelingen.

Het zip-bestand gemaakt ook wordt geüpload naar de storage-account dat is opgegeven tijdens de installatie en wordt gedeeld voor 30 dagen met [Shared Access Signatures (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Een tekstbestand met de naam *zipfilename*_saslink.txt ook in de map log_collection is gemaakt. Dit bestand bevat de SAS-koppeling voor het downloaden van het zip-bestand gemaakt. Iedereen die deze koppeling heeft kan downloaden van het zip-bestand.

Microsoft mag deze SAS-koppeling gebruiken voor het downloaden van de Diagnostics-gegevens voor verder onderzoek door de medewerker op uw ondersteuningsticket werkt.

Als u wilt weergeven in het rapport, net Pak het zipbestand en open **PerfInsights Report.html** bestand.

U kunt ook mogelijk om te downloaden van het zip-bestand rechtstreeks vanuit de portal door te selecteren.

![Gedetailleerde status weergeven](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> De SAS-koppeling weergegeven in de portal werkt mogelijk niet soms vanwege een verkeerd ingedeelde Url (veroorzaakt door een speciale tekens) tijdens coderen en decoderen van de bewerking. De tijdelijke oplossing is om op te halen van de koppeling rechtstreeks vanuit het bestand *_saslink.txt van de virtuele machine.

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen
### <a name="troubleshoot"></a>Problemen oplossen

- Status van extensie-implementatie (in het systeemvak) mogelijk 'Implementatie wordt uitgevoerd' weergegeven, ondanks dat de uitbreiding met succes is ingericht.

    Dit probleem kan worden genegeerd, zolang de status van extensie geeft aan dat de uitbreiding met succes wordt ingericht.
- Kan het enkele problemen tijdens de installatie kunt oplossen met de extensie-Logboeken. De uitvoer van de extensie-uitvoering wordt vastgelegd in bestanden gevonden in de volgende map:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).
