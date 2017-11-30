---
title: Richtlijnen voor het ontwikkelen van Azure Functions | Microsoft Docs
description: Informatie over de Azure Functions-concepten en technieken die u nodig hebt voor het ontwikkelen van functies in Azure, over alle programmeertalen en bindingen.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: Developer guide, azure-functies, functies, gebeurtenisverwerking webhooks, dynamische compute, zonder server-architectuur
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/12/2017
ms.author: tdykstra
ms.openlocfilehash: 80996c8bc6e40665201057ed185700ddaeea170a
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="azure-functions-developers-guide"></a>Azure Functions-handleiding voor ontwikkelaars
Specifieke functies delen in Azure Functions enkele core technische concepten en -onderdelen, ongeacht de taal of een binding die u gebruikt. Voordat u in het leren van gegevens die specifiek zijn voor een bepaalde taal of binding gaan, moet Lees dit overzicht die van toepassing op alle mappen.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [overzicht van Azure Functions](functions-overview.md) en bekend bent met [WebJobs SDK concepten zoals triggers en bindingen de runtime JobHost](https://github.com/Azure/azure-webjobs-sdk/wiki). Azure Functions is gebaseerd op de WebJobs SDK. 

## <a name="function-code"></a>Functiecode
Een *functie* is het primaire concept in Azure Functions. U code schrijven voor een functie in een andere taal van uw keuze en slaat de code en configuratie-bestanden in dezelfde map. De configuratie is met de naam `function.json`, die de JSON-configuratiegegevens bevat. Diverse talen worden ondersteund, en elke service heeft een iets andere ervaring die is geoptimaliseerd voor het meest geschikt zijn voor die taal. 

Het bestand function.json definieert de functiebindingen en andere configuratie-instellingen. De runtime maakt gebruik van dit bestand om te bepalen welke gebeurtenissen u wilt bewaken en het doorgeven van gegevens in en gegevens retourneren van een functie wordt uitgevoerd. Hier volgt een voorbeeld function.json-bestand.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Stel de `disabled` eigenschap `true` om te voorkomen dat de functie wordt uitgevoerd.

De `bindings` eigenschap is waar u zowel triggers en bindingen configureren. Elke binding deelt enkele algemene instellingen en sommige instellingen die specifiek voor een bepaald type van de binding zijn. Elke binding moet de volgende instellingen:

| Eigenschap | Waarden/typen | Opmerkingen |
| --- | --- | --- |
| `type` |Tekenreeks |Bindingstype. Bijvoorbeeld `queueTrigger`. |
| `direction` |'in', 'out' |Hiermee wordt aangegeven of de binding voor het ontvangen van gegevens in de functie of het verzenden van gegevens van de functie. |
| `name` |Tekenreeks |De naam die wordt gebruikt voor de afhankelijke gegevens in de functie. Voor C# is dit de naam van een argument; voor JavaScript is de sleutel in een sleutel/waarde-lijst. |

## <a name="function-app"></a>Functie-app
Een functie-app bestaat uit een of meer afzonderlijke functies die samen worden beheerd door Azure App Service. Alle functies in een functie-app delen de dezelfde prijsstelling, continue implementatie en runtime-versie. Functies die zijn geschreven in meerdere talen kunnen alle dezelfde functie app delen. Een functie-app beschouwen als een manier om te organiseren en gezamenlijk beheren van uw functies. 

## <a name="runtime-script-host-and-web-host"></a>Runtime (scripthost en webhost)
De runtime of scripthost, is de onderliggende host voor de WebJobs SDK die luistert naar gebeurtenissen, verzamelt en verzendt gegevens en uiteindelijk uw code wordt uitgevoerd. 

Om te bevorderen HTTP-triggers, is er ook een host die is ontworpen voor de scripthost in productiescenario's zitten. Met twee hosts eindigen helpt te isoleren van de script-host van de voorzijde verkeer die worden beheerd door de webhost.

## <a name="folder-structure"></a>Mapstructuur
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bij het instellen van een project voor het implementeren van functies in een functie-app in Azure App Service, kunt u deze mapstructuur behandelen als de code van uw site. U kunt bestaande hulpprogramma's zoals continue integratie en implementatie of aangepaste implementatie scripts hiervoor pakketinstallatie implementeren of transpilation code.

> [!NOTE]
> Zorg ervoor dat voor het implementeren van uw `host.json` bestanden en mappen rechtstreeks naar werken de `wwwroot` map. Neem geen de `wwwroot` map in uw implementaties. Anders u uiteindelijk `wwwroot\wwwroot` mappen. 
> 
> 

## <a id="fileupdate"></a>Het bijwerken van de functie app-bestanden
De functie editor ingebouwd in de Azure-portal kunt u bijwerken de *function.json* bestand en het codebestand voor een functie. Te uploaden of andere updatebestanden zoals *package.json* of *project.json* of afhankelijkheden, hebt u andere methoden voor het implementeren.

Functie apps zijn gebouwd op App Service, zodat alle de [implementatie-opties die beschikbaar zijn voor een standaard web-apps](../app-service/app-service-deploy-local-git.md) zijn ook beschikbaar voor de functie apps. Hier volgen enkele methoden die u gebruiken kunt om te uploaden of bijwerken van de functie app-bestanden. 

#### <a name="to-use-app-service-editor"></a>App Service-Editor gebruiken
1. Klik in de Azure Functions-portal op **platformfuncties**.
2. In de **ONTWIKKELINGSPROGRAMMA's** sectie, klikt u op **App Service-Editor**.   
   Nadat de App Service-Editor wordt geladen, ziet u de *host.json* bestands- en mappen onder *wwwroot*. 
5. Open bestanden, bewerken of slepen en neerzetten van uw ontwikkelcomputer om bestanden te uploaden.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>De functie-app SCM (Kudu) eindpunt te gebruiken
1. Ga naar: `https://<function_app_name>.scm.azurewebsites.net`.
2. Klik op **Console voor foutopsporing > CMD**.
3. Navigeer naar `D:\home\site\wwwroot\` bijwerken *host.json* of `D:\home\site\wwwroot\<function_name>` een functie-bestanden bij te werken.
4. Slepen en neerzetten een bestand dat u wilt uploaden naar de juiste map in het raster bestand. Er zijn twee gebieden in het bestand raster waar u een bestand kunt neerzetten. Voor *.zip* bestanden, ziet u een vak met het label 'hier naartoe slepen om te uploaden en pak." Voor andere bestandstypen verwijderen in het raster bestand maar buiten het vak 'pak'.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="to-use-continuous-deployment"></a>Doorlopende implementatie gebruiken
Volg de instructies in het onderwerp [continue implementatie voor Azure Functions](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Parallelle uitvoering
Wanneer meerdere activerende gebeurtenissen sneller plaatsvinden dan een functie single thread-runtime kan verwerken, kan de functie meerdere keren parallel door de runtime worden aangeroepen.  Als een functie-app gebruikt de [verbruik die als host fungeert voor plan](functions-scale.md#how-the-consumption-plan-works), de functie-app kan automatisch geschaald uitbreiden.  Elk exemplaar van de functie-app of de app wordt uitgevoerd op het verbruik hosting-abonnement of een gewone [App Service-plan hostingplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), gelijktijdige functie aanroepen parallel met meerdere threads mogelijk verwerkt.  Het maximum aantal gelijktijdige functie aanroepen in elke functie app-exemplaar afhankelijk van het type van de trigger wordt gebruikt, evenals de resources die door andere functies in de functie-app gebruikt.

## <a name="functions-runtime-versioning"></a>Functies runtime versiebeheer

U kunt configureren dat de versie van de functies runtime met behulp van de `FUNCTIONS_EXTENSION_VERSION` app-instelling. De waarde '~ 1' geeft bijvoorbeeld aan uw App in de functie wordt gebruik 1 als de primaire versie. Functie Apps zijn bijgewerkt naar elke nieuwe secundaire versie zodra ze worden vrijgegeven. Zie voor meer informatie, inclusief het weergeven van de exacte versie van uw app functie [hoe gericht op Azure Functions-runtime-versies](functions-versions.md).

## <a name="repositories"></a>Opslagplaatsen
De code voor Azure Functions is open source en opgeslagen in de GitHub-opslagplaatsen:

* [Azure Functions-runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Functions-portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Functions-sjablonen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-extensies](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindingen
Hier volgt een lijst met alle ondersteunde bindingen.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Melden van problemen
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Azure Functions C# referentie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure Functions F # referentie voor ontwikkelaars](functions-reference-fsharp.md)
* [Azure Functions NodeJS-referentie voor ontwikkelaars](functions-reference-node.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* [Azure Functions: Het traject](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) op de Azure App Service-team-blog. Een overzicht van hoe Azure Functions is ontwikkeld.

