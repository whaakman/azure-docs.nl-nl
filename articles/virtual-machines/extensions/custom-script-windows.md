---
title: De extensie Azure aangepaste scripts voor Windows | Microsoft Docs
description: Configuratietaken voor Windows-VM automatiseren met behulp van de aangepaste scriptextensie
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
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
ms.author: danis
ms.openlocfilehash: 80f9ecd40c5b9504a6554b95bf374046d8253933
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809774"
---
# <a name="custom-script-extension-for-windows"></a>Extensie voor aangepaste scripts voor Windows

De aangepaste Scriptextensie downloads en scripts op virtuele machines in Azure wordt uitgevoerd. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies. De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met Azure CLI, PowerShell, Azure Portal of de REST API van Azure Virtual Machine.

Dit document details over het gebruik van de aangepaste Scriptextensie met behulp van de Azure PowerShell-module, Azure Resource Manager-sjablonen en details stappen voor probleemoplossing in Windows-systemen.

## <a name="prerequisites"></a>Vereisten

> [!NOTE]  
> Gebruik geen extensie voor aangepaste scripts uit te voeren Update-AzureRmVM met dezelfde virtuele machine als de parameter, omdat het wacht op zichzelf.  
>   
> 

### <a name="operating-system"></a>Besturingssysteem

De aangepaste Scriptextensie voor Linux wordt uitgevoerd op de uitbreiding met de extensie ondersteund besturingssysteem van voor meer informatie ziet u dit [artikel](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>De locatie van script

U kunt de extensie op uw Azure Blob storage-referenties voor toegang tot Azure Blob-opslag gebruiken. Locatie van het script kan ook een where, zolang de virtuele machine naar dat eindpunt, zoals GitHub, interne bestandsserver enzovoort doorsturen kunt.


### <a name="internet-connectivity"></a>Verbinding met Internet
Als u nodig hebt voor het downloaden van een script extern zoals GitHub of Azure Storage, klikt u vervolgens extra Firewallnetwerk beveiligingsgroep poorten moeten worden geopend. Bijvoorbeeld als het script zich in Azure Storage bevindt, kunt u toestaan openen met behulp van Azure NSG Service-Tags voor [opslag](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Als uw script op een lokale server, wordt u mogelijk aanvullende firewall/netwerk beveiliging nog steeds nodig poorten van de groep moeten worden geopend.

### <a name="tips-and-tricks"></a>Tips en trucs
* Het hoogste Faalpercentage voor deze extensie wordt veroorzaakt door de syntaxisfouten in het script wordt het script wordt uitgevoerd zonder fouten, test en leggen ook extra logboekregistratie in het script gemakkelijker vinden waar deze is mislukt.
* Schrijven van scripts die idempotent zijn, dus als ze opnieuw meer dan één keer per ongeluk uitvoert ophalen, worden er geen wijzigingen in het systeem.
* Zorg ervoor dat de scripts vereisen geen invoer van de gebruiker wanneer ze worden uitgevoerd.
* Er is 90 minuten toegestaan voor het script wilt uitvoeren, iets langer resulteert in een mislukte bepaling van de extensie.
* Plaats niet opnieuw wordt opgestart in het script en dit zorgt voor problemen met andere extensies die worden geïnstalleerd na opnieuw opstarten, de extensie kan niet worden voortgezet na het opnieuw opstarten. 
* Hebt u een script dat opgestart wordt, toepassingen installeren en uitvoeren van scripts, enzovoort. U moet het opnieuw opstarten met behulp van een geplande taak voor Windows of met de hulpprogramma's zoals DSC of Chef, Puppet-uitbreidingen plannen.
* De uitbreiding wordt een script alleen slechts één keer uitgevoerd als u een script uitvoeren op elke keer opstarten, wilt moet u de uitbreiding wordt gebruikt voor het maken van een geplande taak van Windows.
* Als u plannen wilt wanneer een script wordt uitgevoerd, gebruikt u de uitbreiding voor het maken van een geplande taak van Windows. 
* Wanneer het script wordt uitgevoerd, ziet u alleen een 'transitioning' status van de extensie van de Azure-portal of de CLI. Als u wilt dat u vaker statusupdates van een script wordt uitgevoerd, moet u om uw eigen oplossing te maken.
* Extensie voor aangepaste scripts systeemeigen ondersteunt geen proxy-servers, maar u kunt een bestand overdracht hulpprogramma die ondersteuning biedt voor proxy-servers in uw script zoals *Curl* 
* Houd rekening met het niet standaard directory-locaties die uw scripts of opdrachten kunnen afhankelijk zijn, hebben logica voor het afhandelen van dit.


## <a name="extension-schema"></a>Extensieschema

De configuratie van de aangepaste Scriptextensie geeft bijvoorbeeld de locatie van script en de opdracht om te worden uitgevoerd. U kunt deze configuratie opslaan in configuratiebestanden, geef hem op in de opdrachtregel of opgeven in een Azure Resource Manager-sjabloon. 

U kunt de gevoelige gegevens opslaan in een beveiligde configuratie, waardoor wordt gecodeerd en worden alleen ontsleuteld in de virtuele machine. De configuratie van de beveiligde is handig wanneer de opdracht uitvoering geheimen zoals een wachtwoord bevat.

Deze items moeten worden behandeld als gevoelige gegevens en opgegeven in de configuratie van de instelling voor beveiligde uitbreidingen. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

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
**Opmerking** -slechts één versie van een uitbreiding kan worden geïnstalleerd op een virtuele machine op een punt in tijd, het opgeven van aangepaste scripts tweemaal in dezelfde Resource Manager-sjabloon voor dezelfde virtuele machine mislukt. 

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.Compute | tekenreeks |
| type | CustomScriptExtension | tekenreeks |
| typeHandlerVersion | 1.9 | int |
| fileUris (bijvoorbeeld) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | matrix |
| commandToExecute (bijvoorbeeld) | PowerShell - ExecutionPolicy Unrestricted - bestand configureren muziek app.ps1 | tekenreeks |
| storageAccountName (bijvoorbeeld) | examplestorageacct | tekenreeks |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | tekenreeks |

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Implementatie om problemen te voorkomen, door de namen te gebruiken, zoals hier wordt weergegeven.

#### <a name="property-value-details"></a>Details van de eigenschap
 * `commandToExecute`: (**vereist**, string) de vermelding punt script uit te voeren. Dit veld in plaats daarvan gebruiken als uw opdracht geheimen zoals wachtwoorden bevat, of uw fileUris gevoelig zijn.
* `fileUris`: (optioneel, string array) de URL's voor bestanden die moeten worden gedownload.
* `storageAccountName`: (optioneel, string) de naam van de storage-account. Als u de storage-referenties opgeeft alle `fileUris` URL's moeten zijn voor Azure Blobs.
* `storageAccountKey`: (optioneel, string) de toegangssleutel van storage-account

De volgende waarden kunnen worden ingesteld in de openbare of beveiligde instellingen, wijst u de uitbreiding wordt een willekeurige configuratie waarbij de onderstaande waarden worden ingesteld in de openbare en beveiligde instellingen af.
* `commandToExecute`

Met de openbare instellingen mogelijk handig voor foutopsporing, maar het is raadzaam dat u beveiligde instellingen.

Instellingen voor openbare worden verzonden in normale tekst naar de virtuele machine waar het script wordt uitgevoerd.  Beveiligde instellingen zijn versleuteld met een sleutel die alleen bekend is bij de Azure en de virtuele machine. De instellingen worden opgeslagen met de virtuele machine als ze zijn verzonden, dat wil zeggen als de instellingen zijn gecodeerd worden ze opgeslagen versleuteld op de virtuele machine. Het certificaat dat wordt gebruikt voor het ontsleutelen van de versleutelde waarden is opgeslagen op de virtuele machine en gebruikt voor het ontsleutelen van instellingen (indien nodig) tijdens runtime.

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de aangepaste Scriptextensie uitvoeren tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt. Een voorbeeldsjabloon met de aangepaste Scriptextensie vindt u hier, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureRmVMCustomScriptExtension` opdracht kan worden gebruikt voor de extensie voor aangepaste scripts toevoegen aan een bestaande virtuele machine. Zie voor meer informatie [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
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
In dit voorbeeld kunt hebt u drie scripts die worden gebruikt voor het bouwen van uw server, het aanroepen van de 'commandToExecute' het eerste script, dan hebt u opties op hoe de andere worden genoemd, bijvoorbeeld, u een master-script waarmee de uitvoering, met de juiste fout verwerking, logboekregistratie en statusbeheer.

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
In dit voorbeeld die u kunt een lokale SMB-server gebruiken voor de locatie van uw script Opmerking: u hoeft niet doorgeven in een andere andere instellingen, met uitzondering van *commandToExecute*.

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

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Aangepast script meerdere keren uitvoeren met CLI
Als u de extensie voor aangepaste scripts meer dan één keer worden uitgevoerd wilt, kunt u dit alleen doen in deze omstandigheden:
1. De extensie 'Name'-parameter is hetzelfde als de vorige implementatie van de extensie.
2. U moet de configuratie van bijgewerkt anders de opdracht wordt niet opnieuw uitgevoerd, bijvoorbeeld, kan u toevoegen in een dynamische eigenschap in op de opdracht, zoals een tijdstempel. 

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure PowerShell-module. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

De uitvoer van de extensie-uitvoering is geregistreerd in bestanden gevonden in de volgende map op de virtuele doelmachine.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

De opgegeven bestanden zijn gedownload naar de volgende map op de virtuele doelmachine.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
waar `<n>` is een decimal integer die tussen uitvoeringen van de uitbreiding mag wijzigen.  De `1.*` waarde komt overeen met de werkelijke, huidige `typeHandlerVersion` waarde van de extensie.  Bijvoorbeeld: de werkelijke map kan niet `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Bij het uitvoeren van de `commandToExecute` opdracht, de uitbreiding wordt ingesteld voor deze map (bijvoorbeeld `...\Downloads\2`) als de huidige werkmap. Dit maakt het gebruik van relatieve paden te vinden van de bestanden gedownload de `fileURIs` eigenschap. Zie de onderstaande tabel voor voorbeelden.

Omdat het absolute downloadpad na verloop van tijd variëren kan, is het beter om te kiezen voor relatieve scriptbestand paden in de `commandToExecute` tekenreeks, indien mogelijk. Bijvoorbeeld:
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

Informatie over het pad na het eerste segment van de URI wordt bewaard voor bestanden die worden gedownload de `fileUris` eigenschappenlijst.  Zoals u in de onderstaande tabel, gedownloade bestanden zijn toegewezen in submappen downloaden in overeenstemming met de structuur van de `fileUris` waarden.  

#### <a name="examples-of-downloaded-files"></a>Voorbeelden van gedownloade bestanden

| URI in fileUris | Relatieve locatie voor gedownloade | Absolute locatie gedownload * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Hierboven Wijzig de absolute paden gedurende de levensduur van de virtuele machine, maar niet binnen één uitvoering van de extensie CustomScript.

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).
