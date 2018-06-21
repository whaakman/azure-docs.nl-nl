---
title: Aangepaste scripts uitvoeren op virtuele Linux-machines in Azure | Microsoft Docs
description: Configuratietaken voor Linux-VM automatiseren met behulp van de aangepaste Scriptextensie v2
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
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
ms.author: danis
ms.openlocfilehash: e614c78ca4e8ed7afbce0d3f2423ce137c5225b5
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294972"
---
# <a name="use-the-azure-custom-script-extension-version-2-with-linux-virtual-machines"></a>Gebruik Azure aangepast Script extensie versie 2 met virtuele Linux-machines
Aangepast Script extensie versie 2 downloads en scripts uitvoeren op virtuele machines in Azure. Deze uitbreiding is nuttig voor na de implementatie-configuratie, software-installatie of een andere Configuratiebeheer /-taak. U kunt scripts downloaden van Azure Storage of een andere internetlocatie voor toegankelijk of u kunt ze met de extensie runtime opgeven. 

De aangepaste Scriptextensie integreert met Azure Resource Manager-sjablonen. U kunt deze ook uitvoeren met behulp van Azure CLI, PowerShell, de Azure-portal of de REST-API van Azure virtuele Machines.

Dit artikel wordt uitgelegd hoe u de aangepaste Scriptextensie van Azure CLI, en het uitvoeren van de uitbreiding met behulp van een Azure Resource Manager-sjabloon. In dit artikel bevat ook stappen voor probleemoplossing voor Linux-systemen.


Er zijn twee Linux aangepast scriptextensies:
* Versie 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Versie 2 - Microsoft.Azure.Extensions.CustomScript

Schakel over nieuwe en bestaande implementaties voor het gebruik van de nieuwe versie 2 in plaats daarvan. De nieuwe versie is bedoeld als een vervangende onverwachte. Daarom de migratie is net zo eenvoudig als het wijzigen van de naam en versie, u hoeft niet te wijzigen van uw configuratie voor de uitbreiding.


### <a name="operating-system"></a>Besturingssysteem

De aangepaste Scriptextensie voor Linux wordt uitgevoerd op de uitbreiding met de extensie ondersteund besturingssysteem van voor meer informatie ziet u dit [artikel](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

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
      "timestamp":"123456789",          
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

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld | Gegevenstype | 
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.Compute.Extensions | tekenreeks |
| type | CustomScript | tekenreeks |
| typeHandlerVersion | 2.0 | int |
| fileUris (bijvoorbeeld) | https://github.com/MyProject/Archive/MyPythonScript.py | matrix |
| commandToExecute (bijvoorbeeld) | Python MyPythonScript.py < Mijn param1 > | tekenreeks |
| Script | IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo = | tekenreeks |
| skipDos2Unix (bijvoorbeeld) | false | booleaans |
| tijdstempel (bijvoorbeeld) | 123456789 | 32-bits geheel getal |
| storageAccountName (bijvoorbeeld) | examplestorageacct | tekenreeks |
| storageAccountKey (bijvoorbeeld) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | tekenreeks |

### <a name="property-value-details"></a>Details van de eigenschap
* `skipDos2Unix`: (optioneel, boolean) overslaan dos2unix conversie van de URL van een bestand op basis van een script of script.
* `timestamp` Dit veld is alleen voor het activeren van een opnieuw uitvoeren van het script door de waarde van dit veld te wijzigen (optioneel, 32-bits geheel getal) te gebruiken.  Een geheel getal is toegestaan. Deze moet alleen worden anders is dan de vorige waarde.
 * `commandToExecute`: (**vereist** als het script niet is ingesteld, string) de vermelding punt script uit te voeren. Dit veld in plaats daarvan gebruiken als uw opdracht geheimen zoals wachtwoorden bevat.
* `script`: (**vereist** als commandToExecute niet is ingesteld, string) een base64-codering (en desgewenst gzip'ed) script dat wordt uitgevoerd door/bin/servicel.
* `fileUris`: (optioneel, string array) de URL's voor bestanden die moeten worden gedownload.
* `storageAccountName`: (optioneel, string) de naam van de storage-account. Als u de storage-referenties opgeeft alle `fileUris` URL's moeten zijn voor Azure Blobs.
* `storageAccountKey`: (optioneel, string) de toegangssleutel van storage-account


De volgende waarden kunnen worden ingesteld in de openbare of beveiligde instellingen, wijst u de uitbreiding wordt een willekeurige configuratie waarbij de onderstaande waarden worden ingesteld in de openbare en beveiligde instellingen af.
* `commandToExecute`
* `script`
* `fileUris`

Met de openbare instellingen mogelijk handig voor foutopsporing, maar het is raadzaam dat u beveiligde instellingen.

Instellingen voor openbare worden verzonden in normale tekst naar de virtuele machine waar het script wordt uitgevoerd.  Beveiligde instellingen zijn versleuteld met een sleutel die alleen bekend is bij de Azure en de virtuele machine. De instellingen worden opgeslagen met de virtuele machine als ze zijn verzonden, dat wil zeggen als de instellingen zijn gecodeerd worden ze opgeslagen versleuteld op de virtuele machine. Het certificaat dat wordt gebruikt voor het ontsleutelen van de versleutelde waarden is opgeslagen op de virtuele machine en gebruikt voor het ontsleutelen van instellingen (indien nodig) tijdens runtime.

#### <a name="property-skipdos2unix"></a>Eigenschap: skipDos2Unix

De standaardwaarde is ONWAAR, wat betekent dat dos2unix conversie **is** uitgevoerd.

De vorige versie van CustomScript, Microsoft.OSTCExtensions.CustomScriptForLinux, zou automatisch DOS-bestanden converteren naar UNIX-bestanden door het vertalen van de `\r\n` naar `\n`. Deze vertaling bestaat en is standaard ingeschakeld. Deze conversie wordt toegepast op alle bestanden die zijn gedownload van fileUris of de scriptinstelling op basis van een van de volgende criteria.

* Als de extensie een van de is `.sh`, `.txt`, `.py`, of `.pl` wordt geconverteerd. De scriptinstelling wordt altijd overeen met deze criteria, omdat deze wordt ervan uitgegaan dat een script dat wordt uitgevoerd met /bin/sh en wordt opgeslagen als script.sh op de virtuele machine.
* Als het bestand met begint `#!`.

De conversie dos2unix worden overgeslagen door de skipDos2Unix in te stellen op true.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>",
  "skipDos2Unix": true
}
```

####  <a name="property-script"></a>Eigenschap: script

CustomScript ondersteunt de uitvoering van een gebruiker gedefinieerde script. De instellingen van het script commandToExecute en fileUris combineren tot één afzonderlijke instelling. In plaats van het instellen van een bestand voor het downloaden van Azure-opslag- of GitHub basisvertalingen, kunt u gewoon het script als een instelling coderen. Script kan worden gebruikt om te vervangen commandToExecute en fileUris.

Het script **moet** base64-gecodeerd zijn.  Het script kunt **eventueel** gzip'ed worden. De scriptinstelling kan worden gebruikt in openbare of beveiligde instellingen. De maximale grootte van gegevens in de scriptparameter is 256 KB. Als het script de grootte van deze die worden niet uitgevoerd overschrijdt.

Voor een voorbeeld gegeven van het volgende script in de /script.sh/ bestand opgeslagen.

```sh
#!/bin/sh
echo "Updating packages ..."
apt update
apt upgrade -y
```

De juiste instelling voor CustomScript script zou worden samengesteld door de uitvoer van de volgende opdracht.

```sh
cat script.sh | base64 -w0
```

```json
{
  "script": "IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo="
}
```

Het script kan eventueel worden gzip'ed verder verkleinen (in de meeste gevallen). (CustomScript auto-detecteert het gebruik van gzip-compressie.)

```sh
cat script | gzip -9 | base64 -w 0
```

CustomScript gebruikt het volgende algoritme voor het uitvoeren van een script.

 1. Assert dat de lengte van de waarde van het script niet langer zijn dan 256 KB.
 1. de waarde van het script decoderen van Base64
 1. _poging_ naar gunzip gedecodeerd de base64 waarde
 1. de waarde gedecodeerde (en desgewenst gedecomprimeerde) schrijven naar schijf (/ var/lib/waagent/custom-script/#/script.sh)
 1. Voer het script met _ / bin/v - c /var/lib/waagent/custom-script/#/script.sh.


## <a name="template-deployment"></a>Sjabloonimplementatie
Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie wordt beschreven, kan de aangepaste Scriptextensie uitvoeren tijdens de sjabloonimplementatie van een Azure Resource Manager-in een Azure Resource Manager-sjabloon worden gebruikt. Een voorbeeldsjabloon met de aangepaste Scriptextensie vindt u hier, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


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
      "fileUris": ["https://github.com/MyProject/Archive/MyPythonScript.py"
      ]  
    }
  }
}
```

>[!NOTE]
>De namen van deze eigenschappen zijn hoofdlettergevoelig. Implementatie om problemen te voorkomen, door de namen te gebruiken, zoals hier wordt weergegeven.

## <a name="azure-cli"></a>Azure-CLI
Wanneer u Azure CLI om uit te voeren van de aangepaste Scriptextensie, maakt u een configuratiebestand of bestanden. U moet ten minste 'commandToExecute' hebben.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings ./script-config.json
```

Desgewenst kunt u de instellingen in de opdracht als een tekenreeks JSON-indeling. Hiermee wordt de configuratie wordt opgegeven tijdens de uitvoering en zonder een afzonderlijk configuratiebestand.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-voorbeelden

#### <a name="public-configuration-with-script-file"></a>De openbare configuratie met een scriptbestand

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
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
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

Als u wilt oplossen, eerst Raadpleeg het logboek voor de Linux-Agent, zorg ervoor dat de uitbreiding hebt uitgevoerd, controleren:

```bash
/var/log/waagent.log 
```

Moet u controleren of de uitvoering van de extensie, deze wordt als volgt uitzien:
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
Enkele punten om aan te geven:
1. Schakel is wanneer de opdracht wordt gestart.
2. Download die is gekoppeld aan het downloaden van het pakket van de extensie CustomScript van Azure, niet de scriptbestanden in fileUris opgegeven.


De extensie van het Azure-Script wordt een logboek, kunt u hier vinden:

```bash
/var/log/azure/custom-script/handler.log
```

Moet u controleren of de uitvoering van de induvidual, deze wordt als volgt uitzien:
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
* Extensie van de bestands- en het resultaat van deze downloaden.
* De volgende opdracht wordt uitgevoerd en het resultaat.

U kunt ook de uitvoeringsstatus van de aangepaste Scriptextensie ophalen met behulp van Azure CLI:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

De uitvoer ziet er de volgende tekst:

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
Zie voor de code, de huidige problemen en de versies [aangepast script-extensie linux opslagplaats](https://github.com/Azure/custom-script-extension-linux).

