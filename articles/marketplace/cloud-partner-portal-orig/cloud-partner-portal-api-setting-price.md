---
title: Prijzen voor virtuele machine-aanbiedingen | Microsoft Docs
description: Legt uit de drie methoden voor het opgeven van de prijzen van virtuele machine-aanbiedingen.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: a029477dfd8046863ebfe34cd839562a0b1f3d87
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59607791"
---
<a name="pricing-for-virtual-machine-offers"></a>Prijzen voor virtuele machine-aanbiedingen
==================================

Er zijn drie manieren om op te geven van de prijzen voor virtuele machine-aanbiedingen: aangepast core prijzen, core-prijzen en werkblad prijzen.


<a name="customized-core-pricing"></a>Aangepaste core prijzen
-----------------------

Prijzen zijn afhankelijk van de combinatie van elke regio en core. Elke regio in de lijst verkopen moet worden opgegeven in de **virtualMachinePricing**/**regionPrices** sectie van de definitie.  Gebruik de juiste valuatacodes voor elke [regio](#regions) in uw aanvraag.  Het volgende voorbeeld ziet u deze vereisten:

``` json
    "virtualMachinePricing": 
    {
        ... 
        "coreMultiplier": 
        {
            "currency": "USD",
                "individually": 
                {
                    "sharedcore": 2,
                    "1core": 2,
                    "2core": 3,
                    "4core": 4,
                    "6core": 5,
                    "8core": 2,
                    "12core": 4,
                    "16core": 4,
                    "20core": 4,
                    "24core": 4,
                    "32core": 4,
                    "36core": 4,
                    "40core": 4,
                    "64core": 4,
                    "128core": 4
                }
        }
        ...
     }
```


<a name="per-core-pricing"></a>Prijzen per core
----------------

In dit geval de uitgevers geven één prijs in USD voor hun SKU en alle andere prijzen worden automatisch gegenereerd. De prijs per core is opgegeven in de **één** parameter in de aanvraag.

``` json
     "virtualMachinePricing": 
     {
         ...
         "coreMultiplier": 
         {
             "currency": "USD",
             "single": 1.0
         }
     }
```


<a name="spreadsheet-pricing"></a>Werkblad prijzen
-------------------

De uitgever kan ook hun prijzen werkblad uploaden naar een tijdelijke opslaglocatie en Neem de URI in de aanvraag, zoals andere artefacten bestand. Het werkblad vervolgens is geüpload, om te evalueren van het schema voor het opgegeven prijs vertaald en ten slotte de aanbieding wordt bijgewerkt met informatie over de prijzen. Het werkblad URI wordt geretourneerd door de volgende GET-aanvragen voor de aanbieding en de geëvalueerde prijzen voor de regio.

``` json
     "virtualMachinePricing": 
     {
         ...
         "spreadSheetPricing": 
         {
             "uri": "https://blob.storage.azure.com/<your_spreadsheet_location_here>/prices.xlsx",
         }
     }
```

<a name="regions"></a>Regio's
-------

De volgende tabel bevat de verschillende regio's dat u kunt opgeven voor aangepaste core prijzen en hun bijbehorende valuatacodes.

| **Regio** | **Naam**             | **Valutacode** |
|------------|----------------------|-------------------|
| DZ         | Algerije              | DZD               |
| AR         | Argentinië            | ARS               |
| AU         | Australië            | AUD               |
| AT         | Oostenrijk              | EUR               |
| BH         | Bahrein              | BHD               |
| BY         | Belarus              | RUB               |
| BE         | België              | EUR               |
| BR         | Brazilië               | USD               |
| BG         | Bulgarije             | BGN               |
| CA         | Canada               | CAD               |
| CL         | Chili                | CLP               |
| CO         | Colombia             | COP               |
| CR         | Costa Rica           | CRC               |
| HR         | Kroatië              | HRK               |
| CY         | Cyprus               | EUR               |
| CZ         | Tsjechië       | CZK               |
| DK         | Denemarken              | DKK               |
| DO         | Dominicaanse Republiek   | USD               |
| EC         | Ecuador              | USD               |
| EG         | Egypte                | EGP               |
| SV         | El Salvador          | USD               |
| EE         | Estland              | EUR               |
| FI         | Finland              | EUR               |
| FR         | Frankrijk               | EUR               |
| DE         | Duitsland              | EUR               |
| GR         | Griekenland               | EUR               |
| GT         | Guatemala            | GTQ               |
| HK         | Hongkong SAR        | HKD               |
| HU         | Hongarije              | HUF               |
| IS         | IJsland              | SCHIJFTOEWIJZING               |
| IN         | India                | INR               |
| Id         | Indonesië            | IDR               |
| IE         | Ierland              | EUR               |
| IL         | Israël               | ILS               |
| IT         | Italië                | EUR               |
| JP         | Japan                | JPY               |
| JO         | Jordanië               | JOD               |
| KZ         | Kazachstan           | KZT               |
| KE         | Kenia                | KES               |
| KR         | Korea                | KRW               |
| KW         | Koeweit               | KWD               |
| LV         | Letland               | EUR               |
| LI         | Liechtenstein        | CHF               |
| LT         | Litouwen            | EUR               |
| LU         | Luxemburg           | EUR               |
| MK         | Noord-Macedonië      | MKD               |
| MY         | Maleisië             | MYR               |
| MT         | Malta                | EUR               |
| MX         | Mexico               | MXN               |
| ME         | Montenegro           | EUR               |
| MA         | Marokko              | MAD               |
| NL         | Nederland          | EUR               |
| NZ         | Nieuw-Zeeland          | NZD               |
| NG         | Nigeria              | NGN               |
| NO         | Noorwegen               | NOK               |
| OM         | Oman                 | OMR               |
| PK         | Pakistan             | PKR               |
| PA         | Panama               | USD               |
| PY         | Paraguay             | PYG               |
| PE         | Peru                 | PEN               |
| PH         | Filipijnen          | PHP               |
| PL         | Polen               | PLN               |
| PT         | Portugal             | EUR               |
| PR         | Puerto Rico          | USD               |
| QA         | Qatar                | QAR               |
| RO         | Roemenië              | LOEK               |
| RU         | Rusland               | RUB               |
| SA         | Saoedi-Arabië         | SAR               |
| RS         | Servië               | RSD               |
| SG         | Singapore            | SGD               |
| SK         | Slowakije             | EUR               |
| SI         | Slovenië             | EUR               |
| ZA         | Zuid-Afrika         | ZAR               |
| ES         | Spanje                | EUR               |
| LK         | Sri Lanka            | USD               |
| SE         | Zweden               | SEK               |
| CH         | Zwitserland          | CHF               |
| TW         | Taiwan               | TWD               |
| TH         | Thailand             | THB               |
| TT         | Trinidad en Tobago  | TTD               |
| TN         | Tunesië              | TND               |
| TR         | Turkije               | TRY               |
| UA         | Oekraïne              | UAH               |
| AE         | Verenigde Arabische Emiraten | EUR               |
| GB         | Verenigd Koninkrijk       | GBP               |
| VS         | Verenigde Staten        | USD               |
| UY         | Uruguay              | UYU               |
| VE         | Venezuela            | USD               |
|  |  |  |
