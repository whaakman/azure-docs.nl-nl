---
title: Opmerkingen bij de Release van de verkoper Insights
description: Bevat informatie over wijzigingen in de functie voor verkoper inzichten.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 3/3/2019
ms.author: pabutler
ms.openlocfilehash: c6e9e4fe672c7e171ed4b1cd60655f9e71a562e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943128"
---
# <a name="seller-insights-release-notes"></a>Opmerkingen bij de verkoper Insights release 

(Releasedatum: 1 maart 2019)

Dit artikel bevat informatie over wijzigingen in de functie voor verkoper inzichten in de [Cloud Partner-Portal](https://cloudpartner.azure.com/#insights).

## <a name="release-highlights-for-march-1-2019"></a>Belangrijke functies voor 1 maart 2019 vrijgeven

* *Trend van de klant* toegevoegd aan de samenvatting
* *Top 5 klanten* op Samenvatting overeenstemming met alle Azure-abonnementen een klant heeft
* *Gebruikstrend & actieve Orders Trend genormaliseerd* op Samenvatting verplaatst onder *maandelijkse Orders in een oogopslag*
* *Betalings-afstemmingsrapport* bijgewerkt
* *Top 5 klanten* op Payout overeenstemming met alle Azure-abonnementen een klant heeft
* *Gebruiksrapport* bijgewerkt met de klant-ID
* *Klant in dienst* op Orders & gebruik geeft alle Azure-abonnementen een klant heeft


(Releasedatum: 28 juli 2018)

## <a name="release-highlights-for-july-28-2018"></a>Belangrijke functies de release van 28 juli 2018


-   *Geschatte prijs* voorzien van een weergave van de klant kosten Valuta conversie gevolgen.
-   *Prognose van betalingen* bieden een eerdere inzicht in mogelijke betalingen.
-  *Gebruik verwijzing-ID's* betrouwbaarheid van gegevens tussen het gebruik van klanten en orders voorzien van betalingen
-   *Gebruik op een dagelijks interval* biedt meer granulariteit en meer inzicht in het gebruik van de klant.


### <a name="changes-to-data-structure-and-taxonomy"></a>Wijzigingen in de gegevensstructuur en taxonomie

De volgende tabel bevat de metrische gegevens die zijn toegevoegd of aanzienlijk gewijzigd in deze release. 

| **Nieuwe Term**                   |    **Definitie**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Price (CC)                     | De prijs voor een eenheid van gebruik voor een bepaalde SKU (in van de klant valuta).       |
| Uitgebreide geschatte (CC) | Geschatte uitgebreide kosten in rekening gebracht voor het aantal eenheden van het gebruik van voor een bepaalde SKU (in van de klant valuta). Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt.   |
| Uitgever valuta (PC)        | Valuta voorkeur van de uitgever voor toekenning.                               |
| Geschatte prijs (PC)           | Geschatte prijs voor een eenheid van gebruik voor een bepaalde SKU op basis van de conversie valuta op het gebruik van de datum wordt (in van de uitgever valuta) berekend. Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt.   |
| Uitgebreide geschatte (PC) | Geschatte uitgebreide kosten in rekening gebracht voor het aantal eenheden van het gebruik van voor een bepaalde SKU op basis van de conversie valuta op het gebruik van de datum wordt (in van de uitgever valuta) berekend. Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt. |
| Geschatte Payout (PC)          | Geschatte bedrag voor het aantal eenheden van het gebruik van gedurende een bepaalde SKU gebaseerd op wisselkoers conversie op de datum waarop die het gebruik wordt berekend (in van de uitgever valuta). Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt.   |
| Gebruik van verwijzing                | De id voor een of meer dagen van klanten te worden gebruikt voor een bepaalde SKU die zijn gekoppeld aan een vermelding in het rapport payout. |
|  |  |
