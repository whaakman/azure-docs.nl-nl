---
title: Richtlijnen voor Azure storage tabelontwerp | Microsoft Docs
description: Uw Azure-tabel-service voor de ondersteuning van leesbewerkingen efficiënt ontwerpen.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: d056d29469ad9a60fceeee307aca3c0e1319283c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55454518"
---
# <a name="guidelines-for-table-design"></a>Richtlijnen voor tabelontwerp

Het ontwerpen van tabellen voor gebruik met de service Azure storage-tabel is heel anders dan ontwerpoverwegingen voor een relationele database. Dit artikel bevat richtlijnen voor het ontwerpen van uw service-oplossing van de tabel efficiënt worden gelezen en geschreven efficiënt.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Uw service-oplossing van de tabel lezen efficiënt ontwerpen

* ***Ontwerp voor het uitvoeren van query's in leesintensief toepassingen.*** Wanneer u uw tabellen ontwerpt, denken over de query's (met name de latentie gevoelige bestanden) die u zullen uitvoeren voordat u gaat nadenken over hoe u uw entiteiten wordt bijgewerkt. Dit leidt meestal tot een efficiënte en krachtige oplossing.  
* ***Geef zowel PartitionKey en RowKey in uw query's.*** *Query's verwijzen* zoals dit zijn de meest efficiënt query's voor tabel-service.  
* ***Houd rekening met het opslaan van kopieën maken van entiteiten.*** Tabelopslag is goedkoop dus houd rekening met het opslaan van dezelfde entiteit meerdere keren (met verschillende sleutels) om in te schakelen efficiëntere query's.  
* ***Houd rekening met uw gegevens denormalizing.*** Tabelopslag is goedkoop dus houd rekening met uw gegevens denormalizing. Bijvoorbeeld, opslaan, samenvatting entiteiten zodat query's voor statistische gegevens alleen nodig voor toegang tot één entiteit.  
* ***Samengestelde sleutels waarden gebruiken.*** Zijn de enige sleutels die u hebt **PartitionKey** en **RowKey**. Gebruik bijvoorbeeld samengestelde sleutelwaarden zodat alternatieve versleutelde Toegangspaden naar entiteiten.  
* ***Query-projectie gebruiken.*** U kunt verminderen de hoeveelheid gegevens die u via het netwerk worden overgebracht met behulp van query's die u selecteert alleen de velden die u nodig hebt.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Uw service-oplossing van de tabel schrijven efficiënt ontwerpen  

* ***Maak geen hot-partities.*** Kies de sleutels waarmee u kunt uw verzoeken, verdeeld over meerdere partities op elk moment.  
* ***Vermijd pieken in verkeer.*** Het verkeer via een redelijke termijn vloeiend maken en te voorkomen dat pieken in verkeer.
* ***Niet per se een afzonderlijke tabel voor elk type entiteit maken.*** Wanneer u atomische transacties via Entiteitstypen vereist, kunt u deze meerdere Entiteitstypen opslaan in dezelfde partitie in dezelfde tabel.
* ***Houd rekening met de maximale doorvoer die u moet doen.*** U moet rekening houden met de schaalbaarheidsdoelen voor de Table-service en ervoor te zorgen dat uw ontwerp wordt niet ervoor zorgen dat u meer dan ze.  

Als u deze handleiding leest, ziet u voorbeelden die al deze principes in de praktijk te plaatsen. 

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Ontwerp voor het uitvoeren van query 's](table-storage-design-for-query.md)
- [Versleutelen van gegevens in een tabel](table-storage-design-encrypt-data.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)
