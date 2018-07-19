---
title: Veelgestelde vragen voor Azure Service Fabric NET | Microsoft Docs
description: Meer informatie over veelgestelde vragen en antwoorden voor Azure Service Fabric NET.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 06/25/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 07c0347793f6541a3e047f3f357d0d1b05dc3bca
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136178"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Veelgestelde vragen over Service Fabric NET
Azure Service Fabric NET is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen implementeren zonder beheer van virtuele machines, opslag, of netwerken. In dit artikel vindt u antwoorden op veelgestelde vragen.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Hoe ik een probleem melden of een vraag stellen?

Stel vragen, krijg antwoorden van Microsoft-technici en rapporteren van problemen in de [GitHub-opslagplaats voor service-fabric-mesh-preview](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Quota en kosten

**Wat zijn de kosten die deel uitmaken van de Preview-versie?**

Er zijn geen kosten voor het implementeren van toepassingen of containers naar Mesh-preview. Maar u wordt aangeraden om te verwijderen van de resources die u implementeert en niet laten uitgevoerd, tenzij u actief wilt testen.

**Is er een limiet van het aantal cores en RAM-geheugen?**

Ja, de quota voor elk abonnement zijn:

- Aantal toepassingen - 5 
- Aantal kernen per toepassing tot en met 12 
- Totale RAM-geheugen per toepassing - 48 GB 
- Aantal eindpunten netwerk en binnenkomende: 5  
- Aantal Azure-Volumes die u koppelen kunt - 10 
- Aantal replica's Service – 3 
- De grootste container die u kunt implementeren, is beperkt tot 4 kernen, 16 GB RAM-geheugen.
- U kunt gedeeltelijke kerngeheugens toewijzen aan uw containers in stappen van 0,5 kernen maximaal 6 kernen.

**Kan ik mijn toepassing die 's nachts wordt uitgevoerd verlaten?**

Ja, kunt u, maar u wordt aangeraden de resources die u implementeert en niet laten verwijderen wordt uitgevoerd, tenzij u actief wilt testen. Dit beleid in de toekomst kan worden gewijzigd en de resources kunnen worden verwijderd als ze zijn worden misbruikt.

## <a name="supported-container-os-images"></a>Ondersteunde OS containerinstallatiekopieën
De volgende container OS-installatiekopieën kunnen worden gebruikt bij het implementeren van services.

- Windows - windowsservercore en nanoserver
    - Windows Server 2016
    - Windows Server versie 1709
- Linux
    - Er zijn geen bekende beperkingen

## <a name="features-gaps-and-known-issues"></a>Functies hiaten en bekende problemen

**Na de implementatie van mijn toepassing zich de netwerk-resource die is gekoppeld aan deze niet op een IP-adres**

Er is een bekend probleem vandaag nog met het IP-adres na een vertraging te komen. Controleer de status van de netwerk-resource in een paar minuten om te zien van het bijbehorende IP-adres.

**Mijn toepassing is te krijgen tot de juiste netwerk of het volume-resource mislukt**

In het toepassingsmodel van uw moet u de volledige resource-ID voor netwerken en volumes gebruiken om toegang tot de gekoppelde resource te kunnen. Hier ziet u hoe dit eruitziet in de quickstart-voorbeeld:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

**Ik zie niet de huidige toepassingsmodel ondersteunen een manier voor het versleutelen van moje tajné kódy**

Ja, geheimen versleutelen wordt niet ondersteund in de huidige Private Preview. 

**DNS is niet op dezelfde manier werken in mijn Service Fabric-cluster voor ontwikkeling en Mesh**

Er is een bekend probleem waarbij u mogelijk om te verwijzen naar services anders in uw lokale ontwikkelcluster en Azure NET. Gebruik in uw lokale ontwikkelcluster {serviceName}. {applicationName}. Gebruik in Azure Service Fabric NET {servicename}. Azure NET ondersteunt momenteel geen dns-omzetting voor toepassingen.

Voor andere bekende DNS-problemen bij het uitvoeren van een Service Fabric-cluster voor ontwikkeling in Windows 10, hier te zien: [fouten opsporen in Windows containers](/azure/service-fabric/service-fabric-how-to-debug-windows-containers).

**Er verschijnt deze fout bij het gebruik van de CLI-module _ImportError: de naam 'sdk_no_wait' kan niet importeren**

Als u oudere CLI-versie dan 2.0.30, krijgt u mogelijk deze fout:

```
cannot import name 'sdk_no_wait'
Traceback (most recent call last):
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\cli.py", line 193, in invoke cmd_result = self.invocation.execute(args)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 241, in execute self.commands_loader.load_arguments(command)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 201, in load_arguments self.command_table[command].load_arguments() # this loads the arguments via reflection
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 142, in load_arguments super(AzCliCommand, self).load_arguments()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\commands.py", line 76, in load_arguments cmd_args = self.arguments_loader()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 332, in default_arguments_loader op = handler or self.get_op_handler(operation)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 375, in get_op_handler op = import_module(mod_to_import)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\importlib_init_.py", line 126, in import_module return _bootstrap._gcd_import(name[level:], package, level)
File "", line 978, in _gcd_import
File "", line 961, in _find_and_load
File "", line 950, in _find_and_load_unlocked
File "", line 655, in _load_unlocked
File "", line 678, in exec_module
File "", line 205, in _call_with_frames_removed
File "C:\Users\annayak.azure\cliextensions\azure-cli-sbz\azext_sbz\custom.py", line 18, in 
from azure.cli.core.util import get_file_json, shell_safe_json_parse, sdk_no_wait
ImportError: cannot import name 'sdk_no_wait'.
```

**Er verschijnt een fout met niet overeenkomende distributie naam bij het installeren van de CLI-extensie-pakket**

Dit betekent niet dat de extensie niet zijn geïnstalleerd. U moet nog steeds kunnen de CLI-opdrachten zonder problemen gebruiken.

**Wanneer ik de schaal vergroten, zie ik dat alle mijn containers zijn betrokken, inclusief Mijn actieve**

Dit is een fout en moet worden vastgesteld in de volgende vernieuwing van de runtime.

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over Service Fabric NET de [overzicht](service-fabric-mesh-overview.md).
