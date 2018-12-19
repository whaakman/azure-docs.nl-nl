---
title: Dekking van Azure Maps voor weergave | Microsoft Docs
description: Meer informatie over de Render-dekking van Azure Maps
author: jingjing-z
ms.author: jinzh
ms.date: 03/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 6e4ebd5bfd7225537046d34dd885d04e8a94878f
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "42058672"
---
# <a name="azure-maps-render-coverage"></a>Azure Maps dekking voor weergave

Azure kaarten maakt gebruik van zowel tegels rasterafbeeldingen vectortegels om toewijzingen te maken. De laagste resolutie past de hele wereld op een enkele tegel. Met de hoogste resolutie vertegenwoordigt een enkele tegel 38 vierkante meter. Als u op een kaart inzoomen, dus kunt u steeds meer details weergeven over continenten, regio's, plaatsen en afzonderlijke Streets (straten). Zie voor meer informatie, [zoomniveaus en tegelraster](zoom-levels-and-tile-grid.md).

Maps hoeft echter niet dezelfde mate van informatie en de nauwkeurigheid van de gegevens voor alle regio's. De volgende tabellen bevatten informatie over welk niveau van de weergegeven gegevens die u van elke regio verwachten kunt.

## <a name="legend"></a>Legenda

| Symbool | Betekenis |
|--------|---------|
| ✓ | Regio wordt weergegeven met gedetailleerde gegevens.   |
| Ø | Regio wordt weergegeven met vereenvoudigde. |


## <a name="africa"></a>Afrika 


| Regio | Raster tegels Unified | Unified Vectortegels |
| ------ | :------------------: | :------------------: |
| Algerije                          | ✓ | ✓ |
| Angola                           | ✓ | ✓ |
| Benin                            | ✓ | ✓ |
| Botswana                         | ✓ | ✓ |
| Burkina Faso                     | ✓ | ✓ |
| Burundi                          | ✓ | ✓ |
| Cabo Verde                       |   | ✓ |
| Kameroen                         | ✓ | ✓ |
| Centraal-Afrikaanse Republiek         |   | Ø |
| Tsjaad                             |   | Ø |
| Comoren                          |   | Ø |
| Congo-Brazzaville                            | ✓ | ✓ |
| Congo (DRC) | ✓ | ✓ |
| Ivoorkust                    |   | Ø |
| Djibouti                         |   | Ø |
| Egypte                            | ✓ | ✓ |
| Equatoriaal-Guinea                |   | Ø |
| Eritrea                          |   | Ø |
| Ethiopië                         |   | Ø |
| Gabon                            | ✓ | ✓ |
| Gambia                           |   | Ø |
| Ghana                            | ✓ | ✓ |
| Guinee                           |   | Ø |
| Guinea-Bissau                    |   | Ø |
| Kenia                            | ✓ | ✓ |
| Lesotho                          | ✓ | ✓ |
| Liberia                          |   | Ø |
| Libië                            |   | Ø |
| Madagascar                       |   | Ø |
| Malawi                           | ✓ | ✓ |
| Mali                             | ✓ | ✓ |
| Mauretanië                       | ✓ | ✓ |
| Mauritius                        | ✓ | ✓ |
| Mayotte                          | ✓ | ✓ |
| Marokko                          | ✓ | ✓ |
| Mozambique                       | ✓ | ✓ |
| Namibië                          | ✓ | ✓ |
| Niger                            | ✓ | ✓ |
| Nigeria                          | ✓ | ✓ |
| Réunion                          | ✓ | ✓ |
| Rwanda                           | ✓ | ✓ |
| Sint-Helena, Ascension en Tristan da Cunha |   | Ø |
| Sao Tomé en Principe            |   | Ø |
| Senegal                          | ✓ | ✓ |
| Sierra Leone                     |   | Ø |
| Somalië                          |   | Ø |
| Zuid-Afrika                     | ✓ | ✓ |
| Zuid-Sudan                      |   | Ø |
| Soedan                            |   | Ø |
| Swaziland                        | ✓ | ✓ |
| Verenigde Republiek van Tanzania      | ✓ | ✓ |
| Togo                             | ✓ | ✓ |
| Tunesië                          | ✓ | ✓ |
| Uganda                           | ✓ | ✓ |
| Zambia                           | ✓ | ✓ |
| Zimbabwe                         | ✓ | ✓ |

## <a name="americas"></a>Noord- en Zuid-Amerika

| Regio | Raster tegels Unified | Unified Vectortegels |
| ------ | :------------------: | :------------------: |
| Anguilla                  | ✓ | ✓ |
| Antigua en Barbuda       | ✓ | ✓ |
| Argentinië                 | ✓ | ✓ |
| Aruba                     | ✓ | ✓ |
| Bahama's                   | ✓ | ✓ |
| Barbados                  | ✓ | ✓ |
| Belize                    | ✓ | ✓ |
| Bermuda                   |   | ✓ |
| Plurinationale staat Bolivia |   | ✓ |
| Bonaire, Sint Eustatius en Saba |   | ✓ |
| Brazilië                    | ✓ | ✓ |
| Canada                    | ✓ | ✓ |
| Kaaimaneilanden            | ✓ | ✓ |
| Chili                     | ✓ | ✓ |
| Clipperton-eiland         |   | ✓ |
| Colombia                  | ✓ | ✓ |
| Costa Rica                |   | ✓ |
| Cuba                      | ✓ | ✓ |
| Curaçao                   | ✓ | ✓ |
| Dominica                  | ✓ | ✓ |
| Dominicaanse Republiek        | ✓ | ✓ |
| Ecuador                   |   | ✓ |
| Falklandeilanden |   | ✓ |
| Frans-Guyana             | ✓ | ✓ |
| Groenland                 |   | Ø |
| Grenada                   | ✓ | ✓ |
| Guadeloupe                | ✓ | ✓ |
| Guatemala                 |   | ✓ |
| Guyana                    | ✓ | ✓ |
| Haïti                     | ✓ | ✓ |
| Honduras                  | ✓ | ✓ |
| Jamaica                   | ✓ | ✓ |
| Martinique                | ✓ | ✓ |
| Mexico                    | ✓ | ✓ |
| Montserrat                | ✓ | ✓ |
| Nicaragua                 | ✓ | ✓ |
| Noordelijke Marianen  |   | ✓ |
| Panama                    | ✓ | ✓ | 
| Paraguay                  |   | ✓ |
| Peru                      | ✓ | ✓ |
| Puerto Rico               | ✓ | ✓ |
| Quebec (Canada)           |   | ✓ |
| Saint-Barthélemy          | ✓ | ✓ |
| Saint Kitts en Nevis     | ✓ | ✓ |
| Saint Lucia               | ✓ | ✓ |
| Saint-Martin (Franse)     | ✓ | ✓ |
| Saint-Pierre en Miquelon |   | ✓ |
| Saint Vincent en de Grenadines | ✓ | ✓ |
| Sint-Maarten (Nederlands)      | ✓ | ✓ |
| Zuid-Georgia en de Zuidelijke Sandwicheilanden |   | ✓ |
| Suriname                  |   | ✓ |
| Trinidad en Tobago       | ✓ | ✓ |
| Turks- en Caicoseilanden  | ✓ | ✓ |
| Verenigde Staten             | ✓ | ✓ |
| Uruguay                   | ✓ | ✓ |
| Venezuela                 | ✓ | ✓ |
| Britse Maagdeneilanden   | ✓ | ✓ |
| Amerikaanse Maagdeneilanden      | ✓ | ✓ |

## <a name="asia"></a>Azië 

| Regio | Raster tegels Unified | Unified Vectortegels |
| ------ | :------------------: | :------------------: |
| Afghanistan               |   | Ø |
| Bahrein                   | ✓ | ✓ |
| Bangladesh                |   | Ø |
| Bhutan                    |   | Ø |
| Brits Indische Oceaanterritorium |   | Ø |
| Brunei                    | ✓ | ✓ |
| Cambodja                  |   | Ø |
| China                     |   | Ø |
| Cocos-Keeling-eilanden   |   | Ø |
| Noord-Korea |   | Ø |
| Dokdo en Takeshima       |   | Ø |
| Hongkong                 | ✓ | ✓ |
| Indonesië                 | ✓ | ✓ |
| Iran                      |   | Ø |
| Irak                      | ✓ | ✓ |
| Israël                    |   | ✓ |
| Japan                     |   | Ø |
| Jordanië                    | ✓ | ✓ |
| Kazachstan                |   | ✓ |
| Koeweit                    | ✓ | ✓ |
| Kirgizië                |   | Ø |
| Laos |   | Ø |
| Libanon                   | ✓ | ✓ |
| Macao                     | ✓ | ✓ |
| Maleisië                  | ✓ | ✓ |
| Maldiven                  |   | Ø |
| Mongolië                  |   | Ø |
| Myanmar                   |   | Ø |
| Napal                     |   | Ø |
| Oman                      | ✓ | ✓ |
| Pakistan                  |   | Ø |
| Filippijnen               | ✓ | ✓ |
| Qatar                     | ✓ | ✓ |
| Republiek Korea         | ✓ | Ø |
| Saoedi-Arabië              | ✓ | ✓ |
| Senkaku-eilanden           |   | ✓ |
| Singapore                 | ✓ | ✓|
| Sri Lanka                 |   | Ø |
| Syrië      |   | Ø |
| Taiwan                    | ✓ | ✓ |
| Tadzjikistan                |   | Ø |
| Thailand                  | ✓ | ✓ |
| Democratische republiek Timor-Leste               |   | Ø |
| Turkmenistan              |   | Ø |
| Verenigde Arabische Emiraten      | ✓ | ✓ |
| Amerikaanse ondergeschikte afgelegen eilanden |   | Ø |
| Oezbekistan                |   | Ø |
| Vietnam                   | ✓ | ✓ |
| Jemen                     | ✓ | ✓ |

## <a name="oceania"></a>Oceanië

| Regio | Raster tegels Unified | Unified Vectortegels |
| ------ | :------------------: | :------------------: |
| Amerikaans-Samoa            |   | ✓ |
| Australië                 | ✓ | ✓ |
| Cookeilanden              |   | Ø |
| Fiji                      |   | Ø |
| Frans-Polynesië          |   | Ø |
| Guam                      | ✓ | ✓ |
| Kiribati                  |   | Ø |
| Marshalleilanden          |   | Ø |
| Micronesia                |   | Ø |
| Nauru                     |   | Ø |
| Nieuw-Caledonië             |   | Ø |
| Nieuw-Zeeland               | ✓ | ✓ |
| Niue                      |   | Ø |
| Norfolkeiland            |   | Ø |
| Palau                     |   | Ø |
| Papoea-Nieuw-Guinea          |   | Ø |
| Pitcairn                  |   | Ø |
| Samoa                     |   | Ø |
| Salomonseilanden           |   | Ø|
| Tokelau                   |   | Ø |
| Tonga                     |   | Ø |
| Tuvalu                    |   | Ø |
| Vanuatu                   |   | Ø |
| Wallis en Futuna         |   | Ø |


## <a name="europe"></a>Europa

| Regio | Raster tegels Unified | Unified Vectortegels |
| ------ | :------------------: | :------------------: |
| Albanië                   | ✓ | ✓ |
| Andorra                   | ✓ | ✓ |
| Armenië                   |   | Ø |
| Oostenrijk                   | ✓ | ✓ |
| Azerbeidzjan                |   | Ø |
| Wit-Rusland                   | Ø | ✓ |
| België                   | ✓ | ✓ |
| Bosnische        | ✓ | ✓ |
| Bulgarije                  | ✓ | ✓ |
| Kroatië                   | ✓ | ✓ |
| Cyprus                    | ✓ | ✓ |
| Tsjechië            | ✓ | ✓ |
| Denemarken                   | ✓ | ✓ |
| Estland                   | ✓ | ✓ |
| Faeröer             |   | Ø |
| Finland                   | ✓ | ✓ |
| Frankrijk                    | ✓ | ✓ |
| Georgië                   |   | Ø |
| Duitsland                   | ✓ |✓ |
| Gibraltar                 | ✓ |   |
| Griekenland                    | ✓ | ✓ |
| Guernsey                  |   | ✓ |
| Hongarije                   | ✓ | ✓ |
| IJsland                   | ✓ | ✓ |
| Ierland (Republiek)     | ✓ | ✓ |
| Man               |   | ✓ |
| Italië                     | ✓ | ✓ |
| Jan Mayen                 |   | ✓ |
| Jersey                    |   | ✓ |
| Letland                    | ✓ | ✓ |
| Liechtenstein             | ✓ | ✓ |
| Litouwen                 | ✓ | ✓ |
| Luxemburg                | ✓ | ✓ |
| Macedonië                 | ✓ | ✓ |
| Malta                     | ✓ | ✓ |
| Moldavië                   | ✓ | ✓ |
| Monaco                    | ✓ | ✓ |
| Montenegro                | ✓ | ✓ |
| Nederland               | ✓ | ✓ |
| Noorwegen                    | ✓ | ✓ |
| Polen                    | ✓ | ✓ |
| Portugal                  | ✓ | ✓ |
| Roemenië                   | ✓ | ✓ |
| Russische Federatie        | ✓ | ✓ |
| San Marino                | ✓ | ✓ |
| Servië                    | ✓ | ✓ |
| Slowakije                  | ✓ | ✓ |
| Slovenië                  | ✓ | ✓ |
| Zuidelijke Kurils           |   | ✓ |
| Spanje                     | ✓ | ✓ |
| Spitsbergen                  |   | ✓ |
| Zweden                    | ✓ |   |
| Zwitserland               | ✓ | ✓ |
| Turkije                    | ✓ | ✓ |
| Oekraïne                   | ✓ | ✓ |
| Verenigd Koninkrijk            | ✓ | ✓ |
| Vaticaanstad              | ✓ | ✓ |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Maps rendering, [zoomniveaus en tegelraster](zoom-levels-and-tile-grid.md).

Meer informatie over de [dekking gebieden voor de service Routering kaarten](routing-coverage.md). 