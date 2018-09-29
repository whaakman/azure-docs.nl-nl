---
title: Azure Custom Script Extension voor Windows | Microsoft Docs
description: Taken voor configuratie van Windows-VM automatiseren met behulp van de aangepaste scriptextensie
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/24/2018
ms.author: roiyz
ms.openlocfilehash: 1201e7ec232a5bd45351072949dc0b9c19af434f
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452170"
---
# <a name="custom-script-extension-for-windows"></a>Aangepaste Scriptextensie voor Windows

De aangepaste Scriptextensie downloadt en scripts uitvoeren op Azure virtual machines. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies. De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met Azure CLI, PowerShell, Azure Portal of de REST API van Azure Virtual Machine.

Dit document wordt uitgelegd hoe u de aangepaste Scriptextensie met behulp van de Azure PowerShell-module, Azure Resource Manager-sjablonen en details van stappen voor probleemoplossing in Windows-systemen.

## <a name="prerequisites"></a>Vereisten

> [!NOTE]  
> Gebruik geen aangepaste Scriptextensie om uit te voeren van Update-AzureRmVM met dezelfde virtuele machine als de parameter omdat het wacht op zichzelf.  
>   
> 

### <a name="operating-system"></a>Besturingssysteem

De aangepaste Scriptextensie voor Linux wordt uitgevoerd op de extensie met de extensie ondersteund besturingssysteem van voor meer informatie Zie dit [artikel](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Locatie van het script

De extensie kunt u uw Azure Blob storage-referenties gebruiken voor toegang tot Azure Blob-opslag. U kunt ook kan de locatie van het script worden een where, zolang de virtuele machine naar dit eindpunt, zoals GitHub, interne bestandsserver enzovoort doorsturen kunt.


### <a name="internet-connectivity"></a>Verbinding met Internet
Als u nodig hebt voor het downloaden van een script extern zoals GitHub of Azure Storage, klikt u vervolgens aanvullende Firewallnetwerk beveiligingsgroep poorten moeten worden geopend. Bijvoorbeeld als het script zich in Azure Storage bevindt, kunt u toestaan dat toegang via Azure NSG servicetags voor [opslag](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Als uw script op een lokale server is, moet u mogelijk nog steeds extra firewall/Network Security Group-poorten moeten worden geopend.

### <a name="tips-and-tricks"></a>Tips en trucs
* Het hoogste aantal fouten voor deze extensie wordt veroorzaakt door fouten in de syntaxis in het script test het script wordt uitgevoerd zonder fouten, en ook plaatsen extra logboekregistratie in het script om gemakkelijker vinden wanneer deze is mislukt.
* Schrijven van scripts die idempotent zijn zijn, zodat als ze opnieuw meer dan één keer per ongeluk uitvoert ophalen, worden er geen wijzigingen in het systeem.
* Zorg ervoor dat de scripts hebben geen gebruikersinvoer nodig wanneer ze worden uitgevoerd.
* Er is 90 minuten toegestaan voor het script uit te voeren, iets langer resulteert in een mislukte bepaling van de extensie.
* Plaats niet opnieuw wordt opgestart in het script, wordt dit problemen veroorzaken met andere extensies die worden geïnstalleerd en na opnieuw opstarten, de uitbreiding wordt niet voortgezet na het opnieuw opstarten. 
* Hebt u een script dat zal opnieuw worden opgestart, toepassingen installeren en uitvoeren van scripts, enzovoort. U moet het opnieuw opstarten met behulp van een Windows-taak, of hulpprogramma's zoals DSC of Chef, Puppet-uitbreidingen plannen.
* De extensie wordt een script alleen slechts één keer uitgevoerd als u een script uitvoeren op elke keer opstarten, wilt moet u de extensie gebruikt om te maken van een Windows-taak.
* Als u wilt om te plannen wanneer een script wordt uitgevoerd, moet u de extensie te maken van een Windows-taak. 
* Wanneer het script wordt uitgevoerd, ziet u alleen een 'transitioning' status van de extensie van de Azure portal of de CLI. Als u vaker statusupdates van een script uit te voeren, moet u om uw eigen oplossing te maken.
* Aangepaste scriptextensie biedt geen systeemeigen ondersteuning voor proxy-servers, maar u kunt een hulpprogramma voor bestandsoverdracht die ondersteuning biedt voor proxy-servers in uw script, zoals *Curl* 
* Houd rekening met het niet standaard directory-locaties die uw scripts of opdrachten kunnen afhankelijk zijn, hebben de logica voor het afhandelen van dit.


## <a name="extension-schema"></a>Extensieschema

De configuratie van de aangepaste Scriptextensie bevat items zoals de locatie van het script en de opdracht om te worden uitgevoerd. U kunt deze configuratie opslaan in configuratiebestanden, geef deze op de opdrachtregel of opgeven in een Azure Resource Manager-sjabloon. 

U kunt de gevoelige gegevens opslaan in een beveiligde configuratie die is versleuteld en worden alleen ontsleuteld in de virtuele machine. De configuratie van de beveiligde is nuttig wanneer de opdracht kan worden uitgevoerd geheimen zoals een wachtwoord bevat.

Deze items moeten worden beschouwd als vertrouwelijke gegevens en opgegeven in de configuratie van de instelling voor beveiligde extensies. Azure-VM-extensie beveiligde instellingsgegevens versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```
**Houd er rekening mee** -slechts één versie van een uitbreiding kan worden geïnstalleerd op een virtuele machine op een punt in tijd, op te geven voor aangepaste scripts tweemaal in dezelfde Resource Manager-sjabloon voor dezelfde VM mislukken. 

### <a name="property-values"></a>Waarden van eigenschappen

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.Compute | tekenreeks |
| type | Customscriptextension gebruiken | tekenreeks |
| typeHandlerVersion | 1.9 | int |
| fileUris (bijvoorbeeld) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | matrix |
| commandToExecute (bijvoorbeeld) | PowerShell - ExecutionPolicy Unrestricted - File configureren muziek app.ps1 | tekenreeks |
| storageAccountName (bijvoorbeeld) | examplestorageacct | tekenreeks |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | tekenreeks |

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Om implementatieproblemen te voorkomen, door de namen te gebruiken zoals hier wordt weergegeven.

#### <a name="property-value-details"></a>Details van eigenschap
 * `commandToExecute`: (**vereist**, string) het invoerpunt-script om uit te voeren. Dit veld in plaats daarvan gebruiken als uw opdracht geheimen zoals wachtwoorden bevat, of uw fileUris gevoelig zijn.
* `fileUris`: (optioneel, string-matrix) de URL's voor bestanden die moeten worden gedownload.
* `storageAccountName`: (optioneel, string) de naam van de storage-account. Als u de storage-referenties opgeeft alle `fileUris` moet URL's voor Azure-Blobs.
* `storageAccountKey`: (optioneel, string) de toegangssleutel van storage-account

De volgende waarden kunnen worden ingesteld in de openbare of beveiligde instellingen, de uitbreiding wordt een configuratie waarbij de onderstaande waarden zijn ingesteld in de instellingen voor zowel openbare als beveiligde afwijzen.
* `commandToExecute`

Met openbare instellingen misschien nuttig voor foutopsporing, maar het is raadzaam dat u beveiligde instellingen.

Openbare instellingen worden naar de virtuele machine waar het script wordt uitgevoerd in niet-versleutelde tekst verzonden.  Beveiligde instellingen zijn versleuteld met behulp van een sleutel die alleen bekend bij de Azure- en de virtuele machine. De instellingen worden opgeslagen met de virtuele machine als ze zijn verzonden dat wil zeggen als de instellingen zijn versleuteld ze worden opgeslagen, worden versleuteld op de virtuele machine. Het certificaat voor het ontsleutelen van de versleutelde waarden is opgeslagen op de virtuele machine en voor het ontsleutelen van (indien nodig)-instellingen tijdens runtime.

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie beschreven kan worden gebruikt in een Azure Resource Manager-sjabloon om uit te voeren van de aangepaste Scriptextensie tijdens de sjabloonimplementatie van een Azure Resource Manager. Een voorbeeldsjabloon met de Custom Script Extension vindt u hier [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureRmVMCustomScriptExtension` opdracht kan worden gebruikt om de aangepaste scriptextensie toevoegen aan een bestaande virtuele machine. Zie voor meer informatie, [Set AzureRmVMCustomScriptExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmcustomscriptextension).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```
## <a name="further-examples"></a>Meer voorbeelden

### <a name="using-multiple-script"></a>Met behulp van meerdere Script
In dit voorbeeld kunt hebt u drie scripts die worden gebruikt voor het bouwen van uw server, de 'commandToExecute' aanroepen van het eerste script, dan hebt u opties op hoe de andere worden genoemd, bijvoorbeeld, u een master-script waarmee de uitvoering, met de juiste fout verwerking, logboekregistratie en statusbeheer.

```powershell

$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$Settings = @{"fileUris" = $fileUri};

$storageaccname = "xxxxxxx"
$storagekey = "1234ABCD"
$ProtectedSettings = @{"storageAccountName" = $storageaccname; "storageAccountKey" = $storagekey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzureRmVMExtension -ResourceGroupName myRG `
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "buildserver1" ` 
    -Publisher "Microsoft.Compute" ` 
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" ` 
    -Settings $Settings ` 
    -ProtectedSettings $ProtectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Uitvoeren van scripts van een lokale share
In dit voorbeeld kunt u een lokale SMB-server gebruiken voor de scriptlocatie van uw, houd er rekening mee, hoeft u niet doorgeven in een andere andere instellingen, met uitzondering van *commandToExecute*.

```powershell
$ProtectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};
 
Set-AzureRmVMExtension -ResourceGroupName myRG 
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "serverUpdate" 
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" `
    -ProtectedSettings $ProtectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Het aangepaste script meerdere keren uitvoeren met CLI
Als u wilt de aangepaste scriptextensie meer dan eenmaal wordt uitgevoerd, kunt u dit alleen doen in deze omstandigheden:
1. De extensie 'Name'-parameter is hetzelfde als de vorige implementatie van de extensie.
2. U moet de configuratie van bijgewerkt anders de opdracht wordt niet opnieuw uitgevoerd, bijvoorbeeld, kan u toevoegen in een dynamische eigenschap in met de opdracht, zoals een tijdstempel. 

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure PowerShell-module. Als u wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, moet u de volgende opdracht uitvoeren:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Extensie uitvoering uitvoer wordt naar bestanden die zich onder de volgende map op de virtuele doelmachine geregistreerd.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

De opgegeven bestanden zijn gedownload naar de volgende map op de virtuele doelmachine.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
waar `<n>` is een geheel getal dat kan worden gewijzigd tussen uitvoeringen van de extensie.  De `1.*` waarde komt overeen met de werkelijke, huidige `typeHandlerVersion` waarde van de extensie.  Bijvoorbeeld, de werkelijke map kan worden `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Bij het uitvoeren van de `commandToExecute` opdracht, de uitbreiding wordt ingesteld voor deze map (bijvoorbeeld `...\Downloads\2`) als de huidige werkmap. Hierdoor is het gebruik van relatieve paden te vinden van de bestanden die worden gedownload de `fileURIs` eigenschap. Zie de onderstaande tabel voor meer voorbeelden.

Omdat het absolute downloadpad na verloop van tijd verschillen kan, is het beter om te kiezen voor relatieve scriptbestand paden in de `commandToExecute` string, indien mogelijk. Bijvoorbeeld:
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

Informatie over het pad na het eerste segment van de URI wordt bewaard voor bestanden die worden gedownload de `fileUris` lijst met eigenschappen.  Zoals u in de onderstaande tabel, gedownloade bestanden zijn toegewezen in submappen downloaden in overeenstemming met de structuur van de `fileUris` waarden.  

#### <a name="examples-of-downloaded-files"></a>Voorbeelden van gedownloade bestanden

| URI in fileUris | Relatieve locatie gedownload | Absolute locatie gedownload * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Als wijzigen hierboven, de absolute paden gedurende de levensduur van de virtuele machine, maar niet binnen één uitvoering van de CustomScript-extensie.

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
