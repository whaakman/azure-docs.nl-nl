---
title: Begrijpen hoe de reserveringskorting wordt toegepast op Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe de reserveringskorting wordt toegepast op de ingerichte doorvoer (RU/s) in Azure Cosmos DB.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: banders
ms.reviewer: sngun
ms.openlocfilehash: f83499e52b6405ec5475795a80c5a4325043a441
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904485"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-cosmos-db"></a>Begrijpen hoe de reserveringskorting wordt toegepast op Azure Cosmos DB

Nadat u een Azure Cosmos DB gereserveerde capaciteit koopt, wordt de reserveringskorting wordt automatisch toegepast op Azure Cosmos DB-resources die overeenkomen met de kenmerken en de hoeveelheid van de reservering. Een reservering bevat informatie over de doorvoer die is ingericht voor Azure Cosmos DB-resources. Het niet wordt behandeld in software, netwerken, opslag, of vooraf gedefinieerde container kosten in rekening gebracht.

## <a name="reservation-discount-applied-to-azure-cosmos-db-accounts"></a>De reserveringskorting wordt toegepast op Azure Cosmos DB-accounts

Een reserveringskorting wordt toegepast op [ingerichte doorvoer](../cosmos-db/request-units.md) in termen van aanvraageenheden per seconde (RU/s) op basis van per uur. Voor Azure Cosmos DB-resources die niet volledig uur worden uitgevoerd, worden de reserveringskorting wordt automatisch toegepast op andere Cosmos DB-resources die overeenkomen met de kenmerken van de reservering. De korting kunt toepassen op Azure Cosmos DB-resources die gelijktijdig worden uitgevoerd. Als u geen Cosmos DB-resources die voor een volledig uur worden uitgevoerd en die voldoen aan de kenmerken van de reservering hebt, krijgt u niet het volledige voordeel van de reserveringskorting voor dat uur.

De kortingen worden geschakeld. Reserveringen met hogere aanvraageenheden bieden hogere kortingen. 

De reserveringsaankoop wordt kortingen van toepassing op alle regio's in de verhouding gelijk is aan de regionale prijzen op aanvraag. Zie voor reservering korting ratio's in elke regio, de [reserveringskorting per regio](#reservation-discount-per-region) sectie van dit artikel.

## <a name="reservation-discount-per-region"></a>De reserveringskorting per regio
De reserveringskorting wordt toegepast op Azure Cosmos DB doorvoer kosten op basis van per uur. Deze wordt toegepast op één abonnement of op het ingeschreven/accountbereik. De reserveringskorting is van toepassing op gebruik kan meten in verschillende regio's in de volgende verhoudingen:

|Beschrijving van de meter  |Regio |Verhouding  |
|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/uur - Azië en Stille Oceaan Zuidoost  |  AP - zuidoost    |   1      |
|Azure Cosmos DB - 100 RU/s/uur - Azië en Stille Oceaan Oost |   AP - oost   |    1     |
|Azure Cosmos DB - 100 RU/s/uur - EU Noord|  EU - noord       |    1     |
|Azure Cosmos DB - 100 RU/s/uur - Korea, zuiden|    KR - zuid     |     1    |
|Azure Cosmos DB - 100 RU/s/uur - EU West|    EU - west     |      1   |
|Azure Cosmos DB - 100 RU/s/uur - Korea, midden|   KR - centraal    |       1  |
|Azure Cosmos DB - 100 RU/s/uur - UK-Zuid|   Verenigd Koninkrijk Zuid      |     1    |
|Azure Cosmos DB - 100 RU/s/uur - UK-West|   Verenigd Koninkrijk West      |    1     |
|Azure Cosmos DB - 100 RU/s/uur - UK-Noord |   VK, noord    |     1    |
|Azure Cosmos DB - 100 RU/s/uur - UK-Zuid 2|   VK, zuid 2      |     1    |
|Azure Cosmos DB - 100 RU/s/uur - VS Oost 2|  US - oost 2     |     1    |
|Azure Cosmos DB - 100 RU/s/uur - VS Noord-centraal|   US - noord-centraal      |     1    |
|Azure Cosmos DB - 100 RU/s/uur - VS West|   US - west      |     1    |
|Azure Cosmos DB - 100 RU/s/uur - VS centraal| US - centraal        |     1    |
|Azure Cosmos DB - 100 RU/s/uur - VS West 2|   US - west 2      |      1   |
|Azure Cosmos DB - 100 RU/s/uur - US West-Centraal|   US - west-centraal      |       1  |
|Azure Cosmos DB - 100 RU/s/uur - VS Oost|   US - oost      |  1       |
|Azure Cosmos DB - 100 RU/s/uur - SA-Noord|     SA-Noord    |   1      |
|Azure Cosmos DB - 100 RU/s/uur - SA-West |    SA West      |    1     |
|Azure Cosmos DB - 100 RU/s/uur - Zuid-India|    IN - zuid     |    1.0375    |
|Azure Cosmos DB - 100 RU/s/uur - CA-Oost|   CA - oost      |    1.1      |
|Azure Cosmos DB - 100 RU/s/uur - Japan Oost|   JA - oost      |    1.125     |
|Azure Cosmos DB - 100 RU/s/uur - Japan West|     JA - west    |   1.125       |
|Azure Cosmos DB - 100 RU/s/uur - IN-West|     West-India    |    1.1375     |
|Azure Cosmos DB - 100 RU/s/uur - centraal-India|    IN - centraal     |  1.1375       |
|Azure Cosmos DB - 100 RU/s/uur - Australië Oost|     AU - oost    |   1.15       |
|Azure Cosmos DB - 100 RU/s/uur - CA-centraal|  CA - centraal       |   1.2       |
|Azure Cosmos DB - 100 RU/s/uur - FR-centraal|   FR - centraal      |    1.25      |
|Azure Cosmos DB - 100 RU/s/uur - Brazilië Zuid|  BR - zuid       |   1.5      |
|Azure Cosmos DB - 100 RU/s/uur - Australië-centraal|   AU - centraal      |   1.5      |
|Azure Cosmos DB - 100 RU/s/uur - Australië-centraal 2| AU - centraal 2        |    1.5     |
|Azure Cosmos DB - 100 RU/s/uur - FR-Zuid|    FR - zuid     |    1.625     |

## <a name="scenarios-that-show-how-the-reservation-discount-is-applied"></a>Scenario's die laten zien hoe de reserveringskorting wordt toegepast

Houd rekening met de volgende vereisten voor een reservering:

* Vereiste doorvoer: 50.000 RU/s  
* Regio's gebruikt: 2 

In dit geval zijn uw totale kosten voor on-demand voor 500 hoeveelheid van 100 RU/s-meter in deze twee regio's. Het totaal aantal RU/s-verbruik is 100.000 per uur. 

**Scenario 1**

Bijvoorbeeld, wordt ervan uitgegaan dat u Azure Cosmos DB-implementaties in de regio's VS Noord-centraal en VS-West moet. Elke regio heeft een verbruik van de doorvoer van 50.000 RU/s. De kosten voor uw on-demand zou volledig het saldo van een aankoop van de reservering van 100.000 RU/s.

De korting die betrekking heeft op een reservering wordt berekend als: doorvoer verbruik * reservation_discount_ratio_for_that_region. Voor de regio's VS Noord-centraal en VS-West is de verhouding van de korting reservering 1. De totale korting RU/s zijn dus 100.000. Deze waarde is als volgt berekend: 50.000 * 1 + 50.000 * 1 = 100.000 RU/s. U hebt geen eventuele extra kosten aan de normale tarieven voor betalen per gebruik betalen. 

|Beschrijving van de meter | Regio |Verbruik van doorvoer (RU/s) |De reserveringskorting wordt toegepast op RU/s |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/uur - VS Noord-centraal  |   US - noord-centraal  | 50,000  | 50,000  |
|Azure Cosmos DB - 100 RU/s/uur - VS West  |  US - west   |  50,000  |  50,000 |

**Scenario 2**

Bijvoorbeeld, wordt ervan uitgegaan dat u Azure Cosmos DB-implementaties in de regio's Australië centraal 2 en FR-Zuid moet. Elke regio heeft een verbruik van de doorvoer van 50.000 RU/s. Een aankoop van de reservering van 100.000 RU/s is van toepassing als volgt (ervan uitgaande dat Australië centraal 2 gebruik is eerst korting):

|Beschrijving van de meter | Regio |Verbruik van doorvoer (RU/s) |De reserveringskorting wordt toegepast op RU/s |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/uur - Australië-centraal 2  |  AU - centraal 2   |  50,000  |  50,000   |
|Azure Cosmos DB - 100 RU/s/uur - FR-Zuid  |  FR - zuid   |  50,000 |  15,384  |

Een gebruik van 50.000 eenheden in de regio Australië centraal 2 komt overeen met 75.000 RU/s van factureerbare gebruik (of genormaliseerde gebruik). Deze waarde wordt berekend als: doorvoer verbruik * reservation_discount_ratio_for_that_region. De berekening is gelijk aan 75.000 RU/s van het gebruik van factureerbare of genormaliseerde van. Deze waarde is als volgt berekend: 50.000 * 1.5 = 75.000 RU/s.

De 100.000 RU/s van het kopen van reserveringen zou de 75.000 RU/s in Australië centraal 2 verschuiving. Laat u 25.000 RU/s naar de regio FR-Zuid. Een reserveringskorting van 15,384 RU/s is van de resterende 25.000 RU/s toegepast op de regio FR-Zuid. De kortingswaarde wordt berekend als: 25.000 / 1.625 = 15,384 RU/s. De resterende 34,616 RU/s in de regio FR-Zuid, worden in rekening gebracht tegen de normale betalen per gebruik-tarieven. 

De Azure-facturering systeem, de reservering facturering van het belangrijkste voordeel voor de eerste instantie die wordt verwerkt en dat overeenkomt met de configuratie van de reservering wordt toegewezen. Het is bijvoorbeeld Australië centraal 2 in dit geval.

Om te begrijpen en te bekijken van de toepassing van uw Azure-reserveringen in gebruiksrapporten facturering, Zie [over Azure-reservering gebruik](../billing/billing-understand-reserved-instance-usage-ea.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure reserveringen, de volgende artikelen:

* [Wat zijn Azure reserveringen?](../billing/billing-save-compute-costs-reservations.md)  
* [Betaal vooruit voor Azure Cosmos DB-resources met Azure Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)  
* [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)  
* [Azure-reserveringen beheren](../billing/billing-manage-reserved-vm-instance.md)  
* [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../billing/billing-understand-reserved-instance-usage.md)  
* [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../billing/billing-understand-reserved-instance-usage-ea.md)  
* [Informatie over het gebruik van de reservering voor CSP-abonnementen](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

