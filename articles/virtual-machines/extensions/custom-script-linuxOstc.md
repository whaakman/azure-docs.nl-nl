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
ms.date: 04/25/2018
ms.author: danis
ms.openlocfilehash: eac64a5b456eb040bcb1ac01c3c86dfde0847e57
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="use-the-azure-custom-script-extension-version-1-with-linux-virtual-machines"></a>Gebruik Azure aangepast Script extensie versie 1 met de virtuele Linux-machines
Aangepast Script extensie versie 1 downloads en scripts uitvoeren op virtuele machines in Azure. Deze uitbreiding is nuttig voor na de implementatie-configuratie, software-installatie of een andere Configuratiebeheer /-taak. U kunt scripts downloaden van Azure Storage of een andere internetlocatie voor toegankelijk of u kunt ze met de extensie runtime opgeven. 

De aangepaste Scriptextensie integreert met Azure Resource Manager-sjablonen. U kunt deze ook uitvoeren met behulp van Azure CLI, PowerShell, de Azure-portal of de REST-API van Azure virtuele Machines.

Dit artikel wordt uitgelegd hoe u de aangepaste Scriptextensie van Azure CLI, en het uitvoeren van de uitbreiding met behulp van een Azure Resource Manager-sjabloon. In dit artikel bevat ook stappen voor probleemoplossing voor Linux-systemen.


Er zijn twee Linux aangepast scriptextensies:
* Versie 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Versie 2 - Microsoft.Azure.Extensions.CustomScript

Schakel over nieuwe en bestaande implementaties voor het gebruik van de nieuwe versie ([Microsoft.Azure.Extensions.CustomScript](\custom-script-linux.md)) in plaats daarvan. De nieuwe versie is bedoeld als een vervangende onverwachte. Daarom de migratie is net zo eenvoudig als het wijzigen van de naam en versie, u hoeft niet te wijzigen van uw configuratie voor de uitbreiding.

 

### <a name="operating-system"></a>Besturingssysteem
Linux-distributies ondersteund:

- CentOS 6.5 en hoger
- Debian 8 en hoger
    - Debian 8,7 wordt niet geleverd met Python2 in de meest recente installatiekopieën die CustomScriptForLinux wordt verbroken.
- FreeBSD
- OpenSUSE 13.1 en hoger
- Oracle Linux, 6.4 en hoger
- SUSE Linux Enterprise Server 11 SP3 en hoger
- Ubuntu 12.04 en hoger

### <a name="script-location"></a>De locatie van script

U kunt de extensie op uw Azure Blob storage-referenties voor toegang tot Azure Blob-opslag gebruiken. Locatie van het script kan ook een where, zolang de virtuele machine naar dat eindpunt, zoals GitHub, interne bestandsserver enzovoort doorsturen kunt.

### <a name="internet-connectivity"></a>Verbinding met Internet
Als u nodig hebt voor het downloaden van een script extern zoals GitHub of Azure Storage, klikt u vervolgens extra Firewallnetwerk beveiligingsgroep poorten moeten worden geopend. Bijvoorbeeld als het script zich in Azure Storage bevindt, kunt u toestaan openen met behulp van Azure NSG Service-Tags voor [opslag](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

Als uw script op een lokale server, wordt u mogelijk aanvullende firewall/netwerk beveiliging nog steeds nodig poorten van de groep moeten worden geopend.

### <a name="tips-and-tricks"></a>Tips en trucs
* Het hoogste Faalpercentage voor deze extensie wordt veroorzaakt door de syntaxisfouten in het script wordt het script wordt uitgevoerd zonder fouten, test en leggen ook extra logboekregistratie in het script gemakkelijker vinden waar deze is mislukt.
* Schrijven van scripts die idempotent zijn, dus als ze opnieuw meer dan één keer per ongeluk uitvoert ophalen, worden er geen wijzigingen in het systeem.
* Zorg ervoor dat de scripts vereisen geen invoer van de gebruiker wanneer ze worden uitgevoerd.
* Er is 90 minuten toegestaan voor het script wilt uitvoeren, iets langer resulteert in een mislukte bepaling van de extensie.
* Plaats niet opnieuw wordt opgestart in het script en dit zorgt voor problemen met andere extensies die worden geïnstalleerd na opnieuw opstarten, de extensie kan niet worden voortgezet na het opnieuw opstarten. 
* Hebt u een script dat opgestart wordt, toepassingen installeren en uitvoeren van scripts, enzovoort. U moet het opnieuw opstarten met behulp van een taak Cron of met de hulpprogramma's zoals DSC of Chef, Puppet-uitbreidingen plannen.
* De uitbreiding wordt alleen keer uit te voeren een script als u een script uitvoeren op elke keer opstarten, wilt kunt u [cloud init installatiekopie](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/using-cloud-init) en gebruik een [Scripts Per Boot](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) module. U kunt ook het script gebruiken voor het maken van een service-eenheid Systemd.
* Als u plannen wilt wanneer een script wordt uitgevoerd, gebruikt u de uitbreiding voor het maken van een taak Cron. 
* Wanneer het script wordt uitgevoerd, ziet u alleen een 'transitioning' status van de extensie van de Azure-portal of de CLI. Als u wilt dat u vaker statusupdates van een script wordt uitgevoerd, moet u om uw eigen oplossing te maken.
* Extensie voor aangepaste scripts systeemeigen ondersteunt geen proxy-servers, maar u kunt een bestand overdracht hulpprogramma die ondersteuning biedt voor proxy-servers in uw script zoals *Curl*. 
* Houd rekening met het niet standaard directory-locaties die uw scripts of opdrachten kunnen afhankelijk zijn, hebben logica voor het afhandelen van dit.



## <a name="extension-schema"></a>Extensieschema

De configuratie van de aangepaste Scriptextensie geeft bijvoorbeeld de locatie van script en de opdracht om te worden uitgevoerd. U kunt deze configuratie opslaan in configuratiebestanden, geef hem op in de opdrachtregel of opgeven in een Azure Resource Manager-sjabloon. 

U kunt de gevoelige gegevens opslaan in een beveiligde configuratie, waardoor wordt gecodeerd en worden alleen ontsleuteld in de virtuele machine. De configuratie van de beveiligde is handig wanneer de opdracht uitvoering geheimen zoals een wachtwoord bevat.

Deze items moeten worden behandeld als gevoelige gegevens en opgegeven in de configuratie van de instelling voor beveiligde uitbreidingen. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld.

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

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld | Gegevenstype | 
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.OSTCExtensions | tekenreeks |
| type | CustomScriptForLinux | tekenreeks |
| typeHandlerVersion | 1.5 | int |
| fileUris (bijvoorbeeld) | https://github.com/MyProject/Archive/MyPythonScript.py | matrix |
| commandToExecute (bijvoorbeeld) | Python MyPythonScript.py < Mijn param1 > | tekenreeks |
| enableInternalDNSCheck | true | booleaans |
| storageAccountName (bijvoorbeeld) | examplestorageacct | tekenreeks |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | tekenreeks |

### <a name="property-value-details"></a>Details van de eigenschap
* `fileUris`: (optioneel, string array) de uri-lijst van de scripts
* `enableInternalDNSCheck`: (optioneel, bool) standaardwaarde is True, wordt ingesteld op False om DNS-controle uitschakelen.
* `commandToExecute`: (optioneel, string) de entrypoint-script uit te voeren
* `storageAccountName`: (optioneel, string) de naam van de storage-account
* `storageAccountKey`: (optioneel, string) de toegangssleutel van storage-account

De volgende waarden kunnen worden ingesteld in de openbare of beveiligde instellingen, moet u deze waarden onder set niet hebben in de openbare en beveiligde instellingen.
* `commandToExecute`

Met de openbare instellingen mogelijk handig voor foutopsporing, maar het is raadzaam dat u beveiligde instellingen.

Instellingen voor openbare worden verzonden in normale tekst naar de virtuele machine waar het script wordt uitgevoerd.  Beveiligde instellingen zijn versleuteld met een sleutel die alleen bekend is bij de Azure en de virtuele machine. De instellingen worden opgeslagen met de virtuele machine als ze zijn verzonden, dat wil zeggen als de instellingen zijn gecodeerd worden ze opgeslagen versleuteld op de virtuele machine. Het certificaat dat wordt gebruikt voor het ontsleutelen van de versleutelde waarden is opgeslagen op de virtuele machine en gebruikt voor het ontsleutelen van instellingen (indien nodig) tijdens runtime.


## <a name="template-deployment"></a>Sjabloonimplementatie
Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de aangepaste Scriptextensie uitvoeren tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt. 


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
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Implementatie om problemen te voorkomen, door de namen te gebruiken, zoals hier wordt weergegeven.

## <a name="azure-cli"></a>Azure-CLI
Wanneer u Azure CLI om uit te voeren van de aangepaste Scriptextensie, maakt u een configuratiebestand of bestanden. U moet ten minste 'commandToExecute' hebben.

```azurecli
az vm extension set -n VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.5 \
  --vm-name MyVm --resource-group MyResourceGroup \
  --protected-settings '{"commandToExecute": "echo hello"}'
```

Desgewenst kunt u de instellingen in de opdracht als een tekenreeks JSON-indeling. Hiermee wordt de configuratie wordt opgegeven tijdens de uitvoering en zonder een afzonderlijk configuratiebestand.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-voorbeelden

#### <a name="public-configuration-with-no-script-file"></a>De openbare configuratie zonder script een bestand

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

Een bestand met de openbare configuratie kunt u de URI van het scriptbestand opgeven. U een beveiligde configuratiebestand gebruiken om op te geven van de opdracht om te worden uitgevoerd.

De openbare configuratie voor bestand:

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

Als u wilt oplossen, eerst Raadpleeg het logboek voor de Linux-Agent, zorg ervoor dat de uitbreiding hebt uitgevoerd, controleren:

```bash
/var/log/waagent.log 
```

Moet u controleren of de uitvoering van de extensie, deze wordt als volgt uitzien:
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
Enkele punten om aan te geven:
1. Schakel is wanneer de opdracht wordt gestart.
2. Download die is gekoppeld aan het downloaden van het pakket van de extensie CustomScript van Azure, niet de scriptbestanden in fileUris opgegeven.
3. U ziet ook logboekbestand dat deze naar '/var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log' out schrijft.

Volgende stap is het een controle van het logboekbestand gaat, dit is de indeling:
```bash
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Moet u controleren of de uitvoering van de induvidual, deze wordt als volgt uitzien:
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
* Extensie van de bestands- en het resultaat van deze downloaden.
* De volgende opdracht wordt uitgevoerd en het resultaat.

U kunt ook de uitvoeringsstatus van de aangepaste Scriptextensie ophalen met behulp van Azure CLI:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

De uitvoer ziet er de volgende tekst:

```azurecli
Name                  ProvisioningState    Publisher                   Version  AutoUpgradeMinorVersion
--------------------  -------------------  ------------------------  ---------  -------------------------
CustomScriptForLinux  Succeeded            Microsoft.OSTCExtensions        1.5  True
```

## <a name="next-steps"></a>Volgende stappen
Zie voor de code, de huidige problemen en de versies [extensie CustomScript opslagplaats](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript).

