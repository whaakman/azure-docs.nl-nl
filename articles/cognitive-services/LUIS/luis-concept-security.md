---
title: Informatie over toegang tot toepassingen van LUIS - Azure | Microsoft Docs
description: Leer hoe u toegang tot LUIS ontwerpen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 90b53bf1334a68fcea813653a7f6d28f362fbd2e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950645"
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
Toegang tot het eindpunt naar query LUIS wordt bepaald door de **openbare** instellen van de app op de **instellingen** pagina. Query voor een persoonlijke app-eindpunt wordt gecontroleerd op een bevoegde sleutel met het resterende quotum treffers. Een openbare app-eindpunt query heeft ook een eindpuntsleutel (van degene die ervoor dat de query zorgt) opgeven die ook voor de resterende quotum treffers is ingeschakeld. 

De eindpuntsleutel in de querytekenreeks van de GET-aanvraag wordt doorgegeven, of vraag van de koptekst van het bericht.

![De app instellen op openbaar](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Persoonlijke app-eindpuntbeveiliging
Een persoonlijke app eindpunt is alleen beschikbaar voor het volgende:

|Sleutel en de gebruiker|Uitleg|
|--|--|--|
|De eigenaar van authoring sleutel| Maximaal 1000 eindpunt treffers|
|De deelnemers authoring sleutels| Maximaal 1000 eindpunt treffers|
|Eindpunt-sleutels die zijn toegevoegd vanuit de **[publiceren](luis-how-to-publish-app.md)** pagina|Eigenaar en samenwerkers kunnen eindpunt sleutels toevoegen|

Andere sleutels ontwerpen of eindpunt hebben **geen** toegang.

### <a name="public-app-endpoint-access"></a>Toegang tot het eindpunt van de openbare app
Configureren van de app als **openbare** op de **instellingen** pagina van de app. Nadat een app is geconfigureerd als openbare, _eventuele_ geldig LUIS ontwerpen of LUIS eindpuntsleutel kunt uw app, een query, zolang de sleutel heeft het quotum voor de hele eindpunt niet gebruikt.

Een gebruiker die geen eigenaar of samenwerker, alleen toegang tot een openbare app als de app-ID. LUIS beschikt niet over een openbare _markt_ of andere manier om te zoeken naar een openbare app.  

## <a name="microsoft-user-accounts"></a>Microsoft-gebruikersaccounts
Auteurs en samenwerkers kunnen u sleutels LUIS op de pagina publiceren. Het Microsoft-gebruikersaccount dat wordt gemaakt van de LUIS-sleutel in Azure portal, moet de eigenaar van de app of een medewerker van de app. 

Zie [Azure Active Directory-tenant gebruiker](luis-how-to-account-settings.md#azure-active-directory-tenant-user) voor meer informatie over Active Directory-gebruikersaccounts. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Beveiligen van het eindpunt 
U kunt regelen wie uw LUIS-eindpuntsleutel door het aanroepen van deze in een omgeving met server-naar-server kunt bekijken. Als u LUIS gebruikt van een bot, is de verbinding tussen de bot en LUIS al beveiligd. Als u het eindpunt LUIS rechtstreeks aanroept, moet u een API-serverzijde maken (zoals een Azure [functie](https://azure.microsoft.com/services/functions/)) met beperkte toegang (zoals [AAD](https://azure.microsoft.com/services/active-directory/)). Wanneer de server-side-API wordt aangeroepen en de verificatie en autorisatie worden geverifieerd, geeft u de aanroep naar LUIS. Terwijl deze strategie niet te voorkomen man-in-the-middle-aanvallen dat, het obfuscates het eindpunt van uw gebruikers, kunt u voor het bijhouden van toegang, en kunt u logboekregistratie van eindpunt antwoord toevoegen (zoals [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Naleving van beveiliging
LUIS worden de ISO 27001: 2013- en ISO-audit 27018:2014 met nul inbreuken (bevindingen) in het controlerapport is voltooid. LUIS verkregen bovendien ook de CSA STAR-certificering met de hoogste mogelijke Gold Award voor de evaluatie van de mogelijkheid vervaldatum. Azure is de enige grote openbare cloud service-provider aan deze certificering behalen. Voor meer informatie vindt u de LUIS opgenomen in de bijgewerkte scope-instructie in de Azure-hoofdvenster [naleving overzicht](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) document waarnaar wordt verwezen op [Trust Center](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) ISO-pagina's.  

## <a name="next-steps"></a>Volgende stappen

Zie [Best Practices](luis-concept-best-practices.md) voor meer informatie over het gebruik van intenties en entiteiten voor de beste voorspellingen.