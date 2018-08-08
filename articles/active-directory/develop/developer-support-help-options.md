---
title: Opties voor ondersteuning en hulp voor ontwikkelaars van Azure-identiteit | Microsoft Docs
description: Weet u het verkrijgen van help en ondersteuning voor vragen met betrekking tot ontwikkeling en problemen bij het maken van toepassingen die kunnen worden geïntegreerd met Microsoft Azure-identiteiten (Azure Active Directory en MSA)
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/27/2017
ms.author: celested
ms.reviewer: andret
ms.custom: aaddev
ms.openlocfilehash: 9b2ccce4ef6fad4f7710ba2092d67ac5f4e73b71
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39602009"
---
# <a name="support-and-help-options-for-developers"></a>Opties voor ondersteuning en hulp voor ontwikkelaars 

Ongeacht of u net aan de slag om te integreren met Azure Active Directory, Microsoft identiteiten of Microsoft Graph-API, of wanneer u een nieuwe functie voor uw toepassing implementeert, u moet Help-informatie verkrijgen uit de community of inzicht in hoe vaak er zijn de ondersteunt opties die u als ontwikkelaar hebt. In dit artikel helpt u bij het begrijpen van deze opties, onder een samenvatting:

> [!div class="checklist"]
> * Zoeken om te controleren als de vraag van uw probleem niet is beantwoord door de community, of als een bestaande documentatie voor de functie u wilt implementeren, al bestaat
> * In sommige gevallen wilt u alleen tot het gebruik van onze-ondersteuningsprogramma's om op te sporen een specifiek probleem
> * Als u het antwoord van wat u nodig hebt niet kunt vinden, kunt u een vraag stellen op *Stack Overflow*
> * Als u een probleem met een van onze verificatiebibliotheken kunt vinden, moet u een *GitHub* probleem
> * Ten slotte, als u nodig hebt om te communiceren met iemand anders, kunt u om een ondersteuningsaanvraag te openen


## <a name="search"></a>Search

Hebt u een vraag met betrekking tot ontwikkeling, u mogelijk het antwoord u op onze documentatie moet vinden onze [github-voorbeelden](https://github.com/azure-samples), of antwoorden op [Stack Overflow](https://www.stackoverflow.com) vragen.

### <a name="scoped-search"></a>Afgebakende zoekopdracht
Voor snellere resultaten kunt u uw zoekopdracht naar Stack Overflow, onze documentatie en voorbeelden van onze code beperken met behulp van de volgende op uw [favoriete zoekmachine](https://bing.com):
```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/en-us/graph)
```
Waar *{uw zoektermen}* uw zoekwoorden is.
<br/>

## <a name="use-our-development-support-tools"></a>Gebruik onze ondersteuning ontwikkelhulpprogramma 's

|Hulpprogramma  |Beschrijving  |
|---------|---------|
|[jwt.MS](https://jwt.ms)| Plak een ID of toegang tot tokens voor het decoderen van de claims-namen en waarden |
|[Fout code analyzer](https://apps.dev.microsoft.com/portal/tools/errors)| Plak de foutcode van een ontvangen tijdens het aanmelden of toestemming geven pagina's om te zien van de mogelijke oorzaken en herstel |
|[Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)| Hulpprogramma waarmee u kunt aanvragen en antwoorden op basis van de Microsoft Graph API bekijken|

<br/>

[![Stack Overflow](./media/developer-support-help-options/stackoverflow-logo.png)](https://www.stackoverflow.com)
## <a name="post-a-question-to-stack-overflow"></a>Plaats een vraag in Stack Overflow

Stack Overflow is het aanbevolen kanaal voor vragen met betrekking tot ontwikkeling - waar leden van de community als Microsoft-teamleden direct betrokken zijn op het helpen u om uw probleem te verhelpen.

Als u een antwoord op uw probleem via zoekopdrachten niet vinden, verzendt u een nieuwe vraag naar Stack Overflow: gebruik een van de volgende codes bij het maken van vragen waarmee de community identificeren, klikt u vervolgens uw vraag op elk gewenst moment beantwoorden:

|Onderdeel/gebied  |Tags  |
|---------|---------|
|ADAL-bibliotheek |[[adal]](http://stackoverflow.com/questions/tagged/adal)|
|Bibliotheek met MSAL     |[[msal]](http://stackoverflow.com/questions/tagged/msal)|
|OWIN-middleware  |[[azure active directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)|
|[Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  |[[azure ad b2b]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  |[[azure-ad-b2c]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Microsoft Graph API](https://developer.microsoft.com/graph/) |[[microsoft graph]](http://stackoverflow.com/questions/tagged/microsoft-graph)
|Een andere gebied met betrekking tot verificatie of autorisatie-onderwerpen |[[azure active directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)
<br/>
> [!TIP]
> De volgende posts in Stack Overflow bevatten tips over het maken van vragen en tips over het toevoegen van broncode - volgen van deze richtlijnen kunnen helpen verhogen van de kans op leden van de community om te beoordelen en snel reageren op uw vraag:  
> - [Hoe ik een vraag stellen goed](https://stackoverflow.com/help/how-to-ask)
> - [Over het maken van een minimale, volledig en controleerbare voorbeeld](https://stackoverflow.com/help/mcve)

<br/>


[![Stack Overflow](./media/developer-support-help-options/github-logo.png)](https://www.github.com)
## <a name="create-a-github-issue"></a>Een GitHub-probleem maken

 Als u een bug of een probleem met betrekking tot onze bibliotheken kunt vinden, verhoogt u het probleem op onze GitHub-opslagplaatsen. Omdat onze bibliotheken open-source zijn, bent u ook naar een pull-aanvraag indient. Het volgende artikel bevat een lijst van bibliotheken en hun GitHub-opslagplaatsen:

- [ADAL, MSAL en Owin-middleware](active-directory-authentication-libraries.md) bibliotheken en GitHub-opslagplaatsen

<br/>

## <a name="open-a-support-request"></a>Open een ondersteuningsaanvraag

Als u nodig hebt om te communiceren met iemand anders, kunt u een ondersteuningsaanvraag openen. Als u een Azure-klant bent, zijn er verschillende opties voor ondersteuning beschikbaar. Als u wilt vergelijken plannen, Zie [deze pagina](https://azure.microsoft.com/support/plans/). Developer-ondersteuning is ook beschikbaar voor Azure-klanten. Zie voor meer informatie over het kopen van ondersteuningsabonnementen voor ontwikkelaars [deze pagina](https://azure.microsoft.com/support/plans/developer/).

- Als u al een Azure-Ondersteuningsabonnement, [hier een ondersteuningsaanvraag openen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

- Als u geen Azure-klant bent, kunt u ook een ondersteuningsaanvraag openen met Microsoft via [onze commerciële ondersteuning](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial).

U kunt ook proberen [onze virtuele agent](https://support.microsoft.com/contactus/?ws=support) te verkrijgen van ondersteuning of vragen stellen.

### <a name="free-chat-support-for-a-limited-time"></a>Chatondersteuning voor een beperkte periode gratis

U kunt ook onze chatsupport - die gedurende een beperkte periode gratis voor Microsoft-Partners gebruiken. Als uw bedrijf niet een Microsoft-Partner is, kunt u gratis registreren en andere voordelen verkrijgen door te gaan [hier](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx).

Na het registreren van uw bedrijf, kunt u de chat-aanvraag starten [hier](https://aka.ms/devchat).