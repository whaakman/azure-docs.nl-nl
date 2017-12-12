---
title: Opties voor ondersteuning en hulp voor ontwikkelaars voor Azure Identity | Microsoft Docs
description: "Weet u het verkrijgen van help en ondersteuning voor ontwikkeling-gerelateerde vragen en problemen bij het maken van de toepassing die kunnen worden geïntegreerd met Microsoft Azure-identiteiten (Azure Active Directory en beheerde Serviceaccounts)"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/27/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 06da07aa699d19602449dc365abb971867214a31
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="support-and-help-options-for-developers"></a>Opties voor ondersteuning en hulp voor ontwikkelaars 

Ongeacht of u net begint te integreren met Azure Active Directory, identiteiten van Microsoft of Microsoft Graph API of er tijden die u nodig hebt u hulp nodig van de community of te begrijpen wanneer u een nieuwe functie voor uw toepassing implementeert, zijn de ondersteuning voor opties die u als ontwikkelaar. In dit artikel helpt u bij het begrijpen van onderstaande een samenvatting van deze opties:

> [!div class="checklist"]
> * Zoeken om te controleren als uw probleem vraag niet is beantwoord door de community of als een bestaande documentatie voor de functie u wilt implementeren, al bestaat
> * In sommige gevallen wilt u alleen gebruiken van onze ondersteuningsprogramma's waarmee u kunt fouten opsporen in een specifiek probleem
> * Als u niet kunt het antwoord vinden van wat u nodig hebt, kunt u een vraag stellen op *Stack Overflow*
> * Als u een probleem met een van onze verificatiebibliotheken vinden, verhogen een *GitHub* probleem
> * Ten slotte als u met iemand praten wilt, kunt u een ondersteuningsaanvraag openen


## <a name="search"></a>Search

Als u een vraag ontwikkeling-gerelateerde hebt, u mogelijk het antwoord u op onze documentatie moet vinden onze [github voorbeelden](https://github.com/azure-samples), of antwoorden op [Stack Overflow](https://www.stackoverflow.com) vragen.

### <a name="scoped-search"></a>Afgebakende zoekopdracht
Voor snellere resultaten kunt u de zoekopdracht op Stack Overflow onze documentatie en onze codevoorbeelden beperken met behulp van de volgende op uw [favoriete zoekmachine](https://bing.com):
```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/en-us/graph)
```
Waar *{uw zoektermen}* is uw zoekwoorden.
<br/>

## <a name="use-our-development-support-tools"></a>Gebruik onze ontwikkel-ondersteuningsprogramma 's

|Hulpprogramma  |Beschrijving  |
|---------|---------|
|[jwt.MS](https://jwt.ms)| Een ID of toegang tot tokens voor het decoderen van de claims namen en waarden plakken |
|[Fout code analyzer](https://apps.dev.microsoft.com/portal/tools/errors)| Plak de foutcode van een ontvangen tijdens het aanmelden of toestemming van pagina's om te zien van de mogelijke oorzaken en herstelbewerkingen |
|[Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)| Hulpprogramma waarmee u aanvragen en antwoorden op basis van de Microsoft Graph API Zie|

<br/>

[![Stack-overloop](media/active-directory-develop-help-support/stackoverflow-logo.png)](https://www.stackoverflow.com)
## <a name="post-a-question-to-stack-overflow"></a>Een vraag posten naar Stack Overflow

Stack-overloop is het aanbevolen kanaal voor ontwikkeling-gerelateerde vragen - waar beide leden van de community Microsoft-teamleden zijn direct betrokken helpt u het probleem kunt oplossen.

Als u een antwoord op uw probleem via zoekopdrachten niet kunt vinden, een nieuwe vraag tot een stackoverloop verzenden: gebruik een van de volgende codes bij het maken van vragen om u te helpen bij het identificeren, community vervolgens uw vraag op tijdige wijze beantwoorden:

|Onderdeel/gedeelte  |Tags  |
|---------|---------|
|ADAL-bibliotheek |[[adal]](http://stackoverflow.com/questions/tagged/adal)|
|MSAL-bibliotheek     |[[msal]](http://stackoverflow.com/questions/tagged/msal)|
|OWIN middleware  |[[azure active directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)|
|[Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  |[[azure ad b2b]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  |[[azure ad b2c]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Microsoft Graph API](https://developer.microsoft.com/graph/) |[[microsoft graph]](http://stackoverflow.com/questions/tagged/microsoft-graph)
|Ieder ander gebied met betrekking tot verificatie of autorisatie-onderwerpen |[[azure active directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)
<br/>
> [!TIP]
> De volgende berichten uit de Stack Overflow bevatten tips voor het maken van vragen en tips voor het toevoegen van broncode - deze richtlijnen kunnen u helpen vergroot de kans dat voor leden van de community om te beoordelen en snel reageren op uw vraag:  
> - [Hoe ik een goede vraag stellen](https://stackoverflow.com/help/how-to-ask)
> - [Het maken van een minimale Complete voorbeeld, en geverifieerd](https://stackoverflow.com/help/mcve)

<br/>


[![Stack-overloop](media/active-directory-develop-help-support/github-logo.png)](https://www.github.com)
## <a name="create-a-github-issue"></a>Een GitHub-probleem maken

 Als u een fout of probleem in verband met onze bibliotheken vinden, afkomstig van een probleem op onze GitHub-opslagplaatsen. Omdat onze bibliotheken open-source, bent u ook gratis een pull-aanvraag indienen. Het volgende artikel bevat een lijst met tapewisselaars en de GitHub-opslagplaatsen:

- [ADAL MSAL en Owin middleware](active-directory-authentication-libraries.md) bibliotheken en GitHub-opslagplaatsen

<br/>

## <a name="open-a-support-request"></a>Een ondersteuningsaanvraag openen

Als u met iemand praten wilt, kunt u een ondersteuningsaanvraag openen. Als u een Azure-klant bent, zijn er diverse opties beschikbaar. Als u wilt vergelijken plannen, Zie [deze pagina](https://azure.microsoft.com/support/plans/). Ondersteuning voor ontwikkelaars is ook beschikbaar voor Azure-klanten. Zie voor meer informatie over het kopen van ontwikkelaars ondersteuningsplannen [deze pagina](https://azure.microsoft.com/support/plans/developer/).

- Als u al een Azure ondersteuning van plan bent, [hier een ondersteuningsaanvraag openen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

- Als u niet een Azure-klant bent, kunt u ook een ondersteuningsaanvraag openen met Microsoft via [onze commerciële ondersteuning](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial).

U kunt ook proberen [onze virtuele agent](https://support.microsoft.com/contactus/?ws=support) te verkrijgen van ondersteuning of vragen.

### <a name="free-chat-support-for-a-limited-time"></a>Gratis chatondersteuning voor een beperkte periode

U kunt ook onze support chat - dat voor een beperkte tijd vrij voor Microsoft-Partners gebruiken. Als uw bedrijf niet gelijk is aan een Microsoft-Partner, kunt u gratis inschrijven en andere voordelen verkrijgen door te gaan [hier](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx).

Nadat het inschrijven van uw bedrijf, kunt u de aanvraag chat starten [hier](https://aka.ms/devchat).