---
title: Dekking voor geocodering van Azure Maps | Microsoft Docs
description: Meer informatie over de dekking voor Geocodering van Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 03/22/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a5e5f4ab286289e223a2fe10ff8cf45f43309f04
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65785941"
---
# <a name="azure-maps-geocoding-coverage"></a>Azure Maps-dekking voor geocodering

Wanneer u een locatie met Azure Maps zoekt, wordt de search-service neemt van uw zoektermen en retourneert de breedtegraad en lengtegraad coördinaten, een proces dat geocodering wordt genoemd. Maps hoeft echter niet dezelfde mate van informatie en de nauwkeurigheid van de gegevens voor alle regio's en andere landen. Gebruik dit artikel om te bepalen wat voor soort locaties kunt u op betrouwbare wijze zoeken naar in elke regio. 

De mogelijkheid om geocode in een land/regio is afhankelijk van de gegevens onderweg dekking en de nauwkeurigheid van de geocodering van de service voor geocodering. De volgende categorieën worden gebruikt. Geef het niveau van ondersteuning voor geocodering in elk land/regio.
* **Adres van de punten** -adressen gegevens kan worden omgezet in een breedtegraad/lengtegraad coördinaat binnen de adres-pakket (eigenschap grens). Soms aangeduid als 'Dakantenne' nauwkeurige. Dit is de hoogste mate van nauwkeurigheid is beschikbaar voor adressen. 
* **Getallen bevatten** -adressen zijn geïnterpoleerde naar een breedtegraad/lengtegraad-coördinaat op straat.
* **Naam van de straat niveau** -adressen worden omgezet in de breedtegraad/lengtegraad-coördinaat van de straat die het adres bevat. Het huisnummer kan niet worden verwerkt.
* **Niveau plaats** -plaats het geocoderen van plaatsnamen worden ondersteund.

## <a name="americas"></a>Noord- en Zuid-Amerika

| Land/regio                                       | Adres verwijst | Huis cijfers | Naam van de straat niveau | Niveau plaats | Punten van belang zijn |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Anguilla                                            |                 |                |              |      ✓     |          ✓         |
| Antarctica                                          |                 |                |              |      ✓     |          ✓         |
| Antigua en Barbuda                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Argentinië                                           |       ✓         |        ✓       |       ✓      |      ✓     |          ✓         |
| Aruba                                               |                 |                |              |      ✓     |          ✓         |
| Bahama's                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Barbados                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Belize                                              |                 |                |              |      ✓     |          ✓         |
| Bermuda                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bolivia                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bonaire, Sint Eustatius en Saba                   |                 |                |              |      ✓     |          ✓         |
| Brazilië                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Canada                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kaaimaneilanden                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Chili                                               |       ✓         |        ✓       |       ✓      |      ✓     |          ✓         |
| Colombia                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Costa Rica                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Cuba                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Dominica                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Dominicana                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Ecuador                                             |                 |                |       ✓      |      ✓     |          ✓         |
| El Salvador                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Falklandeilanden                                    |                 |                |              |      ✓     |          ✓         |
| Frans-Guyana                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Grenada                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Guadeloupe                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Guam                                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Guatemala                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Guyana                                              |                |             |           |      ✓     |                 |
| Haiti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Honduras                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Jamaica                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Martinique                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Mexico                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Montserrat                                          |                 |                |              |      ✓     |          ✓         |
| Nicaragua                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Panama                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Paraguay                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Peru                                                |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Puerto Rico                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Saint-Barthélemy                                    |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Kitts en Nevis                               |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Lucia                                         |                 |                |              |      ✓     |          ✓         |
| Saint Martin                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Saint-Pierre en Miquelon                           |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Vincent en de Grenadines                    |                 |                |              |      ✓     |          ✓         |
| Sint-Maarten                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Zuid-Georgia en de Zuidelijke Sandwicheilanden        |                 |                |              |      ✓     |          ✓         |
| Suriname                                            |                 |                |              |      ✓     |          ✓         |
| Trinidad en Tobago                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Amerikaanse ondergeschikte afgelegen eilanden                |                 |                |              |      ✓     |          ✓         |
| Verenigde Staten van Amerika                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Uruguay                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Venezuela                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Britse Maagdeneilanden                              |                 |                |              |      ✓     |          ✓         |
| V.S. Britse Maagdeneilanden                                 |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |

## <a name="asia-pacific"></a>Azië en Stille Oceaan

| Land/regio                                      | Adres verwijst |Huis cijfers | Naam van de straat niveau | Niveau plaats | Punten van belang zijn |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Amerikaans-Samoa                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Australië                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bangladesh                                          |                 |                |              |      ✓     |          ✓         |
| Bhutan                                              |                 |                |              |      ✓     |          ✓         |
| Brits Indische Oceaanterritorium                      |                 |                |              |      ✓     |          ✓         |
| Brunei                                              |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Cambodja                                            |                 |                |              |      ✓     |          ✓         |
| China                                               |                 |                |              |      ✓     |          ✓         |
| Christmaseiland                                    |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Cocos-Keeling-eilanden                             |                 |                |              |      ✓     |          ✓         |
| Comoren                                             |                 |                |              |      ✓     |          ✓         |
| Cookeilanden                                        |                 |                |              |      ✓     |          ✓         |
| Fiji                                                |                  |                |              |      ✓     |          ✓        |
| Frans-Polynesië                                    |                 |                |              |      ✓     |          ✓         |
| Heard- en McDonaldeilanden                   |                 |                |              |      ✓     |          ✓         |
| Hongkong SAR                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Indonesië                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| India                                               |        ✓        |        ✓       |       ✓      |      ✓     |                   |
| Japan                                               |                 |                |              |      ✓     |          ✓         |
| Kiribati                                            |                 |                |              |      ✓     |          ✓         |
| Korea                                         |                 |                |              |      ✓     |          ✓         |
| Laos                                                |                 |                |              |      ✓     |          ✓         |
| Macau SAR                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Maleisië                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Micronesia                                          |                 |                |              |      ✓     |          ✓         |
| Mongolië                                            |                 |                |              |      ✓     |          ✓         |
| Nauru                                               |                 |                |              |      ✓     |          ✓         |
| Nepal                                               |                 |                |              |      ✓     |          ✓         |
| Nieuw-Caledonië                                       |                 |                |              |      ✓     |          ✓         |
| Nieuw-Zeeland                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Niue                                                |                 |                |              |      ✓     |          ✓         |
| Norfolkeiland                                      |                 |                |              |      ✓     |          ✓         |
| Noord-Korea                                         |                 |                |              |      ✓     |          ✓         |
| Noordelijke Marianen                            |                 |                |       ✓      |      ✓     |          ✓         |
| Pakistan                                            |                 |                |              |      ✓     |          ✓         |
| Palau                                               |                 |                |              |      ✓     |          ✓         |
| Papoea-Nieuw-Guinea                                    |                 |                |              |      ✓     |          ✓         |
| Paracel-eilanden                                     |                 |                |              |      ✓     |                    |
| Filipijnen                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Pitcairneilanden                                            |                 |                |              |      ✓     |          ✓         |
| Samoa                                               |                 |                |              |      ✓     |          ✓         |
| Senkaku-eilanden                                     |        ✓        |                |              |      ✓     |          ✓         |
| Singapore                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Salomonseilanden                                     |                 |                |              |      ✓     |          ✓         |
| Zuidelijke Kurils                                     |        ✓        |                |              |      ✓     |          ✓         |
| Spratly eilanden                                     |                 |                |              |      ✓     |                    |
| Sri Lanka                                           |                 |                |              |      ✓     |          ✓         |
| Taiwan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Thailand                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Tokelau                                             |                 |                |              |      ✓     |          ✓         |
| Tonga                                               |                 |                |              |      ✓     |          ✓         |
| Turks- en Caicoseilanden                            |                 |                |              |      ✓     |          ✓         |
| Tuvalu                                              |                 |                |              |      ✓     |          ✓         |
| Vanuatu                                             |                 |                |              |      ✓     |          ✓         |
| Vietnam                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Wallis en Futuna                                   |                 |                |              |      ✓     |          ✓         |

## <a name="europe"></a>Europa

| Land/regio                                      | Adres verwijst |Huis cijfers | Naam van de straat niveau | Niveau plaats | Punten van belang zijn |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Albanië                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Andorra                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Armenië                                             |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Oostenrijk                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Azerbeidzjan                                          |        ✓        |        ✓       |              |      ✓     |          ✓         |
| België                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bosnia And Herzegovina                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bulgarije                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Belarus                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kroatië                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Cyprus                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Tsjechië                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Denemarken                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Estland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Faeröer                                       |                 |                |              |      ✓     |          ✓         |
| Finland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Frankrijk                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Georgië                                             |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Duitsland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Gibraltar                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Griekenland                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Groenland                                           |                 |                |              |      ✓     |          ✓         |
| Guernsey                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Hongarije                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| IJsland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Ierland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Schitterende botanische tuin van Mallards                                         |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Italië                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Jan Mayen                                           |        ✓        |                |              |      ✓     |          ✓         |
| Jersey                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kazachstan                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kosovo                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Kirgizië                                          |                 |                |              |      ✓     |          ✓         |
| Letland                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Liechtenstein                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Litouwen                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Luxemburg                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Noord Macedonië                                     |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Malta                                               |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Moldavië                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Monaco                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Montenegro                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Nederland                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Noorwegen                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Polen                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Portugal                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| +Azores and Madeira                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Roemenië                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Russische Federatie                                  |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| San Marino                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Servië                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slowakije                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slovenië                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Spanje                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Spitsbergen                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Zweden                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Zwitserland                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Tadzjikistan                                          |                 |                |              |      ✓     |          ✓         |
| Turkije                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Turkmenistan                                        |                 |                |              |      ✓     |          ✓         |
| Oekraïne                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Verenigd Koninkrijk                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Oezbekistan                                          |                 |                |              |      ✓     |          ✓         |
| Vaticaanstad                                        |                 |                |       ✓      |      ✓     |          ✓         |


## <a name="middle-east-and-africa"></a>Midden-Oosten en Afrika

| Land/regio                                      | Adres verwijst |Huis cijfers | Naam van de straat niveau | Niveau plaats | Punten van belang zijn |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Afghanistan                                         |                 |                |              |      ✓     |          ✓         |
| Algerije                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Angola                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Bahrein                                             |        ✓        |       ✓        |       ✓      |      ✓     |          ✓         |
| Benin                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Botswana                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Bouveteiland                                       |                 |                |              |      ✓     |          ✓         |
| Burkina Faso                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Burundi                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Kameroen                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Cabo Verde                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Centraal-Afrikaanse Republiek                            |                 |                |       ✓      |      ✓     |          ✓         |
| Tsjaad                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Congo-Brazzaville                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Ivoorkust                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Congo (DRC)                    |                 |                |       ✓      |      ✓     |          ✓         |
| Djibouti                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Egypte                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Equatoriaal-Guinea, Republiek                      |                 |                |       ✓      |      ✓     |          ✓         |
| Eritrea                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Ethiopië                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Franse Zuidelijke en Zuidpoolgebieden|                        |                |              |      ✓     |          ✓         |
| Gabon                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gambia                                              |                 |                |              |      ✓     |          ✓         |
| Ghana                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Guinee                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Guinee-Bissau                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Iran                                                |                 |                |              |      ✓     |          ✓         |
| Irak                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Israël                                              |        ✓        |       ✓        |              |      ✓     |          ✓         |
| Jordanië                                              |        ✓        |       ✓        |       ✓      |      ✓     |          ✓         |
| Kenia                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Koeweit                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Libanon                                             |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Lesotho                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Liberia                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Libië                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Madagascar                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Malawi                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Maldiven                                            |                 |                |              |      ✓     |          ✓         |
| Mali                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Marshalleilanden                                    |                 |                |              |      ✓     |          ✓         |
| Mauretanië                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Mauritius                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Mayotte                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Marokko                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mozambique                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Myanmar                                             |                 |                |              |      ✓     |          ✓         |
| Namibië                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Niger                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Nigeria                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Oman                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Qatar                                               |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Réunion                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Rwanda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Sint-Helena                                        |                 |                |              |      ✓     |          ✓         |
| Saoedi-Arabië                                        |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Senegal                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Seychellen                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Sierra Leone                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Somalië                                             |                 |                |              |      ✓     |          ✓         |
| Zuid-Afrika                                        |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Zuid-Soedan                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Soedan                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Swaziland                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Syrië                                               |                 |                |              |      ✓     |          ✓         |
| Sao Tomé en principe                               |                 |                |       ✓      |      ✓     |          ✓         |
| Tanzania                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Togo                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Tunesië                                             |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Oeganda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Verenigde Arabische Emiraten                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Jemen                                               |                 |                |              |      ✓     |          ✓         |
| Zambia                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Zimbabwe                                            |                 |                |       ✓      |      ✓     |          ✓         |



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Maps geocodering, de [zoeken](https://docs.microsoft.com/rest/api/maps/search) verwijzen naar pagina's.

Meer informatie over de [dekking gebieden voor de toewijzingen van het verkeer service](traffic-coverage.md). 

