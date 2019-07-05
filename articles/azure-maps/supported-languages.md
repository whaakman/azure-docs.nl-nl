---
title: Lokalisatieondersteuning van Azure Maps | Microsoft Docs
description: Meer informatie over ondersteunde talen voor de services van Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: a9446301cc4bb46c989223ad020c7a8e8b353ad3
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446191"
---
# <a name="localization-support-in-azure-maps"></a>Lokalisatieondersteuning in Azure-kaarten

Azure kaarten biedt ondersteuning voor verschillende talen en weergaven op basis van land/regio. Dit artikel bevat de ondersteunde talen en weergaven om te helpen uw Azure Maps-implementatie.


## <a name="azure-maps-supported-languages"></a>Azure Maps ondersteunde talen

Azure Maps zijn voor services in verschillende talen gelokaliseerd. De volgende tabel bevat de ondersteunde taalcodes voor elke service.  
  

| id         | Name                   |  Kaarten | Search | Routering | Verkeer incidenten | Kaartbesturingselement JS | Tijdzone |
|------------|------------------------|:-----:|:------:|:-------:|:-----------------:|:--------------:|:---------:|
| af-ZA      | Afrikaans              |       |    ✓   |    ✓    |                   |                |     ✓     |
| ar-SA      | Arabisch                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| eu-ES      | Baskisch                 |       |    ✓   |         |                   |                |     ✓     |
| bg-BG      | Bulgaars              |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| CA-ES      | Catalaans                |       |    ✓   |         |                   |                |     ✓     |
| zh-HanS    | Chinees (vereenvoudigd)   |       |  zh-CN |         |                   |                |     ✓     |
| zh-HanT    | Chinees (traditioneel)  | zh-TW |  zh-TW |  zh-TW  |                   |      Zh-TW     |     ✓     |
| hr-HR      | Kroatisch               |       |    ✓   |         |                   |                |     ✓     |
| cs-CZ      | Tsjechisch                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| da-DK      | Deens                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| NL-NL      | Nederlands                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| nl-BE      | Nederlands (België)        |       |    ✓   |         |                   |                |     ✓     |
| en-AU      | Engels (Australië)    |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| NL-NZ      | Engels (Nieuw-Zeeland)  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| en-GB      | Engels (Verenigd Koninkrijk) |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| en-US      | Engels (Verenigde Staten)          |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| et-EE      | Estisch               |       |    ✓   |         |         ✓         |                |     ✓     |
| fi-FI      | Fins                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| fr-FR      | Frans                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| fr-CA      | Frans (Canada)      |       |    ✓   |         |                   |                |     ✓     |
| gl-ES      | Galicisch               |       |    ✓   |         |                   |                |     ✓     |
| de-DE      | Duits                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| el GR      | Grieks                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| hij IL      | Hebreeuws                 |       |    ✓   |         |         ✓         |                |     ✓     |
| hi-IN      | Hindi                  |       |        |         |                   |                |     ✓     |
| hu-HU      | Hongaars              |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| id-ID      | Indonesisch             |   ✓   |    ✓    |    ✓    |         ✓         |        ✓       |     ✓     |
| IT-IT      | Italiaans                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ja-JP      | Japans               |       |        |         |                   |                |     ✓     |
| kk-KZ      | Kazachstaans                 |       |    ✓   |         |                   |                |     ✓     |
| ko-KR      | Koreaans                 |   ✓   |        |    ✓    |                   |        ✓       |     ✓     |
| es-419     | Latijns-Amerika Spaans |       |    ✓   |         |                   |                |     ✓     |
| lv-LV      | Lets                |       |    ✓   |         |         ✓         |                |     ✓     |
| lt-LT      | Litouws             |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ms-MY      | Maleis (Latijns)          |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| nb-NO      | Norwegian Bokmål       |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| NGT        | Neutraal begin waarheid - talen voor alle regio's in lokale scripts, indien beschikbaar |   ✓     |        |         |                   |      ✓          |         |
| NGT-Latn   | Neutraal begin waarheid - Latijnse exonyms. Latijns schrift wordt gebruikt, indien beschikbaar |   ✓     |        |         |                   |        ✓         |          |
| pl-PL      | Pools                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| pt-BR      | Portugees (Brazilië)    |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| pt-PT      | Portugees (Portugal)  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ro-RO      | Roemeens               |       |    ✓    |         |         ✓         |                |     ✓     |
| ru-RU      | Russisch                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| sr-Cyrl-RS | Servisch (Cyrillisch)     |       |    Serbian (Cyrillic) (sr-RS)   |         |                   |                |     ✓     |
| sr-Latn-RS | Servisch (Latijns)        |       |        |         |                   |                |     ✓     |
| sk-SK      | Slowaaks              |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| sl-SL      | Sloveens              |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| es-ES      | Spaans                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| es-MX      | Spaans (Mexico)       |   ✓   |        |    ✓    |                   |        ✓       |     ✓     |
| sv -SE     | Zweeds                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| e-e      | Thais                   |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| tr-TR      | Turks                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| uk-UA      | Oekraïens               |       |    ✓   |         |                   |                |     ✓     |
| vi-VN      | Vietnamees             |       |    ✓   |         |                   |                |     ✓     |


## <a name="azure-maps-supported-views"></a>Azure Maps ondersteunde weergaven

> [!Note]
> We introduceren in de volgende landen/regio's Azure Maps op 1 augustus 2019:
>  * Argentinië
>  * India
>  * Marokko
>  * Pakistan
>
> Na 1 augustus 2019 **weergave** instelling van de parameter wordt de geretourneerde kaartinhoud voor de nieuwe regio's / landen bovengenoemde definiëren. We raden u om ervoor te zorgen dat u hebt ingesteld met de parameter weergeven zoals vereist voor de REST-API's en SDK's die van uw services gebruikmaken.
>  
>
>  **Rest-API's:**
>  
>  Zorg ervoor dat u de parameter weergeven als vereiste hebt ingesteld. Weergave-parameter bepaalt welke set geopolitieke betwiste inhoud wordt geretourneerd via Azure kaarten-services. 
>
>  Betrokken Azure Maps REST-Services:
>    
>    * Tegel Serviceoverzicht ophalen
>    * Kaartafbeelding ophalen 
>    * Zoeken bij benadering ophalen
>    * Search POI ophalen
>    * Ophalen van de categorie POI zoeken
>    * Zoeken in de buurt ophalen
>    * Search-adres ophalen
>    * Gestructureerde zoekopdracht-adres ophalen
>    * Adres zoeken omgekeerde ophalen
>    * Zoeken in omgekeerde Cross straat ophalen
>    * Zoeken binnen geometrie post
>    * Bericht voor Search-adres Batch-Preview
>    * Bericht voor Search-adres omgekeerde Batch-Preview
>    * Zoeken op Route post
>    * Post zoeken bij benadering Batch Preview
>
>    
>  **SDK's:**
>
>  Zorg ervoor dat u de parameter weergeven als vereiste hebt ingesteld en hebt u de nieuwste versie van Web SDK- en Android-SDK. Betrokken SDK's:
>
>    * Azure Maps Web SDK
>    * Azure Maps Android SDK


Azure Maps **weergave** parameter (ook wel 'gebruiker regio parameter' genoemd) is een twee letter ISO-3166 landcode die wordt weergegeven de juiste toewijzingen voor dat land/de regio op te geven welke geopolitieke set inhoud betwist is geretourneerd via Azure kaarten-services, inclusief randen en labels worden weergegeven op de kaart. 

Weergave-parameter is standaard ingesteld op **Unified**, zelfs als u deze nog niet hebt gedefinieerd in de aanvraag. Het is uw verantwoordelijkheid om te bepalen van de locatie van uw gebruikers en stel vervolgens de weergave-parameter correct voor die locatie. U kunt ook hebt u de mogelijkheid om in te stellen ' weergave = Auto', waarmee de kaartgegevens op basis van het IP-adres van de aanvraag wordt geretourneerd.  De parameter weergeven in Azure kaarten-moet worden gebruikt in overeenstemming met van toepassing zijnde wetten, met inbegrip van de betreffende toewijzing van het land waar kaarten, afbeeldingen en andere gegevens en van derden inhoud die u bent gemachtigd om via Azure kaarten-beschikbaar wordt gesteld.


De volgende tabel bevat de ondersteunde weergaven.

| Weergave         | Description                            |  Kaarten | Search | Kaartbesturingselement JS |
|--------------|----------------------------------------|:-----:|:------:|:--------------:|
| AE           | Verenigde Arabische Emiraten (Arabisch weergeven)    |   ✓   |        |     ✓          |
| AR           | Argentinië (Argentijnse weergeven)           |   ✓   |    ✓   |     ✓          |
| BH           | Bahrein (Arabisch weergeven)                 |   ✓   |        |     ✓          |
| IN           | India (Indiase weergeven)                    |   ✓   |   ✓     |     ✓          |
| IQ           | Irak (Arabisch weergeven)                    |   ✓   |        |     ✓          |
| JO           | Jordanië (Arabisch weergeven)                  |   ✓   |        |     ✓          |
| KW           | Koeweit (Arabisch weergeven)                  |   ✓   |        |     ✓          |
| LB           | Libanon (Arabisch weergeven)                 |   ✓   |        |     ✓          |
| MA           | Marokko (Marokkaanse weergeven)                |   ✓   |   ✓     |     ✓          |
| OM           | Oman (Arabisch weergeven)                    |   ✓   |        |     ✓          |
| PK           | Pakistan (Pakistaanse weergeven)              |   ✓   |    ✓    |     ✓          |
| PS           | Palestijnse autoriteit (Arabisch weergeven)    |   ✓   |        |     ✓          |
| QA           | Qatar (Arabisch weergeven)                   |   ✓   |        |     ✓          |
| SA           | Saoedi-Arabië (Arabisch weergeven)            |   ✓   |        |     ✓          |
| SY           | Syrië (Arabisch weergeven)                   |   ✓   |        |     ✓          |
| YE           | Jemen (Arabisch weergeven)                   |   ✓   |        |     ✓          |
| Auto         | Retourneert de kaartgegevens op basis van het IP-adres van de aanvraag.|   ✓   |    ✓   |     ✓          |
| Gecentraliseerd      | Geïntegreerde weergave (andere)                  |   ✓   |   ✓     |     ✓          |
