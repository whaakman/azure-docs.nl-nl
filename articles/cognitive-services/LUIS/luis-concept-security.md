---
title: Toegang tot toepassingen LUIS - Azure begrijpen | Microsoft Docs
description: Meer informatie over toegang tot LUIS ontwerpen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 44380e12e6d095e8d40675af0b6b2fddc5e4c4e9
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264264"
---
# <a name="authoring-and-endpoint-user-access"></a>Ontwerp- en eindpunt gebruikerstoegang
Toegang ontwerpen is beschikbaar voor eigenaars en deelnemers. Voor een persoonlijke app eindpunt toegang is voor eigenaren en deelnemers. Voor een openbare app eindpunt access is beschikbaar voor iedereen die hun eigen account LUIS en heeft de openbare app-ID. 

## <a name="access-to-authoring"></a>Toegang tot het ontwerpen
Toegang tot de app vanuit de [LUIS] [ LUIS] website of de [authoring API's](https://aka.ms/luis-authoring-apis) wordt bepaald door de eigenaar van de app. 

De eigenaar en alle deelnemers hebben toegang tot de app te maken. 

|Bevat toegang ontwerpen|Opmerkingen|
|--|--|
|Toevoegen of verwijderen van de endpoint-sleutels||
|Versie exporteren||
|Exporteren van endpoint-Logboeken||
|Versie importeren||
|App openbaar maken|Wanneer een app openbaar is, kan iedereen met een sleutel ontwerpen of eindpunt query uitvoeren op de app.|
|Model wijzigen|
|Publiceren|
|Bekijk eindpunt utterances voor [active learning](label-suggested-utterances.md)|
|Trainen|

## <a name="access-to-endpoint"></a>Toegang tot het eindpunt
Toegang tot het eindpunt naar query LUIS wordt bepaald door de **openbare** instellen van de app voor de **instellingen** pagina. Query voor een persoonlijke app-eindpunt wordt gecontroleerd op een bevoegde sleutel met quotum treffers overgebleven. Een openbare app eindpunt query heeft om toegang te bieden ook een eindpuntsleutel (van degene die aan de query te brengen) die ook is ingeschakeld voor de resterende quotum treffers. 

De eindpuntsleutel in de queryreeks van de GET-aanvraag wordt doorgegeven, of de koptekst van de POST-aanvragen.

![Set app openbare](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Beveiliging van persoonlijke app-eindpunt
Een persoonlijke app-eindpunt is alleen beschikbaar voor het volgende:

|Sleutel en de gebruiker|Uitleg|
|--|--|--|
|Ontwerp van de eigenaar-sleutel| Maximaal 1000 eindpunt treffers|
|De deelnemers authoring sleutels| Maximaal 1000 eindpunt treffers|
|Eindpunt sleutels die zijn toegevoegd vanuit **[publiceren](publishapp.md)** pagina|Eigenaar en deelnemers kunnen eindpunt sleutels toevoegen|

Andere sleutels ontwerpen of eindpunt hebben **geen** toegang.

### <a name="public-app-endpoint-access"></a>Toegang tot de openbare app-eindpunt
Configureren van de app als **openbare** op de **instellingen** pagina van de app. Zodra een app is geconfigureerd als openbaar, _eventuele_ geldig LUIS ontwerpen of LUIS eindpuntsleutel kan uw app opvragen, zolang de sleutel heeft het quotum voor de hele eindpunt niet gebruikt.

Een gebruiker die is geen eigenaar of samenwerker, alleen toegang tot een openbare app als een bepaalde van de app-ID. LUIS beschikt niet over een openbare _markt_ of andere manier om te zoeken naar een openbare app.  

## <a name="microsoft-user-accounts"></a>Microsoft-gebruikersaccounts
Auteurs en deelnemers kunnen u sleutels LUIS op de pagina publiceren. Het Microsoft-gebruikersaccount dat de sleutel LUIS in de Azure portal maakt, moet de eigenaar van de app of een medewerker van de app. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Het eindpunt beveiligen 
U kunt bepalen wie uw LUIS endpoint-sleutel kunnen zien door het aanroepen van deze in een omgeving met server-naar-server. Als u van LUIS van bot's gebruikmaakt, is de verbinding tussen de bot en LUIS al beveiligd. Als u het eindpunt LUIS rechtstreeks aanroept, moet u een API-serverzijde maken (zoals een Azure [functie](https://azure.microsoft.com/services/functions/)) met beperkte toegang (zoals [AAD](https://azure.microsoft.com/services/active-directory/)). Als de API-serverzijde wordt aangeroepen en de verificatie en autorisatie worden geverifieerd, geeft u de aanroep naar LUIS. Terwijl deze strategie niet man-in-the-middle-aanvallen te voorkomen, het obfuscates van uw eindpunt van uw gebruikers, kunt u toegang tot, bijhouden en kunt u toevoegen eindpunt antwoord logboekregistratie (zoals [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Beveiliging
LUIS voltooid de ISO 27001: 2013 en ISO 27018:2014 audit met nul inbreuken (bevindingen) in het auditrapport. Bovendien verkregen LUIS ook de certificering CSA STER met het hoogst mogelijke Gold plaatsen voor de beoordeling van de mogelijkheid vervaldatum. Azure is alleen primaire openbare cloud serviceprovider om te winnen van deze certificeringsinstantie. Voor meer informatie vindt u de LUIS opgenomen in de bijgewerkte scope-instructie in de Azure main [naleving overzicht](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) document waarnaar wordt verwezen in [Vertrouwenscentrum](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) ISO-pagina's.  

## <a name="next-steps"></a>Volgende stappen

Zie [Best Practices](luis-concept-best-practices.md) voor informatie over het gebruik van intents en entiteiten voor de beste voorspellingen.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website