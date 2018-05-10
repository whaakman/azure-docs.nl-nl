---
title: Renderen dekking van Azure Maps | Microsoft Docs
description: Meer informatie over Render dekking van Azure Maps
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 03/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: ab05277c4541ae859f79b1108c4cf8a7beb29271
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="azure-maps-render-coverage"></a>Azure Maps weergeven dekking

Azure Maps maakt gebruik van zowel raster tegels en vector tegels om toewijzingen te maken. Met de laagste resolutie wordt de hele wereld op één venster past. Met de hoogste resolutie vertegenwoordigt één tegel 38 vierkante meters. Als u op een kaart inzoomen, daarom kunt u zien steeds meer informatie over continenten, regio's, plaatsen en afzonderlijke streets. Zie voor meer informatie [niveaus zoomen en raster tegel](zoom-levels-and-tile-grid.md).

Maps heeft echter geen hetzelfde niveau van informatie en nauwkeurigheid voor alle regio's. De volgende tabellen bevatten informatie over welk niveau van de gerenderde details die u van elke regio verwachten kunt.

## <a name="legend"></a>Legenda

| Symbool | Betekenis |
|--------|---------|
| ✓ | Regio wordt aangeduid met gedetailleerde gegevens.   |
| Ø | Regio wordt aangeduid met vereenvoudigde gegevens. |


## <a name="africa"></a>Afrika 


| Regio | Raster tegels Unified | Vector tegels Unified |
| ------ | :------------------: | :------------------: |
| Algerije                          | ✓ | ✓ |
| Angola                           | ✓ | ✓ |
| Benin                            | ✓ | ✓ |
| Botswana                         | ✓ | ✓ |
| Burkina Faso                     | ✓ | ✓ |
| Burundi                          | ✓ | ✓ |
| Kaapverdië                       |   | ✓ |
| Kameroen                         | ✓ | ✓ |
| Centraal-Afrikaanse Republiek         |   | Ø |
| Tsjaad                             |   | Ø |
| Comoren                          |   | Ø |
| Congo                            | ✓ | ✓ |
| Congo-Kinshasa | ✓ | ✓ |
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
| Guinee-Bissau                    |   | Ø |
| Kenia                            | ✓ | ✓ |
| Lesotho                          | ✓ | ✓ |
| Liberia                          |   | Ø |
| Libië                            |   | Ø |
| Madagaskar                       |   | Ø |
| Malawi                           | ✓ | ✓ |
| Mali                             | ✓ | ✓ |
| Mauritanië                       | ✓ | ✓ |
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
| São Tomé en Príncipe            |   | Ø |
| Senegal                          | ✓ | ✓ |
| Sierra Leone                     |   | Ø |
| Somalië                          |   | Ø |
| Zuid-Afrika                     | ✓ | ✓ |
| Zuid-Sudan                      |   | Ø |
| Soedan                            |   | Ø |
| Swaziland                        | ✓ | ✓ |
| Verenigde Tanzania      | ✓ | ✓ |
| Togo                             | ✓ | ✓ |
| Tunesië                          | ✓ | ✓ |
| Uganda                           | ✓ | ✓ |
| Zambia                           | ✓ | ✓ |
| Zimbabwe                         | ✓ | ✓ |

## <a name="americas"></a>Noord- en Zuid-Amerika

| Regio | Raster tegels Unified | Vector tegels Unified |
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
| Saint to (Frans)     | ✓ | ✓ |
| Saint-Pierre en Miquelon |   | ✓ |
| Saint Vincent en de Grenadines | ✓ | ✓ |
| Sint Maarten (Nederlands)      | ✓ | ✓ |
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

| Regio | Raster tegels Unified | Vector tegels Unified |
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
| Oost-Timor               |   | Ø |
| Turkmenistan              |   | Ø |
| Verenigde Arabische Emiraten      | ✓ | ✓ |
| Amerikaanse ondergeschikte afgelegen eilanden |   | Ø |
| Oezbekistan                |   | Ø |
| Vietnam                   | ✓ | ✓ |
| Jemen                     | ✓ | ✓ |

## <a name="oceania"></a>Oceanië

| Regio | Raster tegels Unified | Vector tegels Unified |
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
| Norfolk            |   | Ø |
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

| Regio | Raster tegels Unified | Vector tegels Unified |
| ------ | :------------------: | :------------------: |
| Albanië                   | ✓ | ✓ |
| Andorra                   | ✓ | ✓ |
| Armenië                   |   | Ø |
| Oostenrijk                   | ✓ | ✓ |
| Azerbeidzjan                |   | Ø |
| Wit-Rusland                   | Ø | ✓ |
| België                   | ✓ | ✓ |
| Bosnië-Herzegovina        | ✓ | ✓ |
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
| Heilige Stoel (Vaticaanstad)              | ✓ | ✓ |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Maps rendering [niveaus zoomen en raster tegel](zoom-levels-and-tile-grid.md).

Meer informatie over de [dekking gebieden voor de service Routering Maps](routing-coverage.md). 