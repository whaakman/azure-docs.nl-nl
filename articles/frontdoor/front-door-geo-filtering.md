---
title: Geo-filteren op een domein voor Azure Front Door Service | Microsoft Docs
description: In dit artikel maakt u kennis met het geo-filterbeleid voor Azure Front Door Service (AFD)
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/09/2018
ms.author: sharadag
ms.openlocfilehash: e36253600dd8039940209cb5912cb2e4c2e0fbcf
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988594"
---
# <a name="geo-filtering-geographic-based-access-control-to-azure-front-door-service-frontends"></a>Geo-filteren: toegangscontrole op basis van geografie naar AFD-frontends

Standaard reageert AFD op gebruikersaanvragen, ongeacht de locatie van deze gebruiker. In sommige gevallen kunt u echter de toegang tot uw webtoepassingen per land beperken. Dankzij de toepassingslaagbeveiliging van Azure Front Door kunt u met behulp van aangepaste beschermingsregels beleid definiëren voor een specifiek pad op uw eindpunt, zodat toegang vanuit bepaalde landen wordt toegestaan of geblokkeerd. 

Beleid voor toepassingsbescherming omvat gewoonlijk een verzameling aangepaste regels. Een regel bestaat uit voorwaarden voor overeenkomsten, een actie en een prioriteit. In de voorwaarde voor overeenkomst definieert u een overeenkomende variabele, een operator en een overeenkomende waarde.  Voor de ge-filterregel is REMOTE_ADDR de overeenkomende variabele, GeoMatch de operator, en de waarde is het landnummer van twee letters. U kunt een GeoMatch-voorwaarde combineren met een REQUEST_URI-tekenreeks-voorwaarde voor overeenkomst om een op een pad gebaseerde geo-filterregel te maken.

U kunt beleid voor geo-filteren voor Front Door configureren met behulp van [Azure PowerShell](front-door-tutorial-geo-filtering.md) of de [quick-startsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering).

## <a name="country-code-reference"></a>Verwijzing naar landnummer

|Landnummer | Naam van land |
| ----- | ----- |
| AD | Russische Federatie |
| AE | Verenigde Arabische Emiraten|
| AF | Afghanistan|
| AG | Antigua en Barbuda|
| AL | Albanië|
| AM | Armenië|
| AO | Angola|
| AR | Argentinië|
| AS | Amerikaans-Samoa|
| AT | Oostenrijk|
| AU | Australië|
| AZ | Azerbeidzjan|
| BA | Bosnië en Herzegovina|
| BB | Barbados|
| BD | Bangladesh|
| BE | België|
| BF | Burkina Faso|
| BG | Bulgarije|
| BH | Bahrein|
| BI | Burundi|
| BJ | Benin|
| BL | Saint-Barthélemy|
| BN | Brunei Darussalam|
| BO | Bolivia|
| BR | Brazilië|
| BS | Bahama's|
| BT | Bhutan|
| BW | Botswana|
| BY | Belarus|
| BZ | Belize|
| CA | Canada|
| CD | Democratische Republiek Congo|
| CF | Centraal-Afrikaanse Republiek|
| CH | Zwitserland|
| CI | Cote d'Ivoire|
| CL | Chili|
| CM | Kameroen|
| CN | China|
| CO | Colombia|
| CR | Costa Rica|
| CU | Cuba|
| CV | Kaapverdië|
| CY | Cyprus|
| CZ | Tsjechië|
| DE | Duitsland|
| DK | Denemarken|
| DO | Dominicaanse Republiek|
| DZ | Algerije|
| EC | Ecuador|
| EE | Estland|
| EG | Egypte|
| ES | Spanje|
| ET | Ethiopië|
| FI | Finland|
| FJ | Fiji|
| FM | Micronesië, Federale Staten van|
| FR | Frankrijk|
| GB | Verenigd Koninkrijk|
| GE | Georgië|
| GF | Frans-Guyana|
| GH | Ghana|
| GN | Guinee|
| GP | Guadeloupe|
| GR | Griekenland|
| GT | Guatemala|
| GY | Guyana|
| HK | Hongkong|
| HN | Honduras|
| HR | Kroatië|
| HT | Haiti|
| HU | Hongarije|
| ID | Indonesië|
| IE | Ierland|
| IL | Israël|
| IN | India|
| IQ | Irak|
| IR | Iran, Islamitische Republiek|
| IS | IJsland|
| IT | Italië|
| JM | Jamaica|
| JO | Jordanië|
| JP | Japan|
| KE | Kenia|
| KG | Kirgizië|
| KH | Cambodja|
| KI | Kiribati|
| KN | Saint Kitts en Nevis|
| KP | Noord-Korea|
| KR | Zuid-Korea|
| KW | Koeweit|
| KY | Kaaimaneilanden|
| KZ | Kazachstan|
| LA | Laos|
| LB | Libanon|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesotho|
| LT | Litouwen|
| LU | Luxemburg|
| LV | Letland|
| LY | Libië|
| MA | Marokko|
| MD | Moldavië, Republiek|
| MG | Madagascar|
| MK | Macedonië|
| ML | Mali|
| MM | Myanmar|
| MN | Mongolië|
| MO | Macao|
| MQ | Martinique|
| MR | Mauretanië|
| MT | Malta|
| MV | Maldiven|
| MW | Malawi|
| MX | Mexico|
| MY | Maleisië|
| MZ | Mozambique|
| NA | Namibië|
| NE | Niger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Nederland|
| NO | Noorwegen|
| NP | Nepal|
| NR | Nauru|
| NZ | Nieuw-Zeeland|
| OM | Oman|
| PA | Panama|
| PE | Peru|
| PH | Filipijnen|
| PK | Pakistan|
| PL | Polen|
| PR | Puerto Rico|
| PT | Portugal|
| PW | Palau|
| PY | Paraguay|
| QA | Qatar|
| RE | Réunion|
| RO | Roemenië|
| RS | Servië|
| RU | Russische Federatie|
| RW | Rwanda|
| SA | Saoedi-Arabië|
| SD | Soedan|
| SE | Zweden|
| SG | Singapore|
| SI | Slovenië|
| SK | Slowakije|
| SN | Senegal|
| SO | Somalië|
| SR | Suriname|
| SS | Zuid-Soedan|
| SV | El Salvador|
| SY | Arabische Republiek Syrië|
| SZ | Swaziland|
| TC | Turks- en Caicoseilanden|
| TG | Togo|
| TH | Thailand|
| TN | Tunesië|
| TR | Turkije|
| TT | Trinidad en Tobago|
| TW | Taiwan|
| TZ | Tanzania, Verenigde Republiek|
| UA | Oekraïne|
| UG | Oeganda|
| VS | Verenigde Staten|
| UY | Uruguay|
| UZ | Oezbekistan|
| VC | Saint Vincent en de Grenadines|
| VE | Venezuela|
| VG | Britse Maagdeneilanden|
| VI | Amerikaanse Maagdeneilanden|
| VN | Vietnam|
| ZA | Zuid-Afrika|
| ZM | Zambia|
| ZW | Zimbabwe|

## <a name="next-steps"></a>Volgende stappen

- Informatie over [bescherming van de toepassingslaag met Front Door](front-door-application-security.md).
- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
