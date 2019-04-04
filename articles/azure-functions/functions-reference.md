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
ms.openlocfilehash: 5b2b7f3cd6bfa219b794edc63d6bf8b2784b713c
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891885"
---
# <a name="azure-functions-developers-guide"></a>Azure Functions-handleiding voor ontwikkelaars
In Azure Functions delen specifieke functies een paar core technische concepten en -onderdelen, ongeacht de taal of binding die u gebruikt. Voordat u verder met het leren werken met gegevens die specifiek zijn voor een bepaalde taal of binding, moet u lezen in dit overzicht wordt gegeven die van toepassing op alle hiervan.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [overzicht van Azure Functions](functions-overview.md).

## <a name="function-code"></a>Functiecode aan te geven
Een *functie* is het primaire concept in Azure Functions. Een functie bevat twee belangrijke onderdelen - uw code, die kunnen worden geschreven in verschillende talen en sommige configuratie, het bestand function.json. Dit configuratiebestand wordt voor gecompileerde talen automatisch gegenereerd van aantekeningen in uw code. Voor scripttalen, moet u zelf de config-bestand opgeven.

Het bestand function.json definieert de trigger van de functie, bindingen en andere configuratie-instellingen. Elke functie heeft slechts één trigger. De runtime maakt gebruik van deze config-bestand om te bepalen welke gebeurtenissen u wilt bewaken en hoe u gegevens overbrengen en als resultaat de gegevens van een functie wordt uitgevoerd. Hier volgt een voorbeeld van de function.json-bestand.

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

De `bindings` eigenschap is waar u configureert zowel triggers en bindingen. Elke binding deelt een paar algemene instellingen en bepaalde instellingen die specifiek voor een bepaald type binding zijn. Elke binding vereist de volgende instellingen:

| Eigenschap | Waarden/typen | Opmerkingen |
| --- | --- | --- |
| `type` |string |Bindingstype. Bijvoorbeeld `queueTrigger`. |
| `direction` |'in', 'out' |Geeft aan of de binding voor het ontvangen van gegevens in de functie of het verzenden van gegevens van de functie. |
| `name` |string |De naam die wordt gebruikt voor de gebonden gegevens in de functie. Voor C# is dit de naam van een argument; voor JavaScript is de sleutel in een sleutel/waarde-lijst. |

## <a name="function-app"></a>Function App
Een functie-app biedt een context voor uitvoering in Azure waarin uw functies worden uitgevoerd. Een functie-app bestaat uit een of meer afzonderlijke functies die worden beheerd, geïmplementeerd en geschaald samen. Alle van de functies in een functie-app delen de dezelfde prijsschema, continue implementatie- en runtime-versie. Een functie-app beschouwen als een manier om te organiseren en gezamenlijk beheren van uw functies. 

> [!NOTE]
> Alle functies in een functie-app moeten worden gemaakt in dezelfde taal. In [vorige versies](functions-versions.md) van de Azure Functions-runtime, dit er niet vereist is.

## <a name="folder-structure"></a>mapstructuur
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Het bovenstaande is de standaardinstelling (en aanbevolen) mapstructuur voor een functie-app. Als u de bestandslocatie van een functie code wijzigen wilt, wijzigt u de `scriptFile` sectie van de _function.json_ bestand. We raden u ook aan met behulp van [pakket implementatie](deployment-zip-push.md) aan uw project implementeren in uw functie-app in Azure. U kunt ook bestaande hulpprogramma's zoals [continue integratie en implementatie](functions-continuous-deployment.md) en Azure DevOps.

> [!NOTE]
> Als een pakket handmatig implementeert, zorg ervoor dat u implementeert uw _host.json_ bestanden en mappen rechtstreeks naar werken de `wwwroot` map. Neem niet de `wwwroot` map in uw implementaties. Anders krijgt u uiteindelijk met `wwwroot\wwwroot` mappen.

#### <a name="use-local-tools-and-publishing"></a>Gebruik lokale hulpprogramma's en publiceren
Functie-apps kunnen worden gemaakt en gepubliceerd met behulp van verschillende hulpprogramma's, waaronder [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](functions-create-first-function-vs-code.md), [IntelliJ](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md), en de [Azure Functions Core Tools](./functions-develop-local.md). Zie voor meer informatie, [Code en test Azure Functions lokaal](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

## <a id="fileupdate"></a> Het bewerken van functies in Azure portal
De Functions-editor die is ingebouwd in de Azure-portal kunt u uw code bijwerken en uw *function.json* bestand direct inline. Dit wordt alleen aanbevolen voor kleine wijzigingen of bewijzen van concept - aanbevolen procedure is het gebruik van een lokale ontwikkeling hulpprogramma zoals VS Code.

## <a name="parallel-execution"></a>Parallelle uitvoering
Als er meerdere activerende gebeurtenissen optreedt sneller dan een single-threaded functieruntime ze kan verwerken, kan de functie meerdere keren parallel aanroepen in de runtime.  Als een functie-app gebruikt de [Verbruikshostingplan](functions-scale.md#how-the-consumption-and-premium-plans-work), de functie-app kan automatisch uitschalen.  Elk exemplaar van de functie-app, of de app wordt uitgevoerd op het verbruik hosting plan of een normale [App Service-hostingabonnement](../app-service/overview-hosting-plans.md), gelijktijdige functieaanroepen parallel met meerdere threads mogelijk verwerkt.  Het maximum aantal gelijktijdige functieaanroepen in elke functie-app-exemplaar is afhankelijk van het type van de trigger wordt gebruikt, evenals de resources die worden gebruikt door andere functies in de functie-app.

## <a name="functions-runtime-versioning"></a>Functions runtime versiebeheer

U kunt configureren dat de versie van de Functions-runtime via de `FUNCTIONS_EXTENSION_VERSION` app-instelling. De waarde '~ 2' geeft bijvoorbeeld aan dat uw functie-App als de primaire versie 2.x gebruikt. Functie-Apps zijn bijgewerkt naar de nieuwe secundaire versie zodra ze worden vrijgegeven. Zie voor meer informatie, inclusief het weergeven van de exacte versie van uw functie-app [hoe gericht op versies van Azure Functions-runtime](set-runtime-version.md).

## <a name="repositories"></a>Opslagplaatsen
De code voor Azure Functions is open-source en opgeslagen in de GitHub-opslagplaatsen:

* [Azure Functions](https://github.com/Azure/Azure-Functions)
* [Azure Functions-host](https://github.com/Azure/azure-functions-host/)
* [Azure Functions-portal](https://github.com/azure/azure-functions-ux)
* [Azure Functions-sjablonen](https://github.com/azure/azure-functions-templates)
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

* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* [Code en Azure Functions lokaal testen](./functions-develop-local.md)
* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Azure Functions C# referentie voor ontwikkelaars](functions-reference-csharp.md)
* [Referentie voor ontwikkelaars van Azure Functions-NodeJS](functions-reference-node.md)
