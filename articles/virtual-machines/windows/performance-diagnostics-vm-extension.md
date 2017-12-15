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
ms.openlocfilehash: 5a7dc313f1d6453562e4d5a11ceca03e4459b043
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Prestaties van Azure Diagnostics VM-extensie voor Windows

Azure Diagnostics VM-extensie van prestaties kunt verzamelen van diagnostische gegevens van prestaties van VM's van Windows. De extensie voert analyse en vindt u een rapport van bevindingen en aanbevelingen voor het identificeren en oplossen van prestatieproblemen met voor de virtuele machine. Deze uitbreiding is geïnstalleerd voor probleemoplossing hulpprogramma [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Vereisten

Deze uitbreiding kan worden geïnstalleerd op Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. Het kan ook worden geïnstalleerd op Windows 8.1 en Windows 10.

## <a name="extension-schema"></a>Uitbreidingsschema
De volgende JSON vindt u het schema voor VM-extensie voor Azure prestaties diagnostische gegevens. Deze uitbreiding is vereist voor de naam en de sleutel voor een opslagaccount voor het opslaan van de uitvoer van de diagnostische gegevens en het rapport. Deze waarden gevoelig zijn en moeten worden opgeslagen in de configuratie van een beveiligde instelling. Azure VM-extensie beveiligde instellingsgegevens worden versleuteld en wordt dit alleen ontsleuteld op de virtuele doelmachine. Houd er rekening mee dat **storageAccountName** en **storageAccountKey** zijn hoofdlettergevoelig. Andere vereiste parameters zijn vermeld in de volgende sectie.

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
|apiVersion|2015-06-15|De versie van de API.
|Uitgever|Microsoft.Azure.Performance.Diagnostics|De naamruimte van de uitgever voor de extensie.
|type|AzurePerformanceDiagnostics|Het type van de VM-extensie.
|typeHandlerVersion|1.0|De versie van de extensie-handler.
|performanceScenario|standaard|Het scenario prestaties waarvoor u wilt vastleggen van gegevens. Geldige waarden zijn: **basic**, **vmslow**, **azurefiles**, en **aangepaste**.
|traceDurationInSeconds|300|De duur van de traceringen, als een van de traceeropties zijn geselecteerd.
|perfCounterTrace|P|De optie voor het inschakelen van de teller prestatietracering. Geldige waarden zijn **p** of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde als leeg.
|networkTrace|n|De optie voor het inschakelen van netwerk-Trace. Geldige waarden zijn  **n**  of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde als leeg.
|xperfTrace|x|De optie voor het inschakelen van XPerf Trace. Geldige waarden zijn **x** of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde als leeg.
|storPortTrace|s|De optie StorPort-tracering inschakelen. Geldige waarden zijn **s** of lege waarde. Als u niet vastleggen van deze trace wilt, laat u de waarde als leeg.
|srNumber|123452016365929|Het ticketnummer ondersteuning als deze beschikbaar is. Laat de waarde als leeg als u niet hebt.
|storageAccountName|mystorageaccount|De naam van het opslagaccount voor het opslaan van de logboeken met diagnostische gegevens en de resultaten.
|storageAccountKey|lDuVvxuZB28NNP... hAiRF3voADxLBTcc ==|De sleutel voor het opslagaccount.

## <a name="install-the-extension"></a>De extensie installeren

Volg deze stappen voor de uitbreiding te installeren op Windows virtuele machines:

1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Selecteer de virtuele machine waarop u wilt installeren van deze extensie.

    ![Schermopname van Azure-portal met virtuele machines die zijn gemarkeerd](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Selecteer de **extensies** blade en selecteer **toevoegen**.

    ![Schermopname van extensies blade met toevoegen gemarkeerd](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Selecteer **Azure prestatiecontrole**controleert u de voorwaarden en selecteer **maken**.

    ![Schermafbeelding van de nieuwe resource scherm met Azure prestatiecontrole gemarkeerd](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Geef de parameterwaarden voor de installatie, en selecteer **OK** voor het installeren van de extensie. Zie voor meer informatie over ondersteunde scenario's, [het gebruik van PerfInsights](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Dialoogvenster bestandsnaamextensie schermafbeelding installeren](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Wanneer de installatie voltooid is, ziet u een bericht met deze status.

    ![Schermopname van inrichten is voltooid-bericht](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > De extensie wordt uitgevoerd wanneer het inrichten is voltooid. Het duurt twee minuten of minder om voor het eenvoudige scenario te voltooien. Voor andere scenario's, deze wordt uitgevoerd door de duur van de opgegeven tijdens de installatie.

## <a name="remove-the-extension"></a>Verwijder de extensie
Als u wilt verwijderen van de uitbreiding van een virtuele machine, de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](http://portal.azure.com), selecteer de virtuele machine waaruit u wilt verwijderen van deze extensie en selecteer vervolgens de **extensies** blade. 
2. Selecteer de (**...** ) voor de extensie voor diagnostische gegevens van prestaties vermelding in de lijst en selecteer **verwijderen**.

    ![Schermopname van extensies blade verwijderd die zijn gemarkeerd](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > U kunt ook het extensie-item selecteren en selecteer de **verwijderen** optie.

## <a name="template-deployment"></a>Sjabloonimplementatie
Uitbreidingen van de virtuele machine van Azure kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan worden gebruikt in een Azure Resource Manager-sjabloon. Hiermee wordt de Azure prestaties diagnostische VM-extensie uitgevoerd tijdens de sjabloonimplementatie van een Azure Resource Manager. Hier volgt een voorbeeldsjabloon:

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
De `Set-AzureRmVMExtension` opdracht kan worden gebruikt voor het implementeren van VM-extensie voor Azure prestaties diagnostische gegevens naar een bestaande virtuele machine. Opslaan voordat u de opdracht uitvoert, de openbare en persoonlijke configuraties in een PowerShell-hash-tabel.

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
Het hulpprogramma PerfInsights verzamelt verschillende logboeken, configuratie en diagnostische gegevens, afhankelijk van het geselecteerde scenario. Zie voor meer informatie de [PerfInsights documentatie](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Bekijken en delen van de resultaten

De uitvoer van de uitbreiding wordt opgeslagen in een map. De map met de naam log_collection en vindt u onder het tijdelijke station (meestal D:\log_collection) standaard. Onder deze map ziet u het zip-bestanden met de diagnostische logboeken en een rapport met bevindingen en aanbevelingen.

U kunt ook het zip-bestand vinden in het opslagaccount dat is opgegeven tijdens de installatie. Deze 30 dagen wordt gedeeld met behulp van [Shared Access Signatures (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Een tekstbestand met de naam *zipfilename*_saslink.txt ook in de map log_collection is gemaakt. Dit bestand bevat de SAS-koppeling voor het downloaden van het zip-bestand gemaakt. Iedereen die deze koppeling heeft kan downloaden van het zip-bestand.

Om te helpen de ondersteuningstechnicus van uw ondersteuningsticket gewerkt, kan Microsoft deze SAS-koppeling gebruikt voor het downloaden van de diagnostics-gegevens.

Als u het rapport, pak het zipbestand en open de **PerfInsights Report.html** bestand.

U kunt mogelijk ook downloaden van het zip-bestand rechtstreeks vanuit de portal door te selecteren.

![Schermopname van prestatiecontrole gedetailleerde status](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> De SAS-koppeling weergegeven in de portal werkt mogelijk niet. Dit kan worden veroorzaakt door een verkeerd ingedeelde URL tijdens de bewerkingen coderen en decoderen. In plaats daarvan kunt u de koppeling rechtstreeks vanuit het bestand *_saslink.txt van de virtuele machine ophalen.

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

- Status van extensie-implementatie (in het systeemvak) mogelijk 'Implementatie wordt uitgevoerd' weer, ondanks dat de uitbreiding met succes is ingericht.

    Dit probleem kan worden genegeerd, zolang de status van extensie geeft aan dat de uitbreiding met succes wordt ingericht.
- U kunt sommige problemen oplossen tijdens de installatie met behulp van de logboeken van de extensie. De uitvoer van de extensie-uitvoering wordt vastgelegd in bestanden gevonden in de volgende map:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/), en selecteer **ondersteuning krijgen**. Voor meer informatie over het gebruik van de ondersteuning van Azure de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).
