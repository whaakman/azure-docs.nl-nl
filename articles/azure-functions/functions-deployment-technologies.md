---
title: Implementatietechnologieën dat in Azure Functions | Microsoft Docs
description: Meer informatie over de verschillende manieren waarop u code kunt implementeren op Azure Functions.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 47d8bf33fd686942326db3b1cc606978bf47a1bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594396"
---
# <a name="deployment-technologies-in-azure-functions"></a>Implementatietechnologieën dat in Azure Functions

U kunt een aantal verschillende technologieën gebruiken voor het implementeren van de code van uw Azure Functions-project naar Azure. In dit artikel biedt een volledige lijst van deze technologieën, wordt beschreven welke technologieën zijn beschikbaar voor welke versies van functies, wordt uitgelegd wat er gebeurt wanneer u elke methode en bevat aanbevelingen voor de beste methode te gebruiken in verschillende scenario 's . De verschillende hulpprogramma's die ondersteuning bieden voor implementeren naar Azure Functions zijn afgestemd op de juiste technologie op basis van hun context.

## <a name="deployment-technology-availability"></a>Beschikbaarheid van de implementatie-technologie

> [!IMPORTANT]
> Azure Functions biedt ondersteuning voor meerdere platformen lokale ontwikkeling en hosting op Windows en Linux. Er zijn momenteel drie hostingabonnementen beschikbaar: [Verbruik](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan), en [(Azure App Service) toegewezen](functions-scale.md#app-service-plan). Elk abonnement vertoont verschillend gedrag. Niet alle technologieën voor desktopimplementatie zijn beschikbaar voor elke versie van Azure Functions.

| Implementatietechnologie | Windows-verbruik | Windows Premium (preview) | Windows toegewezen  | Linux-verbruik (preview) | Linux toegewezen |
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
<sup>2</sup> portal bewerken is alleen ingeschakeld voor HTTP en Timer triggers voor Functions op Linux met behulp van het specifieke abonnement.

## <a name="key-concepts"></a>Belangrijkste concepten

Enkele belangrijke concepten zijn essentieel voor informatie over de werking van implementaties in Azure Functions.

### <a name="trigger-syncing"></a>Trigger synchroniseren

Wanneer u een van de triggers wijzigt, moet de infrastructuur functies op de hoogte van de wijzigingen zijn. Synchronisatie gebeurt automatisch voor veel technologieën voor desktopimplementatie. Echter, in sommige gevallen, u moet handmatig synchroniseren uw triggers. Wanneer u uw wijzigingen door te verwijzen naar een extern pakket-URL, lokale Git, cloud-synchronisatie of FTP implementeert, moet u uw triggers handmatig synchroniseren. U kunt triggers op drie manieren synchroniseren:

* Opnieuw opstarten van uw functie-app in Azure portal
* Verzenden van een HTTP POST-aanvraag naar `https://{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` met behulp van de [hoofdsleutel](functions-bindings-http-webhook.md#authorization-keys).
* Verzenden van een HTTP POST-aanvraag naar `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`. Vervang de tijdelijke aanduidingen door uw abonnements-ID, naam van de resourcegroep en de naam van uw functie-app.

## <a name="deployment-technology-details"></a>Details van de implementatie-technologie 

De volgende implementatiemethoden zijn beschikbaar in Azure Functions.

### <a name="external-package-url"></a>Pakket met externe URL

U kunt de URL van een extern pakket gebruiken om te verwijzen naar een extern pakket (.zip)-bestand met de functie-app. Het bestand wordt gedownload van de opgegeven URL en de app wordt uitgevoerd [uitvoeren van pakket](run-functions-from-deployment-package.md) modus.

>__Het gebruik ervan:__ Voeg `WEBSITE_RUN_FROM_PACKAGE` in uw toepassingsinstellingen. De waarde van deze instelling moet een URL (de locatie van het specifieke pakketbestand dat uit te voeren). U kunt instellingen toevoegen beide [in de portal](functions-how-to-use-azure-function-app-settings.md#settings) of [met behulp van de Azure CLI](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). 
>
>Als u Azure Blob-opslag gebruikt, gebruikt u een privé-container met een [handtekening voor gedeelde toegang (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) functies toegang geven tot het pakket. Telkens wanneer u de toepassing opnieuw wordt opgestart, wordt een kopie van de inhoud ophaalt. Referentie moet voor de levensduur van de toepassing geldig zijn.

>__Wanneer te gebruiken:__ Extern pakket-URL is de enige ondersteunde implementatiemethode voor Azure Functions op Linux wordt uitgevoerd in het plan verbruik (Preview). Wanneer u het pakketbestand dat een functie-app wordt verwezen naar bijwerkt, moet u [handmatig synchroniseren triggers](#trigger-syncing) Azure vertellen dat uw toepassing is gewijzigd.

### <a name="zip-deploy"></a>ZIP implementeren

Gebruik zip implementeren als u wilt een ZIP-bestand met de functie-app naar Azure te pushen. (Optioneel) u kunt instellen dat uw app te starten in [uitvoeren van pakket](run-functions-from-deployment-package.md) modus.

>__Het gebruik ervan:__ Implementeren met behulp van uw favoriete clienthulpprogramma: [VS Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure), of de [Azure CLI](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure). Als u handmatig een ZIP-bestand op uw functie-app implementeert, wilt u de instructies in [implementeren vanuit een ZIP-bestand of URL](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url).
>
>Wanneer u implementeert met behulp van zip implementeert, kunt u instellen dat uw app wordt uitgevoerd [uitvoeren van pakket](run-functions-from-deployment-package.md) modus. Om in te stellen uitvoeren van pakket-modus, de `WEBSITE_RUN_FROM_PACKAGE` toepassing de waarde van de instelling voor `1`. Het wordt aangeraden de zip-implementatie. Dit resulteert in sneller laden tijden voor uw toepassingen en is de standaardinstelling voor VS Code, Visual Studio en de Azure CLI.

>__Wanneer te gebruiken:__ ZIP implementeren is het aanbevolen implementatie-technologie voor Azure Functions uitgevoerd op Windows en voor Azure Functions op Linux wordt uitgevoerd in de toegewezen planning.

### <a name="docker-container"></a>Docker-container

U kunt een installatiekopie van Linux-container waarin uw functie-app implementeren.

>__Het gebruik ervan:__ Een Linux-functie-app maken in de toegewezen planning en welke containerinstallatiekopie uitvoeren vanuit opgeven. U kunt dit op twee manieren doen:
>
>* Een Linux-functie-app maken op een Azure App Service-plan in de Azure-portal. Voor **publiceren**, selecteer **Docker-installatiekopie**, en configureer vervolgens de container. Geef de locatie waar de installatiekopie wordt gehost.
>* Een Linux-functie-app op een App Service-plan maken met behulp van de Azure CLI. Voor meer informatie Zie [een functie in Linux maken met behulp van een aangepaste installatiekopie](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).
>
>Om te implementeren op een bestaande app met behulp van een aangepaste container in [Azure Functions Core Tools](functions-run-local.md), gebruikt u de [ `func deploy` ](functions-run-local.md#publish) opdracht.

>__Wanneer te gebruiken:__ Gebruik de Docker-container-optie als u meer controle over de Linux-omgeving waarin uw functie-app wordt uitgevoerd. Dit implementatiemechanisme is alleen beschikbaar voor functies op Linux in een App Service-plan.

### <a name="web-deploy-msdeploy"></a>Web Deploy (MSDeploy)

Web Deploy-pakketten maken en implementeren van uw Windows-toepassingen op een IIS-server, met inbegrip van uw functie-apps die worden uitgevoerd op Windows in Azure.

>__Het gebruik ervan:__ Gebruik [Visual Studio-hulpprogramma's voor Azure Functions](functions-create-your-first-function-visual-studio.md). Schakel de **uitvoeren vanaf het pakketbestand (aanbevolen)** selectievakje.
>
>U kunt ook downloaden [Web implementeren 3.6](https://www.iis.net/downloads/microsoft/web-deploy) en roep `MSDeploy.exe` rechtstreeks.

>__Wanneer te gebruiken:__ Web Deploy wordt ondersteund en heeft geen problemen, maar de beste manier is [zip implementeren met het uitvoeren van pakket ingeschakeld](#zip-deploy). Zie voor meer informatie, de [handleiding voor het ontwikkelen van Visual Studio](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Broncodebeheer

Gebruik bronbeheer uw functie-app verbinden met een Git-opslagplaats. Een update van code in die opslagplaats wordt geactiveerd voor implementatie. Zie voor meer informatie de [Kudu Wiki](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

>__Het gebruik ervan:__ Implementatiecenter in de portal voor Azure Functions gebruiken voor het instellen van publiceren vanuit broncodebeheer. Zie voor meer informatie, [continue implementatie voor Azure Functions](functions-continuous-deployment.md).

>__Wanneer te gebruiken:__ Met behulp van bronbeheer is de aanbevolen procedure voor teams die aan de functie-apps samenwerken. Broncodebeheer is een goede Implementatieoptie waarmee u meer geavanceerde implementatie pijplijnen.

### <a name="local-git"></a>Lokale Git

U kunt lokale Git push-code van uw lokale computer naar Azure Functions gebruiken met behulp van Git.

>__Het gebruik ervan:__ Volg de instructies in [lokale Git-implementatie in Azure App Service](../app-service/deploy-local-git.md).

>__Wanneer te gebruiken:__ In het algemeen raden wij aan dat u een andere implementatiemethode. Wanneer u vanuit lokale Git publiceren, moet u [handmatig synchroniseren triggers](#trigger-syncing).

### <a name="cloud-sync"></a>Cloudsynchronisatie

Gebruik cloud synchronisatie-synchronisatie de inhoud van Dropbox en OneDrive naar Azure Functions.

>__Het gebruik ervan:__ Volg de instructies in [Synchroniseer inhoud van een map in de cloud](../app-service/deploy-content-sync.md).

>__Wanneer te gebruiken:__ In het algemeen raden we aan andere methoden voor het implementeren. Wanneer u met behulp van cloud-synchronisatie publiceert, moet u [handmatig synchroniseren triggers](#trigger-syncing).

### <a name="ftp"></a>FTP

Rechtstreeks bestanden overbrengt naar Azure Functions kunt u FTP.

>__Het gebruik ervan:__ Volg de instructies in [inhoud implementeren met behulp van FTP/s](../app-service/deploy-ftp.md).

>__Wanneer te gebruiken:__ In het algemeen raden we aan andere methoden voor het implementeren. Wanneer u met behulp van FTP publiceert, moet u [handmatig synchroniseren triggers](#trigger-syncing).

### <a name="portal-editing"></a>Portal bewerken

U kunt de bestanden die zich in uw functie-app (in feite implementeren telkens wanneer u uw wijzigingen opslaan) rechtstreeks bewerken in de portal-gebaseerd-editor.

>__Het gebruik ervan:__ Als u uw functies in Azure portal bewerken, moet u hebben [gemaakt van uw functies in de portal](functions-create-first-azure-function.md). Als u wilt behouden een één betrouwbare bron, met een andere implementatiemethode van uw functie is alleen-lezen en voorkomt u dat blijven portal bewerken. Als u wilt terugkeren naar een status waarin u uw bestanden in de Azure-portal kunt bewerken, kunt u handmatig de bewerkingsmodus inschakelen terug naar `Read/Write` en verwijderen van een met betrekking tot implementatie-toepassingsinstellingen (zoals `WEBSITE_RUN_FROM_PACKAGE`). 

>__Wanneer te gebruiken:__ De portal is een goede manier om aan de slag met Azure Functions. U wordt aangeraden dat u de client die hulpprogramma's gebruiken voor intenser ontwikkelwerkzaamheden werken:
>
>* [Aan de slag met VS Code](functions-create-first-function-vs-code.md)
>* [Aan de slag met Azure Functions Core Tools](functions-run-local.md)
>* [Aan de slag met Visual Studio](functions-create-your-first-function-visual-studio.md)

De volgende tabel toont de besturingssystemen en talen die ondersteuning portal bewerken:

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

<sup>*</sup> Portal bewerken is alleen ingeschakeld voor HTTP- en Timer triggers voor Functions op Linux met behulp van het specifieke abonnement.

## <a name="deployment-slots"></a>Implementatiesites

Wanneer u uw functie-app in Azure implementeert, kunt u implementeren op een afzonderlijke implementatiesite in plaats van rechtstreeks in productie wilt nemen. Zie voor meer informatie over implementatiesites [Azure App Service-sleuven](../app-service/deploy-staging-slots.md).

### <a name="deployment-slots-levels-of-support"></a>Implementatiesleuven niveaus van ondersteuning

Er zijn twee niveaus van ondersteuning voor implementatiesites:

* **Algemene beschikbaarheid (GA)** : Volledig ondersteunde en goedgekeurd voor gebruik in productieomgevingen.
* **Preview-versie**: Nog niet ondersteund, maar wordt verwacht dat de status van de algemene beschikbaarheid in de toekomst te bereiken.

| Plan OS/die als host fungeert | Ondersteuningsniveau |
| --------------- | ------ |
| Windows-verbruik | Preview |
| Windows Premium (Preview) | Preview |
| Windows toegewezen | Algemene beschikbaarheid |
| Linux-verbruik | Niet ondersteund |
| Linux toegewezen | Algemene beschikbaarheid |

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie over het implementeren van uw functie-apps: 

+ [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)
+ [Continue levering met behulp van Azure DevOps](functions-how-to-azure-devops.md)
+ [ZIP-implementaties voor Azure Functions](deployment-zip-push.md)
+ [Uw Azure Functions uitvoeren vanuit een pakketbestand](run-functions-from-deployment-package.md)
+ [Automatiseer de implementatie van de resource voor uw functie-app in Azure Functions](functions-infrastructure-as-code.md)
