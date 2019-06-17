---
title: Niet-ondersteunde taal implementaties - aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Klik hier voor meer informatie over het implementeren van paren van niet-ondersteunde taal in aangepaste Translator.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/24/2019
ms.author: v-pawal
ms.openlocfilehash: 0938ba2e839be603c557cc2a87dd0c5aa1dfe1e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66390606"
---
# <a name="unsupported-language-deployments"></a>Implementaties in niet-ondersteunde talen

<!--Custom Translator provides the highest-quality translations possible using the latest techniques in neural machine learning. While Microsoft intends to make neural training available in all languages, there are some limitations that prevent us from being able to offer neural machine translation in all language pairs.-->  

Met de geplande buitengebruikstelling van de Microsoft Translator-Hub, heeft Microsoft uitschakeling alle modellen die momenteel zijn geïmplementeerd via de Hub. Veel mensen hebben geïmplementeerd in de Hub waarvan paren taal worden niet ondersteund in aangepaste Translator modellen.  We wilt niet dat gebruikers in dit geval geen beroep voor het omzetten van de inhoud ervan te doen.

We hebben nu een proces waarmee u uw niet-ondersteunde modellen via de aangepaste Translator implementeren.  Dit proces kunt u om door te gaan voor de omzetting van inhoud met behulp van de meest recente V3-API.  Deze modellen worden gehost totdat u ze worden geïmplementeerd of beschikbaar is de combinatie van taal in aangepaste Translator.  In dit artikel wordt uitgelegd dat het proces voor het implementeren van modellen met niet-ondersteunde taal paren.

## <a name="prerequisites"></a>Vereisten

In de volgorde voor uw modellen en kandidaten zijn voor implementatie, moeten ze voldoen aan de volgende criteria:
* Het project met het model moet zijn gemigreerd uit de Hub aan de aangepaste vertaler met behulp van het hulpprogramma voor migratie.  Het proces voor het migreren van projecten en werkruimten vindt [hier](how-to-migrate.md).
* Het model moet zich in de status van de geïmplementeerde als de migratie plaatsvindt.  
* De combinatie van taal van het model moet een sleutelpaar met niet-ondersteunde taal in aangepaste Translator.  Taal paren waarin een taal wordt ondersteund of naar het Engels, maar het paar zelf bevat geen Engels, zijn kandidaten voor implementaties van niet-ondersteunde taal.  Bijvoorbeeld, zelfs al Franse Engelse en Engels naar Duitse zijn ondersteunde taal voor het sleutelpaar met een niet-ondersteunde taal paar wordt beschouwd als een Hub-model van een Frans, Duits gekoppeld.

## <a name="process"></a>Process
Als u modellen hebt gemigreerd van de Hub die geschikt zijn voor implementatie, kunt u deze vinden door te gaan naar de **instellingen** pagina voor uw werkruimte en schuiven naar het einde van de pagina u ziet een **niet ondersteund Translator Hub trainingen** sectie.  In deze sectie wordt alleen weergegeven als u projecten die voldoen aan de vereisten die hierboven worden vermeld.

![Over het migreren van de Hub](media/unsupported-language-deployments/unsupported-translator-hub-trainings.jpg)

Binnen de **niet-ondersteunde Translator Hub trainingen** pagina selectie van de **ongevraagde trainingen** tabblad bevat modellen die in aanmerking komen voor de implementatie.  Selecteer de modellen die u wilt implementeren en een aanvraag indienen.   Vóór de deadline van de 30 April implementatie, kunt u zoveel modellen als u voor de implementatie wilt.
 
![Over het migreren van de Hub](media/unsupported-language-deployments/unsupported-translator-hub-trainings-list.jpg)

Wanneer verzonden, het model niet langer beschikbaar is op de **ongevraagde trainingen** tabblad en in plaats daarvan wordt weergegeven op de **aangevraagd trainingen** tabblad.  U kunt uw aangevraagde trainingen op elk gewenst moment weergeven.

![Over het migreren van de Hub](media/unsupported-language-deployments/request-unsupported-trainings.jpg) 

## <a name="whats-next"></a>Volgende stappen

De modellen die u hebt geselecteerd voor de implementatie worden opgeslagen als de Hub uit bedrijf genomen is en alle modellen ongedaan gemaakt worden.  U hebt tot 24 mei om te verzenden aanvragen voor de implementatie van niet-ondersteunde modellen.  We gaan deze modellen op 15 juni op het moment waarop ze toegankelijk via de API van Translator V3 zijn implementeren.  Bovendien zijn ze beschikbaar via de API V2 tot 1 juli.  

Voor meer informatie over belangrijke datums in de afschaffing van de controle van de Hub [hier](https://www.microsoft.com/translator/business/hub/).
Eenmaal is geïmplementeerd, normale hosting wordt in rekening gebracht.  Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) voor meer informatie.  

In tegenstelling tot standaard aangepaste Translator-modellen zijn Hub modellen alleen beschikbaar in één regio, dus kosten voor het hosten van meerdere regio's zijn niet van toepassing.  Zodra geïmplementeerd, kunt u zich aan het model van uw Hub worden geïmplementeerd op elk gewenst moment via het gemigreerde aangepaste Translator-project.

## <a name="next-steps"></a>Volgende stappen

- [Een model te trainen](how-to-train-model.md).
- Start met behulp van het model van uw geïmplementeerde aangepaste vertaling via [Microsoft Translator Text-API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
