---
title: Richtlijnen voor het Azure-opslag tabelontwerp | Microsoft Docs
description: Ontwerp van de service voor ondersteuning voor leesbewerkingen efficiënt uw Azure-tabel.
services: storage
documentationcenter: na
author: SnehaGunda
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 5329d33aee1bb1a55e9982b1ba9e3e8329246980
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660816"
---
# <a name="guidelines-for-table-design"></a>Richtlijnen voor het tabelontwerp van de

Ontwerpen tabellen voor gebruik met de Azure storage tabel-service is heel verschillend van ontwerpoverwegingen voor een relationele database. Dit artikel bevat richtlijnen voor het ontwerpen van uw oplossing tabel-service worden efficiënt worden gelezen en geschreven efficiënt.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Uw tabel-service-oplossing lezen efficiënt ontwerpen

* ***Ontwerpen voor het uitvoeren van toepassingen waarin lezen veel query's.*** Tijdens het ontwerpen van uw tabellen nadenken over de query's (met name de latentie gevoelig die) die zal worden uitgevoerd voordat u nadenken over hoe u uw entiteiten wordt bijgewerkt. Dit leidt meestal tot een efficiënte en zodat oplossing.  
* ***Geef zowel PartitionKey en RowKey in uw query's.*** *Query's wijst* zoals dit zijn de meest efficiënt query's een tabel-service.  
* ***Houd rekening met dubbele exemplaren van entiteiten opslaan.*** Tabelopslag goedkope is dus overwegen bij het opslaan van dezelfde entiteit meerdere keren (met verschillende sleutels) om in te schakelen efficiënter query's.  
* ***U kunt uw gegevens denormalizing.*** Tabelopslag goedkope is dus Overweeg denormalizing van uw gegevens. Samenvatting entiteiten bijvoorbeeld opslaan zodat query's voor statistische gegevens alleen hoeft toegang tot één entiteit.  
* ***Gebruik de samengestelde sleutel komt.*** De enige sleutels die u hebt zijn **PartitionKey** en **RowKey**. Samengestelde sleutel komt bijvoorbeeld gebruiken om in te schakelen alternatieve sleutelhash Toegangspaden naar entiteiten.  
* ***Query-projectie gebruiken.*** U kunt verminderen de hoeveelheid gegevens die u via het netwerk overdragen met behulp van query's die alleen de velden die u moet selecteren.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Uw oplossing tabel service schrijven efficiënt ontwerpen  

* ***Maak geen hot partities.*** De optie sleutels waarmee u uw aanvragen verdeeld over meerdere partities op elk moment.  
* ***Vermijd pieken in het verkeer.*** Het verkeer via een redelijke periode vloeiend en pieken in het verkeer worden vermeden.
* ***Niet per se een afzonderlijke tabel voor elk type entiteit maken.*** Wanneer u atomische transacties op Entiteitstypen vereisen, slaat u deze meerdere Entiteitstypen in dezelfde partitie in dezelfde tabel.
* ***U kunt de maximale doorvoer die moeten worden gerealiseerd.*** U moet rekening houden met de schaalbaarheidsdoelen voor de tabel-service en ervoor te zorgen dat uw ontwerp leidt niet tot u ze overschrijden.  

Als u deze handleiding leest, ziet u voorbeelden die al deze principes in de praktijk geplaatst. 

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Ontwerp voor het uitvoeren van query 's](table-storage-design-for-query.md)
- [Tabelgegevens versleutelen](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)