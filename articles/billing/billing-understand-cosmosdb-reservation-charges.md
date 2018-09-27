---
title: Begrijpen hoe de reserveringskorting wordt toegepast op Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe de reserveringskorting wordt toegepast op de ingerichte doorvoer (RU/s) in Azure Cosmos DB.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: cwatson
ms.reviewer: sngun
ms.openlocfilehash: adcd91a8f1b3368d03f4b634e7aef40104d953e3
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393635"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-cosmos-db"></a>Begrijpen hoe de reserveringskorting wordt toegepast op Azure Cosmos DB

Nadat u een Azure Cosmos DB gereserveerde capaciteit koopt, wordt de reserveringskorting wordt automatisch toegepast op Azure Cosmos DB-resources die overeenkomen met de kenmerken en de hoeveelheid van de reservering. Een reservering bevat informatie over de doorvoer die is ingericht voor Azure Cosmos DB-resources en deze niet wordt behandeld in software, netwerken, opslag, of vooraf gedefinieerde container kosten in rekening gebracht.

## <a name="reservation-discount-applied-to-azure-cosmos-db-accounts"></a>De reserveringskorting wordt toegepast op Azure Cosmos DB-accounts

Een reserveringskorting wordt toegepast op [ingerichte doorvoer](../cosmos-db/request-units.md) in termen van aanvraageenheden per second(RU/s) op basis van per uur. Voor Azure Cosmos DB-resources die niet volledig uur worden uitgevoerd, worden de reserveringskorting wordt automatisch toegepast op andere Cosmos DB-resources die overeenkomen met de kenmerken van de reservering. De korting kunt toepassen op Azure Cosmos DB-resources die gelijktijdig worden uitgevoerd. Als u geen Cosmos DB-resources die voor een volledig uur worden uitgevoerd en die voldoen aan de kenmerken van de reservering hebt, krijgt u niet het volledige voordeel van de reserveringskorting voor dat uur.

* De kortingen zijn gelaagd, wat betekent dat reserveringen met hogere aanvraageenheden bieden hogere kortingen.  
* De reserveringsaankoop wordt kortingen van toepassing op alle regio's in de verhouding gelijk is aan de regionale prijzen op aanvraag. Zie voor reservering korting ratio's in elke regio [reserveringskorting per regio](#reservation-discount-per-region) sectie van dit artikel.

## <a name="reservation-discount-per-region"></a>De reserveringskorting per regio
De reserveringskorting wordt toegepast op Azure Cosmos DB doorvoer kosten op basis van per uur op één abonnement of ingeschreven/accountbereik. De reserveringskorting is van toepassing op gebruik kan meten in verschillende regio's in de volgende verhouding:

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
|Azure Cosmos DB - 100 RU/s/uur - SA-West |    SA-West      |    1     |
|Azure Cosmos DB - 100 RU/s/uur - Zuid-India|    IN - zuid     |    1.0375    |
|Azure Cosmos DB - 100 RU/s/uur - CA-Oost|   CA - oost      |    1.1      |
|Azure Cosmos DB - 100 RU/s/uur - Japan Oost|   JA - oost      |    1,125     |
|Azure Cosmos DB - 100 RU/s/uur - Japan West|     JA - west    |   1,125       |
|Azure Cosmos DB - 100 RU/s/uur - IN-West|     West-India    |    1.1375     |
|Azure Cosmos DB - 100 RU/s/uur - centraal-India|    IN - centraal     |  1.1375       |
|Azure Cosmos DB - 100 RU/s/uur - Australië Oost|     AU - oost    |   1,15       |
|Azure Cosmos DB - 100 RU/s/uur - CA-centraal|  CA - centraal       |   1.2       |
|Azure Cosmos DB - 100 RU/s/uur - FR-centraal|   FR - centraal      |    1,25      |
|Azure Cosmos DB - 100 RU/s/uur - Brazilië Zuid|  BR - zuid       |   1.5      |
|Azure Cosmos DB - 100 RU/s/uur - Australië-centraal|   Australië-centraal      |   1.5      |
|Azure Cosmos DB - 100 RU/s/uur - Australië-centraal 2| AU-centraal 2        |    1.5     |
|Azure Cosmos DB - 100 RU/s/uur - FR-Zuid|    FR - zuid     |    1.625     |

## <a name="scenarios-that-show-how-the-reservation-discount-is-applied"></a>Scenario's die laten zien hoe de reserveringskorting wordt toegepast

Houd rekening met de volgende vereisten voor een reservering:

* Vereiste doorvoer: 50.000 RU/s  
* Regio's gebruikt: 2. 

In dit geval zijn uw totale kosten voor on-demand voor 500 hoeveelheid van 100 RU/s meter in deze twee regio's voor een totaal RU/s verbruik van 100.000 per uur. 

**Scenario 1**

Bijvoorbeeld, als u Azure Cosmos DB-implementaties in regio's 'VS Noord-centraal' en 'VS-West', en als elke regio heeft een verbruik van de doorvoer van 50.000 RU/s. De kosten voor uw on-demand zou volledig het saldo van een aankoop van de reservering van 100.000 RU/s.

De korting wordt gedekt door een reservering wordt berekend als (doorvoer verbruik * reservation_discount_ratio_for_that_region). Voor regio's 'VS Noord-centraal' en 'VS-West' is de verhouding van de korting reservering "1". Dus de totale korting RU/s 100.000 RU/s zijn (deze waarde wordt berekend als: 50.000 * 1 + 50.000 * 1 = 100.000 RU/s), en u hoeft te betalen extra kosten in rekening gebracht op de normale betalen per gebruik-tarieven betalen. 

|Beschrijving van de meter | Regio |Verbruik van doorvoer (RU/s) |De reserveringskorting wordt toegepast op RU/s |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/uur - VS Noord-centraal  |   US - noord-centraal  | 50,000  | 50,000  |
|Azure Cosmos DB - 100 RU/s/uur - VS West  |  US - west   |  50,000  |  50,000 |

**Scenario 2**

Bijvoorbeeld, als u Azure Cosmos DB-implementaties in "AU-centraal 2" en 'FR-Zuid' regio's, en als elke regio heeft een verbruik van de doorvoer van 50.000 RU/s. Een aankoop van de reservering van 100.000 RU/s is van toepassing als volgt (ervan uitgaande dat Australië centraal 2 gebruik is eerst korting):

|Beschrijving van de meter | Regio |Verbruik van doorvoer (RU/s) |De reserveringskorting wordt toegepast op RU/s |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/uur - Australië-centraal 2  |  AU-centraal 2   |  50,000  |  50,000   |
|Azure Cosmos DB - 100 RU/s/uur - FR-Zuid  |  FR - zuid   |  50,000 |  15,384  |

50.000 eenheden van het gebruik in de regio "AU-centraal 2" van komt overeen met 75.000 RU/s van factureerbare gebruik (of genormaliseerde gebruik). Deze waarde wordt berekend als (doorvoer verbruik * reservation_discount_ratio_for_that_region) die gelijk is aan 75.000 RU/s (deze waarde wordt berekend als: 50.000 * 1.5 = 75.000 RU/s) van het gebruik van factureerbare of genormaliseerde van. 

100.000 RU/s reserveringsaankoop zou de 75.000 RU/s in "AU-centraal 2" offset en 25.000 RU/s naar de regio 'FR-Zuid' blijft. Van de resterende 25.000 RU/s, een reserveringskorting van 15,384 RU/s (deze waarde wordt berekend als: 25.000 / 1.625 = 15,384 RU/s) wordt toegepast op 'FR-Zuid' regio. De resterende 34,616 RU/s in regio 'FR-Zuid' worden kosten in rekening gebracht tegen de normale betalen per gebruik-tarieven. 

De Azure-facturering systeem, wordt de reservering facturering van het belangrijkste voordeel voor het eerste exemplaar dat wordt verwerkt die overeenkomt met de configuratie van de reservering (bijvoorbeeld, Australië centraal 2 in dit geval) toegewezen.

Om te begrijpen en te bekijken van de toepassing van uw Azure-reserveringen in gebruiksrapporten facturering, Zie [over Azure-reservering gebruik](../billing/billing-understand-reserved-instance-usage-ea.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

* [Wat zijn Azure reserveringen?](../billing/billing-save-compute-costs-reservations.md)  
* [Betaal vooruit voor Azure Cosmos DB-resources met Azure Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)  
* [Betaal vooruit voor SQL Database-compute-resources met Azure SQL Database gereserveerde capaciteit](../sql-database/sql-database-reserved-capacity.md)  
* [Azure-reserveringen beheren](../billing/billing-manage-reserved-vm-instance.md)  
* [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../billing/billing-understand-reserved-instance-usage.md)  
* [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../billing/billing-understand-reserved-instance-usage-ea.md)  
* [Informatie over het gebruik van de reservering voor CSP-abonnementen](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

