---
title: Aangepaste scripts uitvoeren op virtuele Linux-machines in Azure | Microsoft Docs
description: Configuratietaken voor Linux-VM automatiseren met behulp van de aangepaste Scriptextensie v1
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: danis
ms.openlocfilehash: 8638b788762a56813c622c0abffe2a8eae3c70c2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57437102"
---
# <a name="use-the-azure-custom-script-extension-version-1-with-linux-virtual-machines"></a>De versie 1 van Azure Custom Script-extensie gebruiken met virtuele Linux-machines

[!INCLUDE [virtual-machines-extensions-deprecation-statement](../../../includes/virtual-machines-extensions-deprecation-statement.md)]

De Custom Script Extension versie 1 downloads en scripts uitvoeren op Azure virtual machines. Deze uitbreiding is handig voor post-implementatieconfiguratie, software-installatie of een andere Configuratiebeheer /-taak. U kunt scripts downloaden via Azure Storage of een andere toegankelijke internetlocatie of u ze aan de extensie-runtime kunt leveren.

De aangepaste Scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen. U kunt deze ook uitvoeren met behulp van Azure CLI, PowerShell, Azure portal of de REST-API van Azure Virtual Machines.

Dit artikel wordt uitgelegd hoe u de aangepaste Scriptextensie van Azure CLI, en hoe u de extensie uitvoert met behulp van een Azure Resource Manager-sjabloon. Dit artikel bevat ook stappen voor probleemoplossing voor Linux-systemen.

Er zijn twee Custom Script-extensies van Linux:

* Version 1 - Microsoft.OSTCExtensions.CustomScriptForLinux

* Version 2 - Microsoft.Azure.Extensions.CustomScript

Ga op nieuwe en bestaande implementaties voor het gebruik van de nieuwe versie ([Microsoft.Azure.Extensions.CustomScript](custom-script-linux.md)) in plaats daarvan. De nieuwe versie is bedoeld als een vervangende drop-in. Daarom de migratie is net zo gemakkelijk als het wijzigen van de naam en versie, u hoeft niet te wijzigen van de configuratie van de extensie.

### <a name="operating-system"></a>Besturingssysteem

Linux-distributies ondersteund:

* CentOS 6.5 en hoger
* Debian 8 en hoger
  * Debian 8,7 wordt niet geleverd met Python2 in de meest recente installatiekopieën die CustomScriptForLinux verbroken.
* FreeBSD
* OpenSUSE 13.1 en hoger
* Oracle Linux 6.4 en hoger
* SUSE Linux Enterprise Server 11 SP3 en hoger
* Ubuntu 12.04 en hoger

### <a name="script-location"></a>Locatie van het script

De extensie kunt u uw Azure Blob storage-referenties gebruiken voor toegang tot Azure Blob-opslag. U kunt ook kan de locatie van het script worden een where, zolang de virtuele machine naar dit eindpunt, zoals GitHub, interne bestandsserver enzovoort doorsturen kunt.

### <a name="internet-connectivity"></a>Verbinding met Internet

Als u nodig hebt voor het downloaden van een script extern zoals GitHub of Azure Storage, klikt u vervolgens aanvullende Firewallnetwerk beveiligingsgroep poorten moeten worden geopend. Bijvoorbeeld als het script zich in Azure Storage bevindt, kunt u toestaan dat toegang via Azure NSG servicetags voor [opslag](../../virtual-network/security-overview.md#service-tags).

Als uw script op een lokale server is, moet u mogelijk nog steeds extra firewall/Network Security Group-poorten moeten worden geopend.

### <a name="tips-and-tricks"></a>Tips en trucs

* Het hoogste aantal fouten voor deze extensie wordt veroorzaakt door fouten in de syntaxis in het script test het script wordt uitgevoerd zonder fouten, en ook plaatsen extra logboekregistratie in het script om gemakkelijker vinden wanneer deze is mislukt.
* Schrijven van scripts die idempotent zijn zijn, zodat als ze opnieuw meer dan één keer per ongeluk uitvoert ophalen, worden er geen wijzigingen in het systeem.
* Zorg ervoor dat de scripts hebben geen gebruikersinvoer nodig wanneer ze worden uitgevoerd.
* Er is 90 minuten toegestaan voor het script uit te voeren, iets langer resulteert in een mislukte bepaling van de extensie.
* Plaats niet opnieuw wordt opgestart in het script, wordt dit problemen veroorzaken met andere extensies die worden geïnstalleerd en na opnieuw opstarten, de uitbreiding wordt niet voortgezet na het opnieuw opstarten. 
* Hebt u een script dat zal opnieuw worden opgestart, toepassingen installeren en uitvoeren van scripts, enzovoort. U moet het opnieuw opstarten met behulp van een Cron-taak, of hulpprogramma's zoals DSC of Chef, Puppet-uitbreidingen plannen.
* De extensie wordt alleen een script één keer uitgevoerd, als u een script uitvoeren op elke keer opstarten, wilt kunt u [cloud-init-installatiekopie](../linux/using-cloud-init.md) en gebruik een [Scripts Per opstarten](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) module. U kunt het script ook gebruiken om een Systemd service-eenheid te maken.
* Als u wilt om te plannen wanneer een script wordt uitgevoerd, moet u de extensie hebben een Cron-taak maken.
* Wanneer het script wordt uitgevoerd, ziet u alleen een 'transitioning' status van de extensie van de Azure portal of de CLI. Als u vaker statusupdates van een script uit te voeren, moet u om uw eigen oplossing te maken.
* Aangepaste scriptextensie biedt geen systeemeigen ondersteuning voor proxy-servers, maar u kunt een hulpprogramma voor bestandsoverdracht die ondersteuning biedt voor proxy-servers in uw script, zoals *Curl*.
* Houd rekening met het niet standaard directory-locaties die uw scripts of opdrachten kunnen afhankelijk zijn, hebben de logica voor het afhandelen van dit.

## <a name="extension-schema"></a>Extensieschema

De configuratie van de aangepaste Scriptextensie bevat items zoals de locatie van het script en de opdracht om te worden uitgevoerd. U kunt deze configuratie opslaan in configuratiebestanden, geef deze op de opdrachtregel of opgeven in een Azure Resource Manager-sjabloon. 

U kunt de gevoelige gegevens opslaan in een beveiligde configuratie die is versleuteld en worden alleen ontsleuteld in de virtuele machine. De configuratie van de beveiligde is nuttig wanneer de opdracht kan worden uitgevoerd geheimen zoals een wachtwoord bevat.

Deze items moeten worden beschouwd als vertrouwelijke gegevens en opgegeven in de configuratie van de instelling voor beveiligde extensies. Azure-VM-extensie beveiligde instellingsgegevens versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "<url>"
      ],
      "enableInternalDNSCheck": true
    },
    "protectedSettings": {
      "storageAccountName": "<storage-account-name>",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "<command>"
    }
  }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Uitgever | Microsoft.OSTCExtensions | string |
| type | CustomScriptForLinux | string |
| typeHandlerVersion | 1.5 | int |
| fileUris (bijvoorbeeld) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (bijvoorbeeld) | python MyPythonScript.py \<my-param1\> | string |
| enableInternalDNSCheck | true | booleaans |
| storageAccountName (bijvoorbeeld) | examplestorageacct | string |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |

### <a name="property-value-details"></a>Details van eigenschap

* `fileUris`: (optioneel, string-matrix) de lijst van de uri van de scripts
* `enableInternalDNSCheck`: (optioneel, bool) standaardwaarde is True, wordt ingesteld op False om DNS-controle uitschakelen.
* `commandToExecute`: (optioneel, string) het entrypoint script om uit te voeren
* `storageAccountName`: (optioneel, string) de naam van opslagaccount
* `storageAccountKey`: (optioneel, string) de toegangssleutel van storage-account

De volgende waarden kunnen worden ingesteld in de openbare of beveiligde instellingen, moet u deze waarden onder de set niet hebben in zowel de openbare als de beveiligde instellingen.

* `commandToExecute`

Met openbare instellingen misschien nuttig voor foutopsporing, maar het is raadzaam dat u beveiligde instellingen.

Openbare instellingen worden naar de virtuele machine waar het script wordt uitgevoerd in niet-versleutelde tekst verzonden.  Beveiligde instellingen zijn versleuteld met behulp van een sleutel die alleen bekend bij de Azure- en de virtuele machine. De instellingen worden opgeslagen met de virtuele machine als ze zijn verzonden dat wil zeggen als de instellingen zijn versleuteld ze worden opgeslagen, worden versleuteld op de virtuele machine. Het certificaat voor het ontsleutelen van de versleutelde waarden is opgeslagen op de virtuele machine en voor het ontsleutelen van (indien nodig)-instellingen tijdens runtime.

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie beschreven kan worden gebruikt in een Azure Resource Manager-sjabloon om uit te voeren van de aangepaste Scriptextensie tijdens de sjabloonimplementatie van een Azure Resource Manager.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": ["http://MyAccount.blob.core.windows.net/vhds/MyShellScript.sh"]
    },
    "protectedSettings": {
      "storageAccountName": "MyAccount",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "sh MyShellScript.sh"
    }
  }
}
```

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Om implementatieproblemen te voorkomen, door de namen te gebruiken zoals hier wordt weergegeven.

## <a name="azure-cli"></a>Azure-CLI

Wanneer u Azure CLI om uit te voeren van de aangepaste Scriptextensie gebruikt, maakt u een configuratiebestand of de bestanden. U moet ten minste 'commandToExecute' hebben.

```azurecli
az vm extension set -n VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.5 \
  --vm-name MyVm --resource-group MyResourceGroup \
  --protected-settings '{"commandToExecute": "echo hello"}'
```

Desgewenst kunt u de instellingen in de opdracht opgeven als een tekenreeks met JSON-indeling. Hiermee wordt de configuratie wordt opgegeven tijdens de uitvoering en zonder een afzonderlijk configuratiebestand.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-voorbeelden

#### <a name="public-configuration-with-no-script-file"></a>Openbare configuratie met niet-scriptbestand

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Configuratie van openbare en beveiligde bestanden

U een openbare-configuratiebestand gebruiken om op te geven van het scriptbestand URI. U kunt een beveiligd configuratiebestand gebruiken om op te geven van de opdracht om te worden uitgevoerd.

Openbare configuratiebestand:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"]
}
```

Beveiligde configuratiebestand:  

```json
{
  "commandToExecute": "./config-music.sh <param1>"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer de aangepaste Scriptextensie wordt uitgevoerd, kan het script wordt gemaakt of gedownload naar een map die vergelijkbaar is met het volgende voorbeeld. Uitvoer van de opdracht wordt ook opgeslagen in deze map in `stdout` en `stderr` bestanden.

```bash
/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-<version>/download/1
```

Problemen oplossen, eerst het logboek van de Linux-Agent controleren, zorg ervoor dat de extensie is uitgevoerd, controleren:

```bash
/var/log/waagent.log
```

U ziet er voor het uitvoeren van de extensie, ziet het er ongeveer als volgt:

```text
2018/04/26 15:29:44.835067 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Target handler state: enabled
2018/04/26 15:29:44.867625 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] [Enable] current handler state is: notinstalled
2018/04/26 15:29:44.959605 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Download, message=Download succeeded, duration=59
2018/04/26 15:29:44.993269 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Initialize extension directory
2018/04/26 15:29:45.022972 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Update settings file: 0.settings
2018/04/26 15:29:45.051763 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Install extension [customscript.py -install]
2018/04/26 15:29:45 CustomScriptForLinux started to handle.
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] cwd is /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Change log file to /var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log
2018/04/26 15:29:46.088212 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Install, message=Launch command succeeded: customscript.py -install, duration=1005
2018/04/26 15:29:46.133367 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable extension [customscript.py -enable]
2018/04/26 15:29:46 CustomScriptForLinux started to handle.
..
2018/04/26 15:29:47.178163 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Enable, message=Launch command succeeded: customscript.py -enable, duration=1012
```

Enkele punten om te weten:

1. Inschakelen is wanneer de opdracht wordt uitgevoerd.
1. Downloaden is gekoppeld aan het downloaden van het pakket met de CustomScript-uitbreiding van Azure, niet de scriptbestanden in fileUris opgegeven.
1. U kunt ook zien welke logboekbestand schrijven af`/var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log
`

Volgende stap is om te gaan een controle van het logboekbestand, dit is de indeling:

```bash
/var/log/azure/<extension-name>/<version>/extension.log file.
```

U ziet er voor de uitvoering van de afzonderlijke, ziet het er ongeveer als volgt:

```text
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable,transitioning,0,Launching the script...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] sequence number is 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] setting file path is/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/config/0.settings
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] JSON config: {"runtimeSettings": [{"handlerSettings": {"protectedSettings": "MIIB0AYJKoZIhvcNAQcDoIIBwTCCAb0CAQAxggF+hnEXRtFKTTuKiFC8gTfHKupUSs7qI0zFYRya", "publicSettings": {"fileUris": ["https://dannytesting.blob.core.windows.net/demo/myBash.sh"]}, "protectedSettingsCertThumbprint": "4385AB21617C2452FF6998C0A37F71A0A01C8368"}}]}
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Config decoded correctly.
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Will try to download files, number of retries = 10, wait SECONDS between retrievals = 20s
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Downloading,transitioning,0,Downloading files...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] No azure storage account and key specified in protected settings. Downloading scripts from external links...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Converting /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh from DOS to Unix formats: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Removing BOM of /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Succeeded to download files, retry count = 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Internal DNS is ready, retry count = 0
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47
2018/04/26 15:29:47
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Daemon,success,0,Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47
2018/04/26 15:29:47
```

Hier kunt u zien:

* Dit logboek is inschakelen opdracht starten
* De instellingen doorgegeven aan de extensie
* De extensie-bestand en het resultaat van deze downloaden.
* De volgende opdracht wordt uitgevoerd en het resultaat.

U kunt ook de uitvoeringsstatus van de aangepaste Scriptextensie ophalen met behulp van Azure CLI:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

De uitvoer lijkt op de volgende tekst:

```azurecli
Name                  ProvisioningState    Publisher                   Version  AutoUpgradeMinorVersion
--------------------  -------------------  ------------------------  ---------  -------------------------
CustomScriptForLinux  Succeeded            Microsoft.OSTCExtensions        1.5  True
```

## <a name="next-steps"></a>Volgende stappen

Zie voor de code, de huidige problemen en de versies [CustomScript-extensie opslagplaats](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript).
