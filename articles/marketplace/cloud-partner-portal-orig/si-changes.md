---
title: Opmerkingen bij de verkoper Insights Release | Microsoft Docs
description: Bevat informatie over wijzigingen in de functie voor verkoper inzichten.
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
ms.openlocfilehash: ec3784a91f8aeb7f0fedd13c9ab86a844832a578
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809446"
---
<a name="seller-insights-release-notes"></a>Opmerkingen bij de verkoper Insights release 
===============================
(Releasedatum: 28 juli 2018)

Dit artikel bevat informatie over wijzigingen in de functie voor verkoper inzichten in de [Cloud Partner-Portal](https://cloudpartner.azure.com/#insights).


<a name="release-highlights"></a>Hoogtepunten van release
------------------

-   *Geschatte prijs* voorzien van een weergave van de klant kosten Valuta conversie gevolgen.
-   *Prognose van betalingen* bieden een eerdere inzicht in mogelijke betalingen.
-  *Gebruik verwijzing-ID's* betrouwbaarheid van gegevens tussen het gebruik van klanten en orders voorzien van betalingen
-   *Gebruik op een dagelijks interval* biedt meer granulariteit en meer inzicht in het gebruik van de klant.


<a name="changes-to-data-structure-and-taxonomy"></a>Wijzigingen in de gegevensstructuur en taxonomie
--------------------------------------

De volgende tabel bevat de metrische gegevens die zijn toegevoegd of aanzienlijk gewijzigd in deze release. 

| **Nieuwe Term**                   |    **Definitie**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Prijs (CC)                     | De prijs voor een eenheid van gebruik voor een bepaalde SKU (in van de klant valuta).       |
| Uitgebreide geschatte (CC) | Geschatte uitgebreide kosten in rekening gebracht voor het aantal eenheden van het gebruik van voor een bepaalde SKU (in van de klant valuta). Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt.   |
| Uitgever valuta (PC)        | Valuta voorkeur van de uitgever voor toekenning.                               |
| Geschatte prijs (PC)           | Geschatte prijs voor een eenheid van gebruik voor een bepaalde SKU op basis van de conversie valuta op het gebruik van de datum wordt (in van de uitgever valuta) berekend. Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt.   |
| Uitgebreide geschatte (PC) | Geschatte uitgebreide kosten in rekening gebracht voor het aantal eenheden van het gebruik van voor een bepaalde SKU op basis van de conversie valuta op het gebruik van de datum wordt (in van de uitgever valuta) berekend. Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt. |
| Geschatte Payout (PC)          | Geschatte bedrag voor het aantal eenheden van het gebruik van gedurende een bepaalde SKU gebaseerd op wisselkoers conversie op de datum waarop die het gebruik wordt berekend (in van de uitgever valuta). Deze waarde is mogelijk niet precies vanwege fouten afronding of moet worden afgekapt.   |
| Gebruik van verwijzing                | De id voor een of meer dagen van klanten te worden gebruikt voor een bepaalde SKU die zijn gekoppeld aan een vermelding in het rapport payout. |
|  |  |
