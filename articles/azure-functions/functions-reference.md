---
title: Richtlijnen voor het ontwikkelen van Azure Functions | Microsoft Docs
description: Informatie over de Azure Functions-concepten en technieken die u nodig hebt voor het ontwikkelen van functies in Azure, met alle programming talen en bindingen.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Developer guide, azure functions, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: d2b05c83f77a58e224760d90d111b270d71a6514
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092424"
---
# <a name="azure-functions-developers-guide"></a>Azure Functions-handleiding voor ontwikkelaars
In Azure Functions delen specifieke functies een paar core technische concepten en -onderdelen, ongeacht de taal of binding die u gebruikt. Voordat u verder met het leren werken met gegevens die specifiek zijn voor een bepaalde taal of binding, moet u lezen in dit overzicht wordt gegeven die van toepassing op alle hiervan.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [overzicht van Azure Functions](functions-overview.md) en bekend bent met [WebJobs SDK-concepten, zoals triggers en bindingen de runtime JobHost](https://github.com/Azure/azure-webjobs-sdk/wiki). Azure Functions is gebaseerd op de WebJobs SDK. 

## <a name="function-code"></a>Functiecode aan te geven
Een *functie* is het primaire concept in Azure Functions. U schrijft code voor een functie in een taal van uw keuze en sla de code en configuratie-bestanden in dezelfde map. De configuratie is met de naam `function.json`, die JSON-configuratiegegevens bevat. Verschillende talen worden ondersteund, en elk pakket heeft een enigszins ervaring die is geoptimaliseerd voor het meest geschikt zijn voor die taal. 

Het bestand function.json definieert de functiebindingen en andere configuratie-instellingen. De runtime maakt gebruik van dit bestand om te bepalen welke gebeurtenissen u wilt controleren en het doorgeven van gegevens in en als resultaat de gegevens van een functie wordt uitgevoerd. Hier volgt een voorbeeld van de function.json-bestand.

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

De `bindings` eigenschap is waar u configureert zowel triggers en bindingen. Elke binding deelt een paar algemene instellingen en bepaalde instellingen die specifiek voor een bepaald type binding zijn. Elke binding vereist de volgende instellingen:

| Eigenschap | Waarden/typen | Opmerkingen |
| --- | --- | --- |
| `type` |tekenreeks |Bindingstype. Bijvoorbeeld `queueTrigger`. |
| `direction` |'in', 'out' |Geeft aan of de binding voor het ontvangen van gegevens in de functie of het verzenden van gegevens van de functie. |
| `name` |tekenreeks |De naam die wordt gebruikt voor de gebonden gegevens in de functie. Voor C# is dit de naam van een argument; voor JavaScript is de sleutel in een sleutel/waarde-lijst. |

## <a name="function-app"></a>Function App
Een functie-app bestaat uit een of meer afzonderlijke functies die samen worden beheerd door Azure App Service. Alle van de functies in een functie-app delen de dezelfde prijsschema, continue implementatie- en runtime-versie. Functies die zijn geschreven in meerdere talen kunnen alle dezelfde functie-app delen. Een functie-app beschouwen als een manier om te organiseren en gezamenlijk beheren van uw functies. 

## <a name="runtime-script-host-and-web-host"></a>Runtime (scripthost en web-host)
De runtime of scripthost, is de onderliggende host voor de WebJobs SDK die luistert naar gebeurtenissen, verzamelt en verzendt gegevens, en uiteindelijk uw code wordt uitgevoerd. 

Om te kunnen HTTP-triggers, is er ook een webhost die is ontworpen om te blijven in het zicht van de scripthost in productiescenario's. Twee hosts die beëindigen helpt om te isoleren van de scripthost uit het voorste deel verkeer die worden beheerd door de web-host.

## <a name="folder-structure"></a>Mapstructuur
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bij het instellen van een project voor het implementeren van functies aan een functie-app in Azure App Service, kunt u deze mapstructuur behandelen als de code van uw site. Kunt u bestaande hulpprogramma's, zoals continue integratie en implementatie, of aangepaste implementatie scripts voor installatie van het pakket tijd implementeren of transpilation code.

> [!NOTE]
> Zorg ervoor dat u implementeert uw `host.json` bestanden en mappen rechtstreeks naar werken de `wwwroot` map. Neem niet de `wwwroot` map in uw implementaties. Anders krijgt u uiteindelijk met `wwwroot\wwwroot` mappen. 
> 
> 

## <a id="fileupdate"></a> Het bijwerken van de functie app-bestanden
De functie-editor die is ingebouwd in de Azure-portal kunt u werken de *function.json* bestand en het codebestand voor een functie. Uploaden of andere updatebestanden zoals *package.json* of *project.json* of afhankelijkheden, die u moet gebruiken van andere methoden voor het implementeren.

Functie-apps zijn gebouwd op App Service, waardoor alle de [implementatie-opties die beschikbaar zijn om de standaard web-apps](../app-service/app-service-deploy-local-git.md) zijn ook beschikbaar voor functie-apps. Hier volgen enkele methoden die u gebruiken kunt om te uploaden of bijwerken van de functie app-bestanden. 

#### <a name="to-use-app-service-editor"></a>App Service-Editor gebruiken
1. Klik in de Azure Functions-portal op **platformfuncties**.
2. In de **ONTWIKKELTOOLS** sectie, klikt u op **App Service-Editor**.   
   Nadat de App Service-Editor wordt geladen, ziet u de *host.json* bestands- en functie mappen onder *wwwroot*. 
5. Open bestanden bewerken, of slepen en neerzetten van de ontwikkelcomputer om bestanden te uploaden.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Om de functie-app SCM (Kudu) eindpunt te gebruiken
1. Navigeer naar: `https://<function_app_name>.scm.azurewebsites.net`.
2. Klik op **Foutopsporingsconsole > CMD**.
3. Navigeer naar `D:\home\site\wwwroot\` om bij te werken *host.json* of `D:\home\site\wwwroot\<function_name>` van een functie-bestanden bij te werken.
4. Slepen en neerzetten een bestand dat u wilt uploaden naar de juiste map in het raster bestand. Er zijn twee gebieden in het bestand raster waar u een bestand kunt neerzetten. Voor *.zip* bestanden, er wordt een vak weergegeven met het label "Sleep hier om te uploaden en pak deze uit." Voor andere bestandstypen verwijderen in het raster bestand, maar buiten het vak 'unzip'.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="to-use-continuous-deployment"></a>Continue implementatie gebruiken
Volg de instructies in het onderwerp [continue implementatie voor Azure Functions](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Parallelle uitvoering
Als er meerdere activerende gebeurtenissen optreedt sneller dan een single-threaded functieruntime ze kan verwerken, kan de functie meerdere keren parallel aanroepen in de runtime.  Als een functie-app gebruikt de [Verbruikshostingplan](functions-scale.md#how-the-consumption-plan-works), de functie-app kan automatisch uitschalen.  Elk exemplaar van de functie-app, of de app wordt uitgevoerd op het verbruik hosting plan of een normale [App Service-hostingabonnement](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), gelijktijdige functieaanroepen parallel met meerdere threads mogelijk verwerkt.  Het maximum aantal gelijktijdige functieaanroepen in elke functie-app-exemplaar is afhankelijk van het type van de trigger wordt gebruikt, evenals de resources die worden gebruikt door andere functies in de functie-app.

## <a name="functions-runtime-versioning"></a>Functions runtime versiebeheer

U kunt configureren dat de versie van de Functions-runtime via de `FUNCTIONS_EXTENSION_VERSION` app-instelling. Bijvoorbeeld, de waarde '~ 1' geeft aan dat de functie-App wordt gebruik 1 als de primaire versie. Functie-Apps zijn bijgewerkt naar de nieuwe secundaire versie zodra ze worden vrijgegeven. Zie voor meer informatie, inclusief het weergeven van de exacte versie van uw functie-app [hoe gericht op versies van Azure Functions-runtime](set-runtime-version.md).

## <a name="repositories"></a>Opslagplaatsen
De code voor Azure Functions is open-source en opgeslagen in de GitHub-opslagplaatsen:

* [Azure Functions-runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Functions-portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Functions-sjablonen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindingen
Hier volgt een tabel met alle ondersteunde bindingen.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Problemen met fouten die afkomstig zijn van de bindingen? Controleer de [foutcodes voor Azure Functions-Binding](functions-bindings-error-pages.md) documentatie.

## <a name="reporting-issues"></a>Problemen melden
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Azure Functions C# referentie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure Functions F # referentie voor ontwikkelaars](functions-reference-fsharp.md)
* [Referentie voor ontwikkelaars van Azure Functions-NodeJS](functions-reference-node.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* [Azure Functions: Het traject](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) op het teamblog van Azure App Service. Een overzicht van hoe Azure Functions is ontwikkeld.

