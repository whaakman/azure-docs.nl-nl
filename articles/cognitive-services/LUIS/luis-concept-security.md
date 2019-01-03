---
title: Beveiliging wanneer ze samenwerken
titleSuffix: Language Understanding - Azure Cognitive Services
description: Ontwerpen toegang is beschikbaar voor de eigenaar en samenwerkers. Eindpunt toegang is voor een persoonlijke app beschikbaar voor de eigenaar en samenwerkers.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: diberry
ms.openlocfilehash: 3de58f244012ee0460812fba1ceb5ab12f60aa51
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53602576"
---
# <a name="authoring-and-endpoint-user-access"></a>Ontwerp- en eindpunt gebruikerstoegang
Ontwerpen toegang is beschikbaar voor de eigenaar en samenwerkers. Eindpunt toegang is voor een persoonlijke app beschikbaar voor de eigenaar en samenwerkers. Voor een openbare app is eindpunt toegang beschikbaar voor iedereen die hun eigen account LUIS en heeft de openbare app-ID. 

## <a name="access-to-authoring"></a>Toegang tot het ontwerpen
Toegang tot de app uit de [LUIS](luis-reference-regions.md#luis-website) website of de [API's ontwerpen](https://aka.ms/luis-authoring-apis) wordt bepaald door de eigenaar van de app. 

De eigenaar en alle deelnemers hebt toegang tot het ontwerpen van de app. 

|Omvat toegang ontwerpen|Opmerkingen|
|--|--|
|Toevoegen of verwijderen van de eindpunt-sleutels||
|Exporteren van versie||
|Eindpunt-logboeken exporteren||
|Versie wordt geïmporteerd||
|App openbaar maken|Wanneer een app openbaar is, kan iedereen met een sleutel ontwerpen of eindpunt query uitvoeren op de app.|
|Model wijzigen|
|Publiceren|
|Bekijk eindpunt uitingen voor [actief leren](luis-how-to-review-endoint-utt.md)|
|Trainen|

## <a name="access-to-endpoint"></a>Toegang tot het eindpunt

Toegang tot het eindpunt van de query wordt beheerd door een instelling op de **toepassingsinformatie** pagina in de **beheren** sectie. 

![De app instellen op openbaar](./media/luis-concept-security/set-application-as-public.png)

|[Persoonlijke eindpunt](#private-app-endpoint-security)|[Openbaar eindpunt](#public-app-endpoint-access)|
|:--|:--|
|Beschikbaar voor de eigenaar en samenwerkers|Beschikbaar voor de eigenaar, medewerkers en iedereen die niemand app-ID|

### <a name="private-app-endpoint-security"></a>Persoonlijke app-eindpuntbeveiliging

Een persoonlijke app eindpunt is alleen beschikbaar voor het volgende:

|Sleutel en de gebruiker|Uitleg|
|--|--|--|
|De eigenaar van authoring sleutel| Maximaal 1000 eindpunt treffers|
|De deelnemers authoring sleutels| Maximaal 1000 eindpunt treffers|
|Een willekeurige toets door de auteur of een medewerker die is toegewezen aan LUIS|Op basis van gebruik van de sleutel-laag|

#### <a name="microsoft-user-accounts"></a>Microsoft-gebruikersaccounts

Auteurs en samenwerkers kunnen sleutels toewijzen aan een persoonlijke LUIS-app. Het Microsoft-gebruikersaccount dat wordt gemaakt van de LUIS-sleutel in Azure portal moet de eigenaar van de app of een medewerker van de app. U kunt een sleutel toewijzen aan een persoonlijke app uit een andere Azure-account.

Zie [Azure Active Directory-tenant gebruiker](luis-how-to-collaborate.md#azure-active-directory-tenant-user) voor meer informatie over Active Directory-gebruikersaccounts. 

### <a name="public-app-endpoint-access"></a>Toegang tot het eindpunt van de openbare app

Nadat een app is geconfigureerd als openbare, _eventuele_ geldig LUIS ontwerpen of LUIS eindpuntsleutel kunt uw app, een query, zolang de sleutel heeft het quotum voor de hele eindpunt niet gebruikt.

Een gebruiker die geen eigenaar of samenwerker, alleen toegang tot een openbare app als de app-ID. LUIS beschikt niet over een openbare _markt_ of andere manier om te zoeken naar een openbare app.  

Een openbare app is gepubliceerd in alle regio's, zodat een gebruiker met een sleutel op basis van een regio LUIS resource toegang heeft tot de app in welke regio gekoppeld aan de resource-sleutel is.

## <a name="microsoft-user-accounts"></a>Microsoft-gebruikersaccounts

Auteurs en samenwerkers kunnen u sleutels LUIS op de pagina publiceren. Het Microsoft-gebruikersaccount dat wordt gemaakt van de LUIS-sleutel in Azure portal moet de eigenaar van de app of een medewerker van de app. 

Zie [Azure Active Directory-tenant gebruiker](luis-how-to-collaborate.md#azure-active-directory-tenant-user) voor meer informatie over Active Directory-gebruikersaccounts. 

## <a name="securing-the-endpoint"></a>Beveiligen van het eindpunt 

U kunt regelen wie uw LUIS-eindpuntsleutel door het aanroepen van deze in een omgeving met server-naar-server kunt bekijken. Als u LUIS gebruikt van een bot, is de verbinding tussen de bot en LUIS al beveiligd. Als u het eindpunt LUIS rechtstreeks aanroept, moet u een API-serverzijde maken (zoals een Azure [functie](https://azure.microsoft.com/services/functions/)) met beperkte toegang (zoals [AAD](https://azure.microsoft.com/services/active-directory/)). Wanneer de server-side-API wordt aangeroepen en de verificatie en autorisatie worden geverifieerd, geeft u de aanroep naar LUIS. Terwijl deze strategie niet te voorkomen man-in-the-middle-aanvallen dat, het obfuscates het eindpunt van uw gebruikers, kunt u voor het bijhouden van toegang, en kunt u logboekregistratie van eindpunt antwoord toevoegen (zoals [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Naleving van beveiliging
 
[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-security-compliance.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [Best Practices](luis-concept-best-practices.md) voor meer informatie over het gebruik van intenties en entiteiten voor de beste voorspellingen.
