---
title: Prestaties van Azure Diagnostics VM-extensie voor Windows | Microsoft Docs
description: Introduceert Azure prestaties diagnostische VM-extensie voor Windows.
services: virtual-machines-windows'
documentationcenter: ''
author: genlin
manager: cshepard
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 4663da6d28d62230ced937cdb5e597a1236c7f99
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258941"
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Prestaties van Azure Diagnostics VM-extensie voor Windows

Azure Diagnostics VM-extensie voor prestaties kunt u diagnostische prestatiegegevens verzamelen van Windows-VM's. De extensie voert analyse en biedt een rapport van de bevindingen en aanbevelingen om u te identificeren en oplossen van problemen met prestaties op de virtuele machine. Deze extensie wordt geïnstalleerd voor een hulpprogramma voor probleemoplossing met de naam [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Vereisten

Deze uitbreiding kan worden geïnstalleerd op Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. Het kan ook worden geïnstalleerd op Windows 8.1 en Windows 10.

## <a name="extension-schema"></a>Extensieschema
De volgende JSON bevat het schema voor VM-extensie voor Azure prestaties diagnostische gegevens. Deze extensie is vereist voor de naam en sleutel voor een opslagaccount voor het opslaan van de uitvoer van de diagnostische gegevens en het rapport. Deze waarden zijn gevoelig. Sleutel van het opslagaccount moet worden opgeslagen in de configuratie van een beveiligde instellen. Azure VM-extensie beveiligde instellingsgegevens worden versleuteld en deze alleen ontsleuteld en dan op de virtuele doelmachine. Houd er rekening mee dat **storageAccountName** en **storageAccountKey** zijn hoofdlettergevoelig. Andere vereiste parameters zijn vermeld in de volgende sectie.

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
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"        
        }
      }
    }
```

### <a name="property-values"></a>Waarden van eigenschappen

|   **Naam**   |**Waarde / voorbeeld**|       **Beschrijving**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|De versie van de API.
|Uitgever|Microsoft.Azure.Performance.Diagnostics|De naamruimte van de uitgever voor de extensie.
|type|AzurePerformanceDiagnostics|Het type van de VM-extensie.
|typeHandlerVersion|1.0|De versie van de extensie-handler.
|performanceScenario|standaard|Het scenario prestaties waarvoor gegevens vast te leggen. Geldige waarden zijn: **basic**, **vmslow**, **Azure Files**, en **aangepaste**.
|traceDurationInSeconds|300|De duur van de traceringen, als een van de traceeropties zijn geselecteerd.
|perfCounterTrace|p|Optie voor het inschakelen van de prestaties teller tracering. Geldige waarden zijn **p** of lege waarde. Als u niet vastleggen van deze tracering wilt, laat u de waarde als leeg zijn.
|networkTrace|n|Optie voor het inschakelen van netwerk-Trace. Geldige waarden zijn **n** of lege waarde. Als u niet vastleggen van deze tracering wilt, laat u de waarde als leeg zijn.
|xperfTrace|x|Optie voor het inschakelen van XPerf tracering. Geldige waarden zijn **x** of lege waarde. Als u niet vastleggen van deze tracering wilt, laat u de waarde als leeg zijn.
|storPortTrace|s|Optie voor het inschakelen van StorPort-Trace. Geldige waarden zijn **s** of lege waarde. Als u niet vastleggen van deze tracering wilt, laat u de waarde als leeg zijn.
|srNumber|123452016365929|Het ticketnummer ondersteuning als deze beschikbaar is. Laat de waarde als leeg als u dit niet hebt.
|requestTimeUtc|2017-09-28T22:08:53.736Z|Huidige datum en tijd in Utc. Als u gebruikmaakt van de portal voor deze extensie te installeren, hoeft u niet voor deze waarde.
|resourceId|/Subscriptions/ {subscriptionId} /resourceGroups/ {resourceGroupName} /providers/ {resourceProviderNamespace} / {resourceType} / {resourceName}|De unieke id van een virtuele machine.
|storageAccountName|mystorageaccount|De naam van het opslagaccount voor het opslaan van de logboeken met diagnostische gegevens en de resultaten.
|storageAccountKey|lDuVvxuZB28NNP…hAiRF3voADxLBTcc==|De sleutel voor het opslagaccount.

## <a name="install-the-extension"></a>De extensie installeren

Volg deze instructies voor de extensie installeren op Windows-machines:

1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Selecteer de virtuele machine waar u deze extensie te installeren.

    ![Schermopname van het Azure-portal, met virtuele machines die zijn gemarkeerd](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Selecteer de **extensies** blade, en selecteer **toevoegen**.

    ![Schermafbeelding van de extensies blade met toevoegen gemarkeerd](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Selecteer **Azure Prestatiediagnoses**, lees de voorwaarden en bepalingen, en selecteer **maken**.

    ![Schermafbeelding van de nieuwe resource-scherm, met Azure Prestatiediagnoses gemarkeerd](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Geef de parameterwaarden voor de installatie op en selecteer **OK** voor het installeren van de extensie. Zie voor meer informatie over ondersteunde scenario's, [PerfInsights gebruiken](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Dialoogvenster bestandsnaamextensie Screenshot van installeren](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Wanneer de installatie voltooid is, ziet u een bericht met deze status.

    ![Bericht Screenshot van inrichting geslaagd](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > De extensie wordt uitgevoerd wanneer het inrichten is voltooid. Het duurt twee minuten of minder om voor het eenvoudige scenario te voltooien. Voor andere scenario's, wordt deze uitgevoerd via de duur van de opgegeven tijdens de installatie.

## <a name="remove-the-extension"></a>Verwijder de extensie
Als u wilt verwijderen van de extensie van een virtuele machine, de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](http://portal.azure.com), selecteer de virtuele machine van waaruit u wilt deze extensie te verwijderen en selecteer vervolgens de **extensies** blade. 
2. Selecteer de (**...** ) voor de extensie voor diagnostische gegevens van prestaties vermelding in de lijst en selecteer **verwijderen**.

    ![Schermafbeelding van de extensies blade verwijderen gemarkeerd](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > U kunt ook de extensie-item selecteren en selecteer de **verwijderen** optie.

## <a name="template-deployment"></a>Sjabloonimplementatie
Extensies voor Azure virtuele machines kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie worden beschreven, kan worden gebruikt in een Azure Resource Manager-sjabloon. Hiermee wordt de Azure prestaties diagnostische VM-extensie uitgevoerd tijdens de sjabloonimplementatie van een Azure Resource Manager. Hier volgt een voorbeeldsjabloon:

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
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>PowerShell-implementatie
De `Set-AzureRmVMExtension` opdracht kan worden gebruikt voor het implementeren van VM-extensie voor Azure prestaties diagnostische gegevens naar een bestaande virtuele machine.

PowerShell

````
$PublicSettings = @{ "storageAccountName"="mystorageaccount";"performanceScenario"="basic";"traceDurationInSeconds"=300;"perfCounterTrace"="p";"networkTrace"="";"xperfTrace"="";"storPortTrace"="";"srNumber"="";"requestTimeUtc"="2017-09-28T22:08:53.736Z";"resourceId"="VMResourceId" }
$ProtectedSettings = @{"storageAccountKey"="mystoragekey" }

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS
````

## <a name="information-on-the-data-captured"></a>Meer informatie over de gegevens die zijn vastgelegd
Het hulpprogramma PerfInsights verzamelt verschillende logboeken, configuratie en diagnostische gegevens, afhankelijk van het geselecteerde scenario. Zie voor meer informatie de [PerfInsights documentatie](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Weergeven en delen van de resultaten

Uitvoer van de extensie kunt u vinden in een zip-bestand geüpload naar het opslagaccount dat u opgeeft tijdens de installatie en wordt gedeeld voor 30 dagen met behulp van [Shared Access Signatures (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Dit zip-bestand bevat de logboeken met diagnostische gegevens en een rapport met resultaten en aanbevelingen. Een SAS-koppeling naar het zip-bestand voor uitvoer kan worden gevonden in een tekstbestand met de naam *zipfilename*_saslink.txt onder de map **C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics \\ \<versie >**. Iedereen die deze koppeling kan voor het downloaden van het zip-bestand.

Om te helpen de ondersteuningstechnicus van uw ondersteuningsticket gewerkt, kan Microsoft deze SAS-koppeling gebruiken om de diagnostische gegevens te downloaden.

Als u wilt weergeven van het rapport, pak het zipbestand en open de **PerfInsights Report.html** bestand.

U moet ook mogelijk om te downloaden van het zip-bestand rechtstreeks vanuit de portal door te selecteren van de extensie.

![Schermafbeelding van de prestaties van diagnostische gegevens over gedetailleerde status](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> De SAS-koppeling weergegeven in de portal werken soms niet. Dit kan worden veroorzaakt door een onjuist gevormde URL tijdens de bewerkingen voor coderen en decoderen. In plaats daarvan krijgt u de koppeling rechtstreeks vanuit het bestand *_saslink.txt van de virtuele machine.

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

- Status van de extensie-implementatie (in het systeemvak) weergeven 'Implementatie wordt uitgevoerd' ondanks dat de extensie is ingericht.

    Dit probleem kan veilig worden genegeerd, zolang de status van de extensie geeft aan dat de extensie is ingericht.
- U kunt enkele problemen oplossen tijdens de installatie met behulp van de extensie-Logboeken. Extensie uitvoering uitvoer wordt vastgelegd op bestanden die zijn gevonden in de volgende map:

        C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics\<version>

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/), en selecteer **ondersteuning krijgen**. Voor meer informatie over het gebruik van Azure-ondersteuning, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
