---
title: OpenAPI metagegevens in Azure Functions | Microsoft Docs
description: Overzicht van ondersteuning voor Azure Functions OpenAPI
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: b6aacc536e589a2036aba5a0784a4ba71641a59e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Ondersteuning voor OpenAPI 2.0-metagegevens in de Azure-functies (preview)
OpenAPI 2.0 (voorheen Swagger) ondersteuning voor metagegevens in Azure Functions is een preview-functie die u gebruiken kunt om te schrijven van de definitie van een OpenAPI 2.0 binnen een functie-app. Vervolgens kunt u dat bestand met de functie-app hosten.

[OpenAPI metagegevens](http://swagger.io/) biedt de mogelijkheid een functie die als host fungeert voor een REST-API om te worden verbruikt door een groot aantal andere software. Deze software bevat de offerings Microsoft zoals PowerApps en de [API Apps-functie van Azure App Service](../app-service/app-service-web-overview.md), hulpprogramma's voor ontwikkelaars van derden, zoals [Postman](https://www.getpostman.com/docs/importing_swagger), en [veel meer pakketten](http://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>Het is raadzaam beginnen met de [zelfstudie aan de slag](./functions-api-definition-getting-started.md) en vervolgens te retourneren aan dit document voor meer informatie over specifieke functies.

## <a name="enable"></a>OpenAPI definitie ondersteuning inschakelen
U kunt alle OpenAPI-instellingen configureren op de **API-definitie** pagina in de functie-app **platformfuncties**.

Zodat het genereren van de definitie van een gehoste OpenAPI en een Quick Start-definitie ingesteld **API-definitie bron** naar **functie (Preview)**. **Externe URL** biedt de mogelijkheid uw functie te gebruiken van de definitie van een OpenAPI die heeft die elders wordt gehost.

## <a name="generate-definition"></a>Genereren van een geraamte Swagger van metagegevens van de functie
Een sjabloon kunt u de definitie van de eerste OpenAPI vrienden. De functie voor definitie-sjabloon maakt de definitie van een sparse OpenAPI door alle metagegevens in het bestand function.json voor elk van de functies van uw HTTP-trigger. Moet u meer informatie over de API van Vul de [OpenAPI specificatie](http://swagger.io/specification/), zoals sjablonen voor aanvraag en -antwoord.

Zie voor stapsgewijze instructies de [zelfstudie aan de slag](./functions-api-definition-getting-started.md).

### <a name="templates"></a>Beschikbare sjablonen

|Naam| Beschrijving |
|:-----|:-----|
|De definitie van de gegenereerde|De definitie van een OpenAPI met de maximale hoeveelheid gegevens die kan worden afgeleid van de bestaande metagegevens van de functie.|

### <a name="quickstart-details"></a>Metagegevens van de opgenomen in de definitie van de gegenereerde

De volgende tabel voor de Azure portal-instellingen en de bijbehorende gegevens die in function.json staat zoals is toegewezen aan de gegenereerde Swagger-basis.

|Swagger.JSON|Portal-gebruikersinterface|Function.JSON|
|:----|:-----|:-----|
|[Host](http://swagger.io/specification/#fixed-fields-15)|**App-instellingen werken** > **App Service-instellingen** > **overzicht** > **URL**|*Niet aanwezig*
|[Paden](http://swagger.io/specification/#paths-object-29)|**Integreren** > **geselecteerd HTTP-methoden**|Bindingen: Route
|[Pad-Item](http://swagger.io/specification/#path-item-object-32)|**Integreren** > **Routesjabloon**|Bindingen: methoden
|[Beveiliging](http://swagger.io/specification/#security-scheme-object-112)|**Sleutels**|*Niet aanwezig*|
|operationID *|**Route + toegestane termen**|Route + toegestane termen|

\*De bewerkings-ID is alleen vereist voor integratie met PowerApps en stroom.
> [!NOTE]
> De x-ms-overzicht uitbreiding biedt een weergavenaam in Logic Apps, PowerApps en stroom.
>
> Zie voor meer informatie, [aanpassen van uw Swagger-definitie voor PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>Gebruik CI/CD voor het instellen van een API-definitie

 API-definitie die als host fungeert in de portal voordat u broncodebeheer wijzigen van uw API-definitie van broncodebeheer inschakelt, moet u inschakelen. Volg deze instructies:

1. Blader naar **API-definitie (preview)** in de functie app-instellingen.
  1. Stel **API-definitie bron** naar **functie**.
  1. Klik op **genereren API-definitie sjabloon** en vervolgens **opslaan** voor het maken van de Sjabloondefinitie van een om later te wijzigen.
  1. Houd er rekening mee uw API-definitie URL en de sleutel.
1. [Continue integratie/doorlopende implementatie (CI/CD) ingesteld](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).
2. Wijzigen van swagger.json in broncodebeheer op \site\wwwroot\.azurefunctions\swagger\swagger.json.

Nu wijzigingen aan swagger.json in de opslagplaats worden gehost door de functie-app op de API definitie-URL en de sleutel die u hebt genoteerd in stap 1.c.

## <a name="next-steps"></a>Volgende stappen
* [Zelfstudie aan de slag](functions-api-definition-getting-started.md). Probeer onze scenario als de definitie van een OpenAPI in actie wilt weergeven.
* [Azure Functions GitHub-opslagplaats](https://github.com/Azure/Azure-Functions/). Bekijk de functies te ons feedback geven over de API-definitie ondersteuning preview opslagplaats. Maak een probleem met de GitHub voor alles wat die u bijgewerkte wilt zien.
* [Naslaginformatie over Azure Functions developer](functions-reference.md). Meer informatie over het coderen van functies en definiëren van triggers en bindingen.
