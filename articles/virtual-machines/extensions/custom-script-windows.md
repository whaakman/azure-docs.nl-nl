---
title: Azure Custom Script Extension voor Windows | Microsoft Docs
description: Taken voor configuratie van Windows-VM automatiseren met behulp van de aangepaste scriptextensie
services: virtual-machines-windows
manager: carmonm
author: georgewallace
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/02/2019
ms.author: gwallace
ms.openlocfilehash: b71ba69bcf4965ea607e097c392573e77aab6865
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65408278"
---
# <a name="custom-script-extension-for-windows"></a>Aangepaste Scriptextensie voor Windows

De aangepaste Scriptextensie downloadt en scripts uitvoeren op Azure virtual machines. Deze uitbreiding is handig voor post-implementatieconfiguratie, software-installatie, of een andere configuratie- of beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies. De aangepaste Scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan worden uitgevoerd met de Azure CLI, PowerShell, Azure-portal of de REST-API van Azure virtuele Machine.

Dit document wordt uitgelegd hoe u de aangepaste Scriptextensie met behulp van de Azure PowerShell-module, Azure Resource Manager-sjablonen en details van stappen voor probleemoplossing in Windows-systemen.

## <a name="prerequisites"></a>Vereisten

> [!NOTE]  
> Gebruik geen aangepaste Scriptextensie om uit te voeren van Update-AzVM met dezelfde virtuele machine als de parameter omdat het wacht op zichzelf.  

### <a name="operating-system"></a>Besturingssysteem

De Custom Script Extension voor Windows wordt uitgevoerd op de extensie met de extensie ondersteund OSs, voor meer informatie, Zie [Azure-extensie ondersteunde besturingssystemen](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Locatie van het script

U kunt de uitbreiding voor het gebruik van uw Azure Blob storage-referenties voor toegang tot Azure Blob-opslag kunt configureren. Locatie van het script kan overal worden, zolang de virtuele machine naar dit eindpunt, zoals GitHub of een interne bestandsserver doorsturen kunt.

### <a name="internet-connectivity"></a>Verbinding met Internet

Als u een script extern zoals downloaden vanuit GitHub of Azure Storage wilt, klikt u vervolgens extra firewallconfiguratie en Network Security Group-poorten moeten worden geopend. Als uw script bevindt zich in Azure Storage, kunt u bijvoorbeeld toegang met behulp van Azure NSG servicetags voor toestaan [opslag](../../virtual-network/security-overview.md#service-tags).

Als uw script op een lokale server is, moet u mogelijk nog steeds extra firewallconfiguratie en Network Security Group-poorten moeten worden geopend.

### <a name="tips-and-tricks"></a>Tips en trucs

* Het hoogste aantal fouten voor deze extensie is vanwege syntaxisfouten in de van de in het script test het script wordt uitgevoerd zonder fouten, en ook plaatsen extra logboekregistratie in het script om gemakkelijker vinden wanneer deze is mislukt.
* Scripts schrijven die idempotent zijn. Dit zorgt ervoor dat als ze opnieuw per ongeluk worden uitgevoerd, worden er geen wijzigingen in het systeem.
* Zorg ervoor dat de scripts geen gebruikersinvoer nodig hebben wanneer ze worden uitgevoerd.
* De uitvoering van het script mag 90 minuten duren. Als het langer duurt, mislukt de inrichting van de extensie.
* Neem opnieuw opstarten niet in het script op. Deze actie veroorzaakt problemen met andere extensies die worden geïnstalleerd. Na het opnieuw opstarten wordt de extensie niet voortgezet.
* Hebt u een script dat ertoe leiden dat een opnieuw opstarten, klikt u vervolgens installeert toepassingen en scripts uitvoeren, kunt u het opnieuw opstarten met behulp van een Windows-taak plannen of hulpprogramma's zoals DSC, Chef of Puppet-extensies gebruiken.
* De extensie voert een script slechts eenmaal uit. Wilt u dat een script bij iedere start wordt uitgevoerd, dan moet u de extensie gebruiken om een geplande Windows-taak te maken.
* Als u wilt plannen wanneer een script wordt uitgevoerd, dan moet u de extensie gebruiken om een geplande Windows-taak te maken.
* Wanneer het script wordt uitgevoerd, ziet u alleen de extensiestatus 'overgang maken' van de Azure-portal of CLI. Als u meer statusupdates van een actief script wilt, dan moet u uw eigen oplossing maken.
* Aangepaste scriptextensie biedt geen systeemeigen ondersteuning voor proxy-servers, maar u kunt een hulpprogramma voor bestandsoverdracht die ondersteuning biedt voor proxy-servers in uw script, zoals *Curl*
* Houd rekening met niet-standaard maplocaties waar uw scripts of opdrachten eventueel gebruik van maken. Pak deze situatie logisch aan.

## <a name="extension-schema"></a>Extensieschema

De configuratie van de aangepaste Scriptextensie bevat items zoals de locatie van het script en de opdracht om te worden uitgevoerd. U kunt deze configuratie opslaan in configuratiebestanden, geef deze op de opdrachtregel of opgeven in een Azure Resource Manager-sjabloon.

U kunt de gevoelige gegevens opslaan in een beveiligde configuratie die is versleuteld en worden alleen ontsleuteld in de virtuele machine. De configuratie van de beveiligde is nuttig wanneer de opdracht kan worden uitgevoerd geheimen zoals een wachtwoord bevat.

Deze items moeten worden beschouwd als vertrouwelijke gegevens en opgegeven in de configuratie van de instelling voor beveiligde extensies. Azure-VM-extensie beveiligde instellingsgegevens versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

```json
{
    "apiVersion": "2018-06-01",
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
            ],
            "timestamp":123456789
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

> [!NOTE]
> Slechts één versie van een uitbreiding kan worden geïnstalleerd op een virtuele machine op een punt in tijd, op te geven voor aangepaste scripts tweemaal in dezelfde Resource Manager-sjabloon voor dezelfde VM mislukken.

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Compute | string |
| type | CustomScriptExtension | string |
| typeHandlerVersion | 1.9 | int |
| fileUris (bijvoorbeeld) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | array |
| timestamp (bijvoorbeeld) | 123456789 | 32-bits geheel getal |
| commandToExecute (bijvoorbeeld) | powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 | string |
| storageAccountName (bijvoorbeeld) | examplestorageacct | string |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Om implementatieproblemen te voorkomen, door de namen te gebruiken zoals hier wordt weergegeven.

#### <a name="property-value-details"></a>Details van eigenschap

* `commandToExecute`: (**vereist**, string) het invoerpunt-script om uit te voeren. Dit veld in plaats daarvan gebruiken als uw opdracht geheimen zoals wachtwoorden bevat, of uw fileUris gevoelig zijn.
* `fileUris`: (optioneel, string-matrix) de URL's voor bestanden die moeten worden gedownload.
* `timestamp` (optioneel, 32-bits geheel getal) te gebruiken in dit veld alleen voor het activeren van een opnieuw uitvoeren van het script door de waarde van dit veld te wijzigen.  Een geheel getal-waarde is toegestaan. alleen moet deze anders is dan de vorige waarde.
* `storageAccountName`: (optioneel, string) de naam van de storage-account. Als u de storage-referenties opgeeft alle `fileUris` moet URL's voor Azure-Blobs.
* `storageAccountKey`: (optioneel, string) de toegangssleutel van storage-account

De volgende waarden kunnen worden ingesteld in de openbare of beveiligde instellingen, de uitbreiding wordt een configuratie waarbij de onderstaande waarden zijn ingesteld in de instellingen voor zowel openbare als beveiligde afwijzen.

* `commandToExecute`

Met openbare instellingen misschien nuttig voor foutopsporing, maar het wordt aanbevolen dat u beveiligde instellingen.

Openbare instellingen worden naar de virtuele machine waar het script wordt uitgevoerd in niet-versleutelde tekst verzonden.  Beveiligde instellingen zijn versleuteld met behulp van een sleutel die alleen bekend bij de Azure- en de virtuele machine. De instellingen worden opgeslagen met de virtuele machine als ze zijn verzonden, dat wil zeggen, als de instellingen zijn versleuteld deze opgeslagen versleuteld op de virtuele machine. Het certificaat voor het ontsleutelen van de versleutelde waarden is opgeslagen op de virtuele machine en voor het ontsleutelen van (indien nodig)-instellingen tijdens runtime.

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Het JSON-schema, dat wordt beschreven in de vorige sectie kan worden gebruikt in een Azure Resource Manager-sjabloon om uit te voeren van de aangepaste Scriptextensie tijdens de implementatie. De volgende voorbeelden laten zien hoe de aangepaste scriptextensie gebruiken:

* [Zelfstudie: Extensies voor virtuele machines met Azure Resource Manager-sjablonen implementeren](../../azure-resource-manager/resource-manager-tutorial-deploy-vm-extensions.md)
* [Implementeer twee Tier-toepassing op Windows- en Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzVMCustomScriptExtension` opdracht kan worden gebruikt om de aangepaste scriptextensie toevoegen aan een bestaande virtuele machine. Zie voor meer informatie, [Set AzVMCustomScriptExtension](/powershell/module/az.compute/set-azvmcustomscriptextension).

```powershell
Set-AzVMCustomScriptExtension -ResourceGroupName <resourceGroupName> `
    -VMName <vmName> `
    -Location myLocation `
    -FileUri <fileUrl> `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="additional-examples"></a>Aanvullende voorbeelden

### <a name="using-multiple-scripts"></a>Met behulp van meerdere scripts

In dit voorbeeld hebt u drie scripts die worden gebruikt voor het bouwen van uw server. De **commandToExecute** roept het eerste script, hebt u de opties op hoe de andere worden genoemd. U kunt bijvoorbeeld een master-script waarmee de uitvoering, met de juiste foutafhandeling, logboekregistratie en statusbeheer hebben. De scripts worden gedownload naar de lokale computer voor het uitvoeren. Bijvoorbeeld in `1_Add_Tools.ps1` roept u `2_Add_Features.ps1` door toe te voegen `.\2_Add_Features.ps1` voor het script, en Herhaal dit proces voor de andere scripts u definiëren in `$settings`.

```powershell
$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$settings = @{"fileUris" = $fileUri};

$storageAcctName = "xxxxxxx"
$storageKey = "1234ABCD"
$protectedSettings = @{"storageAccountName" = $storageAcctName; "storageAccountKey" = $storageKey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzVMExtension -ResourceGroupName <resourceGroupName> `
    -Location <locationName> `
    -VMName <vmName> `
    -Name "buildserver1" `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion "1.9" `
    -Settings $settings    `
    -ProtectedSettings $protectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Uitvoeren van scripts van een lokale share

In dit voorbeeld kunt u een lokale SMB-server gebruiken voor de locatie van uw script. Op deze manier kunt u niet nodig hebt voor alle andere instellingen, met uitzondering van **commandToExecute**.

```powershell
$protectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};

Set-AzVMExtension -ResourceGroupName <resourceGroupName> `
    -Location <locationName> `
    -VMName <vmName> `
    -Name "serverUpdate"
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion "1.9" `
    -ProtectedSettings $protectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Het aangepaste script meerdere keren uitvoeren met CLI

Als u de aangepaste scriptextensie meer dan één keer uitgevoerd wilt, kunt u deze actie in deze omstandigheden alleen doen:

* De extensie **naam** parameter is hetzelfde als de vorige implementatie van de extensie.
* Werk de configuratie anders wordt die de opdracht wordt niet opnieuw kan worden uitgevoerd. U kunt in een dynamische eigenschap toevoegen in de opdracht, zoals een tijdstempel.

U kunt ook instellen de [ForceUpdateTag](/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension.forceupdatetag) eigenschap **waar**.

### <a name="using-invoke-webrequest"></a>Invoke-WebRequest gebruiken

Als u [Invoke-WebRequest](/powershell/module/microsoft.powershell.utility/invoke-webrequest) in uw script, moet u de parameter `-UseBasicParsing` of anders ontvangt u de volgende fout tijdens het controleren van de gedetailleerde status:

```error
The response content cannot be parsed because the Internet Explorer engine is not available, or Internet Explorer's first-launch configuration is not complete. Specify the UseBasicParsing parameter and try again.
```

## <a name="classic-vms"></a>Klassieke VM's

U kunt de Azure portal of de klassieke Azure-PowerShell-cmdlets gebruiken voor het implementeren van de aangepaste Scriptextensie op klassieke virtuele machines.

### <a name="azure-portal"></a>Azure Portal

Navigeer naar uw klassieke VM-resource. Selecteer **extensies** onder **instellingen**.

Klik op **+ toevoegen** en kies in de lijst met resources **Custom Script Extension**.

Op de **-extensie installeren** pagina, selecteert u het lokale PowerShell-bestand, en geen argumenten invullen en op **Ok**.

### <a name="powershell"></a>PowerShell

Gebruik de [Set AzureVMCustomScriptExtension](/powershell/module/servicemanagement/azure/set-azurevmcustomscriptextension) cmdlet kan worden gebruikt om de aangepaste scriptextensie toevoegen aan een bestaande virtuele machine.

```powershell
# define your file URI
$fileUri = 'https://xxxxxxx.blob.core.windows.net/scripts/Create-File.ps1'

# create vm object
$vm = Get-AzureVM -Name <vmName> -ServiceName <cloudServiceName>

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri $fileUri -Run 'Create-File.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure PowerShell-module. Als u wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, moet u de volgende opdracht uitvoeren:

```powershell
Get-AzVMExtension -ResourceGroupName <resourceGroupName> -VMName <vmName> -Name myExtensionName
```

Uitvoer van de extensie is geregistreerd in de bestanden die zich onder de volgende map op de virtuele doelmachine.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

De opgegeven bestanden zijn gedownload naar de volgende map op de virtuele doelmachine.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```

waar `<n>` is een decimaal geheel getal, uitvoeringen van de uitbreiding kan worden gewijzigd.  De `1.*` waarde komt overeen met de werkelijke, huidige `typeHandlerVersion` waarde van de extensie.  Bijvoorbeeld, de werkelijke map kan worden `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Bij het uitvoeren van de `commandToExecute` opdracht, de uitbreiding wordt ingesteld voor deze map (bijvoorbeeld `...\Downloads\2`) als de huidige werkmap. Dit proces maakt het gebruik van relatieve paden te vinden van de bestanden die worden gedownload de `fileURIs` eigenschap. Zie de onderstaande tabel voor meer voorbeelden.

Omdat het absolute downloadpad na verloop van tijd verschillen kan, is het beter om te kiezen voor relatieve scriptbestand paden in de `commandToExecute` string, indien mogelijk. Bijvoorbeeld:

```json
"commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

Informatie over het pad na het eerste segment van de URI wordt bewaard voor bestanden die worden gedownload de `fileUris` lijst met eigenschappen.  Zoals u in de onderstaande tabel, gedownloade bestanden zijn toegewezen in submappen downloaden in overeenstemming met de structuur van de `fileUris` waarden.  

#### <a name="examples-of-downloaded-files"></a>Voorbeelden van gedownloade bestanden

| URI in fileUris | Relatieve locatie gedownload | Absolute locatie gedownload <sup>1</sup> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<sup>1</sup> de absolute paden wijzigen gedurende de levensduur van de virtuele machine, maar niet binnen één uitvoering van de CustomScript-extensie.

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
