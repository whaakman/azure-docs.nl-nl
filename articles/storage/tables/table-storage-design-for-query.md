---
title: Azure storage-tabellen voor query's ontwerpen | Microsoft Docs
description: Ontwerp tabellen voor query's in Azure-tabelopslag.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.component: tables
ms.openlocfilehash: 8b4ae066edc1c62c25762b5c6feebce1ecfff5a2
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521537"
---
# <a name="design-for-querying"></a>Ontwerp voor query's
Oplossingen voor tabel-service kunnen worden gelezen intensieve, intensieve schrijven of een combinatie van de twee. In dit artikel richt zich op de belangrijke zaken op moet letten wanneer u de Table-service voor de ondersteuning van leesbewerkingen efficiënt ontwerpt. Een ontwerp dat ondersteunt bewerkingen efficiënt lezen is meestal ook efficiënt voor schrijfbewerkingen. Er zijn echter aanvullende overwegingen op moet letten bij het ontwerpen van voor de ondersteuning van schrijfbewerkingen, in het artikel is besproken [ontwerp voor een gegevenswijziging](table-storage-design-for-modification.md).

Een goed uitgangspunt voor het ontwerpen van uw tabel-service-oplossing waarmee u kunt gegevens efficiënt lezen is om te vragen 'welke query's wordt mijn toepassing hoeft uit te voeren om op te halen van de benodigde gegevens uit de tabelservice?'  

> [!NOTE]
> Met de Table-service is het belangrijk dat u het ontwerp van de juiste een omdat het is moeilijk en kostbaar later wijzigen. Bijvoorbeeld in een relationele database is het vaak mogelijk voor het oplossen van problemen met prestaties door te indexen toe te voegen aan een bestaande database: dit kan niet worden gebruikt met de Table-service.  
> 
> 

Deze sectie richt zich op de belangrijkste problemen die u rekening houden moet bij het ontwerpen van uw tabellen voor het uitvoeren van query's. De onderwerpen aan bod in deze sectie zijn onder andere:

* [Hoe uw eigen keuze aan PartitionKey en RowKey heeft gevolgen voor prestaties van query 's](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Een juiste PartitionKey kiezen](#choosing-an-appropriate-partitionkey)
* [Query's voor de Table-service te optimaliseren](#optimizing-queries-for-the-table-service)
* [Sorteren van gegevens in de Table-service](#sorting-data-in-the-table-service)

## <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Hoe uw eigen keuze aan PartitionKey en RowKey heeft gevolgen voor prestaties van query 's
De volgende voorbeelden wordt ervan uitgegaan dat de table-service opslaat werknemer entiteiten met de volgende structuur (de meeste van de voorbeelden laat de **Timestamp** eigenschap voor de duidelijkheid):  

| *De naam van kolom* | *Gegevenstype* |
| --- | --- |
| **PartitionKey** (naam van afdeling) |Reeks |
| **RowKey** (werknemer-Id) |Reeks |
| **Voornaam** |Reeks |
| **LastName** |Reeks |
| **Leeftijd** |Geheel getal |
| **EmailAddress** |Reeks |

Het artikel [overzicht van Azure Table storage](table-storage-overview.md) beschrijft een aantal van de belangrijkste functies van de Azure Table-service waarvoor een directe invloed op het ontwerpen van query. Dit resulteert in de volgende algemene richtlijnen voor het ontwerpen van query's tabel-service. Merk op dat de filtersyntaxis van de gebruikt in de onderstaande voorbeelden uit de tabelservice REST-API voor meer informatie naar [entiteiten opvragen](https://docs.microsoft.com/rest/api/storageservices/Query-Entities).  

* Een ***punt Query*** is de meest efficiënte zoekopdracht om te gebruiken en wordt aanbevolen om te worden gebruikt voor zoekopdrachten met hoog volume of zoekacties laagste latentie vereisen. Dergelijke query de indexen kunt gebruiken om te zoeken op een afzonderlijke entiteit zeer efficiënt door op te geven op beide de **PartitionKey** en **RowKey** waarden. Bijvoorbeeld: $filter = (PartitionKey eq 'Verkoop') en (RowKey eq '2')  
* Ten tweede beste is een ***Bereikquery*** die gebruikmaakt van de **PartitionKey** en filters op een scala aan **RowKey** die moeten worden geretourneerd van meer dan één entiteit. De **PartitionKey** waarde geeft een specifieke partitie en de **RowKey** waarden identificeren een subset van de entiteiten in de betreffende partitie. Bijvoorbeeld: $filter = PartitionKey eq 'Verkoop en RowKey ge van' en RowKey lt 'T'  
* Derde beste is een ***partitie scannen*** die gebruikmaakt van de **PartitionKey** en filters op een andere niet-sleuteleigenschap en die meer dan één entiteit kunnen retourneren. De **PartitionKey** waarde geeft een specifieke partitie en de eigenschap waarden selecteren voor een subset van de entiteiten in de betreffende partitie. Bijvoorbeeld: $filter = PartitionKey eq 'Verkoop'- en achternaam eq 'Smith'  
* Een ***tabel scannen*** omvat niet de **PartitionKey** en is zeer inefficiënt omdat wordt gezocht in alle van de partities die gezamenlijk uw tabel op zijn beurt voor elke overeenkomende entiteiten. Wordt uitgevoerd om een tabelscan, ongeacht of uw filter maakt gebruik van de **RowKey**. Bijvoorbeeld: $filter = LastName eq 'Jones'  
* Query's die meerdere entiteiten retourneren, retourneren ze gesorteerd **PartitionKey** en **RowKey** volgorde. Om te voorkomen dat de entiteiten in de client codeactiviteit, kies een **RowKey** die de meest voorkomende sorteervolgorde definieert.  

Houd er rekening mee dat als u met een '**of**"om op te geven van een filter op basis van **RowKey** waarden resulteert in een partitie-scan en wordt niet beschouwd als een bereikquery. Daarom moet u query's die gebruikmaken van filters zoals voorkomen: $filter = PartitionKey eq 'Verkoop' (RowKey eq '121' of een RowKey eq '322')  

Zie voor voorbeelden van code op de client die de Storage-clientbibliotheek gebruiken voor het uitvoeren van efficiënte query's:  

* [Een point-query met de Storage-clientbibliotheek uitvoeren](table-storage-design-patterns.md#executing-a-point-query-using-the-storage-client-library)
* [Meerdere entiteiten met behulp van LINQ ophalen](table-storage-design-patterns.md#retrieving-multiple-entities-using-linq)
* [Server-side-projectie](table-storage-design-patterns.md#server-side-projection)  

Zie voor voorbeelden van de client-side-code die meerdere Entiteitstypen die zijn opgeslagen in dezelfde tabel kunnen worden verwerkt:  

* [Werken met heterogene Entiteitstypen](table-storage-design-patterns.md#working-with-heterogeneous-entity-types)  

## <a name="choosing-an-appropriate-partitionkey"></a>Een juiste PartitionKey kiezen
Uw eigen keuze aan **PartitionKey** de noodzaak voor het inschakelen van het gebruik van EGTs (om te zorgen voor consistentie) moet verdelen op basis van de vereiste voor uw entiteiten verdelen over meerdere partities (zodat een schaalbare oplossing).  

Op een extreme kan voor het opslaan van alle uw entiteiten in één partitie, maar dit kan de schaalbaarheid van uw oplossing beperken en weg kunnen staan de table-service wordt het verdelen van aanvragen. Op de andere extreme, kunt u één entiteit per partitie, die wel uiterst schaalbare en waarmee de table-service te verdelen van aanvragen, maar dat zou voorkomen dat u met behulp van de entiteit-groepstransacties opslaan.  

Een ideale **PartitionKey** is een waarmee u kunt efficiënt query's gebruiken en die voldoende partities om te controleren of uw oplossing is schaalbaar is. Normaal gesproken vindt u dat uw entiteiten een geschikte eigenschap die wordt verspreid uw entiteiten over voldoende partities hebben.

> [!NOTE]
> Bijvoorbeeld in een systeem waarin informatie over gebruikers- of werknemers, gebruikers-id is mogelijk een goede PartitionKey. Mogelijk hebt u verschillende entiteiten die gebruikmaken van een bepaalde gebruikers-id als de partitiesleutel. Elke entiteit die gegevens over een gebruiker worden opgeslagen in één partitie is gegroepeerd en dus deze entiteiten zijn toegankelijk via de entiteit-groepstransacties, terwijl u nog steeds uiterst schaalbare.
> 
> 

Zijn er aanvullende overwegingen bij de keuze van **PartitionKey** die betrekking hebben op hoe u wordt invoegen, bijwerken en verwijderen van entiteiten. Zie voor meer informatie, [het ontwerpen van tabellen voor wijziging van gegevens](table-storage-design-for-modification.md).  

## <a name="optimizing-queries-for-the-table-service"></a>Query's voor de Table-service te optimaliseren
De service van de tabel indexeert automatisch uw entiteiten met behulp van de **PartitionKey** en **RowKey** waarden in één geclusterde index, dus de reden dat wijst u query's zijn het meest efficiënt te gebruiken. Er zijn echter geen indexen dan die op de geclusterde index op de **PartitionKey** en **RowKey**.

Veel ontwerpen moeten voldoen aan de vereisten voor het opzoeken van de entiteiten op basis van meerdere criteria inschakelen. Bijvoorbeeld zoeken naar werknemers entiteiten op basis van e-mail, werknemer-id of achternaam op. De patronen die worden beschreven in [tabel ontwerppatronen](table-storage-design-patterns.md) adres van deze typen vereiste en manieren om te werken om het feit dat de Table-service biedt geen secundaire indexen te beschrijven:  

* [Intra-partitie secundaire index patroon](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -Store meerdere kopieën van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) voor de snelle en efficiënte zoekopdrachten en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden.  
* [Secundaire index tussen partitie patroon](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -Store meerdere kopieën van elke entiteit met behulp van verschillende **RowKey** waarden in afzonderlijke partities of in afzonderlijke tabellen om in te schakelen van snelle en efficiënte zoekopdrachten en alternatieve sorteren orders met behulp van verschillende **RowKey** waarden.  
* [Index entiteiten patroon](table-storage-design-patterns.md#index-entities-pattern) -entiteiten zodat efficiënte zoekopdrachten die resulteren in een lijst met entiteiten index onderhouden.  

## <a name="sorting-data-in-the-table-service"></a>Sorteren van gegevens in de Table-service
De Table-service retourneert entiteiten gesorteerd in oplopende volgorde op basis van **PartitionKey** en vervolgens op **RowKey**. Deze sleutels worden tekenreekswaarden en om ervoor te zorgen dat numerieke waarden correct sorteren, moet u deze converteren naar een vaste lengte en ze worden opgevuld met nullen. Bijvoorbeeld, als de waarde van de werknemer-id die u gebruiken als de **RowKey** is een geheel getal, moet u de werknemer-id converteren **123** naar **00000123**.  

Veel toepassingen zijn vereisten voor het gebruik van de gegevens gesorteerd op verschillende plekken: bijvoorbeeld werknemers sorteren op naam, of lid wordt van datum. De volgende patronen adres van het alternatieve sorteervolgorde voor uw entiteiten:  

* [Intra-partitie secundaire index patroon](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) - Store meerdere kopieën van elke entiteit met behulp van verschillende RowKey waarden (in dezelfde partitie) voor snelle en efficiënte zoekopdrachten voor bestanden en alternatieve sorteren orders met behulp van verschillende RowKey waarden.  
* [Secundaire index tussen partitie patroon](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - Store meerdere kopieën van elke entiteit die verschillende waarden voor de RowKey in afzonderlijke partities in afzonderlijke tabellen met snelle en efficiënte zoekopdrachten voor bestanden en alternatieve sorteren orders met behulp van verschillende RowKey waarden .
* [Logboek tail patroon](table-storage-design-patterns.md#log-tail-pattern) -ophalen van de *n* entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** gesorteerd in omgekeerde datum en de volgorde van tijd-waarde.  

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Waardoor relaties worden gemaakt](table-storage-design-modeling.md)
- [Versleutelen van gegevens in een tabel](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)
