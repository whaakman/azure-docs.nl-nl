---
title: OpenAPI-metagegevens in Azure Functions | Microsoft Docs
description: Overzicht van de OpenAPI-ondersteuning in Azure Functions
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: 93e6b8c606c0a6d7abebeb515b938a45001757c1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950366"
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Ondersteuning voor OpenAPI 2.0-metagegevens in Azure Functions (preview)
OpenAPI 2.0 (voorheen Swagger) ondersteuning voor metagegevens in Azure Functions is een preview-functie die u gebruiken kunt om te schrijven van de definitie van een OpenAPI 2.0 in een functie-app. U kunt vervolgens dat bestand hosten met behulp van de functie-app.

> [!IMPORTANT]
> De OpenAPI-preview-functie is alleen beschikbaar in de runtime 1.x. Meer informatie over het maken van een 1.x-functie-app [vindt u hier](./functions-versions.md#creating-1x-apps).

[OpenAPI-metagegevens](http://swagger.io/) kunt u een functie die als host fungeert voor een REST-API om te worden verbruikt door een groot aantal andere software. Deze software bevat Microsoft-aanbiedingen zoals PowerApps en de [API Apps-functie van Azure App Service](../app-service/app-service-web-overview.md), hulpprogramma's voor ontwikkelaars van derden, zoals [Postman](https://www.getpostman.com/docs/importing_swagger), en [nog veel meer pakketten](http://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>We raden u aan te beginnen met de [zelfstudie aan de slag](./functions-api-definition-getting-started.md) en vervolgens terugkeert naar dit document voor meer informatie over specifieke functies.

## <a name="enable"></a>Ondersteuning voor OpenAPI-definitie
U kunt alle OpenAPI-instellingen configureren op de **API-definitie** pagina in uw functie-app **platformfuncties**.

> [!NOTE]
> API-definitie-functie van de functie is momenteel niet ondersteund voor Bèta-runtime.

Instellen zodat de generatie van een gehoste OpenAPI-definitie en de definitie van een snelstartgids **bron van API-definitie** naar **functie (Preview)**. **Externe URL** Hiermee staat u de functie te gebruiken van een OpenAPI-definitie die ergens anders is gehost.

## <a name="generate-definition"></a>Het geraamte Swagger uit uw functie metagegevens genereren
Aan de hand van een sjabloon kunt u beginnen met het schrijven van uw eerste OpenAPI-definitie. De functie voor definitie-sjabloon maakt een eenvoudige OpenAPI-definitie met behulp van alle metagegevens in het bestand function.json voor elk van de functies van uw HTTP-trigger. U moet invullen meer informatie over uw API van de [OpenAPI-specificatie](http://swagger.io/specification/), zoals aanvraag- en -sjablonen.

Zie voor stapsgewijze instructies, de [zelfstudie aan de slag](./functions-api-definition-getting-started.md).

### <a name="templates"></a>Beschikbare sjablonen

|Naam| Beschrijving |
|:-----|:-----|
|Gegenereerde definitie|Een OpenAPI-definitie met de maximale hoeveelheid gegevens die kan worden afgeleid uit bestaande metagegevens van de functie.|

### <a name="quickstart-details"></a>Opgenomen metagegevens in de gegenereerde definitie

In de volgende tabel staat voor de Azure portal-instellingen en de bijbehorende gegevens in de function.json zoals deze is toegewezen aan de gegenereerde Swagger-basis.

|Swagger.json|Portal UI|Function.json|
|:----|:-----|:-----|
|[Host](http://swagger.io/specification/#fixed-fields-15)|**Functie van app-instellingen** > **App Service-instellingen** > **overzicht** > **URL**|*Niet aanwezig*
|[Paden](http://swagger.io/specification/#paths-object-29)|**Integreer** > **geselecteerde HTTP-methoden**|Bindingen: Route
|[Pad-Item](http://swagger.io/specification/#path-item-object-32)|**Integreer** > **Routesjabloon**|Bindingen: methoden
|[Beveiliging](http://swagger.io/specification/#security-scheme-object-112)|**sleutels**|*Niet aanwezig*|
|bewerkings-id *|**Route + toegestane bewerkingen**|Route + toegestane bewerkingen|

\*De bewerkings-ID is alleen vereist voor integratie met PowerApps en Flow.
> [!NOTE]
> De x-ms-summary-extensie biedt een weergavenaam in Logic Apps, PowerApps en Flow.
>
> Zie voor meer informatie, [uw Swagger-definitie aanpassen voor PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>CI/CD gebruiken om in te stellen van een API-definitie

 API-definitie die als host fungeert in de portal voordat u broncodebeheer om te wijzigen van uw API-definitie vanuit broncodebeheer inschakelt, moet u inschakelen. Volg deze instructies:

1. Blader naar **API-definitie (preview)** in de instellingen van uw functie-app.
  1. Stel **bron van API-definitie** naar **functie**.
  1. Klik op **sjabloon voor genereren van API-definitie** en vervolgens **opslaan** te maken van de Sjabloondefinitie van een later aan te passen.
  1. Houd er rekening mee voor uw API-definitie URL en de sleutel.
1. [Instellen van continue integratie/continue implementatie (CI/CD)](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).
2. Wijzigen van swagger.json in Bronbeheer op \site\wwwroot\.azurefunctions\swagger\swagger.json.

Nu wijzigingen in swagger.json in uw opslagplaats worden gehost door uw functie-app op de API URL-definitie en de sleutel die u hebt genoteerd in stap 1.c.

## <a name="next-steps"></a>Volgende stappen
* [Zelfstudie aan de slag](functions-api-definition-getting-started.md). Probeer onze scenario als u wilt een OpenAPI-definitie in actie zien.
* [Azure Functions-GitHub-opslagplaats](https://github.com/Azure/Azure-Functions/). Kijk eens naar de opslagplaats van de functies naar ons feedback geven over de API-definitie ondersteuning voor Preview-versie. Een GitHub-probleem maken voor alles wat die u bijgewerkte wilt zien.
* [Referentie voor ontwikkelaars van Azure Functions](functions-reference.md). Meer informatie over het coderen van functies en het definiëren van triggers en bindingen.
