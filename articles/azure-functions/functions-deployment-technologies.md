---
title: Implementatietechnologieën dat in Azure Functions | Microsoft Docs
description: Meer informatie over de ins en outs van de verschillende manieren waarop die u code op Azure Functions implementeren kunt.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 10976c9cf16dfab4c31d0d77c519dc3277204a51
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67293046"
---
# <a name="deployment-technologies-in-azure-functions"></a>Implementatietechnologieën dat in Azure Functions

Er zijn een paar verschillende technologieën die u gebruiken kunt voor het implementeren van de code van uw Azure Functions-project naar Azure. In dit artikel biedt een volledige lijst van deze technologieën, geeft aan welke technologieën zijn beschikbaar voor welke versies van functies, wordt uitgelegd wat er werkelijk gebeurt wanneer elke methode wordt gebruikt, en aanbevelingen voor de beste methode bevat voor gebruik in verschillende scenario's. De verschillende hulpprogramma's die ondersteuning bieden voor implementeren naar Azure Functions zijn afgestemd op de juiste technologie op basis van hun context.

## <a name="deployment-technology-availability"></a>Beschikbaarheid van de implementatie-technologie

> [!IMPORTANT]
> Azure Functions ondersteunt cross-platform lokale ontwikkeling en hosting op twee besturingssystemen: Windows en Linux. Er zijn drie hostingabonnementen momenteel beschikbaar is, elk met verschillende gedrag - [verbruik](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan), en [(App Service) toegewezen](functions-scale.md#app-service-plan). Niet alle technologieën voor desktopimplementatie zijn beschikbaar voor elke versie van Azure Functions.

| Implementatietechnologie | Windows-verbruik | Windows-premium (preview) | Windows toegewezen  | Linux-verbruik (preview) | Linux toegewezen |
|-----------------------|:-------------------:|:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| URL van de externe pakket<sup>1</sup> |✔|✔|✔|✔|✔|
| ZIP implementeren |✔|✔|✔| |✔|
| Docker-container | | | | |✔|
| Web Deploy |✔|✔|✔| | |
| Broncodebeheer |✔|✔|✔| |✔|
| Lokale Git<sup>1</sup> |✔|✔|✔| |✔|
| Synchronisatie in de cloud<sup>1</sup> |✔|✔|✔| |✔|
| FTP<sup>1</sup> |✔|✔|✔| |✔|
| Portal bewerken |✔|✔|✔| |✔<sup>2</sup>|

<sup>1</sup> implementatietechnologie waarvoor [handmatige trigger synchroniseren](#trigger-syncing).
<sup>2</sup> portal bewerken is ingeschakeld voor alleen HTTP- en Timer triggers voor Functions op Linux met behulp van de toegewezen plannen.

## <a name="key-concepts"></a>Belangrijkste concepten

Voordat u doorgaat, is het belangrijk om te leren van enkele belangrijke concepten die essentieel zijn voor informatie over de werking van implementaties in Azure Functions.

### <a name="trigger-syncing"></a>Trigger synchroniseren

Wanneer u een van de triggers wijzigt, moet de infrastructuur functies op de hoogte van deze wijzigingen. Deze synchronisatie gebeurt automatisch voor veel technologieën voor desktopimplementatie. Echter, in sommige gevallen moet u handmatig synchroniseren uw triggers. Wanneer u uw met behulp van een extern pakket-URL, lokale Git, cloud-synchronisatie of FTP-updates implementeert, moet u ervoor dat u handmatig synchroniseren uw triggers zijn. U kunt triggers op drie manieren synchroniseren:

* Opnieuw opstarten van uw functie-app in Azure portal
* Verzenden van een HTTP POST-aanvraag naar `https://www.{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` met behulp van de [hoofdsleutel](functions-bindings-http-webhook.md#authorization-keys).
* Verzenden van een HTTP POST-aanvraag naar `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`. Vervang de tijdelijke aanduidingen door uw abonnements-ID, naam van de resourcegroep en de naam van uw functie-app.

## <a name="deployment-technology-details"></a>Details van de implementatie-technologie  

### <a name="external-package-url"></a>Pakket met externe URL

Hiermee kunt u verwijzen naar een extern pakket (.zip)-bestand met de functie-app. Het bestand wordt gedownload van de opgegeven URL en de app wordt uitgevoerd [uitvoeren-van-Package](run-functions-from-deployment-package.md) modus.

>__Het gebruik ervan:__ Voeg `WEBSITE_RUN_FROM_PACKAGE` in uw toepassingsinstellingen. De waarde van deze instelling moet een URL - de locatie van het specifieke pakketbestand dat u wilt uitvoeren. U kunt instellingen toevoegen beide [in de portal](functions-how-to-use-azure-function-app-settings.md#settings) of [met behulp van de Azure CLI](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). Als u Azure blob-opslag gebruikt, moet u een privé-container met een [Shared Access Signature (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-a-storage-account-by-using-a-shared-access-signature-sas) functies toegang geven tot het pakket. Op elk gewenst moment de toepassing opnieuw wordt opgestart deze worden opgehaald een kopie van de inhoud, wat betekent dat de verwijzing geldig voor de levensduur van de toepassing zijn moet.

>__Wanneer te gebruiken:__ Dit is de implementatie met alleen-methode ondersteund voor Azure Functions op Linux wordt uitgevoerd in het plan verbruik (Preview). Tijdens het bijwerken van het pakketbestand dat verwijst naar een functie-app, moet u [handmatig synchroniseren triggers](#trigger-syncing) Azure vertellen dat uw toepassing is gewijzigd.

### <a name="zip-deploy"></a>ZIP implementeren

Hiermee kunt u een zip-bestand met de functie-app naar Azure te pushen. (Optioneel) u kunt ook opgeven als u wilt dat uw app te starten in [uitvoeren-van-Package](run-functions-from-deployment-package.md) modus.

>__Het gebruik ervan:__ Implementeren met behulp van uw favoriete clienthulpprogramma voor het - [VS Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure), of de [Azure CLI](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure). Volg de instructies op voor het handmatig implementeren een zip-bestand naar uw functie-app [implementeren vanuit een zip-bestand of url](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url).
>
>Bovendien wanneer implementeren via zip implementeert, kunnen gebruikers opgeven voor het uitvoeren van de app uit in [uitvoeren-van-Package](run-functions-from-deployment-package.md) modus door in te stellen de `WEBSITE_RUN_FROM_PACKAGE` instellingswaarde van toepassing als `1`. Deze optie wordt aangeraden en levert sneller laden tijden voor uw toepassingen. Dit wordt gedaan door de standaardwaarde voor de bovenstaande clienthulpprogramma's.

>__Wanneer te gebruiken:__ Dit is de aanbevolen implementatie-technologie voor Azure Functions uitgevoerd op Windows en Azure Functions op Linux wordt uitgevoerd in de toegewezen planning.

### <a name="docker-container"></a>Docker-container

Een installatiekopie van Linux-container waarin uw functie-app implementeren.

>__Het gebruik ervan:__ Een Linux-functie-app maken in de toegewezen planning en opgeven welke containerinstallatiekopie uitvoeren vanaf. U kunt dit op twee manieren doen:
>
>* Een Linux-functie-app maken op een App Service-plan in de Azure-portal. Selecteer **Docker-installatiekopie** voor **publiceren**, en configureert u de container, zodat de locatie waar de installatiekopie wordt gehost.
>* Een Linux-functie-app maken op een App Service-abonnement via de Azure CLI. Leer hoe u aan de hand [een functie in Linux maken met een aangepaste installatiekopie](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).
>
>Als u wilt implementeren op een bestaande app met behulp van een aangepaste container, gebruikt u de [ `func deploy` ](functions-run-local.md#publish) opdracht van de [Azure Functions Core Tools](functions-run-local.md).

>__Wanneer te gebruiken:__ Gebruik deze optie als u meer controle over de Linux-omgeving waarin uw functie-app wordt uitgevoerd. Dit implementatiemechanisme is alleen beschikbaar voor functies op Linux in een App Service-plan.

### <a name="web-deploy-msdeploy"></a>Web implementeren (MSDeploy)

Pakketten maken en implementeren van uw Windows-toepassingen op een IIS-server, met inbegrip van uw Azure-functie-apps die worden uitgevoerd op Windows.

>__Het gebruik ervan:__ Gebruik de [Visual Studio-hulpprogramma's voor Azure Functions](functions-create-your-first-function-visual-studio.md), en maatstreepjes niet de `Run from package file (recommended)` selectievakje.
>
>U kunt ook aanroepen `MSDeploy.exe` direct na het downloaden van [Web implementeren 3.6](https://www.iis.net/downloads/microsoft/web-deploy).

>__Wanneer te gebruiken:__ Deze implementatietechnologie wordt ondersteund en heeft geen problemen, maar de beste manier is nu [Zip implementeren met het uitvoeren van pakket ingeschakeld](#zip-deploy). Voor meer informatie gaat u naar de [handleiding voor het ontwikkelen van Visual Studio](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Broncodebeheer

Hiermee kunt u uw functie-app verbinden met een git-opslagplaats, zodat eventuele updates aan code in die opslagplaats implementatie wordt geactiveerd. Voor meer informatie, bekijk de [Kudu Wiki](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

>__Het gebruik ervan:__ De Center-implementatie in de Azure Functions-portal gebruiken voor het instellen van publiceren vanuit broncodebeheer. Zie voor meer informatie, [continue implementatie voor Azure Functions](functions-continuous-deployment.md).

>__Wanneer te gebruiken:__ De aanbevolen procedure voor teams die samenwerken aan de functie-apps met behulp van bronbeheer is en dit is een geweldige optie waarmee u meer geavanceerde implementatie pijplijnen.

### <a name="local-git"></a>Lokale git

Hiermee kunt u code van uw lokale computer pushen naar Azure Functions met behulp van Git.

>__Het gebruik ervan:__ Volg de instructies op [lokale Git-implementatie in Azure App Service](../app-service/deploy-local-git.md).

>__Wanneer te gebruiken:__ Andere methoden voor het implementeren, worden in het algemeen aanbevolen. Bij het publiceren van lokale git, moet u [handmatig synchroniseren triggers](#trigger-syncing).

### <a name="cloud-sync"></a>Cloudsynchronisatie

Hiermee kunt u de inhoud van Dropbox en OneDrive naar Azure Functions synchroniseren.

>__Het gebruik ervan:__ Volg de instructies in [Synchroniseer inhoud van een map in de cloud](../app-service/deploy-content-sync.md).

>__Wanneer te gebruiken:__ Andere methoden voor het implementeren, worden in het algemeen aanbevolen. Bij het publiceren van met het synchroniseren van de cloud, moet u [handmatig synchroniseren triggers](#trigger-syncing).

### <a name="ftp"></a>FTP

Kunt u rechtstreeks en draagt bestanden asynchroon naar Azure Functions.

>__Het gebruik ervan:__ Volg de instructies in [implementeren van inhoud met behulp van FTP/s](../app-service/deploy-ftp.md).

>__Wanneer te gebruiken:__ Andere methoden voor het implementeren, worden in het algemeen aanbevolen. Bij het publiceren met FTP, moet u [handmatig synchroniseren triggers](#trigger-syncing).

### <a name="portal-editing"></a>Portal bewerken

Met behulp van de editor op basis van een portal, kunt u rechtstreeks bewerken bestanden op uw functie-app (in feite implementeren telkens wanneer u klikt op **opslaan**).

>__Het gebruik ervan:__ Als u uw functies in Azure portal bewerken, moet u hebben [gemaakt van uw functies in de portal](functions-create-first-azure-function.md). Met behulp van een andere implementatiemethode, kunt u de functie alleen-lezen en wordt voorkomen dat verdere portal bewerken, een één betrouwbare bron behouden. Als u wilt terugkeren naar een status waarin u uw bestanden via Azure portal kunt bewerken, kunt u handmatig de bewerkingsmodus inschakelen terug naar `Read/Write` en verwijderen van een met betrekking tot implementatie-toepassingsinstellingen (zoals `WEBSITE_RUN_FROM_PACKAGE`). 

>__Wanneer te gebruiken:__ De portal is een uitstekende manier om aan de slag met Azure Functions, maar voor alle intenser projecten met behulp van de client Tools wordt aanbevolen:
>
>* [Aan de slag met VS Code](functions-create-first-function-vs-code.md)
>* [Aan de slag met Azure Functions Core Tools](functions-run-local.md)
>* [Aan de slag met Visual Studio](functions-create-your-first-function-visual-studio.md)

De volgende tabel ziet u de besturingssystemen en de talen waarvoor de portal bewerken wordt ondersteund:

| | Windows-verbruik | Windows Premium (Preview) | Windows toegewezen | Linux-verbruik (Preview) | Linux toegewezen |
|-|:-----------------: |:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| C# | | | | | |
| C# Script |✔|✔|✔| |✔<sup>*</sup>|
| F# | | | | | |
| Java | | | | | |
| JavaScript (Node.js) |✔|✔|✔| |✔<sup>*</sup>|
| Python (Preview) | | | | | |
| PowerShell (Preview) |✔|✔|✔| | |
| TypeScript (Node.js) | | | | | |

<sup>*</sup> Portal bewerken is ingeschakeld voor alleen HTTP- en Timer triggers voor Functions op Linux met behulp van de toegewezen plannen.

## <a name="deployment-slots"></a>Implementatiesites

Wanneer u uw functie-app in Azure implementeert, kunt u implementeren op een afzonderlijke implementatiesite in plaats van rechtstreeks naar productie. Zie voor meer informatie over implementatiesites [de documentatie van Azure App Service-sleuven](../app-service/deploy-staging-slots.md).

### <a name="deployment-slots-levels-of-support"></a>Implementatiesleuven niveaus van ondersteuning

Er zijn twee niveaus van ondersteuning:

* _Algemeen beschikbaar (GA)_ : volledig ondersteund en goedgekeurd voor gebruik in productieomgevingen.
* _Preview-versie_ - nog niet ondersteund, maar wordt verwacht dat de status van de algemene beschikbaarheid in de toekomst te bereiken.

| Plan OS/die als host fungeert | Ondersteuningsniveau |
| --------------- | ------ |
| Windows-verbruik | Preview |
| Windows Premium (Preview) | Preview |
| Windows toegewezen | Algemeen beschikbaar |
| Linux-verbruik | Niet-ondersteund |
| Linux toegewezen | Algemeen beschikbaar |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het implementeren van uw functie-apps in de volgende artikelen: 

+ [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)
+ [Continue levering met behulp van Azure DevOps](functions-how-to-azure-devops.md)
+ [ZIP-implementaties voor Azure Functions](deployment-zip-push.md)
+ [Uw Azure Functions uitvoeren vanuit een pakketbestand](run-functions-from-deployment-package.md)
+ [Automatiseer de implementatie van de resource voor uw functie-app in Azure Functions](functions-infrastructure-as-code.md)
