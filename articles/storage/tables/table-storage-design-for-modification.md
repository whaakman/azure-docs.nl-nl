---
title: Azure storage-tabellen voor wijziging van gegevens | Microsoft Docs
description: Ontwerp tabellen voor wijziging van de gegevens in de Azure table storage.
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
ms.openlocfilehash: 6c008175f01521ce4f96d13e58244dc72d9f6990
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660822"
---
# <a name="design-for-data-modification"></a>Ontwerp voor wijziging van gegevens
In dit artikel is gericht op de ontwerpoverwegingen voor het optimaliseren van toevoegingen, updates, en worden verwijderd. In sommige gevallen moet u de verhouding tussen ontwerpen die optimaliseren voor een query op modellen die voor wijziging van gegevens, optimaliseren net als in ontwerpen voor relationele databases (Hoewel de technieken voor het beheren van de ontwerp-en nadelen andere in een relationele database) evalueren. De sectie [ontwerppatronen voor tabel](#table-design-patterns) beschrijft een aantal gedetailleerde ontwerppatronen voor de tabel-service en worden enkele deze verschillen. In de praktijk vindt u veel ontwerpen die zijn geoptimaliseerd voor het uitvoeren van query's entiteiten ook geschikt voor entiteiten wijzigen.  

## <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>De prestaties van insert, update en delete-bewerkingen optimaliseren
Als u wilt bijwerken of verwijderen van een entiteit, moet u kunnen worden geïdentificeerd met behulp van de **PartitionKey** en **RowKey** waarden. In dit opzicht van uw keuze **PartitionKey** en **RowKey** voor entiteiten wijzigen dezelfde criteria voor uw keuze volgen moet voor de ondersteuning van punt query's omdat u wilt identificeren entiteiten zo efficiënt mogelijk. U niet wilt dat een entiteit Zoek om te detecteren met behulp van een inefficiënte partitie of tabel scan de **PartitionKey** en **RowKey** waarden die u wilt bijwerken of verwijderen.  

De volgende patronen in de sectie [ontwerppatronen voor tabel](#table-design-patterns) adres optimaliseert de prestaties of de insert, update en verwijderbewerkingen:  

* [Hoog volume patroon verwijderen](table-storage-design-patterns.md#high-volume-delete-pattern) -de verwijdering van een groot aantal entiteiten inschakelen door het opslaan van alle entiteiten voor gelijktijdige verwijdering in hun eigen afzonderlijke tabel; u de entiteiten verwijderen door de tabel verwijderen.  
* [Patroon van de reeks gegevens](table-storage-design-patterns.md#data-series-pattern) -Store voltooid gegevensreeksen in één entiteit om te beperken het aantal aanvragen die u aanbrengt.  
* [Wide entiteiten patroon](table-storage-design-patterns.md#wide-entities-pattern) -gebruik van meerdere fysieke entiteiten voor het opslaan van logische entiteiten met meer dan 252 eigenschappen.  
* [Grote entiteiten patroon](table-storage-design-patterns.md#large-entities-pattern) -blob storage gebruiken voor het opslaan van grote eigenschapswaarden.  

## <a name="ensure-consistency-in-your-stored-entities"></a>De consistentie in uw opgeslagen entiteiten
De andere belangrijke factoren die van invloed is op uw keuze van sleutels voor het optimaliseren van gegevenswijzigingen is hoe u de consistentie met behulp van atomische transacties. U kunt alleen een EGT bewerkingen uitvoeren op entiteiten die zijn opgeslagen in dezelfde partitie.  

De volgende patronen in het artikel [ontwerppatronen voor tabel](table-storage-design-patterns.md) adres consistentie beheren:  

* [Intra-partitie secundaire index patroon](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -opslaan meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden.  
* [Secundaire tussen partitioneren van index patroon](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) : opslaan van meerdere exemplaren van elke entiteit die gebruikmaken van verschillende waarden voor de RowKey in afzonderlijke partities of in afzonderlijke tabellen waarmee snel en efficiënt zoekacties en alternatieve sorteren orders met behulp van verschillende **RowKey** waarden.  
* [Uiteindelijk consistent transacties patroon](table-storage-design-patterns.md#eventually-consistent-transactions-pattern) -uiteindelijk consistent gedrag over grenzen van partities of opslag system grenzen inschakelen met behulp van Azure wachtrijen.
* [Index entiteiten patroon](table-storage-design-patterns.md#index-entities-pattern) -onderhouden index entiteiten zodat efficiënte zoekopdrachten die lijsten van entiteiten retourneren.  
* [Denormalization patroon](table-storage-design-patterns.md#denormalization-pattern) -combineren van de bijbehorende gegevens samen in één entiteit waarmee u kunt alle gegevens die u nodig hebt met een query één punt ophalen.  
* [Patroon van de reeks gegevens](table-storage-design-patterns.md#data-series-pattern) -Store voltooid gegevensreeksen in één entiteit om te beperken het aantal aanvragen die u aanbrengt.  

Zie de sectie voor informatie over entiteit groepstransacties [entiteit groepstransacties](table-storage-design.md#entity-group-transactions).  

## <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Zorg ervoor dat uw ontwerp voor efficiënte wijzigingen vereenvoudigt efficiënt query 's
Een ontwerp voor een efficiënte query resulteert in efficiënt wijzigingen, maar u moet altijd in veel gevallen evalueren of dit het geval is voor uw specifieke scenario. Sommige van de patronen in het artikel [ontwerppatronen voor tabel](table-storage-design-patterns.md) expliciet evalueren verschillen tussen het uitvoeren van query's en entiteiten wijzigen en u moet altijd rekening gehouden met het nummer van elk type bewerking.  

De volgende patronen in het artikel [ontwerppatronen voor tabel](table-storage-design-patterns.md) adres verschillen tussen ontwerpen voor efficiënt query's en ontwerpen voor wijziging van efficiënte gegevens:  

* [Samengestelde sleutelpatroon](table-storage-design-patterns.md#compound-key-pattern) -gebruik samengestelde **RowKey** waarden om in te schakelen van een client voor het opzoeken van gerelateerde gegevens met een query één punt.  
* [Logboek tail patroon](table-storage-design-patterns.md#log-tail-pattern) -ophalen van de *n* entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** waarde die in omgekeerde datum en tijd volgorde worden gesorteerd.  
