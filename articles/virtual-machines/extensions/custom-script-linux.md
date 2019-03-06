---
title: Aangepaste scripts uitvoeren op virtuele Linux-machines in Azure | Microsoft Docs
description: Configuratietaken voor Linux-VM automatiseren met behulp van de aangepaste Scriptextensie v2
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/25/2018
ms.author: roiyz
ms.openlocfilehash: b8f343c2293df6a1dec808addf8881c27514fc06
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57436644"
---
# <a name="use-the-azure-custom-script-extension-version-2-with-linux-virtual-machines"></a>De versie 2 van Azure Custom Script-extensie gebruiken met virtuele Linux-machines
De Custom Script Extension versie 2 downloads en scripts uitvoeren op Azure virtual machines. Deze uitbreiding is handig voor post-implementatieconfiguratie, software-installatie of een andere Configuratiebeheer /-taak. U kunt scripts downloaden via Azure Storage of een andere toegankelijke internetlocatie of u ze aan de extensie-runtime kunt leveren. 

De aangepaste Scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen. U kunt deze ook uitvoeren met behulp van Azure CLI, PowerShell of de REST-API van Azure Virtual Machines.

Dit artikel wordt uitgelegd hoe u de aangepaste Scriptextensie van Azure CLI, en hoe u de extensie uitvoert met behulp van een Azure Resource Manager-sjabloon. Dit artikel bevat ook stappen voor probleemoplossing voor Linux-systemen.


Er zijn twee Custom Script-extensies van Linux:
* Version 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Version 2 - Microsoft.Azure.Extensions.CustomScript

Schakel over nieuwe en bestaande implementaties voor het gebruik van de nieuwe versie 2 in plaats daarvan. De nieuwe versie is bedoeld als een vervangende drop-in. Daarom de migratie is net zo gemakkelijk als het wijzigen van de naam en versie, u hoeft niet te wijzigen van de configuratie van de extensie.


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
* Hebt u een script dat zal opnieuw worden opgestart, toepassingen installeren en uitvoeren van scripts, enzovoort. U moet het opnieuw opstarten met behulp van een Cron-taak, of hulpprogramma's zoals DSC of Chef, Puppet-uitbreidingen plannen.
* De extensie wordt alleen een script één keer uitgevoerd, als u een script uitvoeren op elke keer opstarten, wilt kunt u [cloud-init-installatiekopie](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init) en gebruik een [Scripts Per opstarten](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) module. U kunt het script ook gebruiken om een Systemd service-eenheid te maken.
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
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "skipDos2Unix":false,
      "timestamp":123456789          
    },
    "protectedSettings": {
       "commandToExecute": "<command-to-execute>",
       "script": "<base64-script-to-execute>",
       "storageAccountName": "<storage-account-name>",
       "storageAccountKey": "<storage-account-key>",
       "fileUris": ["https://.."]  
    }
  }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype | 
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Uitgever | Microsoft.Compute.Extensions | string |
| type | CustomScript | string |
| typeHandlerVersion | 2.0 | int |
| fileUris (bijvoorbeeld) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (bijvoorbeeld) | python MyPythonScript.py <my-param1> | string |
| Script | IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo= | string |
| skipDos2Unix (bijvoorbeeld) | false | booleaans |
| tijdstempel (bijvoorbeeld) | 123456789 | 32-bits geheel getal |
| storageAccountName (bijvoorbeeld) | examplestorageacct | string |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |

### <a name="property-value-details"></a>Details van eigenschap
* `skipDos2Unix`: (optioneel, boolean) dos2unix conversie van URL's op basis van een script bestand of script overslaan.
* `timestamp` (optioneel, 32-bits geheel getal) te gebruiken in dit veld alleen voor het activeren van een opnieuw uitvoeren van het script door de waarde van dit veld te wijzigen.  Een geheel getal-waarde is toegestaan. alleen moet deze anders is dan de vorige waarde.
 * `commandToExecute`: (**vereist** als script niet is ingesteld, string) het invoerpunt-script om uit te voeren. Dit veld in plaats daarvan gebruiken als uw opdracht geheimen zoals wachtwoorden bevat.
* `script`: (**vereist** als commandToExecute niet ingesteld, string) een base64-gecodeerd (en eventueel gzip'ed) script/bin/sh uitgevoerd.
* `fileUris`: (optioneel, string-matrix) de URL's voor bestanden die moeten worden gedownload.
* `storageAccountName`: (optioneel, string) de naam van de storage-account. Als u de storage-referenties opgeeft alle `fileUris` moet URL's voor Azure-Blobs.
* `storageAccountKey`: (optioneel, string) de toegangssleutel van storage-account


De volgende waarden kunnen worden ingesteld in de openbare of beveiligde instellingen, de uitbreiding wordt een configuratie waarbij de onderstaande waarden zijn ingesteld in de instellingen voor zowel openbare als beveiligde afwijzen.
* `commandToExecute`
* `script`
* `fileUris`

Met openbare instellingen misschien nuttig voor foutopsporing, maar het is raadzaam dat u beveiligde instellingen.

Openbare instellingen worden naar de virtuele machine waar het script wordt uitgevoerd in niet-versleutelde tekst verzonden.  Beveiligde instellingen zijn versleuteld met behulp van een sleutel die alleen bekend bij de Azure- en de virtuele machine. De instellingen worden opgeslagen met de virtuele machine als ze zijn verzonden dat wil zeggen als de instellingen zijn versleuteld ze worden opgeslagen, worden versleuteld op de virtuele machine. Het certificaat voor het ontsleutelen van de versleutelde waarden is opgeslagen op de virtuele machine en voor het ontsleutelen van (indien nodig)-instellingen tijdens runtime.

#### <a name="property-skipdos2unix"></a>Eigenschap: skipDos2Unix

De standaardwaarde is ingesteld op false, wat betekent dat de conversie dos2unix **is** uitgevoerd.

De vorige versie van CustomScript, Microsoft.OSTCExtensions.CustomScriptForLinux, zou automatisch DOS-bestanden converteren naar UNIX-bestanden door het vertalen van de `\r\n` naar `\n`. Deze omzetting bestaat en is standaard ingeschakeld. Deze conversie een hoeveelheid wordt toegepast op alle bestanden die zijn gedownload uit fileUris of de scriptinstelling op basis van een van de volgende criteria voldoen.

* Als de extensie een van de is `.sh`, `.txt`, `.py`, of `.pl` wordt geconverteerd. De scriptinstelling van het wordt altijd overeen met deze criteria, omdat deze wordt ervan uitgegaan dat een script dat wordt uitgevoerd met /bin/sh en wordt opgeslagen als script.sh op de virtuele machine.
* Als het bestand wordt gestart met `#!`.

De conversie dos2unix worden overgeslagen door in te stellen de skipDos2Unix op ' True '.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>",
  "skipDos2Unix": true
}
```

####  <a name="property-script"></a>Eigenschap: script

CustomScript ondersteunt de uitvoering van een door de gebruiker gedefinieerd script. De scriptinstellingen commandToExecute en fileUris combineren in een afzonderlijke instelling. U kunt gewoon het script als een instelling in plaats van dat voor het instellen van een bestand downloaden uit Azure storage of GitHub basisvertalingen coderen. Script kan worden gebruikt om te vervangen commandToExecute en fileUris.

Het script **moet** worden base64-gecodeerd.  Het script kunt **eventueel** gzip'ed worden. De scriptinstelling van het kan worden gebruikt in de openbare of beveiligde instellingen. De maximale grootte van de gegevens van de scriptparameter is 256 KB. Als het script groter is dan deze grootte die wordt niet uitgevoerd.

Bijvoorbeeld het volgende script dat is opgeslagen in het bestand /script.sh/ gegeven.

```sh
#!/bin/sh
echo "Updating packages ..."
apt update
apt upgrade -y
```

De juiste instelling van de CustomScript-script zou worden samengesteld door de uitvoer van de volgende opdracht uit.

```sh
cat script.sh | base64 -w0
```

```json
{
  "script": "IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo="
}
```

Het script kan eventueel worden gzip'ed om verder te beperken grootte (in de meeste gevallen). (CustomScript automatisch gedetecteerd het gebruik van gzip-compressie.)

```sh
cat script | gzip -9 | base64 -w 0
```

CustomScript maakt gebruik van de volgende algoritme voor het uitvoeren van een script.

 1. Assert dat de lengte van de waarde van het script niet groter zijn dan 256 KB.
 1. Base64-waarde van het script worden gedecodeerd
 1. _poging_ naar gunzip de met base64 waarde gedecodeerd
 1. de waarde van het gedecodeerde (en eventueel gedecomprimeerde) schrijven naar schijf (/ var/lib/waagent/custom-script/#/script.sh)
 1. Voer het script met behulp van _ / bin/sh - c /var/lib/waagent/custom-script/#/script.sh.


## <a name="template-deployment"></a>Sjabloonimplementatie
Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie beschreven kan worden gebruikt in een Azure Resource Manager-sjabloon om uit te voeren van de aangepaste Scriptextensie tijdens de sjabloonimplementatie van een Azure Resource Manager. Een voorbeeldsjabloon met de Custom Script Extension vindt u hier [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


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
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <param2>",
      "fileUris": ["https://github.com/MyProject/Archive/hello.sh"
      ]  
    }
  }
}
```

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Om implementatieproblemen te voorkomen, door de namen te gebruiken zoals hier wordt weergegeven.

## <a name="azure-cli"></a>Azure-CLI
Wanneer u Azure CLI om uit te voeren van de aangepaste Scriptextensie gebruikt, maakt u een configuratiebestand of de bestanden. U moet ten minste 'commandToExecute' hebben.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings ./script-config.json
```

Desgewenst kunt u de instellingen in de opdracht opgeven als een tekenreeks met JSON-indeling. Hiermee wordt de configuratie wordt opgegeven tijdens de uitvoering en zonder een afzonderlijk configuratiebestand.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-voorbeelden

#### <a name="public-configuration-with-script-file"></a>Openbare configuratie met een scriptbestand

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Azure CLI-opdracht:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json
```

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
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
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
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \ 
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Problemen oplossen
Wanneer de aangepaste Scriptextensie wordt uitgevoerd, kan het script wordt gemaakt of gedownload naar een map die vergelijkbaar is met het volgende voorbeeld. Uitvoer van de opdracht wordt ook opgeslagen in deze map in `stdout` en `stderr` bestanden.

```bash
/var/lib/waagent/custom-script/download/0/
```

Problemen oplossen, eerst het logboek van de Linux-Agent controleren, zorg ervoor dat de extensie is uitgevoerd, controleren:

```bash
/var/log/waagent.log 
```

U ziet er voor het uitvoeren van de extensie, ziet het er ongeveer als volgt:
```text
2018/04/26 17:47:22.110231 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] [Enable] current handler state is: notinstalled
2018/04/26 17:47:22.306407 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Download, message=Download succeeded, duration=167
2018/04/26 17:47:22.339958 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Initialize extension directory
2018/04/26 17:47:22.368293 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Update settings file: 0.settings
2018/04/26 17:47:22.394482 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Install extension [bin/custom-script-shim install]
2018/04/26 17:47:23.432774 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Install, message=Launch command succeeded: bin/custom-script-shim install, duration=1007
2018/04/26 17:47:23.476151 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Enable extension [bin/custom-script-shim enable]
2018/04/26 17:47:24.516444 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Enable, message=Launch command succeeded: bin/custom-sc
```
Enkele punten om te weten:
1. Inschakelen is wanneer de opdracht wordt uitgevoerd.
2. Downloaden is gekoppeld aan het downloaden van het pakket met de CustomScript-uitbreiding van Azure, niet de scriptbestanden in fileUris opgegeven.


De Scriptextensie van Azure produceert een logboek dat u hier kunt vinden:

```bash
/var/log/azure/custom-script/handler.log
```

U ziet er voor de uitvoering van de afzonderlijke, ziet het er ongeveer als volgt:
```text
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=start
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=pre-check
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="comparing seqnum" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="seqnum saved" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="reading configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="read configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating json schema"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="json schema valid"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsing configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsed configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating configuration logically"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validated configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="creating output directory" path=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="created output directory"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 files=1
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download start"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download complete" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing protected commandToExecute" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executed command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=enabled
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=end
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
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Volgende stappen
Zie voor de code, de huidige problemen en de versies [aangepaste-script-extensie-linux-opslagplaats](https://github.com/Azure/custom-script-extension-linux).

