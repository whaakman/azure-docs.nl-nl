---
title: Informatie over uw factuur voor Azure Cosmos DB
description: In dit artikel wordt uitgelegd hoe u meer informatie over uw factuur voor Azure Cosmos DB met enkele voorbeelden.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 5c3da9a69204d7229ddcbbf39ec389684258e389
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55470685"
---
# <a name="understand-your-azure-cosmos-db-bill"></a>Meer informatie over uw factuur voor Azure Cosmos DB

Als een volledig beheerde cloud-native-databaseservice, Azure Cosmos DB vereenvoudigt de facturering in rekening gebracht alleen voor ingerichte doorvoer en opslag verbruikt. Er zijn geen extra licentiekosten, hardware, hulpprogramma kosten, of faciliteit kosten in vergelijking met on-premises of alternatieven IaaS gehost. Wanneer u rekening houden met de meerdere regio mogelijkheden van Azure Cosmos DB, biedt de database-service een aanzienlijke vermindering van de kosten in vergelijking met bestaande on-premises of IaaS-oplossingen.

U wordt gefactureerd per uur op basis van de ingerichte doorvoer en de verbruikte opslag met Azure Cosmos DB. Voor de ingerichte doorvoer is, is de eenheid voor facturering 100 RU/sec. per uur, in rekening gebracht tegen $0.008 per uur, uitgaande van de standaard openbare prijzen, raadpleegt u de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/). Voor de verbruikte opslag, zijn de gefactureerde $0,25 per 1 GB aan opslag per maand, Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/). 

In dit artikel wordt een aantal voorbeelden om te begrijpen van de details u op de maandelijkse factuur ziet. De getallen die worden weergegeven in de voorbeelden is mogelijk anders als uw Azure Cosmos-containers een andere hoeveelheid doorvoer die is ingericht hebben, als het verdeeld over meerdere regio's of voor een andere gedurende meer dan een maand worden uitgevoerd.

## <a name="billing-examples"></a>Voorbeelden van facturering

### <a name="billing-example---throughput-on-a-container-full-month"></a>Voorbeeld van de facturering - doorvoer voor een container (volledige maand)

* Stel dat u een doorvoer van 1000 RU/sec. configureren voor een container en deze aanwezig is voor 24 uur * 30 dagen voor de maand = 720 uren totale.  

* 1000 RU/sec. is 10 eenheden van 100 RU/sec. per uur voor elk uur dat de containers bestaat (dat wil zeggen, 1000/100 = 10). 

* 10 eenheden per uur vermenigvuldigen met de kosten van $0.008 (per 100 RU/sec. per uur) = $0,08 per uur. 

* Vermenigvuldigen met de $0,08 per uur door het aantal uren in de maand is gelijk aan $0,08 * 24 uur * 30 dagen $57.60 = voor de maand.  

* De totale maandelijkse factuur 7200 eenheden (van 100 ru's), die het kost $57.60.

### <a name="billing-example---throughput-on-a-container-partial-month"></a>Voorbeeld van de facturering - doorvoer voor een container (deel van een maand)

* Stel dat we een container maken met ingerichte doorvoer van 2.500 RU/sec. De container is echter 24 uur in de maand (bijvoorbeeld, verwijderen we het 24 uur nadat we dit hebt gemaakt).  

* Zien we 600 eenheden op de factuur (2.500 RU/sec. / 100 RU/seconde per eenheid * 24 uur). De kosten worden $4.80 (600 eenheden * $0.008 per eenheid).

* Totale factuur voor die maand worden $4.80.

### <a name="billing-rate-if-storage-size-changes"></a>Tarief facturering als maximale grootte wordt gewijzigd

Opslagcapaciteit wordt in rekening gebracht in eenheden van de maximale hoeveelheid opgeslagen gegevens per uur (in GB) gedurende een maand. Als u bijvoorbeeld 100 GB aan opslag hebt gebruikt tijdens de eerste helft van de maand en 50 GB in de tweede helft, krijgt u een factuur voor het equivalent van 75 GB aan opslag gedurende die maand.

### <a name="billing-rate-when-container-or-a-set-of-containers-are-active-for-less-than-an-hour"></a>Tarieven wanneer container of een set van containers actief zijn minder dan een uur

U bent vast bedrag in rekening gebracht voor elk uur dat de container of de database, ongeacht het gebruik bestaat of als de container of de database actief minder dan een uur is. Als u een container of de database maken en deze vijf minuten later verwijdert, wordt uw factuur één uur opnemen.

### <a name="billing-rate-when-throughput-on-a-container-or-database-scales-updown"></a>Tarief facturering tijdens doorvoer voor een container of de database omhoog/omlaag schalen

Als u ingerichte doorvoer te om 9:30 uur van 400 RU/sec. naar 1000 RU/sec. verhogen en ochtends ingerichte doorvoer om 10:45 uur weer verlaagt naar 400 RU/sec., wordt u gefactureerd voor twee uren van 1.000 RU/sec. 

Als u ingerichte doorvoer voor een container of een set met containers om 9:30 uur 100 kB RU/sec. 200-K RU/sec. en vervolgens lagere ingerichte doorvoer om 10:45 uur terug naar 100 kB RU/sec. verhoogt, wordt u gefactureerd voor twee uren van 200 K RU/sec.

### <a name="billing-example-multiple-containers-each-with-dedicated-provisioned-throughput-mode"></a>Facturering-voorbeeld: meerdere containers, elk met een toegewezen ingerichte doorvoermodus

* Als u een Azure Cosmos-account in VS-Oost 2 met twee containers met ingerichte doorvoer van 500 RU/sec. en 700 RU/sec., respectievelijk maakt, hebt u zou een totale ingerichte doorvoer van 1200 RU/sec.  

* U kosten in rekening worden gebracht 1200/100 * $0.008 = $0,096 / uur. 

* Als uw doorvoer moet worden gewijzigd en u hebt van de container capaciteit verhoogd met 500 RU/sec. terwijl ook een nieuwe onbeperkte container maakt met 20.000 RU/sec., zou de totale ingerichte capaciteit 22.200 RU/sec. (1.000 RU/sec + 1.200 RU/sec. + 20, 000RU per seconde).  

* Uw factuur's vervolgens gewijzigd in: $0.008 x 222 = $1.776 / uur. 

* In een maand met 720 uur (24 uur * 30 dagen), als voor 500 uren ingerichte doorvoer is 1.200 RU/sec. en voor de resterende 220 uur ingerichte doorvoer 22.200 RU/sec., ziet u uw maandelijkse factuur: 500 x $0,096 / uur + 220 x $1.776 / uur = $438.72 / maand.

![Toegewezen doorvoer factuur voorbeeld](./media/understand-your-bill/bill-example1.png)

### <a name="billing-example-containers-with-shared-throughput-mode"></a>Facturering-voorbeeld: containers met de doorvoermodus gedeelde

* Als u een Azure Cosmos-account in VS-Oost 2 met twee Azure Cosmos-databases (met een set van containers die de doorvoer op het databaseniveau van de delen) met de ingerichte doorvoer van 50-K RU/sec. en 70-K RU/sec., respectievelijk maakt, zijn er in totaal ingericht doorvoer van 120 K RU/sec.  

* U kosten in rekening worden gebracht 1200 x $0.008 = $9.60 / uur. 

* Als uw doorvoer moet worden gewijzigd en u de ingerichte doorvoer van elke database met 10 K RU/sec. voor elke database verhoogd, en u een nieuwe container aan de eerste database met speciaal toegewezen doorvoermodus met 15 K RU/sec. met uw database gedeelde doorvoer toevoegen, uw totale ingerichte capaciteit zou 155-K RU/sec. (60 K RU/sec. + 80 K RU/sec. + 15 K RU/sec.) zijn.  

* Uw factuur's vervolgens gewijzigd in: 1.550 * $0.008 = $12.40 / uur.  

* In een maand van 720 uur als 300 uur ingerichte doorvoer 120-K RU/sec. is en voor de resterende 420 uur ingerichte doorvoer 155-K RU/sec., is factuur uw maandelijkse: 300 x $9.60 / uur + 420 x $12.40 / uur = $2,880 + $5,208 = $8,088 / maand. 

![Voorbeeld van de factuur doorvoer gedeeld](./media/understand-your-bill/bill-example2.png)

## <a name="billing-examples-with-geo-replication-and-multi-master"></a>Voorbeelden van facturering met geo-replicatie en meerdere masters  

U kunt toevoegen/verwijderen Azure-regio's overal ter wereld bij uw Azure Cosmos DB-databaseaccount op elk gewenst moment. De doorvoer die u hebt geconfigureerd voor verschillende Azure Cosmos DB-databases en containers wordt gereserveerd in elk van de Azure-regio's die zijn gekoppeld aan uw Azure Cosmos-database-account. Als de som van de ingerichte doorvoer (RU/sec.) geconfigureerd voor alle databases en containers in uw Azure Cosmos-database-account (ingericht per uur) T is en het aantal Azure-regio's die zijn gekoppeld aan uw databaseaccount is N, is het totale aantal ingerichte doorvoer voor een opgegeven uur, voor uw Azure-Cosmos-databaseaccount, (a) geconfigureerd met een enkele schrijfregio gelijk aan T x N RU/sec. en (b is) geconfigureerd met alle regio's kan nu verwerken schrijfbewerkingen is gelijk aan T x (N + 1) RU/sec , respectievelijk. Ingerichte doorvoer (één schrijfregio) kosten $0.008/ uur per 100 RU/sec. en ingerichte doorvoer met meerdere beschrijfbare regio's (meerdere masters config) kosten $0,016 / per uur per 100 RU/sec. (Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)). Of de één schrijven regio of meerdere regio's voor schrijven, wordt Azure Cosmos DB kunt u gegevens lezen van andere regio's.

### <a name="billing-example-multi-region-azure-cosmos-account-single-region-writes"></a>Facturering-voorbeeld: Azure Cosmos-account voor meerdere regio's, één regio schrijft

Stel, dat u hebt een Azure Cosmos-container in West-Amerika. De container wordt gemaakt met doorvoer 10 K RU/sec. en u deze maand 1 TB aan gegevens opslaan. Stel dat u drie regio's (VS-Oost, Noord-Europa en Oost-Azië) toevoegen aan uw Azure Cosmos-account, elk met de dezelfde opslag en doorvoer. Uw totale maandfactuur zal worden (uitgaande van 30 dagen in een maand). Uw factuur wordt als volgt: 

|**Item** |**Gebruik (maand)** |**Snelheid** |**Maandelijkse kosten** |
|---------|---------|---------|-------|
|Doorvoerfactuur voor container in US - west      | 10 K RU/sec. * 24 * 30    |$0.008 per 100 RU/sec. per uur   |$576|
|Doorvoerfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost       | 3 * 10K RU/sec. * 24 * 30    |$0.008 per 100 RU/sec. per uur  |$1,728|
|Opslagfactuur voor container in US - west      | 250 GB    |$ 0,25/GB  |$62.50|
|Opslagfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost      | 3 * 250 GB    |$ 0,25/GB  |$187.50|
|**Totaal**     |     |  |**$2,554**|

*We gaan ook wordt ervan uitgegaan dat u bijvoorbeeld 100 GB aan gegevens elke maand van de container in West-Amerika om gegevens te repliceren in VS-Oost, Noord-Europa en Oost-Azië. U wordt gefactureerd voor uitgaand verkeer volgens de standaardtarieven voor gegevensoverdracht.*

### <a name="billing-example-multi-region-azure-cosmos-account-multi-region-writes"></a>Facturering-voorbeeld: Azure Cosmos-account voor meerdere regio's, meerdere regio schrijft

Stel dat u een Azure Cosmos-container maken in VS-West. De container wordt gemaakt met doorvoer 10 K RU/sec. en u deze maand 1 TB aan gegevens opslaan. Stel dat u toevoegt drie regio's (VS-Oost, Noord-Europa en Oost-Azië), elk met dezelfde opslag en doorvoer en u wilt dat de mogelijkheid om te schrijven naar de containers in alle regio's die zijn gekoppeld aan uw Azure Cosmos-account. Uw totale maandfactuur zal worden (uitgaande van 30 dagen in een maand) als volgt:

|**Item** |**Gebruik (maand)**|**Snelheid** |**Maandelijkse kosten** |
|---------|---------|---------|-------|
|Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)       | 10 K RU/sec. * 24 * 30    |$0,016 per 100 RU/sec. per uur    |$1,152 |
|Doorvoerfactuur voor 3 extra regio's: VS-Oost, Noord-Europa en Oost-Azië (alle regio's worden geschreven)        | (3 + 1) * 10 K RU/sec. * 24 * 30    |$0,016 per 100 RU/sec. per uur   |$4,608 |
|Opslagfactuur voor container in US - west      | 250 GB    |$ 0,25/GB  |$62.50|
|Opslagfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost      | 3 * 250 GB    |$ 0,25/GB  |$187.50|
|**Totaal**     |     |  |**$6,010**|

*We gaan ook wordt ervan uitgegaan dat u bijvoorbeeld 100 GB aan gegevens elke maand van de container in West-Amerika om gegevens te repliceren in VS-Oost, Noord-Europa en Oost-Azië. U wordt gefactureerd voor uitgaand verkeer volgens de standaardtarieven voor gegevensoverdracht.*

### <a name="billing-example-azure-cosmos-account-with-multi-master-database-level-throughput-including-dedicated-throughput-mode-for-some-containers"></a>Facturering-voorbeeld: Azure Cosmos-account met meerdere masters, op databaseniveau doorvoer, met inbegrip van toegewezen doorvoer-modus voor enkele containers

Laten we eens in het volgende voorbeeld, waar we hebben een Azure Cosmos-account voor meerdere regio's waar alle regio's beschrijfbare (meerdere masters config) zijn. Voor het gemak veronderstellen we opslaggrootte blijven constant en niet wijzigen en laat het hier te houden in het voorbeeld eenvoudiger. De ingerichte doorvoer tijdens de maand variëren als volgt (uitgaande 30 dagen of uren 720): 

[0-100 uren]:  

* We een drie regio Azure Cosmos-account hebt gemaakt (VS-West, VS-Oost, Noord-Europa), waarbij alle regio's geschreven worden 

* We bij het maken van een database (D1) met gedeelde doorvoer 10K RU/sec 

* We bij het maken van een database (D2) met gedeelde doorvoer 30-K RU/sec. en  

* We een container (C1) gemaakt met speciaal toegewezen doorvoer 20 K RU/sec 

[101-200 uren]:  

* We opgeschaald database 50 K RU/sec. (D1) 

* We opgeschaald 70 K RU/sec. database (D2)  

* We verwijderde container (C1)  

[201-300 uren]:  

* We container (C1) opnieuw gemaakt met speciaal toegewezen doorvoer 20 K RU/sec 

[301-400 uur]:  

* Er is een van de regio's verwijderd uit Azure Cosmos-account (# beschrijfbare regio's is nu 2) 

* We verkleind database 10 K RU/sec. (D1) 

* We opgeschaald database 80 K RU/sec. (D2)  

* We verwijderde opnieuw container (C1) 

[401-500 uren]:  

* We verkleind database (D2) naar 10 K RU/sec.  

* We container (C1) opnieuw gemaakt met speciaal toegewezen doorvoer 20 K RU/sec 

[501-700 uren]:  

* We opgeschaald 20 K RU/sec. database (D1)  

* We database (D2) tot 100 K RU/sec. opgeschaald  

* We verwijderde opnieuw container (C1)  

[701-720 uren]:  

* We verkleind database (D2) tot 50 K RU/sec.  

De wijzigingen in de totale ingerichte doorvoer 720 kantooruren voor de maand zijn visueel wordt weergegeven in de afbeelding hieronder: 

![Voorbeeld van de echte leven](./media/understand-your-bill/bill-example3.png)

De totale maandelijkse factuur wordt worden (uitgaande van 30 dagen/720 uur per maand) wordt als volgt berekend:

|**Uur**  |**RU/s** |**Item** |**Gebruik (per uur)** |**Kosten** |
|---------|---------|---------|-------|-------|
|[0-100] |D1:10K <br/>D2:30K <br/>C1:20K |Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)  | `D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 30 K RU/sec/100 * $0.016 * 100 hours = $480` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$960  |
| | |Doorvoerfactuur voor 2 regio's: VS-Oost, Noord-Europa (alle regio's worden geschreven)  |`(2 + 1) * (60 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2,880  |
|[101-200] |D1:50K <br/>D2:70K <br/>C1: -- |Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` |$1920  |
| | |Doorvoerfactuur voor 2 regio's: VS-Oost, Noord-Europa (alle regio's worden geschreven)  |`(2 + 1) * (120 K RU/sec /100 * $0.016) * 100 hours = $5,760`  |$5,760  |
|[201-300]  |D1:50K <br/>D2:70K <br/>C1:20K |Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` <br/>`C1: 20 K RU/sec/100 * $0,016 * 100 uur = $320 |$2,240`  |
| | |Doorvoerfactuur voor 2 regio's: VS-Oost, Noord-Europa (alle regio's worden geschreven)  |`(2 + 1) * (140 K RU/sec /100 * $0.016-) * 100 hours = $6,720` |$6,720 |
|[301-400] |D1:10K <br/>D2:80K <br/>C1: -- |Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 80 K RU/sec/100 * $0.016 * 100 hours = $1,280`  |$1,440   |
| | |Doorvoerfactuur voor 2 regio's: VS-Oost, Noord-Europa (alle regio's worden geschreven)  |`(1 + 1) * (90 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2,880  |
|[401-500] |D1:10K <br>D2:10K <br>C1:20K |Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`D2: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$640  |
| | |Doorvoerfactuur voor 2 regio's: VS-Oost, Noord-Europa (alle regio's worden geschreven)  |`(1 + 1) * (40 K RU/sec /100 * $0.016) * 100 hours = $1,280`  |$1,280  |
|[501-700] |D1:20K <br>D2:100K <br>C1: -- |Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)  |`D1: 20 K RU/sec/100 * $0.016 * 200 hours = $640` <br>`D2: 100 K RU/sec/100 * $0.016 * 200 hours = $3,200` |$3,840  |
| | |Doorvoerfactuur voor 2 regio's: VS-Oost, Noord-Europa (alle regio's worden geschreven)  |`(1 + 1) * (120 K RU/sec /100 * $0.016) * 200 hours = $1,280`  |$7,680  |
|[701-720] |D1:20K <br/>D2:50K <br/>C1: -- |Doorvoerfactuur voor container in West-Amerika (alle regio's worden geschreven)  |`D1: 20 K RU/sec/100 *$0.016 * 20 hours = $64` <br/>`D2: 50 K RU/sec/100 *$0.016 * 20 hours = $160` |$224  |
| | |Doorvoerfactuur voor 2 regio's: VS-Oost, Noord-Europa (alle regio's worden geschreven)  |`(1 + 1) * (70 K RU/sec /100 * $0.016) * 20 hours = $448`  |$224  |
|| |**Totale maandelijkse kosten**  | |**$38,688**   |

## <a name="proactively-estimating-your-monthly-bill"></a>Proactief schatting van uw maandelijkse factuur  

Laten we eens een ander voorbeeld, waarin u wilt proactief schatting van uw factuur voor einde van de maand. U kunt een schatting maken uw factuur als volgt:

|**Kosten voor gegevensopslag** | |
|----|----|
|Gem. Record grootte (KB) |1 |
|Aantal Records  |100,000,000  |
|Totale opslag (GB)  |100 |
|Maandelijkse kosten per GB  |$0.25  |
|Verwachte maandelijkse kosten voor opslag   |$25.00  |

<br>

|**Doorvoer kosten** | | | |
|----|----|----|----|
|Type bewerking| Aanvragen/sec| Gem. RU/request| RU's die nodig zijn|
|Schrijven| 100 | 5 | 500|
|Lezen| 400| 1| 400|

Totaal aantal RU/sec.: 500 + 400 = 900 kosten per uur: 900/100 * $0.008 = $0.072 maandelijkse kosten verwacht voor doorvoer (uitgaande van 31 dagen): $0.072 * 24 uur per dag * 31 $53.57 =

**Totale maandelijkse kosten**

Totale maandelijkse kosten = maandelijkse kosten voor opslag + maandelijkse kosten voor de doorvoer van de totale maandelijkse kosten = $25,00 + $53.57 $78.57 =

*Prijzen kunnen variëren per regio. Zie voor bijgewerkte prijzen, de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/).*

## <a name="billing-with-azure-cosmos-db-reserved-capacity"></a>Facturering met Azure Cosmos DB gereserveerde capaciteit

Azure Cosmos DB gereserveerde capaciteit kunt u om aan te schaffen ingerichte doorvoer van tevoren (een gereserveerde capaciteit of een reservering) die kan worden toegepast op alle Azure Cosmos DB-databases en containers (voor een API of gegevens-model) in alle Azure-regio's. Omdat de ingerichte doorvoer prijs varieert per regio, helpt het gereserveerde capaciteit beschouwen als een financieel tegoed dat u hebt aangeschaft tegen een gereduceerd tarief, dat kan worden opgehaald voor de ingerichte doorvoer voor de respectieve prijs in elke regio. Stel dat u hebt een Azure-Cosmos-account met een enkele container ingericht met 50-K RU/sec. en globaal gerepliceerd twee regio's: VS-Oost en Japan-Oost. Als u ervoor de optie voor betalen per gebruik kiest, zou u betalen:  

* in VS-Oost: voor 50-K RU/sec. tegen het tarief van $0.008 per 100 RU/sec. in deze regio 

* in Japan (Oost): voor 50-K RU/sec. tegen het tarief van $0.009 per 100 RU/sec. in deze regio

Uw totale factuur (zonder gereserveerde capaciteit) zijn (uitgaande 30 dagen of uren 720): 

|**Regio**| **Prijs per 100 RU/s per uur**|**Eenheden (RU/s)**|**Gefactureerd bedrag (per uur)**| **Gefactureerd bedrag (maandelijks)**|
|----|----|----|----|----|
|US - oost|$0.008 |50 K|$4|$2,880 |
|Japan - oost|$0.009 |50 K| $4.50 |$3,240 |
|Totaal|||$8.50|$6,120 |

Laten we eens dat u in plaats daarvan gereserveerde capaciteit hebt gekocht. U kunt gereserveerde capaciteit voor 100 kB RU/sec. voor de prijs van $56,064 voor één jaar (op 20% korting) of $6.40 per uur kopen. Zie de gereserveerde capaciteit prijzen op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)).  

* De kosten van doorvoer (betalen naar gebruik): 100.000 RU/sec/100 * $0.008/uur * 8760 uren in een jaar = $70,080 

* Kosten van doorvoer (met gereserveerde capaciteit) $70,080 op 20% korting $56,064 = 

Wat u daadwerkelijk hebt aangeschaft, is een tegoed van $8 per uur voor 100 K RU/sec. met behulp van de catalogusprijs in VS-Oost, voor de prijs van $6.40 per uur. U kunt vervolgens tekenen omlaag vanaf deze reservering vooraf betaalde doorvoer op uurbasis voor de capaciteit van de ingerichte doorvoer in een globale Azure-regio op de desbetreffende regionale prijzen instellen voor uw abonnement. In dit voorbeeld, waarin u 50 K RU/sec. elke in VS-Oost en Japan-Oost inricht, u kunt tekenen $8.00 waard is om van de ingerichte doorvoer per uur, en wordt de overschrijding van $0,50 per uur (of $360/ maand) in rekening gebracht. 

|**Regio**| **Prijs per 100 RU/s per uur**|**Eenheden (RU/s)**| **Gefactureerd bedrag (per uur)**| **Gefactureerd bedrag (maandelijks)**|
|----|----|----|----|----|
|US - oost|$0.008 |50 K|$4|$2,880 |
|Japan - oost|$0.009 |50 K| $4.50 |$3,240 |
|||Betalen naar gebruik|$8.50|$6120|
|Aangeschafte gereserveerde capaciteit|$0.0064 (20% korting) |100 RU/sec. of vooraf aangeschafte $8-capaciteit |-$8|-$5,760 |
|Nettofactuur|||$0.50 |$360 |

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan naar meer informatie over kostenoptimalisatie in Azure Cosmos DB met de volgende artikelen:

* Meer informatie over [hoe Cosmos DB prijsmodel is kostenefficiënt voor klanten](total-cost-ownership.md)
* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [doorvoer kosten optimaliseren](optimize-cost-throughput.md)
* Meer informatie over [opslagkosten te optimaliseren](optimize-cost-storage.md)
* Meer informatie over [optimaliseren van de kosten van lees- en schrijfbewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [de kosten van query's optimaliseren](optimize-cost-queries.md)
* Meer informatie over [optimaliseren van de kosten van Azure Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md)
