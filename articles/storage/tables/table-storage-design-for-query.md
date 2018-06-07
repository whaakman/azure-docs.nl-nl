---
title: Azure storage-tabellen voor query's ontwerpt | Microsoft Docs
description: Ontwerp tabellen voor query's in Azure-tabelopslag.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: b8d2033b0b29caddf165f4b582c7d0578109480c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660826"
---
# <a name="design-for-querying"></a>Ontwerp voor het uitvoeren van query 's
Oplossingen voor tabel-service kunnen worden gelezen intensief schrijven intensief of een combinatie van beide. Dit artikel is gericht op de zaken op moet letten tijdens het ontwerpen van de service voor ondersteuning voor leesbewerkingen efficiënt uw tabel. Een ontwerp dat ondersteunt lees-en schrijfopdrachten efficiënt is meestal ook efficiënt voor schrijfbewerkingen. Er zijn echter aanvullende overwegingen op moet letten wanneer ontwerpen voor de ondersteuning van schrijfbewerkingen, beschreven in het artikel [ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md).

Een goed uitgangspunt voor het ontwerpen van uw tabel-service-oplossing kunt u gegevens efficiënt te lezen is vragen "welke query mijn toepassing moeten worden uitgevoerd voor het ophalen van de benodigde gegevens uit de tabelservice?"  

> [!NOTE]
> Met de tabel-service is het belangrijk dat u het ontwerp van de juiste vooraf omdat het is moeilijk en kostbaar later wijzigen. Bijvoorbeeld in een relationele database is het vaak mogelijk prestaties om problemen te verhelpen door indexen toe te voegen aan een bestaande database: dit kan niet worden gebruikt met de tabel-service.  
> 
> 

Deze sectie legt de nadruk op de belangrijkste die u houden moet bij het ontwerpen van uw tabellen voor het uitvoeren van query's. De onderwerpen in deze sectie beschreven, zijn onder andere:

* [Hoe uw keuze van PartitionKey en RowKey van invloed is op prestaties van query 's](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Een juiste PartitionKey kiezen](#choosing-an-appropriate-partitionkey)
* [Optimaliseren van query's voor de tabel-service](#optimizing-queries-for-the-table-service)
* [Sorteren van gegevens in de tabel-service](#sorting-data-in-the-table-service)

## <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Hoe uw keuze van PartitionKey en RowKey van invloed is op prestaties van query 's
De volgende voorbeelden wordt ervan uitgegaan dat de tabelservice werknemer entiteiten met de volgende structuur wordt opgeslagen (de meeste van de voorbeelden weglaten de **tijdstempel** eigenschap voor de duidelijkheid):  

| *Kolomnaam* | *Gegevenstype* |
| --- | --- |
| **PartitionKey** (naam van de afdeling) |Reeks |
| **RowKey** (werknemer-Id) |Reeks |
| **Voornaam** |Reeks |
| **LastName** |Reeks |
| **leeftijd** |Geheel getal |
| **EmailAddress** |Reeks |

Het artikel [overzicht van Azure Table storage](table-storage-overview.md) beschrijft een aantal van de belangrijkste functies van de service Azure Table waarvoor een directe invloed op ontwerpen voor query. Deze leiden tot de volgende algemene richtlijnen voor het ontwerpen van query's tabel-service. De filtersyntaxis gebruikt in de volgende voorbeelden wordt uit de tabelservice REST API, Zie voor meer informatie [Query entiteiten](https://docs.microsoft.com/rest/api/storageservices/Query-Entities).  

* Een ***punt Query*** is het meest efficiënt lookup te gebruiken en wordt aanbevolen voor grote zoekopdrachten of zoekacties laagste latentie vereisen moet worden gebruikt. Dergelijke query kunt de indexen gebruiken een afzonderlijke entiteit zeer efficiënt vinden door op te geven van zowel de **PartitionKey** en **RowKey** waarden. Bijvoorbeeld: $filter = (PartitionKey eq 'Sales') en (RowKey eq '2')  
* Tweede in de rij is een ***Bereikquery*** die gebruikmaakt van de **PartitionKey** en filters op een reeks **RowKey** waarden te retourneren van meer dan één entiteit. De **PartitionKey** waarde identificeert een specifieke partitie en de **RowKey** waarden een subset van de entiteiten in de betreffende partitie identificeren. Bijvoorbeeld: $filter = PartitionKey eq 'Verkoop en de RowKey ge' en RowKey lt t '  
* Derde beste is een ***partitie scannen*** die gebruikmaakt van de **PartitionKey** en filters op een andere niet-sleutelkenmerk eigenschap en die mogelijk meer dan één entiteit geretourneerd. De **PartitionKey** waarde identificeert een specifieke partitie en de eigenschap waarden selecteren voor een subset van de entiteiten in de betreffende partitie. Bijvoorbeeld: $filter = PartitionKey eq 'Verkoop' en LastName eq 'Smith'  
* Een ***tabel scannen*** omvat niet de **PartitionKey** en zeer inefficiënte omdat alle van de partities die gezamenlijk uw tabel voor de overeenkomende entiteiten op zijn beurt wordt doorzocht. Wordt uitgevoerd om een tabelscan ongeacht of het filter gebruikt de **RowKey**. Bijvoorbeeld: $filter = LastName eq 'Jones'  
* Query's die meerdere entiteiten retourneren retourneren ze gesorteerd **PartitionKey** en **RowKey** volgorde. Om te voorkomen de entiteiten in de client te sorteren, kies een **RowKey** de meest voorkomende sorteervolgorde te definiëren.  

Houd er rekening mee dat als u met een '**of**' om op te geven van een filter op basis van **RowKey** waarden resulteert in een partitie scan en wordt niet behandeld als een bereikquery. U moet daarom query's die worden gebruikt, zoals filters: $filter = PartitionKey eq 'Sales' (RowKey eq '121' of een RowKey eq '322')  

Zie voor voorbeelden van clientcode die gebruikmaken van de Storage-clientbibliotheek efficiënt query's uitvoeren:  

* [Met behulp van de Storage-clientbibliotheek punt query's uitvoeren](table-storage-design-patterns.md#executing-a-point-query-using-the-storage-client-library)
* [Meerdere entiteiten met behulp van LINQ ophalen](table-storage-design-patterns.md#retrieving-multiple-entities-using-linq)
* [Projectie-serverzijde](table-storage-design-patterns.md#server-side-projection)  

Voor voorbeelden van clientcode dat meerdere Entiteitstypen die zijn opgeslagen in dezelfde tabel kan verwerken, Zie:  

* [Werken met heterogene Entiteitstypen](table-storage-design-patterns.md#working-with-heterogeneous-entity-types)  

## <a name="choosing-an-appropriate-partitionkey"></a>Een juiste PartitionKey kiezen
Uw keuze van **PartitionKey** moet een balans vinden tussen de noodzaak om het gebruik van EGTs (consistentie te garanderen) de vereiste voor de distributie van de entiteiten over meerdere partities (zodat een schaalbare oplossing).  

Op één extreme u alle entiteiten in uw kan opslaan in een enkele partitie, maar dit kan de schaalbaarheid van uw oplossing beperken en zou voorkomen dat de tabelservice kunnen verdelen aanvragen. Het andere uiterste, kunt u één entiteit per partitie, die wel uiterst schaalbare en waardoor de tabelservice op aanvragen voor taakverdeling, maar dat zou voorkomen dat u entiteitstransacties opslaan.  

Een ideaal **PartitionKey** is een die u kunt gebruiken efficiënt query's en met voldoende partities om te controleren of uw oplossing schaalbaar is. Normaal gesproken zult u merken dat uw entiteiten een geschikte eigenschap die de entiteiten over voldoende partities verdeelt hebben.

> [!NOTE]
> Bijvoorbeeld in een systeem dat wordt informatie over gebruikers- of werknemers opgeslagen, gebruikers-id is mogelijk een goede PartitionKey. Mogelijk hebt u verschillende entiteiten die een opgegeven gebruikers-id als de partitiesleutel gebruiken. Elke entiteit die gegevens over een gebruiker opslaat in een enkele partitie worden gegroepeerd en dus deze entiteiten zijn toegankelijk via entiteit groepstransacties, terwijl u nog steeds zeer schaalbaar.
> 
> 

Er zijn aanvullende overwegingen bij de keuze van **PartitionKey** die betrekking hebben op hoe u wilt invoegen, bijwerken en verwijderen van entiteiten. Zie voor meer informatie [tabellen voor wijziging van gegevens ontwerpen](table-storage-design-for-modification.md).  

## <a name="optimizing-queries-for-the-table-service"></a>Optimaliseren van query's voor de tabel-service
De tabelservice indexeert automatisch de entiteiten met behulp van de **PartitionKey** en **RowKey** waarden in een enkele geclusterde index, daarom de reden dat wijst u query's zijn het meest efficiënt te gebruiken. Er zijn echter geen indexen dan die op de geclusterde index op de **PartitionKey** en **RowKey**.

Veel ontwerpen moeten voldoen aan de vereisten voor het opzoeken van de entiteiten die zijn gebaseerd op meerdere criteria inschakelen. Bijvoorbeeld zoeken naar werknemer entiteiten op basis van e-mailadres werknemer-id of achternaam op. De patronen die zijn beschreven in [ontwerppatronen voor tabel](table-storage-design-patterns.md) deze typen vereiste op te lossen en beschrijven manieren van het feit dat de tabel-service biedt geen secundaire indexen te omzeilen:  

* [Intra-partitie secundaire index patroon](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -opslaan meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden.  
* [Secundaire tussen partitioneren van index patroon](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -opslaan meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden in afzonderlijke partities of in afzonderlijke tabellen snelle en efficiënte zoekacties en alternatieve sorteren inschakelen orders met behulp van verschillende **RowKey** waarden.  
* [Index entiteiten patroon](table-storage-design-patterns.md#index-entities-pattern) -onderhouden index entiteiten zodat efficiënte zoekopdrachten die lijsten van entiteiten retourneren.  

## <a name="sorting-data-in-the-table-service"></a>Sorteren van gegevens in de tabel-service
De tabel-service retourneert entiteiten in oplopende volgorde op basis van gesorteerd **PartitionKey** en vervolgens op **RowKey**. Deze sleutels worden tekenreekswaarden en om ervoor te zorgen dat numerieke waarden correct sorteren, moet u deze converteren naar een vaste lengte en ze worden opgevuld met nullen. Bijvoorbeeld, als de waarde van de werknemer-id die u gebruikt als de **RowKey** is een geheel getal, moet u de werknemer-id converteren **123** naar **00000123**.  

Veel toepassingen hebben vereisten voor het gebruik van gegevens in verschillende volgorden gesorteerd: werknemers bijvoorbeeld sorteren op naam of door datum. De volgende patronen te houden hoe de sorteervolgorde voor uw entiteiten alternatieve:  

* [Intra-partitie secundaire index patroon](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) : opslaan van meerdere exemplaren van elke entiteit die gebruikmaken van verschillende RowKey waarden (in dezelfde partitie) waarmee snel en efficiënt zoekacties en alternatieve sorteren orders met behulp van verschillende RowKey waarden.  
* [Secundaire tussen partitioneren van index patroon](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) : opslaan van meerdere exemplaren van elke entiteit met verschillende waarden voor de RowKey in afzonderlijke partities in afzonderlijke tabellen inschakelen snel en efficiënt zoekacties en alternatieve sorteren bestellingen met behulp van verschillende RowKey waarden .
* [Logboek tail patroon](table-storage-design-patterns.md#log-tail-pattern) -ophalen van de *n* entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** waarde die in omgekeerde datum en tijd volgorde worden gesorteerd.  

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Relaties te modelleren](table-storage-design-modeling.md)
- [Tabelgegevens versleutelen](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)
